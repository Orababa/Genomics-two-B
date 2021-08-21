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

## Introduction (by @Toxhin)
*Mycobacterium tuberculosis* is a Gram-positive rod-shaped bacterium that is known to cause tuberculosis. Tuberculosis (TB) is one of the world’s most dangerous infectious diseases and contributes to about 1.4 million annual deaths globally. Mutational changes in the genome of M. tuberculosis strains is one of the major contributing factors to its multidrug resistance nature and increased virulence. 

The genomics-two-B team recreated a galaxy tutorial on M. tuberculosis bacterium variant analysis which aims at identifying the variants, filtering the variants,predicting drug resistance from the identified variants and annotating the variants. 

This markdown contains the comprehensive step-wise procedure carried out during the recreation of the tutorial.

-----

## Get Your Data (by @Toxhin)

The data can be obtained using two methods:
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

### Quality Control of the Input Datasets (by @Morenike) 
Search and select `FastQC 🔧⚙️` from tools list and fill in the details below:

+ “Short read data from your current history”: select both FASTQ datasets
+ pay attention to the top part where Short read data from your current history is selected
+ leave all other parameters at their default values and click *Execute*.

<img src = "https://user-images.githubusercontent.com/70078984/130161538-76b88286-3af1-4ffb-a300-20f2eacde894.png" alt = "contents" width = "25" height = "25" /> </a> Tip: To Select Multiple Datasets

### 
click on <img src = "https://user-images.githubusercontent.com/70078984/130161941-23e6c3a1-5d0b-44b8-8aab-5813716f96b4.png" alt = "contents" width = "15" height = "15" /> 
</a> multiple datasets.

### 
The result should be four new datasets. One with the calculated raw data and another with an HTML report of the findings for each input dataset.
These will get added to your history.

While one could indepedently examine the quality control report for each set of reads (forward and reverse), it is quite useful to examine them side by side using the **MultiQC TOOL**

### Combining QC results (by @Suchitraa)

