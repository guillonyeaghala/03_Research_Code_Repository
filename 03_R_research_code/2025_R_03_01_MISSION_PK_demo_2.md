---
title: "MISSION PK Demographic info"
output: html_document
date: "2024-12-31"
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

``` {r}
#| fake R chunks, echo = FALSE, 
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

#I should chedk the decon package small tutorial at "https://bioconductor.org/packages/release/bioc/vignettes/SpatialDecon/inst/doc/SpatialDecon_vignette.html"
#and "https://bioconductor.org/packages/release/bioc/vignettes/SpatialDecon/inst/doc/SpatialDecon_vignette_NSCLC.html"

#You can also hide sections names (https://stackoverflow.com/questions/59429319/r-markdown-html-output-hide-a-section-of-the-document-of-an-html-rmarkdown-out)

```

## Load packages {.hidden .unlisted .unnumbered}

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#Package information is located at "https://bioconductor.org/packages/release/bioc/html/SpatialDecon.html" 
#Sometimes R markdown doesn't like installing packages (https://forum.posit.co/t/cannnot-knit-r-markdown/23456/2)
#So it is better to comment out this step
#if (!require("BiocManager", quietly = TRUE))
#    install.packages("BiocManager")
#BiocManager::install("SpatialDecon")

#install.packages("markdown")
#install.packages("GeomxTools")
#install.packages("tidyverse")
#install.packages("readxl")
#install.packages("dplyr")
#install.packages("ggplot2")
#install.packages("ggpubr")
#install.packages("kableExtra")
#install.packages("compositions")

#install.packages("BiocManager")
#library(BiocManager)
#BiocManager::install("SpatialDecon")

```

## Load packages {.hidden .unlisted .unnumbered}

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#setwd("/Users/Scottiejj/Desktop/Spatial RoI/")
#setwd("/Users/Scottiejj/Desktop/Spatial RoI/")
#setwd("/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/Analysis_GO/")
setwd("/Users/guillaumeonyeaghala/Desktop/MPA_EHR_QUINTILES_Analysis/")

library(SpatialDecon)
library(GeomxTools)
library(tidyverse)
library(readxl)
library(dplyr)
library(ggplot2)
library(ggpubr)
library(kableExtra)
library(compositions)
```

## Load data example {.hidden .unlisted .unnumbered}

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#remove rows with missing Glom or Slide
#gf.data<-read_xlsx("clinical data for biopsies, 6-18-2024.xlsx",sheet = 1) N=37
#DEC2024: Casey got a new clinical dataset for biopsies ('/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/david analysis 10-29-2024/cgd_demo_data_20241029.csv')
#I moved it to ('/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/Analysis_GO/00_cgd_demo_data_20241029.xlsx'). N=47

gf.data<-read_xlsx("00_clinical data for biopsies, 6-18-2024.xlsx",sheet = 1)
gf.data2<-read_xlsx("00_cgd_demo_data_20241029.xlsx",sheet = 'cgd_demo_data_20241029')

#View the dataset
view(gf.data)

#check variable names
names(gf.data)

#Update variable name
names(gf.data)[1]<-"PID"
names(gf.data2)[1]<-"PID"

#Subset onlty the PID and graft failure variables
gf.data<-gf.data%>%select(PID,graftfail_usrds)%>%rename(GF=graftfail_usrds)
gf.data2<-gf.data2%>%select(PID,graftfail_usrds)%>%rename(GF=graftfail_usrds)

#data.notes<-read_excel("Q3 for R with notes_updated .xlsx",sheet = 2)%>%filter(!is.na(Glom)&!is.na(Slide)) 
#This data is the equivalent of the annotation dataset in the deconvolution tutorials
data.notes<-read_excel("00_Q3 for R with notes_updated .xlsx",sheet = 2)%>%filter(!is.na(Glom)&!is.na(Slide)) 

#reduced.data<-readxl::read_excel("Dorr q3-norm reduced.xlsx",sheet = 3)%>%as.matrix()
#This dataset is the equivalent to the normalized dataset in the deconvolution tutorials (specifically the third tab in the spreadsheet)
reduced.data<-readxl::read_excel("00_Dorr q3-norm reduced.xlsx",sheet = 3)%>%as.matrix()

#Here, we are extracting the names in the first column as a vector, forcing all the other observations to be numeric (for the normalized gene abundance counts) and finally adding back the gene names contained in the first column 
genes<-reduced.data[,1]
reduced.data<-reduced.data[,-1]
reduced.data<-apply(reduced.data,2,as.numeric)
rownames(reduced.data)<-genes

