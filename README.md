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

-----

## Look For Contamination with Kraken2 (Optional)


## Find Variants with Snippy


## Further Variant Filtering and TB-Profiling

### [`TB Variant Filter`]

1. Search and select **`TB Variant Filter- 🛠️`** from the tools list and adjust filters as below.


       >>'Select Filter variants by region, Filter variants close to indels and Filter sites by read alignment depth.

2. Choose the VCF files you wish to be filtered.
3. **`Execute☑️`**
4. Open the new VCF file.

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






























### [`Run samtools stat` to generate statistics for BAM dataset]

1. On the tools sections, Search and select `samtools stat 🛠️`
2. Fill with the following details;
   >BAM File
   >> Ensure the mapped reads (bam) file of the snippy output is selected.

3. Keep the rest parameters unchanged.

4. Then `Execute`☑️

5. View the output, paying attention to the sequences, reads mapped and reads unmapped results.







































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
