# Analysis of the differential gene expression
## Identification of the differentially expressed features
After being able to obtain the files with the counts of the reads mapped to the Drosophila genes for each of the 7 samples, the next step is identifying the differential gene expression induced by the Pasilla gene depletion 👩‍💻👨‍💻 Here is our tips & tricks for getting this task done.

**First**, you should upload your datasets to [Galaxy](https://usegalaxy.eu/). In our case, we uploaded the three treated and four untreated datasets as shown here. 
![Input](https://user-images.githubusercontent.com/67822771/130218303-1ce10894-4925-4be2-b814-f380810c7c91.PNG)

**Second**, we chose to use ```DESeq2 Software``` for the differential gene expression analysis of our RNA-seq data. The ```DESeq2 software``` has multiple benefitial usages as shown in the following figure. Before starting the analysis, the software combines the count files of the different samples into one table to apply normalization for sequencign depth, library composition and gene length. However, for our data, the gene length normalization is not required because we are comparing the counts for the same gene between differenet sample groups. 
![benefit](https://user-images.githubusercontent.com/67822771/130219587-7429f866-5acc-4115-9e6c-4350c5c104b8.PNG)

**Moving to the Differential Gene Expression (DGE) analysis, its two main tasks are as follows:**

- Estimating the biological variance using the replicates for each condition.
- Estimating the significance of expression differences between any two conditions.

As a clarification, there are two types of replicates: technical replicates & biological replicates. A technical replicate is obtained by measuring the same experiment several times (e.g. multiple sequencing of the same library). While, a biological replicate is obtained by both performing & measuring the experiment multiple times. For our data, there are 4 biological replicates (samples) without treatment and 3 biological replicates with treatment where the Pasilla gene is depleted by RNAi.

In the differential expression analysis, multiple **factors** with several levels can be incorporated representing the known sources of variation (e.g. treatment, tissue type, gender, batches). Each factor can have two or more **levels** to show its conditions. After the normalization step, specifying the factors can help compare the response of the expression of any gene to the presence of different levels of a factor in a statistically reliable way.

**To srart**, The paramters of the ```DESeq2``` tool on Galaxy should be adjusted by adding all the factors that may affect gene expression in your experiment. In our multi-factor analysis, we worked on two factors:

- Treatment (either treated or untreated).
- Sequencing type (paired-end or single-end).

In the following figures, you can find provided the adjusted settings for the first level of each of our factors. 

Here, treatment is the primary factor that we are interested in. 
![Factor 1](https://user-images.githubusercontent.com/67822771/130224885-8e8575c7-6568-444c-b7fe-1b22d89cac0e.PNG)

The sequencing type is further information we know about the data that might contribute to differences in gene expression.
![factor 2](https://user-images.githubusercontent.com/67822771/130224904-f0e7c532-e506-4f4b-9447-6275dc5e8013.PNG)

After executing this step, ```DESeq2``` generated 3 outputs.

``` 1. The normalized counts for each gene in the samples.
    2. A graphical summary of the results, useful to evaluate the quality of the experiment, including:
      - PCA
      - Heatmap
      - Dispersion estimates.
      - Histogram
      - MA plot
    3. A summary file with the following values for each gene:
       - Gene identifiers
       - Mean normalized counts, averaged over all samples from both conditions
       - Fold change in log2 (logarithm base 2)
       - Standard error estimate for the log2 fold change estimate
       - Wald statistic 
       - p-value for the statistical significance of this change 
       - p-value adjusted for multiple testing with the Benjamini-Hochberg procedure, which controls false discovery rate (FDR)
```
> Note: The log2 fold changes are based on the primary factor level 1 vs factor level 2, hence the input order of factor levels is important. Here, DESeq2 computes fold changes of ‘treated’ samples against ‘untreated’ from the first factor ‘Treatment’, i.e. the values correspond to up- or downregulation of genes in treated samples.

1. The normalized counts are presented in a table with the genes added in (rows) and the samples highlighted in (columns) as shown in the following figure:

![result 1](https://user-images.githubusercontent.com/67822771/130226202-af7fe545-b1e6-4395-91a9-f7cf94782157.PNG)

2. The PCA run on the normalized counts of the samples was plotted using the first 2 dimensiosn PC1 (57% Variance) & PC2 (27% Variance) as shown on the following figure. 

![pca](https://user-images.githubusercontent.com/67822771/130227503-415152d2-6245-479e-b936-fc0c83708652.PNG)

> The PCA helps in evaluating the relations between the different replicates. It allows the researchers to detect unwanted variations (e.g. batch effects). In this analysis, each  replicate was plotted as an individual data point. It can be concluded, that the replicates were successfully grouped togther according to the levels of the two factors and no    hidden effects seemed to be present on the data.

3. The Heatmaps are used to give an overview of the similarities and dissimilarities between samples through clustering them. Additionally, they follow a color code to represent the distance between the samples in which the darker shades (dark blue in this heatmap) means shorter distance, i.e. the two samples are close to eachother given the normalized counts. *In our analysis, a heatmap of the sample-to-sample distance matrix (with clustering) was generated based on the normalized counts as shown in the following figure.*

![hearmap](https://user-images.githubusercontent.com/67822771/130227513-e72c8e23-a423-4b6a-95aa-1e7ddc0b3772.PNG)

> Based on the interpretation of the heatmap, it can be concluded that it confirms the results of the PCA as the samples were first grouped by the treatment (the first factor) and secondly by the sequencing type (the second factor), as in the PCA plot.

4. The Dispersion estimates plot is generated from the final estimates shrunk from the gene-wise estimates towards the fitted estimates. In this figure, you can see that the plot labeled as follows: gene-wise estimates (black), the fitted values (red), and the final maximum a posteriori estimates used in testing (blue). 

![dispersion](https://user-images.githubusercontent.com/67822771/130227547-0da0339b-0353-4640-8241-fadcde2a04f3.PNG)

5. This Histogram was generated showing the  p-values for the genes under the comparison between the two levels of the first factor.

![histo](https://user-images.githubusercontent.com/67822771/130227567-74b7d45a-29f7-4360-a0c6-81385c1ce9af.PNG)

6. The MA plots display the global view of the relationship between the expression change of conditions (log ratios, M), the average expression strength of the genes (average mean, A), and the ability of the algorithm to detect differential gene expression. In the following figure, we have the significant genes passing the treshold of (p-value < 0.1) colored in red.

![MA plot](https://user-images.githubusercontent.com/67822771/130227598-98c00e1e-b7d7-43ec-a0ce-09cd9a1f886b.PNG)

Finally, Here is the top of the summary file showing the (base mean, log2 fold change, wald-stats, p-value, etc.) of the differentially expressed genes.

![chosen](https://user-images.githubusercontent.com/67822771/130275692-ffd9889f-7161-4dd2-89d5-a8c79b23b9a9.PNG)

```
Final Remark: If you target adding the interaction between two factors (e.g. treated for paired-end data vs untreated for single-end), the DESeq2 software should be run for another time using only one factor with the following 4 levels:
  - treated-PE
  - untreated-PE
  - treated-SE
  - untreated-SE
```
Let's now move to the annotation workk!! 🥳🥳

## Extraction and annotation of differentially expressed genes

In this part, we will show you the steps to extract the most differentially expressed genes in your dataset according to the applied treatment and with an absolute **fold change of (> 1).**

**First**, the ```Filter``` *data on any column using simple expressions Tool* is used to extract genes with a significant change in gene expression between treated and untreated samples. The p-value is agjusted to be below 0.05. The other settings can be checked here as well. 

![filter](https://user-images.githubusercontent.com/67822771/130277804-eb6fb091-43ec-4546-8bd1-dce4318bea24.PNG)

> For the results, We got **1,091** genes (6.21% of the total) with a significant change in gene expression between treated and untreated samples. 

![filter results](https://user-images.githubusercontent.com/67822771/130277973-4c00ecb5-c821-4f7e-8ed1-5cdf46184216.PNG)

**Second**, The ```Filter``` tool is used agaun to only select the genes with an absolute fold change (FC) > 1. The adjusted settings are shown in the following figure.

![filter2](https://user-images.githubusercontent.com/67822771/130278516-2648f647-9a10-4e77-a60f-8a34586e6241.PNG)

> For the results, only **130** genes (11.92% of the significantly differentially expressed genes) were conserved.

![genes with significant](https://user-images.githubusercontent.com/67822771/130278280-b74dec55-b97a-410d-9324-de63cc815b45.PNG)

**Third**, Currently, we have a table showing the most differentially expressed genes with their IDs, mean normalized counts (averaged over all samples from both conditions), log2 fold change and other information. The genes are presented usign their IDs from the corresponding database (e.g FBgn0003360). However, it is prefered to get the gene names and their location in the genome. These information can be obtained from the annotation file used for the mapping and counting steps. Accordingly, the Ensembl gene annotation for Drosophila melanogaster([Drosophila_melanogaster.BDGP6.87.gtf](https://zenodo.org/record/4541751/files/Drosophila_melanogaster.BDGP6.87.gtf)) was imported.

![gtv](https://user-images.githubusercontent.com/67822771/130279033-da021a78-fded-4f3a-9d2c-c84660294974.PNG)

**Fourth**, the ```Annotate DESeq2/DEXSeq output tables``` tool was used for this purpose with the following adjustments. 

![annotation](https://user-images.githubusercontent.com/67822771/130279367-309035f8-290a-4f6c-b045-976e1e67c7e4.PNG)

```
The new output file has the following details added:

- Gene identifiers
- Mean normalized counts over all samples
- Log2 fold change
- Standard error estimate for the log2 fold change estimate
- Wald statistic
- p-value for the Wald statistic
- p-value adjusted for multiple testing with the Benjamini-Hochberg procedure for the Wald statistic
- Chromosome
- Start
- End
- Strand
- Feature
- Gene name
```
![annotation results](https://user-images.githubusercontent.com/67822771/130279395-7edd4cda-ce0e-4d1e-afe6-a5b5fd992733.PNG)

**Fifth** However, the generated file doesn't have a header to specify each column. Accordingly, we created a file named *Pasted Entry* of type *tabular* with the following shown data.
![pasted entry](https://user-images.githubusercontent.com/67822771/130279445-f01640f1-106f-40a2-84d2-d1637090c0a1.PNG)

Afterwards, the ```Concatenate datasets ``` tool was used to obtain the final annotation file as shown here.

![final final](https://user-images.githubusercontent.com/67822771/130279479-b3859c0e-c493-4306-96c4-db3a592e014d.PNG)

Well Done!! You have finished the differential gene expression analysis of your data. Hope you enjoyed it! 🤗

# Visualization of Differential gene expression

**` A: `**   After adjusting p values and annotation of genes, the most differentially expressed genes were selected. They were joined by using “Join two datasets” which used the input as normalized counts generated by DESeq2. The cut tool is used to extract the columns with the gene IDs and normalized counts. The differentially expressed genes are represented in terms of log<sub>2</sub>FC(fold-change) by DESeq2.(how many times the change in expression has occurred). Then, heatmap was generated using heatmap2.
## 1. Visualisation of the normalised counts
*To extract the normalized counts for the interesting genes, we join the normalized count table generated by DESeq2 with the table we just generated.*

 ## `👉 To plot the heatmap`
|Steps||
|:-----:|---|
|  a.| param-file |
|  b.| :file_folder: Input should have column headers”: `Normalized counts for the most differentially expressed genes` |
|  c.| Advanced - log transformation”/“Data transformation”: `Log2(value) transform my data` |
|  d.| Enable data clustering”: `Yes`|
|  e.| Labeling columns and rows”: `Label columns and not rows` |
|  f.| Coloring groups”: `Blue to white to red`|

<h1 align="center">

![6](https://user-images.githubusercontent.com/57266535/130248627-46327f99-9921-469e-b3a5-a7d19f737efb.png)
</h1>

**` B: `** Another statistical measure is to calculate the z score and represent expression in terms of deviation from mean. This is done using table compute function for normalized counts; which says how far from mean values our actual readings are (in terms of standard deviations). Note that +-3SD has around 99.6% of data. Lower expressed (downregulated) genes will have negative SD, and upregulated will have positive SD. Our samples will show a plus or minus representation. Another heatmap is plotted to see how individual samples fare.

## `👉 Table Compute` with the following parameters to first subtract the mean values per row
- After adjusting p values and annotation of genes, the most differentially expressed genes were selected. They were joined by using “Join two datasets” which used the input as normalized counts generated by DESeq2. The cut tool is used to extract the columns with the gene IDs and normalized counts. The differentially expressed genes are represented in terms of log<sub>2</sub>FC(fold-change) by DESeq2.(how many times the change in expression has occurred). Then, heatmap was generated using heatmap2.

## `👉 To plot the heatmap`

 - "Input Single or Multiple Tables”: `Single Table`

|Steps||
|:-----:|---|
|a.| param-file| 
|b.| Input should have column headers”: `Normalized counts for the most differentially expressed genes`|
|c.| Advanced - log transformation”/“Data transformation”: `Log2(value) transform my data`|
|d.| Enable data clustering”: `Yes`|
|e.| Labeling columns and rows”: `Label columns and not rows` |
|f.| Coloring groups”: `Blue to white to red` |

### `👉 Result`

|Steps||
|:-----:|---|
|a.|param-file|
|b.|“Table”: `Normalized counts for the most differentially expressed genes`|
|c.|“Type of table operation”: `Perform a full table operation`|
|d.|Custom expression on ‘table’, along ‘axis’ (0 or 1)”: `table.sub(table.mean(1), 0)`|
|e.|The table.mean(1) expression computes the mean for each row (here the genes) and table.sub(table.mean(1), 0) subtracts each value by the mean of the row (computed with table.mean(1))|

- Input Single or Multiple Tables”: Multiple Table

|Steps||
|:-----:|---|
|a.| param-file|
|b.|“Table1”: `Normalized counts for the most differentially expressed genes`|
|c.|Click on: “Insert Tables”|
|d.|“Table2”: `Table compute output`|
|e.| Custom expression on `‘table2.div(table1.std(1),0)‘`|
|f.| The table1.std(1) expression computes the standard deviations of each row on the 1st table (normalized counts) and table2.div divides the values of 2nd table (previously computed) by these standard deviations.|
|e.| Rename the output to Z-scores for the most differentially expressed genes|

## `👉 To plot the heatmap`

|Steps||
|:-----:|---|
|a.|“Input should have column headers”: `Z-scores for the most differentially expressed genes`|
|b.|“Advanced - log transformation”/“Data transformation”: `Plot the data as it is`|
|c.|“Enable data clustering”: `Yes`|
|d.|“Labeling columns and rows”: `Label columns and not rows`|
|e.|“Coloring groups”: `Blue to white to red`|

<h1 align="center">

![6](https://user-images.githubusercontent.com/57266535/130248627-46327f99-9921-469e-b3a5-a7d19f737efb.png) 
</h3>

The samples are clustering by treatment, all the treated csamples can be visualised togteher and untreated are together on the heatmap.


## 2. Computation and Visualisation of Z-score
*The Z-score gives the number of standard-deviations that a value is away from the mean of all the values in the same group, here the same gene.*

Another statistical measure is to calculate the z score and represent expression in terms of deviation from mean. This is done using table compute function for normalized counts; which says how far from mean values our actual readings are (in terms of standard deviations). Note that +-3SD has around 99.6% of data. Lower expressed (downregulated) genes will have negative SD, and upregulated will have positive SD. Our samples will show a plus or minus representation. Another heatmap is plotted to see how individual samples fare.

``` Table Compute Tool```
```
- with the following parameters to first subtract the mean values per row
- “Input Single or Multiple Tables”: Single Table
- param-file
- “Table”: Normalized counts for the most differentially expressed genes
- “Type of table operation”: Perform a full table operation
- Custom expression on ‘table’, along ‘axis’ (0 or 1)”: table.sub(table.mean(1), 0)
- The table.mean(1) expression computes the mean for each row (here the genes) and table.sub(table.mean(1), 0) subtracts each value by the mean of the row (computed with table.mean(1))

```
``` Table Compute Tool```
```
- “Input Single or Multiple Tables”: Multiple Table
- param-file
- “Table1”: Normalized counts for the most differentially expressed genes
- Click on: “Insert Tables”
- “Table2”: Table compute output
- Custom expression on ‘table2.div(table1.std(1),0)‘
- The table1.std(1) expression computes the standard deviations of each row on the 1st table (normalized counts) and table2.div divides the values of 2nd table (previously computed) by these standard deviations.
- Rename the output to Z-scores for the most differentially expressed genes
 ```
<h3> To plot the heatmap: </h3>
 ```
- “Input should have column headers”: Z-scores for the most differentially expressed genes
- “Advanced - log transformation”/“Data transformation”: Plot the data as it is
- “Enable data clustering”: Yes
- “Labeling columns and rows”: Label columns and not rows
- “Coloring groups”: Blue to white to red
```
 <h3> Result </h3>
<h3 align="center">
 
![7](https://user-images.githubusercontent.com/57266535/130248956-0a218b31-76e6-417f-8627-33a2577141b1.png)
</h1>





</h3>
The Z-score ranges from -3 standard deviations up to +3 standard deviations. When a gene is differentially expressed between two groups (here treated and untreated), the Z-scores for this gene will be (mostly) positive for the samples in one group and (mostly) negative for the samples in the other group. 
<h1>
 Team Contributors
 </h1>
   
  
  
  Steps | Names
------------ | ------------- 
Identification of the differentially expressed features | Utkarsha, Osama, Johny, Nikita, Rana, Yasmeen | 
Extraction and annotation of differentially expressed genes | Rana, Jaspreet, Chigozie, Pankaj, Osama, Nikita|
Visualization of the normalized counts  | Utkarsha, Dawoud, Tosin, Ankita, Rana, Osama | 
Computation and visualization of the Z-score | Amira, Saket, Rana, Tosin, diyar | 
Markdown Documentation | Utkarsha, Rana, Jaspreet, Osama |