#Glom data
#remove rows with missing Glom or Slide
#data.notes<-read_excel("/Users/Scottiejj/Desktop/Spatial RoI/Q3 for R with notes_updated .xlsx",sheet = 2)%>%filter(!is.na(Glom)) 
#This data is the equivalent of the annotation dataset in the deconvolution tutorials
data.notes<-read_excel("00_Q3 for R with notes_updated .xlsx",sheet = 2)%>%filter(!is.na(Glom))
dim(data.notes)#N=557 ROI

#exclude PID starts with 4 (Hennepin) (This is not equivalent to removing the slide variable although none of the Hennepin observations had slide = 1)

data.notes2<-data.notes%>%filter(!grepl("^4",PID))  #N = 363 RoI, (dec2024 skip to include HCMC observations)

#Checking frequencies
#https://www.geeksforgeeks.org/frequency-table-in-r/
#https://cran.r-project.org/web/packages/vcdExtra/vignettes/creating.html

freq <- table(data.notes2$PID, data.notes2$Slide)
freq
dim(freq) #28 unique PIDs across 7 slides, 363 RoIs

freq <- table(data.notes$PID, data.notes$Slide)
freq
dim(freq) #36 unique PIDs across 7 slides
dim(data.notes) #N=557 ROI

#GF data
#gf.data<-read_xlsx("/Users/Scottiejj/Desktop/Spatial RoI/clinical data for biopsies, 6-18-2024.xlsx",sheet = 1)
gf.data2<-read_xlsx("00_clinical data for biopsies, 6-18-2024.xlsx",sheet = 1)
#renaming the pid column and then subsetting the dataset to only the PID and graft failure variables
names(gf.data2)[1]<-"PID"
gf.data2<-gf.data2%>%dplyr::rename(GF=graftfail_usrds)%>%select(PID,GF)%>%mutate(GF=as.factor(GF))

#GF data
#gf.data<-read_xlsx("/Users/Scottiejj/Desktop/Spatial RoI/clinical data for biopsies, 6-18-2024.xlsx",sheet = 1)
#DEC2024: Casey got a new clinical dataset for biopsies ('/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/david analysis 10-29-2024/cgd_demo_data_20241029.csv')
#I moved it to ('/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/Analysis_GO/00_cgd_demo_data_20241029.xlsx'). N=47

gf.data<-read_xlsx("00_cgd_demo_data_20241029.xlsx",sheet = 'cgd_demo_data_20241029')
dim(gf.data) #N=47

#renaming the pid column and then subsetting the dataset to only the PID and graft failure variables
names(gf.data)[1]<-"PID"
gf.data<-gf.data%>%dplyr::rename(GF=graftfail_usrds)%>%select(PID,GF)%>%mutate(GF=as.factor(GF))

#Gene expression data
#reduced.data<-readxl::read_excel("/Users/Scottiejj/Desktop/Spatial RoI/Dorr q3-norm reduced.xlsx",sheet = 3)
#This dataset is the equivalent to the normalized dataset in the deconvolution tutorials (specifically the third tab in the spreadsheet) The rows are each gene and each ROI is in a column
reduced.data<-readxl::read_excel("00_Dorr q3-norm reduced.xlsx",sheet = 3)%>%as.matrix()
dim(reduced.data) #592 ROIs, counted the 1st column (gene names) as observations (592+1 = 593)

#view(reduced.data)

#Here we are removing the first column which contains the gene names, then transposing the columns(each ROI segment) to rows so it can be merged with the data notes, which is the equivalent of the annotation dataset
genes<-unlist(reduced.data[,1])
reduced.data<-reduced.data[,-1]%>%as.matrix()
reduced.data<-data.frame(t(reduced.data[,-1]%>%as.matrix()))%>%rownames_to_column(var = "SegmentDisplayName")%>%inner_join(data.notes,by="SegmentDisplayName")
ROI<-reduced.data$SegmentDisplayName

#I think this undoes the data transformation above so we end back with genes in rows and ROIs in columns. However with the merging step we have filtered the dataset to match the annotation dataset as seen in the dim command
reduced.data<-reduced.data%>%select(-c(names(data.notes)))%>%as.matrix()%>%t()
rownames(reduced.data)<-genes
colnames(reduced.data)<-ROI
dim(reduced.data) #556 RoIs

## Run Decon, Default Gene Expression Matrix, and decon, kidney cell expression specific matrix, not stratified by Glom

