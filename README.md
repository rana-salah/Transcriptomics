# Transcriptomics

# Functional enrichment analysis

After extraction and annotation of differentially expressed genes, it is important for us to identify biological functions that might be impacted by the depletion of Passilla(ps). But first, we would like to know if the differentially expressed genes are enriched transcripts of genes which belong to more common or specific catogories. We performed two(2) analysis to address this-\
<li>gene ontology analysis</li>
<li>Kegg pathway analysis</li>

## 1.Gene Ontology Analysis

```Goseq R bioconductor package``` was used for this analysis

To begin the analysis, goseq was used to quantify the length bias present in the dataset under
consideration. This is done by calculating a Probability Weighting Function or PWF which can
be thought of as a function which gives the probability that a gene will be differentially expressed
(DE) based on the length alone. We obtain a weight for each gene, depending on its length, given by the PWF.

The input files is the differentially expressed genes from all genes assayed in the RNA-Seq experiment

   - "the Gene IDs (unique within the file): in uppercase letters"
   - "a boolean indicating whether the gene is differentially expressed or not (True if differentially expressed or False if not)"

![GO_5 1](https://user-images.githubusercontent.com/88287437/130139574-3d9e4341-a22d-4b74-8889-e9638bdf78fa.PNG)

goseq generates outputs with the following columns for each GO term:

   - "Category": GO category
   - "over_rep_pval": p-value for over-representation of the term in the differentially expressed genes
   - "under_rep_pval": p-value for under-representation of the term in the differentially expressed genes
   - "numDEInCat": number of differentially expressed genes in this category
   - "numInCat": number of genes in this category term: detail of the term
   - "ontology": MF (Molecular Function - molecular activities of gene products), CC (Cellular Component - where gene products are active), BP (Biological Process - pathways and 
   larger processes made up of the activities of multiple gene products)
   - "p.adjust.over_represented": p-value for over-representation of the term in the differentially expressed genes"
   - P.adjust.under_represented: p-value for under-representation of the term in the differentially expressed genes" 
   procedure
    
![GO_RANK 1](https://user-images.githubusercontent.com/88287437/130142391-1d2631b5-dc3f-48ee-aa7e-3351c208876c.PNG)

Results:

A graph with the top 10 over-represented GO terms.The x-axis is the percentage of genes in the category that have been identified as differentially expressed


[GO_TOP[2].pdf](https://github.com/rana-salah/Transcriptomics/files/7017700/GO_TOP.2.pdf)

 ## 2.KEGG Pathway Analysis

```goseq``` was also used to identify KEGG pathways for our analysis. The KEGG pathway database is a collection of pathway maps representing the current knowledge on the molecular interaction, reaction and relation networks.for our KEGG analysis two(2) files were generated

inputs include

![GO_KEGG 1](https://user-images.githubusercontent.com/88287437/130145621-0202a61f-7e1a-4659-a125-d1589b2f6438.PNG)

Results:

```pathview``` generates a collection with the KEGG visualization: one file per pathway.The colored boxes are genes in the pathway that are differentially expressed 

    - "green is for value below 0, it means for genes with a log2FC < 0"
    - "red for genes with a log2FC > 0"

![Galaxy18--Pathview_on_data_17__KEGG_Pathway_(dme00010)- 1](https://user-images.githubusercontent.com/88287437/130145732-05f35a39-71d6-4af4-840e-1f5f33157886.png)

![Galaxy37--Pathview_on_data_35__KEGG_Pathways__dme03040- 1](https://user-images.githubusercontent.com/88287437/130145880-13195cfb-6e5d-444b-9dc7-0c7bce2b8188.png)

<h2> TEAM CONTRIBUTORS </h2>
<table>
  <tr>
    <td>GO analysis</td>
    <td>Amira,TosinA,ShurutiG, Bandana, Johny</td> 
  </tr>
   <tr>
    <td>KEGG pathway Analysis</td>
   <td>Amira, Rana, TosinA, Chigozie</td>
   </tr>
     <tr>
    <td>Documentation and creation of ReadMe file</td>
    <td>TosinA</td> 
     </tr>
</table>




