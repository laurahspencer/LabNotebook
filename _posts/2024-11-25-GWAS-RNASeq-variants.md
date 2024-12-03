---
layout: post
title: GWAS with RNASeq-derived SNPs
--- 

I previously pulled SNPs from RNASeq data. Not many sites remain after filtering. Regardless, it's another dataset that can potentially identify trait-associated genotypes/sites, which in addition to our lcWGS-based GWAS could reveal some actually influential markers. 

Here are the scripts I used to pull genotypes from RNASeq: 

###  /home/lspencer/pcod-juv-temp/scripts/genotype-rnaseq_1.sh
```
cat genotype-rnaseq_1.sh
#!/bin/bash

#SBATCH --job-name=rna-genotype
#SBATCH --mail-user=laura.spencer@noaa.gov
#SBATCH --output=/home/lspencer/pcod-juv-temp/variants/outfiles/genotype-rnaseq-%A_%a.out
#SBATCH --mail-type=ALL
#SBATCH --cpus-per-task=10
#SBATCH -t 30-0:0:0
#SBATCH --array=1-80%20

module load aligners/bowtie2
module load bio/samtools
source /home/lspencer/venv/bin/activate
module load bio/vcftools

source ~/.bashrc
mamba activate singularity-3.8.6

REF=/home/lspencer/references/pcod-ncbi
INPUT=/home/lspencer/pcod-juv-temp/trimmed-data
OUTPUT=/home/lspencer/pcod-juv-temp/variants

### EXECUTE THE FOLLOWING CODE ONCE
# Move to the directory containing pcod genome
# Bowtie2 looks for index in current directory - easier just to be in it from the get-go
#cd ${REF}

## Create bowtie2 index for Pcod genome
#echo "Building bowtie2 genome index"
#bowtie2-build \
#--threads 1 \
#${REF}/GCF_031168955.1_ASM3116895v1_genomic.fna \
#${REF}/GCF_031168955.1_ASM3116895v1_genomic.fna >> "${OUTPUT}/01-bowtie2-build.txt" 2>&1

#Create a FASTA sequence dictionary file for genome (needed by gatk)
#echo "Creating sequence dictionary (.dict)"
#singularity exec /home/lspencer/docker_images/gatk_4.6.0.0.sif gat CreateSequenceDictionary \
#-R ${REF}/GCF_031168955.1_ASM3116895v1_genomic.fna \
#-O ${REF}/GCF_031168955.1_ASM3116895v1_genomic.dict

# Initialize row number
#row_number=1

# Loop through each file in the directory
#for file in ${INPUT}/*.trimmed.R1.fastq.gz; do
#    echo "${row_number}:${file}" >> ${OUTPUT}/input_list.txt
#    row_number=$((row_number + 1))
#done

## EXECUTE THE FOLLOWING CODE IN AN ARRAY
JOBS_FILE=${OUTPUT}/input_list.txt
IDS=$(cat ${JOBS_FILE})

for sample_line in ${IDS}
do
        job_index=$(echo ${sample_line} | awk -F ":" '{print $1}')
        file=$(echo ${sample_line} | awk -F ":" '{print $2}')
        if [[ ${SLURM_ARRAY_TASK_ID} == ${job_index} ]]; then
                break
        fi
done

# Extract sample name
sample="$(basename -a ${file} | cut -d "." -f 1)"
file_R1="${sample}.trimmed.R1.fastq.gz"
file_R2="${sample}.trimmed.R2.fastq.gz"
map_file="${sample}.bowtie.sam"

# # Align trimmed reads to genome
# bowtie2 \
# -x ${REF}/GCF_031168955.1_ASM3116895v1_genomic.fna \
# --sensitive \
# --threads 10 \
# --no-unal \
# -1 ${INPUT}/${file_R1} \
# -2 ${INPUT}/${file_R2} \
# -S ${OUTPUT}/${map_file}

# Move to output directory
cd ${OUTPUT}

# # Convert sam to bam and sort
# samtools view --threads 10 -b ${map_file} | samtools sort -o ${sample}.sorted.bam
#
# # Remove sam file to save space
# rm ${map_file}

# Add read group ID to bam
singularity exec /home/lspencer/docker_images/gatk_4.6.0.0.sif gatk AddOrReplaceReadGroups \
I=${sample}.sorted.bam \
O=${sample}.sorted.RG.bam \
RGID=1 \
RGLB=${sample} \
RGPL=ILLUMINA \
RGPU=unit1 \
RGSM=${sample}

# Deduplicate using gatk MarkDuplicates
singularity exec /home/lspencer/docker_images/gatk_4.6.0.0.sif gatk MarkDuplicates \
I=${sample}.sorted.RG.bam \
O=${sample}.dedup.bam \
M=${sample}.dup_metrics.txt \
REMOVE_DUPLICATES=true

# Remove RG bam file
rm ${sample}.sorted.RG.bam

# Split reads spanning splicing events
singularity exec /home/lspencer/docker_images/gatk_4.6.0.0.sif gatk SplitNCigarReads \
--create-output-bam-index false \
-R ${REF}/GCF_031168955.1_ASM3116895v1_genomic.fna \
-I ${sample}.dedup.bam \
-O ${sample}.dedup-split.bam

rm ${sample}.dedup.bam

# Index the final bam file
samtools index ${sample}.dedup-split.bam

# Call variants using HaplotypeCaller
singularity exec /home/lspencer/docker_images/gatk_4.6.0.0.sif gatk HaplotypeCaller \
-R ${REF}/GCF_031168955.1_ASM3116895v1_genomic.fna \
-I ${sample}.dedup-split.bam \
-O ${sample}.variants.g.vcf \
-L intervals.bed \
--native-pair-hmm-threads 20 \
-ERC GVCF
```