#https://github.com/Nanostring-Biostats/CellProfileLibrary/blob/master/Human/Human_datasets_metadata.csv

#The spatialdecon function takes 3 arguments of expression data:

#The normalized data.
#A matrix of expected background for all data points in the normalized data matrix.
#Optionally, either a matrix of per-data-point weights, or the raw data, which is used to derive weights (low counts are #less statistically stable, and this allows spatialdecon to down-weight them.) In the parts below this is discussed as #the safeTME matrices or even with the options on creating a custom matrix profile

#We estimate each data point’s expected background from the negative control probes from its corresponding observation:


#Here, we are extracting the names in the first column as a vector, forcing all the other observations to be numeric (foe the normalized gene abundance counts) and finally adding back the gene names contained in the first column
#Since we filtered the observations here down to 556 RoIs, we need to reapply the numeric conversion of the matrix before running the negative background generation step
#genes<-reduced.data[,1]
#reduced.data.test<-reduced.data[,-1]
reduced.data<-apply(reduced.data,2,as.numeric)
rownames(reduced.data)<-genes

dim(reduced.data)

#There should be a column for negative probe. Use rownames instead of names as it is a matrix
#names(norm)
#Notice that the third tab of the excel spreadsheet (which has the normalized data) does not have a negative probe row, which is why it was estimated in the next step below.

#rownames(reduced.data)

# and define a background matrix in which each column (observation) is the
# appropriate value of per-observation background:
#The sweep function is use to add a certain value to all columns or row in a matrix #(https://www.r-bloggers.com/2022/07/how-to-use-the-sweep-function-in-r/)

#Create background matrix for negative control probes to derive weights
bg= sweep(reduced.data * 0, 2, rep(1,dim(reduced.data)[2]), "+")
rownames(bg)<-genes


#Run SpatialDecon using defualt cell profile matrix
res = spatialdecon(norm = reduced.data,
                   bg = bg,
                   align_genes = T)

##Kidney

kidney_matrix <- download_profile_matrix(species = "Human",age_group = "Adult", matrixname = "Kidney_HCA")

res_kidney = spatialdecon(norm = reduced.data,
                   bg = bg,
                   align_genes = T,
                   X = kidney_matrix)

##Brief overview of the results
#the resulting R object is a combination of various datasets after the deconvolusion is run. Use the structure function to look at it (https://www.codecademy.com/resources/docs/r/built-in-functions/str: str, The str() function displays the internal structure of an object such as an array, list, matrix, factor, or data frame.)
#See https://bioconductor.org/packages/release/bioc/vignettes/SpatialDecon/inst/doc/SpatialDecon_vignette.html for more info
# Based on the reference manual found at https://bioconductor.org/packages/release/bioc/html/SpatialDecon.html
#runspatialdecon 
#In pData
#– beta: matrix of cell abundance estimates, cells in rows and observations in columns
#– p: matrix of p-values for H0: beta == 0
#– t: matrix of t-statistics for H0: beta == 0
#– se: matrix of standard errors of beta values
#– prop_of_all: rescaling of beta to sum to 1 in each observation
#– prop_of_nontumor: rescaling of beta to sum to 1 in each observation, excluding tumor abundance estimates
#– sigmas: covariance matrices of each observation’s beta estimates 
#• In assayData
#– yhat: a matrix of fitted values
#– resids: a matrixofresiduals from the modelfit. (log2(pmax(y, lower_thresh))- log2(pmax(xb, lower_thresh))).

str(res)
#view(res$beta) #X axis is cell type, Y axis is the proportion within each ROI
#view(res$sigmas)
#view(res$yhat)
#view(res$resids)
#view(res$p)
#view(res$t)
#view(res$prop_of_all)
#view(res$prop_of_nontumor)
#view(res$X)

#So what we get are datasets where the rows are the cell types, and the columns are the ROI or samples in this case.
#Here we focus on the cell proportions (prop_of_all)
#We transpose that dataset so that the ROI segments are in rows and then the cell types are in columns. 
#Then we add specific variables from the data.notes annotation file so we can make figures by merging based on ROI segments in rows
#Finally add the graft failure information based on the PID variable from the gf.data derived from the clinical dataset

cells<-rownames(res$prop_of_all)
prop=data.frame(t(res$prop_of_all))
prop$SegmentDisplayName<-rownames(prop)
dim(prop) #556 ROIs, 19 cell types

dim(data.notes) #556 ROIs
data.notes<-data.notes%>%select(SegmentDisplayName,Glom,Slide,PID,ROI,`CD45+`,`CD68+`)  