1. Search and select **`MultiQC 🔧⚙️`** from the tools list and fill in the details as below

     - In Results
     
       * `Which tool was used to generate logs?`: **FastQC**
       * In FastQC output
      
         * `Type of FastQC output?`: **Raw data**
         * `FastQC output`: Select both RawData files as shown below:
       ![Input_details](https://github.com/suchitrathapa/Microchitra/blob/main/MultiQC_1.png)
2. Keep the remaining parameters unchanged
3. Then **`Execute☑️`**
4. Observe the output MultiQC webpage file by clicking on 👁️ button on the file.
5. Look out for quality scores (phred scores, mean score), duplication level and adpator content then, decide the quality is sufficient for analysis or not.


### Quality Trimming (by @Micholufemi)

1. Search and select `Trimmomatic 🔧⚙️` from the search tools, to clean up the reads and remove the poor quality sections. The process is as shown below:

* “Single-end or paired-end reads?”: select "Paired End (two separate input files)"
* <img src = "https://user-images.githubusercontent.com/70078984/130161941-23e6c3a1-5d0b-44b8-8aab-5813716f96b4.png" alt = "contents" width = "15" height = "15" />  “Input FASTQ file (R1/first of pair)”: select "004-2_1.fastq.gz"
* <img src = "https://user-images.githubusercontent.com/70078984/130161941-23e6c3a1-5d0b-44b8-8aab-5813716f96b4.png" alt = "contents" width = "15" height = "15" />  “Input FASTQ file (R2/second of pair)”: select "004-2_2.fastq.gz"
* select "Trimmomatic operation to perform" and 
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

### Run Kraken2 (by @Adedoyinsoye)

1. search and select `Kraken2 🔧⚙️` from the search tools. The steps  are shown below:
 
  * “Single or paired reads”: select "Paired"
  * use the trimmed reads in the kraken analysis highlighting both the Forward Stand (R1 paired) and the Reverse strand (R2 paired)
* Print scientific names instead of just taxids”: select "Yes"
* “Enable quick operation”: select "Yes"
* under “Create reports”:
  * “Print a report with aggregrate counts/clade to file”: select "Yes"
* “Select a Kraken2 database”: select "Standard"
* click on **`Execute☑️`**

2. inspect the report produced by Kraken


-----

## Find Variants with Snippy

Snippy is a tool for rapid bacterial SNP calling and core genome alignments. It finds SNPs between a haploid reference genome and your NGS sequence reads. It will find both insertions/deletions (indels) and substitutions (snp).

If Snippy is given an annotated reference in Genbank format (.gbk), it will run a tool called SnpEff which will figure out the effect of any changes on the genes and other features. If Snippy is given the reference sequence alone without the annotations, it will not run SnpEff.

An annotated reference built from the inferred M. tuberculosis ancestral reference genome and the gene annotation from the H37Rv strain are available and will be used in this case.

### Run Snippy (by @AdunniEagle)


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

-----

## Further Variant Filtering and TB-Profiling

### Run Snippy (by @Ahhmedsamehh)

1. search and select **`TB Variant Filter 🔧⚙️`** from the tools list and adjust parameters as below:

* select VCF file to be filtered. The file is in the format - `snippy on data XX, data XX, and data XX mapped reads VCF file`
* select filters to be applied: `Filter variants by region`, `Filter variants close to indels` and `Filter sites by read alignment depth`.

2. **`Execute☑️`**

3. Open the new VCF file.

### Run TB Profiler and TB Variant Report

1. **`TB-Profiler profile 🔧⚙️`** (by @Neemahj)
- search and select `TB-Profiler profile 🔧⚙️` from search tools
- Input file type: select "BAM"
- The file to work on: _snippy on data XX, data XX, and data XX mapped reads (bam)_
- Click on **`Execute☑️`**

* TB Profiler produces 3 output files
  * its own VCF file
  * a report about the sample including its likely lineages and any AMR found
  * a [.json](https://.json.org) formatted results file.


2. Snippy prepends GENE_ to gene names in the VCF annotation, when it is run with Genbank format input. This causes a problem for TB Variant report, hence, there is need to edit the output with sed.

**`Text transformation with sed 🔧⚙️`** (by @CamiliaKamal2)
* “File to process”: TB Variant Filter on data XX
* “SED Program”: s/GENE_//g 


3. **`TB Variant Report 🔧⚙️`** (by @christina)
* “Input SnpEff annotated M.tuberculosis VCF(s)”: Text transformation on data XX
* “TBProfiler Drug Resistance Report (Optional)”: TB-Profiler Profile on data XX: Results.json

-----

## View Snippy Output in JBrowse 

### Run JBrowse (by @Engy)

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

-----
 
## Different Samples,Different Stories (Optional)

A southern African M. tuberculosis sample (sample 18-1) from the same study in Zenodo (aka. ERR1750907) is analysed. In some ways, it is quite different from the sample analysed in the tutorial thus far.

### Take a closer look at sample 18-1

1. **Fetch the data from Zenodo** (by @HossamHatem)
* click on upload data
* choose paste/fetch data
* paste your URLs into the text box

##### https://zenodo.org/record/3960260/files/018-1_1.fastq.gz
##### https://zenodo.org/record/3960260/files/018-1_2.fastq.gz
* click on `start`, and then `close`


###
2. **Examine the sequence quality with `FastQC🔧⚙️`** (by @Mennatullah)

The FastQC tool input form is shown below. Attention should be paid to only the top part where Short read data from your current history is selected. Leave all the other parameters at their default values and click Execute.

![image](https://user-images.githubusercontent.com/70078984/130250472-daeea5e0-a655-4abf-b837-500c04612e7d.png)


* four new datasets (one with the calculated raw data and another with an html report of the findings for each input dataset) will get added to the history
* from the report, we got that the data needs to be trimmed to remove the bad-quality base pairs, and adaptor contamination 


3. **Examine the sample composition with  `Kraken2 🔧⚙️`** (by @Mosopefoluwa) 

follow the steps below:

* search and select **`Kraken2 🔧⚙️`** from the tools list.
  * "Single or paired reads": select `Paired`
  * "Forward strand": select `018-1_1.fastq.gz`
  * "Reverse strand": select `018-1_2.fastq.gz`
* "Print scientific names instead of taxids": select `Yes`
* "Enable quick operation": select `Yes`
* under "Create report":
  * “Print a report with aggregrate counts/clade to file”: select `Yes`
* Select a Kraken2 database: Standard
* **`Execute☑️`**


-----

The next example is SRR12416842 from an Indonesia study of multi-drug resistant (MDR) tuberculosis.

### Take a closer look at sample SRR12416842
1. **Fetch the data from EBI European Nucleotide Archive** (by @Mustafa)
>>ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR124/042/SRR12416842/SRR12416842_1.fastq.gz
>>ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR124/042/SRR12416842/SRR12416842_2.fastq.gz


2. **Examine the sequence quality with `FastQC🔧⚙️`** (by @AmiraT)

* In the tools list search for **FastQC** and select it

* To select the input files

  * click multiple dataset icon
  * choose the  two previously uploaded datasets
  * leave the other parameters at their default values

* click execute

Four new files will be added to your history. The result for each dataset will be a HTML report (the webpage file) and the rawData file. 


3. **Perform quality trimming with  `Trimmomatic🔧⚙️`** (by @Omnia)


* search and select **'Trimmomatic'** from the tools list and fill in the details as below
 
  * “Single-end or paired-end reads?”: **_Paired End (two separate input files)_**
   
  * “Input FASTQ file (R1/first of pair)”: select `SRR12416842_1.fastq.gz`
   
  * “Input FASTQ file (R2/second of pair)”: select `SRR12416842_2.fastq.gz`
   
  * “Select Trimmomatic operation to perform“ : Keep the default value of **Sliding window trimming** and adjust the average quality required to 30
   
* “+Insert Trimmomatic Operation”:
  * “Select Trimmomatic operation to perform”: Drop reads below a specified length (MINLEN)
  * “Minimum length of reads to be kept”: 20
  
* **`Execute☑️`**

4 new datasets will be created in the history, containing the 4 trimommatic data (2 paired and 2 unpaired).

Inspect the output trimommatic results  webpage file by clicking on 👁️ button on the file

4. **Map the samples to the M. tuberculosis reference genome with Snippy** (by @SuliJimoh)

### 
1. search and select **'Snippy'** from the tools list and fill in the details as below:

* “Will you select a reference genome from your history or use a built-in index?”: Use a genome from history and build index
* “Use the following dataset as the reference sequence”: Mycobacterium_tuberculosis_ancestral_reference.gbk
* “Single or Paired-end reads”: Paired
* “Select first set of reads”: Trimmomatic on SRR12416842_1.fastq.gz(R1 paired)
* “Select second set of reads”: Trimmomatic on SRR12416842_2.fastq.gz (R2 paired)
* Under “Advanced parameters”
  * “Minimum proportion for variant evidence”: 0.1 (This is so we can see possible rare variants in our sample)
*Under “Output selection” select only the following:
  * “The final annotated variants in VCF format”

  * “A simple tab-separated summary of all the variants”

  * “The alignments in BAM format”.

* finally, select **`Execute☑️`**

![optional task 2-1](https://user-images.githubusercontent.com/88276401/130168756-3d462195-b7a4-41bb-a653-dea4852fec79.PNG)

![optional task 2-2](https://user-images.githubusercontent.com/88276401/130168775-c2f28073-6b1f-49f0-89a0-e14297992b58.PNG)

3 new datasets will be created in the history, a vcf file, snps table and a mapped reads file in bam format.


2. Inspect the Snippy VCF output to check the number of variant discovered by **'Snippy'** on 👁️ button on the file

###
5. **Run samtools stats** (by @Osmond)

* On the tools sections, search and select `samtools stat 🔧⚙️`
* Fill with the following details:
   + BAM File
   + Ensure the mapped reads (bam) file of the snippy output is selected.

* Keep the rest parameters unchanged.

* Then `Execute`☑️

* View the output and pay attention to the sequences, reads mapped and reads unmapped results.


###
6. **Run the `BAM Coverage Plotter 🔧⚙️` tool on the mapped reads BAM file that you got from snippy** (by @YomnaElkaramany)

* search and select **'BAM Coverage Plotter'** from the tools list and fill in the details as below:
* "Will you select a reference genome from your history or use a built-in genome?" : use a genome from history in fasta format 

  * "select the reference genome in fasta format" : MTB_ancestor_reference.fasta
 
  * " select the BAM file that you got from snippy. " : Snippy on data XX, data XX and data XX mapped reads (bam)

* **`Execute☑️`**

* Observe the output trimommatic results  webpage file by clicking on 👁️ button on the file.

-----

###
The data (ouput) from the analysis performed in this tutorial can be found in the histories below, which have been made acccesible.

* https://usegalaxy.eu/u/genomicstwob/h/mtb-variant-calling-genomicstwob-sample-004-2
* https://usegalaxy.eu/u/genomicstwob/h/mtb-variant-calling-genomicstwob-sample-18-1
* https://usegalaxy.eu/u/genomicstwob/h/optional-task-2

-----

# <img src = "https://user-images.githubusercontent.com/70078984/130058100-b045f3e2-f951-4148-aa71-4388391f5389.png" alt = "contibutors" width = "50" height = "50" /> </a>Contributors

Name | Slack Username | Contribution
---| ---| ---
Orababa Oluwatosin| @Toxhin | Introduction
Orababa Oluwatosin| @Toxhin | Get Your Data
Adeniran Fadekemi| @Morenike| Quality Control 
Suchitra Thapa| @Suchitraa| Quality Control 
Michael Olufemi| @Micholufemi| Quality Control 
Adedoyin Adesoye| @Adedoyinsoye| Search for Contamination Using Kraken2
Sobambo Adedotun| @AdunniEagle| Finding Variants Using Snippy
Ahmed Sameh| @Ahhmedsamehh| Further Variant Filtering and TB Profiling	
Idowu Jinadu| @Neemahj| Further Variant Filtering and TB Profiling
Camilia Mohamed Kamal| @CamiliaKamal2|	Further Variant Filtering and TB Profiling
Christina Ashraf| @christina| Further Variant Filtering and TB Profiling
Engy Khaled| @Engy|	View Snippy Output in JBrowser
Hossam Hatim| @HossamHatem|	Different Samples, Different Stories (Sample 18-1)
Menna|@Mennatullah|	Different Samples, Different Stories (Sample 18-1)
Alabi Mosopefoluwa| @Mosopefoluwa|	Different Samples, Different Stories (Sample 18-1)
Mustafa Mansour| @Mustafa|	Different Samples, Different Stories (Sample SRR12416842)
Amira Tounsi| @AmiraT|	Different Samples, Different Stories (Sample SRR12416842)
Omnia Alaa| @Omnia|	Different Samples, Different Stories (Sample SRR12416842)
Suliat Jimoh| @SuliJimoh|	Different Samples, Different Stories (Sample SRR12416842)
Osmond Obinna| @Osmond|	Different Samples, Different Stories (Sample SRR12416842)
Yomna Yasser| @YomnaElkaramany|	Different Samples, Different Stories (Sample SRR12416842)


###### Each team member worked on a subtask in the analysis and updated the markdown. Members of the team also practiced the entire tutorial on their personal galaxy accounts.

the tutorial reproduced was obatined from: https://training.galaxyproject.org/training-material/topics/variant-analysis/tutorials/tb-variant-analysis/tutorial.html

![image](https://user-images.githubusercontent.com/70078984/130314125-789968eb-312e-4570-8d7a-515dad05b5a5.png)


The reproduction of this tutorial was encouraged by HackBio

![image](https://user-images.githubusercontent.com/70078984/130058063-fd718b9e-c64e-4611-b44f-93d7a22d132e.png)
