# Genomics-two-B: Genomics Lab Working on Targeted Sequencing Dataset
## M. tuberculosis Variant Analysis
### By:

![image](https://user-images.githubusercontent.com/70078984/130058063-fd718b9e-c64e-4611-b44f-93d7a22d132e.png)


# <img src = "https://user-images.githubusercontent.com/70078984/130062938-6e2101b6-73f2-49b4-9ee9-22dd919f8dae.png" alt = "contents" width = "40" height = "40" /> </a> Contents
- [Introduction](#introduction)
- [Get Your Data](#get-your-data)
- [Quality Control](#quality-control)
- [Look For Contamination with Kraken2 (Optional)](#look-for-contamination-with-kraken2--optional-)
- [Find Variants with Snippy](#find-variants-with-snippy)
- [Further Variant Filtering and TB-Profiling](#further-variant-filtering-and-tb-profiling)
- [View Snippy Output in JBrowse](#view-snippy-output-in-jbrowse)
- [Different Samples, Different Stories (Optional)](#different-samples--different-stories--optional-)

Reference : <small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Introduction


## Get Your Data

## Quality Control
### FastQC




---
### [`MultiQC`](http://multiqc.info/) 

1. Search and select **`MultiQC- 🛠️`** from the tools list and fill in the details as below.

     >Results

      >`Which tool was used generate logs?` **_FastQC_**
      >> FastQC output
      
      >> `Type of FastQC output?` **_Raw data_**
      
      >>>`Output of BAMtools` **_FastQC on data: RawData_** (Select both RawData files)
2. Keep the rest parameters unchanged
3. Then **`Execute☑️`**
4. Observe the output MultiQC webpage file by clicking on 👁️ button on the file


### [**_quality trimming_**]


### ['quality trimming'] (to clean up the reads and remove the poor quality sections)

1. search and select **'Trimmomatic'** from the tools list and fill in the details as below
 
   >>“Single-end or paired-end reads?”: **_Paired End (two separate input files)_**
   
   >>“Input FASTQ file (R1/first of pair)”: 004-2_1.fastq.gz
   
   >>“Input FASTQ file (R2/second of pair)”: 004-2_2.fastq.gz
   
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



   

-----

## Look For Contamination with Kraken2 (Optional)


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
7. It was executed to show the following results
In a total of 1086 variants the first variant on the list is a Substitution of a C to a T which is supported by 134 reads.
![](https://res.cloudinary.com/adedotun/image/upload/v1629387184/samples/hackbio%20task/snippy_C-T_l0xyp7.png)



## Further Variant Filtering and TB-Profiling

### [`TB Variant Filter`]

1. Search and select **`TB Variant Filter- 🛠️`** from the tools list and adjust filters as below.


       >>'Select Filter variants by region, Filter variants close to indels and Filter sites by read alignment depth.

2. Choose the VCF files you wish to be filtered.
3. **`Execute☑️`**
4. Open the new VCF file.

###  
### ['Text transformation with sed'] 
1. “File to process”: TB Variant Filter on data 21
2. “SED Program”: s/GENE_//g 
>> https://usegalaxy.eu/datasets/11ac94870d0bb33a2ea3b318a7a7d7be/display?to_ext=vcf 

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

 
## Different Samples, Different Stories (Optional)
### feach data and introduction 
1.Fetch the data from EBI European Nucleotide Archive
>>ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR124/042/SRR12416842/SRR12416842_1.fastq.gz
>>ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR124/042/SRR12416842/SRR12416842_2.fastq.gz


















### BAM Coverage Plotter
1. search and select **'BAM Coverage Plotter'** from the tools list and fill in the details as below
>> "Will you select a reference genome from your history or use a built-in genome?" : use a genome from history in fasta formate 

>> "select the reference genome in fasta format" : (https://usegalaxy.eu/datasets/11ac94870d0bb33aca1bc094e2415e7e/display?to_ext=fasta)

>> " select the BAM file that you got from snippy. " : (https://usegalaxy.eu/datasets/11ac94870d0bb33a646435297267fb5a/display?to_ext=bam)

2. **`Execute☑️`** 

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
Adedotun Sobambo| @AdunniEagle| Finding Variants Using Snippy
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