prop<-merge(data.notes,prop,by="SegmentDisplayName")
dim(prop)

gf.data<-read_xlsx("00_cgd_demo_data_20241029.xlsx",sheet = 'cgd_demo_data_20241029')
dim(gf.data) #N=47
names(gf.data)[1]<-"PID"
gf.data<-gf.data%>%dplyr::rename(GF=graftfail_usrds)%>%select(PID,GF)%>%mutate(GF=as.factor(GF))
prop<-merge(gf.data,prop,by="PID")
dim(prop) #423 ROIs, 26 covariates, we are loosing a bunch of ROIs because the PIDs in data.notes do not match the PIDs in gf.data

#Redoing the merging step to catch all PIDs
cells<-rownames(res$prop_of_all)
prop=data.frame(t(res$prop_of_all))
prop$SegmentDisplayName<-rownames(prop)
dim(prop) #556 ROIs, 19 cell types

dim(data.notes) #556 ROIs
data.notes<-data.notes%>%select(SegmentDisplayName,Glom,Slide,PID,ROI,`CD45+`,`CD68+`)  

prop<-merge(data.notes,prop,by="SegmentDisplayName")
dim(prop)

dim(gf.data) #need to match some of the pids in data.notes to merge 

freq <- table(data.notes$PID, data.notes$Slide)
freq

#GF data
#gf.data<-read_xlsx("/Users/Scottiejj/Desktop/Spatial RoI/clinical data for biopsies, 6-18-2024.xlsx",sheet = 1)
#DEC2024: Casey got a new clinical dataset for biopsies ('/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/david analysis 10-29-2024/cgd_demo_data_20241029.csv')
#I moved it to ('/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/Analysis_GO/00_cgd_demo_data_20241029.xlsx'). N=47

gf.data3<-read_xlsx("00_cgd_demo_data_20241029.xlsx",sheet = 'cgd_demo_data_20241029')
dim(gf.data3) #N=47

freq.data3 <- table(gf.data3$pid)
freq.data3
freq

#Since the PIDs had some typos and changes in data.notes, I manually made changes in excel and reimported the file (At casey's request I removed 4058.2 and 40058.3)
gf.data<-read_xlsx("00_cgd_demo_data_20241029_2.xlsx",sheet = 'cgd_demo_data_20241029')
dim(gf.data) #N=47

freq.data <- table(gf.data$pid)
freq.data
freq

#renaming the pid column and then subsetting the dataset to only the PID and graft failure variables
#names(gf.data)[1]<-"PID"
names(gf.data)[2]<-"PID"
gf.data<-gf.data%>%dplyr::rename(GF=graftfail_usrds)%>%select(PID,GF)%>%mutate(GF=as.factor(GF))


prop<-merge(gf.data,prop,by="PID")

dim(prop) #556 ROIs, after updting the PIDs manually in excel for gf.data I was able to catch the missing ROIs. (Down to 518 after removing 4058.2 and 40058.3 from the analysis)

#reshape for boxplot 
prop_long<-prop%>%pivot_longer(cols = all_of(cells), names_to = "Cell", values_to = "Value")
prop_long<-prop_long%>%mutate(Glom=as.factor(prop_long$Glom))

## Plot results of Default Cell Profile Matrix (Cell type comparisons by Glom)

#*: p <= 0.05

#**: p <= 0.01

#***: p <= 0.001

#****: p <= 0.0001

#Looks like you do not need to rerun the anova when using the stat compare means function from ggplot?
#https://www.rdocumentation.org/packages/ggpubr/versions/0.6.0/topics/stat_compare_means
ggplot(prop_long, aes(x = Cell, y = Value, fill = Glom)) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons, Glom vs Non Glom, Default cell matrix",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_long$Value) + 0.5) 

```

## Load mission data {.hidden .unlisted .unnumbered}

```{r}

#Demographic data

mission.data<-read_xlsx("/Users/guillaumeonyeaghala/Desktop/MPA_EHR_QUINTILES_Analysis/00_MISSION study demographics and PK n=84 04302025_tomerge.xlsx",sheet = 'Demographics')
dim(mission.data) #N=84

#GWAS data

gwas.data<-read_xlsx("/Users/guillaumeonyeaghala/Desktop/MPA_EHR_QUINTILES_Analysis/00_All 96 samples ID_tomerge.xlsx",sheet = '96 sample ID filtered')
dim(gwas.data) #N=96

#Creating a new indicator variable for ease of filtering (https://rpubs.com/prlicari13/541675)
mission.data$mission_check <- 1

