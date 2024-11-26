---
layout: post
title: Refining growth/condition marker discovery in Pacific cod, GWAS Day 4
--- 

I am interested to see whether I can use the putative growth/condition markers detected in experimental Pacific cod to predict "performance" in our reference/wild caught cod from different marine regions (Northern Bering Sea, Aleutians, Eastern Bering Sea / western Gulf of Alaska, eastern Gulf of Alaska). I have a beagle file containing genotype likelihoods for sites detected in both experimental and reference fish. I filtered that beagle file for only our putative growth/performance markers, then ran PCAngsd to generate a covariance matrix, then PCA in R. My thought is that I could potentially use a growth/condition gradient along a PC axis from experimental fish to identify reference fish populations/regions that lie on the high growth/condition end (i.e. potential high performers). 

Here is the PCA of those ~270 putative growth/condition markers with growth/condition index of experimental fish indicated by the gradient, and all reference fish in gray. Second figure is a regression of growth/condition index ~ PC1 scores for experimental fish only, which I could possibly use to potentially predict performance in reference fish - the more negative PC1 score the better?. The boxplot shows PC1 scores for reference fish, potentially indicating eGOA fish have the most negative PC1 score / best performance indicator and tagged/NBS fish have the highest PC1 scor. 

![image](https://github.com/user-attachments/assets/e1a235ec-4f58-48e3-840f-035b96edf113)

![image](https://github.com/user-attachments/assets/619d5915-c367-4444-8938-4d2abab914d1)

![image](https://github.com/user-attachments/assets/2d8adf08-fb3c-453a-ae2a-723d0c20a6bc)

#### What's happening here
This is strange, though. When I plot PC1 scores for experimental fish alongside reference fish, obviously the variability is much higher in experimental fish. BUT also - there are differences among temperature treatments, which isn't a good sign. I believe this indicates that the marker discovery is being influenced by growth/condition differences among treatments. This suggests to me that I need to run my GWAS analyses separately for each temperature and look for 1) common markers among treatments that could indicate general growth/condition performance, and 2) unique markers among treatments that could reflect temperature-specific markers.  

![image](https://github.com/user-attachments/assets/7cc69e5c-470f-4df3-a0c5-bc865fb47475)

## Re-run GWAS separately for each temperature 
Here I perform four separate GWAS runs for each temperature treatment (0,5,9,16), with n=40/treatment for all but the 16C group (n=37, unfortunately we don't have good DNA data for three that died!). I created four gwas subdirectories (temp-#), re-ran ANGSD to generate beagle files with genotype likelihoods, then ran `angsd -doAsso`. Here is an example set of scripts used for 0C group, located in the directory **/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas/temp-0/**:  

### angsdARRAY.sh - generate GLs (and some other files) for each chromosome with desired ANGSD settings 
```
#!/bin/bash

#SBATCH --cpus-per-task=10
#SBATCH --time=7-00:00:00
#SBATCH --job-name=angsd_0
#SBATCH --output=/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas/temp-0/angsd_%A-%a.out
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
temp="0"
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
### concat.sh - concatenage beagles (containing GLs) into one whole genome beagle file 
```
#!/bin/bash

#SBATCH --cpus-per-task=10
#SBATCH --job-name=concat-beagles-0
#SBATCH --mail-type=ALL
#SBATCH --mail-user=laura.spencer@noaa.gov
#SBATCH --output=/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas/temp-0/concat.out

# Define the input directory containing all .beagle.gz files
temp="0"
input_dir="/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas/temp-${temp}"
output_file="${input_dir}/wholegenome-${temp}.beagle"

# Extract the header from the first .beagle.gz file
zcat "${input_dir}/Chr1_0.beagle.gz" | head -n 1 > "${output_file}"

# Loop through chromosomes 1 to 24 and concatenate their contents (excluding headers)
for i in {1..24}; do
    file="${input_dir}/Chr${i}_0.beagle.gz"
    if [[ -f "$file" ]]; then
        echo "Processing $file..."
        zcat "$file" | tail -n +2 >> "${output_file}"
    else
        echo "Warning: File $file not found. Skipping..."
    fi
done

# Compress the resulting .beagle file
gzip "${output_file}"

echo "Concatenation complete. Output written to ${output_file}.gz"
```

### GWAS.sh - Impute genotype probabilities, and perform association tests using various traits using both  "raw" GLs and imputed GPs 
_NOTE: here I only show code for GWAS using the raw GLs, since imputed GPs didn't produce any results (not sure why)_ 

```
#!/bin/bash
#SBATCH --time=0-20:00:00
#sbatch -p himem
#SBATCH --job-name=gwas-0
#SBATCH --mail-type=ALL
#SBATCH --mail-user=laura.spencer@noaa.gov
#SBATCH --output=/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas/temp-0/gwas-0.out

module load bio/angsd/0.940

temp="0"
base=/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas
beagle=${base}/temp-${temp}/wholegenome-${temp}.beagle.gz
imputed=${base}/temp-${temp}/wholegenome-${temp}-imputed.beagle.gz.wholegenome-${temp}.beagle.gz.gprobs.gz

# ---------------------
#  CPI from all 4 growth and condition metrics (SGR-sl, SGR-ww, HSI, Kwet)
angsd \
-doMaf 4 \
-beagle ${beagle} \
-yQuant ${base}/temp-${temp}/${temp}-pi.grow.cond1.txt \
-doAsso 4 \
-out ${base}/temp-${temp}/${temp}-gwas-pi-grow-cond1.out \
-fai /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic.fna.fai

# ------------------
#  CPI from 3 growth and condition metrics (SGR-sl, SGR-ww, HSI)
angsd \
-doMaf 4 \
-beagle ${beagle} \
-yQuant ${base}/temp-${temp}/${temp}-pi.grow.cond2.txt \
-doAsso 4 \
-out ${base}/temp-${temp}/${temp}-gwas-pi-grow-cond2.out \
-fai /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic.fna.fai

# ------------------
#  CPI from 2 growth and condition metrics (SGR-ww, HSI)
angsd \
-doMaf 4 \
-beagle ${beagle} \
-yQuant ${base}/temp-${temp}/${temp}-pi.grow.cond3.txt \
-doAsso 4 \
-out ${base}/temp-${temp}/${temp}-gwas-pi-grow-cond3.out \
-fai /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic.fna.fai

# ------------------
# SGR - standard length
angsd \
-doMaf 4 \
-beagle ${beagle} \
-yQuant ${base}/temp-${temp}/${temp}-sgr-sl.txt \
-doAsso 4 \
-out ${base}/temp-${temp}/${temp}-gwas-sgr-sl.out \
-fai /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic.fna.fai

# SGR - wet weight
angsd \
-doMaf 4 \
-beagle ${beagle} \
-yQuant ${base}/temp-${temp}/${temp}-sgr-ww.txt \
-doAsso 4 \
-out ${base}/temp-${temp}/${temp}-gwas-sgr-ww.out \
-fai /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic.fna.fai

# HSI
angsd \
-doMaf 4 \
-beagle ${beagle} \
-yQuant ${base}/temp-${temp}/${temp}-hsi.txt \
-doAsso 4 \
-out ${base}/temp-${temp}/${temp}-gwas-hsi.out \
-fai /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic.fna.fai

# KWet
angsd \
-doMaf 4 \
-beagle ${beagle} \
-yQuant ${base}/temp-${temp}/${temp}-kwet.txt \
-doAsso 4 \
-out ${base}/temp-${temp}/${temp}-gwas-kwet.out \
-fai /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic.fna.fai
```
### Examine GWAS results in R 

Here is R Code I used to import, filter, view, and annotate GWAS-identified "putative markers": 
```
temp="0"

#lrt.temp<-read.table(gzfile(paste0("../integrated/gwas/by-temp/", temp, "-gwas-hsi.out.lrt0.gz")), header=T, sep="\t")
#lrt.temp<-read.table(gzfile(paste0("../integrated/gwas/by-temp/", temp, "-gwas-kwet.out.lrt0.gz")), header=T, sep="\t")
lrt.temp<-read.table(gzfile(paste0("../integrated/gwas/by-temp/", temp, "-gwas-sgr-sl.out.lrt0.gz")), header=T, sep="\t")
#lrt.temp<-read.table(gzfile(paste0("../integrated/gwas/by-temp/", temp, "-gwas-sgr-ww.out.lrt0.gz")), header=T, sep="\t")
#lrt.temp<-read.table(gzfile(paste0("../integrated/gwas/by-temp/", temp, "-gwas-pi-grow-cond1.out.lrt0.gz")), header=T, sep="\t")
#lrt.temp<-read.table(gzfile(paste0("../integrated/gwas/by-temp/", temp, "-gwas-pi-grow-cond2.out.lrt0.gz")), header=T, sep="\t")
#lrt.temp<-read.table(gzfile(paste0("../integrated/gwas/by-temp/", temp, "-gwas-pi-grow-cond3.out.lrt0.gz")), header=T, sep="\t")

#we have a few LRT values that are -999, we should remove them. 
length(lrt.temp$LRT) # number of loci 
length(which(lrt.temp$LRT == -999)) #number that will be removed 

length(which(lrt.temp$LRT != -999))

#remove the values that are not -999 and that are negative
lrt.temp_filt<-lrt.temp[-c(which(lrt.temp$LRT == -999),which(lrt.temp$LRT <= 0)),]

#add snp "name" from rownumber 
lrt.temp_filt$SNP<-paste("r",1:length(lrt.temp_filt$Chromosome), sep="")

# get pvalues from chisq 
lrt.temp_filt$pvalue<-pchisq(lrt.temp_filt$LRT, df=1, lower=F)

hist(lrt.temp_filt$LRT, breaks=50)
hist(lrt.temp_filt$pvalue, breaks=50)
qqnorm(lrt.temp_filt$pvalue)

# #we also need to make sure we don't have any tricky values like those below
# lrt.temp_filt<-lrt.temp_filt[-c(which(lrt.temp_filt$pvalue == "NaN" ),
#                       which(lrt.temp_filt$pvalue == "Inf"),
#                       which(lrt.temp_filt$LRT == "Inf")),]
                      
# jpeg(filename = paste0("../integrated/gwas/by-temp/", temp, "-growth-condition-logp3-manhattan.jpeg"),
#      width = 900, height = 450, quality = 100)
manhattan(lrt.temp_filt %>% mutate(chr=as.numeric(as.factor(Chromosome))), 
          chr="chr", bp="Position", p="pvalue", genomewideline = 3, cex=1, suggestiveline = F,
          main=paste0("Putative growth/condition associated markers\n Temperature = ", temp), 
          highlight = (lrt.temp_filt %>% mutate(logp=-log10(pvalue)) %>% filter(logp>3))$SNP)
#dev.off()

lrt.temp_filt %>% mutate(logp=-log10(pvalue)) %>% filter(logp>3) %>% arrange(desc(LRT)) %>%
  unite("marker", Chromosome:Position, sep="_") %>% select(marker) %>% 
  write_delim(file=paste0("../integrated/gwas/", temp, "-gc1-markers-logp3.txt"),  col_names = F)

# did this already in previous code chunk 
# # Prepare gene regions with a 2000 bp flank
# genes4gwas <- pcod.gtf %>% mutate(ncbi_id=paste0("GeneID:", ncbi_id)) %>% 
#   filter(feature=="gene") %>% 
#   mutate(start_flank=start-2000) %>%
#   mutate(start_flank=as.integer(case_when(start_flank<0~0, TRUE~start_flank))) #%>% 
# 
# gene_ranges <- GRanges(
#   seqnames =genes4gwas$seqname,
#   ranges=IRanges(
#   start=genes4gwas$start_flank,
# #  start = genes4gwas$start,
#   end=genes4gwas$end),
#   ncbi_id=genes4gwas$ncbi_id,
#   gene_id=genes4gwas$gene_id)

# Prepare SNP sites
gc1.sites.temp <- lrt.temp_filt %>% mutate(logp=-log10(pvalue)) %>% filter(logp>3) %>% mutate(marker=paste0(Chromosome, "_", Position))

snp_ranges.temp <- GRanges(
  seqnames = gc1.sites.temp$Chromosome,
  ranges=IRanges(
    start = gc1.sites.temp$Position,
    end = gc1.sites.temp$Position),
  pvalue=gc1.sites.temp$pvalue,
  Major=gc1.sites.temp$Major,
  Minor=gc1.sites.temp$Minor)

# Find overlaps between gene flanks and SNP sites
overlaps.temp <- findOverlaps(gene_ranges, snp_ranges.temp)

# Extract matching rows
overlapping_genes.temp <- genes4gwas[queryHits(overlaps.temp), ]
overlapping_snps.temp <- gc1.sites.temp[subjectHits(overlaps.temp), ]

# Combine results into a single data frame
result.temp <- cbind(overlapping_genes.temp, overlapping_snps.temp) %>% 
  mutate(feature=case_when(Position>=start~"gene", Position<start~"upstream")) %>% 
  dplyr::select(-c(score, frame, attributes,biotype, exon)) %>% 
#  dplyr::select(feature, seqname, strand, start_flank, start, end, Position, gene_id, ncbi_id, gene_id) %>% 
  left_join(pcod.blast.GO[c("ncbi_id", "spid", "gene", "protein_names", "gene_ontology_biological_process")] %>% 
              mutate(ncbi_id=paste0("GeneID:", ncbi_id)), by="ncbi_id")

# result.temp %>% dplyr::select(-Position) %>% distinct() %>% group_by(feature) %>%  tally() %>% 
#   mutate(total=nrow(genes4gwas)) %>% mutate(perc=round(100*n/total, 2))

# Table of putative markers 
result.temp %>% dplyr::select(Chromosome, Position, LRT, pvalue, spid, protein_names) %>% 
ungroup() %>% 
distinct() %>% arrange(desc(LRT)) %>% 
mutate(across(c(LRT:pvalue), ~ signif(.x, 3))) #%>% write_clip()

# NOTE: this beagle contains GLs for ALL treatments, and is missing some markers that were identified in the treatment-specific ANGSD run 
exp.beagle.gc1.sites.temp <- 
  exp.beagle %>% filter(marker %in% gc1.sites.temp$marker) %>% 
  pivot_longer(X100_1:X9_3, names_to = "individual.allele", values_to = "probability") %>% 
  separate(individual.allele, into=c("individual", "allele"), sep = "_") %>% 
  mutate(allele=case_when(
    allele==1 ~ paste0(allele1, "/", allele1),
    allele==2 ~ paste0(allele1, "/", allele2),
    allele==3 ~ paste0(allele2, "/", allele2))) %>% 
    mutate(allele_base = allele %>%
           str_replace_all("0", "A") %>%
           str_replace_all("1", "C") %>%
           str_replace_all("2", "G") %>%
           str_replace_all("3", "T")) %>% 
  mutate(probability=round(as.numeric(probability), digits=4)) %>% 
  group_by(marker, individual) %>% 
  filter(!(n() == 3 & all(probability == 0.3333))) %>%  
  slice_max(probability,n=1) %>% 
    mutate(individual=gsub("X", "", individual), allele_base) %>%
    left_join(phenotypes2, by=c("individual"="genetic_sampling_count")) %>%
mutate(temperature = replace_na(temperature, "16")) #%>% 
#  filter(temperature==temp)


gc1.markers.temp <- (gc1.sites.temp %>% filter(marker %in% exp.beagle$marker) %>% arrange(desc(LRT)))$marker
for (i in 1:length(gc1.markers.temp)) {
  meta <- exp.beagle.gc1.sites.temp %>% filter(marker==gc1.markers.temp[i]) %>% ungroup() %>%  
  select(marker) %>% distinct() %>%  
  left_join(result.temp %>% mutate(marker=paste0(Chromosome, "_", Position))) 
  
  print(exp.beagle.gc1.sites.temp %>% filter(marker==gc1.markers.temp[i]) %>% #filter(probability>0.75) %>% 
#  filter(!is.na(allele_base)) %>% droplevels() %>% 
    ggplot() + geom_boxplot(aes(x=allele_base, 
                                y=SGR.ww.trt,  # <---- CHANGE THIS TO MATCH THE GWAS TRAIT 
                                fill=temperature), alpha=0.7, outlier.shape = NA) + 
    geom_point(aes(x=allele_base, 
                   y=SGR.ww.trt,  # <---- CHANGE THIS TO MATCH THE GWAS TRAIT 
                   color=point), position=position_jitter(w = 0.2,h = 0)) + 
  theme_minimal() + facet_wrap(~temperature, scales="free_y", nrow = 2, ncol=2) +
  scale_fill_manual(name="temperature", 
                   values=c("0"="#2c7bb6", "5"="#abd9e9", "9"="#fdae61", "16"="#d7191c"),
                   labels=c("0"="0degC","5"="5degC","9"="9degC (optimal)","16"="16degC")) +
  scale_color_manual(name=NULL, values=c("No.eGOA"="red", "No.wGOA"="black", "Yes.wGOA"="green2"), 
                     labels=c("No.wGOA"="Survived",  "No.eGOA"="eGOA cluster in PCA", "Yes.wGOA"="Died")) + 
  ggtitle(paste0("Marker: ", meta$marker, "\n Gene ID: ", meta$gene_id, "\n Protein: ", meta$protein_names)))
}
```

## Preliminary/example results from temp-specific GWAS (0C shown here)

![image](https://github.com/user-attachments/assets/0b55f556-c52f-4be1-ae62-b912ef55e0eb)

**Table:** Standard length growth-rate associated markers (log10(p)>3) within/upstream of annotated genes 
| Chromosome  | Position | LRT      | pvalue       | spid   | protein_names                                                     |    |
|-------------|----------|----------|--------------|--------|-------------------------------------------------------------------|----|
| NC_082385.1 | 33281341 | 17.99196 | 2.218401e-05 | Q7RTR2 | NLR family CARD domain-containing protein 3 (CARD15-like protein) |    |
| NC_082388.1 | 13102026 | 11.59819 | 6.601597e-04 | NA     | NA                                                                | NA |

![image](https://github.com/user-attachments/assets/84df01ad-f3a9-4f31-91f2-c3097d8d7c16)
![image](https://github.com/user-attachments/assets/b44b9209-d3cf-4edb-8805-495bc41766f9)

**Table:** Wet weight growth-rate associated markers (log10(p)>3) within/upstream of annotated genes 
| Chromosome  | Position | LRT  | pvalue   | spid   | protein_names                                                     |
|-------------|----------|------|----------|--------|-------------------------------------------------------------------|
| NC_082385.1 | 30466239 | 14.5 | 0.000144 | Q7RTR2 | NLR family CARD domain-containing protein 3 (CARD15-like protein) |
| NC_082402.1 | 19376042 | 12.9 | 0.000322 | Q7RTR2 | NLR family CARD domain-containing protein 3 (CARD15-like protein) |
| NC_082385.1 | 30466211 | 12.6 | 0.000386 | Q7RTR2 | NLR family CARD domain-containing protein 3 (CARD15-like protein) |
| NC_082391.1 | 7766734  | 12.5 | 0.000414 | NA     | NA                                                                |
| NC_082403.1 | 2801483  | 12.0 | 0.000521 | O73895 | Tapasin (TPN) (TPSN) (TAP-associated protein)                     |

![image](https://github.com/user-attachments/assets/0d89edb5-cb0d-4ee9-8d6b-35b1b4be224e)
![image](https://github.com/user-attachments/assets/2483af16-0e5c-44ee-88f3-c04a24dc53fc)
![image](https://github.com/user-attachments/assets/188d1cc5-106d-432f-ba8e-2e1f5c78bd2a)
![image](https://github.com/user-attachments/assets/a114b5f2-f450-48f4-a484-b95b5dfc4b4e)


**Table:** Hepatosomatic index associated markers (log10(p)>3) within/upstream of annotated genes 
| Chromosome  | Position | LRT  | pvalue   | spid   | protein_names                                                                         |
|-------------|----------|------|----------|--------|---------------------------------------------------------------------------------------|
| NC_082385.1 | 33615649 | 17.7 | 2.55e-05 | Q62158 | Zinc finger protein RFP                                                               |
| NC_082388.1 | 3450129  | 13.7 | 2.10e-04 | NA     | NA                                                                                    |
| NC_082382.1 | 24876070 | 13.7 | 2.13e-04 | Q80WM9 | Tumor necrosis factor receptor superfamily member 14 (Herpes virus entry mediator A)  |
| NC_082393.1 | 1357919  | 11.2 | 7.97e-04 | Q00341 | Vigilin (High density lipoprotein-binding protein)                                    |

![image](https://github.com/user-attachments/assets/19935310-75d9-442f-a28f-b18d8b0144f0)
![image](https://github.com/user-attachments/assets/d2eba36f-6b69-448b-a6ab-b5c2ced19d09)
![image](https://github.com/user-attachments/assets/a03beb1e-59e8-4371-b505-b7e61817c0c1)

## Resolving GWAS issue with 16C 
For some _unknown_ reason the GWAS ran successfully on raw genotype likelihoods for 0C, 5C, and 9C, but NOT for 16C. I kept encountering a "segmentation fault" error, which I've previously encountered with angsd v0.940 (but it's weird that it's only happening with one set of samples). After TOO MUCH TIME troubleshooting, I've decided to pivot to another approach. Here, I try to use angsd v0.933 instead. That version kept throwing an error related to improperly handing chromosome name; it couldn't get past the "_" in each chromosome id_site (NC_XXXXX.1_#####) in the beagle files, so I never attempted to use it. But now I might have to! So, I'll modify the input beagles and genome reference to remove that first "_".

#### Modify pcod genome file to remove "_" in chromosome ID 
```
sed '/^>/ s/NC_/NC/' /home/lspencer/references/pcod-ncbi/GCF_031168955.1_ASM3116895v1_genomic_rehead.fa > pcod-genome.fa  #use sed to replace "NC_" with "NC" in header lines

(base) [lspencer@node28 gwas]$ head pcod-genome.fa #view results
>NC082382.1
CAGACCTCCAATAGAGAGCTGCTCCCCCTTCGCACAAAGCCGCTGCTGGTGAATGCTCGAAGCGTTGTGTGATTGAATCG
CTTTAATGCCGTTCCATGTCACGTTGATCGTTTTTTTGCACAACGAGCAAAAAGCTTCCCGGTCATTTCCCATCACGGGT
TTCAGCCAGTCTTTAAATGTCGGGTCGTCAACCCATGTTTGCGAaaacttgcatttacccattcTCTCATAGAGCCAGAA
ACTCAGCCGTACAGAACTCTGAGGGGAAAAGGCGGAAAATGTTGctggtgttacaccgaattctgtgacccaccgaaaag
tgtggcCCCAGGGGGTGcagttgcacattttctgcaacatgcacgtgtgcattataacaaaaaacataaataaaacataa
gatctctggtgggtctttctttttttgatactaacattaattctaaacactattttacacagtagcctatactaggaaat
gctcaaaggtaaatcagcagAATTTAACcatgactgtagctttttatgggtaaagaagcaacggtgaaggacccTACTAA
ACGCccgtctgtaaaatgctaatcaaatcaatcaaatgtatttattaagcacctttaataaaaaggtaattggttggggg
gagatgttatgttttatgtatgtttttgtcatgcctgtctacgcttcaaactcgtgcatattgcagtaaatatgcaacag

(base) [lspencer@node28 gwas]$ samtools faidx pcod-genome.fa #index new genome file 

(base) [lspencer@node28 gwas]$ cat pcod-genome.fa.fai #view index file 
NC082382.1      26289739        12      80      81
NC082383.1      23805147        26618385        80      81
NC082384.1      26984552        50721109        80      81
NC082385.1      35077496        78042980        80      81
NC082386.1      22175970        113558957       80      81
NC082387.1      27900680        136012139       80      81
NC082388.1      28186669        164261590       80      81
NC082389.1      23760065        192800605       80      81
NC082390.1      23393856        216857683       80      81
NC082391.1      23728004        240543975       80      81
NC082392.1      26267057        264568592       80      81
NC082393.1      27587113        291164000       80      81
NC082394.1      22312454        319095964       80      81
NC082395.1      26048790        341687336       80      81
NC082396.1      24646214        368061748       80      81
NC082397.1      29952890        393016052       80      81
NC082398.1      16295523        423343366       80      81
NC082399.1      21077965        439842596       80      81
NC082400.1      17993618        461184048       80      81
NC082401.1      23303860        479402599       80      81
NC082402.1      19478734        502997770       80      81
NC082403.1      19411328        522720001       80      81
NC082404.1      20003359        542373983       80      81
NC036931.1      16569   562627396       80      81
```

#### Modify beagle file to remove "_" in chromosome ID 
```
zcat wholegenome-16.beagle.gz | sed 's/NC_/NC/g' | gzip > wholegenome-16-rehead.beagle.gz

zcat wholegenome-16-rehead.beagle.gz | head | cut -f1-9
marker  allele1 allele2 Ind0    Ind0    Ind0    Ind1    Ind1    Ind1
NC082382.1_1851 1       3       0.000044        0.333333        0.666622        0.969687        0.030313        0.000000
NC082382.1_2119 2       0       0.000266        0.999468        0.000266        0.799979        0.200021        0.000000
NC082382.1_3516 1       3       0.000266        0.999468        0.000266        0.941162        0.058838        0.000000
NC082382.1_6996 0       2       0.666622        0.333333        0.000044        0.941162        0.058838        0.000000
NC082382.1_8587 2       0       0.004238        0.995762        0.000000        0.969687        0.030313        0.000000
NC082382.1_9999 0       2       0.799979        0.200021        0.000000        0.001063        0.998937        0.000000
NC082382.1_10949        1       0       0.799979        0.200021        0.000000        0.799979        0.200021        0.000000
NC082382.1_11009        3       1       0.888865        0.111135        0.000000        0.000266        0.999468        0.000266
NC082382.1_26931        3       0       0.666622        0.333333        0.000044        0.969687        0.030313        0.000000
```
#### Run gwas.sh using angsd v0.933 and edited genome index file 
Here's example code, I don't included all angsd -doAsso runs due to space

```
module load bio/angsd/0.933 #important to use this version

base=/home/lspencer/pcod-lcwgs-2023/analysis-20240606/experimental/gwas
beagle=${base}/temp-16/wholegenome-16-rehead.beagle.gz
fai=${base}/cod-genome.fa.fai

# FIRST PERFORM IMPUTATION (to fill in NA values) with beagle

# All experimental fish
java -Xmx15000m -jar /home/lspencer/programs/beagle.jar \
like=${beagle} \
out=${base}/temp-16/imputed

imputed=${base}/temp-16/imputed.wholegenome-16-rehead.beagle.gz.gprobs.gz

# ---------------------
#  CPI from all 4 growth and condition metrics (SGR-sl, SGR-ww, HSI, Kwet)
angsd \
-doMaf 4 \
-beagle ${beagle} \
-yQuant ${base}/temp-16/16-pi.grow.cond1.txt \
-doAsso 4 \
-out ${base}/temp-16/16-gwas-pi-grow-cond1.out \
-fai ${fai}
```

## Working on pcod genotype imputation pipeline 
I previously performed genotype imputation without any reference panel/map. I did some reading, and it looks like I could greatly improve imputation accuracy if I provide phased haplotype reference panel, built from other Pacific cod WGS data. I happen to have a lcWGS data from  ~600 reference fish (depth ~3x) AND the big/little fish from 2021 (depth ~14x). I'm going to see if I can use those datasets to build the phased reference panel. 

_NOTE: phased reference panel: genetic variants are assigned to their respective chromosomes, distinguishing which alleles came from one parent versus the other._

NOTE: I tried to use a beagle file (merged to have both ref & big/little data) as a reference panel, but no dice. I need a VCF file, so I'll build that from "scratch" using angsd's doVCF option 

### Run ANGSD to generate VCF with genotype probabilities from reference population and Big/Little 2021 cod 

I'm working in this repon on Sedna:  /home/lspencer/pcod-general/imputation-ref-panel, and ran the below code in the script **angsd-impute-ref-panel.sh.** It produced BCF files for each chromosome. 

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
(base) [lspencer@node28 imputation-ref-panel]$ pwd
/home/lspencer/pcod-general/imputation-ref-panel
```
I then concatenated the BCF files into one using `bcftools concat`, then converted to VCF, filtered for minor allele frequency > 5% and max missingness 15%. This produced the file **whole-genome_miss15.vcf.gz**.  There are still some missing genotypes, so here I try to impute those using BEAGLE v5.4:  

```
java -jar /home/lspencer/programs/beagle.v5.4.jar gt=/home/lspencer/pcod-general/imputation-ref-panel/whole-genome_miss15.vcf.gz gp=true impute=true out=/home/lspencer/pcod-general/imputed

beagle.29Oct24.c8e.jar (version 5.4)
Copyright (C) 2014-2022 Brian L. Browning
Enter "java -jar beagle.29Oct24.c8e.jar" to list command line argument
Start time: 11:17 AM PST on 25 Nov 2024

Command line: java -Xmx1136m -jar beagle.29Oct24.c8e.jar
  gt=/home/lspencer/pcod-general/imputation-ref-panel/whole-genome_miss15.vcf.gz
  gp=true
  impute=true
  out=/home/lspencer/pcod-general/imputed
  nthreads=1

No genetic map is specified: using 1 cM = 1 Mb

Reference samples:                    0
Study     samples:                  708

Window 1 [NC_036931.1:47-16349]
Study     markers:                   84

Burnin  iteration 1:           5 seconds
Burnin  iteration 2:           1 second
Burnin  iteration 3:           1 second

Estimated ne:                  5216323
Estimated err:                 6.4e-03

Phasing iteration 1:           0 seconds
Phasing iteration 2:           0 seconds
Phasing iteration 3:           0 seconds
Phasing iteration 4:           0 seconds
Phasing iteration 5:           0 seconds
Phasing iteration 6:           0 seconds
Phasing iteration 7:           0 seconds
Phasing iteration 8:           0 seconds
Phasing iteration 9:           0 seconds
Phasing iteration 10:          0 seconds
Phasing iteration 11:          0 seconds
Phasing iteration 12:          0 seconds

Window 2 [NC_082382.1:8368-26260017]
Study     markers:               17,298

...
```

yada yada yada 


This should produce a VCF file with phased haplotypes, and a log file. I should next assess the phasing quality using tools like SHAPEIT's quality checks or Beagle's output metrics.
