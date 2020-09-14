---
title: Genome Studio QC
author: Tanya Major
date: '2018-08-21'
slug: genome-studio-qc
categories: ["QC"]
tags: ["GenomeStudio"]
weight: 0
enableToc: yes
tocLevels:
  - h2
  - h3
  - h4
---

This guide provides a comprehensive set of steps for the manual curation of genotype calls within Genome Studio. Genome Studio's automatic clustering algorithms are reported to be accurate for ~ 99 % of SNPs. The other ~ 1 % need to be manually reviewed. This guide offers a consistent and logical approach and is aimed at reducing the workload where possible. The steps here are a combination of those published by Guo _et al._ (2014) and Illumina (2016).

In our experience, the steps outlined by Illumina (2016) provide a more logical and complete framework for the curation of autosomal SNPs, than those published by Guo _et al._ (2014). However, Guo _et al._ (2014) include more detailed instructions for the curation of haploid SNPs and also include post-quality control steps which are invaluable.

This guide is laid out in the following order:  

  1. Load data into Genome Studio  
  2. Clustering within Genome Studio  
  3. QC of Haploid SNPs  
  4. QC of Autosomal SNPs  
  5. Final Quality Thresholds  
  6. Exporting data & Post-QC steps  

## Support Files

- [human coreExome 24 v1.0 and v1.1](http://support.illumina.com/array/array_kits/humancoreexome-24-beadchip-kit/downloads.html) accessed 18/4/2016

## References

Guo, Yan, Jing He, Shilin Zhao, Hui Wu, Xue Zhong, Quanhu Sheng, David C. Samuels, Yu Shyr, and Jirong Long. "Illumina human exome genotyping array clustering and quality control." nature protocols 9, no. 11 (2014): 2643-2662.

Illumina (2016). Infinium Genotyping Data Analysis. [Technical Data Sheet]. Retrieved 18 January, 2016 from http://www.illumina.com/Documents/products/technotes/technote_infinium_genotyping_data_analysis.pdf


***

##  1. Load data into Genome Studio  

Follow **Step 1** of Guo _et al._ (2014). 

##  2. Clustering within Genome Studio  

Follow **Steps 2 to 6** of Guo _et al._ (2014).
   Briefly:

    2) Auto clustering
    3) Filter samples with a call-rate <94.5%
    4) Exclude these samples - update SNP statistics
    5) Unselect "show excluded samples" in the cluster graph
    6) Exclude all samples with call-rate <98.5% and re cluster - don't update statistics

_Important_: Step 6 is essential. If you skip Step 6 you will end up with thousands of SNPs (which require manual fixing) with false AB clusters such as shown below. 

![False AB clusters](https://github.com/nickb-/qcSNP/blob/master/ClusterAnalysis/ExamplePlots/Monomorphic_MoveAB_Example.jpg)

Our own experience suggests that these mis-called AB clusters are almost exclusively due to troublesome samples with call rate < 0.99. Excluding these and reclustering places the AB cluster back in the middle of the plot and avoids this problem.
  

***

##  3. QC of Haploid SNPs  

To speed up the QC process we deviate slightly from Guo _et al._ (2014) for the QC of the Haploid chromosomes.  

**Step 3a)**: In 'Samples Table', sort samples Gender and select all the Male samples. These should now be highlighted in yellow in the SNP Graph. All Female samples should be coloured according to their genotype.  
  

**Step 3b)**: Follow steps 12 - 17 of Guo _et al._ (2014).  
NOTE: you will need to edit the PAR_SNPs.txt file. Open this file and add the following column headers: Name, PAR.  
  

**Step 3c)**: QC of X Chromosome

    a. Follow step 18 Guo _et al._ (2014). [Filter: CHR = X AND PAR != 1]
         - If you have the Male samples selected, the bright yellow points stand out really clearly.  
         - You can then check all of the X Chromosome SNPs by ordering by ABFreq, starting at the top and rapidly scanning through the SNPs looking for yellow areas in the center of the plot.   
         - We recommend reviewing all of the X Chromosome SNPs with an ABFreq > 0, though it may be possible to stop prior to this ~ ABFreq < 0.2. Scanning through the remainder however will only take a couple of minutes, so is worthwhile.

    b. Change the SNP filter of CHR = X and PAR = 1 to check all the pseudo autosomal SNPs labelled as the X chromosome. These should look similar to a normal autosomal SNP, check the clusters are well defined, if not zero the selected SNP.
  

