# shenlab-breastcancer
CNV analysis on Regeneron samples

We used XHMM and CANOES to call CNVs on 100 trios from the PCGC study for QC of our variant calling methods. We then focused on XHMM to triage the reproducibility and quality of the data. For all analyses the interval file RGN_PCGC.interval_list was used, which is an intersection of intervals represented in the PCGC and RGN exom
e capture kits generated by Qiang.

All bams for the PCGC study can be found here: 
/home/local/ARCS/hq2130/WES/CHD_NimbleGenV2/bam/

The list of 300 trio bams can be found here: 
/home/local/users/sai2116/PCGC_QC/100_trios.list 

The results of the QC analysis can be found in that same directory. 

We then used XHMM to call CNVs in 1331 samples sequenced by Regeneron.
The Regeneron bam files are split among the following directories:

/mnt/BigData/WENDY/BreastCancer/WES_data/Regeneron/bams/

/home/local/users/sai2116/RGN_BigData_bams (for bams in the above path that needed RG fixing*)

/mnt/BigData/WENDY/RGN_rare_disease/bams/

/home/local/users/sai2116/RGN_WES_rare_bams (for bams in the above path that needed RG fixing*)

A total of 1331 bam files were used as input to generate Depth of Coverage files. The list of all subject IDs used can be found here (note that 7 bams couldn't be located in the above directories and were therefore not included in the CNV calling):

/home/local/users/sai2116/subject_ID_master.txt

There are two samples with duplicate bams (220897 and 220904, 222357 and 222966), so only one of each of these was included in the DoC calling. There are only 55 trios in the Regeneron data according to the clinical information in the Dropbox folder from Wendy Chung's lab, and I saved the trio pedigree information here: 

/home/local/users/sai2116/cnv_calling/Rgn.trios.ped 

*Please refer to Qiang's github for a description of the RG error present in some bams, which was due to a plate swap that happened at Regeneron during sequencing. The RG in the header was fixed but the RG flag for each read still retained the wrong sample ID. To fix the header I used this script: /home/local/users/sai2116/Run_FixRGs.modified.sh

-----------------------------------

To call Depth Of Coverage using GATK I used this script which is modeled after Ashley's scripts:

/home/local/users/sai2116/PCGC_QC/Run_GATK_DOC.sh

Type 'bash Run_GATK_DOC.sh -H' for explanation of the variables used. You need GNU Parallel installed.
The output of this script for the 100 PCGC trios is stored in docfiles in the same directory.

CNVs were then called using XHMM using the Depth of Coverage sample_interval_summary output:

/home/local/users/sai2116/cnv_calling/Run_XHMM.sh

You need an extreme GC target file and a low complexity region file to go with this script if you want to filter out these two sources of error. Please refer to the XHMM documentation for detailed explanation. The following two files were obtained from the hg19.fasta reference genome:

/home/local/users/sai2116/cnv_calling/low_complexity_targets.txt
/home/local/users/sai2116/cnv_calling/extreme_gc_targets.txt

The output of the XHMM CNV calling algorithm is stored in PCGC.XHMM.cnv and other files associated with the analysis are labeled with the "PCGC.XHMM" identifier. There were 5340 CNVs called before any filtering.

-----------------------------------

To call CNVs in the Regeneron samples I used the same scripts to generate DoC files and call CNVs using XHMM. 
The results are stored in this directory: 

/home/local/users/sai2116/cnv_calling/

All 1331 Regeneron DoC files can be found here:

/home/local/users/sai2116/cnv_calling/RGN_1331.DOC.list

The results from the XHMMM CNV calling algorithm are stored in that same directory and have the "RGN_1331" identifier. There were 70294 CNVs called before any filtering.