##Dr. Guan will likely want the race data on all the 96 samples, I will link it to the mtx or mgx data file instead
#Merging

dim(mission.data) #N=84
names(mission.data)
mission.data.2<-mission.data%>%select(Patient_ID,age_tx,age_PK,ethnicity,gender,donar_status,date_PKvisit,date_tx,tx_to_pk_days,cohort,predom_race,mission_check) 

dim(gwas.data) #N=96
names(gwas.data)
gwas.data.2<-gwas.data%>%select(Index,Sample_ID,Call_Rate,Blood_sample_ID,Patient_ID,Month_collection) 

gwas.demo.data<-merge(gwas.data.2,mission.data.2,by="Patient_ID")

```

```{r}
#Categorizing the EHR into quintiles for quick analysis

dim(mission.data) #N=84
names(mission.data)
mission.data.3<-mission.data%>%select(Patient_ID,age_tx,age_PK,ethnicity,gender,donar_status,date_PKvisit,date_tx,tx_to_pk_days,cohort,predom_race,MPA_AUC5.12_0.12,mission_check) 

#'/Users/guillaumeonyeaghala/Desktop/MPA_EHR_QUINTILES_Analysis/01_16S_Program 101_R_43_RmarkdownMPAEHRanalysis.Rmd'
#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm)
#https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
vquint = quantile(mission.data.3$MPA_AUC5.12_0.12, c(0:5/5))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

mission.data.3$EHRquint = with(mission.data.3, 
                                           cut(MPA_AUC5.12_0.12, 
                                               vquint, 
                                               include.lowest = T, 
                                               labels = c("Q1", "Q2", "Q3", "Q4", "Q5")))

#mtx mapping file

mtxmapping.data<-read_xlsx("/Users/guillaumeonyeaghala/Desktop/MPA_EHR_QUINTILES_Analysis/00_metatx_mapping_toshare_unc.xlsx",sheet = 'Metatx_list_HUMANN_clean')
dim(mtxmapping.data) #N=86

names(mtxmapping.data)[3]<-"Patient_ID"
names(mtxmapping.data)
mtx.demo.data<-merge(mtxmapping.data,mission.data.3,by="Patient_ID")

#write.csv(aa_result, '/Users/guillaumeonyeaghala/Desktop/DE_glom_aa_vs_nonaa_custom', row.names = TRUE)
getwd()
setwd("~/Desktop")
write.csv(mtx.demo.data, '/Users/guillaumeonyeaghala/Desktop/mtx_demo_data', row.names = TRUE)
```

# testing ASN2025 abstract data {.hidden .unlisted .unnumbered}

```{r}

#total number of reads

mtx.reads<-read_xlsx("/Users/guillaumeonyeaghala/Desktop/MPA_EHR_QUINTILES_Analysis/00_Shotgun_list_HUMANN_sorting_collection_date_time_V10_readsprep.xlsx",sheet = 'Sheet2')
dim(mtx.reads) #N=269

#BGUS blast data

bgus.data<-read_xlsx("/Users/guillaumeonyeaghala/Desktop/MPA_EHR_QUINTILES_Analysis/00_BGUS counts all PK_v3.xlsx",sheet = 'Sheet2')
dim(bgus.data) #N=85

#Adding the total reads to the bgus blast data
bgus.reads.data<-merge(bgus.data,mtx.reads,by="metatx_id")

#write.csv(aa_result, '/Users/guillaumeonyeaghala/Desktop/DE_glom_aa_vs_nonaa_custom', row.names = TRUE)
getwd()
setwd("~/Desktop")
write.csv(bgus.reads.data, '/Users/guillaumeonyeaghala/Desktop/mtx_bgus_reads', row.names = TRUE)

```

#Check to see if there is an association between total number of reads (relative abundance)

```{r}

#PK & demo data

#mission.data<-read_xlsx("/Users/guillaumeonyeaghala/Desktop/MPA_EHR_QUINTILES_Analysis/00_MISSION study demographics and PK n=84 04302025_tomerge.xlsx",sheet = 'Demographics')

mission.data<-read_xlsx("/Volumes/RAVPOWER2/Temp_MPA_EHR_QUINTILES_Analysis/00_MISSION study demographics and PK n=84 04302025_tomerge.xlsx",sheet = 'Demographics')

dim(mission.data) #N=84

