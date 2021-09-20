---
title: "Adding Annotations to VCFs"
date: 2021-09-21T11:29:24+13:00
author: "Murray Cadzow"
draft: false
weight: 0
enableToc: true
tags: ["R", "howto"]
tocLevels: ["h2", "h3", "h4"]
---

The main consideration is to make sure that the reference resource matches the genomic build of your VCF. i.e. make sure you are using GRCh37 resources for a GRCh37 aligned VCF, or GRCh38 resources for a GRCh38 aligned VCF. 

## dbSNP

A vcf from dbSNP can be used to annotate rsIDs into the ID column of a vcf. For instance the dbSNP v151 VCF can be downloaded here ftp://ftp.ncbi.nih.gov/snp/organisms/human_9606_b151_GRCh37p13/VCF/ - one key thing is make sure that you use the same genome build as your VCF that you want to annotate.

Once you have a copy (Merrimanlab can find it in `/Volumes/archive/merrimanlab/reference_files/VCF/dbSNP_reference/dbsnp151_GRCh37/dbsnp_151_20180423.vcf.gz`)

Then to annotate using bcftools

```
# use on the server
module load bcftools/bcftools-1.11

bcftools annotate -a dbsnp_151_20180423.vcf.gz -c ID -o myvcf.dbsnp_annotated.vcf.gz -O z myvcf.vcf.gz
```

## dbNSFP

### Creation of latest dbNSFP 

https://sites.google.com/site/jpopgen/dbNSFP
```
wget ftp://dbnsfp:dbnsfp@dbnsfp.softgenetics.com/dbNSFP4.0a.zip
unzip dbNSFP4.0a.zip
```

The file has GRCh38 coordinates as the default, so to change them you need this perl script [dbNSFP_sort.pl](https://raw.githubusercontent.com/pcingola/SnpEff/master/scripts_build/dbNSFP_sort.pl) and the GRCh37 coordinates are columns 8 and 9, but the script is zero indexed hence using 7 and 8 as the column numbers.
```
# Set to your downloaded dbNSFP version
version="4.0a"

# Replace coordinates by columns 7 and 8 (hg19 coordinates) and sort by those coordinates
zcat dbNSFP${version}_variant.chr*.gz | dbNSFP_sort.pl 7 8 > dbNSFP4.0a_hg19.txt

# Compress and index
bgzip dbNSFP${version}_hg19.txt
tabix -s 1 -b 2 -e 2 dbNSFP${version}_hg19.txt.gz
```

Once this annotation file is created it can be used to annotate VCF files using `SnpSift`.

For Merrimanlab, a version is available at `/Volumes/archive/merrimanlab/reference_files/dbNSFP/dbNSFP4.0a/dbNSFP4.0a_hg19.txt.gz`

### Annotate 

Make sure to check out the readme that came with dbNSFP for an idea of the annotations that are available (`/Volumes/archive/merrimanlab/reference_files/dbNSFP/dbNSFP4.0a/dbNSFP4.0a.readme.txt`).


To perform the annotation, we use `SnpSift` which comes as part of `snpEff`.


```
module load snpEff/snpEff_4.3t

java -jar $SNPSIFT dbnsfp -db dbNSFP4.0a_hg19.txt.gz -f $(zcat /Volumes/archive/merrimanlab/reference_files/dbNSFP/dbNSFP4.0a/dbNSFP4.0a_hg19.txt.gz | head -1 | tr "\t" "\n" | grep -v "hg18\|hg19\|Geuvadis_eQTL_target_gene" | sed -n 5,"$"p | tr "\n" "\," |sed "s/,$//g") myvcf.vcf.gz | bgzip -c > myvcf.dbnsfp.vcf.gz
```

## Effect predictors

### snpEff

```
module load snpEff/snpEff_4.3t
java -jar $SNPEFF GRCh37.75 -stats myvcf_stats.html -lof -csvStats myvcf_stats.csv myvcf.vcf.gz > myvcf.ann.vcf
```