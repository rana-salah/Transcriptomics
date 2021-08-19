<h1 align="center"> Quality Control </h1>

# Preparing the reads
## Import data from URLs

- Read sequences are usually stored in compressed (gzipped) FASTQ files.The raw reads used in this tutorial were obtained from zenodo library from the link mentioned in the image below:


<h1 align="center"> 
  
![data1](https://user-images.githubusercontent.com/57266535/130147539-53ee007c-000f-4431-9f98-624d21b70faa.png)
</h1>

- The reads are raw data from the sequencing machine without any pretreatments. They need to be assessed for their quality.

1)	Quality control

Before analysis, we have to carry out a quality check for our reads to get rid of any errors resulted during the sequencing process such as incorrect nucleotides being called and prevent misinterpretation of the data. Hence, Quality Assessment is important to check the quality of your sequenced reads.

Here, we use:

a.) 

| **Fast QC** | Description |
|----------| ----------------------------------------------------------|
| Function | A quality control tool for high throughput sequence data. |
|  Steps | Go to NGS: QC and manipulation / Select FastQC / Press Execute.|
|  Input file| .fastqsanger |
|  Output files | (I) Raw data: It consists of text description. (II) Web page – It consists of detail graphical representation of your fastq data. |

Note: One can select multiple datasets to run multiple files simultaneously.

<h1 align="center"> 
  
![data2](https://user-images.githubusercontent.com/57266535/130148312-ba648c91-1b63-4e58-bc02-37a4b610a474.png)
</h1>

b.)

| **MultiQC** | Description |
|----------| ----------------------------------------------------------|
| Function | Summarises the output from numerous bioinformatics tools (FastQC, Cutadapt, BAM etc.) into a single report. |
|  Steps | Go to NGS: QC and manipulation / Select MultiQC / Select FastQC Raw datas / Press Execute.|
|  Input file| FastQC Raw Data |

<h1 align="center"> 
  
![data3](https://user-images.githubusercontent.com/57266535/130148318-178028ff-fe6b-48f2-bad6-5f4faac486aa.png)
</h1>
c.)

| **Cutadapt** | Description |
|----------| ----------------------------------------------------------|
| Function | To improve the quality of sequences via trimming and filtering for Transcriptome alignment and is used to remove overrepresented reads.
|  Steps | a.) Data Upload |
|--------| b.) To trim Low quality sequences, Select the Paired end according to our raw data GSM1177 and GSM1180 and the forward, reverse read respectively.|
|--------| c.) In Filter option the “minimum length = 20”|
|--------| d.) In Read modification option “Quality cut-off = 20”|
|--------| e.) In Output option “Report = Yes”|
|  Input file| FastQC file |

<h1 align="center"> 
   
![data5](https://user-images.githubusercontent.com/57266535/130148319-2c116dea-c09a-492c-971f-67c6b6d1ff00.png)
</h1>

## Results: 

**1.) MultiQC:** All files except GSM461180_2 have a high proportion of duplicated reads which is expected in RNA-Seq data. The “Per base sequence quality” is globally good with a slight decrease at the end of the sequences. Moreover, almost no known adapters and overrepresented sequences are present.

**2.) Cutadapt:** For GSM461177 5,072,810 forward reads and 8,648,619 bp of reverse reads have been removed due to quality. GSM461180 has a forward bandwidth of 10,224,537 bps and a reverse bandwidth of 51,746,850  bps.: At the conclusion of the reads, the quality of the reverse reads dropped more than the forward reads, especially for GSM461180.For GSM461177, 147,810 (2.4%) readings were too short, whereas 1101875 (12.4%) were too short for GSM461180.

<h1 align="center">   

![data-6](https://user-images.githubusercontent.com/57266535/130150826-e4c48041-8327-47df-a3f8-0a071a2bccf2.png)

![data6](https://user-images.githubusercontent.com/57266535/130153144-3aac5182-a5c5-4dff-8ca2-4644542d84c7.png)
![data-6](https://user-images.githubusercontent.com/57266535/130153081-61cfa7c7-43ac-480e-ad01-299541ae3c8c.png)
</h1>

<h2> TEAM CONTRIBUTORS </h2>
<table>
  <tr>
   <td><strong>Quality Control (subsamples)</strong>
   </td>
   <td>Yasmin, Eman
   </td>
  </tr>
  <tr>
   <td>Quality Control (full files)
   </td>
   <td> Bandana, Jaspreet
   </td>
  </tr>
  <tr>
   <td>Results
   </td>
   <td> Bandana, Jaspreet, Eman
   </td>
  </tr>
  <tr>
   <td>
Creating Readme


   </td>
   <td>Jaspreet
   </td>
  </tr>
  <tr>
   <td>
Documentation


   </td>
   <td>Jaspreet, Bandana, Eman
   </td>
  </tr>
</table>