mission.data.4<-mission.data%>%select(Patient_ID,MPA_AUC_0_12,MPA_AUC_5_12,MPA_AUC_6_12,MPA_AUC6.12_0.12,
                                      MPA_EHR_Peaks_count,MPA_AUC_0_12_dose_adj,MPA_AUC_5_12_dose_adj,
                                      MPA_AUC_6_12_dose_adj,MPAG_AUC6.12_0.12,MPAG_AUC5.12_0.12,MPAG_AUC_0_12_dose_adj,
                                      MPAG_AUC_5_12_dose_adj,MPAG_AUC_6_12_dose_adj) 

#BGUS demo data

#bgus.data<-read_xlsx("/Users/guillaumeonyeaghala/Desktop/MPA_EHR_QUINTILES_Analysis/00_mtx_bgus_reads.xlsx",sheet = 'Sheet1')

bgus.data<-read_xlsx("/Volumes/RAVPOWER2/Temp_MPA_EHR_QUINTILES_Analysis/00_mtx_bgus_reads_V3.xlsx",sheet = 'Sheet1')

dim(bgus.data) #n=82
names(bgus.data)

#Adding the other PK variables to look at
bgus.data.2<-merge(bgus.data,mission.data.4,by="Patient_ID")

#Log transforming the reads "https://bookdown.org/rwnahhas/IntroToR/transform-a-numeric-variable.html"

hist(bgus.data.2$BGUS_reads_adjusted)

bgus.data.2$log_BGUS_reads_adjusted <- log(bgus.data.2$BGUS_reads_adjusted)

bgus.data.2$log2_BGUS_reads_adjusted <- log2(bgus.data.2$BGUS_reads_adjusted)

#also testing centered log ratios (https://stats.stackexchange.com/questions/305965/can-i-use-the-clr-centered-log-ratio-transformation-to-prepare-data-for-pca)
#Need to install the compositions package (https://search.r-project.org/CRAN/refmans/compositions/html/clr.html)
#bgus.data$clr_BGUS_reads_adjusted <- clr(bgus.data$BGUS_reads_adjusted)

hist(bgus.data.2$log_BGUS_reads_adjusted)
hist(bgus.data.2$log2_BGUS_reads_adjusted)
hist(bgus.data.2$clr_BGUS_reads_adjusted)

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
bgus.data.ps<-bgus.data.2%>%
  filter(cohort %in% c("PROS"))

bgus.data.cs<-bgus.data.2%>%
  filter(cohort %in% c("CS"))

#Checking correlation overall with EHR
#https://stats.stackexchange.com/questions/153937/how-to-compute-a-p-value-for-the-pearson-correlation-in-r

names(bgus.data.2)

cor.test(bgus.data.2$MPA_AUC5.12_0.12,bgus.data.2$BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_AUC5.12_0.12,bgus.data.2$log_BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_AUC5.12_0.12,bgus.data.2$log2_BGUS_reads_adjusted)

cor.test(bgus.data.2$MPA_AUC6.12_0.12,bgus.data.2$BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_AUC6.12_0.12,bgus.data.2$log_BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_AUC6.12_0.12,bgus.data.2$log2_BGUS_reads_adjusted)

cor.test(bgus.data.2$MPA_AUC_0_12_dose_adj,bgus.data.2$BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_AUC_0_12_dose_adj,bgus.data.2$log_BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_AUC_0_12_dose_adj,bgus.data.2$log2_BGUS_reads_adjusted)

cor.test(bgus.data.2$MPA_AUC_5_12_dose_adj,bgus.data.2$BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_AUC_5_12_dose_adj,bgus.data.2$log_BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_AUC_5_12_dose_adj,bgus.data.2$log2_BGUS_reads_adjusted)

cor.test(bgus.data.2$MPA_AUC_6_12_dose_adj,bgus.data.2$BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_AUC_6_12_dose_adj,bgus.data.2$log_BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_AUC_6_12_dose_adj,bgus.data.2$log2_BGUS_reads_adjusted)

cor.test(bgus.data.2$MPAG_AUC_0_12_dose_adj,bgus.data.2$BGUS_reads_adjusted)
cor.test(bgus.data.2$MPAG_AUC_0_12_dose_adj,bgus.data.2$log_BGUS_reads_adjusted)
cor.test(bgus.data.2$MPAG_AUC_0_12_dose_adj,bgus.data.2$log2_BGUS_reads_adjusted)

cor.test(bgus.data.2$MPAG_AUC_5_12_dose_adj,bgus.data.2$BGUS_reads_adjusted)
cor.test(bgus.data.2$MPAG_AUC_5_12_dose_adj,bgus.data.2$log_BGUS_reads_adjusted)
cor.test(bgus.data.2$MPAG_AUC_5_12_dose_adj,bgus.data.2$log2_BGUS_reads_adjusted)

