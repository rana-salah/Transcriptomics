# Transcriptomics

# Mapping

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

<h2> TEAM CONTRIBUTORS </h2>
<table>
  <tr>
   <td><strong>Mapping</strong>
   </td>
   <td>Yasmin, Dawoud, Saket, Bandana, Nirvana, Johny, Favour, Ankita, Eman
   </td>
  </tr>
  <tr>
   <td>Inspection of Mapping results
   </td>
   <td>Yasmeen, Nirvana, Dawoud, Bandana
   </td>
  </tr>
  <tr>
   <td>Counting the number of reads per annotated gene
   </td>
   <td>Yasmeen, Saket, Johny
   </td>
  </tr>
  <tr>
   <td>Estimation of the strandness
   </td>
   <td>Eman, Saket, Johny
   </td>
  </tr>
  <tr>
   <td>
Counting reads per genes


   </td>
   <td>Ankita, Favour, Nirvana
   </td>
   </tr>
   <tr>
   <td>
Creating Readme


   </td>
   <td>Saket, Yasmeen, Johny, Dawoud
   </td>
  </tr>
  <tr>
   <td>
Documentation


   </td>
   <td>Eman, Yasmein, Dawoud, Johny
   </td>
  </tr>
</table>



