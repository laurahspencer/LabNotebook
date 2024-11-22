---
layout: post
title: GWAS Day 5 - trouble shooting etc. 
--- 

I met with Mary Beth to discuss how we could potentially identify sequences for new primers that could identify Pacific cod sex. The reference Pacific cod genome may have been generated from a female, and thus lacks the male-specific "Y" region of chromosome 11 (presumably). 

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
Perhaps I need to have original genotype likelihood files (e.g. beagle, bcf/vcf) directly output from ANGSD to run GWAS, rather than using my custom script to subset a beagle files for samples of interest.  Let's develop the pipeline with the fish exposed to the WARM temperatuer (16C). 

Step 0. Generate reference panel/map of genotypes 
- I ran ANGSD using all our reference fish (from a variety of marine regions, low coverage, 3x) PLUS the Big/Little 2021 fish (moderate coverage, 14x)  

Step 1. Re-run ANGSD multiple times, once for each group (temp, lipid samples) 
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


