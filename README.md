# Transcriptomics

# Functional enrichment analysis

Ater extraction and annotation of differentially expressed genes it is important for us to identify biological functions that might be impacted by the depletion of Passilla(ps). But first, we would like to know if the differentially expressed genes are enriched transcripts of genes which belong to more common or specific catogories. We performed 2 analysis to address this
-gene ontology analysis
-Kegg pathway analysis

## 1.Gene Ontology Analysis

```Goseq R bioconductor package``` was used for this analysis

To begin the analysis, goseq was used to quantify the length bias present in the dataset under
consideration. This is done by calculating a Probability Weighting Function or PWF which can
be thought of as a function which gives the probability that a gene will be differentially expressed
(DE)based on the lenght alone. We obtain a weight for each gene, depending on its length, given by the PWF.

The input files is the differentially expressed genes from all genes assayed in the RNA-Seq experiment

   - "the Gene IDs (unique within the file), in uppercase letters"
   - "a boolean indicating whether the gene is differentially expressed or not (True if differentially expressed or False if not)"

![GO_5 1](https://user-images.githubusercontent.com/88287437/130139574-3d9e4341-a22d-4b74-8889-e9638bdf78fa.PNG)


