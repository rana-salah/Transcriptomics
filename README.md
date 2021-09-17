

# Mapping step

***
## 1. Alignment step


> We need to first figure out where the sequences originated from in the genome, so we can then determine to which genes they belong. \
> When a reference genome for the organism is available, this process is known as **aligning** or **“mapping”** the reads to the reference. 

```STAR tool``` was used 
Drosophila_melanogaster.BDGP6.87.gtf was our reference genome and was retrieved from data library
**[click here for the link](https://zenodo.org/record/4541751/files/Drosophila_melanogaster.BDGP6.87.gtf)**
 The input files were cutadapt outputs with PairedEnd reads of sample GSM461177 and GSM461180
 
    - Custom or built-in reference genome”: Use a built-in index
    - “Reference genome with or without an annotation”: use genome reference without builtin gene-model
    - “Select reference genome”: Fly (_Drosophila Melanogaster_): dm6 
    - “Gene model (gff3,gtf) file for splice junctions”: the imported _Drosophila_melanogaster.BDGP6.87.gtf_
    - “Length of the genomic sequence around annotated junctions”: 36
    - This parameter should be length of reads - 1 (read length of fastq file was 37 so we put 36)
      
 <h3 align="center">
 
 ![alt_text](output/1.jpg) 
 
</h3>
    
 ```MultiQC```: to aggregate the STAR logs
 
 <h3 align="center">
 
 ![alt_text](output/2.png)

</h3>
    
 Results
 <h3 align="center">
 
 ![alt_text](output/3.png)

</h3>
    
 More than 83% for GSM461177 and more than 79% for GSM461180. We can proceed with the analysis since only percentages below 70% should be investigated for potential contamination.
## 2. BAM File Inspection

<kbd>*GSM461177*

<h3 align="center">
 
 ![alt_text](output/4.png "GSM461177")
 
 </h3>
 
 <kbd>*GSM461180*
 
<h3 align="center">
 
 ![alt_text](output/5.png "GSM461180")
 
  </h3>
    
## 3. Further Inspection on IGV (mouse over for description)

*Display on local IGV*

<h3 align="center">

<img src="https://user-images.githubusercontent.com/71490918/130114580-8d34a23c-7255-43e7-9267-694c9ebc85f6.png" width=75% height=75%>

</h3>

*Zoom onto Chr4 loci on IGV*

<h3 align="center">

<img src="https://user-images.githubusercontent.com/71490918/130115430-f2160aa2-9dbb-4aa5-b0a5-33e19f84a301.png" width=75% height=75%>

</h3>
 
*IGV panel*

<h3 align="center">

<img src="https://user-images.githubusercontent.com/71490918/130114757-5d77f6a7-1871-4b48-9dc1-c695bab41361.png" width=75% height=75%>

</h3>
 
*Sashimi plot*

<h3 align="center">

<img src="https://user-images.githubusercontent.com/71490918/130115294-ad7ad1c1-d9bb-4c25-b5c7-7d512183c18e.png" width=75% height=75%>

</h3>
  
### Further check for the quality of the data: 
  by inspecting read duplication level, number of reads mapped to each chromosome, gene body coverage, and read distribution across features

 - **Duplicate reads**

  We observed some duplication level events in the data from our first ```MultiQC``` webpage report. 
  
<h3 align="center">

  <img src="https://user-images.githubusercontent.com/71490918/130114845-c19be5b2-2187-4741-b1db-79e0d329be8d.png" width=75% height=75%>

</h3>

These duplicate reads can come from over amplification (PCR in dependent library preparations – like with short read sequencing like illumina) or from highly-expressed genes (which is usually kept in RNA-Seq differential expression analysis).
  
```MarkDuplicates``` (on outputs of ```RNA STAR```): To check for duplicate reads
  
```MultiQC```: To aggregate results from both samples

  
<h3 align="center">

  <img src="https://user-images.githubusercontent.com/71490918/130114409-21dcd2de-0bb2-4b0f-b018-58fb15c69457.png" width=75% height=75%>

</h3>
  
<h3 align="center">

Results show that the sample <kbd>*GSM461177* 

has 9.2% of duplicated reads while <kbd>*GSM461180* 
  
has 10.0%.
  
</h3>

In general, up to 50% of duplication can be considered normal to obtain. So, both our samples are good.
  
- **Number of reads mapped to each chromosome**
  To check for mitochondrial contamination, sex of samples or see if any chromosome has highly expressed genes.
  
*Drosophila melanogaster* has 4 chromosomal pairs: X/Y, 2, 3, and 4. 
  
```Samtools IdxStats```: To check for the number of reads mapped to each chromosome and then ```MultiQC```: To aggregate results from both samples

<h3 align="center">

  <img src="https://user-images.githubusercontent.com/71490918/130114089-24f2f846-883d-4742-8bd6-972445bf0bde.png" width=75% height=75%>

</h3>
 
We observed that reads from both samples mapped mostly to chromosome 2 (chr2L and chr2R), 3 (chr3L and chr3R), X and a sequence that is unassigned to the reference (chrUn_CP). Only some reads mapped to mitochondrial chromosome (chrM).


<h3 align="center">

  <img src="https://user-images.githubusercontent.com/71490918/130115052-aa54ba39-c726-4396-9692-89fdc933c8a8.png" width=75% height=75%>

</h3>

Both samples a most probably from female flies.
  
- **Gene Body Coverage**

A gene body refers to the different regions of a gene. A gene has regulatory regions such as the promoter region etcetera. It is important to check if reads coverage is uniform over gene body or if there is any 5’/3’ bias. A bias towards the 5’ end of genes could indicate RNA degradation while a 3’ bias could indicate that the data is from a 3’ assay (such as PCR dependent sequencing technologies).

```Gene Body Coverage (BAM)```: To assess gene body coverage on the results of RNA STAR (i.e. .bam files) and inputting the reference gene as model (i.e. after converting the drosophila reference .gtf file to BED format using ```Convert GTF to BED12```). 
 
 
 ```MultiQC```: To aggregate results from both samples onto the output.

 
<h3 align="center">

  <img src="https://user-images.githubusercontent.com/71490918/130114937-aa2bfbf0-cd34-4200-a78b-71180e762d47.png" width=75% height=75%>

</h3>
  
For both samples, 

<kbd>*GSM461177*
  
<kbd>*GSM461180*
  
there is even gene coverage from 5’ to 3’ ends (despite some noise in the middle). So, no obvious bias in both samples.
  
- **Read distribution across features**

With RNA-Seq data, we expect most reads to map to exons rather than introns or intergenic regions. Before going further in counting and differential expression analysis, it may be interesting to check the distribution of reads across known gene features (exons, CDS, 5’ UTR, 3’ UTR, introns, intergenic regions). For example, a high number of reads mapping to intergenic regions may indicate the presence of DNA contamination.

```Gene Body Coverage (BAM)```: To assess read distribution and identify the position of the different gene features on the results of the annotation file parsed against an input of the reference gene in BED format (output of ```Convert GTF to BED12```).

```MultiQC```: To aggregate results from both samples onto the output.
  
<h3 align="center">

  <img src="https://user-images.githubusercontent.com/71490918/130114982-5401cc84-088b-4e01-a3af-e883f5a5eb06.png" width=75% height=75%>

</h3>

Most of the reads from both samples are mapped to exons (≥80%). Only ~2% to introns and ~4-5% to intergenic regions. This confers validity to our RNA-Seq data and mapping.
    
## 4.Counting the number of reads per annotated gene
Estimation of the strandness:
First, we used Convert GTF to BED12 Tool: to convert the GTF file to BED

<h3 align="center">
 
 ![alt_text](output/15.jpg)
 
 </h3>
    
Then Infer Experiment Tool: to determine the library strandness:

<h3 align="center">
 
![alt_text](output/16.jpg)

</h3>
    
Results: 

<h3 align="center">
 
 ![alt_text](output/17.jpg)
 
 </h3>
    
Since the two “Fractions of reads explained by” the numbers are close to each other or nearly equal, we conclude that the library is not a ```strand-specific dataset (unstranded).```
## 5. Counting reads per genes;
```FeatureCounts Tool:``` was used to count the number of reads per gene
The input files are the mapped bam files and also the annotation file which is the Drosophilla melanongaster .gtf file.

<h3 align="center">
 
 ![alt_text](output/18.jpg)
 
 </h3>
    
 ```MultiQC Tool:``` was used to aggregate the report

<h3 align="center">
 
 ![alt_text](output/19.jpg)
 
 </h3>
Results:

<h3 align="center">
 
 ![alt_text](output/20.png)
 
  </h3>
    
Around 63% of the reads have been assigned to genes: this quantity is good enough.If the percentage falls below 50%, you should investigate where your reads are mapping (inside genes or not, with IGV) and then check that the annotation corresponds to the correct reference genome version.

=======
Quality_Control
<h1 align="center"> Quality Control </h1>

# Preparing the reads
<h2> :point_right: Import data from URLs </h2>

- Read sequences are usually stored in compressed (gzipped) FASTQ files.The raw reads used in this tutorial were obtained from zenodo library from the link mentioned in the image below:


<h1 align="center"> 
  
![data1](https://user-images.githubusercontent.com/57266535/130147539-53ee007c-000f-4431-9f98-624d21b70faa.png)
</h1>

- The reads are raw data from the sequencing machine without any pretreatments. They need to be assessed for their quality.

<h2> :point_right:	Quality control </h2>

   Before analysis, we have to carry out a quality check for our reads to get rid of any errors resulted during the sequencing process such as incorrect nucleotides being called and prevent misinterpretation of the data. Hence, Quality Assessment is important to check the quality of your sequenced reads.

 Here, we use:

# 1.

|**Fast QC** | Description |
|:----------:| ------------|
| Function | A quality control tool for high throughput sequence data. |
|  Steps | Go to NGS: `QC and manipulation / Select FastQC / Press Execute.`|
|  Input file| .fastqsanger |
|  Output files | (I) Raw data: It consists of text description.|
||(II) Web page – It consists of detail graphical representation of your fastq data. |

```Note:``` One can select multiple datasets to run multiple files simultaneously.

<h1 align="center"> 
  
![data2](https://user-images.githubusercontent.com/57266535/130148312-ba648c91-1b63-4e58-bc02-37a4b610a474.png)
</h1>

# 2.

| **MultiQC** | Description |
|:----------:| ----------|
| Function | Summarises the output from numerous bioinformatics tools (FastQC, Cutadapt, BAM etc.) into a single report. |
|  Steps | Go to NGS: `QC and manipulation / Select MultiQC / Select FastQC Raw datas / Press Execute.`|
|  Input file| FastQC Raw Data |

<h1 align="center"> 
  
![data3](https://user-images.githubusercontent.com/57266535/130148318-178028ff-fe6b-48f2-bad6-5f4faac486aa.png)
</h1>

# 3.

| **Cutadapt** | Description |
|:----------:| --------------|
| Function | To improve the quality of sequences via trimming and filtering for Transcriptome alignment and is used to remove overrepresented reads.
|  Steps | a.) Data Upload |
|| b.) To trim Low quality sequences, Select the Paired end according to our raw data GSM1177 and GSM1180 and the forward, reverse read respectively.|
|| c.) In Filter option the “minimum length = 20”|
|| d.) In Read modification option “Quality cut-off = 20”|
|| e.) In Output option “Report = Yes”|
|  Input file| FastQC file |

