---
title: "My Example R Markdown"
author: "G.C.O"
date: "`r Sys.Date()`"
output:
  html_document:
    toc: true
    toc_depth: 3
    toc_float: 
      collapsed: false
---

``` {r}
#| fake R chunks for a set of reminder notes about markdown, echo = FALSE, 
#| results='hide', warning=FALSE

#Creating an R markdown document

#clearing the environment

#rm(list=ls())

#Installing the packages
#install.packages("rmarkdown")
#library(rmarkdown)

#You can start a basic R markdown document by going to file > new file > new R markdown document.

#You will NEED to go the R markdown file "/Users/guillaumeonyeaghala/Downloads/R_43_Example_RMarkdown_basic.Rmd" to follow along

#Once you are done with your R markdown package, you can click on the knit option within the R markdown window to generate the R markdown file
#HTML files are easier to generate because PDF files require additional packages to run in markdown. 

###When you are in a markdown document, you can use the run command rather than the knit command to run the code directly within R studio. the knit option will compile things into a finalized document

#Here is how you start a chunk of r code in the markdown file. In between the ` lines, the code will behave exactly as if it was running within base R or R studio 
#```{r summary, echo=FALSE, results = 'hide', warning = FALSE, message = FALSE)}
#R chunk options
#You can control how R code is displayed and how the output appears by adding chunk options within the curly brackets after the first set of backticks. Chunks can also be given names, which is useful for troubleshooting or naming figures that are saved in a chunk of R code.

#```{r summary, echo=FALSE}
#now you are writing in R
#summary(cars)
#```

#``` {r}
##| fake R chunks for a set of reminder notes about markdown, echo = FALSE, 
##| results='hide', warning=FALSE

#In the above code "summary" is the name of the chunk, and we are setting the chunk option "echo" to be FALSE. 

#here are a few common ones:

#To hide the R code itself, but not the R code output, set (echo = FALSE)
#To hide the R code output, but not the R code itself, set (results = 'hide')
#To hide various messages R will throw at you (warning = FALSE, message = FALSE)
#To adjust the figure size/placement (fig.align = 'center', out.height='80%')

#For example if we pick all these options you will not the code, results or warning messages. #It might be useful do set this for bits of code you are using when learning Rcode sections within a project.
#```{r summary, echo=FALSE, results = 'hide', warning = FALSE, message = FALSE)}
#now you are writing in R
#summary(cars)
#```

#```{r summary, echo=FALSE, results = 'hide', warning = FALSE, message = FALSE)}
#You can set global options for R (for example hiding all the R code if you are generating an html file for a collaborator with only the results )
#The knitr package is part of R markdown so you can directly reference the command

#```{r}
#knitr::opts_chunk$set(echo=FALSE)
#```

#```{r summary, echo=FALSE, results = 'hide', warning = FALSE, message = FALSE)}
#You can also hide messages, warning and errors here !!! However try to avoid hiding errors as these are usually important.

#```{r}
#knitr::opts_chunk$set(echo=FALSE, message=FALSE, warning=FALSE, error=FALSE)
#```

#```{r summary, echo=FALSE, results = 'hide', warning = FALSE, message = FALSE)}
#Once you set a global option, you can then activate individual R chunks by doing

#```{r, echo=TRUE}
#Your R code can go here
#```

#```{r summary, echo=FALSE, results = 'hide', warning = FALSE, message = FALSE)}
#By default, your R markdown file will display ## next you each result line. You can modify it as shown below using the comment option, in this case we are setting it as a blank value

#```{r, comment = ""}
#Your R code can go here
#```

#```{r summary, echo=FALSE, results = 'hide', warning = FALSE, message = FALSE)}
#You can inline R code to reference specific information so that it autoupdates as shown below using a single backtick followed by R

#the data was from  `r unique(atus$year)` and contained `r nrow(atus)` respondents. the average age of respondents was `r mean(atus$age, na.rm=TRUE)` years, 
#(SD = `r sd(atus$year)`, range `r min(atus$age)` - `r max(atus$age)`) 

#If you need to tweak the number of significant figures, you can do so with the round option `r round(mean(atus$age, na.rm=TRUE),2)`


#Session information this is useful to keep track of packages

#```{r}
#sessionInfo()
#```

#Clearing the session and loading the necessary packages

#```{r}

#rm(list=ls())

#``` 

#Installing the packages
#install.packages("rmarkdown")
#library(rmarkdown)

#Here are some useful links I have found about hiding chunk & text from your markdown file
#https://stackoverflow.com/questions/59429319/r-markdown-html-output-hide-a-section-of-the-document-of-an-html-rmarkdown-out
#https://bookdown.org/yihui/rmarkdown-cookbook/hide-one.html
#https://zsmith27.github.io/rmarkdown_crash-course/lesson-5-code-chunks-and-inline-code.html

```

# Restarting the analysis in earnest


``` {r}
#| analysis notes 1, echo = FALSE, 
#| results='hide', warning=FALSE

#Learning how to use MAASLIN for microbiome analysis (MOSIO and Shotgun data ATC2024)

#https://huttenhower.sph.harvard.edu/maaslin/
#https://github.com/biobakery/Maaslin2
#https://github.com/biobakery/biobakery/wiki/maaslin2

#https://github.com/biobakery/biobakery/wiki/maaslin2
#I am downloading the demo folder from "https://github.com/biobakery/Maaslin2?tab=readme-ov-file" so that I can take a look at how the datasets are formated (found in /Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_C_01_Programs_01_MISSION _16S_Programs_MAR2023/02_16S_Program 98_Maaslin2-master/inst/extdata). However I will be installing MAASLIN through R.

#2. Installing MaAsLin 2
#2.1 With Bioconductor

#You might encounter an error message that states:

#Warning message: package ‘Maaslin2’ is not available (for R version x.x.x) 
#This can be due to one of two reasons:

#Your R version is too old. Check this with sessionInfo() - if it is older than 3.6, download #the most updated version per instructions above.

#You have an older version of Bioconductor. MaAsLin2 requires Bioconductor >= 3.10. You can #check your Bioconductor version with BiocManager::version(). If the returned version is #anything older than 3.10, you can update with

#BiocManager::install(version = "3.10")

#And then

#BiocManager::install("Maaslin2")

#Once you have the correct R version, you can install MaAsLin 2 with Bioconductor:

#if(!requireNamespace("BiocManager", quietly = TRUE))
#  install.packages("BiocManager")
#BiocManager::install("Maaslin2")

#3. Microbiome Association Detection with MaAsLin 2
#3.1 MaAsLin 2 Input
#Set up: Launch a fresh R session and in the R script window:

#getwd()
#setwd("~/Desktop")
#dir.create("R_Maaslin_tutorial") # Create a new directory
#setwd("R_Maaslin_tutorial") # Change the current working directory 
#getwd() #check if directory has been successfully changed

#Load MaAsLin 2 package into the R environment

#library(Maaslin2)
#?Maaslin2
```

#Loading the Maaslin2 package

```{r}
if(!requireNamespace("BiocManager"))
  install.packages("BiocManager")
BiocManager::install("Maaslin2")
```

```{r}
#Load MaAsLin 2 package into the R environment

library(Maaslin2)
?Maaslin2

```

``` {r}
#| analysis notes 2, echo = FALSE, 
#| results='hide', warning=FALSE

#MaAsLin 2 requires two input files, one for taxonomic or functional feature abundances, and one for sample metadata.

#Data (or features) file
#This file is tab-delimited.
#Formatted with features as columns and samples as rows.
#The transpose of this format is also okay.
#Possible features in this file include microbes, genes, pathways, etc.

#Metadata file
#This file is tab-delimited.
#Formatted with features as columns and samples as rows.
#The transpose of this format is also okay.

#The data file can contain samples not included in the metadata file (along with the reverse case). For both cases, those samples not included in both files will be removed from the analysis. Also the samples do not need to be in the same order in the two files.

#Example input files can be found in the inst/extdata folder of the MaAsLin 2 source. 
#I am downloading the demo folder from "https://github.com/biobakery/Maaslin2?tab=readme-ov-file" so that I can take a look at how the datasets are formated (found in /Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_C_01_Programs_01_MISSION _16S_Programs_MAR2023/02_16S_Program 98_Maaslin2-master/inst/extdata). However I will be installing MAASLIN through R.

```

``` {r}
#Example input files can be found in the inst/extdata folder of the MaAsLin 2 source. The files provided were generated from the HMP2 data which can be downloaded from https://ibdmdb.org/ .

#input_data = system.file("extdata", "HMP2_taxonomy.tsv", package="Maaslin2") 
# The abundance table file: input_data

#input_metadata = system.file("extdata", "HMP2_metadata.tsv", package="Maaslin2") 
# The metadata table file: input_metadata

#get the pathway (functional) data - place holder
#download.file("https://raw.githubusercontent.com/biobakery/biobakery_workflows/master/examples/tutorial/stats_vis/input/pathabundance_relab.tsv", "./pathabundance_relab.tsv")

#HMP2_taxonomy.tsv: is a tab-demilited file with species as columns and samples as rows. It #is a subset of the taxonomy file so it just includes the species abundances for all samples.

#HMP2_metadata.tsv: is a tab-delimited file with samples as rows and metadata as columns. It #is a subset of the metadata file so that it just includes some of the fields.

#pathabundance_relab.tsv: is a tab-delimited file with samples as columns and pathway level #features as rows. It is a subset of the pathway file so it just includes the species #abundances for all samples.

```

``` {r}
#| analysis notes 3, echo = FALSE, 
#| results='hide', warning=FALSE

#Saving inputs as data frames

#df_input_data = read.table(file             = input_data,
#                           header           = TRUE,
#                           sep              = "\t", 
#                           row.names        = 1,
#                           stringsAsFactors = FALSE)

#df_input_data[1:5, 1:5]

#df_input_metadata = read.table(file             = input_metadata, 
#                               header           = TRUE, 
#                               sep              = "\t", 
#                               row.names        = 1,
#                               stringsAsFactors = FALSE)

#df_input_metadata[1:5, ]

#df_input_path = read.csv("./pathabundance_relab.tsv", 
#                         sep              = "\t", 
#                         stringsAsFactors = FALSE, 
#                         row.names        = 1)
#df_input_path[1:5, 1:5]
```

#Installing Packages

```{r}
if(!requireNamespace("BiocManager"))
  install.packages("BiocManager")
BiocManager::install("tidyverse")
BiocManager::install("vegan")
BiocManager::install("lefser")
BiocManager::install("semtree")
```

#Loading Packages
```{r}
#I will try using read tsv instead

library(tidyverse)
library(vegan)
library(readxl)
library(dplyr)
library(ggplot2)
library(pheatmap)
library(lme4)
library(geepack)
library(knitr)
library(kableExtra)
library(ggpubr)
library(stringr)
library(lefser)
library(semtree)

```

#Loading test files

```{r}
#df_input_data = read_tsv("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_C_01_Programs_01_MISSION _16S_Programs_MAR2023/02_16S_Program 98_Maaslin2-master/inst/extdata/HMP2_taxonomy.tsv")
df_input_data = read_tsv("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_4_01_Programs_01_Python Coding/Bioinf_2_Resources_4_01_Programs_01_MISSION _16S_Programs_MAR2023/02_16S_Program 98_Maaslin2-master/inst/extdata/HMP2_taxonomy.tsv")
df_input_data[1:5, 1:5]

#df_input_metadata = read_tsv("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_C_01_Programs_01_MISSION _16S_Programs_MAR2023/02_16S_Program 98_Maaslin2-master/inst/extdata/HMP2_metadata.tsv")
df_input_metadata = read_tsv("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_4_01_Programs_01_Python Coding/Bioinf_2_Resources_4_01_Programs_01_MISSION _16S_Programs_MAR2023/02_16S_Program 98_Maaslin2-master/inst/extdata/HMP2_metadata.tsv")
df_input_metadata[1:5, 1:5]

```

#Changing the file type to a data frame
```{r}
#https://www.rdocumentation.org/packages/utils/versions/3.6.2/topics/read.table
#https://www.statology.org/r-read-table/

#df_input_data = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_C_01_Programs_01_MISSION _16S_Programs_MAR2023/02_16S_Program 98_Maaslin2-master/inst/extdata/HMP2_taxonomy.tsv', header = TRUE, sep = "\t", row.names = 1)
df_input_data = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_4_01_Programs_01_Python Coding/Bioinf_2_Resources_4_01_Programs_01_MISSION _16S_Programs_MAR2023/02_16S_Program 98_Maaslin2-master/inst/extdata/HMP2_taxonomy.tsv', header = TRUE, sep = "\t", row.names = 1)
df_input_data[1:5, 1:5]

#How to check the data type in r
#https://libraryguides.mcgill.ca/c.php?g=699776&p=4968544#:~:text=To%20determine%20the%20type%20of,integer()%2C%20as.
#https://www.projectpro.io/recipes/check-data-type-r
#https://www.rforecology.com/post/data-types-in-r/
str(df_input_data)

#https://www.rdocumentation.org/packages/utils/versions/3.6.2/topics/read.table
#https://www.statology.org/r-read-table/

#df_input_metadata = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_C_01_Programs_01_MISSION _16S_Programs_MAR2023/02_16S_Program 98_Maaslin2-master/inst/extdata/HMP2_metadata.tsv', header = TRUE, sep = "\t", row.names = 1)
df_input_metadata = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_4_01_Programs_01_Python Coding/Bioinf_2_Resources_4_01_Programs_01_MISSION _16S_Programs_MAR2023/02_16S_Program 98_Maaslin2-master/inst/extdata/HMP2_metadata.tsv', header = TRUE, sep = "\t", row.names = 1)
df_input_metadata[1:5, 1:5]


```

``` {r}
#| analysis notes 4, echo = FALSE, 
#| results='hide', warning=FALSE

#3.2 Running MaAsLin 2

#Several different types of statistical models can be used for association testing in MaAsLin (e.g. simple linear, zero-inflated, count-based, etc.). We have evaluated their pros and cons in the associated manuscript, and while the default is generally appropriate for most analyses, a user might want to select a different model under some circumstances. This is true for many different microbial community data types (taxonomy or functional profiles), environments (human or otherwise), and measurements (counts or relative proportions), as long as alternative models are used with appropriately modified normalization/transformation schemes.

#For models, if your inputs are counts, then you can use NEGBIN and ZINB, whereas, for non-count (e.g. percentage, CPM, or relative abundance) input, you can use LM and CPLM.
#Among the normalization approaches implemented in MaAsLin 2, TMM and CSS only work on counts, and they also return normalized counts, unlike TSS and CLR. Therefore, if your inputs are counts, you can use the above two normalizations (i.e., TMM, CSS, or NONE (in case the data is already normalized)) without a further transformation (i.e. transform = NONE).
#Among the non-count models, CPLM requires the data to be positive. Therefore, any transformation that produces negative values will typically NOT work for CPLM.
#All the non-LM models use an intrinsic log link transformation due to their close connection to GLMs and they are recommended to be run with transform = NONE.
#Apart from that, LM is the only model that works on both positive and negative values (following normalization/transformation), and (as per our manuscript), it is generally much more robust to parameter changes (which are typically limited for non-LM models). Regarding whether to use LM, CPLM, or other models, intuitively, CPLM or a zero-inflated alternative should perform better in the presence of zeroes, but based on our benchmarking, we do not have evidence that CPLM is significantly better than LM in practice.

#Model (-m ANALYSIS_METHOD)	Data type	Normalization (-n NORMALIZATION)	Transformation (-t TRANSFORM)
#LM	count and non-count	TSS,CLR, NONE	LOG, LOGIT, AST,NONE
#CPLM	count and non-count	TSS, TMM, CSS, NONE	NONE
#NEGBIN	count	TMM, CSS, NONE	NONE
#ZINB	count	TMM, CSS, NONE	NONE
#NOTE: generally you will also need to consider what thresholds you have used to filter the data (i.e. the prevalence and abundance threshold) to reduce outliers and false positives. See section 4.3.1 Prevalence and abundance filtering in MaAsLin 2 for some more details on this.

#The following command runs MaAsLin 2 on the HMP2 data, running a multivariable regression model to test for the association between microbial species abundance versus IBD diagnosis and dysbiosis scores (fixed_effects = c("diagnosis", "dysbiosis")). For any categorical variable with more than 2 levels, you will also have to specify which variable should be the reference level by using (reference = c("diagnosis,nonIBD")). NOTE: adding a space between the variable and level might result in the wrong reference level being used. Output are generated in a folder called demo_output under the current working directory (output = "demo_output"). In this case, the example input data has been pre-normalized and pre-filtered, so we turn off the default normalization and prevalence filtering as well.

```

#Setting up the location of the output files and running the test 

```{r}
# I need to figure out where R will spit out my results

getwd()
setwd("~/Desktop")

#Providing data frames as input
#If you are familiar with the data frame class in R, you can also provide data frames for input_data and input_metadata for MaAsLin 2, instead of file names. One potential benefit of this approach is that it allows you to easily manipulate these input data within the R environment.

fit_data2 = Maaslin2(input_data     = df_input_data, 
                     input_metadata = df_input_metadata, 
                     min_prevalence = 0,
                     normalization  = "NONE",
                     output         = "demo_output2", 
                     fixed_effects  = c("diagnosis", "dysbiosis"),
                     reference      = c("diagnosis,nonIBD"))

```

# Next steps: Adjusting the methodology for the underlying model;
https://forum.biobakery.org/t/error-running-maaslin2/127/8
https://forum.biobakery.org/t/maaslin-input-error/3323/4
https://forum.biobakery.org/t/maaslin2-glm-fit-error/2571/6
https://stats.stackexchange.com/questions/164457/r-glmer-warnings-model-fails-to-converge-model-is-nearly-unidentifiable
https://github.com/biobakery/biobakery/wiki/maaslin2
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8714082/

# Repeating the MPAEHR analysis stratified by cohort before combining them
``` {r}
getwd()
setwd("~/Desktop")
library(tidyverse)
library(vegan)
library(Maaslin2)
?Maaslin2
```

# Run MAASLIN2 models
``` {r}