**Step 3d)**: QC of Y Chromosome  
If you follow Guo _et al._ (2014), this step will take you a couple of hours. To speed this up, we suggest the following:  

    a. In the 'SNP Table' change the filter to  [CHR = Y  AND PAR != 1] OR [CHR = 0 AND Name "has" Y]

    b. In the 'Sample Table' select all of the Female samples. Right click and exclude these samples. 
       **NOTE:** that you do not need to update the Sample or heritability statistics if prompted. 
       Doing so would take quite a while, and as we are adding the females back in later, is not necessary.  
 
    c. In the 'SNP Table' select all SNPs (these should only be the Y chromosome SNPs)  

    d. Right click the selected SNPs and "Recluster Selected SNPs". 
       This may take a few minutes, but it will hopefully remove a large portion of poorly clustered SNPs
       When prompted, update the SNP statistics.  

    e. In the 'Samples Table' select all Female samples with a call rate >98.5% (the ones we excluded in "b") and "Include" these again. 
       Once again, there is no need to update the sample and heritability statistics if prompted.  

    f. Follow Step 19 Guo _et al._ (2014). (NB/ Guo _et al._ have a typo, sort by call freq not call rate)

    f. Add a new column to the SNP Table that estimates the Male Call Freq (#calls/~#men) for Y Chromosome SNPs. Double check this value is > 95% for all Y Chromosome SNPs.

    g. Change the SNP filter to CHR = Y and PAR = 1 to check all the pseudo autosomal SNPs on the Y chromosome. These
       should look similar to a normal autosomal SNP, check the clusters are well defined, if not zero the selected SNP.  
  

**Step 3e)**: Follow Step 20 & 21 Guo _et al._ (2014).  (NB/ Guo _et al._ has a typo, mitochondria are haploid not diploid)
NOTE: though not stated, be sure to change the filter to [CHR = MT] OR [CHR = 0 AND Name "has" MT] OR [CHR = 0 AND Name "has" Mito]. 
  

This is the end of the Haploid QC steps. Autosomal QC will be based on the Illumina (2016) protocol. Ignore Steps 22 - 33 of Guo _et al._ (2014).   
  

***

##  4. QC of Autosomal SNPs  

**BaseFilter:** Filter the SNP Table to exclude the X, Y, and MT chromosomes. In addition, apply a filter on Edited = 0 (i.e. only shows SNPs we have not edited). [CHR != X AND CHR != Y AND CHR != MT] AND [CHR != 0 AND Name "lacks" Y AND Name "lacks" MT AND Name "lacks" Mito] AND [Edited = 0]

   NOTE: This filter has been saved to GenomeStudio as 'autosome base filter' and can be easily applied by loading it in the Filter window. It leaves SNPs on the XY chromosome as these are pseudo autosomal SNPs so should be treated similarly.

We will refer to this filter as the BaseFilter throughout the rest of this guide.  


**Step 4a)**: Cluster Separation Thresholds  

