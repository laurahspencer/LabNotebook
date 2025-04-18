---
layout: post
title: GWAS Day 5 - trouble shooting etc. 
--- 

## Hunting for primer sequences for a benchtop male/female assay for Pacific cod
I met with Mary Beth to discuss how we could potentially identify sequences for new primers that could identify Pacific cod sex. The reference Pacific cod genome may have been generated from a female, and thus lacks the male-specific "Y" region of chromosome 11 (presumably). 

#### Get reference sequences for alignment 

I downloaded the **gadmor2.1.gz** fasta file from [FigShare](https://figshare.com/s/313f8fe1fdcc82571a99?file=12351962), which is the sequence published alongside the Atlantic cod sex marker paper, [Kirubakaran et al. 2019](https://www.nature.com/articles/s41598-018-36748-8#data-availability), renamed it, then looked at the chromosome headers/names to see what we're working with. There are two LG11 sequences, one that appears to be female-specific, and the other one is presumably male-specific (?). I extracted those two and saved to a separate file. I'll use those to re-align our lcWGS data from sexed fish. 

```
wget https://figshare.com/ndownloader/files/12351962?private_link=313f8fe1fdcc82571a99
seqtk subseq <(zcat gadmor2.1.gz) gadmor2.1_chr11.txt | gzip > gadmor2.1_chr11.fasta
mv 12351962?private_link=313f8fe1fdcc82571a99 gadmor2.1.gz 
zcat gadmor2.1.gz | grep ">" 
  >LG11-12518000-X-specific-seq-and-flanks
  >LG11
echo -e "LG11\nLG11-12518000-X-specific-seq-and-flanks" > gadmor2.1_chr11.txt
module load bio/seqtk
seqtk subseq <(zcat gadmor2.1.gz) gadmor2.1_chr11.txt | gzip > gadmor2.1_chr11.fasta.gz    #extract just LG11 sequences 
```
#### Create lists of trimmed male/female R1/R2 files: 
```
awk '{print $0 "\t/home/lspencer/pcod-lcwgs-2023/analysis-20240606/reference/trimmed/" $0 "_trimmed_clipped_R1_paired.fq.gz"}' ${output_dir}/male-ids.txt | cut -f2 > ${output_dir}/male-fq-R1.txt
awk '{print $0 "\t/home/lspencer/pcod-lcwgs-2023/analysis-20240606/reference/trimmed/" $0 "_trimmed_clipped_R2_paired.fq.gz"}' ${output_dir}/male-ids.txt | cut -f2 > ${output_dir}/male-fq-R2.txt
awk '{print $0 "\t/home/lspencer/pcod-lcwgs-2023/analysis-20240606/reference/trimmed/" $0 "_trimmed_clipped_R1_paired.fq.gz"}' ${output_dir}/female-ids.txt | cut -f2 > ${output_dir}/female-fq-R1.txt
awk '{print $0 "\t/home/lspencer/pcod-lcwgs-2023/analysis-20240606/reference/trimmed/" $0 "_trimmed_clipped_R2_paired.fq.gz"}' ${output_dir}/female-ids.txt | cut -f2 > ${output_dir}/female-fq-R2.txt

grep -Fxf <(cut -f2 temp-0/0-samples.txt) ../pcod-exp_filtered_bamslist.txt > temp-0/bamslist_0.txt
/home/lspencer/pcod-lcwgs-2023/analysis-20240606/reference/scripts/pcod-refs_alignARRAY_input.txt
```

#### Concatenate all fastq files by sex 
I concatenated all trimmed fastq files by sex and read (R1, R2) using the script **concat-by-sex.sh**
```
module load bio/seqtk
input_dir="/home/lspencer/pcod-lcwgs-2023/analysis-20240606/reference/trimmed"
output_dir="/home/lspencer/pcod-sex"

# Concatenate male reads
cat ${output_dir}/male-fq-R1.txt | xargs -I{} zcat {} | gzip > ${output_dir}/male-R1.fq.gz
#cat ${output_dir}/male-fq-R2.txt | xargs -I{} zcat {} | gzip > ${output_dir}/male-R2.fq.gz

# Concatenate female reads
#cat ${output_dir}/female-fq-R1.txt | xargs -I{} zcat {} | gzip > ${output_dir}/female-R1.fq.gz
#cat ${output_dir}/female-fq-R2.txt | xargs -I{} zcat {} | gzip > ${output_dir}/female-R2.fq.gz
```

#### Align concatenated reads

I use the script **alignARRAY.sh** to align each concatenated batch of male & female reads to Atlantic cod chromosome 11 sequences 

```
module unload aligners/bwa/0.7.17 bio/samtools/1.11 bio/bamtools/2.5.1 bio/picard/2.23.9 bio/bamutil/1.0.5
module load aligners/bwa/0.7.17 bio/samtools/1.11 bio/bamtools/2.5.1 bio/picard/2.23.9 bio/bamutil/1.0.5

base=/home/lspencer/pcod-sex
ref=${base}/gadmor2.1_chr11.fasta
align=${base}/align

JOBS_FILE=/home/lspencer/pcod-sex/alignARRAY_input.txt
IDS=$(cat ${JOBS_FILE})

for sample_line in ${IDS}
do
        job_index=$(echo ${sample_line} | awk -F ":" '{print $1}')
        fq_r1=$(echo ${sample_line} | awk -F ":" '{print $2}')
        fq_r2=$(echo ${sample_line} | awk -F ":" '{print $3}')
        if [[ ${SLURM_ARRAY_TASK_ID} == ${job_index} ]]; then
                break
        fi
done

sample_id=$(echo $fq_r1 | sed 's!^.*/!!')
sample_id=${sample_id%%_*}

bwa index ${ref}

bwa mem -M -t 10 ${ref} ${fq_r1} ${fq_r2} 2> ${align}/${sample_id}_bwa-mem.out > ${align}/${sample_id}.sam

samtools view -bS -F 4 ${align}/${sample_id}.sam > ${align}/${sample_id}.bam
rm ${align}/${sample_id}.sam

samtools view -h ${align}/${sample_id}.bam | samtools view -buS - | samtools sort -o ${align}/${sample_id}_sorted.bam
rm ${align}/${sample_id}.bam

java -jar $PICARD MarkDuplicates I=${align}/${sample_id}_sorted.bam O=${align}/${sample_id}_sorted_dedup.bam M=${align}/${sample_id}_dups.log VALIDATION_STRINGENCY=SILENT REMOVE_DUPLICATES=true
rm ${align}/${sample_id}_sorted.bam

bam clipOverlap --in ${align}/${sample_id}_sorted_dedup.bam --out ${align}/${sample_id}_sorted_dedup_clipped.bam --stats
rm ${align}/${sample_id}_sorted_dedup.bam

samtools depth -aa ${align}/${sample_id}_sorted_dedup_clipped.bam | cut -f 3 | gzip > ${align}/${sample_id}.depth.gz

samtools index ${align}/${sample_id}_sorted_dedup_clipped.bam
```
To be continued ... 

### Trying to correctly impute genotypes and subset beagle for temp-specific and lipid-specific GWAS 
I'm running into issues with GWAS when I use beagle files that I've subset for specific samples. I'm thinking this is because I changed the header line to include sample IDs?  Not sure, but it's worth a try to see if that's the cause. Here I re-try to subest beagle GLs file for only samples with lipid data (based on sample IDs in header column), rename the header to change it back to Ind0, Ind1, Ind2 etc., impute using Beagle v3, then run GWAS to identify composite index (growth + condition + lipid storage) associated loci. 

THIS DIDN'T WORK. Keeping the code here in case it's useful in the future. 

#### Step 0. Subset beagle for only lipid samples 
Did so in the script **subset-merged-beagle.sh** 

```
# Input files
input="/home/lspencer/pcod-lcwgs-2023/analysis-20240606/wgsassign2"
input_beagle="${input}/join-beagles-temp/rehead_beagle2.gz"
id_file="/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas/samples-with-lipids.txt"
output_beagle="/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas/experimental-with-lipids.beagle.gz"
output_beagle_rehead="/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas/experimental-with-lipids-rehead.beagle.gz"

# Extract IDs from the id_file into a regex pattern (exact match)
ids=$(awk '{print "^" $1 "$"}' $id_file | paste -sd '|' -)

# Process the input file
zcat $input_beagle | awk -v ids="$ids" '
BEGIN { FS=OFS="\t"; }
NR==1 {
    # Print the first three columns (marker, allele1, allele2)
    header_line = $1 OFS $2 OFS $3;
    for (i=4; i<=NF; i++) {
        # Remove suffixes and check if the column matches any ID
        col_name = gensub(/_AA$|_AB$|_BB$/, "", "g", $i)
        if (col_name ~ ids) {
            header_line = header_line OFS $i
            cols_to_print[i] = 1
        }
    }
    print header_line
}
NR>1 {
    line = $1 OFS $2 OFS $3;
    for (i=4; i<=NF; i++) {
        if (i in cols_to_print) {
            line = line OFS $i
        }
    }
    print line
}
' | gzip > $output_beagle
```

#### Step 1. Change header line to not include sample IDs 
Also did so in the script **subset-merged-beagle.sh**: 

```
### Create rename_beagle.tab file:
zcat ${output_beagle} | head -n 1 | tr '\t' '\n' | tail -n +4 | \
awk 'BEGIN {count=0}
     {
       if (NR % 3 == 1) count++;  # Increment group every 3 rows
       print $0 "\tInd" count-1  # Assign Ind<number> based on group
     }' > rename_beagle.tab

### Rename beagle 
mapping_file="rename_beagle.tab"
zcat "$output_beagle" > temp_input_file.tsv

while IFS=$'\t' read -r old new; do
  sed -i "s/${old}/${new}/g" temp_input_file.tsv
done < "$mapping_file"

gzip -c temp_input_file.tsv > "$output_beagle_rehead"
rm temp_input_file.tsv
```

#### Step 2: Impute missing genotype probabilities 
This is in script ** but I ran it interactively on Sedna. 

```
module load bio/angsd/0.940

base=/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental
beagle_lipids=${base}/gwas/experimental-with-lipids2_renamed.beagle.gz
impute_lipids=impute

java -Xmx15000m -jar /home/lspencer/programs/beagle.jar \
> like=${beagle_lipids} \
> out=${base}/gwas/impute-test
```

#### Step 4: Run GWAS 
```
angsd \
-doMaf 4 \
-beagle ${impute_lipids} \
-yQuant ${base}/gwas/cpi.txt \
-doAsso 4 \
-cov ${base}/gwas/lipid-only-temp-covar.txt \
-out ${base}/gwas/gwas-CPI.out \
-fai /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic.fna.fai
```

NOPE. Didnt work. 

## New pipeline to run GWAS on specific subets of samples (for each temperature, for samples with lipid data)  
Perhaps I need to have original genotype likelihood files (e.g. beagle, bcf/vcf) directly output from ANGSD to run GWAS, rather than using my custom script to subset a beagle files for samples of interest. Let's do that here while also trying to use a reference panel during the genotype imputation step. 

### Step 0. Generate reference panel of genotype (probabilties?) 

I ran ANGSD using all our reference fish (from a variety of marine regions, low coverage, 3x) PLUS the Big/Little 2021 fish (moderate coverage, 14x). I did this in the directory /home/lspencer/pcod-general/imputation-ref-panel/ using the script **angsd-impute-ref-panel.sh**:
```
#!/bin/bash

#SBATCH --cpus-per-task=10
#SBATCH --time=0-20:00:00
#SBATCH --job-name=angsd
#SBATCH --output=/home/lspencer/pcod-general/imputation-ref-panel/angsd-ref-panel_%A-%a.out
#SBATCH --mail-type=ALL
#SBATCH --mail-user=lspencer@noaa.gov
#SBATCH --array=1-24%24

module unload bio/angsd/0.940
module load bio/angsd/0.940

JOBS_FILE=/home/lspencer/pcod-general/imputation-ref-panel/angsdARRAY_input.txt
IDS=$(cat ${JOBS_FILE})

for sample_line in ${IDS}
do
        job_index=$(echo ${sample_line} | awk -F ":" '{print $1}')
        contig=$(echo ${sample_line} | awk -F ":" '{print $2}')
        if [[ ${SLURM_ARRAY_TASK_ID} == ${job_index} ]]; then
                break
        fi
done

angsd -b /home/lspencer/pcod-general/imputation-ref-panel/bamslist-ref-panel.txt -ref /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic.fa \
-r ${contig}: -out /home/lspencer/pcod-general/imputation-ref-panel/panel_${contig}_polymorphic \
-nThreads 10 -uniqueOnly 1 -doCounts 1 -setMinDepth 700 -remove_bads 1 -trim 0 -C 50 -minMapQ 35 -minQ 30 \
-dobcf 1 -doGlf 2 -GL 1 -doMaf 1 -doMajorMinor 1 -dopost 1 -dogeno 32 \
-minMaf 0.05 -SNP_pval 1e-10 -only_proper_pairs 1
```

This created separate genotype likelihood files for each chromosome. I made sure BCF files were included in the ouptut, with the option `-dobcf 1` (I tried using -doVcf but it said that was deprecated). Now I want to merge and convert genotype data into one whole-genome VCF file. 
```
module load bio/bcftools

# Index bcf files
for bcf_file in *.bcf; do
    echo "Indexing $bcf_file..."
    bcftools index "$bcf_file"
done

# concatenate and convert to vcf 
bcftools concat -O b -o whole-genome.bcf $(ls *.bcf | sort)
bcftools view -i 'F_MISSING<0.15' whole-genome.bcf -o whole-genome_miss15.vcf -O v # filter for missingness<15%) #365,950 siotes
bcftools view -i 'F_MISSING<0.10' whole-genome.bcf -o whole-genome_miss10.vcf -O v # filter for missingness<10%) #124,663 sites
bcftools view -i 'F_MISSING<0.05' whole-genome.bcf -o whole-genome_miss5.vcf -O v # filter for missingness<5%) #12,379 sites
bcftools view -i 'F_MISSING=0' whole-genome.bcf -o whole-genome_miss0.vcf -O v # filter for missingness=0) #81 sites 
bgzip -c whole-genome_miss15.vcf > whole-genome_miss15.vcf.gz
tabix -p vcf whole-genome.vcf.gz
```

### Step 1. Re-run ANGSD multiple times, once for each group (temp, lipid samples) 
Generate new bamslists for each temperature treatment and for samples with lipids 
```
cd /home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas
awk '{print $0 "\t/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/bamtools/pcod-exp_" $0 "_sorted_dedup_clipped.bam"}' temp-0/0-samples.txt > temp-0/temp_file && mv temp-0/temp_file temp-0/0-samples.txt
awk '{print $0 "\t/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/bamtools/pcod-exp_" $0 "_sorted_dedup_clipped.bam"}' temp-5/5-samples.txt > temp-5/temp_file && mv temp-5/temp_file temp-5/5-samples.txt
awk '{print $0 "\t/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/bamtools/pcod-exp_" $0 "_sorted_dedup_clipped.bam"}' temp-9/9-samples.txt > temp-9/temp_file && mv temp-9/temp_file temp-9/9-samples.txt
awk '{print $0 "\t/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/bamtools/pcod-exp_" $0 "_sorted_dedup_clipped.bam"}' temp-16/16-samples.txt > temp-16/temp_file && mv temp-16/temp_file temp-16/16-samples.txt
awk '{print $0 "\t/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/bamtools/pcod-exp_" $0 "_sorted_dedup_clipped.bam"}' samples-with-lipids/samples-with-lipids.txt > samples-with-lipids/temp_file && mv samples-with-lipids/temp_file samples-with-lipids/samples-with-lipids.txt

grep -Fxf <(cut -f2 temp-0/0-samples.txt) ../pcod-exp_filtered_bamslist.txt > temp-0/bamslist_0.txt
grep -Fxf <(cut -f2 temp-5/5-samples.txt) ../pcod-exp_filtered_bamslist.txt > temp-5/bamslist_5.txt
grep -Fxf <(cut -f2 temp-9/9-samples.txt) ../pcod-exp_filtered_bamslist.txt > temp-9/bamslist_9.txt
grep -Fxf <(cut -f2 temp-16/16-samples.txt) ../pcod-exp_filtered_bamslist.txt > temp-16/bamslist_16.txt
grep -Fxf <(cut -f2 samples-with-lipids/samples-with-lipids.txt) ../pcod-exp_filtered_bamslist.txt > samples-with-lipids/bamslist_samples-with-lipids.txt
```

Run ANGSD separately for each temperature group. I reset min/max depth and nIndividuals based on 40 samples/treatment. I generated lots of output files, not knowing what I'll need for imputation.
```
cat temp-16/angsdARRAY.sh
#!/bin/bash

#SBATCH --cpus-per-task=10
#SBATCH --time=7-00:00:00
#SBATCH --job-name=angsd_16
#SBATCH --output=/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas/temp-16/angsd_%A-%a.out
#SBATCH --mail-type=FAIL
#SBATCH --mail-user=laura.spencer@noaa.gov
#SBATCH --array=1-24%6

## ANGSD Settings
# -setminDepth 50 is based on 50% of the individuals and the average sequencing coverage of 3X (i.e. 50 ~ 3x * 0.5*40)
# -setmaxDepth 300 is based on a very generous 2 times all the individuals and the average depth, to avoid sites overly sequenced (i.e. 300 >~ 2 * 40 * 3x)
# -minInd 20 sets calling only variants with ~50% of individuals
# -minMaqQ was 15, now 30
# -minQ was 15, now 33
# -GL was 1, now 2

module unload bio/angsd/0.933
module load bio/angsd/0.933
base=/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental
temp="16"
workdir=${base}/gwas/temp-${temp}
bams=${workdir}/bamslist_${temp}.txt

# array input is chromosomes
JOBS_FILE=${base}/scripts/pcod-exp_angsdARRAY_input.txt
IDS=$(cat ${JOBS_FILE})

for sample_line in ${IDS}
do
        job_index=$(echo ${sample_line} | awk -F ":" '{print $1}')
        contig=$(echo ${sample_line} | awk -F ":" '{print $2}')
        if [[ ${SLURM_ARRAY_TASK_ID} == ${job_index} ]]; then
                break
        fi
done

angsd -b ${bams} \
-ref /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic.fa \
-r ${contig}: \
-out ${workdir}/Chr${job_index}_${temp} \
-nThreads 10 \
-uniqueOnly 1 \
-remove_bads 1 \
-trim 0 \
-C 50 \
-minMapQ 30 \
-minQ 33 \
-doCounts 1 \
-setminDepth 50 \
-setmaxDepth 300 \
-GL 2 \
-doBcf 1 \
-doPost 1 \
-doGeno 32 \
-doGlf 2 \
-doMaf 1 \
-doMajorMinor 1 \
-doDepth 1 \
-dumpCounts 3 \
-only_proper_pairs 1 \
-minInd 20 \
-minmaf 0.05 \
-SNP_pval 1e-10 \
-baq 1
```

### Step 2- Concatenate chromosome-specific beagles into whole genome beagle (for each temperature)  
Using scripts `concat.sh`:

```
# Define the input directory containing all .beagle.gz files
temp="16"
input_dir="/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas/temp-${temp}"
output_file="${input_dir}/wholegenome-${temp}.beagle"

# Extract the header from the first .beagle.gz file (Chr1_16.beagle.gz)
zcat "${input_dir}/Chr1_16.beagle.gz" | head -n 1 > "${output_file}"

# Loop through chromosomes 1 to 24 and concatenate their contents (excluding headers)
for i in {1..24}; do
    file="${input_dir}/Chr${i}_16.beagle.gz"
    if [[ -f "$file" ]]; then
        echo "Processing $file..."
        zcat "$file" | tail -n +2 >> "${output_file}"
    else
        echo "Warning: File $file not found. Skipping..."
    fi
done

# Compress the resulting .beagle file
gzip "${output_file}"
```

Here are how many sites there are in each beagle: 
- temp-0/wholegenome-0.beagle.gz -- 472,976  
- temp-5/wholegenome-5.beagle.gz -- 499,738  
- temp-9/wholegenome-9.beagle.gz -- 467,064  
- temp-16/wholegenome-16.beagle.gz -- 426,761  

### Step 3- Impute genotypes (fill in missing or low confidence genotypes) based on linkage 
Using the script `impute.sh` I perform imputation twice:

1. Without a reference, using Beagle v3
```
java -Xmx15000m -jar /home/lspencer/programs/beagle.jar like=${base}/temp-0/wholegenome-0.beagle.gz out=${base}/temp-0/wholegenome-0-imputed.beagle.gz
java -Xmx15000m -jar /home/lspencer/programs/beagle.jar like=${base}/temp-5/wholegenome-5.beagle.gz out=${base}/temp-5/wholegenome-5-imputed.beagle.gz
java -Xmx15000m -jar /home/lspencer/programs/beagle.jar like=${base}/temp-9/wholegenome-9.beagle.gz out=${base}/temp-9/wholegenome-9-imputed.beagle.gz
java -Xmx15000m -jar /home/lspencer/programs/beagle.jar like=${base}/temp-16/wholegenome-16.beagle.gz out=${base}/temp-16/wholegenome-16-imputed.beagle.gz
```
3. With my attempt at a reference panel, which I generated from reference fish + big/little 2021 fish. This first required concatenating the BCF files containing genotypes (for each temperature) then convert them to VCF 
```
bcftools concat -O b -o whole-genome-0.bcf $(for i in {1..24}; do echo "Chr${i}_0.bcf"; done)  
bcftools concat -O b -o whole-genome-5.bcf $(for i in {1..24}; do echo "Chr${i}_5.bcf"; done)  
bcftools concat -O b -o whole-genome-9.bcf $(for i in {1..24}; do echo "Chr${i}_9.bcf"; done)
bcftools concat -O b -o whole-genome-16.bcf $(for i in {1..24}; do echo "Chr${i}_16.bcf"; done)

bcftools view whole-genome-0.bcf -o whole-genome-0.vcf -O v
bcftools view whole-genome-5.bcf -o whole-genome-5.vcf -O v
bcftools view whole-genome-9.bcf -o whole-genome-9.vcf -O v
bcftools view whole-genome-16.bcf -o whole-genome-16.vcf -O v
```
Now I try to run beagle v4.1 with the "reference panel"
```
ref=/home/lspencer/pcod-general/imputation-ref-panel/whole-genome_miss15.vcf.gz
base=/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas

java -Xmx15000m -jar /home/lspencer/programs/beagle.v4.1.jar gl=${base}/temp-0/whole-genome-0.vcf ref=${ref} out=${base}/temp-0/wholegenome-0-imputed-ref.vcf
java -Xmx15000m -jar /home/lspencer/programs/beagle.v4.1.jar gl=${base}/temp-5/whole-genome-5.vcf ref=${ref} out=${base}/temp-5/wholegenome-5-imputed-ref.vcf
java -Xmx15000m -jar /home/lspencer/programs/beagle.v4.1.jar gl=${base}/temp-9/whole-genome-9.vcf ref=${ref} out=${base}/temp-9/wholegenome-9-imputed-ref.vcf
java -Xmx15000m -jar /home/lspencer/programs/beagle.v4.1.jar gl=${base}/temp-16/whole-genome-16.vcf ref=${ref} out=${base}/temp-16/wholegenome-16-imputed-ref.vcf

