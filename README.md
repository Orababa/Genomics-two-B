# Genomics-two-B: Genomics Lab Working on Targeted Sequencing Dataset
## M. tuberculosis Variant Analysis

## <img src = "https://user-images.githubusercontent.com/70078984/130062938-6e2101b6-73f2-49b4-9ee9-22dd919f8dae.png" alt = "contents" width = "40" height = "40" /> </a> Contents
- [Introduction](#introduction)
- [Get Your Data](#get-your-data)
- [Quality Control](#quality-control)
- [Look For Contamination with Kraken2 (Optional)](#look-for-contamination-with-kraken2-optional)
- [Find Variants with Snippy](#find-variants-with-snippy)
- [Further Variant Filtering and TB-Profiling](#further-variant-filtering-and-tb-profiling)
- [View Snippy Output in JBrowse](#view-snippy-output-in-jbrowse)
- [Different Samples, Different Stories (Optional)](#different-samples--different-stories-optional)

Reference: <small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Introduction 
*Mycobacterium tuberculosis* is a Gram-positive rod-shaped bacterium that is known to cause tuberculosis. Tuberculosis (TB) is one of the world’s most dangerous infectious diseases and contributes to about two million annual deaths globally. Mutational changes in the genome of M. tuberculosis strains is one of the major contributing factors to its multidrug resistance nature and increased virulence. 

The genomics-two-B team recreated a galaxy tutorial on M. tuberculosis bacterium variant analysis which aims at identifying the variants, filtering the variants,predicting drug resistance from the identified variants and annotating the variants. This markdown contains the comprehensive step-wise procedure carried out during the recreation of the tutorial.

-----

## Get Your Data

The data used in the tutorial can be obtained using two methods:
1. By importing the files from Zenodo.
- copy the Zenodo link location
- open the galaxy upload manager and select the fetch/paste option
- paste the copied link in the text box
- click on the start option and close the upload window
     
2. By importing the files from one's computer library
- click on the galaxy upload manager
- navigate to the choose from local file option
- select the files to be uploaded and click on start
- close the upload window

### 
Zenodo links for the datasets are shown below:

#####
https://zenodo.org/record/3960260/files/004-2_1.fastq.gz

https://zenodo.org/record/3960260/files/004-2_2.fastq.gz

https://zenodo.org/record/3960260/files/Mycobacterium_tuberculosis_ancestral_reference.gbk

https://zenodo.org/record/3960260/files/MTB_ancestor_reference.fasta

https://zenodo.org/record/3960260/files/Mycobacterium_tuberculosis_h37rv.ASM19595v2.45.chromosome.Chromosome.gff3

-----

## Quality Control
This step serves to identify possible issues with the raw sequenced read input data before starting the “real” analysis steps.

Some of the typical problems with NGS data can be reduced by pre-processing affected sequencing reads before trying to map them to the reference genome. Early detection of some other, more severe problems may at least save a lot of time spent on analysing low-quality data that is not worth the effort.

Here is how we performed a standard quality check on our input data.

### Quality Control of the Input Datasets
Search and select `FastQC 🔧⚙️` from tools list and fill in the details below:

+ “Short read data from your current history”: select both FASTQ datasets
+ pay attention to the top part where Short read data from your current history is selected
+ leave all other parameters at their default values and click *Execute*.

### <img src = "https://user-images.githubusercontent.com/70078984/130161538-76b88286-3af1-4ffb-a300-20f2eacde894.png" alt = "contents" width = "25" height = "25" /> </a> Tip: To Select Multiple Datasets

### 
click on <img src = "https://user-images.githubusercontent.com/70078984/130161941-23e6c3a1-5d0b-44b8-8aab-5813716f96b4.png" alt = "contents" width = "15" height = "15" /> 
</a> multiple datasets.

### 
The result should be four new datasets. One with the calculated raw data and another with an HTML report of the findings for each input dataset.
These will get added to your history.

While one could indepedently examine the quality control report for each set of reads (forward and reverse), it is quite useful to examine them side by side using the **MultiQC TOOL**

### Combining QC results

1. Search and select **`MultiQC 🔧⚙️`** from the tools list and fill in the details as below

     - In Results
     
       * `Which tool was used to generate logs?`: **FastQC**
       * In FastQC output
      
         * `Type of FastQC output?` **Raw data**
         * `Output of BAMtools` **_FastQC on data: RawData_** (Select both RawData files)
         ![Input_details](https://github.com/suchitrathapa/Microchitra/blob/main/MultiQC_1.png)
2. Keep the remaining parameters unchanged
3. Then **`Execute☑️`**
4. Observe the output MultiQC webpage file by clicking on 👁️ button on the file.
     ![Output_file](https://github.com/suchitrathapa/Microchitra/blob/main/MultiQC_2.png)
5. Look out for quality scores (phred scores, mean score), duplication level and adpator content then, decide the quality is sufficient for analysis or not.


### Quality Trimming

1. Search and select `Trimmomatic 🔧⚙️` from the search tools, to clean up the reads and remove the poor quality sections. The process is as shown below:

* “Single-end or paired-end reads?”: select "Paired End (two separate input files)"
* <img src = "https://user-images.githubusercontent.com/70078984/130161941-23e6c3a1-5d0b-44b8-8aab-5813716f96b4.png" alt = "contents" width = "15" height = "15" />  “Input FASTQ file (R1/first of pair)”: select "004-2_1.fastq.gz"
* <img src = "https://user-images.githubusercontent.com/70078984/130161941-23e6c3a1-5d0b-44b8-8aab-5813716f96b4.png" alt = "contents" width = "15" height = "15" />  “Input FASTQ file (R2/second of pair)”: select "004-2_2.fastq.gz"
* Select "Trimmomatic operation to perform" and 
  * Keep the default value of **Sliding window trimming** and adjust the average quality required to 30
* select “+Insert Trimmomatic Operation”
  * click on “Select Trimmomatic operation to perform”: Drop reads below a specified length (MINLEN)
  * “Minimum length of reads to be kept”: 20
click on **`Execute☑️`**

2. finally, check the output produced by Trimmomatic
-----

## Look For Contamination with Kraken2 (Optional)

Looking for contamination in our reads is essential to producing a good work, as other sources of DNA accidentally or inadvertently get mixed in with our sample and this can confound our snp analysis. 

Kraken 2 is an effective way of looking at which species is represented in our reads and so we can easily spot possible contamination of our sample. 

### Run Kraken2

1. search and select `Kraken2 🔧⚙️` from the search tools. The steps  are shown below:
 
  * “Single or paired reads”: select "Paired"
  * the already trimmed reads are used in the kraken analysis highlighting both the Forward Stand (R1 paired) and the Reverse strand (R2 paired)
* Print scientific names instead of just taxids”: select "Yes"
* “Enable quick operation”: select "Yes"
* under “Create reports”:
  * “Print a report with aggregrate counts/clade to file”: select "Yes"
* “Select a Kraken2 database”: select "Standard"
* click on **`Execute☑️`**

2. inspect the report produced by Kraken

![](https://res.cloudinary.com/adedoyinsoye/image/upload/v1629442271/HACKBIO/Screenshot_19_t6vvut.png)

A report of the analysis was produced later on:
 
![](https://res.cloudinary.com/adedoyinsoye/image/upload/v1629442272/HACKBIO/Screenshot_20_aly0hu.png)

From the report generated; it was inferred that about 91% of the reads were positively identified as Mycobacterium. The others found were bacteria from the same kingdom. There were no contaminating human or viral sequences detected.

-----

## Find Variants with Snippy

Snippy is a tool for rapid bacterial SNP calling and core genome alignments. It finds SNPs between a haploid reference genome and your NGS sequence reads. It will find both insertions/deletions (indels) and substitutions (snp).

If Snippy is given an annotated reference in Genbank format (.gbk), it will run a tool called SnpEff which will figure out the effect of any changes on the genes and other features. If Snippy is given the reference sequence alone without the annotations, it will not run SnpEff.

An annotated reference built from the inferred M. tuberculosis ancestral reference genome and the gene annotation from the H37Rv strain are available and will be used in this case.

### Run Snippy


1. select the reference genome "from history and build index"
 
2. use `Mycobacterium_tuberculosis_ancestral_reference.gbk` dataset as the reference sequence

3. select `Paired` for the end reads

4. select Trimmomatic on X (R1 paired) as first set of reads

5. select Trimmomatic on X (R2 paired) as second set of reads

![](https://res.cloudinary.com/adedotun/image/upload/v1629387169/samples/hackbio%20task/snippy_1_uynhgv.png)

6. in the “Advanced parameters”, adjust “Minimum proportion for variant evidence” to 0.1, so we can see possible rare variants in the sample

![](https://res.cloudinary.com/adedotun/image/upload/v1629387173/samples/hackbio%20task/snippy_2_cjs4h1.png)

7. under “Output selection”, select the following:
- “The final annotated variants in VCF format”
- “A simple tab-separated summary of all the variants” and
- “The alignments in BAM format” 
- deselect others

8. click on **`Execute☑️`** 

9. the following results are obtained

In a total of 1086 variants the first variant on the list is a Substitution of a C to a T which is supported by 134 reads.
![](https://res.cloudinary.com/adedotun/image/upload/v1629387184/samples/hackbio%20task/snippy_C-T_l0xyp7.png)

-----

## Further Variant Filtering and TB-Profiling

### Run Snippy

1. search and select **`TB Variant Filter 🔧⚙️`** from the tools list and adjust parameters as below:

* select VCF file to be filtered. The file is in the format - `snippy on data XX, data XX, and data XX mapped reads VCF file`
* select filters to be applied: `Filter variants by region`, `Filter variants close to indels` and `Filter sites by read alignment depth`.

2. **`Execute☑️`**

3. Open the new VCF file.

### Run TB Profiler and TB Variant Report

1. **`TB-Profiler profile 🔧⚙️`**
- search and select `TB-Profiler profile 🔧⚙️` from search tools
- Input file type: select "BAM"
- The file to work on: _snippy on data 16, data 15, and data 3 mapped reads (bam)_
- Click on **`Execute☑️`**

* TB Profiler produces 3 output files
  * its own VCF file
  * a report about the sample including its likely lineages and any AMR found
  * a [.json](https://.json.org) formatted results file.


2. Snippy prepends GENE_ to gene names in the VCF annotation, when it is run with Genbank format input. This causes a problem for TB Variant report, hence, there is need to edit the output with sed.

**`Text transformation with sed 🔧⚙️`**
* “File to process”: TB Variant Filter on data XX
* “SED Program”: s/GENE_//g 

>> https://usegalaxy.eu/datasets/11ac94870d0bb33a2ea3b318a7a7d7be/display?to_ext=vcf 

3. **`TB Variant Report 🔧⚙️`**
* “Input SnpEff annotated M.tuberculosis VCF(s)”: Text transformation on data XX
* “TBProfiler Drug Resistance Report (Optional)”: TB-Profiler Profile on data XX: Results.json

-----

## View Snippy Output in JBrowse 

### Run JBrowse

1.	search and select **JBrowse 🔧⚙️`** from the tools list and adjust parameters as below.
  * “Reference genome to display”: Use a genome from history
    * “Select the reference genome”: https://zenodo.org/record/3497110/files/MTB_ancestor_reference.fasta
  * Produce Standalone Instance”: Yes
  * "Genetic Code”: 11: The Bacterial, Archaeal and Plant Plastid Code
  * “JBrowse-in-Galaxy Action”: New JBrowse Instance
      
2. set up different 3 tracks:
+ 'Track 1 - sequence reads: Click on Insert Track Group and fill it with
  + 'Track Category' to sequence reads
  + click on 'Insert Annotation Track' and fill it with
    + “Track Type” to BAM Pileups
    + “BAM Track Data” to snippy on data XX, data XX, and data XX mapped reads (bam)
    + “Autogenerate SNP Track” to Yes
    + “Track Visibility” to On for new users
    
 
+ Track 2 - variants: Click on Insert Track Group and fill it with
  + “Track Category” to variants
  + click on Insert Annotation Track and fill it with
    + “Track Type” to VCF SNPs
    + “SNP Track Data” to TB Variant Filter on data XX and
    + “Track Visibility” to On for new users  
     
+ Track 3 - annotated reference: Click on Insert Track Group and fill it with 
  + “Track Category” to annotated reference.
  + Click on Insert Annotation Track and fill it with
    + “Track Type” to GFF/GFF3/BED Features
    + “GFF/GFF3/BED Track Data” to https://zenodo.org/record/3531703/files/Mycobacterium_tuberculosis_h37rv.ASM19595v2.45.chromosome.Chromosome.gff3
    + “JBrowse Track Type [Advanced]” to Canvas Features
    + click on “JBrowse Styling Options [Advanced]”
    + “JBrowse style.label” to product
    + “JBrowse style.description” to product and
    + “Track Visibility” to On for new users
   
3. **`Execute☑️`**
4. A new dataset will be created in your history, containing the JBrowse interactive visualisation

 
## Different Samples,Different Stories (Optional)

1. Fetch the data from Zenodo:
   * choose Optional task 1 history
   * click on upload data
   * choosw paste/fetch data
   * paste your URLs
   * click on colse
   
2.	check for data quality using fastQC
3.	  { to find out the quality report} 
4.	1. Select from options at the left Fasta/Fastq 
5.	2. Choose FastQC to read quality report **FastQC**
6.	3. Choose your data set 
7.	>> insert multiple files, and choose the previously uploaded files
8.	>> follow default parameters 
9.	4. press **execute**
10.	5. Four files will be created composed of two html page for the quality report 
11.	6. Form the report we got that the data need to be trimmed to remove the bad-quality base pairs, and adaptor contamination 
	

3. Examine the sample composition with  **`Kraken2 🔧⚙️`** by following the steps below:
* search and select **`Kraken2 🔧⚙️`** from the tools list.
  * "Single or paired reads": `Paired`
  * "Forward strand": `018-1_1.fastq.gz`
  * "Reverse strand": `018-1_2.fastq.gz`
* "Print scientific names instead of taxids": `Yes`
* "Enable quick operation": `Yes`
* under "Create report":
  * “Print a report with aggregrate counts/clade to file”: `Yes`
* Select a Kraken2 database: Standard
* **`Execute☑️`**




## introduction 
Illumina HiSeq 2500 paired end sequencing; Whole genome sequencing analysis of multi-drug resistant Mycobacterium tuberculosis from Java, Indonesia

### fetch data and introduction
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

![seq quality](https://user-images.githubusercontent.com/88459905/130209416-e192b6c9-ba9e-443e-9af5-77dc9348d94e.PNG)
![fs st](https://user-images.githubusercontent.com/88459905/130209463-bc7c5db1-3109-4890-b328-67eda2793be2.PNG)

### quality trimming
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




### *_BAM Coverage Plotter_*
1. search and select **'BAM Coverage Plotter'** from the tools list and fill in the details as below
>> "Will you select a reference genome from your history or use a built-in genome?" : use a genome from history in fasta formate 

>> "select the reference genome in fasta format" : (https://usegalaxy.eu/datasets/11ac94870d0bb33aca1bc094e2415e7e/display?to_ext=fasta)

>> " select the BAM file that you got from snippy. " : (https://usegalaxy.eu/datasets/11ac94870d0bb33a646435297267fb5a/display?to_ext=bam)

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
![image](https://user-images.githubusercontent.com/70078984/130058063-fd718b9e-c64e-4611-b44f-93d7a22d132e.png)