Here, the objective is to identify the LOWER limit on cluster separation (the point below which "the majority of loci are unsuccessful" (Illumina, 2016, n.p.). In addition, we will aim to identify an approximate UPPER limit above which all loci are successful. Between these two limits are the SNPs we will have to review.  

steps referred to in Illumina 2016 are referencing section "Evaluating and Editing SNP Cluster Positions"
  
Part One:  
   
  - Sort by ClusterSep (descending)  
  - Rapidly scan through SNPs to identify the rough UPPER limit for ClusterSeparation. Once identified, you may adjust the filter if you like.  
  - Rapidly scan through the SNPs to identify the rough LOWER limit for ClusterSeparation. Zero all SNPs below this threshold.  

Part Two:  

  - (optional) add LOWER < ClusterSep < UPPER to the BaseFilter  
  - Follow **Step 2**, Illumina (2016)  
  

NOTE: For unambiguous SNPs you may be able to improve the CallFreq by adjusting the cluster positions. We found the following appropriate:  
  

    - CallFreq < 0.93 were reliably poor. 
      These were zeroed unless there was an obvious biological reason the low CallFreq 
      (for example structural polymorphisms).  

    - CallFreq > 0.93 were able to be reliably edited. 
      IMPORTANT: at the end we zero all SNPs with a CallFreq < 0.95 & Edited = 0. 
      If you move a cluster at all, the Edited flag changes from 0 to 1. 
      Therefore, **if you try unsuccessfully to edit a SNP, you must manually ZERO it**.  
  

This is the most subjective step. It is tempting to keep SNPs with CallFreq ~ 0.94 after you have manually edited. However, we recommend that you try to be as impartial as possible here. if you edit a SNP and cannot get the Call Freq > 0.95, then zero it and move on.  


**Step 4b)**: Reset filter to BaseFilter then follow Step 3, Illumina (2016).  
In addition to Illumina (2016), Table 1 suggests a hard limit: AB R Mean < 0.2 should be zeroed. Regardless of cluster definition, AB R Mean < 0.2 suggests problems with the detection of these SNPs. 
    Edit: Filter out SNPs with AB Freq = 0 before performing this step.


**Step 4c)**: Reset filter to BaseFilter then follow Step 4, Illumina (2016).  

**Step 4d)**: Reset filter to BaseFilter then follow Steps 5 & 6, Illumina (2016) if you have family/relatedness information for your data. If you don't have this info these QC steps can be performed outside Genome Studio.

**Step 4e)**: Reset filter to BaseFilter then follow Step 7, Illumina (2016)

**Step 4f)**: Detect False Homozygotes (replaces Step 8, Illumina (2016))  

    - To the BaseFilter add: ABFreq = 0  

    - Sort SNPs by MinorFreq (ascending)  

    - Review SNPs with MinorFreq < 0.05 - 0.1. 
      "A SNP can be edited unless clusters overlap or cannot be separated unambiguously" (Illumina, 2016, n.p.).    
  
**Step 4g)**: Hard Filters on Monomorphic SNPs.  

Table 1 (Illumina (2016)) suggests the following hard filters on Theta values. These thresholds are indicative of detection errors and therefore, of SNPs which cannot be trusted.  

    - Reset filter to the BaseFilter then: remove edited = 0, add AAFreq = 1 & AA T Mean > 0.3. Zero these SNPs.  
    - Reset filter to the BaseFilter then: remove edited = 0, add AAFreq = 1 & AA T Dev > 0.06. Zero these SNPs.  
    - Reset filter to the BaseFilter then: remove edited = 0, add BBFreq = 1 & BB T Mean < 0.7. Zero these SNPs.  
    - Reset filter to the BaseFilter then: remove edited = 0, add BBFreq = 1 & BB T Dev > 0.06. Zero these SNPs.  

***

##  5. Final Quality Thresholds  

In Step 2 we removed all samples with a Call Rate < 0.99. We will now add all of the samples back in and recalculate the sample / SNP statistics.  

**Step 5a)**: Remove all SNP filters. In the 'Samples Table' select all samples and include these. When prompted, recalculate the sample statistics and sample heritability (this may take a little while depending on the number of samples).  

**Step 5b)**: In the 'Sample Table' sort by Call Rate (ascending). Exclude all samples with a Call Rate < 0.98.  

**Step 5c)**: Final check of SNP CallFreq (make sure no SNP filters are on)

    - Filter all SNPs with CallFreq < 0.95.  
    - Review these SNPs, manually editing where possible.  

**Step 5d)**: Hard limit on CallFreq  

    - Filter: CallFreq < 0.95 & Chromosome != Y.  
    - Zero these SNPs.

    - Estimate what a CallFreq < 0.95 for men only would look like in the full group (eg. 2734 men in a group of 4053 people would give a CallFreq of 0.64 in the whole group for a Y chromosome SNP with a CallFreq of 0.95 in men only [2734 * 0.95 /4053])
    - Filter: CallFreq < calculated number (eg 0.64) & Chromosome = Y.
    - Zero these SNPs.

**Step 5e)**: Save the project.  

***

##  6. Exporting data & Post-QC steps   

Follow Steps 34 onwards, Guo _et al._ (2014). 