cor.test(bgus.data.2$MPAG_AUC_6_12_dose_adj,bgus.data.2$BGUS_reads_adjusted)
cor.test(bgus.data.2$MPAG_AUC_6_12_dose_adj,bgus.data.2$log_BGUS_reads_adjusted)
cor.test(bgus.data.2$MPAG_AUC_6_12_dose_adj,bgus.data.2$log2_BGUS_reads_adjusted)

cor.test(bgus.data.2$MPA_EHR_Peaks_count,bgus.data.2$BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_EHR_Peaks_count,bgus.data.2$log_BGUS_reads_adjusted)
cor.test(bgus.data.2$MPA_EHR_Peaks_count,bgus.data.2$log2_BGUS_reads_adjusted)

cor.test(bgus.data.2$MPA_EHR_Peaks_count,bgus.data.2$BGUS_reads_adjusted)

#cor.test(bgus.data$MPA_AUC5.12_0.12,bgus.data$clr_BGUS_reads_adjusted)
#bgus.data$B0VKX9_gusA_adjusted <-bgus.data$B0VKX9_gusA_EeGUS_Eubacterium_eligens_ATCC_27750/bgus.data$Total_reads
#cor.test(bgus.data$MPA_AUC5.12_0.12,bgus.data$B0VKX9_gusA_adjusted) 

cor.test(bgus.data.ps$MPA_AUC5.12_0.12,bgus.data.ps$BGUS_reads_adjusted)
cor.test(bgus.data.ps$MPA_AUC5.12_0.12,bgus.data.ps$log_BGUS_reads_adjusted)
cor.test(bgus.data.ps$MPA_AUC5.12_0.12,bgus.data.ps$log2_BGUS_reads_adjusted)

cor.test(bgus.data.cs$MPA_AUC5.12_0.12,bgus.data.cs$BGUS_reads_adjusted)
cor.test(bgus.data.cs$MPA_AUC5.12_0.12,bgus.data.cs$log_BGUS_reads_adjusted)
cor.test(bgus.data.cs$MPA_AUC5.12_0.12,bgus.data.cs$log2_BGUS_reads_adjusted)

cor.test(bgus.data.ps$MPA_AUC5.12_0.12,bgus.data.ps$MPA_EHR_Peaks_count)
cor.test(bgus.data.cs$MPA_AUC5.12_0.12,bgus.data.cs$MPA_EHR_Peaks_count)
cor.test(bgus.data.2$MPA_AUC5.12_0.12,bgus.data.2$MPA_EHR_Peaks_count)

#Linear model building (https://www.rdocumentation.org/packages/stats/versions/3.6.2/topics/lm)
#summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ log_BGUS_reads_adjusted, data=bgus.data, subset = Collection %in% c(1)))

summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ BGUS_reads_adjusted, data=bgus.data))
summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ BGUS_reads_adjusted + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data))

summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ log_BGUS_reads_adjusted, data=bgus.data))
summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ log_BGUS_reads_adjusted + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data))

summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ log2_BGUS_reads_adjusted, data=bgus.data))
summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ log2_BGUS_reads_adjusted + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data))

summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ BGUS_reads_adjusted, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ BGUS_reads_adjusted + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))

summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ log_BGUS_reads_adjusted, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ log_BGUS_reads_adjusted + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))

summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ log2_BGUS_reads_adjusted, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data$MPA_AUC5.12_0.12 ~ log2_BGUS_reads_adjusted + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))

summary(lm(bgus.data$BGUS_reads_adjusted ~ EHRquint, data=bgus.data))
summary(lm(bgus.data$BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data))

summary(lm(bgus.data$log_BGUS_reads_adjusted ~ EHRquint, data=bgus.data))
summary(lm(bgus.data$log_BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data))

summary(lm(bgus.data$log2_BGUS_reads_adjusted ~ EHRquint, data=bgus.data))
summary(lm(bgus.data$log2_BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data))

summary(lm(bgus.data$BGUS_reads_adjusted ~ EHRquint, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data$BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))

summary(lm(bgus.data$log_BGUS_reads_adjusted ~ EHRquint, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data$log_BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))