###  /home/lspencer/pcod-juv-temp/scripts/genotype-rnaseq_2.sh
```
#!/bin/bash

#SBATCH --job-name=rnaseq-genotype-filter
#SBATCH -p himem
#SBATCH --mem=1400G
#SBATCH --mail-user=laura.spencer@noaa.gov
#SBATCH --output=/home/lspencer/pcod-juv-temp/variants/genotype-rnaseq-filter.out
#SBATCH --mail-type=ALL
#SBATCH -t 30-0:0:0

module load bio/samtools
source /home/lspencer/venv/bin/activate
module load bio/vcftools

source ~/.bashrc
mamba activate singularity-3.8.6

REF=/home/lspencer/references/pcod-ncbi
OUTPUT=/home/lspencer/pcod-juv-temp/variants

cd ${OUTPUT}

# create sample map of all gvcfs
#rm -f sample_map.txt  #remove if already exists
#echo "Creating sample map of all gvcfs"
#for file in *variants.g.vcf
#do
#sample="$(basename -a $file | cut -d "." -f 1)"
#echo -e "$sample\t$file" >> sample_map.txt
#done

# Aggregate single-sample GVCFs into GenomicsDB
# Note: the intervals file requires a specific name - e.g. for .bed format, it MUST be "intervals.bed"
#echo "Aggregating single-sample GVCFs into GenomicsDB"
#rm -r GenomicsDB/ #can't already have a GenomicsDB directory, else will fail
#singularity exec /home/lspencer/docker_images/gatk_4.6.0.0.sif gatk GenomicsDBImport \
#--java-options '-Xmx1200g' \
#--genomicsdb-workspace-path GenomicsDB/ \
#-L intervals.bed \
#--sample-name-map sample_map.txt \
#--reader-threads 20 >> "GenomicsDBImport_stout.txt" 2>&1

# Joint genotype
echo "Joint genotyping"
singularity exec /home/lspencer/docker_images/gatk_4.6.0.0.sif gatk GenotypeGVCFs \
-R ${REF}/GCF_031168955.1_ASM3116895v1_genomic.fna \
-V gendb://GenomicsDB \
-O pcod_rnaseq_genotypes.vcf.gz \
>> "GenotypeGVCFs_stout.txt" 2>&1

#echo "Index VCF"
singularity exec /home/lspencer/docker_images/gatk_4.6.0.0.sif gatk IndexFeatureFile \
-I /home/lspencer/pcod-juv-temp/variants/pcod_rnaseq_genotypes.vcf.gz

# Hard filter variants
echo "Hard filtering variants"
singularity exec /home/lspencer/docker_images/gatk_4.6.0.0.sif gatk VariantFiltration \
-R ${REF}/GCF_031168955.1_ASM3116895v1_genomic.fna \
-V pcod_rnaseq_genotypes.vcf.gz \
-O pcod_rnaseq_genotypes-filtered.vcf.gz \
--filter-name "FS" \
--filter "FS > 60.0" \
--filter-name "QD" \
--filter "QD < 2.0" \
--filter-name "QUAL30" \
--filter "QUAL < 30.0" \
--filter-name "SOR3" \
--filter "SOR > 3.0" \
--filter-name "DP15" \
--filter "DP < 15" \
--filter-name "DP150" \
--filter "DP > 150" \
--filter-name "AF30" \
--filter "AF < 0.30" >> "Filter-variants_stout.txt" 2>&1

# Select only SNPs that pass filtering
echo "Selecting SNPs that pass fitering"
singularity exec /home/lspencer/docker_images/gatk_4.6.0.0.sif gatk SelectVariants \
-R ${REF}/GCF_031168955.1_ASM3116895v1_genomic.fna \
-V pcod_rnaseq_genotypes-filtered.vcf.gz \
--exclude-filtered TRUE \
--select-type-to-include SNP \
-O pcod_rnaseq_genotypes-filtered-true.vcf.gz \
 >> "SelectVariants_stout.txt" 2>&1

 # Create another vcf of SNPs filtered for loci with max 0%, 5%, 10%, 15%, 20%, and 25% missing rate, and remove loci with <5% minor allele frequency

 vcftools --gzvcf \
 "pcod_rnaseq_genotypes-filtered-true.vcf.gz" \
 --max-missing 0.50 --maf 0.05 --recode --recode-INFO-all --out \
 "pcod_rnaseq_genotypes-final_miss50"

 vcftools --gzvcf \
 "pcod_rnaseq_genotypes-filtered-true.vcf.gz" \
 --max-missing 0.70 --maf 0.05 --recode --recode-INFO-all --out \
 "pcod_rnaseq_genotypes-final_miss30"
```

### Impute missing genotypes - _START HERE_ 
Before running GWAS I'm going to try to impute missing genotypes using our reference panel (VCF of genotypes) built from the ~600 reference fish plus the ~50 big/little 2021 fish that were collected off Kodiak. 

```
java -jar /home/lspencer/programs/beagle.v5.4.jar nthreads=10 gt=/home/lspencer/pcod-juvenile/variants/pcod_rnaseq_genotypes-final_miss50.vcf.gz ref=######## gp=true impute=true out=/home/lspencer/pcod-juvenile/variants
```