getwd()
setwd("~/Desktop")

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_5_12_AUC", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_5_12_AUC_diet", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"))                            

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_EHR40perc", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_EHR40perc_diet", 
                            fixed_effects  = c("EHR_class_40perc","Dim_1","Dim_2"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_EHRtertv2", 
                            fixed_effects  = c("EHRtertv2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_EHRtertv2_diet", 
                            fixed_effects  = c("EHRtertv2","Dim_1","Dim_2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_EHRtertv2binary", 
                            fixed_effects  = c("EHRtertv2binary"),
                            reference = c("EHRtertv2binary,0"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_EHRtertv2binary_diet", 
                            fixed_effects  = c("EHRtertv2binary","Dim_1","Dim_2"),
                            reference = c("EHRtertv2binary,0"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl_diet", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_5_12_mpag", 
                            fixed_effects  = c("MPAG_AUC_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_5_12_mpag_diet", 
                            fixed_effects  = c("MPAG_AUC_0_12","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_5_12_ratio", 
                            fixed_effects  = c("MPA_MPAG_ratio"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_PS_5_12_ratio_diet", 
                            fixed_effects  = c("MPA_MPAG_ratio","Dim_1","Dim_2"))  

```

# Prospective arm: Loading up microbiome data

``` {r}
df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D11_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

#Using the new pcs from the expanded diet data

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_PS.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_crcl_PS.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

```

#Cross-sectional arm: Loading up microbiome data

```{r}

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D11_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D11_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

#Using the new pcs from the expanded diet data

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_CS.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_crcl_CS.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everthing else (smart to do so for each arm)
#https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

```

#Run MAASLIN2 models

```{r}

getwd()
setwd("~/Desktop")

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_5_12_AUC", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_5_12_AUC_diet", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"))                            

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_EHR40perc", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_EHR40perc_diet", 
                            fixed_effects  = c("EHR_class_40perc","Dim_1","Dim_2"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_EHRtertv2", 
                            fixed_effects  = c("EHRtertv2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_EHRtertv2_diet", 
                            fixed_effects  = c("EHRtertv2","Dim_1","Dim_2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_EHRtertv2binary", 
                            fixed_effects  = c("EHRtertv2binary"),
                            reference = c("EHRtertv2binary,0"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_EHRtertv2binary_diet", 
                            fixed_effects  = c("EHRtertv2binary","Dim_1","Dim_2"),
                            reference = c("EHRtertv2binary,0"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_CS_5_12_crcl", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_CS_5_12_crcl_diet", 
 #                           fixed_effects  = c("MPA_renal_cl_L_hr","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_5_12_mpag", 
                            fixed_effects  = c("MPAG_AUC_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_5_12_mpag_diet", 
                            fixed_effects  = c("MPAG_AUC_0_12","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_5_12_ratio", 
                            fixed_effects  = c("MPA_MPAG_ratio"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CS_5_12_ratio_diet", 
                            fixed_effects  = c("MPA_MPAG_ratio","Dim_1","Dim_2"))                 

```

#Full MISSION cohort

```{r}
df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D11_merged_abundance_table_kneaddata_species_prep_APR2024_V2.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

#Using the new pcs from the expanded diet data

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_crcl.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everthing else (smart to do so for each arm)
#https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

```

# Run MAASLIN2 Models

```{r}

getwd()
setwd("~/Desktop")

#Run models

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_ALL_5_12_AUC", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","Cohort"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_ALL_5_12_AUC_diet", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2","Cohort"))                            

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_ALL_EHRtertv2", 
                            fixed_effects  = c("EHRtertv2","Cohort"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_ALL_EHRtertv2_diet", 
                            fixed_effects  = c("EHRtertv2","Dim_1","Dim_2","Cohort"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_ALL_EHRtertv2binary", 
                            fixed_effects  = c("EHRtertv2binary","Cohort"),
                            reference = c("EHRtertv2binary,0"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_ALL_EHRtertv2binary_diet", 
                            fixed_effects  = c("EHRtertv2binary","Dim_1","Dim_2","Cohort"),
                            reference = c("EHRtertv2binary,0"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_ALL_5_12_mpag", 
                            fixed_effects  = c("MPAG_AUC_0_12","Cohort"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_ALL_5_12_mpag_diet", 
                            fixed_effects  = c("MPAG_AUC_0_12","Dim_1","Dim_2","Cohort"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_ALL_5_12_ratio", 
                            fixed_effects  = c("MPA_MPAG_ratio","Cohort"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_ALL_5_12_ratio_diet", 
                            fixed_effects  = c("MPA_MPAG_ratio","Dim_1","Dim_2","Cohort")) 
```

# Testing out dplyr to create a summary table for the poster

```{r}
#clear the environment
rm(list = ls())

#installing packages
install.packages("dplyr")
install.packages("tidyr")
install.packages("kableExtra")

#Installing the labelled package to edit the summary statistics
#https://stackoverflow.com/questions/76341658/how-to-add-column-labels-when-piping-in-r-with-dplyr-and-labelled
install.packages("labelled")

```

# Load the packages

```{r}
#Load packages

library(dplyr)
library(tidyr)
library(kableExtra)
library(labelled) 

```

#Using the Full MISSION cohort data for the table
```{r}

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_crcl.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12
```

#Summarizing the continuous data

```{r}

summary(df_input_metadata_mission) #Check the data type for all the variable we want to combine

#Summarising the variables of interest
#https://dplyr.tidyverse.org/reference/summarise.html

##Build the summaries from dplyr (chaining command)

dattab <-
df_input_metadata_mission %>%
  dplyr::select(Cohort, age_at_PK, gender, predom_race, eGFR_Epi_RF_Cr2021, total_bilirubin_mg_dL, albumin_g_dL, MMF_tot_daily_dose_mg, MPA_AUC_0_12, MPA_AUC5_12_0_12) %>%
  labelled::set_variable_labels(age_at_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_Epi_RF_Cr2021 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MMF_tot_daily_dose_mg = "MMF daily dose", MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", 
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)")  %>% 
  dplyr::group_by(Cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_age = mean(age_at_PK),
                   sd_age = sd(age_at_PK),
                   mean_eGFR = mean(eGFR_Epi_RF_Cr2021),
                   sd_eGFR = sd(eGFR_Epi_RF_Cr2021),
                   mean_bilirubin = mean(total_bilirubin_mg_dL),
                   sd_bilirubin = sd(total_bilirubin_mg_dL),
                   mean_albumin = mean(albumin_g_dL),
                   sd_albumin = sd(albumin_g_dL),
                   mean_MMF = mean(MMF_tot_daily_dose_mg),
                   sd_MMF = sd(MMF_tot_daily_dose_mg),
                   mean_AUC = mean(MPA_AUC_0_12),
                   sd_AUC = sd(MPA_AUC_0_12),
                   mean_EHR = mean(MPA_AUC5_12_0_12),
                   sd_EHR = sd(MPA_AUC5_12_0_12))# %>%  
#Reshape the summarize output so that the case and control columns are side by side  
#  pivot_wider (names_from = Cohort,
#               values_from = c(mean_age, sd_age, mean_eGFR, sd_eGFR, mean_bilirubin, sd_bilirubin,
#                               mean_albumin, sd_albumin, mean_MMF, sd_MMF, mean_AUC, sd_AUC, mean_EHR, sd_EHR)) %>% #You #have to use the concatenate option here so that the values appear side by side, not staggered.
#  dplyr::select(variable, value, count_Case, percent_Case, count_Control, percent_Control) %>% #You can use the select #command in dplyr to force a certain order for your chosen variables
#  dplyr::mutate(variable = factor(variable, levels = c("AgeYear", "sex", "grade"))) %>% #Here we are forcing R to #display the row within the variable "variable" in a specific order
#  dplyr::arrange(variable)

dattab_check <-
df_input_metadata_mission %>%
  dplyr::select(Cohort, age_at_PK, gender, predom_race, eGFR_Epi_RF_Cr2021, total_bilirubin_mg_dL, albumin_g_dL, MMF_tot_daily_dose_mg, MPA_AUC_0_12, MPA_AUC5_12_0_12) %>%
  labelled::set_variable_labels(age_at_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_Epi_RF_Cr2021 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MMF_tot_daily_dose_mg = "MMF daily dose", MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", 
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)")  %>% 
  #dplyr::group_by(Cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_age = mean(age_at_PK),
                   sd_age = sd(age_at_PK),
                   mean_eGFR = mean(eGFR_Epi_RF_Cr2021),
                   sd_eGFR = sd(eGFR_Epi_RF_Cr2021),
                   mean_bilirubin = mean(total_bilirubin_mg_dL),
                   sd_bilirubin = sd(total_bilirubin_mg_dL),
                   mean_albumin = mean(albumin_g_dL),
                   sd_albumin = sd(albumin_g_dL),
                   mean_MMF = mean(MMF_tot_daily_dose_mg),
                   sd_MMF = sd(MMF_tot_daily_dose_mg),
                   mean_AUC = mean(MPA_AUC_0_12),
                   sd_AUC = sd(MPA_AUC_0_12),
                   mean_EHR = mean(MPA_AUC5_12_0_12),
                   sd_EHR = sd(MPA_AUC5_12_0_12))
```

#Summarizing the categorical data (https://stackoverflow.com/questions/34587317/using-dplyr-to-create-summary-proportion-table-with-several-categorical-factor-v)

```{r}
dattab2 <-
df_input_metadata_mission %>%
  dplyr::select(Cohort, age_at_PK, gender, predom_race, eGFR_ml_min_1_73m2, total_bilirubin_mg_dL, albumin_g_dL, MMF_tot_daily_dose_mg, MPA_AUC_0_12, MPA_AUC5_12_0_12) %>%
  labelled::set_variable_labels(age_at_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_ml_min_1_73m2 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MMF_tot_daily_dose_mg = "MMF daily dose", MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", 
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)")  %>% 
  dplyr::group_by(Cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::count(gender) %>%
  dplyr::mutate(freq = n / sum(n))

dattab3 <-
df_input_metadata_mission %>%
  dplyr::select(Cohort, age_at_PK, gender, predom_race, eGFR_ml_min_1_73m2, total_bilirubin_mg_dL, albumin_g_dL, MMF_tot_daily_dose_mg, MPA_AUC_0_12, MPA_AUC5_12_0_12) %>%
  labelled::set_variable_labels(age_at_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_ml_min_1_73m2 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MMF_tot_daily_dose_mg = "MMF daily dose", MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", 
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)")  %>% 
  dplyr::group_by(Cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::count(predom_race) %>%
  dplyr::mutate(freq = n / sum(n))

```

# Testing out code from "/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/R_23_Creating tables with dplyr.R" to make a table

```{r}
#setworking directory
setwd("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/")

#read in the data

dat <- read.csv("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/R_23_Data_for_table.csv")

##Build the summaries from dplyr (chaining command)

summary(dat) #Check the data type for all the variable we want to combine

dattab <-
dat %>%
  dplyr::select(-age, -IQ) %>% # you can use the minus sign in front of a variable name to remove it from your dataset. We removed the continuous variables to tabulate the information on categorical variables first.
  dplyr::mutate(grade = as.character(grade)) %>%
  pivot_longer(sex:AgeYear,
               names_to = "variable",
               values_to = "value") %>% #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). the sex:AgeYear abbreviation lets us select every column between the tow without needing to type them out.
  dplyr::group_by(group, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(group, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100)%>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = group,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered.
  dplyr::select(variable, value, count_Case, percent_Case, count_Control, percent_Control) %>% #You can use the select command in dplyr to force a certain order for your chosen variables
  dplyr::mutate(variable = factor(variable, levels = c("AgeYear", "sex", "grade"))) %>% #Here we are forcing R to display the row within the variable "variable" in a specific order
  dplyr::arrange(variable)

##Beautify the table using the kableExtra package

dattab %>%
  kable(digits=2, col.names=c(" "," ", "count", "percent", "count", "percent")) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F)%>% #We can use preset table settings within R to set our table 
  pack_rows("Age (Years)", start_row = 1, end_row = 2) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 
  pack_rows("Sex", start_row = 3, end_row = 4) %>%
  pack_rows("Grade", start_row = 5, end_row = 7) %>%
  add_header_above(c(" "=2, "Case" = 2, "Control" = 2)) #Here we are adding a header column, and tricking R into keeping it blank for specific columns and filled in for others. (I did two blank columns as I did not delete the variable column as in the tutorial)  
  
#Add tests statistics in R to a table (chisquare and p value)

age.chisq <-
dattab %>%
  dplyr::ungroup() %>%
  dplyr::filter(variable == "AgeYear") %>%
  dplyr::select(count_Case, count_Control) %>%
  chisq.test()  #Since we are saving it as an object, we will be able to reference specific statistics later as seen below

age.chisq$statistic
age.chisq$p.value

sex.chisq <-
  dattab %>%
  dplyr::ungroup() %>%
  dplyr::filter(variable == "sex") %>%
  dplyr::select(count_Case, count_Control) %>%
  chisq.test()

grade.chisq <-
  dattab %>%
  dplyr::ungroup() %>%
  dplyr::filter(variable == "grade") %>%
  dplyr::select(count_Case, count_Control) %>%
  chisq.test()

#Make a single dataframe with all of your output for ease of use. This table can be merged with the table we have been working on in Kable, so we have to name the merging variable the same way

chisqstat <- data.frame(variable = c("AgeYear", "sex", "grade"),
                        stat = c(age.chisq$statistic, sex.chisq$statistic, grade.chisq$statistic),
                        pval = c(age.chisq$p.value,  sex.chisq$p.value , grade.chisq$p.value))

dattab %>%
  full_join (chisqstat, by="variable") %>% #This step will join our statistic values to the dattab table, using the full_join command to merge
  kable(digits=2, col.names=c(" "," ", "count", "percent", "count", "percent", "Chi-square", "p-value")) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F) %>% #We can use preset table settings within R to set our table 
  collapse_rows(columns = 7, valign= "middle") %>% #Here we are collapsing the columns with chisquare and pvlaue results as they repeat themselves
  pack_rows("Age (Years)", start_row = 1, end_row = 2) %>% #The packrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 
  pack_rows("Sex", start_row = 3, end_row = 4) %>%
  pack_rows("Grade", start_row = 5, end_row = 7) %>%
  add_header_above(c(" "=2, "Case" = 2, "Control" = 2, " "=2))

#getting R to manually edit some variables for us using the paste command

paste0("Case (n= ", length(which(dat$group=="Case")), ")")
paste0("Control (n= ", length(which(dat$group=="Control")), ")")

CaseHeader <- paste0("Case (n= ", length(which(dat$group=="Case")), ")")
ControlHeader <- paste0("Control (n= ", length(which(dat$group=="Control")), ")")

#Then you need to replicate the add_header_above section in a dataframe, which will replace the last line of code
headerinfo <- data.frame(headername = c(" ", CaseHeader, ControlHeader, " "),
                         colspan = c(2,2,2,2))

dattab %>%
  full_join (chisqstat, by="variable") %>% #This step will join our statistic values to the dattab table, using the full_join command to merge
  kable(digits=2, col.names=c(" "," ", "count", "percent", "count", "percent", "Chi-square", "p-value")) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F) %>% #We can use preset table settings within R to set our table 
  collapse_rows(columns = 7, valign= "middle") %>% #Here we are collapsing the columns with chisquare and pvlaue results as they repeat themselves
  pack_rows("Age (Years)", start_row = 1, end_row = 2) %>% #The packrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 
  pack_rows("Sex", start_row = 3, end_row = 4) %>%
  pack_rows("Grade", start_row = 5, end_row = 7) %>%
  add_header_above(headerinfo)

```

#Reapeating the coding for generating a descriptive table for the categorical variables
```{r}
#Loading the dataset
df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_crcl.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Build the summaries from dplyr (chaining command)

dattab_test <-
df_input_metadata_mission %>%
  dplyr::select(Cohort, gender, predom_race) %>% #Select only the categorical variables here
  tidyr::pivot_longer( cols = c("gender", "predom_race"),
               names_to = "variable",
               values_to = "value") %>% #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  dplyr::group_by(Cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(Cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) %>%
  #Reshape the summarize output so that the stratifed columns are side by side  
  pivot_wider (names_from = Cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered.
  dplyr::select(variable, value, count_PS, percent_PS, count_CS, percent_CS) %>% #You can use the select command in dplyr to force a certain order for your chosen variables
  
##Beautify the table using the kableExtra package

dattab_test %>%
  kable(digits=2, col.names=c(" "," ", "count", "percent", "count", "percent")) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F)%>% #We can use preset table settings within R to set our table 
  pack_rows("Sex", start_row = 1, end_row = 2) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 
  pack_rows("Ancestry", start_row = 3, end_row = 7) %>%
  add_header_above(c(" "=2, "Prospective Cohort" = 2, "Cross-sectional Cohort" = 2)) #Here we are adding a header column, and tricking R into keeping it blank for specific columns and filled in for others. (I did two blank columns as I did not delete the variable column as in the tutorial) 

#Add tests statistics in R to a table (chisquare and p value)

sex.chisq <-
dattab_test %>%
  dplyr::ungroup() %>%
  dplyr::filter(variable == "gender") %>%
  dplyr::select(count_PS, count_CS) %>%
  chisq.test()  #Since we are saving it as an object, we will be able to reference specific statistics later as seen below

sex.chisq$statistic
sex.chisq$p.value

#Also need to replace the NA values #https://tidyr.tidyverse.org/reference/replace_na.html
ancestry.chisq <-
  dattab_test %>%
  dplyr::ungroup() %>%
  dplyr::filter(variable == "predom_race") %>%
  tidyr::replace_na(list(count_PS = 0, percent_PS = 0)) %>%
  dplyr::select(count_PS, count_CS) %>%
  chisq.test()

ancestry.chisq$statistic
ancestry.chisq$p.value

#Make a single dataframe with all of your output for ease of use. This table can be merged with the table we have been working on in Kable, so we have to name the merging variable the same way

chisqstat <- data.frame(variable = c("gender", "predom_race"),
                        stat = c(sex.chisq$statistic, ancestry.chisq$statistic),
                        pval = c(sex.chisq$p.value,  ancestry.chisq$p.value))

dattab_test %>%
  full_join (chisqstat, by="variable") %>% #This step will join our statistic values to the dattab table, using the full_join command to merge
  kable(digits=2, col.names=c(" "," ", "count", "percent", "count", "percent", "Chi-square", "p-value")) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F) %>% #We can use preset table settings within R to set our table 
  collapse_rows(columns = 7, valign= "middle") %>% #Here we are collapsing the columns with chisquare and pvlaue results as they repeat themselves
#  collapse_rows(columns = 8, valign= "middle") %>% #Here we are collapsing the columns with chisquare and pvlaue results as they repeat themselves
  pack_rows("Sex", start_row = 1, end_row = 2) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 
  pack_rows("Ancestry", start_row = 3, end_row = 7) %>%
  add_header_above(c(" "=2, "Prospecitve Cohort" = 2, "Cross_sectional Cohort" = 2, " "=2)) #Here we are adding a header column, and tricking R into keeping it blank for specific columns and filled in for others. (I did two blank columns as I did not delete the variable column as in the tutorial) 

```

# Metatranscriptomics data prep

## df_input_metadata
``` {r}
#Merge the MPAEHR_metadata file with the metatranscriptomics mapping file

#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis
#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file

democlinical<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. I will rename the M4 to PK to ease the merging process

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
democlinical$Month[democlinical$Month=="M4"] <- "PK" 

#Import the mapping file
mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

freq <-table(mtxmapping$Month)
freq

#Filter it to only the PK observations

mtxmapping.pk<-mtxmapping%>%filter(grepl("PK|pk",Month))

#Now match the PIDs from mtxmapping.pk to those in the democlinical file

mtxmapping.pk<-mtxmapping.pk%>%filter(PID%in%democlinical$PID)
democlinical.pk<-democlinical%>%filter(PID%in%mtxmapping.pk$PID)

#Merging both datasets to make the demographic dataset for the metatranscriptomics analysis

mtxmapping.pk.all<-merge(mtxmapping.pk,democlinical.pk,by="PID") 

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metagx_id, .before=1)
mgxmapping.pk.all<-mgxmapping.pk.all%>%filter(!PID==4036)
mtxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_id, .before=1)
mtxpathmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_path_id, .before=1)

#Generate PS and CS subdatasets for further analyses

mgxmapping.pk.ps<-mgxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
mgxmapping.pk.cs<-mgxmapping.pk.all%>%filter(Cohort.x =="CS") #47

mtxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
mtxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort.x =="CS") #47

mtxpathmapping.pk.ps<-mtxpathmapping.pk.all%>%filter(Cohort.x =="PS")  #35
mtxpathmapping.pk.cs<-mtxpathmapping.pk.all%>%filter(Cohort.x =="CS") #47

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

write.table(mgxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

write.table(mgxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

write.table(mgxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

write.table(mtxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

write.table(mtxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

write.table(mtxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

write.table(mtxpathmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

write.table(mtxpathmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

write.table(mtxpathmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_all.txt", sep = "\t", row.names = FALSE)

```

## df_input_data (mtx_species)

``` {r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.species.xlsx",sheet = "Sheet1")

#Filter the dataset based on partial string using the stringr package
#https://stackoverflow.com/questions/59829668/filter-according-to-partial-match-of-string-variable-in-r

#mtxec4.reduced <- mtxec4 %>% filter(!str_detect(Gene_family, "UNGROUPED|UNMAPPED|NO_NAME"))

#Note that the subgene families use the "g_" string so you can use that to reduce the EC data to the major family. Same with the "unclassified term"
#mtxec4.reduced <- mtxec4 %>% filter(!str_detect(Gene_family, "UNGROUPED|UNMAPPED|NO_NAME|g_|unclassified"))

#Now transpose the data

gene_list<-unlist((mgxspecies[,1]))
sample_list<-unlist((mgxspecies[1,]))

gene_list<-unlist((mtxec4.reduced[,1]))
sample_list<-unlist((mtxec4.reduced[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mgx.all<-data.frame(t(mgxspecies))
#mtx.all<-data.frame(t(mtxec4.reduced))

#Trying to get rid of the colnames that snuck in the data
mgx.all <- mgx.all %>% filter(!str_detect(1, "clade_name"))
#mtx.all <- mtx.all %>% filter(!str_detect(1, "Gene_family"))

str(mgx.all)
names(mgx.all)
rownames(mgx.all)<-colnames(mgxspecies) 

#str(mtx.all)
#names(mtx.all)
#rownames(mtx.all)<-colnames(mtxec4.reduced) 

mgx.all<-mgx.all%>%filter(!grepl("s__Blautia_wexlerae",X1))
#mtx.all<-mtx.all%>%filter(!grepl("1.1.1.1: Alcohol_dehydrogenase",X1))

colnames(mgx.all)<-gene_list
names(mgx.all)

#colnames(mtx.all)<-gene_list
#names(mtx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

mgx.all$metagx_id <- rownames(mgx.all)
#mtx.all$metatx_id <- rownames(mtx.all)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgx.all<-mgx.all%>%relocate(metagx_id, .before=1)
#mtx.all<-mtx.all%>%relocate(metatx_id, .before=1)

#Create subset mtx files

mgx.pk<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.all$metagx_id)
mgx.ps<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.ps$metagx_id)
mgx.cs<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.cs$metagx_id)

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

write.table(mgx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_ps.txt", sep = "\t", row.names = FALSE)

write.table(mgx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_cs.txt", sep = "\t", row.names = FALSE)

write.table(mgx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt", sep = "\t", row.names = FALSE)

write.table(mgx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

```

## df_input_data (mtx_pathabundance)

``` {r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

mtxpath<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.pathabund.xlsx",sheet = "Sheet1")

#mtxec4<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/DEC2024_WTC_Metatranscriptomics_Analysis/DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.genefamilies.cpm.l4ec.names.xlsx",sheet = "Sheet1")

#Filter the dataset based on partial string using the stringr package
#https://stackoverflow.com/questions/59829668/filter-according-to-partial-match-of-string-variable-in-r

mtxpath.reduced <- mtxpath%>% filter(!str_detect(Pathway, "UNGROUPED|UNMAPPED|NO_NAME|UNINTEGRATED"))
#mtxec4.reduced <- mtxec4 %>% filter(!str_detect(Gene_family, "UNGROUPED|UNMAPPED|NO_NAME"))

#Note that the subgene families use the "g_" string so you can use that to reduce the EC data to the major family. Same with the "unclassified term"

mtxpath.reduced <- mtxpath%>% filter(!str_detect(Pathway, "UNGROUPED|UNMAPPED|NO_NAME|UNINTEGRATED|g_|unclassified"))
#mtxec4.reduced <- mtxec4 %>% filter(!str_detect(Gene_family, "UNGROUPED|UNMAPPED|NO_NAME|g_|unclassified"))

#Now transpose the data

gene_list<-unlist((mtxpath.reduced[,1]))
sample_list<-unlist((mtxpath.reduced[1,]))

#gene_list<-unlist((mtxec4.reduced[,1]))
#sample_list<-unlist((mtxec4.reduced[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mtx.path.all<-data.frame(t(mtxpath.reduced))
#mtx.all<-data.frame(t(mtxec4.reduced))

#Trying to get rid of the colnames that snuck in the data
mtx.path.all <- mtx.path.all %>% filter(!str_detect(1, "Pathway"))
#mtx.all <- mtx.all %>% filter(!str_detect(1, "Gene_family"))

str(mtx.path.all)
names(mtx.path.all)
rownames(mtx.path.all)<-colnames(mtxpath.reduced)

#str(mtx.all)
#names(mtx.all)
#rownames(mtx.all)<-colnames(mtxec4.reduced)

mtx.path.all<-mtx.path.all%>%filter(!grepl("1CMET2-PWY: folate transformations III (E. coli)",X1))
mtx.path.all <- mtx.path.all %>% filter(!str_detect(1, "1CMET2-PWY: folate transformations III (E. coli)"))
#mtx.all<-mtx.all%>%filter(!grepl("1.1.1.1: Alcohol_dehydrogenase",X1))

colnames(mtx.path.all)<-gene_list
names(mtx.path.all)

#colnames(mtx.all)<-gene_list
#names(mtx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file
mtx.path.all$metatx_path_id <- rownames(mtx.path.all)
#mtx.all$metatx_id <- rownames(mtx.all)

mtx.path.all<-mtx.path.all%>%filter(!grepl("Pathway",metatx_path_id))
#mtx.all<-mtx.all%>%filter(!grepl("1.1.1.1: Alcohol_dehydrogenase",X1))

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mtx.path.all<-mtx.path.all%>%relocate(metatx_path_id, .before=1)
#mtx.all<-mtx.all%>%relocate(metatx_id, .before=1)

#Create subset mtx files

mtx.path.pk<-mtx.path.all%>%filter(metatx_path_id%in%mtxpathmapping.pk.all$metatx_path_id)
mtx.path.ps<-mtx.path.all%>%filter(metatx_path_id%in%mtxpathmapping.pk.ps$metatx_path_id)
mtx.path.cs<-mtx.path.all%>%filter(metatx_path_id%in%mtxpathmapping.pk.cs$metatx_path_id)

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

write.table(mtx.path.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_path_ps.txt", sep = "\t", row.names = FALSE)

write.table(mtx.path.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_path_cs.txt", sep = "\t", row.names = FALSE)

write.table(mtx.path.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_path_pk.txt", sep = "\t", row.names = FALSE)

write.table(mtx.path.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_path_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

```

## df_input_data (mtx_genefamilies)

``` {r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

mtxec4<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.genefamilies.cpm.l4ec.names.xlsx",sheet = "Sheet1")

#Filter the dataset based on partial string using the stringr package
#https://stackoverflow.com/questions/59829668/filter-according-to-partial-match-of-string-variable-in-r

mtxec4.reduced <- mtxec4 %>% filter(!str_detect(Gene_family, "UNGROUPED|UNMAPPED|NO_NAME"))

#Note that the subgene families use the "g_" string so you can use that to reduce the EC data to the major family. Same with the "unclassified term"
mtxec4.reduced <- mtxec4 %>% filter(!str_detect(Gene_family, "UNGROUPED|UNMAPPED|NO_NAME|g_|unclassified"))

#Now transpose the data

gene_list<-unlist((mtxec4.reduced[,1]))
sample_list<-unlist((mtxec4.reduced[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mtx.all<-data.frame(t(mtxec4.reduced))

#Trying to get rid of the colnames that snuck in the data
mtx.all <- mtx.all %>% filter(!str_detect(1, "Gene_family"))
str(mtx.all)
names(mtx.all)
rownames(mtx.all)<-colnames(mtxec4.reduced) 
mtx.all<-mtx.all%>%filter(!grepl("1.1.1.1: Alcohol_dehydrogenase",X1))

colnames(mtx.all)<-gene_list
names(mtx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file
mtx.all$metatx_id <- rownames(mtx.all)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mtx.all<-mtx.all%>%relocate(metatx_id, .before=1)

#Create subset mtx files
mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

```

#Prepping the MISSION mgx (from MTX) data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
library(vegan)
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#Genus level analysis;

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#had to use fill = true (https://stackoverflow.com/questions/19455070/confusing-error-in-r-error-in-scanfile-what-nmax-sep-dec-quote-skip-nli)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

#Calculating the ratio of MPAG AUC over MPA AUC during the EHR window
df_input_metadata_mission$MPAG_MPA_ratio<-df_input_metadata_mission$MPAG_AUC_0_12_dose_adj/df_input_metadata_mission$MPA_AUC_0_12_dose_adj
```

#Running the model for the abstract

``` {r}

#I need to figure out where R will spit out my results
getwd()
setwd("~/Desktop")

#You should use normalization = "none" since we already have relative abundances:
#https://forum.biobakery.org/t/maaslin2-normalization-methods/896
#https://forum.biobakery.org/t/normalization-methods/6766
#https://forum.biobakery.org/t/guide-for-normalization-transformation-for-maaslin2-input/6717

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0,
#                            normalization  = "NONE",
#                            output         = "demo_output_mission", 
#                            fixed_effects  = c("Month"),
#                            reference = c("Month,M1"))

## Adding covariates to the model
#https://stackoverflow.com/questions/19713228/lmer-error-grouping-factor-must-be-number-of-observations
#https://stats.stackexchange.com/questions/446414/in-r-error-number-of-levels-of-each-grouping-factor-must-be-number-of-obser

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.15,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
#                            fixed_effects  = c("EHR_class_40perc"),
#                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
#                            reference = c("EHR_class_40perc,low"))

#Since I am using CPM for the MTX data, I will try to use the --transform option (https://github.com/biobakery/Maaslin2) as it was the method mentioned in the 5ASA paper.

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 1,
#                            normalization  = "NONE",
#                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
#                            fixed_effects  = c("EHR_class_40perc"),
#                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
#                            reference = c("EHR_class_40perc,low"))

#Getting lots of errors in the adjusted model so taking out covariates

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 1,
#                            normalization  = "NONE",
#                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
#                            fixed_effects  = c("EHR_class_40perc"),
#                            reference = c("EHR_class_40perc,low"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "none",
#                            output         = "demo_output_5_12_AUC_diet", 
#                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"),                        # 
#                            random_effects  = c("Cohort","antibiotics_PK_2"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "none",
#                            output         = "demo_output_5_12_AUC_diet", 
#                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"),                        # 
#                            random_effects  = c("Cohort"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "none",
#                            output         = "demo_output_5_12_AUC_diet", 
#                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"),                        # 
#                            random_effects  = c("Cohort"))


```

#Run MGX models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_AUC", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_AUC_diet", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"))                         

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHR40perc", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHR40perc_diet", 
                            fixed_effects  = c("EHR_class_40perc","Dim_1","Dim_2"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2", 
                            fixed_effects  = c("EHRtertv2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2_diet", 
                            fixed_effects  = c("EHRtertv2","Dim_1","Dim_2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2binary", 
                            fixed_effects  = c("EHRtertv2binary"),
                            reference = c("EHRtertv2binary,0"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2binary_diet", 
                            fixed_effects  = c("EHRtertv2binary","Dim_1","Dim_2"),
                            reference = c("EHRtertv2binary,0"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl_diet", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_mpag", 
                            fixed_effects  = c("MPAG_AUC_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_mpag_diet", 
                            fixed_effects  = c("MPAG_AUC_0_12","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_ratio", 
                            fixed_effects  = c("MPA_MPAG_ratio"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_ratio_diet", 
                            fixed_effects  = c("MPA_MPAG_ratio","Dim_1","Dim_2")) 

#Adding specific PK parameters from Dr. Jacobson

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPAAUCDA", 
                            fixed_effects  = c("MPA_AUC_0_12_dose_adj"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPAGAUCDA", 
                            fixed_effects  = c("MPAG_AUC_0_12_dose_adj"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPAGMPA", 
                            fixed_effects  = c("MPAG_MPA_ratio"))

#got a flag for the EGFR variable so wanted to check it

table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)
print (df_input_metadata_mission$EHR_class_40perc)

df_input_metadata_mission$EGFRnum <- as.numeric(df_input_metadata_mission$eGFR_ml_min_1_73m2)
mean(df_input_metadata_mission$EGFRnum) 

```

#Testing out the lefseR package (https://www.bioconductor.org/packages/release/bioc/vignettes/lefser/inst/doc/lefser.html) anf (https://docs.cosmosid.com/docs/lefse-biomarker-discovery-analysis)
```{r}

#Analysis example

#Prepare input
#lefser package include the demo dataset, zeller14, which is the microbiome data from colorectal cancer (CRC) patients and controls (Zeller et al. 2014).

#In this vignette, we excluded the ‘adenoma’ condition and used control/CRC as the main classes and age category as sub-classes (adult vs. senior) with different numbers of samples: control-adult (n = 46), control-senior (n = 20), CRC-adult (n = 45), and CRC-senior (n = 46).

#data(zeller14)
#zeller14 <- zeller14[, zeller14$study_condition != "adenoma"]

#The class and subclass information is stored in the colData slot under the study_condition and age_category columns, respectively.

## Contingency table
#table(zeller14$age_category, zeller14$study_condition)

#You can select only the terminal node using get_terminal_nodes function.

#tn <- get_terminal_nodes(rownames(zeller14))
#zeller14tn <- zeller14[tn,]

#https://www.rdocumentation.org/packages/semtree/versions/0.9.20/topics/getTerminalNodes

#tn <- getTerminalNodes(rownames(zeller14))
#zeller14tn <- zeller14[tn,]

#Run lefser
#The lefser function returns a data.frame with two columns - the names of selected features (the features column) and their effect size (the scores column).

#There is a random number generation step in the lefser algorithm to ensure that more than half of the values for each features are unique. In most cases, inputs are sparse, so in practice, this step is handling 0s. So to reproduce the identical result, you should set the seed before running lefser.

#set.seed(1234)
#res <- lefser(df_input_data_mission, # relative abundance only with terminal nodes
#              classCol = df_input_metadata_mission$EHRtertv2binary,
#              subclassCol = df_input_metadata_mission$MPA_AUC5_12_0_12)
#head(res)
```

#Prepping the PS MISSION mgx (from MTX) data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
library(vegan)
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_ps.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#Genus level analysis;

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#had to use fill = true (https://stackoverflow.com/questions/19455070/confusing-error-in-r-error-in-scanfile-what-nmax-sep-dec-quote-skip-nli)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_ps.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

```

#Running the model for the abstract

``` {r}

#I need to figure out where R will spit out my results
getwd()
setwd("~/Desktop")

#You should use normalization = "none" since we already have relative abundances:
#https://forum.biobakery.org/t/maaslin2-normalization-methods/896
#https://forum.biobakery.org/t/normalization-methods/6766
#https://forum.biobakery.org/t/guide-for-normalization-transformation-for-maaslin2-input/6717

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0,
#                            normalization  = "NONE",
#                            output         = "demo_output_mission", 
#                            fixed_effects  = c("Month"),
#                            reference = c("Month,M1"))

## Adding covariates to the model
#https://stackoverflow.com/questions/19713228/lmer-error-grouping-factor-must-be-number-of-observations
#https://stats.stackexchange.com/questions/446414/in-r-error-number-of-levels-of-each-grouping-factor-must-be-number-of-obser

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.15,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
#                            fixed_effects  = c("EHR_class_40perc"),
#                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
#                            reference = c("EHR_class_40perc,low"))

#Since I am using CPM for the MTX data, I will try to use the --transform option (https://github.com/biobakery/Maaslin2) as it was the method mentioned in the 5ASA paper.

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 1,
#                            normalization  = "NONE",
#                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
#                            fixed_effects  = c("EHR_class_40perc"),
#                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
#                            reference = c("EHR_class_40perc,low"))

#Getting lots of errors in the adjusted model so taking out covariates

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 1,
#                            normalization  = "NONE",
#                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
#                            fixed_effects  = c("EHR_class_40perc"),
#                            reference = c("EHR_class_40perc,low"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "none",
#                            output         = "demo_output_5_12_AUC_diet", 
#                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"),                        # 
#                            random_effects  = c("Cohort","antibiotics_PK_2"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "none",
#                            output         = "demo_output_5_12_AUC_diet", 
#                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"),                        # 
#                            random_effects  = c("Cohort"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "none",
#                            output         = "demo_output_5_12_AUC_diet", 
#                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"),                        # 
#                            random_effects  = c("Cohort"))


```

#Run MGX models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_AUC", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_AUC_diet", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"))                         

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHR40perc", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHR40perc_diet", 
                            fixed_effects  = c("EHR_class_40perc","Dim_1","Dim_2"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2", 
                            fixed_effects  = c("EHRtertv2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2_diet", 
                            fixed_effects  = c("EHRtertv2","Dim_1","Dim_2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2binary", 
                            fixed_effects  = c("EHRtertv2binary"),
                            reference = c("EHRtertv2binary,0"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2binary_diet", 
                            fixed_effects  = c("EHRtertv2binary","Dim_1","Dim_2"),
                            reference = c("EHRtertv2binary,0"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl_diet", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_mpag", 
                            fixed_effects  = c("MPAG_AUC_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_mpag_diet", 
                            fixed_effects  = c("MPAG_AUC_0_12","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_ratio", 
                            fixed_effects  = c("MPA_MPAG_ratio"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_ratio_diet", 
                            fixed_effects  = c("MPA_MPAG_ratio","Dim_1","Dim_2"))  

#got a flag for the EGFR variable so wanted to check it

table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)
print (df_input_metadata_mission$EHR_class_40perc)

df_input_metadata_mission$EGFRnum <- as.numeric(df_input_metadata_mission$eGFR_ml_min_1_73m2)
mean(df_input_metadata_mission$EGFRnum) 

```

#Prepping the CS MISSION mgx (from MTX) data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
library(vegan)
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_cs.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#Genus level analysis;

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#had to use fill = true (https://stackoverflow.com/questions/19455070/confusing-error-in-r-error-in-scanfile-what-nmax-sep-dec-quote-skip-nli)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_cs.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

```

#Running the model for the abstract

``` {r}

#I need to figure out where R will spit out my results
getwd()
setwd("~/Desktop")

#You should use normalization = "none" since we already have relative abundances:
#https://forum.biobakery.org/t/maaslin2-normalization-methods/896
#https://forum.biobakery.org/t/normalization-methods/6766
#https://forum.biobakery.org/t/guide-for-normalization-transformation-for-maaslin2-input/6717

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0,
#                            normalization  = "NONE",
#                            output         = "demo_output_mission", 
#                            fixed_effects  = c("Month"),
#                            reference = c("Month,M1"))

## Adding covariates to the model
#https://stackoverflow.com/questions/19713228/lmer-error-grouping-factor-must-be-number-of-observations
#https://stats.stackexchange.com/questions/446414/in-r-error-number-of-levels-of-each-grouping-factor-must-be-number-of-obser

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.15,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
#                            fixed_effects  = c("EHR_class_40perc"),
#                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
#                            reference = c("EHR_class_40perc,low"))

#Since I am using CPM for the MTX data, I will try to use the --transform option (https://github.com/biobakery/Maaslin2) as it was the method mentioned in the 5ASA paper.

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 1,
#                            normalization  = "NONE",
#                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
#                            fixed_effects  = c("EHR_class_40perc"),
#                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
#                            reference = c("EHR_class_40perc,low"))

#Getting lots of errors in the adjusted model so taking out covariates

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 1,
#                            normalization  = "NONE",
#                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
#                            fixed_effects  = c("EHR_class_40perc"),
#                            reference = c("EHR_class_40perc,low"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "none",
#                            output         = "demo_output_5_12_AUC_diet", 
#                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"),                        # 
#                            random_effects  = c("Cohort","antibiotics_PK_2"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "none",
#                            output         = "demo_output_5_12_AUC_diet", 
#                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"),                        # 
#                            random_effects  = c("Cohort"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "none",
#                            output         = "demo_output_5_12_AUC_diet", 
#                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"),                        # 
#                            random_effects  = c("Cohort"))


```

#Run MGX models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_AUC", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_AUC_diet", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"))                         

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHR40perc", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHR40perc_diet", 
                            fixed_effects  = c("EHR_class_40perc","Dim_1","Dim_2"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2", 
                            fixed_effects  = c("EHRtertv2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2_diet", 
                            fixed_effects  = c("EHRtertv2","Dim_1","Dim_2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2binary", 
                            fixed_effects  = c("EHRtertv2binary"),
                            reference = c("EHRtertv2binary,0"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_EHRtertv2binary_diet", 
                            fixed_effects  = c("EHRtertv2binary","Dim_1","Dim_2"),
                            reference = c("EHRtertv2binary,0"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl_diet", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_mpag", 
                            fixed_effects  = c("MPAG_AUC_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_mpag_diet", 
                            fixed_effects  = c("MPAG_AUC_0_12","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_ratio", 
                            fixed_effects  = c("MPA_MPAG_ratio"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_5_12_ratio_diet", 
                            fixed_effects  = c("MPA_MPAG_ratio","Dim_1","Dim_2"))  

#got a flag for the EGFR variable so wanted to check it

table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)
print (df_input_metadata_mission$EHR_class_40perc)

df_input_metadata_mission$EGFRnum <- as.numeric(df_input_metadata_mission$eGFR_ml_min_1_73m2)
mean(df_input_metadata_mission$EGFRnum) 

```

#Prepping the MISSION path data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
library(vegan)
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_path_pk.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#Genus level analysis;

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#had to use fill = true (https://stackoverflow.com/questions/19455070/confusing-error-in-r-error-in-scanfile-what-nmax-sep-dec-quote-skip-nli)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

#Calculating the ratio of MPAG AUC over MPA AUC during the EHR window
df_input_metadata_mission$MPAG_MPA_ratio<-df_input_metadata_mission$MPAG_AUC_0_12_dose_adj/df_input_metadata_mission$MPA_AUC_0_12_dose_adj

```

#Running the model for the abstract

``` {r}

#I need to figure out where R will spit out my results
getwd()
setwd("~/Desktop")

#You should use normalization = "none" since we already have relative abundances:
#https://forum.biobakery.org/t/maaslin2-normalization-methods/896
#https://forum.biobakery.org/t/normalization-methods/6766
#https://forum.biobakery.org/t/guide-for-normalization-transformation-for-maaslin2-input/6717

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0,
#                            normalization  = "NONE",
#                            output         = "demo_output_mission", 
#                            fixed_effects  = c("Month"),
#                            reference = c("Month,M1"))

## Adding covariates to the model
#https://stackoverflow.com/questions/19713228/lmer-error-grouping-factor-must-be-number-of-observations
#https://stats.stackexchange.com/questions/446414/in-r-error-number-of-levels-of-each-grouping-factor-must-be-number-of-obser

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.15,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
                            reference = c("EHR_class_40perc,low"))

#Since I am using CPM for the MTX data, I will try to use the --transform option (https://github.com/biobakery/Maaslin2) as it was the method mentioned in the 5ASA paper.

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 1,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
                            reference = c("EHR_class_40perc,low"))

#Getting lots of errors in the adjusted model so taking out covariates

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 1,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

```

#Run path models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_AUC", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 100,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_AUC_diet", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"))                            

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHR40perc", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHR40perc_diet", 
                            fixed_effects  = c("EHR_class_40perc","Dim_1","Dim_2"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2", 
                            fixed_effects  = c("EHRtertv2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2_diet", 
                            fixed_effects  = c("EHRtertv2","Dim_1","Dim_2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2binary", 
                            fixed_effects  = c("EHRtertv2binary"),
                            reference = c("EHRtertv2binary,0"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2binary_diet", 
                            fixed_effects  = c("EHRtertv2binary","Dim_1","Dim_2"),
                            reference = c("EHRtertv2binary,0"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl_diet", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_mpag", 
                            fixed_effects  = c("MPAG_AUC_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_mpag_diet", 
                            fixed_effects  = c("MPAG_AUC_0_12","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_ratio", 
                            fixed_effects  = c("MPA_MPAG_ratio"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_ratio_diet", 
                            fixed_effects  = c("MPA_MPAG_ratio","Dim_1","Dim_2"))  

#got a flag for the EGFR variable so wanted to check it

table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)
print (df_input_metadata_mission$EHR_class_40perc)

df_input_metadata_mission$EGFRnum <- as.numeric(df_input_metadata_mission$eGFR_ml_min_1_73m2)
mean(df_input_metadata_mission$EGFRnum) 

#Adding specific PK parameters from Dr. Jacobson

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPAAUCDA", 
                            fixed_effects  = c("MPA_AUC_0_12_dose_adj"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPAGAUCDA", 
                            fixed_effects  = c("MPAG_AUC_0_12_dose_adj"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPAGMPA", 
                            fixed_effects  = c("MPAG_MPA_ratio"))
```

#Prepping the MISSION PS path data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
library(vegan)
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_path_ps.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#Genus level analysis;

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#had to use fill = true (https://stackoverflow.com/questions/19455070/confusing-error-in-r-error-in-scanfile-what-nmax-sep-dec-quote-skip-nli)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_ps.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)
#df_input_metadata_mission<-df_input_metadata_mission%>%relocate(metatx_path_id, .before=1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

#Calculating the ratio of MPAG AUC over MPA AUC during the EHR window
df_input_metadata_mission$MPAG_MPA_ratio<-df_input_metadata_mission$MPAG_AUC_0_12_dose_adj/df_input_metadata_mission$MPA_AUC_0_12_dose_adj

```

#Running the model for the abstract

``` {r}

#I need to figure out where R will spit out my results
getwd()
setwd("~/Desktop")

#You should use normalization = "none" since we already have relative abundances:
#https://forum.biobakery.org/t/maaslin2-normalization-methods/896
#https://forum.biobakery.org/t/normalization-methods/6766
#https://forum.biobakery.org/t/guide-for-normalization-transformation-for-maaslin2-input/6717

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0,
#                            normalization  = "NONE",
#                            output         = "demo_output_mission", 
#                            fixed_effects  = c("Month"),
#                            reference = c("Month,M1"))

## Adding covariates to the model
#https://stackoverflow.com/questions/19713228/lmer-error-grouping-factor-must-be-number-of-observations
#https://stats.stackexchange.com/questions/446414/in-r-error-number-of-levels-of-each-grouping-factor-must-be-number-of-obser

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.15,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
                            reference = c("EHR_class_40perc,low"))

#Since I am using CPM for the MTX data, I will try to use the --transform option (https://github.com/biobakery/Maaslin2) as it was the method mentioned in the 5ASA paper.

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 1,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
                            reference = c("EHR_class_40perc,low"))

#Getting lots of errors in the adjusted model so taking out covariates

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 1,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

```

#Run path models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_AUC", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 100,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_AUC_diet", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"))                            

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHR40perc", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHR40perc_diet", 
                            fixed_effects  = c("EHR_class_40perc","Dim_1","Dim_2"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2", 
                            fixed_effects  = c("EHRtertv2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2_diet", 
                            fixed_effects  = c("EHRtertv2","Dim_1","Dim_2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2binary", 
                            fixed_effects  = c("EHRtertv2binary"),
                            reference = c("EHRtertv2binary,0"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2binary_diet", 
                            fixed_effects  = c("EHRtertv2binary","Dim_1","Dim_2"),
                            reference = c("EHRtertv2binary,0"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl_diet", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_mpag", 
                            fixed_effects  = c("MPAG_AUC_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_mpag_diet", 
                            fixed_effects  = c("MPAG_AUC_0_12","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_ratio", 
                            fixed_effects  = c("MPA_MPAG_ratio"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_ratio_diet", 
                            fixed_effects  = c("MPA_MPAG_ratio","Dim_1","Dim_2")) 

#Adding specific PK parameters from Dr. Jacobson

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_MPAAUCDA", 
                            fixed_effects  = c("MPA_AUC_0_12_dose_adj"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_MPAGAUCDA", 
                            fixed_effects  = c("MPAG_AUC_0_12_dose_adj"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_MPAGMPA", 
                            fixed_effects  = c("MPAG_MPA_ratio"))

#got a flag for the EGFR variable so wanted to check it

table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)
print (df_input_metadata_mission$EHR_class_40perc)

df_input_metadata_mission$EGFRnum <- as.numeric(df_input_metadata_mission$eGFR_ml_min_1_73m2)
mean(df_input_metadata_mission$EGFRnum) 

```

#Prepping the MISSION CS path data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
library(vegan)
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_path_cs.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#Genus level analysis;

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#had to use fill = true (https://stackoverflow.com/questions/19455070/confusing-error-in-r-error-in-scanfile-what-nmax-sep-dec-quote-skip-nli)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_cs.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)
#df_input_metadata_mission<-df_input_metadata_mission%>%relocate(metatx_path_id, .before=1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

#Calculating the ratio of MPAG AUC over MPA AUC during the EHR window
df_input_metadata_mission$MPAG_MPA_ratio<-df_input_metadata_mission$MPAG_AUC_0_12_dose_adj/df_input_metadata_mission$MPA_AUC_0_12_dose_adj

```

#Running the model for the abstract

``` {r}

#I need to figure out where R will spit out my results
getwd()
setwd("~/Desktop")

#You should use normalization = "none" since we already have relative abundances:
#https://forum.biobakery.org/t/maaslin2-normalization-methods/896
#https://forum.biobakery.org/t/normalization-methods/6766
#https://forum.biobakery.org/t/guide-for-normalization-transformation-for-maaslin2-input/6717

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0,
#                            normalization  = "NONE",
#                            output         = "demo_output_mission", 
#                            fixed_effects  = c("Month"),
#                            reference = c("Month,M1"))

## Adding covariates to the model
#https://stackoverflow.com/questions/19713228/lmer-error-grouping-factor-must-be-number-of-observations
#https://stats.stackexchange.com/questions/446414/in-r-error-number-of-levels-of-each-grouping-factor-must-be-number-of-obser

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.15,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
                            reference = c("EHR_class_40perc,low"))

#Since I am using CPM for the MTX data, I will try to use the --transform option (https://github.com/biobakery/Maaslin2) as it was the method mentioned in the 5ASA paper.

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 1,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
                            reference = c("EHR_class_40perc,low"))

#Getting lots of errors in the adjusted model so taking out covariates

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 1,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

```

#Run path models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_AUC", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 100,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_AUC_diet", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"))                            

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHR40perc", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHR40perc_diet", 
                            fixed_effects  = c("EHR_class_40perc","Dim_1","Dim_2"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2", 
                            fixed_effects  = c("EHRtertv2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2_diet", 
                            fixed_effects  = c("EHRtertv2","Dim_1","Dim_2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2binary", 
                            fixed_effects  = c("EHRtertv2binary"),
                            reference = c("EHRtertv2binary,0"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_EHRtertv2binary_diet", 
                            fixed_effects  = c("EHRtertv2binary","Dim_1","Dim_2"),
                            reference = c("EHRtertv2binary,0"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl_diet", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_mpag", 
                            fixed_effects  = c("MPAG_AUC_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_mpag_diet", 
                            fixed_effects  = c("MPAG_AUC_0_12","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_ratio", 
                            fixed_effects  = c("MPA_MPAG_ratio"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_5_12_ratio_diet", 
                            fixed_effects  = c("MPA_MPAG_ratio","Dim_1","Dim_2"))  

#Adding specific PK parameters from Dr. Jacobson

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_MPAAUCDA", 
                            fixed_effects  = c("MPA_AUC_0_12_dose_adj"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_MPAGAUCDA", 
                            fixed_effects  = c("MPAG_AUC_0_12_dose_adj"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_MPAGMPA", 
                            fixed_effects  = c("MPAG_MPA_ratio"))

#got a flag for the EGFR variable so wanted to check it

table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)
print (df_input_metadata_mission$EHR_class_40perc)

df_input_metadata_mission$EGFRnum <- as.numeric(df_input_metadata_mission$eGFR_ml_min_1_73m2)
mean(df_input_metadata_mission$EGFRnum) 

```

#Prepping the MISSION mtx data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
library(vegan)
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D14_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#Genus level analysis;

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/D15_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_dietpcs.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#had to use fill = true (https://stackoverflow.com/questions/19455070/confusing-error-in-r-error-in-scanfile-what-nmax-sep-dec-quote-skip-nli)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Users/guillaumeonyeaghala/Downloads/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

```

#Running the model for the abstract

``` {r}

#I need to figure out where R will spit out my results
getwd()
setwd("~/Desktop")

#You should use normalization = "none" since we already have relative abundances:
#https://forum.biobakery.org/t/maaslin2-normalization-methods/896
#https://forum.biobakery.org/t/normalization-methods/6766
#https://forum.biobakery.org/t/guide-for-normalization-transformation-for-maaslin2-input/6717

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0,
#                            normalization  = "NONE",
#                            output         = "demo_output_mission", 
#                            fixed_effects  = c("Month"),
#                            reference = c("Month,M1"))

## Adding covariates to the model
#https://stackoverflow.com/questions/19713228/lmer-error-grouping-factor-must-be-number-of-observations
#https://stats.stackexchange.com/questions/446414/in-r-error-number-of-levels-of-each-grouping-factor-must-be-number-of-obser

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.15,
                            min_abundance = 0.01,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
                            reference = c("EHR_class_40perc,low"))

#Since I am using CPM for the MTX data, I will try to use the --transform option (https://github.com/biobakery/Maaslin2) as it was the method mentioned in the 5ASA paper.

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 1,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            random_effects  = c("Cohort","age_at_PK","gender","predom_race","eGFR_ml_min_1.73m2"                          ,"final_St_CrCl_1.73","total_bilirubin.mg.dL","albumin_g.dL"),
                            reference = c("EHR_class_40perc,low"))

#Getting lots of errors in the adjusted model so taking out covariates

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 1,
                            normalization  = "NONE",
                            output         = "demo_output_CSPS_EHR40PCT_adjusted", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

```

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

``` {r}
# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

```

#Run MTX models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_5_12_AUC", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 100,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_5_12_AUC_diet", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","Dim_1","Dim_2"))                            

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_EHR40perc", 
                            fixed_effects  = c("EHR_class_40perc"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_EHR40perc_diet", 
                            fixed_effects  = c("EHR_class_40perc","Dim_1","Dim_2"),
                            reference = c("EHR_class_40perc,low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_EHRtertv2", 
                            fixed_effects  = c("EHRtertv2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_EHRtertv2_diet", 
                            fixed_effects  = c("EHRtertv2","Dim_1","Dim_2"),
                            reference = c("EHRtertv2,Low"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_EHRtertv2binary", 
                            fixed_effects  = c("EHRtertv2binary"),
                            reference = c("EHRtertv2binary,0"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_EHRtertv2binary_diet", 
                            fixed_effects  = c("EHRtertv2binary","Dim_1","Dim_2"),
                            reference = c("EHRtertv2binary,0"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr"))

#fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
#                            input_metadata = df_input_metadata_mission, 
#                            min_prevalence = 0.1,
#                            min_abundance = 0.01,
#                            normalization  = "NONE",
#                            output         = "demo_output_PS_5_12_crcl_diet", 
#                            fixed_effects  = c("MPA_renal_cl_L_hr","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_5_12_mpag", 
                            fixed_effects  = c("MPAG_AUC_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_5_12_mpag_diet", 
                            fixed_effects  = c("MPAG_AUC_0_12","Dim_1","Dim_2"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_5_12_ratio", 
                            fixed_effects  = c("MPA_MPAG_ratio"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 10,
                            normalization  = "TSS",
                            transform = "LOG",
                            output         = "demo_output_PS_5_12_ratio_diet", 
                            fixed_effects  = c("MPA_MPAG_ratio","Dim_1","Dim_2"))  

#got a flag for the EGFR variable so wanted to check it

table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)
print (df_input_metadata_mission$EHR_class_40perc)

df_input_metadata_mission$EGFRnum <- as.numeric(df_input_metadata_mission$eGFR_ml_min_1_73m2)
mean(df_input_metadata_mission$EGFRnum) 

```

#Preparing a combined dataset with taxonomy, EHR and superfamily data for Correlation network analysis.
```{r}

#Start from '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt' and import in excel
#Add the data from mgx_pk to mgxmapping_pk_all
#Then select the 2 BGUS superfamilies in mtx_path_pk . Remember to remove "FR45109.1.r1_kneaddata_Abundance" from the dataset
#Save the datset in "'/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_mtx_mapping_pk_all.xlsx'"
#For the presentation , I also included data from '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt' and saved the updated file to "'/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_mtx_mapping_pk_all_2.xlsx'
#Use the R file in '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_34_ggplot2course.R' for ggplot coding
```

# read in ggplot data  ------------------------------------------------------------

```{r}

if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install()
BiocManager::install(c("phyloseq", "ggplot2"))
BiocManager::install(c("seqgroup"))		
install.packages("gitcreds")
install.packages("credentials")
install.packages("devtools")
install.packages('readxl')
install.packages('readr')
install.packages('dplyr')
install.packages("tidyverse")
install.packages("igraph")
install.packages("tidyverse")
install.packages("igraph")

```

# ggplot libraries ---------------------------------------------------------------

```{r}

library("ggplot2")
library("gitcreds")
library("credentials")
library("devtools")
library("data.table")
library("readxl")
library("readr")
library("dplyr")
library("tidyverse")
library("igraph")

```

#Setting up GGPLOT2

```{r}

mgxmtxpk <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_mtx_mapping_pk_all_2.xlsx", sheet = "mgxmapping_pk_all")
head(mgxmtxpk)

```

#GGPLOT tutorial

```{r}

#Understanding ggplot

#Recall from the previous section that the "gg" in "ggplot" stands for the grammar of graphics.
#The first component of a graph is aesthetics, which include things like color, size, position, etc. that denote different values of the variable(s) that you are graphing.
#Once aesthetics (i.e. color, size, position, etc.) are mapped to data values, they need to be represented visually on the graph by something - for example, a dot, line, or bar.  These things we actually see on the plot are called geometric objects, what ggplot2 calls "geoms" for short

#If we think of plots as:
  
#  data +
  
#  stat transformations +
  
#  geometric objects +
  
#  scaling +
  
#  labels

#It’s easy to translate what we want to see to ggplot2’s syntax:
  
#  ggplot() + 
#  stat_() + 
#  geom_() + 
#  scale_() + 
#  labs()

#Set working directory

setwd("/Users/guillaumeonyeaghala/Downloads/")

#Loading dataset
load("/Users/guillaumeonyeaghala/Downloads/R_33_MIDUS_subset-1.Rdata")
load('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_33_MIDUS_subset-1.Rdata')

#Check the data
summary(midus)

#categorical variables: mhealth, worry
#continuous variables: hrswork, worksatis

#Visualizing the data

#Plotting two continuous variables
#you use the aesthetics command "aes()" to take a variable and mapping it visually (mapping a visual asethetic unto data)

ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis)) #First layer: Set the x axis and y axis

#Second layer: what kind of geometric object can we use to look at the data (bar, point, lines , ect..)

ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis)) + 
  geom_point() #Second layer: Set the shape of each data point

ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis)) + 
  geom_point(color="blue") #Second layer: Can also specify specific geom characteristics such as the color of the point

ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis)) + 
  geom_point(color="Orchid", alpha=0.3, size = 1, shape=9) #Second layer: Can tack on more options

ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis)) + 
  geom_point(aes(color=mhealth), alpha=0.3, size = 1, shape=9) #layer 2.5: The previous examples set the options for all variables. But we can also set conditional formatting within the geom options. For example, here we are changing the color based on each mhealth value

ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis)) + 
  geom_point(aes(color=mhealth), alpha=0.3, size = 1, shape=9) + 
  geom_smooth() #layer 2.5: We can add more than one geom object. 

ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis)) + 
  geom_point(aes(color=mhealth), alpha=0.3, size = 1, shape=9) + 
  geom_smooth(method="lm", se= FALSE) #layer 2.5: We can add more than one geom object.  here we look up the options to find out how to add a best fit line , and remove the confidence interval with se = false

ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis)) + 
  geom_point(aes(color=mhealth), alpha=0.3, size = 1, shape=9) + 
  geom_smooth(method="lm", se= FALSE, aes(color=mhealth)) #layer 2.5: even more option, here we are stratifying each best fit line by mhealth as well

ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis, color=mhealth)) + 
  geom_point(alpha=0.3, size = 1, shape=9) + 
  geom_smooth(method="lm", se= FALSE) #layer 1.5: We could stratify on the first command so we dont have to use aes(color=mhealth) in each subsequent step

#can assign a piece of a ggplot layer as an R object for ease of manipulation

p <- ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis, color=mhealth)) 
p + geom_text(label="a") # for example, you can change the geometric shape of each object to a letter
p + geom_text(aes(label=age), size= 3) # It is more useful to map to a specific value from your data

#Plotting One continuous and one categorical variable

ggplot2::ggplot(midus, aes(x= worry, y=worksatis)) + 
  geom_point() #set the categorical variable on X axis and the continuous variable as the Y axis

ggplot2::ggplot(midus, aes(x= worry, y=worksatis)) + 
  geom_bar(stat="summary", fun="mean") #The previous view was not very informative, maybe we can use a bargraph instead. We will need to specify the statistic with the stat option and the function for the statistic with the fun option.

ggplot2::ggplot(midus, aes(x= worry, y=worksatis)) + 
  geom_bar(stat="summary", fun="mean", aes(color=mhealth)) #Geom_bar has two different coloring attributes: color and fill. Here we are using aes as we are mapping a color from the data

ggplot2::ggplot(midus, aes(x= worry, y=worksatis)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=mhealth)) #Geom_bar has two different coloring attributes: color and fill. Fill controls the box color itself.

#Dealing with missing variables.
#One option is to filter it out using the dplyr function filter

ggplot2::ggplot(dplyr::filter(midus, !is.na(worry), !is.na(mhealth)), aes(x= worry, y=worksatis)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=mhealth))

#We can also achieve the same result using chaining from dplyr. Note that here you only need to reference the midus dataset at the top of the chain

midus %>%
  dplyr::filter(!is.na(worry), !is.na(mhealth))  %>%
  ggplot2::ggplot (aes(x=worry, y=worksatis)) +
  geom_bar(stat="summary", fun="mean", aes(fill=mhealth))
  

#Save the filter and ggplot base layer as an object

wmh <- midus %>%
  dplyr::filter(!is.na(worry), !is.na(mhealth))  %>%
  ggplot2::ggplot (aes(x=worry, y=worksatis)) 

wmh + 
  geom_bar(stat="summary", fun="mean", aes(fill=mhealth))

wmh + 
  geom_bar(stat="summary", fun="mean", aes(fill=mhealth),
           position = "stack") #The default geom_bar option is to have each individual bar stacked

wmh + 
  geom_bar(stat="summary", fun="mean", aes(fill=mhealth),
           position = "dodge") #This can be changed to cluster the bars

wmh + 
  geom_bar(stat="summary", fun="mean", aes(fill=mhealth),
           position = "fill") #The position = fill option converts your numeric variable to its proportion equivalent

#We can do a line graph

wmh + 
  geom_line() #this isn't useful, needs a summary

wmh + 
  geom_line(stat="summary", fun="mean") #We will need to specify the statistic with the stat option and the function for the statistic with the fun option.

wmh + 
  geom_line(stat="summary", fun="mean", group=1 ) #in addition to stat and function, we also need to tell ggplot where we want to draw the line through each group (remember that the X axis is a categorical variable)

wmh + 
  geom_line(stat="summary", fun="mean", 
            aes(group=mhealth)) #We can map to a specific categorical variable as well

wmh + 
  geom_line(stat="summary", fun="mean", 
            aes(group=mhealth, color=mhealth)) #We can also make it more understandavle by coloring each strata

#It is often easier to modify your variables ahead of time in DPLYR and then get a simpler plot output
#We use summarize instead of mutate because dplyr:summarize generates a new variable in the R memory for calculations, whereas dplyr::mutate creates a new variable column in the dataset (which generates a new variable per each observation in your dataset)
#Note that you need to group by all the variables you need for the summary and stratification downstream as well

midus %>%
  dplyr::filter(!is.na(worry), !is.na(mhealth))  %>%
  dplyr::group_by(worry,mhealth) %>%
  dplyr::summarise(meanworksat = mean(worksatis,na.rm = TRUE)) %>%
  ggplot2::ggplot(aes(x=worry, y=meanworksat)) + 
  geom_bar(stat = "identity", aes(fill=mhealth),
           position = "dodge") + 
  scale_fill_brewer(palette = "Pastel1") #We can use the scale option to adjust a specific mapping option (ie the option within aes, the aesthetic)

midus %>%
  dplyr::filter(!is.na(worry), !is.na(mhealth))  %>%
  dplyr::group_by(worry,mhealth) %>%
  dplyr::summarise(meanworksat = mean(worksatis,na.rm = TRUE)) %>%
  ggplot2::ggplot(aes(x=worry, y=meanworksat)) + 
  geom_bar(stat = "identity", aes(fill=mhealth),
           position = "dodge") + 
  scale_fill_brewer(palette = "Pastel1", 
                    name = "Self rated mental health") #We can rename our legend here too

midus %>%
  dplyr::filter(!is.na(worry), !is.na(mhealth))  %>%
  dplyr::group_by(worry,mhealth, sex) %>%
  dplyr::summarise(meanworksat = mean(worksatis,na.rm = TRUE)) %>%
  ggplot2::ggplot(aes(x=worry, y=meanworksat)) + 
  geom_bar(stat = "identity", aes(fill=mhealth),
           position = "dodge") + 
  scale_fill_brewer(palette = "Pastel1", 
                    name = "Self rated mental health") + 
  facet_grid(sex~.)#We can also strafiy the graphs, but remember to amend the group_by command accordingly. here sex~. is for rows, and .~sex is for columns

midus %>%
  dplyr::filter(!is.na(worry), !is.na(mhealth))  %>%
  dplyr::group_by(worry,mhealth, sex) %>%
  dplyr::summarise(meanworksat = mean(worksatis,na.rm = TRUE)) %>%
  ggplot2::ggplot(aes(x=worry, y=meanworksat)) + 
  geom_bar(stat = "identity", aes(fill=mhealth),
           position = "dodge") + 
  scale_fill_brewer(palette = "Pastel1", 
                    name = "Self rated mental health") + 
  facet_grid(.~sex) #We can also strafiy the graphs, but remember to amend the group_by command accordingly. here sex~. is for rows, and .~sex is for columns

midus %>%
  dplyr::filter(!is.na(worry), !is.na(mhealth))  %>%
  dplyr::group_by(worry,mhealth, marstat) %>%
  dplyr::summarise(meanworksat = mean(worksatis,na.rm = TRUE)) %>%
  ggplot2::ggplot(aes(x=worry, y=meanworksat)) + 
  geom_bar(stat = "identity", aes(fill=mhealth),
           position = "dodge") + 
  scale_fill_brewer(palette = "Pastel1", 
                    name = "Self rated mental health") + 
  facet_wrap(marstat~.) #Can use facet wrap instead of facet grid for more complex variables with many levels, such as marital status

midus %>%
  dplyr::filter(!is.na(worry), !is.na(mhealth), !is.na(marstat))  %>%
  dplyr::group_by(worry,mhealth, marstat) %>%
  dplyr::summarise(meanworksat = mean(worksatis,na.rm = TRUE)) %>%
  ggplot2::ggplot(aes(x=worry, y=meanworksat)) + 
  geom_bar(stat = "identity", aes(fill=mhealth),
           position = "dodge") + 
  scale_fill_brewer(palette = "Pastel1", 
                    name = "Self rated mental health") + 
  facet_wrap(.~marstat, nrow = 3) +
  labs (x = "Amount of worry compared to others", 
        y = "Mean work satisfaction",
        title = "Work and worry my mental health and marriage status")  #More complicated facet wrap where we force the grid to rows and then we decide on the number of rows

#How to add themes in ggplot 

#First let's save the core plot to an R object

wwmh <- midus %>%
  dplyr::filter(!is.na(worry), !is.na(mhealth), !is.na(marstat))  %>%
  dplyr::group_by(worry,mhealth, marstat) %>%
  dplyr::summarise(meanworksat = mean(worksatis,na.rm = TRUE)) %>%
  ggplot2::ggplot(aes(x=worry, y=meanworksat)) + 
  geom_bar(stat = "identity", aes(fill=mhealth),
           position = "dodge") + 
  scale_fill_brewer(palette = "Pastel1", 
                    name = "Self rated mental health") + 
  labs (x = "Amount of worry compared to others", 
        y = "Mean work satisfaction",
        title = "Work and worry my mental health and marriage status")

wwmh + ggplot2::theme_bw()
wwmh + ggplot2::theme_classic()
wwmh + ggplot2::theme_dark()
wwmh + ggplot2::theme_minimal()

#Other uses of the theme

wwmh + 
  ggplot2::theme_minimal() + 
  ggplot2::theme(axis.text.x = element_text(angle=90)) #rotating the axis for legibility

#Saving the plot:

ggplot2::ggsave( wwmh, file = "pathname of the file", 
                 device = "pdf")


###ggplot activity

#clearing the environment

rm(list=ls())

#Installing the packages
install.packages("ggplot2")
library(ggplot2)

install.packages("dplyr")
library(dplyr)

#Understanding ggplot

#Recall from the previous section that the "gg" in "ggplot" stands for the grammar of graphics.
#The first component of a graph is aesthetics, which include things like color, size, position, etc. that denote different values of the variable(s) that you are graphing.
#Once aesthetics (i.e. color, size, position, etc.) are mapped to data values, they need to be represented visually on the graph by something - for example, a dot, line, or bar.  These things we actually see on the plot are called geometric objects, what ggplot2 calls "geoms" for short

#If we think of plots as:

#  data +

#  stat transformations +

#  geometric objects +

#  scaling +

#  labels

#It’s easy to translate what we want to see to ggplot2’s syntax:

#  ggplot() + 
#  stat_() + 
#  geom_() + 
#  scale_() + 
#  labs()

#Set working directory

setwd("/Users/guillaumeonyeaghala/Downloads/")

#Loading dataset
load("/Users/guillaumeonyeaghala/Downloads/R_33_MIDUS_subset-1.Rdata")

#Check the data
summary(midus)

ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis, color=mhealth)) + 
  geom_point(alpha=0.3, size = 1, shape=9) + 
  geom_smooth(method="lm", se= FALSE) #layer 1.5: We could stratify on the first command so we dont have to use aes(color=mhealth) in each subsequent step

#can assign a piece of a ggplot layer as an R object for ease of manipulation

p <- ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis, color=mhealth)) 
p + geom_text(label="a") # for example, you can change the geometric shape of each object to a letter

#Add a fourth variable using facecetting

ggplot2::ggplot(midus, aes(x= hrswork, y=worksatis, color=mhealth)) + 
  geom_point(alpha=0.3, size = 1, shape=9) + 
  geom_smooth(method="lm", se= FALSE) + 
  facet_grid(.~sex) #We can also strafiy the graphs, but remember to amend the group_by command accordingly. here sex~. is for rows, and .~sex is for columns

#Create a bar graph showing the mean work satisfaction for respondents by the age they first
#worked for pay. Plot separate color bars by sex, and position the bars next to each other.
#Facet the graph by level of education completed. Remove the NAs from the plot.

#worksatis
#ageworkforpay 
#sex
#educ

#I will use a dplyr chain to do this

midus %>%
  dplyr::filter(!is.na(worksatis), !is.na(ageworkforpay ), !is.na(sex), !is.na(educ))  %>%
  dplyr::group_by(ageworkforpay,sex, educ) %>%
  dplyr::summarise(meanworksat = mean(worksatis,na.rm = TRUE)) %>%
  ggplot2::ggplot(aes(x=ageworkforpay, y=meanworksat)) + 
  geom_bar(stat = "identity", aes(fill=sex),
           position = "dodge") + 
  scale_fill_brewer(palette = "Pastel1", 
                    name = "Reported sex") + 
  facet_wrap(.~educ, nrow = 3) + 
  labs (x = "Age category when participant started working for pay", 
        y = "Mean work satisfaction",
        title = "Work satisfaction and paid work age by reported sex and education level") #More complicated facet wrap where we force the grid to rows and then we decide on the number of rows

#Create a scatter plot of mindfulness by life satisfaction, using respondent’s age as the text
#label rather than a point. Color the points by mental health ratings, and add separate
#color-coded linear best fit lines to the plot for each mental health rating. Finally, vary the size
#of the text labels by income (hint: you may need to remove NAs for the plot to work).

#Check the data
summary(midus)

#lifesat  
#age 
#mhealth  
#mindful

#I will use a dplyr chain to do this

midus %>%
  dplyr::filter(!is.na(lifesat), !is.na(age), !is.na(mhealth), !is.na(mindful), !is.na(income))  %>%
  ggplot2::ggplot(aes(x=mindful, y=lifesat, color=mhealth)) + 
  geom_point(alpha=0.3, size = 1, shape=9) + 
  geom_smooth(method="lm", se= FALSE) + 
  geom_text(aes(label=mindful, size= income)) # It is more useful to map to a specific value from your data

```

#Testing plots for mtx data

```{r}

mgxmtxpk <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_mtx_mapping_pk_all_2.xlsx", sheet = "mgxmapping_pk_all")
head(mgxmtxpk)

#Visualizing the data

#Plotting two continuous variables
#you use the aesthetics command "aes()" to take a variable and mapping it visually (mapping a visual asethetic unto data)

ggplot2::ggplot(mgxmtxpk, aes(x= Beta_glucuronidase)) #First layer: Set the x axis and y axis

#Second layer: what kind of geometric object can we use to look at the data (bar, point, lines , ect..)
#https://www.sthda.com/english/wiki/ggplot2-histogram-plot-quick-start-guide-r-software-and-data-visualization

# Basic histogram
#ggplot(df, aes(x=weight)) + geom_histogram()

# Change the width of bins
#ggplot(df, aes(x=weight)) + 
#  geom_histogram(binwidth=1)

# Change colors
#p<-ggplot(df, aes(x=weight)) + 
#  geom_histogram(color="black", fill="white")
#p

ggplot2::ggplot(mgxmtxpk, aes(x= Beta_glucuronidase)) + 
  geom_histogram() #Second layer: Set the shape of each data point

#Check variable type
summary(mgxmtxpk$Beta_glucuronidase)
str(mgxmtxpk$Beta_glucuronidase)

#Convert variable to numeric
mgxmtxpk$Beta_glucuronidase_num <- as.numeric(mgxmtxpk$Beta_glucuronidase)
str(mgxmtxpk$Beta_glucuronidase)
str(mgxmtxpk$Beta_glucuronidase_num)

ggplot2::ggplot(mgxmtxpk, aes(x= Beta_glucuronidase_num)) + 
  geom_histogram() #Second layer: Set the shape of each data point

ggplot2::ggplot(mgxmtxpk, aes(x= Beta_glucuronidase_num)) + 
  geom_histogram(color="darkblue", fill="lightblue") #Adding colors

#save the base plot 
p <- ggplot2::ggplot(mgxmtxpk, aes(x= Beta_glucuronidase_num)) + 
  geom_histogram(color="darkblue", fill="lightblue") #Adding colors

p+ geom_vline(aes(xintercept=mean(Beta_glucuronidase_num)),
            color="blue", linetype="dashed", size=1) #Adding mean value  

#Per the guideline in https://stackoverflow.com/questions/69779702/ggplot-multi-panel-facet-scatter-plots-separated-by-multiple-variables-and-not, data is best in long format for plots  

#Selecting the ec4 pathways that will be used in long format

mgxmtxpk.plot<-mgxmtxpk%>%select(PID,
                          Cohort.x,
                          Beta_galactosidase,
                          Beta_glucuronidase,
                          Beta_glucosidase,
                          Beta_mannosidase,
                          Beta_fructofuranosidase)%>%
        mutate(Beta_galactosidase_num=as.numeric(Beta_galactosidase),
         Beta_glucuronidase_num=as.numeric(Beta_glucuronidase),
         Beta_glucosidase_num=as.numeric(Beta_glucosidase),
         Beta_mannosidase_num=as.numeric(Beta_mannosidase),
         Beta_fructofuranosidase_num=as.numeric(Beta_fructofuranosidase))

#reshape for boxplot
ec4prep <-mgxmtxpk.plot%>%select(
                          Beta_galactosidase_num,
                          Beta_glucuronidase_num,
                          Beta_glucosidase_num,
                          Beta_mannosidase_num,
                          Beta_fructofuranosidase_num)

ec4type<-colnames(ec4prep)

mgxmtxpk.plot_long<-mgxmtxpk.plot%>%pivot_longer(cols = all_of(ec4type),
                                                 names_to = "EC4_protein", 
                                                 values_to = "EC4_protein_CPM_Value")

mgxmtxpk.plot_long<-mgxmtxpk.plot_long%>%select(PID,
                          Cohort.x,
                          EC4_protein,
                          EC4_protein_CPM_Value)

#https://www.sthda.com/english/wiki/ggplot2-histogram-plot-quick-start-guide-r-software-and-data-visualization

#Split the plot into multiple panels :

#p<-ggplot(df, aes(x=weight))+
#  geom_histogram(color="black", fill="white")+
#  facet_grid(sex ~ .)
#p

# Add mean lines
#p+geom_vline(data=mu, aes(xintercept=grp.mean, color="red"),
#            linetype="dashed")

#p<-ggplot(mgxmtxpk.plot_long, aes(x=EC4_protein_CPM_Value))+
#  geom_histogram(color="black", fill="white")+
#  facet_grid(EC4_protein ~ .)
#p

p<-ggplot(mgxmtxpk.plot_long, aes(x=EC4_protein_CPM_Value))+
  geom_histogram(color="black", fill="white")+
  facet_wrap(EC4_protein ~ .)
p

# Add mean line
p+ geom_vline(aes(xintercept=mean(EC4_protein_CPM_Value)),
            color="red", linetype="dashed", size=1)

#testing out separating by cohort
p<-ggplot(mgxmtxpk.plot_long, aes(x=EC4_protein_CPM_Value,color=Cohort.x))+
  geom_histogram(position = "dodge") +
  facet_wrap(EC4_protein ~ .)
p

#Selecting the Pathabundance groups that will be used in long format

mgxmtxpk.plot<-mgxmtxpk%>%select(PID,
                          Cohort.x,
                          GALACT_GLUCUROCAT_PWY,
                          GLUCUROCAT_PWY)%>%
        mutate(GALACT_GLUCUROCAT_PWY_num=as.numeric(GALACT_GLUCUROCAT_PWY),
        GLUCUROCAT_PWY_num=as.numeric(GLUCUROCAT_PWY))

#reshape for boxplot
pwyprep <-mgxmtxpk.plot%>%select(
                          GALACT_GLUCUROCAT_PWY_num,
                          GLUCUROCAT_PWY_num)

pwytype<-colnames(pwyprep)

mgxmtxpk.plot_long<-mgxmtxpk.plot%>%pivot_longer(cols = all_of(pwytype),
                                                 names_to = "pwy_type", 
                                                 values_to = "pwy_CPM_Value")

mgxmtxpk.plot_long<-mgxmtxpk.plot_long%>%select(PID,
                          Cohort.x,
                          pwy_type,
                          pwy_CPM_Value)

#https://www.sthda.com/english/wiki/ggplot2-histogram-plot-quick-start-guide-r-software-and-data-visualization

#Split the plot into multiple panels :

#p<-ggplot(df, aes(x=weight))+
#  geom_histogram(color="black", fill="white")+
#  facet_grid(sex ~ .)
#p

# Add mean lines
#p+geom_vline(data=mu, aes(xintercept=grp.mean, color="red"),
#            linetype="dashed")

#p<-ggplot(mgxmtxpk.plot_long, aes(x=EC4_protein_CPM_Value))+
#  geom_histogram(color="black", fill="white")+
#  facet_grid(EC4_protein ~ .)
#p

p<-ggplot(mgxmtxpk.plot_long, aes(x=pwy_CPM_Value))+
  geom_histogram(color="black", fill="white")+
  facet_wrap(pwy_type ~ .)
p

# Add mean line
p+ geom_vline(aes(xintercept=mean(pwy_CPM_Value)),
            color="red", linetype="dashed", size=1)

#testing out separating by cohort
p<-ggplot(mgxmtxpk.plot_long, aes(x=EC4_protein_CPM_Value,color=Cohort.x))+
  geom_histogram(position = "dodge") +
  facet_wrap(EC4_protein ~ .)
p

```

#Running a correlation network analysis on the superfamily data ('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_4_01_Programs_01_Python Coding/Bioinf_2_Resources_4_01_Programs_01_MISSION _16S_Programs_MAR2023/02_16S_Program 99_songbird_updated_balance_analysis_mission_shotgun_test.Rd')

```{r}

#Loading up the dataset I created to check for correlation networks based on the code kindly provided by Abdelrhaman;

#Tutorial for igraph can be found at (https://r.igraph.org/articles/igraph.html)

#clear memory ------------------------------------------------------------

rm(list = ls())

#Load packages;

#To use a package in a new R session, it will have to be loaded. This can be done in your package manager, or at the command line using the library() command:

if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install()

BiocManager::install(c("phyloseq", "ggplot2"))
library("phyloseq")
library("ggplot2")

install.packages("gitcreds")
library("gitcreds")

install.packages("credentials")
library("credentials")

BiocManager::install(c("seqgroup"))		
install.packages("devtools")
library("phyloseq")
library("ggplot2")
library(devtools)

library(data.table)
install.packages('readxl')
library(readxl)
install.packages('readr')
library(readr)
library(ggplot2)
install.packages('dplyr')
library(dplyr)
install.packages("tidyverse")
install.packages("igraph")
library(tidyverse)
library(igraph)

install.packages("tidyverse")
install.packages("igraph")
library(tidyverse)
library(igraph)
```

# libraries ---------------------------------------------------------------

```{r}

library("phyloseq")
library("ggplot2")
library("gitcreds")
library("credentials")
library("phyloseq")
library("ggplot2")
library(devtools)
library(data.table)
library(readxl)
library(readr)
library(ggplot2)
library(dplyr)
library(tidyverse)
library(igraph)

```

# read in data ------------------------------------------------------------

```{r}

#Upload the excel spreadsheets to test correlation networks (you can right click on the file and hold the option key on MAC to copy the path name);

MPAG_10X <- read_excel("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_07_01_Documents_UMGC_05_12192022_Israni4_Project_019and020_16sSequencing/02_ASN_abstracts_2023_Invitro_16S/48_11_MAP_MPAG_10X_bacteria.xlsx", sheet = "MPAG_10X_bacteria")
head(MPAG_10X)

MPAG_Lysis <- read_excel("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_07_01_Documents_UMGC_05_12192022_Israni4_Project_019and020_16sSequencing/02_ASN_abstracts_2023_Invitro_16S/48_12_MAP_MPAG_Lysis_bacteria.xlsx", sheet = "MPAG_Lysis_bacteria")
head(MPAG_Lysis)

MOSIOgenus <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/G05_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_igraph.xlsx", sheet = "Sheet6")
head(MOSIOgenus)

MOSIOgenusdietpcs <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/G05_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_dietpcs_igraph.xlsx", sheet = "Sheet8")
head(MOSIOgenusdietpcs)

MOSIOgenusdietpcscovars <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/G05_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_dietpcs_igraph_dietcovars.xlsx", sheet = "Sheet10")
head(MOSIOgenusdietpcscovars)

MOSIOgenusdietpcscovars <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/G05_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_dietpcs_igraph_dietcovars.xlsx", sheet = "Sheet11")
head(MOSIOgenusdietpcscovars)

MOSIOgenusdietpcscovarsbgus <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/G05_merged_abundance_table_kneaddata_genus_prep_APR2024_V2_PS_maaslin2_M1_dietpcs_igraph_dietcovars_bgus.xlsx", sheet = "Sheet12")
head(MOSIOgenusdietpcscovarsbgus)

MOSIOspeciesdietpcscovarsbgus <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/G06_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2_M1_dietpcs_igraph_dietcovars_bgus.xlsx", sheet = "Sheet13")
head(MOSIOspeciesdietpcscovarsbgus)

mgxmtxpk <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_mtx_mapping_pk_all.xlsx", sheet = "mgxmapping_pk_all")
head(mgxmtxpk)

```

# Preparing the dataset

```{r}

# Create correlation matrix
PK_mic_impETA <- PK_mic_ETA%>%select(-c(ETA3, ETA4,ETA7, ETA9, ET10,ET13 ))
corr_mat.ETA_BGA <- cor(PK_mic_impETA[,-1], method="pearson")
corr_mat.ETA_BGA_subs <- corr_mat.ETA_BGA
corr_mat.ETA_BGA_subs[c(1:9),c(1:9)] <- 0
corr_mat.ETA_BGA_subs[c(10:52), c(10:52)] <- 0
corr_mat.ETA_BGA_subs[abs(round(corr_mat.ETA_BGA_subs,1)) < 0.3] <- 0

# correlation network for MPAG turnover measures -------------------------------------------------------

# Create correlation matrix MPAG_10X
# https://stackoverflow.com/questions/27125672/what-does-function-mean-in-r
# https://www.statmethods.net/management/subset.html

colnames(MPAG_10X)

#Drop the bacteria with less that 10 percent incidence

#MPAG_10X$Bifidobacterium_adolescentis MPAG_10X$Bifidobacterium_bifidum, MPAG_10X$Bifidobacterium_breve, #MPAG_10X$Bifidobacterium_pseudolongum, MPAG_10X$Bacteroides_fragilis,
#MPAG_10X$Clostridium_butyricum, MPAG_10X$Clostridium_paraputrificum, MPAG_10X$Clostridium_perfringens,
#MPAG_10X$Marvinbryantia_formatexigens, MPAG_10X$Clostridium_bifermentans, #MPAG_10X$Subdoligranulum_variabile 

PK_MPAG_10x_Bacteria_names <- c("C10X_0_Mins_ug_mL", "C10X_30_Mins_ug_mL", "C10X_60_Mins_ug_mL", "C10X_90_Mins_ug_mL", "C10X_120_Mins_ug_mL", "Collinsella_aerofaciens", "Bacteroides_ovatus", "Bacteroides_uniformis", "Clostridium_clostridioforme",  "Roseburia_inulinivorans", "Ruminococcus_gnavus", "Faecalibacterium_prausnitzii")

#Test https://rachaellappan.github.io/16S-analysis/correlation-between-otus-with-sparcc.html

# Filter out low abundance OTus
control.NPS <- control.NPS[-which(rowMeans(control.NPS) < 2),]
#In order to get the test to work, I first had to subset so that I only looked at the continuous variables not the pids.
testmosiogenus <- MOSIOgenus[1:57, 2:570]
testmosiogenus2 <-colMeans(testmosiogenus) 
testmosiogenus <-testmosiogenus[-which(colMeans(testmosiogenus) < 1.0),]
testmosiogenus <-testmosiogenus[-which(rowMeans(testmosiogenus) < 1.0),]

# Other option here https://stackoverflow.com/questions/68695829/delete-columns-based-on-average
m1[, colMeans(m1, na.rm = TRUE) >= 30, drop = FALSE]

m2<- testmosiogenus[, colMeans(testmosiogenus, na.rm = TRUE) >= 1.0, drop = FALSE]

testmosiogenusdietpcs <- MOSIOgenusdietpcs[1:41, 2:570]
m2<- testmosiogenusdietpcs[, colMeans(testmosiogenusdietpcs, na.rm = TRUE) >= 1.0, drop = FALSE]

testmosiogenusdietpcscovars <- MOSIOgenusdietpcscovars[1:41, 3:576]
m2covars<- testmosiogenusdietpcscovars[, colMeans( testmosiogenusdietpcscovars, na.rm = TRUE) >= 1.0, drop = FALSE]

testmosiogenusdietpcscovars <- MOSIOgenusdietpcscovars[1:41, 3:577]
m2covars<- testmosiogenusdietpcscovars[, colMeans( testmosiogenusdietpcscovars, na.rm = TRUE) >= 1.0, drop = FALSE]

#For the BGUS anlysis I will drop all observations without any diet data
#https://www.statmethods.net/management/subset.html
#Subset  based on variable values

MOSIOgenusdietpcscovarsbgus2 <- subset(MOSIOgenusdietpcscovarsbgus, diet_pc_check==1)

testmosiogenusdietpcscovarsbgus <- MOSIOgenusdietpcscovarsbgus2[1:41, 10:587]
m2covarsbgus<- testmosiogenusdietpcscovarsbgus[, colMeans( testmosiogenusdietpcscovarsbgus, na.rm = TRUE) >= 1.0, drop = FALSE]

#Only looking a bugs associated with diarrhea in the subset adjusted for diet;
#https://www.statmethods.net/management/subset.html
#Subset  based on variable values
MOSIOspeciesdietpcscovarsbgus2 <- subset(MOSIOspeciesdietpcscovarsbgus, diet_pc_check==1)

testmosiospeciesdietpcscovarsbgus <- MOSIOspeciesdietpcscovarsbgus2[1:41, 10:21]

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
mgxmtxpk$MPA_MPAG_ratio<-mgxmtxpk$MPA_AUC_5_12/mgxmtxpk$MPAG_AUC_5_12

mgxmtxpk2<-mgxmtxpk%>%select(metagx_id,metatx_path_id,shotgun_id,PID,Cohort.x,MPA_AUC_0_12,MPA_AUC_5_12,MPA_AUC5_12_0_12,MPAG_AUC_0_12,MPA_MPAG_ratio,GALACT_GLUCUROCAT_PWY,GLUCUROCAT_PWY,s__Collinsella_aerofaciens,s__Bacteroides_ovatus,s__Bacteroides_uniformis,s__Enterocloster_clostridioformis,s__Roseburia_inulinivorans,s__Ruminococcus_gnavus,s__Faecalibacterium_prausnitzii,s__Bacteroides_thetaiotaomicron,s__Blautia_wexlerae)

#names(mgxmtxpk2)
#testmgxmtxpk2 <- mgxmtxpk2[1:87, 13:21]
#reduced.data<-apply(reduced.data,2,as.numeric)
#testmgxmtxpk2 <- apply(testmgxmtxpk2,2,as.numeric)
#testmgxmtxpk3 <- colMeans(testmgxmtxpk2)

#Changing the name of the 10X variables to their standardized concentrations (There is no standardized measure for T0);

colnames(MPAG_10X)
PK_MPAG_10x_Bacteria_names <- c("C10X_T30_standardized", "C10X_T60_standardized", "C10X_T90_standardized", "C10X_T120_standardized", "Collinsella_aerofaciens", "Bacteroides_ovatus", "Bacteroides_uniformis", "Clostridium_clostridioforme",  "Roseburia_inulinivorans", "Ruminococcus_gnavus", "Faecalibacterium_prausnitzii")

PK_MPAG_10x_Bacteria <- MPAG_10X[PK_MPAG_10x_Bacteria_names]

colnames(PK_MPAG_10x_Bacteria)

corr_mat_PK_MPAG_10x_Bacteria <- cor(PK_MPAG_10x_Bacteria, method="pearson")

corr_mat_mosiogenus <- cor(m2, method="pearson")

corr_mat_mosiogenusdietpcs <- cor(m2covars, method="pearson")

corr_mat_mosiogenusdietpcsbgus <- cor(m2covarsbgus, method="pearson")

corr_mat_mosiospeciesdietpcsbgus <- cor(testmosiospeciesdietpcscovarsbgus, method="pearson")

mgxmtxpkps2<-mgxmtxpk2%>%filter(Cohort.x=="PS")

mgxmtxpkcs2<-mgxmtxpk2%>%filter(Cohort.x=="CS")

mgxmtxpk4<-mgxmtxpk2%>%select(MPA_AUC_0_12,MPA_AUC_5_12,MPA_AUC5_12_0_12,MPAG_AUC_0_12,MPA_MPAG_ratio,GALACT_GLUCUROCAT_PWY,GLUCUROCAT_PWY,s__Collinsella_aerofaciens,s__Bacteroides_ovatus,s__Bacteroides_uniformis,s__Ruminococcus_gnavus,s__Faecalibacterium_prausnitzii,s__Bacteroides_thetaiotaomicron,s__Blautia_wexlerae)

mgxmtxpkps4<-mgxmtxpkps2%>%select(MPA_AUC_0_12,MPA_AUC_5_12,MPA_AUC5_12_0_12,MPAG_AUC_0_12,MPA_MPAG_ratio,GALACT_GLUCUROCAT_PWY,GLUCUROCAT_PWY,s__Collinsella_aerofaciens,s__Bacteroides_ovatus,s__Bacteroides_uniformis,s__Ruminococcus_gnavus,s__Faecalibacterium_prausnitzii,s__Bacteroides_thetaiotaomicron,s__Blautia_wexlerae)

mgxmtxpkcs4<-mgxmtxpkcs2%>%select(MPA_AUC_0_12,MPA_AUC_5_12,MPA_AUC5_12_0_12,MPAG_AUC_0_12,MPA_MPAG_ratio,GALACT_GLUCUROCAT_PWY,GLUCUROCAT_PWY,s__Collinsella_aerofaciens,s__Bacteroides_ovatus,s__Bacteroides_uniformis,s__Ruminococcus_gnavus,s__Faecalibacterium_prausnitzii,s__Bacteroides_thetaiotaomicron,s__Blautia_wexlerae)

mgxmtxpk5<-mgxmtxpk2%>%select(MPA_AUC5_12_0_12,GALACT_GLUCUROCAT_PWY,GLUCUROCAT_PWY,s__Collinsella_aerofaciens,s__Bacteroides_ovatus,s__Bacteroides_uniformis,s__Ruminococcus_gnavus,s__Faecalibacterium_prausnitzii,s__Bacteroides_thetaiotaomicron,s__Blautia_wexlerae)

mgxmtxpk6<-mgxmtxpk2%>%select(MPAG_AUC_0_12,GALACT_GLUCUROCAT_PWY,GLUCUROCAT_PWY,s__Collinsella_aerofaciens,s__Bacteroides_ovatus,s__Bacteroides_uniformis,s__Ruminococcus_gnavus,s__Faecalibacterium_prausnitzii,s__Bacteroides_thetaiotaomicron,s__Blautia_wexlerae)

mgxmtxpk7<-mgxmtxpk2%>%select(MPA_MPAG_ratio,GALACT_GLUCUROCAT_PWY,GLUCUROCAT_PWY,s__Collinsella_aerofaciens,s__Bacteroides_ovatus,s__Bacteroides_uniformis,s__Ruminococcus_gnavus,s__Faecalibacterium_prausnitzii,s__Bacteroides_thetaiotaomicron,s__Blautia_wexlerae)

#Dropping enterocloster and roseburia because of low abundance and prevalence
#mgxmtxpk4<-mgxmtxpk2%>%select(GALACT_GLUCUROCAT_PWY,GLUCUROCAT_PWY,s__Collinsella_aerofaciens,s__Bacteroides_ovatus,s__Bacteroides_uniformis,s__Enterocloster_clostridioformis,s__Roseburia_inulinivorans,s__Ruminococcus_gnavus,s__Faecalibacterium_prausnitzii,s__Bacteroides_thetaiotaomicron,s__Blautia_wexlerae)

write.table(mgxmtxpk4, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_mtx_pk.txt", sep = "\t", row.names = FALSE)

corr_mat_mgxmtx <- cor(mgxmtxpk4, method="pearson")

corr_mat_mgxmtx <- cor(mgxmtxpkps4, method="pearson")

corr_mat_mgxmtx <- cor(mgxmtxpkcs4, method="pearson")

#Tricking the matrix to remove the correlation between10x levels and between the bacteria
head(corr_mat_PK_MPAG_10x_Bacteria)
corr_mat_PK_MPAG_10x_Bacteria_sub <- corr_mat_PK_MPAG_10x_Bacteria

head(corr_mat_PK_MPAG_10x_Bacteria_sub)
colnames(corr_mat_PK_MPAG_10x_Bacteria_sub)
corr_mat_PK_MPAG_10x_Bacteria_sub[c(1:3),c(1:3)] <- 0
corr_mat_PK_MPAG_10x_Bacteria_sub[c(1:4),c(1:4)] <- 0

head(corr_mat_PK_MPAG_10x_Bacteria_sub)
colnames(corr_mat_PK_MPAG_10x_Bacteria_sub)
corr_mat_PK_MPAG_10x_Bacteria_sub[c(4:10), c(4:10)] <- 0
corr_mat_PK_MPAG_10x_Bacteria_sub[c(5:11), c(5:11)] <- 0
corr_mat_PK_MPAG_10x_Bacteria_sub[abs(round(corr_mat_PK_MPAG_10x_Bacteria_sub,1)) < 0.3] <- 0

head(corr_mat_PK_MPAG_10x_Bacteria_sub)

head(corr_mat_mosiogenusdietpcs)
corr_mat_mosiogenusdietpcs[c(1:5), c(1:5)] <- 0
corr_mat_mosiogenusdietpcs[c(6:25), c(6:25)] <- 0

head(corr_mat_mosiogenusdietpcs)
corr_mat_mosiogenusdietpcs[c(1:6), c(1:6)] <- 0
corr_mat_mosiogenusdietpcs[c(7:26), c(7:26)] <- 0

head(corr_mat_mosiogenusdietpcsbgus)
corr_mat_mosiogenusdietpcsbgus[c(1:6), c(1:6)] <- 0
corr_mat_mosiogenusdietpcsbgus[c(7:7), c(7:7)] <- 0
corr_mat_mosiogenusdietpcsbgus[c(8:27), c(8:27)] <- 0

head(corr_mat_mosiospeciesdietpcsbgus)
corr_mat_mosiospeciesdietpcsbgus[c(1:6), c(1:6)] <- 0
corr_mat_mosiospeciesdietpcsbgus[c(7:12), c(7:12)] <- 0

names(mgxmtxpk4)
corr_mat_mgxmtx[c(1:5), c(1:5)] <- 0
corr_mat_mgxmtx[c(6:14), c(6:14)] <- 0

```

# CORRELATION NETWORK USING IGRAPH ########################

```{r}

##Since the graph matrix shows the self correlations of 1.0, I have to trick the table into ignoring the autocorrelation (https://stackoverflow.com/questions/40086031/remove-self-loops-and-vertex-with-no-edges)
#I can use the following comment "Use the function graph_from_adjacency_matrix to convert your adjacency matrix into a graph and set the argument diag=F.""

# Convert correlation matrix to a graph object
graph_10X <- graph.adjacency(abs(corr_mat_PK_MPAG_10x_Bacteria_sub), mode="undirected", weighted=TRUE)

graph_mosiogenus <- graph.adjacency(abs(corr_mat_mosiogenus), mode="undirected", weighted=TRUE)
graph_mosiogenus <- graph.adjacency(abs(corr_mat_mosiogenus), mode="undirected", weighted=TRUE, diag=F)

#Including diet data
graph_mosiogenusdietpcs <- graph.adjacency(abs(corr_mat_mosiogenusdietpcs), mode="undirected", weighted=TRUE)

#Looking at BGUS data
graph_mosiogenusdietpcsbgus <- graph.adjacency(abs(corr_mat_mosiogenusdietpcsbgus), mode="undirected", weighted=TRUE)

#only diarrhea associated bugs after adjusting for diet
graph_mosiospeciesdietpcsbgus <- graph.adjacency(abs(corr_mat_mosiospeciesdietpcsbgus), mode="undirected", weighted=TRUE)

#MGX and MTX data for BGUS superfamilies

graph_mgxmtxpk <- graph.adjacency(abs(corr_mat_mgxmtx), mode="undirected", weighted=TRUE)

# Set the vertex names to the IDs
V(graph_10X)$name <- colnames(PK_MPAG_10x_Bacteria)
V(graph_10X)$label.cex <- 0.9
V(graph_10X)$label.dist <- 0.1
V(graph_10X)$label.background.color <- "black"
ew <- E(graph_10X)$weight * 7

V(graph_mosiogenus)$name <- colnames(m2)
V(graph_mosiogenus)$label.cex <- 0.9
V(graph_mosiogenus)$label.dist <- 0.1
V(graph_mosiogenus)$label.background.color <- "black"
ew <- E(graph_mosiogenus)$weight * 1

V(graph_mosiogenusdietpcs)$name <- colnames(m2covars)
V(graph_mosiogenusdietpcs)$label.cex <- 0.9
V(graph_mosiogenusdietpcs)$label.dist <- 0.1
V(graph_mosiogenusdietpcs)$label.background.color <- "black"
ew <- E(graph_mosiogenusdietpcs)$weight * 1

V(graph_mosiogenusdietpcsbgus)$name <- colnames(m2covarsbgus)
V(graph_mosiogenusdietpcsbgus)$label.cex <- 0.9
V(graph_mosiogenusdietpcsbgus)$label.dist <- 0.1
V(graph_mosiogenusdietpcsbgus)$label.background.color <- "black"
ew <- E(graph_mosiogenusdietpcsbgus)$weight * 1

V(graph_mosiospeciesdietpcsbgus)$name <- colnames(testmosiospeciesdietpcscovarsbgus)
V(graph_mosiospeciesdietpcsbgus)$label.cex <- 0.9
V(graph_mosiospeciesdietpcsbgus)$label.dist <- 0.1
V(graph_mosiospeciesdietpcsbgus)$label.background.color <- "black"
ew <- E(graph_mosiospeciesdietpcsbgus)$weight * 1

V(graph_mgxmtxpk)$name <- colnames(mgxmtxpk4)
V(graph_mgxmtxpk)$label.cex <- 0.9
V(graph_mgxmtxpk)$label.dist <- 0.1
V(graph_mgxmtxpk)$label.background.color <- "black"
ew <- E(graph_mgxmtxpk)$weight * 1

# Identify modules using the Louvain algorithm
louvain_10X <- cluster_louvain(graph_10X)
louvain_10X <- cluster_louvain(graph_mosiogenus)
louvain_10X <- cluster_louvain(graph_mosiogenusdietpcs)
louvain_10X <- cluster_louvain(graph_mosiogenusdietpcsbgus)
louvain_10X <- cluster_louvain(graph_mosiospeciesdietpcsbgus)
louvain_10X <- cluster_louvain(graph_mgxmtxpk)


# Plot the network with the nodes colored by module
#https://www.r-bloggers.com/2018/05/visualizing-graphs-with-overlapping-node-groups/

par(bg="white")
plot(louvain_10X, graph_10X, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Microbiome-MPAG turnnover correlation network (threshold = 0.3)",
     vertex.label.color="black", vertex.label.dist=2, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew)
     
par(bg="white")
plot(louvain_10X, graph_mosiogenus, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Bacterial correlation network at first collection (threshold = 0.3)",
     vertex.label.color="black", vertex.label.dist=2, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew)     

par(bg="white")
plot(louvain_10X, graph_mosiogenus, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Bacterial correlation network at first collection, diet pcs (threshold = 0.3)",
     vertex.label.color="black", vertex.label.dist=2, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew)    

par(bg="white")
plot(louvain_10X, graph_mosiogenusdietpcs, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Correlation with dietary data at baseline (threshold = 0.3)",
     vertex.label.color="black", vertex.label.dist=2, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew)   

par(bg="white")
plot(louvain_10X, graph_mosiogenusdietpcs, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Correlation with diet and HEI at baseline (threshold = 0.3)",
     vertex.label.color="black", vertex.label.dist=2, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew)   

par(bg="white")
plot(louvain_10X, graph_mosiogenusdietpcsbgus, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Correlation with diet, HEI and bgus at baseline (threshold = 0.3)",
     vertex.label.color="black", vertex.label.dist=2, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew)  

par(bg="white")
plot(louvain_10X, graph_mosiospeciesdietpcsbgus, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Correlation Network with diet and HEI at baseline (threshold = 0.3)",
     vertex.label.color="black", vertex.label.dist=2, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew) 

par(bg="white")
plot(louvain_10X, graph_mgxmtxpk, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Correlation Network with MGX & MTX (threshold = 0.3)",
     vertex.label.color="black", vertex.label.dist=2, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew) 
     
# Calculate degree centrality
deg_cen <- degree(graph_10X)

# Calculate betweenness centrality
bet_cen <- betweenness(graph_10X)

# Calculate eigenvector centrality
eig_cen <- eigen_centrality(graph_10X)$vector

# Print the centrality measures for each node
cbind(deg_cen, bet_cen, eig_cen)%>%write.csv(file = "/Users/guillaumeonyeaghala/Downloads/10X_Centrality_metrics.csv",row.names = T)

#EXPORT TO CHRIS to work on cytoscape: 
adj <- get.data.frame(graph_10X, what = "edges")
write.csv(adj, file = "/Users/guillaumeonyeaghala/Downloads/10X_Correlation_Network_0.4.csv")

```

# Testing out dplyr to create a summary table for the poster

```{r}
#clear the environment
rm(list = ls())

#installing packages
install.packages("dplyr")
install.packages("tidyr")
install.packages("kableExtra")

#Installing the labelled package to edit the summary statistics
#https://stackoverflow.com/questions/76341658/how-to-add-column-labels-when-piping-in-r-with-dplyr-and-labelled
install.packages("labelled")

```

# Load the packages

```{r}
#Load packages

library(dplyr)
library(tidyr)
library(kableExtra)
library(labelled) 

```

#Using the Full MISSION cohort data for the table
```{r}

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_crcl.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

mgxmtxpk <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_mtx_mapping_pk_all.xlsx", sheet = "mgxmapping_pk_all")
head(mgxmtxpk)

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
                                           cut(MPA_AUC5_12_0_12, 
                                               vTert, 
                                               include.lowest = T, 
                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

mgxmtxpk$MPA_MPAG_ratio<-mgxmtxpk$MPA_AUC_5_12/mgxmtxpk$MPAG_AUC_5_12

```

#Summarizing the continuous data

```{r}

summary(df_input_metadata_mission) #Check the data type for all the variable we want to combine
summary(mgxmtxpk)

#Summarising the variables of interest
#https://dplyr.tidyverse.org/reference/summarise.html

##Build the summaries from dplyr (chaining command)

dattab <-
df_input_metadata_mission %>%
  dplyr::select(Cohort, age_at_PK, gender, predom_race, eGFR_Epi_RF_Cr2021, total_bilirubin_mg_dL, albumin_g_dL, MMF_tot_daily_dose_mg, MPA_AUC_0_12, MPA_AUC5_12_0_12) %>%
  labelled::set_variable_labels(age_at_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_Epi_RF_Cr2021 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MMF_tot_daily_dose_mg = "MMF daily dose", MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", 
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)")  %>% 
  dplyr::group_by(Cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_age = mean(age_at_PK),
                   sd_age = sd(age_at_PK),
                   mean_eGFR = mean(eGFR_Epi_RF_Cr2021),
                   sd_eGFR = sd(eGFR_Epi_RF_Cr2021),
                   mean_bilirubin = mean(total_bilirubin_mg_dL),
                   sd_bilirubin = sd(total_bilirubin_mg_dL),
                   mean_albumin = mean(albumin_g_dL),
                   sd_albumin = sd(albumin_g_dL),
                   mean_MMF = mean(MMF_tot_daily_dose_mg),
                   sd_MMF = sd(MMF_tot_daily_dose_mg),
                   mean_AUC = mean(MPA_AUC_0_12),
                   sd_AUC = sd(MPA_AUC_0_12),
                   mean_EHR = mean(MPA_AUC5_12_0_12),
                   sd_EHR = sd(MPA_AUC5_12_0_12))# %>%  
#Reshape the summarize output so that the case and control columns are side by side  
#  pivot_wider (names_from = Cohort,
#               values_from = c(mean_age, sd_age, mean_eGFR, sd_eGFR, mean_bilirubin, sd_bilirubin,
#                               mean_albumin, sd_albumin, mean_MMF, sd_MMF, mean_AUC, sd_AUC, mean_EHR, sd_EHR)) %>% #You #have to use the concatenate option here so that the values appear side by side, not staggered.
#  dplyr::select(variable, value, count_Case, percent_Case, count_Control, percent_Control) %>% #You can use the select #command in dplyr to force a certain order for your chosen variables
#  dplyr::mutate(variable = factor(variable, levels = c("AgeYear", "sex", "grade"))) %>% #Here we are forcing R to #display the row within the variable "variable" in a specific order
#  dplyr::arrange(variable)

dattab_check <-
df_input_metadata_mission %>%
  dplyr::select(Cohort, age_at_PK, gender, predom_race, eGFR_Epi_RF_Cr2021, total_bilirubin_mg_dL, albumin_g_dL, MMF_tot_daily_dose_mg, MPA_AUC_0_12, MPA_AUC5_12_0_12) %>%
  labelled::set_variable_labels(age_at_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_Epi_RF_Cr2021 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MMF_tot_daily_dose_mg = "MMF daily dose", MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", 
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)")  %>% 
  #dplyr::group_by(Cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_age = mean(age_at_PK),
                   sd_age = sd(age_at_PK),
                   mean_eGFR = mean(eGFR_Epi_RF_Cr2021),
                   sd_eGFR = sd(eGFR_Epi_RF_Cr2021),
                   mean_bilirubin = mean(total_bilirubin_mg_dL),
                   sd_bilirubin = sd(total_bilirubin_mg_dL),
                   mean_albumin = mean(albumin_g_dL),
                   sd_albumin = sd(albumin_g_dL),
                   mean_MMF = mean(MMF_tot_daily_dose_mg),
                   sd_MMF = sd(MMF_tot_daily_dose_mg),
                   mean_AUC = mean(MPA_AUC_0_12),
                   sd_AUC = sd(MPA_AUC_0_12),
                   mean_EHR = mean(MPA_AUC5_12_0_12),
                   sd_EHR = sd(MPA_AUC5_12_0_12))

#WTC_2025
summary(mgxmtxpk)

dattab_check <-
mgxmtxpk %>%
  dplyr::select(Cohort.x, age_at_PK, gender, predom_race, eGFR_Epi_RF_Cr2021, total_bilirubin_mg_dL, albumin_g_dL, MMF_tot_daily_dose_mg, MPA_AUC_0_12, MPA_AUC5_12_0_12) %>%
  labelled::set_variable_labels(age_at_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_Epi_RF_Cr2021 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MMF_tot_daily_dose_mg = "MMF daily dose", MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", 
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)")  %>% 
  #dplyr::group_by(Cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_age = mean(age_at_PK),
                   sd_age = sd(age_at_PK),
                   mean_eGFR = mean(eGFR_Epi_RF_Cr2021),
                   sd_eGFR = sd(eGFR_Epi_RF_Cr2021),
                   mean_bilirubin = mean(total_bilirubin_mg_dL),
                   sd_bilirubin = sd(total_bilirubin_mg_dL),
                   mean_albumin = mean(albumin_g_dL),
                   sd_albumin = sd(albumin_g_dL),
                   mean_MMF = mean(MMF_tot_daily_dose_mg),
                   sd_MMF = sd(MMF_tot_daily_dose_mg),
                   mean_AUC = mean(MPA_AUC_0_12),
                   sd_AUC = sd(MPA_AUC_0_12),
                   mean_EHR = mean(MPA_AUC5_12_0_12),
                   sd_EHR = sd(MPA_AUC5_12_0_12))

#Summarizing the categorical data (https://stackoverflow.com/questions/34587317/using-dplyr-to-create-summary-proportion-table-with-several-categorical-factor-v)

dattab2 <-
df_input_metadata_mission %>%
  dplyr::select(Cohort, age_at_PK, gender, predom_race, eGFR_ml_min_1_73m2, total_bilirubin_mg_dL, albumin_g_dL, MMF_tot_daily_dose_mg, MPA_AUC_0_12, MPA_AUC5_12_0_12) %>%
  labelled::set_variable_labels(age_at_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_ml_min_1_73m2 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MMF_tot_daily_dose_mg = "MMF daily dose", MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", 
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)")  %>% 
  dplyr::group_by(Cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::count(gender) %>%
  dplyr::mutate(freq = n / sum(n))

dattab3 <-
df_input_metadata_mission %>%
  dplyr::select(Cohort, age_at_PK, gender, predom_race, eGFR_ml_min_1_73m2, total_bilirubin_mg_dL, albumin_g_dL, MMF_tot_daily_dose_mg, MPA_AUC_0_12, MPA_AUC5_12_0_12) %>%
  labelled::set_variable_labels(age_at_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_ml_min_1_73m2 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MMF_tot_daily_dose_mg = "MMF daily dose", MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", 
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)")  %>% 
  dplyr::group_by(Cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::count(predom_race) %>%
  dplyr::mutate(freq = n / sum(n))

dattab2 <-
mgxmtxpk %>%
  dplyr::select(Cohort.x, age_at_PK, gender, predom_race, eGFR_ml_min_1_73m2, total_bilirubin_mg_dL, albumin_g_dL, MMF_tot_daily_dose_mg, MPA_AUC_0_12, MPA_AUC5_12_0_12) %>%
  labelled::set_variable_labels(age_at_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_ml_min_1_73m2 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MMF_tot_daily_dose_mg = "MMF daily dose", MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", 
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)")  %>% 
  dplyr::group_by(Cohort.x) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::count(gender) %>%
  dplyr::mutate(freq = n / sum(n))

dattab3 <-
mgxmtxpk %>%
  dplyr::select(Cohort.x, age_at_PK, gender, predom_race, eGFR_ml_min_1_73m2, total_bilirubin_mg_dL, albumin_g_dL, MMF_tot_daily_dose_mg, MPA_AUC_0_12, MPA_AUC5_12_0_12) %>%
  labelled::set_variable_labels(age_at_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_ml_min_1_73m2 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MMF_tot_daily_dose_mg = "MMF daily dose", MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", 
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)")  %>% 
  dplyr::group_by(Cohort.x) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::count(predom_race) %>%
  dplyr::mutate(freq = n / sum(n))
```

# Testing out code from "/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/R_23_Creating tables with dplyr.R" to make a table

```{r}
#setworking directory
setwd("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/")

#read in the data

dat <- read.csv("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/R_23_Data_for_table.csv")

##Build the summaries from dplyr (chaining command)

summary(dat) #Check the data type for all the variable we want to combine

dattab <-
dat %>%
  dplyr::select(-age, -IQ) %>% # you can use the minus sign in front of a variable name to remove it from your dataset. We removed the continuous variables to tabulate the information on categorical variables first.
  dplyr::mutate(grade = as.character(grade)) %>%
  pivot_longer(sex:AgeYear,
               names_to = "variable",
               values_to = "value") %>% #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). the sex:AgeYear abbreviation lets us select every column between the tow without needing to type them out.
  dplyr::group_by(group, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(group, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100)%>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = group,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered.
  dplyr::select(variable, value, count_Case, percent_Case, count_Control, percent_Control) %>% #You can use the select command in dplyr to force a certain order for your chosen variables
  dplyr::mutate(variable = factor(variable, levels = c("AgeYear", "sex", "grade"))) %>% #Here we are forcing R to display the row within the variable "variable" in a specific order
  dplyr::arrange(variable)

##Beautify the table using the kableExtra package

dattab %>%
  kable(digits=2, col.names=c(" "," ", "count", "percent", "count", "percent")) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F)%>% #We can use preset table settings within R to set our table 
  pack_rows("Age (Years)", start_row = 1, end_row = 2) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 
  pack_rows("Sex", start_row = 3, end_row = 4) %>%
  pack_rows("Grade", start_row = 5, end_row = 7) %>%
  add_header_above(c(" "=2, "Case" = 2, "Control" = 2)) #Here we are adding a header column, and tricking R into keeping it blank for specific columns and filled in for others. (I did two blank columns as I did not delete the variable column as in the tutorial)  
  
#Add tests statistics in R to a table (chisquare and p value)

age.chisq <-
dattab %>%
  dplyr::ungroup() %>%
  dplyr::filter(variable == "AgeYear") %>%
  dplyr::select(count_Case, count_Control) %>%
  chisq.test()  #Since we are saving it as an object, we will be able to reference specific statistics later as seen below

age.chisq$statistic
age.chisq$p.value

sex.chisq <-
  dattab %>%
  dplyr::ungroup() %>%
  dplyr::filter(variable == "sex") %>%
  dplyr::select(count_Case, count_Control) %>%
  chisq.test()

grade.chisq <-
  dattab %>%
  dplyr::ungroup() %>%
  dplyr::filter(variable == "grade") %>%
  dplyr::select(count_Case, count_Control) %>%
  chisq.test()

#Make a single dataframe with all of your output for ease of use. This table can be merged with the table we have been working on in Kable, so we have to name the merging variable the same way

chisqstat <- data.frame(variable = c("AgeYear", "sex", "grade"),
                        stat = c(age.chisq$statistic, sex.chisq$statistic, grade.chisq$statistic),
                        pval = c(age.chisq$p.value,  sex.chisq$p.value , grade.chisq$p.value))

dattab %>%
  full_join (chisqstat, by="variable") %>% #This step will join our statistic values to the dattab table, using the full_join command to merge
  kable(digits=2, col.names=c(" "," ", "count", "percent", "count", "percent", "Chi-square", "p-value")) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F) %>% #We can use preset table settings within R to set our table 
  collapse_rows(columns = 7, valign= "middle") %>% #Here we are collapsing the columns with chisquare and pvlaue results as they repeat themselves
  pack_rows("Age (Years)", start_row = 1, end_row = 2) %>% #The packrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 
  pack_rows("Sex", start_row = 3, end_row = 4) %>%
  pack_rows("Grade", start_row = 5, end_row = 7) %>%
  add_header_above(c(" "=2, "Case" = 2, "Control" = 2, " "=2))

#getting R to manually edit some variables for us using the paste command

paste0("Case (n= ", length(which(dat$group=="Case")), ")")
paste0("Control (n= ", length(which(dat$group=="Control")), ")")

CaseHeader <- paste0("Case (n= ", length(which(dat$group=="Case")), ")")
ControlHeader <- paste0("Control (n= ", length(which(dat$group=="Control")), ")")

#Then you need to replicate the add_header_above section in a dataframe, which will replace the last line of code
headerinfo <- data.frame(headername = c(" ", CaseHeader, ControlHeader, " "),
                         colspan = c(2,2,2,2))

dattab %>%
  full_join (chisqstat, by="variable") %>% #This step will join our statistic values to the dattab table, using the full_join command to merge
  kable(digits=2, col.names=c(" "," ", "count", "percent", "count", "percent", "Chi-square", "p-value")) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F) %>% #We can use preset table settings within R to set our table 
  collapse_rows(columns = 7, valign= "middle") %>% #Here we are collapsing the columns with chisquare and pvlaue results as they repeat themselves
  pack_rows("Age (Years)", start_row = 1, end_row = 2) %>% #The packrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 
  pack_rows("Sex", start_row = 3, end_row = 4) %>%
  pack_rows("Grade", start_row = 5, end_row = 7) %>%
  add_header_above(headerinfo)

```

#Reapeating the coding for generating a descriptive table for the categorical variables
```{r}
#Loading the dataset
df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_crcl.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Build the summaries from dplyr (chaining command)

dattab_test <-
df_input_metadata_mission %>%
  dplyr::select(Cohort, gender, predom_race) %>% #Select only the categorical variables here
  tidyr::pivot_longer( cols = c("gender", "predom_race"),
               names_to = "variable",
               values_to = "value") %>% #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  dplyr::group_by(Cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(Cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) %>%
  #Reshape the summarize output so that the stratifed columns are side by side  
  pivot_wider (names_from = Cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered.
  dplyr::select(variable, value, count_PS, percent_PS, count_CS, percent_CS) %>% #You can use the select command in dplyr to force a certain order for your chosen variables
  
##Beautify the table using the kableExtra package

dattab_test %>%
  kable(digits=2, col.names=c(" "," ", "count", "percent", "count", "percent")) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F)%>% #We can use preset table settings within R to set our table 
  pack_rows("Sex", start_row = 1, end_row = 2) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 
  pack_rows("Ancestry", start_row = 3, end_row = 7) %>%
  add_header_above(c(" "=2, "Prospective Cohort" = 2, "Cross-sectional Cohort" = 2)) #Here we are adding a header column, and tricking R into keeping it blank for specific columns and filled in for others. (I did two blank columns as I did not delete the variable column as in the tutorial) 

#Add tests statistics in R to a table (chisquare and p value)

sex.chisq <-
dattab_test %>%
  dplyr::ungroup() %>%
  dplyr::filter(variable == "gender") %>%
  dplyr::select(count_PS, count_CS) %>%
  chisq.test()  #Since we are saving it as an object, we will be able to reference specific statistics later as seen below

sex.chisq$statistic
sex.chisq$p.value

#Also need to replace the NA values #https://tidyr.tidyverse.org/reference/replace_na.html
ancestry.chisq <-
  dattab_test %>%
  dplyr::ungroup() %>%
  dplyr::filter(variable == "predom_race") %>%
  tidyr::replace_na(list(count_PS = 0, percent_PS = 0)) %>%
  dplyr::select(count_PS, count_CS) %>%
  chisq.test()

ancestry.chisq$statistic
ancestry.chisq$p.value

#Make a single dataframe with all of your output for ease of use. This table can be merged with the table we have been working on in Kable, so we have to name the merging variable the same way

chisqstat <- data.frame(variable = c("gender", "predom_race"),
                        stat = c(sex.chisq$statistic, ancestry.chisq$statistic),
                        pval = c(sex.chisq$p.value,  ancestry.chisq$p.value))

dattab_test %>%
  full_join (chisqstat, by="variable") %>% #This step will join our statistic values to the dattab table, using the full_join command to merge
  kable(digits=2, col.names=c(" "," ", "count", "percent", "count", "percent", "Chi-square", "p-value")) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F) %>% #We can use preset table settings within R to set our table 
  collapse_rows(columns = 7, valign= "middle") %>% #Here we are collapsing the columns with chisquare and pvlaue results as they repeat themselves
#  collapse_rows(columns = 8, valign= "middle") %>% #Here we are collapsing the columns with chisquare and pvlaue results as they repeat themselves
  pack_rows("Sex", start_row = 1, end_row = 2) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 
  pack_rows("Ancestry", start_row = 3, end_row = 7) %>%
  add_header_above(c(" "=2, "Prospecitve Cohort" = 2, "Cross_sectional Cohort" = 2, " "=2)) #Here we are adding a header column, and tricking R into keeping it blank for specific columns and filled in for others. (I did two blank columns as I did not delete the variable column as in the tutorial) 

```

# correlation network for PK measures -------------------------------------------------------

```{r}

# Create correlation matrix MPAG_10X
# https://stackoverflow.com/questions/27125672/what-does-function-mean-in-r
# https://www.statmethods.net/management/subset.html

colnames(MPAG_10X)

#Drop the bacteria with less that 10 percent incidence

#MPAG_10X$Bifidobacterium_adolescentis MPAG_10X$Bifidobacterium_bifidum, MPAG_10X$Bifidobacterium_breve, #MPAG_10X$Bifidobacterium_pseudolongum, MPAG_10X$Bacteroides_fragilis,
#MPAG_10X$Clostridium_butyricum, MPAG_10X$Clostridium_paraputrificum, MPAG_10X$Clostridium_perfringens,
#MPAG_10X$Marvinbryantia_formatexigens, MPAG_10X$Clostridium_bifermentans, #MPAG_10X$Subdoligranulum_variabile 

PK_MPAG_10x_Bacteria_names2 <- c("MPA_AUC_0_12_dose_adj", "MPA_EHR_5_12", "Collinsella_aerofaciens", "Bacteroides_ovatus", "Bacteroides_uniformis", "Clostridium_clostridioforme",  "Roseburia_inulinivorans", "Ruminococcus_gnavus", "Faecalibacterium_prausnitzii")

PK_MPAG_10x_Bacteria2 <- MPAG_10X[PK_MPAG_10x_Bacteria_names2]

colnames(PK_MPAG_10x_Bacteria2)

corr_mat_PK_MPAG_10x_Bacteria2 <- cor(PK_MPAG_10x_Bacteria2, method="pearson")

#Tricking the matrix to remove the correlation between10x levels and between the bacteria
head(corr_mat_PK_MPAG_10x_Bacteria2)
corr_mat_PK_MPAG_10x_Bacteria2_sub <- corr_mat_PK_MPAG_10x_Bacteria2

colnames(corr_mat_PK_MPAG_10x_Bacteria2_sub)
head(corr_mat_PK_MPAG_10x_Bacteria2_sub)
corr_mat_PK_MPAG_10x_Bacteria2_sub[c(1:2),c(1:2)] <- 0

head(corr_mat_PK_MPAG_10x_Bacteria2_sub)
colnames(corr_mat_PK_MPAG_10x_Bacteria2_sub)
corr_mat_PK_MPAG_10x_Bacteria2_sub[c(3:9), c(3:9)] <- 0
corr_mat_PK_MPAG_10x_Bacteria2_sub[abs(round(corr_mat_PK_MPAG_10x_Bacteria2_sub,1)) < 0.3] <- 0

# Convert correlation matrix to a graph object
graph_10X2 <- graph.adjacency(abs(corr_mat_PK_MPAG_10x_Bacteria2_sub), mode="undirected", weighted=TRUE)

# Set the vertex names to the IDs
V(graph_10X2)$name <- colnames(PK_MPAG_10x_Bacteria2)
V(graph_10X2)$label.cex <- 0.9
V(graph_10X2)$label.dist <- 0.5
V(graph_10X2)$label.background.color <- "black"
ew2 <- E(graph_10X2)$weight * 7

# Identify modules using the Louvain algorithm
louvain_10X2 <- cluster_louvain(graph_10X2)

# Plot the network with the nodes colored by module

par(bg="white")
plot(louvain_10X2, graph_10X2, vertex.color=louvain_10X2$membership, vertex.label.cex=0.5,main="Microbiome- PK indices correlation network (threshold = 0.3)",
     vertex.label.color="black", vertex.label.dist=2,font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew2)

#Some loop weirness is happening
https://igraph.org/r/doc/which_multiple.html
# The page above suggested that I had missed removing a correlation network which I then fixed

# Calculate degree centrality
deg_cen2 <- degree(graph_10X2)

# Calculate betweenness centrality
bet_cen2 <- betweenness(graph_10X2)

# Calculate eigenvector centrality
eig_cen2 <- eigen_centrality(graph_10X2)$vector

# Print the centrality measures for each node
cbind(deg_cen2, bet_cen2, eig_cen2)%>%write.csv(file = "/Users/guillaumeonyeaghala/Downloads/10X2_Centrality_metrics.csv",row.names = T)

#EXPORT TO CHRIS to work on cytoscape: 
adj <- get.data.frame(graph_10X2, what = "edges")
write.csv(adj, file = "/Users/guillaumeonyeaghala/Downloads/10X2_Correlation_Network_0.4.csv")

```

# correlation network for PK measures -------------------------------------------------------

```{r}
# Create correlation matrix MPAG_10X with both PK and turnover data in the same figure
# https://stackoverflow.com/questions/27125672/what-does-function-mean-in-r
# https://www.statmethods.net/management/subset.html

colnames(MPAG_10X)
#Drop the bacteria with less that 10 percent incidence

#MPAG_10X$Bifidobacterium_adolescentis MPAG_10X$Bifidobacterium_bifidum, MPAG_10X$Bifidobacterium_breve, #MPAG_10X$Bifidobacterium_pseudolongum, MPAG_10X$Bacteroides_fragilis,
#MPAG_10X$Clostridium_butyricum, MPAG_10X$Clostridium_paraputrificum, MPAG_10X$Clostridium_perfringens,
#MPAG_10X$Marvinbryantia_formatexigens, MPAG_10X$Clostridium_bifermentans, #MPAG_10X$Subdoligranulum_variabile 

PK_MPAG_10x_Bacteria_names <- c("MPA_AUC_0_12_dose_adj", "MPA_EHR_5_12", "C10X_0_Mins_ug_mL", "C10X_30_Mins_ug_mL", "C10X_60_Mins_ug_mL", "C10X_90_Mins_ug_mL", "C10X_120_Mins_ug_mL", "Collinsella_aerofaciens", "Bacteroides_ovatus", "Bacteroides_uniformis", "Clostridium_clostridioforme",  "Roseburia_inulinivorans", "Ruminococcus_gnavus", "Faecalibacterium_prausnitzii")

#Changing the name of the 10X variables to their standardized concentrations (There is no standardized measure for T0);

colnames(MPAG_10X)
PK_MPAG_10x_Bacteria_names <- c("MPA_AUC_0_12_dose_adj", "MPA_EHR_5_12", "C10X_T30_standardized", "C10X_T60_standardized", "C10X_T90_standardized", "C10X_T120_standardized", "Collinsella_aerofaciens", "Bacteroides_ovatus", "Bacteroides_uniformis", "Clostridium_clostridioforme",  "Roseburia_inulinivorans", "Ruminococcus_gnavus", "Faecalibacterium_prausnitzii")

PK_MPAG_10x_Bacteria <- MPAG_10X[PK_MPAG_10x_Bacteria_names]

colnames(PK_MPAG_10x_Bacteria)

corr_mat_PK_MPAG_10x_Bacteria <- cor(PK_MPAG_10x_Bacteria, method="pearson")

#Tricking the matrix to remove the correlation between10x levels and between the bacteria
colnames(PK_MPAG_10x_Bacteria)
head(corr_mat_PK_MPAG_10x_Bacteria)
corr_mat_PK_MPAG_10x_Bacteria_sub <- corr_mat_PK_MPAG_10x_Bacteria

head(corr_mat_PK_MPAG_10x_Bacteria_sub)
colnames (corr_mat_PK_MPAG_10x_Bacteria_sub)
corr_mat_PK_MPAG_10x_Bacteria_sub[c(1:2),c(1:2)] <- 0

head(corr_mat_PK_MPAG_10x_Bacteria_sub)
colnames(corr_mat_PK_MPAG_10x_Bacteria_sub)
corr_mat_PK_MPAG_10x_Bacteria_sub[c(3:6), c(3:6)] <- 0

head(corr_mat_PK_MPAG_10x_Bacteria_sub)
colnames(corr_mat_PK_MPAG_10x_Bacteria_sub)
corr_mat_PK_MPAG_10x_Bacteria_sub[c(7:13), c(7:13)] <- 0
corr_mat_PK_MPAG_10x_Bacteria_sub[abs(round(corr_mat_PK_MPAG_10x_Bacteria_sub,1)) < 0.3] <- 0

# Convert correlation matrix to a graph object
graph_10X <- graph.adjacency(abs(corr_mat_PK_MPAG_10x_Bacteria_sub), mode="undirected", weighted=TRUE)

# Set the vertex names to the IDs
V(graph_10X)$name <- colnames(PK_MPAG_10x_Bacteria)
V(graph_10X)$label.cex <- 0.9
V(graph_10X)$label.dist <- 0.1
V(graph_10X)$label.background.color <- "black"
ew <- E(graph_10X)$weight * 7

# Identify modules using the Louvain algorithm
louvain_10X <- cluster_louvain(graph_10X)

# Plot the network with the nodes colored by module

par(bg="white")
plot(louvain_10X, graph_10X, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Microbiome-MPAG turnover correlation network (threshold = 0.3)",
     vertex.label.color="black",vertex.label.dist=5, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew)
     
#Trying to increase the distance between the label and the note for readability (https://r-graph-gallery.com/248-igraph-plotting-parameters.html)
par(bg="white")
plot(louvain_10X, graph_10X, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Microbiome-MPAG turnover correlation network (threshold = 0.3)",
     vertex.label.color="black",vertex.label.dist=2, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew)

# Calculate degree centrality
deg_cen <- degree(graph_10X)

# Calculate betweenness centrality
bet_cen <- betweenness(graph_10X)

# Calculate eigenvector centrality
eig_cen <- eigen_centrality(graph_10X)$vector

# Print the centrality measures for each node
cbind(deg_cen, bet_cen, eig_cen)%>%write.csv(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/03_ASN_abstracts_2023_Invitro_16S/48_43_MAP_10X_Centrality_metrics_comb.csv",row.names = T)

#EXPORT TO CHRIS to work on cytoscape: 
adj <- get.data.frame(graph_10X, what = "edges")
write.csv(adj, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/03_ASN_abstracts_2023_Invitro_16S/48_44_MAP_10X_Correlation_Network_0_3.csv")

```

# correlation network for PK measures (AUC vs Invitro only) -------------------------------------------------------

```{r}

# Create correlation matrix MPAG_10X with both PK and turnover data in the same figure
# https://stackoverflow.com/questions/27125672/what-does-function-mean-in-r
# https://www.statmethods.net/management/subset.html

colnames(MPAG_10X)
#Drop the bacteria with less that 10 percent incidence

#MPAG_10X$Bifidobacterium_adolescentis MPAG_10X$Bifidobacterium_bifidum, MPAG_10X$Bifidobacterium_breve, #MPAG_10X$Bifidobacterium_pseudolongum, MPAG_10X$Bacteroides_fragilis,
#MPAG_10X$Clostridium_butyricum, MPAG_10X$Clostridium_paraputrificum, MPAG_10X$Clostridium_perfringens,
#MPAG_10X$Marvinbryantia_formatexigens, MPAG_10X$Clostridium_bifermentans, MPAG_10X$Subdoligranulum_variabile 

PK_MPAG_10x_Bacteria_names <- c("MPA_AUC_0_12_dose_adj", "MPA_EHR_5_12", "C10X_0_Mins_ug_mL", "C10X_30_Mins_ug_mL", "C10X_60_Mins_ug_mL", "C10X_90_Mins_ug_mL", "C10X_120_Mins_ug_mL")

#Changing the name of the 10X variables to their standardized concentrations (There is no standardized measure for T0);

colnames(MPAG_10X)
PK_MPAG_10x_Bacteria_names <- c("MPA_AUC_0_12_dose_adj", "MPA_EHR_5_12", "C10X_T30_standardized", "C10X_T60_standardized", "C10X_T90_standardized", "C10X_T120_standardized")

PK_MPAG_10x_Bacteria <- MPAG_10X[PK_MPAG_10x_Bacteria_names]

colnames(PK_MPAG_10x_Bacteria)

corr_mat_PK_MPAG_10x_Bacteria <- cor(PK_MPAG_10x_Bacteria, method="pearson")

#Tricking the matrix to remove the correlation between10x levels and between the bacteria
colnames(PK_MPAG_10x_Bacteria)
head(corr_mat_PK_MPAG_10x_Bacteria)
corr_mat_PK_MPAG_10x_Bacteria_sub <- corr_mat_PK_MPAG_10x_Bacteria

head(corr_mat_PK_MPAG_10x_Bacteria_sub)
colnames (corr_mat_PK_MPAG_10x_Bacteria_sub)
corr_mat_PK_MPAG_10x_Bacteria_sub[c(1:2),c(1:2)] <- 0

head(corr_mat_PK_MPAG_10x_Bacteria_sub)
colnames(corr_mat_PK_MPAG_10x_Bacteria_sub)
corr_mat_PK_MPAG_10x_Bacteria_sub[c(3:6), c(3:6)] <- 0
corr_mat_PK_MPAG_10x_Bacteria_sub[abs(round(corr_mat_PK_MPAG_10x_Bacteria_sub,1)) < 0.3] <- 0

# Convert correlation matrix to a graph object
graph_10X <- graph.adjacency(abs(corr_mat_PK_MPAG_10x_Bacteria_sub), mode="undirected", weighted=TRUE)

# Set the vertex names to the IDs
V(graph_10X)$name <- colnames(PK_MPAG_10x_Bacteria)
V(graph_10X)$label.cex <- 0.9
V(graph_10X)$label.dist <- 0.1
V(graph_10X)$label.background.color <- "black"
ew <- E(graph_10X)$weight * 7

# Identify modules using the Louvain algorithm
louvain_10X <- cluster_louvain(graph_10X)

# Plot the network with the nodes colored by module

par(bg="white")
plot(louvain_10X, graph_10X, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Microbiome-MPAG turnover correlation network (threshold = 0.3)",
     vertex.label.color="black",vertex.label.dist=5, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew)
     
#Trying to increase the distance between the label and the note for readability (https://r-graph-gallery.com/248-igraph-plotting-parameters.html)
par(bg="white")
plot(louvain_10X, graph_10X, vertex.color=louvain_10X$membership, vertex.label.cex=0.5,main="Microbiome-MPAG turnover correlation network (threshold = 0.3)",
     vertex.label.color="black",vertex.label.dist=2, font.bold=T,vertex.label.family="sans", vertex.label.font=1, edge.width=ew)

# Calculate degree centrality
deg_cen <- degree(graph_10X)

# Calculate betweenness centrality
bet_cen <- betweenness(graph_10X)

# Calculate eigenvector centrality
eig_cen <- eigen_centrality(graph_10X)$vector

# Print the centrality measures for each node
cbind(deg_cen, bet_cen, eig_cen)%>%write.csv(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/03_ASN_abstracts_2023_Invitro_16S/48_43_MAP_10X_Centrality_metrics_comb.csv",row.names = T)

#EXPORT TO CHRIS to work on cytoscape: 
adj <- get.data.frame(graph_10X, what = "edges")
write.csv(adj, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/18_HHRI_UMGC_Projects/04_12192022_Israni4_Project_019and020_16sSequencing/Documents/03_ASN_abstracts_2023_Invitro_16S/48_44_MAP_10X_Correlation_Network_0_3.csv")

```


```{r}

```

```{r}

```

```{r}

```

```{r}

```

```{r}

```