summary(lm(bgus.data$log2_BGUS_reads_adjusted ~ EHRquint, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data$log2_BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data, subset = EHRquint %in% c("Q1","Q5")))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ EHRquint, data=bgus.data.ps, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.ps, subset = EHRquint %in% c("Q1","Q5")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ EHRquint, data=bgus.data.ps, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.ps, subset = EHRquint %in% c("Q1","Q5")))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ EHRquint, data=bgus.data.ps, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.ps, subset = EHRquint %in% c("Q1","Q5")))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ EHRquint, data=bgus.data.cs, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.cs, subset = EHRquint %in% c("Q1","Q5")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ EHRquint, data=bgus.data.cs, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.cs, subset = EHRquint %in% c("Q1","Q5")))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ EHRquint, data=bgus.data.cs, subset = EHRquint %in% c("Q1","Q5")))
summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ EHRquint + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.cs, subset = EHRquint %in% c("Q1","Q5")))

#https://forum.biobakery.org/t/questions-about-choosing-analysis-method-masslin3/7907
#Testing other PK parameters

summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPA_AUC_0_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPA_AUC_0_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPA_AUC_0_12_dose_adj + cohort, data=bgus.data.2))

summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPA_AUC_5_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPA_AUC_5_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPA_AUC_5_12_dose_adj + cohort, data=bgus.data.2))

summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPA_AUC_6_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPA_AUC_6_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPA_AUC_6_12_dose_adj + cohort, data=bgus.data.2))

summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPAG_AUC_0_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPAG_AUC_0_12_dose_adj + cohort, data=bgus.data.2))

summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPAG_AUC_5_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPAG_AUC_5_12_dose_adj + cohort, data=bgus.data.2))

summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPAG_AUC_6_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPAG_AUC_6_12_dose_adj + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPAG_AUC_6_12_dose_adj + cohort, data=bgus.data.2))

#Peaks

summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.2))
summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.2))

summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.2))
summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.2))

summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.2))
summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort, data=bgus.data.2))
summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.2))

summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.2, subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort, data=bgus.data.2, subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(bgus.data.2$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.2, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.2, subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort, data=bgus.data.2, subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(bgus.data.2$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.2, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.2, subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort, data=bgus.data.2, subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(bgus.data.2$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count + cohort + donar_status + predom_race + gender + age_PK, data=bgus.data.2, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Stratified analyses, PS

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Stratified analyses, CS

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##Plotting 2 variables side by sides
#https://stackoverflow.com/questions/67536892/how-to-plot-two-histograms-of-different-variables-in-one-ggplot-with-legend-and
#https://www.sthda.com/english/wiki/ggplot2-axis-scales-and-transformations
#https://stackoverflow.com/questions/63003022/change-scale-on-x-axis-in-ggplot-in-r

names(bgus.data.2)

#bgus.data.2$Uniref90_Beta_glucuronidase_cpm <- bgus.data.2$3_2_1_31_Beta_glucuronidase
#bgus.data.2$Blast_Beta_glucuronidase_cpm <- bgus.data.2$Blast_BGUS_cpm

TwoHistos <- ggplot (bgus.data.2) +
  labs(color="Variable name",x="BGUS transcripts in Copies per Million", y="Number of Samples")+
  geom_histogram(aes(x=Uniref90_Beta_glucuronidase_cpm, fill= "Uniref90_Beta_glucuronidase_cpm"),  alpha = 0.2 ) + 
  geom_histogram(aes(x=Blast_Beta_glucuronidase_cpm, fill= "Blast_Beta_glucuronidase_cpm"), alpha = 0.2) + 
  scale_fill_manual(values = c("blue","red"))
TwoHistos 

TwoHistos + scale_x_continuous(n.breaks = 5)

summary(bgus.data.2$Uniref90_Beta_glucuronidase_cpm)
summary(bgus.data.2$Blast_Beta_glucuronidase_cpm)

#Pearson correlation figures for the abstract

cor.test(bgus.data.ps$BGUS_reads_adjusted,bgus.data.ps$MPA_EHR_Peaks_count)
cor.test(bgus.data.ps$MPA_AUC5.12_0.12,bgus.data.ps$MPA_EHR_Peaks_count)
cor.test(bgus.data.ps$MPA_AUC5.12_0.12,bgus.data.ps$BGUS_reads_adjusted)

#https://scientificallysound.org/2016/04/04/r-pearson-correlation/

scatter_plot <- ggplot(bgus.data.ps, aes(bgus.data.ps$BGUS_reads_adjusted, bgus.data.ps$MPA_EHR_Peaks_count))
scatter_plot

scatter_plot + geom_point() + labs(x = "βGUS transcripts", y = "Number of EHR peaks") + geom_smooth(method="lm")
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