<h1 align="center"> 
   
![data5](https://user-images.githubusercontent.com/57266535/130148319-2c116dea-c09a-492c-971f-67c6b6d1ff00.png)
</h1>

<h2> :point_right:	Results </h2> 

```1.) MultiQC:``` All files except GSM461180_2 have a high proportion of duplicated reads which is expected in RNA-Seq data. The “Per base sequence quality” is globally good with a slight decrease at the end of the sequences. Moreover, almost no known adapters and overrepresented sequences are present. </p>

```2.) Cutadapt:``` For GSM461177 5,072,810 forward reads and 8,648,619 bp of reverse reads have been removed due to quality. GSM461180 has a forward bandwidth of 10,224,537 bps and a reverse bandwidth of 51,746,850  bps.: At the conclusion of the reads, the quality of the reverse reads dropped more than the forward reads, especially for GSM461180.For GSM461177, 147,810 (2.4%) readings were too short, whereas 1101875 (12.4%) were too short for GSM461180.

<h1 align="center">   

![data-6](https://user-images.githubusercontent.com/57266535/130150826-e4c48041-8327-47df-a3f8-0a071a2bccf2.png)

![data6](https://user-images.githubusercontent.com/57266535/130153144-3aac5182-a5c5-4dff-8ca2-4644542d84c7.png)
![data-6](https://user-images.githubusercontent.com/57266535/130153081-61cfa7c7-43ac-480e-ad01-299541ae3c8c.png)
</h1>
=======
<!-- Typing SVG by DenverCoder1 - https://github.com/DenverCoder1/readme-typing-svg -->
<p align="center">
  <a href="https://github.com/DenverCoder1/readme-typing-svg"><img src="https://readme-typing-svg.herokuapp.com/?lines=%20Welcome%20to%20Transcriptomics%20Lab%20;A%20diverse%20group%20of%20enthusiasts;Working%20on%20transcript%2D%27omics%27%20&center=true&width=440&height=45&color=f75c7e&vCenter=true&size=24"></a>
</p>
<h3 align="center">
  <img src="https://user-images.githubusercontent.com/88287648/130135203-b6c055e5-0839-4b64-baf2-86b55200130e.gif" width="500" height="250">
</h3>

<h2 align="center"> 
  Hello!!<img src="https://media.giphy.com/media/hvRJCLFzcasrR4ia7z/giphy.gif" width="28">
  Welcome to Transcriptomics Lab- Where we do all things RNA:mechanical_arm: :tada: </h2> 
  :man_scientist: :woman_scientist: We are research enthusiasts from diverse backgrounds in life sciences:scientist: :man_scientist:  .In our lab we study transcriptome to provide insight into how genes work:microbe: . This ReadMe provides complete analysis of an RNA-seq experiment profiling Drosophila cells after the depletion of a regulatory gene called Pasilla (Ps).

Enjoy!🌟 

# <p align = "center"> <img src = "https://www.pngkey.com/png/full/399-3992475_bonhomme-loupe-png-transparent-background-person-with-magnifying.png" width = "70" height = "70" /> </a>  Summary </p>

***A SNEAK PEEK INTO OUR LAB***

Pasilla gene 🧬 encodes a set of proteins that are most similar to those found in humans Nova-1 and Nova-2 are the names of two satellites. Nova-1 and Nova-2 are nuclear RNA-binding proteins that are normally expressed in the CNS and regulate splicing directly.There are numerous applications for RNA sequencing data with a reference genome, and there is no optimal pipeline for all cases. We will go over all  the major steps in Reference based RNA-seq data analysis 💻, such as quality control (FastQC, MutiQC, Cutadapt), reads alignment (RNA STAR, IGV), gene and transcript quantification (HT-Seq count), differential gene expression (DEseq2), functional profiling (goseq) and advanced analysis. Using this [tutorial](https://training.galaxyproject.org/training-material/topics/transcriptomics/tutorials/ref-based/tutorial.html#introduction), we were able to do differnetial  gene expression analysis besides identifying the genes and pathways regulated by the Pasilla gene as they were  affected by its depletion. To know more about our adventures 🤯 with the analysis process, don'y hesitate to check the following hyperlinks 🔥🔗. You will explore a lot about the field of transcriptomics in the next minutes 👀 

## <p align = "center"> 📌 Our Workflow 📝  </p>
<h3 align="center">
<img src= "https://user-images.githubusercontent.com/88304342/130128293-3c8839a6-48ae-4633-b6dc-162a6afd06e8.png" width="800" height="600">
</h3>

*To check out the results of our workflow in galaxy, click on the links below:*

**[Quality_Control](https://usegalaxy.org/u/jasbio/h/analysis)** |  **[Mapping](https://usegalaxy.org/u/yasmien/h/rna-seq-tutorial)** | **[Analysis_Visualisation_Functional Enrichment](https://usegalaxy.eu/u/utkarsha/h/deseq2)**

<h1 align="center"> 📃 Datasets and Results 📈 </h1>

***Click on each image to know the detailed procedure to conduct this analysis step***
<p align="center">
       <a href = "https://docs.google.com/document/d/1d4vWIoz1llimFA5zCjPaglrZrBX8Co6gbt9WcvK6l1Y/edit">
          <kbd> <img src="https://user-images.githubusercontent.com/88287648/130311901-89251a88-70b5-4004-b3b5-22f2cc77677f.png" width = "300" height ="250"> </kbd>
     </a>
  <a href="https://github.com/rana-salah/Transcriptomics/tree/Quality_Control">
      <kbd> <img src="https://user-images.githubusercontent.com/88287648/130313730-7a244afd-4687-4d04-ab7f-9e7e5d034967.png" width = "300" height = "250"> </kbd>
    </a>
    <br>
  
  <a href="https://github.com/rana-salah/Transcriptomics/tree/Mapping">
    <kbd> <img src="https://user-images.githubusercontent.com/88287648/130312527-3edcaba1-29d6-4dab-b8b2-2b8502befd68.png" width = "300" height = "250"> </kbd>
 </a>
  <br>
  <a href="https://github.com/rana-salah/Transcriptomics/tree/DE-analysis">
      <kbd> <img src="https://user-images.githubusercontent.com/67822771/130317235-cf4491cf-b4d9-47f2-95ef-2cb1a0c86559.png" width = "300" height ="250"> </kbd>
  </a>
  <a href="https://github.com/rana-salah/Transcriptomics/tree/visualization">
      <kbd> <img src="https://user-images.githubusercontent.com/67822771/130317248-ce75699a-15c1-4430-acc5-cde7ef500f91.png" width = "300" height ="250"> </kbd>
  </a>
    <br>
  <a href="https://github.com/rana-salah/Transcriptomics/tree/Functional-Analysis-(GO-and-KEGG)">
    <kbd> <img src="https://user-images.githubusercontent.com/88287648/130313044-a3d0d4c5-112a-4c39-a19b-f0a9fbfab2a0.png" width = "300" height ="250"> </kbd> 

  </a>


</p>
Finally, after finishing our analysis 🎉🎉, our team decided to start an initiative to help all the members in learning from each other and in developing the biggest set of skills 🧰during this stage! Hence, we organized a training on Friday, where we had a workshop for each step in the workflow. The highly passionate members 👨‍🔬 👩‍🔬 who volunteered to give the workshops are highlighted in the contributions list. In this workshop, the moderators explained the purpose of doing each step in the tutorial and how it can be benefitial for the analysis :bookmark: beside highlighting some improvement points. Also, they did the anlysis process practically to help the other members follow their steps. At the end, there was a troubleshooting and a Q&A session. 


***Getting to the end of our work, are you excited to meet our team members?!!*** 😍🥳🥳


# <p align = "center"> 👩‍🔬 Team Members 👨‍🔬 </p>

</p>
<h3 align="center">
<img src = "https://user-images.githubusercontent.com/67822771/130310781-f2c6ecf2-4c51-4b6e-a22a-5d79a28d83fb.gif" width = "500" heigth = "200"> <img src = "https://user-images.githubusercontent.com/88287648/130283020-9a0a12b4-f51e-4079-92c5-390b33ef3b8d.gif" width = "500" heigth = "200"> 
<img src = "https://user-images.githubusercontent.com/67822771/130310808-8c67a1f0-46b1-4568-b594-425a95cc789c.gif" width = "500" heigth = "200"> <img src = "https://user-images.githubusercontent.com/67822771/130310818-c91bf2f0-921b-4833-9caf-1b68774e901c.gif" width = "500" heigth = "200"> 
<img src = "https://user-images.githubusercontent.com/67822771/130310932-50ac9646-1804-4f84-bc31-67ef491a34e9.gif" width = "500" heigth = "200"> <img src = "https://user-images.githubusercontent.com/67822771/130310947-8ffca48e-e53a-4468-a21e-00555b7f0e60.gif" width = "500" heigth = "200"> 
<img src = "https://user-images.githubusercontent.com/67822771/130310849-6063c2f9-41f1-457b-8d3d-493ed4b6cf9e.gif" width = "500" heigth = "200"> <img src = "https://user-images.githubusercontent.com/67822771/130310853-84e38542-67a1-454c-aa46-e0938a805edc.gif" width = "500" heigth = "200">
<img src = "https://user-images.githubusercontent.com/67822771/130310984-3475d21c-a5e0-4159-b9e3-ed33187b7b19.gif" width = "500" heigth = "200"> <img src = "https://user-images.githubusercontent.com/67822771/130310873-56dd33b9-8cfe-4259-8885-9a2d17705065.gif" width = "500" heigth = "200">
<img src = "https://user-images.githubusercontent.com/67822771/130313891-6a1a2c0c-d458-4f32-89d9-de7bebf63e0d.gif" width = "500" heigth = "200"> <img src = "https://user-images.githubusercontent.com/67822771/130313898-e940148e-df40-4465-9ed1-cf28c3874e80.gif" width = "500" heigth = "200">
<img src = "https://user-images.githubusercontent.com/67822771/130310998-5d2766d6-99e5-4ee3-b932-08bae863cdf7.gif" width = "500" heigth = "200"> <img src = "https://user-images.githubusercontent.com/67822771/130311001-7f77b193-6563-4d0e-bff5-c9ab9b3ccbeb.gif" width = "500" heigth = "200">
<img src = "https://user-images.githubusercontent.com/67822771/130311010-f7e06905-edab-438b-bde8-9e6f38ebb90d.gif" width = "500" heigth = "200"> <img src = "https://user-images.githubusercontent.com/67822771/130311013-f555b172-9544-4ae5-85f5-0ad136e03d3a.gif" width = "500" heigth = "200">
<img src = "https://user-images.githubusercontent.com/67822771/130311057-503fa8b2-92c1-4380-8988-e9c4466a3c56.gif" width = "500" heigth = "200"> <img src = "https://user-images.githubusercontent.com/67822771/130311063-0b93298c-37f4-4145-9b2e-9ca3805526e1.gif" width = "500" heigth = "200">
<img src = "https://user-images.githubusercontent.com/67822771/130311067-04a6646b-4679-47e5-972d-d92c619c7243.gif" width = "500" heigth = "200"> <img src = "https://user-images.githubusercontent.com/67822771/130311072-0c625935-4523-4585-8b10-f5f509e96292.gif" width = "500" heigth = "200">
  
## <p align = "center"> 👩‍💻 Contributions 👨‍💻 </p>


| Team Sub-groups | Specific Task | Contributors  | Slack IDs |
| :-------------: | :-----------: | :------------:| :-------: |
| Quality Control | Sub-samples   | Yasmeen & Eman | @Sam & @Eman|
| Quality Control | Full datasets | Bandana, Jaspreet, Pankaj| @Bandana, @Jaspreet, @Pankaj|
| Quality Control | Markdown Documnetation | Jaspreet, Bandana, Eman| @Eman, @Bandana & @Jaspreet |
|     Mapping     | Inspection of Mapping Results | Yasmeen, Bandana, Dawoud, Nirvana | @Sam, @Bandana, @Dawoud, @Nirvana |
|     Mapping     | Counting the number of reads per annotated gene | Yasmeen, Saket, Johny | @Sam, @Saket, @Johny |
|     Mapping     | Estimation of strandness | Eman, Saket, Johny | @Saket, @Johny, @Eman|
|     Mapping     | Counting reads per genes | Ankita, Favour, Nirvana | @Anku., @Nirvana, @OYEFAVOUR |
|     Mapping     | Markdown Documentation | Eman, Yasmeen, Dawoud, Johny| @Sam & @Eman, @Dawoud, @Johny |
| Differential Gene Expression Analysis | Identification of the differentially expressed features | Rana, Utkarsha, Osama, Nikita, Chigozie, Yasmeen, Johny, Shruti| @RanaSalah, @-Utkarsha12-, @Osama, @Nikita2Chimera, @GozieNkwocha, @Sam, @Johny, @ShrutiG |
| Differential Gene Expression Analysis | Extraction of annotation of differentially expressed genes | Utkarsha, Osama, Rana, Jaspreet, Nikita, Chigozie | @RanaSalah, @-Utkarsha12-, @Osama, @Nikita2Chimera, @GozieNkwocha, @Jaspreet|
| Differential Gene Expression Analysis | Markdown Documentation | Rana| @RanaSalah |
| Visualization of the DE genes' expression | Visualization of the normalized counts | Osama, Utkarsha, Rana, Ankita, TosinA, Dawoud | @RanaSalah, @-Utkarsha12-, @Osama, @Anku., @TosinA, @Dawoud|
| Visualization of the DE genes' expression | Computation & Visualization of the Z-score | Saket, Utkarsha, Osama, Rana, Ankita, TosinA, Diyar |@Saket, @RanaSalah, @-Utkarsha12-, @Osama, @Anku., @TosinA, @diyar |
| Visualization of the DE genes' expression | Markdown Documentation | Osama, Jaspreet, Utkarsha| @Osama, @Jaspreet, @-Utkarsha12-|
| Functional enrichment analysis of the DE genes | Gene Ontology analysis | Amira, TosinA, Bandana, Johny, Shruti|@Amira, @TosinA, @Bandana, @Johny, @ShrutiG |
| Functional enrichment analysis of the DE genes | KEGG pathways analysis | Amira, TosinA, Chigozie, Rana| @Amira, @TosinA, @GozieNkwocha, @RanaSalah |
| GitHUb Markdown Development | Format & Organization   | Main ReadMe: Utkarsha, Osama, Rana, Ankita, TosinA, Bandana. Quality control: Jaspreet. Mapping: Saket, Yasmeen, Johny, Dawoud. Differential Gene Expression Analysis: Rana. Visualization: Osama, Jaspreet, Utkarsha. Functional Enrichment Analysis: TosinA | @RanaSalah, @-Utkarsha12-, @Osama, @Anku., @TosinA, @Bandana, @Jaspreet, @Sam, @Saket, @Johny, @Dawoud |
| Graphical Abstract Design |   | Rana, Osama, Jaspreet, Ankita, Diyar | @RanaSalah, @Osama, @Jaspreet, @Anku., @diyar|
| Advertisement |  Writing post on transfer-market | Tosin | @TosinA|
| Training | Moderated the training workshops & presented the workflow steps practically | Quality control: Yasmeen & Jaspreet. Mapping: Saket, Yasmeen. Differential Gene Expression Analysis: Rana & Osama. Visualization: Osama. Functional Enrichment Analysis: Amira| @Sam & @Jaspreet, @Saket, @RanaSalah, @Osama, @Amira|
  
# <p align = "center"> References </p>

Trapnell, C., L. Pachter, and S. L. Salzberg, 2009 TopHat: discovering splice junctions with RNA-Seq. Bioinformatics 25: 1105–1111. https://academic.oup.com/bioinformatics/article/25/9/1105/203994
  
Bérénice Batut, Mallory Freeberg, Mo Heydarian, Anika Erxleben, Pavankumar Videm, Clemens Blank, Maria Doyle, Nicola Soranzo, Peter van Heusden, 2021 Reference-based RNA-Seq data analysis (Galaxy Training Materials). https://training.galaxyproject.org/training-material/topics/transcriptomics/tutorials/ref-based/tutorial.html Online; accessed Sat Aug 21 2021
  
Batut et al., 2018 Community-Driven Data Analysis Training for Biology Cell Systems 10.1016/j.cels.2018.05.012

Levin, J. Z., M. Yassour, X. Adiconis, C. Nusbaum, D. A. Thompson et al., 2010 Comprehensive comparative analysis of strand-specific RNA sequencing methods. Nature Methods 7: 709. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3005310/
  
Young, M. D., M. J. Wakefield, G. K. Smyth, and A. Oshlack, 2010 Gene ontology analysis for RNA-seq: accounting for selection bias. Genome Biology 11: R14. https://genomebiology.biomedcentral.com/articles/10.1186/gb-2010-11-2-r14
  
Marcel, M., 2011 Cutadapt removes adapter sequences from high-throughput sequencing reads. EMBnet.journal 17: http://journal.embnet.org/index.php/embnetjournal/article/view/200
  
Brooks, A. N., L. Yang, M. O. Duff, K. D. Hansen, J. W. Park et al., 2011 Conservation of an RNA regulatory map between Drosophila and mammals. Genome Research 21: 193–202. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3032923/
  
Robinson, J. T., H. Thorvaldsdóttir, W. Winckler, M. Guttman, E. S. Lander et al., 2011 Integrative genomics viewer. Nature Biotechnology 29: 24. https://www.nature.com/nbt/journal/v29/n1/abs/nbt.1754.html
  
Wang, L., S. Wang, and W. Li, 2012 RSeQC: quality control of RNA-seq experiments. Bioinformatics 28: 2184–2185. https://www.ncbi.nlm.nih.gov/pubmed/22743226
  
Dobin, A., C. A. Davis, F. Schlesinger, J. Drenkow, C. Zaleski et al., 2013 STAR: ultrafast universal RNA-seq aligner. Bioinformatics 29: 15–21. https://academic.oup.com/bioinformatics/article/29/1/15/272537
  
Liao, Y., G. K. Smyth, and W. Shi, 2013 featureCounts: an efficient general purpose program for assigning sequence reads to genomic features. Bioinformatics 30: 923–930. https://academic.oup.com/bioinformatics/article/31/2/166/2366196
  
Kim, D., G. Pertea, C. Trapnell, H. Pimentel, R. Kelley et al., 2013 TopHat2: accurate alignment of transcriptomes in the presence of insertions, deletions and gene fusions. Genome Biology 14: R36. https://genomebiology.biomedcentral.com/articles/10.1186/gb-2013-14-4-r36
  
Luo, W., and C. Brouwer, 2013 Pathview: an R/Bioconductor package for pathway-based data integration and visualization. Bioinformatics 29: 1830–1831. https://academic.oup.com/bioinformatics/article-abstract/29/14/1830/232698
  
Love, M. I., W. Huber, and S. Anders, 2014 Moderated estimation of fold change and dispersion for RNA-seq data with DESeq2. Genome Biology 15: 550. https://genomebiology.biomedcentral.com/articles/10.1186/s13059-014-0550-8
  
Kim, D., B. Langmead, and S. L. Salzberg, 2015 HISAT: a fast spliced aligner with low memory requirements. Nature Methods 12: 357. https://www.nature.com/articles/nmeth.3317
  
Anders, S., P. T. Pyl, and W. Huber, 2015 HTSeq—a Python framework to work with high-throughput sequencing data. Bioinformatics 31: 166–169. https://academic.oup.com/bioinformatics/article/31/2/166/2366196
  
Ewels, P., M. Magnusson, S. Lundin, and M. Käller, 2016 MultiQC: summarize analysis results for multiple tools and samples in a single report. Bioinformatics 32: 3047–3048. https://academic.oup.com/bioinformatics/article/32/19/3047/2196507
  
Thurmond, J., J. L. Goodman, V. B. Strelets, H. Attrill, L. S. Gramates et al., 2018 FlyBase 2.0: the next generation. Nucleic Acids Research 47: D759–D765. https://academic.oup.com/nar/article-abstract/47/D1/D759/5144957
  
Kim, D., J. M. Paggi, C. Park, C. Bennett, and S. L. Salzberg, 2019 Graph-based genome alignment and genotyping with HISAT2 and HISAT-genotype. Nature Biotechnology 37: 907–915. https://www.nature.com/articles/s41587-019-0201-4
  
Used usegalaxy.org: "The sequencing data were uploaded to the Galaxy web platform, and we used the public server at usegalaxy.org to analyze the data ( Brooks et al. 2011)."

</h3>
</p>
<h3 align="center">

  <img src="https://user-images.githubusercontent.com/88287648/130313233-545de160-c88c-48de-9742-b9bf3cd81ebb.gif" width="1000" height="300">
</h3

