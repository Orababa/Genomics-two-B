# Genomics-two-B: Genomics Lab Working on Targeted Sequencing Dataset
## M. tuberculosis Variant Analysis
### By:

![image](https://user-images.githubusercontent.com/70078984/130058063-fd718b9e-c64e-4611-b44f-93d7a22d132e.png)



# <img src = "https://user-images.githubusercontent.com/70078984/130062938-6e2101b6-73f2-49b4-9ee9-22dd919f8dae.png" alt = "contents" width = "40" height = "40" /> </a> Contents
- [Introduction](#introduction)
- [Get Your Data](#get-your-data)
- [Quality Control](#quality-control)
- [Look For Contamination with Kraken2 (Optional)](#look-for-contamination-with-kraken2-optional)
- [Find Variants with Snippy](#find-variants-with-snippy)
- [Further Variant Filtering and TB-Profiling](#further-variant-filtering-and-tb-profiling)
- [View Snippy Output in JBrowse](#view-snippy-output-in-jbrowse)
- [Different Samples, Different Stories (Optional)](#different-samples-different-stories-optional)

Reference : <small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Introduction

Mycobacterium tuberculosis is a Gram-positive rod-shaped bacterium that is known to cause tuberculosis. Tuberculosis (TB) is one of the world’s most dangerous infectious diseases and contributes to about two million annual deaths globally. Mutational changes in the genome of M. tuberculosis strains is one of the major contributing factors to its multidrug resistance nature and increased virulence. The genomics-two-B team recreated a galaxy tutorial on M. tuberculosis bacterium variant calling which aims at checking singe nucleotide polymorphism (SNP) in the DNA sequence of M. tuberculosis strains by comparing to an ancestral genome. This markdown contains the comprehensive step-wise procedure carried out during the recreation of the tutorial.

## Get Your Data

The data for the tutorial can be obtained using two methods;
1. By pasting the zenodo link to the data library on galaxy upload manager through the paste/fetch option.
     
     >> copy the zenodo link
     
     >> open the galaxy upload manager and select the fetch/paste option
     
     >> Click on the start option and close the upload window
     
2. By uploading the downloaded dataset from one's computer library
     
     >> Click on the galaxy upload manager
     
     >> Navigate to the choose from local file option
     
     >> Select the files to be uploaded and click on start
     
     >> Close the upload window

## Quality Control
This step serves to identify possible issues with the raw sequenced read input data before embarking on the “real” analysis steps
Some of the typical problem, with NGS data can be mitigated by preprocessing affected sequencing reads before trying to map them to the reference genome. Detecting possible severe problems early may at least save you a lot of tone spent on analysing low quality data that is not worth the effort.
Here is how we perform a standard quality check on our input data and only point out a few interesting aspect about that data.
### FastQC
Search and select [FastQC 🔧⚙️] from tools list and fill in the details below

>“Short read data from your current history”: select both FASTQ datasets
>Ensure the top part where “short read data from your current history” is selected.
>Leave all other parameters at their default values and click Execute.

The result should be four new datasets

One with the calculated raw data
and another with an HTML report of the findings for each input dataset.
This will get added to your history.
While one could examine the quality control report for each set of reads (forward and reverse) independently, it is quite useful to examine them side by side using the MultiQC TOOL

---
### [`MultiQC`](http://multiqc.info/) 

1. Search and select **`MultiQC- 🛠️`** from the tools list and fill in the details as below

     >Results

      >`Which tool was used generate logs?` **_FastQC_**
      >> FastQC output
      
      >> `Type of FastQC output?` **_Raw data_**
      
      >>>`Output of BAMtools` **_FastQC on data: RawData_** (Select both RawData files)
2. Keep the rest parameters unchanged
3. Then **`Execute☑️`**
4. Observe the output MultiQC webpage file by clicking on 👁️ button on the file



### [**_quality trimming_**] 
1. Search and select Trimmomatic from the search tools. The process is as shown below:

“Single-end or paired-end reads?”: Paired End (two separate input files)
param-files “Input FASTQ file (R1/first of pair)”: 004-2_1.fastq.gz
param-files “Input FASTQ file (R2/second of pair)”: 004-2_2.fastq.gz
Select Trimmomatic operation to perform
Keep the default value of Sliding window trimming and adjust the average quality required to 30
“+Insert Trimmomatic Operation”
“Select Trimmomatic operation to perform”: Drop reads below a specified length (MINLEN)
“Minimum length of reads to be kept”: 20
Click on Execute
Inspect the output produced by Trimmomatic






   

-----

## Look For Contamination with Kraken2 (Optional)

Looking for contamination in our reads is essential to producing a good work, as other sources of DNA accidentally or inadvertantly get mixed in with our sample and this can  confound our snp analysis. Kraken 2 is an effective way of looking at which species is represented in our reads and so we can easily spot possible contamination of our sample. 
The already trimmed sequences were used in the kraken analysis highlighting both the 
-Forward Stand and the Reverse strand
![](https://res.cloudinary.com/adedoyinsoye/image/upload/v1629442271/HACKBIO/Screenshot_19_t6vvut.png)
A report of the analysis was produced later on; 
![](https://res.cloudinary.com/adedoyinsoye/image/upload/v1629442272/HACKBIO/Screenshot_20_aly0hu.png)
From the report generated; it was infered that about 91% of the reads were positively identified as Mycobacterium. The others found were bacteria from the same kingdom. There were no contaminating human or viral sequences detected.


## Find Variants with Snippy
###Snippy(Finding variant using Snippy)
1. Mycobacterium_tuberculosis_ancestral_reference.gbk dataset was used as the reference sequence”: 

2.Paired was selected for the end reads

3.first set of reads wa selected as Trimmomatic on X (R1 paired)

4.Second set of reads was selected as Trimmomatic on X (R2 paired)
![](https://res.cloudinary.com/adedotun/image/upload/v1629387169/samples/hackbio%20task/snippy_1_uynhgv.png)

5.In the “Advanced parameters”, “Minimum proportion for variant evidence” was adjusted to 0.1, so we can see possible rare variants in the sample
![](https://res.cloudinary.com/adedotun/image/upload/v1629387173/samples/hackbio%20task/snippy_2_cjs4h1.png)

6.Under “Output selection”, “The final annotated variants in VCF format”, “A simple tab-separated summary of all the variants” and “The alignments in BAM format” were selected

7. It was executed to show one of the following results
In a total of 1086 variants the first variant on the list is a Substitution of a C to a T which is supported by 134 reads.
![](https://res.cloudinary.com/adedotun/image/upload/v1629387184/samples/hackbio%20task/snippy_C-T_l0xyp7.png)



## Further Variant Filtering and TB-Profiling

### [`TB Variant Filter`]

1. Search and select **`TB Variant Filter- 🛠️`** from the tools list and adjust filters as below.


       >>'Select Filter variants by region, Filter variants close to indels and Filter sites by read alignment depth.

2. Choose the VCF files you wish to be filtered.
3. **`Execute☑️`**
4. Open the new VCF file.

  
### ['Text transformation with sed'] 
1. “File to process”: TB Variant Filter on data 21
2. “SED Program”: s/GENE_//g 
>> https://usegalaxy.eu/datasets/11ac94870d0bb33a2ea3b318a7a7d7be/display?to_ext=vcf 

### ['TB variant report']
1. “Input SnpEff annotated M.tuberculosis VCF(s)”: Text transformation on data 21
2. “TBProfiler Drug Resistance Report (Optional)”: TB-Profiler Profile on data 21: Results.json

### View Snippy Output in JBrowse 

### [`JBrowse`]

1. going through VCF files and to be more clear Search and select **JBrowse 🛠️`** from the tools list and adjust filters as below.

     >>“Reference genome to display”: Use a genome from history
     
     >>“Select the reference genome”: (it would be selected directly from your History)
     
     >>Produce Standalone Instance”: Yes
     
     >>"Genetic Code”: 11: The Bacterial, Archaeal and Plant Plastid Code
     
     >>“JBrowse-in-Galaxy Action”: New JBrowse Instance
      
2. Set up different 3 tracks:
     
     >>'Track 1 - sequence reads: Click on Insert Track Group and fill it with.
     
     >> 'Track Category' to sequence reads
     
     >> ' Click on Insert Annotation Track' and fill it with
     
     >> “Track Type” to BAM Pileups
     
     >> “BAM Track Data” to snippy on data XX, data XX, and data XX mapped reads (bam)
     
     >> “Autogenerate SNP Track” to Yes
     
     >> “Track Visibility” to On for new users
    
 
     >>Track 2 - variants: Click on Insert Track Group and fill it with
     
     >>“Track Category” to variants
     
     >>Click on Insert Annotation Track and fill it with, “Track Type” to VCF SNPs, -“SNP Track Data” to TB Variant Filter on data XX and “Track Visibility” to On for new users  
     
     >>Track 3 - annotated reference: Click on Insert Track Group and fill it with “Track Category” to annotated reference.
     >>
     >>B-Click on Insert Annotation Track and fill it with
     
     >>1-“Track Type” to GFF/GFF3/BED Features
     
     >>“GFF/GFF3/BED Track Data” to https://zenodo.org/record/3531703/files/Mycobacterium_tuberculosis_h37rv.ASM19595v2.45.chromosome.Chromosome.gff3
   
     >>“JBrowse Track Type [Advanced]” to Canvas Features
     
     >>Click on “JBrowse Styling Options [Advanced]”
     
     >>“JBrowse style.label” to product
     
     >>“JBrowse style.description” to product
     
     >>“Track Visibility” to On for new users
   
3. **`Execute☑️`**
4. A new dataset will be created in your history, containing the JBrowse interactive visualisation


 
## Different Samples,Different Stories (Optional)
### feach data and introduction
1.Fetch the data from EBI European Nucleotide Archive
>>ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR124/042/SRR12416842/SRR12416842_1.fastq.gz
>>ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR124/042/SRR12416842/SRR12416842_2.fastq.gz
###   check for data quality using fastQC
  { to find out the quality report} 
1. Select from options at the left Fasta/Fastq 
2. Choose FastQC to read quality report **FastQC**
3. Choose your data set 
>> insert multiple files, and choose the previously uploaded files
>> follow default parameters 
4. press **execute**
5. Four files will be created composed of two html page for the quality report 
6. Form the report we got that the data need to be trimmed to remove the bad-quality base pairs, and adaptor contamination 


## Different Samples, Different Stories (Optional)


### A. Hands-on: Take a closer look at sample 18-1













### B. Hands-on: Take a closer look at sample SRR12416842

Illumina HiSeq 2500 paired end sequencing; Whole genome sequencing analysis of multi-drug resistant Mycobacterium tuberculosis from Java, Indonesia SAMPLE

### Fetch data 
1.Fetch the data from EBI European Nucleotide Archive
>>ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR124/042/SRR12416842/SRR12416842_1.fastq.gz
>>ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR124/042/SRR12416842/SRR12416842_2.fastq.gz



### FastQC
To find out the quality report and to ensure that the raw data looks good and there are no problems in the dataset

1.In the tools list search for **FastQC** and select it

2.To select the input files

>>click multiple dataset icon
>>Choose the  two previously uploaded datasets
>>leave the other parameters at their default values

4.And just click execute

Four new files will be added to your History.
The result for each datasets will be a HTML report (the webpage file) and the rawData file. 


### Quality trimming
 (to clean up the reads and remove the poor quality sections)


1. search and select **'Trimmomatic'** from the tools list and fill in the details as below
 
   >>“Single-end or paired-end reads?”: **_Paired End (two separate input files)_**
   
   >>“Input FASTQ file (R1/first of pair)”:  SRR12416842_1.fastq.gz
   
   >>“Input FASTQ file (R2/second of pair)”:  SRR12416842_2.fastq.gz
   
   >>“Select Trimmomatic operation to perform“ : Keep the default value of Sliding window trimming and adjust the average quality required to 30
   
2. “+Insert Trimmomatic Operation”:
   >> “Select Trimmomatic operation to perform”: Drop reads below a specified length (MINLEN)
   >> “Minimum length of reads to be kept”: 20
  
3. **`Execute☑️`**
4. A new 4 dataset will be created in the history, containing the 4 trimommatic data (2 paired and 2 unpaired) : 
>> https://usegalaxy.eu/datasets/11ac94870d0bb33a2a0e9f4987bf4808/display?to_ext=fastqsanger.gz
>> https://usegalaxy.eu/datasets/11ac94870d0bb33a0c5fe35823cc6abc/display?to_ext=fastqsanger.gz
>> https://usegalaxy.eu/datasets/11ac94870d0bb33a5947887af43ba1e1/display?to_ext=fastqsanger.gz
>> https://usegalaxy.eu/datasets/11ac94870d0bb33ae46b0f83bf78d1b5/display?to_ext=fastqsanger.gz

5.  Observe the output trimommatic results  webpage file by clicking on 👁️ button on the file


### Mapping the samples to the **_M. tuberculosis_** reference genome 
 Mapping the samples to the **_M. tuberculosis_** reference genome using snippy tool to find variants
1. Sarch and select **'Snippy'** from the tools list and fill in the details as below


>>“Will you select a reference genome from your history or use a built-in index?”: Use a genome from history and build index

>>“Use the following dataset as the reference sequence”: Mycobacterium_tuberculosis_ancestral_reference.gbk

>>“Single or Paired-end reads”: Paired

>>“Select first set of reads”: Trimmomatic on SRR12416842_1.fastq.gz(R1 paired)

>>“Select second set of reads”: Trimmomatic on SRR12416842_2.fastq.gz (R2 paired)

2. Under “Advanced parameters”

>>“Minimum proportion for variant evidence”: 0.1 (This is so we can see possible rare variants in our sample)

3. Under “Output selection” select only the following:
>>“The final annotated variants in VCF format”

>>“A simple tab-separated summary of all the variants”

>>“The alignments in BAM format”.

Then
**`Execute☑️`**

![optional task 2-1](https://user-images.githubusercontent.com/88276401/130168756-3d462195-b7a4-41bb-a653-dea4852fec79.PNG)

![optional task 2-2](https://user-images.githubusercontent.com/88276401/130168775-c2f28073-6b1f-49f0-89a0-e14297992b58.PNG)

 A new 3 dataset will be created in the history, a vcf file, snps table and a mapped reads file in bam format

Inspect the Snippy VCF output to check the number of variant discovered by **'Snippy'** on 👁️ button on the file


### Samtools Stats


### [`Run samtools stat` to generate statistics for BAM dataset]

1. On the tools sections, Search and select `samtools stat 🛠️`
2. Fill with the following details;
   >BAM File
   >> Ensure the mapped reads (bam) file of the snippy output is selected.

3. Keep the rest parameters unchanged.

4. Then `Execute`☑️

5. View the output, paying attention to the sequences, reads mapped and reads unmapped results.







### BAM Coverage Plotter
1. search and select **'BAM Coverage Plotter'** from the tools list and fill in the details as below
>> "Will you select a reference genome from your history or use a built-in genome?" : use a genome from history in fasta formate 

>> "select the reference genome in fasta format" :(https://usegalaxy.eu/datasets/11ac94870d0bb33aca1bc094e2415e7e/display?to_ext=fasta)

>> " select the BAM file that you got from snippy. " :(https://usegalaxy.eu/datasets/11ac94870d0bb33a646435297267fb5a/display?to_ext=bam)

2. **`Execute☑️`*

3. Observe the output trimommatic results  webpage file by clicking on 👁️ button on the file

>> https://usegalaxy.eu/datasets/11ac94870d0bb33a53e5694133aa9eb8/display?to_ext=png 







































# <img src = "https://user-images.githubusercontent.com/70078984/130058100-b045f3e2-f951-4148-aa71-4388391f5389.png" alt = "contibutors" width = "50" height = "50" /> </a>Contributors

Name | Slack Username | Contribution
---| ---| ---
Orababa Oluwatosin| @Toxhin | Obtaining Dataset and Introduction
Adeniran Fadekemi| @Morenike| Quality Control of Input Dataset Using FastQC 
Suchitra Thapa| @Suchitraa| Combining QC Results Using MultiQC
Michael Olufemi| @Micholufemi| Quality Trimming Using Trimmomatic
Adedoyin Adesoye| @Adedoyinsoye| Search for Contamination Using Kraken2
Sobambo Adedotun| @AdunniEagle| Finding Variants Using Snippy
Ahmed Sameh| @Ahhmedsamehh| Filtering TB Variant	
Neemah Jinadu| @Neemahj| TB profiling Using TB Profiler Profile
Camilia Mohamed Kamal| @CamiliaKamal2|	Text Transformation with sed
Christina Ashraf| @christina| TB variant report
Engy Khaled| @Engy|	View Snippy Output in JBrowser
Hossam Hatim| @HossamHatem|	Obtaining Dataset (OPTIONAL TASK) and Introduction
Menna|@Mennatullah|	Examining Sequence Quality with FastQC
Alabi Mosopefoluwa| @Mosopefoluwa|	Examining Sample Composition with Kraken2
Mustafa Mansour| @Mustafa|	Fetching Data and Introduction
Amira Tounsi| @AmiraT|	Examining Sequence Quality with FastQC
Omnia Alaa| @Omnia|	Quality Trimming with Trimmomatic
Suliat Jimoh| @SuliJimoh|	Finding Variants Using Snippy
Osmond Obinna| @Osmond|	Running Samtools on the BAM File
Yomna Yasser| @YomnaElkaramany|	Running BAM Coverage Plotter on the BAM File


###### Each team member worked on a subtask in the analysis and updated the markdown.
