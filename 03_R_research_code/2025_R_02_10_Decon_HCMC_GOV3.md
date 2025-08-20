---
title: "Decon_Default Cell Matrix_12312024"
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

#install.packages("GeomxTools")
#install.packages("tidyverse")
#install.packages("readxl")
```

## Load data {.hidden .unlisted .unnumbered}

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#setwd("/Users/Scottiejj/Desktop/Spatial RoI/")
#setwd("/Users/Scottiejj/Desktop/Spatial RoI/")
setwd("/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/Analysis_GO/")
library(SpatialDecon)
library(GeomxTools)
library(tidyverse)
library(readxl)
library(dplyr)
library(ggplot2)
library(ggpubr)
```

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
```

## Run Decon {.hidden .unlisted .unnumbered}

The spatialdecon function 

The spatialdecon function takes 3 arguments of expression data:

The normalized data.
A matrix of expected background for all data points in the normalized data matrix.
Optionally, either a matrix of per-data-point weights, or the raw data, which is used to derive weights (low counts are less statistically stable, and this allows spatialdecon to down-weight them.) In the parts below this is discussed as the safeTME matrices or even with the options on creating a custom matrix profile

We estimate each data point’s expected background from the negative control probes from its corresponding observation:

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#There should be a column for negative probe. Use rownames instead of names as it is a matrix
#names(norm)
#Notice that the third tab which has the normalized data does not have a negative probe row, which is why it was estimated in the next step below.

rownames(reduced.data)

# and define a background matrix in which each column (observation) is the
# appropriate value of per-observation background:
#The sweep function is use to add a certain value to all columns or row in a matrix #(https://www.r-bloggers.com/2022/07/how-to-use-the-sweep-function-in-r/)

#Create background matrix for negative control probes to derive weights
bg= sweep(reduced.data * 0, 2, rep(1,dim(reduced.data)[2]), "+")
rownames(bg)<-genes

#Run SpatialDecon using default cell profile matrix
res = spatialdecon(norm = reduced.data,
                   bg = bg,
                   align_genes = T)

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
```

#Prepping the dataset to plot results (UMN only, ROI# = 264) {.hidden .unlisted .unnumbered}

This dataset only includes UMN observations (all PIDs start with 3 after filtering, because the data.notes file was retricted to those observations based on the slide indicator variable in the second tab of the spreadsheet)

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#The res object is a collection of smaller datasets from the spatial deconvolution. 
#What deconvolution does is linking the cell types from the spatial data on the slide to the genetic data within each ROI.
#So what we get are datasets where the rows are the cell types, and the columns are the ROI or samples in this case.
#Here we focus on the cell proportions (prop_of_all)

#Got a knitr error here (https://forum.posit.co/t/knit-error-output-to-word-error-in-check-for-xquartz-x11-library-is-missing-install-xquartz-from-xquartz-macosforge-org/18907)

view(res$prop_of_all)
cells<-rownames(res$prop_of_all)
prop=data.frame(t(res$prop_of_all))

#Saving the ROI names for merging
prop$SegmentDisplayName<-rownames(prop)
View(prop)

```

#Plot results of Default Cell Profile Matrix (Cell type comparisons by Glom) {.hidden .unlisted .unnumbered}

*: p <= 0.05

**: p <= 0.01

***: p <= 0.001

****: p <= 0.0001

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#So what we get are datasets where the rows are the cell types, and the columns are the ROI or samples in this case.
#Here we focus on the cell proportions (prop_of_all)
#We transpose that dataset so that the ROI segments are in rows and then the cell types are in columns. 
#Then we add specific variables from the data.notes annotation file so we can make figures by merging based on ROI segments in rows
#Finally add the graft failure information based on the PID variable from the gf.data derived from the clinical dataset

cells<-rownames(res$prop_of_all)
prop=data.frame(t(res$prop_of_all))
prop$SegmentDisplayName<-rownames(prop)
data.notes<-data.notes%>%select(SegmentDisplayName,Glom,Slide,PID,ROI,`CD45+`,`CD68+`)  
prop<-merge(data.notes,prop,by="SegmentDisplayName")
prop<-merge(gf.data,prop,by="PID")

#reshape for boxplot 
prop_long<-prop%>%pivot_longer(cols = all_of(cells), names_to = "Cell", values_to = "Value")
prop_long<-prop_long%>%mutate(Glom=as.factor(prop_long$Glom))

##Group by cell and run anova on Glom status
#Skip this step here and review it in the next code chunk

#prop_long_p<-prop_long%>%pivot_wider(names_from = Cell,values_from = Value)
#anova_results <- lapply(prop_long_p[, c(cells)], function(x) {
#  anova_result <- aov(x ~ prop_long_p$Glom)
#  summary(anova_result)[[1]][["Pr(>F)"]][1]
#})%>%as.data.frame()
#anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
#prop_long<-merge(prop_long,anova_results,by="Cell")


#label x-axis cells with p-value
#ggplot(prop_long, aes(x = Cell, y = Value, fill = Glom)) +
#  geom_boxplot(position = position_dodge(0.9)) +
#  labs(title = "Cell Type Comparisons by Glom, Default cell matrix",
#       x = "Cell Type",
#       y = "Value") +
# theme_minimal() +
#  theme(axis.text.x = element_text(angle = 90, hjust = 1))+
#  stat_compare_means(method = "anova", label = "p.signif", 
#                     label.y = max(prop_long$Value) + 0.5) 

```

#Table of anova results (Default cell matrix, Glom status) {.hidden .unlisted .unnumbered}

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#Make this a hidden R chunk

#So what we get are datasets where the rows are the cell types, and the columns are the ROI or samples in this case.
#Here we focus on the cell proportions (prop_of_all)
#We transpose that dataset so that the ROI segments are in rows and then the cell types are in columns. 
#Then we add specific variables from the data.notes annotation file so we can make figures by merging based on ROI segments in rows
#Finally add the graft failure information based on the PID variable from the gf.data derived from the clinical dataset

cells<-rownames(res$prop_of_all)
prop=data.frame(t(res$prop_of_all))
prop$SegmentDisplayName<-rownames(prop)
data.notes<-data.notes%>%select(SegmentDisplayName,Glom,Slide,PID,ROI,`CD45+`,`CD68+`)  
prop<-merge(data.notes,prop,by="SegmentDisplayName")
prop<-merge(gf.data,prop,by="PID")

#reshape for boxplot 
prop_long<-prop%>%pivot_longer(cols = all_of(cells), names_to = "Cell", values_to = "Value")
prop_long<-prop_long%>%mutate(Glom=as.factor(prop_long$Glom))

##Group by cell and run anova on Glom status
#The code here runs an anova against each category of x (here being cell type) by  glom vs non glom phenoptype
#This steps seems to be useful in generating the mean values for cell types in each ROI, irrespective of phenotype? (which is used in combination with the stat_compare_means function below)?? We don't use it again for the kidney specific cell matrix (will need to review this code for myself)

view(res$prop_of_all)
view(prop)
view(prop_long)

prop_long_p<-prop_long%>%pivot_wider(names_from = Cell,values_from = Value) #We need to turn it back to wide with covariate data

view(prop_long_p)

#Since we previously created an R vector with "cells<-rownames(res$prop_of_all)", we can pass this as the list of variable names with L apply which applies a function over a list of vectors (https://www.rdocumentation.org/packages/base/versions/3.6.2/topics/lapply)

anova_results <- lapply(prop_long_p[, c(cells)], function(x) {
  anova_result <- aov(x ~ prop_long_p$Glom)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()

view(anova_results)

#These last steps are just testing merging the p-values from the anova table with the original dataset frame
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
prop_long<-merge(prop_long,anova_results,by="Cell")
```

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#Full anova table put together

prop_long_p<-prop_long%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_long_p[, c(cells)], function(x) {
  anova_result <- aov(x ~ prop_long_p$Glom)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

```

#Plotting Cell Type Comparisons by Glom (ROI # = 264) {.hidden .unlisted .unnumbered}

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#Matching the ROI names to the data in the annotation datasets
#data.notes<-data.notes%>%select(SegmentDisplayName,Glom,Slide,PID,ROI,`CD45+`,`CD68+`)  ##264 people 
#prop<-merge(data.notes,prop,by="SegmentDisplayName")
#prop<-merge(gf.data,prop,by="PID")

#View(prop)

#reshape for boxplot (using tidyR)
#prop_long<-prop%>%pivot_longer(cols = all_of(cells), names_to = "Cell", values_to = "Value")
#prop_long<-prop_long%>%mutate(Glom=as.factor(prop_long$Glom),GF=as.factor(prop_long$GF))

#ggplot(prop_long, aes(x = Cell, y = Value, fill = Glom)) +
#  geom_boxplot(position = position_dodge(0.9)) +
#  labs(title = "Cell Type Comparisons by Glom",
#       x = "Cell Type",
#       y = "Value") +
#  theme_minimal() +
#  theme(axis.text.x = element_text(angle = 90, hjust = 1)) 

```

#Plotting Cell Type Comparisons by Graft failure status (ROI # = 264) {.hidden .unlisted .unnumbered}

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#ggplot(prop_long, aes(x = Cell, y = Value, fill = GF)) +
#  geom_boxplot(position = position_dodge(0.9)) +
#  labs(title = "Cell Type Comparisons by Graft Failure",
#       x = "Cell Type",
#       y = "Value") +
#  theme_minimal() +
#  theme(axis.text.x = element_text(angle = 90, hjust = 1)) 

```

#Percentage of each Cell Type by Glom status (ROI # = 264) {.hidden .unlisted .unnumbered}

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE


###mean by group
prop_glom<-prop%>%group_by(Glom)%>%summarise_at(.vars=cells,mean)

prop_glom_long <- prop_glom %>%
  pivot_longer(cols = -c(Glom), names_to = "Gene", values_to = "Mean")
```

## Decon analysis 12/31/2024

Install additional packages

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE
#https://rpkgs.datanovia.com/ggpubr/ (publication ready ggplot packages)
#Sometimes R markdown doesn't like installing packages (https://forum.posit.co/t/cannnot-knit-r-markdown/23456/2)
#So it is better to comment out this step
#install.packages("ggpubr")

#Jan2025: Add the kable option to neatly print the output
#https://stackoverflow.com/questions/33319457/display-a-data-frame-as-table-in-r-markdown
#install.packages("kableExtra")

```

# Load necessary packages

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#setwd("/Users/Scottiejj/Desktop/Spatial RoI/")
setwd("/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/Analysis_GO/")
library(SpatialDecon)
library(GeomxTools)
library(tidyverse)
library(readxl)
library(dplyr)
library(ggplot2)
library(ggpubr)
library(knitr)
library(kableExtra)
```

# Load the data {.hidden .unlisted .unnumbered}

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

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
```

## Run Decon, Default Gene Expression Matrix, and decon, kidney cell expression specific matrix, not stratified by Glom

https://github.com/Nanostring-Biostats/CellProfileLibrary/blob/master/Human/Human_datasets_metadata.csv

The spatialdecon function takes 3 arguments of expression data:

The normalized data.
A matrix of expected background for all data points in the normalized data matrix.
Optionally, either a matrix of per-data-point weights, or the raw data, which is used to derive weights (low counts are less statistically stable, and this allows spatialdecon to down-weight them.) In the parts below this is discussed as the safeTME matrices or even with the options on creating a custom matrix profile

We estimate each data point’s expected background from the negative control probes from its corresponding observation:

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

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
```

# Data prep Default Cell Profile Matrix (Cell type comparisons by Glom) {.hidden .unlisted .unnumbered}
*: p <= 0.05

**: p <= 0.01

***: p <= 0.001

****: p <= 0.0001

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

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

```

## Plot results of Default Cell Profile Matrix (Cell type comparisons by Glom)

*: p <= 0.05

**: p <= 0.01

***: p <= 0.001

****: p <= 0.0001

```{r}
#| echo = FALSE, 
#| warning=FALSE

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

# Table of anova results (Default cell Matrix, Glom vs Non Glom)

```{r}
#| echo = FALSE, 
#| warning=FALSE

prop_long_p<-prop_long%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_long_p[, c(cells)], function(x) {
  anova_result <- aov(x ~ prop_long_p$Glom)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

#Checking how to pull out the estimate & p-value from the anova model for interpretation
#https://blog.minitab.com/en/adventures-in-statistics-2/understanding-analysis-of-variance-anova-and-the-f-test

#view(prop_long_p)
#anova_result_2 <- aov(prop_long_p$macrophages ~ prop_long_p$Glom)
#summary(anova_result_2)
#summary(anova_result_2)[[1]][["Pr(>F)"]][1]
#summary(anova_result_2)[[1]][["Mean Sq"]][1]
#summary(anova_result_2)[[1]][["Mean Sq","F value","Pr(>F)"]][1]
#summary(anova_result_2)[[1]][["Mean Sq","F value","Pr(>F)"]][3]

#Plot only the significant results at Dr. Israni's request

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
prop_long_p_sub<-prop_long%>%
  filter(Cell %in% c("macrophages","B.memory","plasma","T.CD4.naive","T.CD8.naive","T.CD8.memory",
                     "pDCs","mDCs","monocytes.C","monocytes.NC.I","neutrophils","Treg","edothelial.cells","fibroblasts"))

ggplot(prop_long_p_sub, aes(x = Cell, y = Value, fill = as.factor(Glom))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons, Glom vs NonGlom tissues, statistically significant results",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_long_p_sub$Value) + 0.5) 

#Making a table comparing the proportion of cell types for significant results

#Since the F result is not an "effect estimate" I will make descriptive tables for the mean per group and pvalue where possible 
#One option would be "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" but there are no examples handling continuous variables there
#Maybe use '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"?

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
prop_long_p_sub$glom_status[prop_long_p_sub$Glom == 0] <- "NoGlom"
prop_long_p_sub$glom_status[prop_long_p_sub$Glom == 1] <- "Glom"

#view(prop_long_p_sub)

# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
prop_long_p_sub$cell_type = paste0(prop_long_p_sub$Cell,".",prop_long_p_sub$glom_status) 
#view(prop_long_p_sub)

#based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"

Cellsummary <- prop_long_p_sub %>%
  dplyr::select(-Glom, -Slide) %>%
  dplyr::group_by(glom_status, Cell) %>%
  dplyr::summarise(MeanCell= mean(Value, na.rm=TRUE),
                   SeCell = sd(Value, na.rm=TRUE)/sqrt(n()),
                   Count = n())

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#Cellsummary$glom_status[Cellsummary$Glom == 0] <- "NoGlom"
#Cellsummary$glom_status[Cellsummary$Glom == 1] <- "Glom"
  
# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
#Cellsummary$Cell_type = paste0(Cellsummary$Cell,".",Cellsummary$glom_status) 

#Based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" to pivot so the case and control groups are side by side
Cellsummary2 <- Cellsummary %>%
  pivot_wider (names_from = glom_status,
               values_from = c(MeanCell, SeCell, Count)) 

view(Cellsummary2)
knitr::kable(Cellsummary2, format = "html")

#It is still staggered, check later #"https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html"

#Cellsummary2 <- Cellsummary %>%
#  dplyr::select(-Glom, -Cell) %>%
#  pivot_wider (names_from = Glom,
#               values_from = c(Cell_type))

```

## DEC2024: Loading data for race, AR status to rerun analyses for GF vs no GF, race and AR stratified by Glom status

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#Glom data
#remove rows with missing Glom or Slide
#data.notes<-read_excel("/Users/Scottiejj/Desktop/Spatial RoI/Q3 for R with notes_updated .xlsx",sheet = 2)%>%filter(!is.na(Glom)) 
#This data is the equivalent of the annotation dataset in the deconvolution tutorials
data.notes<-read_excel("00_Q3 for R with notes_updated .xlsx",sheet = 2)%>%filter(!is.na(Glom)) #N=557 ROI
#exclude PID starts with 4 (Hennepin) (This is not equivalent to removing the slide variable although none of the Hennepin observations had slide = 1)
data.notes2<-data.notes%>%filter(!grepl("^4",PID))  #N = 363 RoI

#Checking frequencies
#https://www.geeksforgeeks.org/frequency-table-in-r/
#https://cran.r-project.org/web/packages/vcdExtra/vignettes/creating.html

freq <- table(data.notes2$PID, data.notes2$Slide)
freq
dim(freq) #28 unique PIDs across 7 slides

freq <- table(data.notes$PID, data.notes$Slide)
freq
dim(freq) #36 unique PIDs across 7 slides

#GF data
#gf.data<-read_xlsx("/Users/Scottiejj/Desktop/Spatial RoI/clinical data for biopsies, 6-18-2024.xlsx",sheet = 1)
gf.data4<-read_xlsx("00_clinical data for biopsies, 6-18-2024.xlsx",sheet = 1)

#DEC2024: Casey got a new clinical dataset for biopsies ('/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/david analysis 10-29-2024/cgd_demo_data_20241029.csv')
#I moved it to ('/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/Analysis_GO/00_cgd_demo_data_20241029.xlsx'). N=47

gf.data5<-read_xlsx("00_cgd_demo_data_20241029.xlsx",sheet = 'cgd_demo_data_20241029')
dim(gf.data5) #N=47

freq.data5 <- table(gf.data5$pid)
freq.data5

#Since the PIDs had some typos and changes in data.notes, I manually made changes in excel and reimported the file (Casey requested I remove the repeated observations 4058.2 and 40058.3)
gf.data<-read_xlsx("00_cgd_demo_data_20241029_2.xlsx",sheet = 'cgd_demo_data_20241029')
dim(gf.data) #N=47

#renaming the pid column and then subsetting the dataset to only the PID and graft failure variables
#names(gf.data)[1]<-"PID"
names(gf.data)[2]<-"PID"
#gf.data<-gf.data%>%dplyr::rename(GF=graftfail_usrds)%>%select(PID,GF)%>%mutate(GF=as.factor(GF))

##Indicate Acute Rejection, and race and toxicity in dataset
gf.data$AR_Cellular<-ifelse(stringr::str_detect(gf.data$DIAGPRI,"Acute Rejection - Cellular"),no=ifelse(stringr::str_detect(gf.data$DIAGPRI,"No abnormalities"),no = NA,yes=0),yes=1)
gf.data$AR_AM<-ifelse(stringr::str_detect(gf.data$DIAGPRI,"Acute Rejection - Antibody Mediated"),no=ifelse(stringr::str_detect(gf.data$DIAGPRI,"No abnormalities"),no = NA,yes=0),yes=1)
gf.data$Toxicity<-ifelse(stringr::str_detect(gf.data$DIAGPRI,"Toxicity"),no=ifelse(stringr::str_detect(gf.data$DIAGPRI,"No abnormalities"),no = NA,yes=0),yes=1)

#Ajay requested we compare AR_Cellular to AMR directly)

gf.data$ACR_AMR[gf.data$DIAGPRI == "Acute Rejection - Cellular"] <- "ACR"
gf.data$ACR_AMR[gf.data$DIAGPRI == "Acute Rejection - Antibody Mediated"] <- "AMR"

gf.data$ACR_AMR_ind[gf.data$DIAGPRI == "Acute Rejection - Cellular"] <- 1
gf.data$ACR_AMR_ind[gf.data$DIAGPRI == "Acute Rejection - Antibody Mediated"] <- 2

gf.data<-gf.data%>%dplyr::rename(GF=graftfail_usrds)%>%select(PID,GF,AR_Cellular,AR_AM,Toxicity,black,ACR_AMR,ACR_AMR_ind)%>%
  mutate(GF=as.factor(GF),black=as.factor(black),AR_Cellular=as.factor(AR_Cellular),
         AR_AM=as.factor(AR_AM),Toxicity=as.factor(Toxicity),ACR_AMR_ind=as.factor(ACR_AMR_ind))

dim(gf.data) #N=47

#Checking frequencies
#https://www.geeksforgeeks.org/frequency-table-in-r/
#https://cran.r-project.org/web/packages/vcdExtra/vignettes/creating.html

freq <- table(gf.data$PID, gf.data$black)
freq
dim(freq) #47 unique PIDs

#Gene expression data
#reduced.data<-readxl::read_excel("/Users/Scottiejj/Desktop/Spatial RoI/Dorr q3-norm reduced.xlsx",sheet = 3)
#This dataset is the equivalent to the normalized dataset in the deconvolution tutorials (specifically the third tab in the spreadsheet) The rows are each gene and each ROI is in a column
reduced.data<-readxl::read_excel("00_Dorr q3-norm reduced.xlsx",sheet = 3)%>%as.matrix()
dim(reduced.data) #593 ROIs

#Here we are removing the first column which contains the gene names, then transposing the columns(each ROI segment) to rows so it can be merged with the data notes, which is the equivalent of the annotation dataset
genes<-unlist(reduced.data[,1])
reduced.data<-reduced.data[,-1]%>%as.matrix()
reduced.data<-data.frame(t(reduced.data[,-1]%>%as.matrix()))%>%rownames_to_column(var = "SegmentDisplayName")%>%inner_join(data.notes,by="SegmentDisplayName")
ROI<-reduced.data$SegmentDisplayName

#I think this undoes the data transformation above so we end back with genes in rows and ROIs in columns. However with the merging step we have filtered the dataset to match the annotation dataset as seen in the dim command
reduced.data<-reduced.data%>%select(-c(names(data.notes)))%>%as.matrix()%>%t()
rownames(reduced.data)<-genes
colnames(reduced.data)<-ROI
dim(reduced.data) #556 RoIs, since we kept the HCMC data this time around from data.notes (but now 518 after dropping 4058.2 and 340058.3)

```

# Run Decon, Stratified by Glom,Default Matrix

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#Gene expression data
#This dataset is the equivalent to the normalized dataset in the deconvolution tutorials (specifically the third tab in the spreadsheet) The rows are each gene and each ROI is in a column
#reduced.data<-readxl::read_excel("/Users/Scottiejj/Desktop/Spatial RoI/Dorr q3-norm reduced.xlsx",sheet = 3)

reduced.data<-readxl::read_excel("00_Dorr q3-norm reduced.xlsx",sheet = 3)%>%as.matrix()
dim(reduced.data) #593 ROIs

#Here we are removing the first column which contains the gene names, then transposing the columns(each ROI segment) to rows so it can be merged with the data notes, which is the equivalent of the annotation dataset
genes<-unlist(reduced.data[,1])
reduced.data<-reduced.data[,-1]%>%as.matrix()
reduced.data<-data.frame(t(reduced.data[,-1]%>%as.matrix()))%>%rownames_to_column(var = "SegmentDisplayName")%>%inner_join(data.notes,by="SegmentDisplayName")
dim(reduced.data)  #556 RoIs, since we kept the HCMC data this time around from data.notes (but now 518 after dropping 4058.2 and 340058.3)

#Stratifying the data based on glom phenotype (Stratify before merging with data.notes because at that point we change it to a matrix which cannot be filtered)

reduced.data.glom<-reduced.data%>%filter(Glom==1)
ROI_glom<-reduced.data.glom$SegmentDisplayName
reduced.data.glom<-reduced.data.glom%>%select(-c(names(data.notes)))%>%as.matrix()%>%t()
rownames(reduced.data.glom)<-genes
colnames(reduced.data.glom)<-ROI_glom
dim(reduced.data.glom) #155 ROIs

reduced.data.nonglom<-reduced.data%>%filter(Glom==0)
ROI_nonglom<-reduced.data.nonglom$SegmentDisplayName
reduced.data.nonglom<-reduced.data.nonglom%>%select(-c(names(data.notes)))%>%as.matrix()%>%t()
rownames(reduced.data.nonglom)<-genes
colnames(reduced.data.nonglom)<-ROI_nonglom
dim(reduced.data.nonglom) #401 ROIs

#Here, we are extracting the names in the first column as a vector, forcing all the other observations to be numeric (foe the normalized gene abundance counts) and finally adding back the gene names contained in the first column

#Since we filtered the observations here down to 363 RoIs, we need to reapply the numeric conversion of the matrix before running the negative background generation step
#genes<-reduced.data[,1]
#reduced.data.test<-reduced.data[,-1]
reduced.data.glom<-apply(reduced.data.glom,2,as.numeric)
rownames(reduced.data.glom)<-genes

dim(reduced.data.glom)

#Run Decon
bg.glom= sweep(reduced.data.glom * 0, 2, rep(1,dim(reduced.data.glom)[2]), "+")
rownames(bg.glom)<-genes

res.glom = spatialdecon(norm = reduced.data.glom,
                   bg = bg.glom,
                   align_genes = T)

#Since we filtered the observations here down to 363 RoIs, we need to reapply the numeric conversion of the matrix before running the negative background generation step
#genes<-reduced.data[,1]
#reduced.data.test<-reduced.data[,-1]
reduced.data.nonglom<-apply(reduced.data.nonglom,2,as.numeric)
rownames(reduced.data.nonglom)<-genes

dim(reduced.data.nonglom)

#Run Decon
bg.nonglom= sweep(reduced.data.nonglom * 0, 2, rep(1,dim(reduced.data.nonglom)[2]), "+")
rownames(bg.nonglom)<-genes
res.nonglom = spatialdecon(norm = reduced.data.nonglom,
                   bg = bg.nonglom,
                   align_genes = T)
```

# Plot Default cell matrix, glom, GF

```{r}
#view(res.glom$prop_of_all)
cells_glom<-rownames(res.glom$prop_of_all)
prop_glom=data.frame(t(res.glom$prop_of_all))
prop_glom$SegmentDisplayName<-rownames(prop_glom)
prop_glom<-merge(data.notes,prop_glom,by="SegmentDisplayName")
prop_glom<-merge(gf.data,prop_glom,by="PID")

#reshape for boxplot
prop_glom_long<-prop_glom%>%pivot_longer(cols = all_of(cells_glom), names_to = "Cell", values_to = "Value")

ggplot(prop_glom_long, aes(x = Cell, y = Value, fill = as.factor(GF))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of Glom tissue, Graft Failure vs No Graft Failure",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long$Value) + 0.5) 

```

# Table of anova results, default cell matrix, glom, GF

```{r}
#| echo = FALSE, 
#| warning=FALSE

prop_glom_long_p<-prop_glom_long%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_glom_long_p[, c(cells_glom)], function(x) {
  anova_result <- aov(x ~ prop_glom_long_p$GF)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

#Plot only the significant results at Dr. Israni's request

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
prop_glom_long_sub<-prop_glom_long%>%
  filter(Cell %in% c("macrophages","mast","plasma","neutrophils"))

ggplot(prop_glom_long_sub, aes(x = Cell, y = Value, fill = as.factor(GF))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of Glom tissue, Graft failure vs No Graft Failure, statistically significant results",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long_sub$Value) + 0.5) 

#Making a table comparing the proportion of cell types for significant results

#Since the F result is not an "effect estimate" I will make descriptive tables for the mean per group and pvalue where possible 
#One option would be "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" but there are no examples handling continuous variables there
#Maybe use '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"?

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
prop_glom_long_sub$gf_status[prop_glom_long_sub$GF == 0] <- "No Graft Failure"
prop_glom_long_sub$gf_status[prop_glom_long_sub$GF == 1] <- "Graft Failure"

#view(prop_long_p_sub)

# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
prop_glom_long_sub$cell_type = paste0(prop_glom_long_sub$Cell,".",prop_glom_long_sub$gf_status) 
#view(prop_long_p_sub)

#based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"

Cellsummary <- prop_glom_long_sub %>%
  dplyr::select(-GF, -Slide) %>%
  dplyr::group_by(gf_status, Cell) %>%
  dplyr::summarise(MeanCell= mean(Value, na.rm=TRUE),
                   SeCell = sd(Value, na.rm=TRUE)/sqrt(n()),
                   Count = n())

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#Cellsummary$glom_status[Cellsummary$Glom == 0] <- "NoGlom"
#Cellsummary$glom_status[Cellsummary$Glom == 1] <- "Glom"
  
# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
#Cellsummary$Cell_type = paste0(Cellsummary$Cell,".",Cellsummary$glom_status) 

#Based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" to pivot so the case and control groups are side by side
Cellsummary2 <- Cellsummary %>%
  pivot_wider (names_from = gf_status,
               values_from = c(MeanCell, SeCell, Count)) 

view(Cellsummary2)
knitr::kable(Cellsummary2, format = "html")

#It is still staggered, check later #"https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html"

#Cellsummary2 <- Cellsummary %>%
#  dplyr::select(-Glom, -Cell) %>%
#  pivot_wider (names_from = Glom,
#               values_from = c(Cell_type))

```

# Plot Default cell matrix, nonglom, GF

```{r}

#view(res.nonglom$prop_of_all)
cells_nonglom<-rownames(res.nonglom$prop_of_all)
prop_nonglom=data.frame(t(res.nonglom$prop_of_all))
prop_nonglom$SegmentDisplayName<-rownames(prop_nonglom)
prop_nonglom<-merge(data.notes,prop_nonglom,by="SegmentDisplayName")
prop_nonglom<-merge(gf.data,prop_nonglom,by="PID")

#reshape for boxplot
prop_nonglom_long<-prop_nonglom%>%pivot_longer(cols = all_of(cells_nonglom), names_to = "Cell", values_to = "Value")

ggplot(prop_nonglom_long, aes(x = Cell, y = Value, fill = as.factor(GF))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of nonGlom tissue, Graft Failure vs No Graft Failure",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_nonglom_long$Value) + 0.5) 

```

# Table of anova results, default cell matrix, nonglom, GF

```{r}
#| echo = FALSE, 
#| warning=FALSE

prop_nonglom_long_p<-prop_nonglom_long%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_nonglom_long_p[, c(cells_glom)], function(x) {
  anova_result <- aov(x ~ prop_nonglom_long_p$GF)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

#Plot only the significant results at Dr. Israni's request

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
prop_nonglom_long_sub<-prop_nonglom_long%>%
  filter(Cell %in% c("macrophages","plasma","pDCs","mDCs","monocytes.NC.I","neutrophils",
                     "fibroblasts"))

ggplot(prop_nonglom_long_sub, aes(x = Cell, y = Value, fill = as.factor(GF))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of nonGlom tissue, Graft Failure vs No Graft Failure, statistically significant results",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long_sub$Value) + 0.5) 

#Making a table comparing the proportion of cell types for significant results

#Since the F result is not an "effect estimate" I will make descriptive tables for the mean per group and pvalue where possible 
#One option would be "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" but there are no examples handling continuous variables there
#Maybe use '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"?

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
prop_nonglom_long_sub$gf_status[prop_nonglom_long_sub$GF == 0] <- "No Graft Failure"
prop_nonglom_long_sub$gf_status[prop_nonglom_long_sub$GF == 1] <- "Graft Failure"

#view(prop_long_p_sub)

# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
prop_nonglom_long_sub$cell_type = paste0(prop_nonglom_long_sub$Cell,".",prop_nonglom_long_sub$gf_status) 
#view(prop_long_p_sub)

#based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"

Cellsummary <- prop_nonglom_long_sub %>%
  dplyr::select(-GF, -Slide) %>%
  dplyr::group_by(gf_status, Cell) %>%
  dplyr::summarise(MeanCell= mean(Value, na.rm=TRUE),
                   SeCell = sd(Value, na.rm=TRUE)/sqrt(n()),
                   Count = n())

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#Cellsummary$glom_status[Cellsummary$Glom == 0] <- "NoGlom"
#Cellsummary$glom_status[Cellsummary$Glom == 1] <- "Glom"
  
# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
#Cellsummary$Cell_type = paste0(Cellsummary$Cell,".",Cellsummary$glom_status) 

#Based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" to pivot so the case and control groups are side by side
Cellsummary2 <- Cellsummary %>%
  pivot_wider (names_from = gf_status,
               values_from = c(MeanCell, SeCell, Count)) 

view(Cellsummary2)
knitr::kable(Cellsummary2, format = "html")

#It is still staggered, check later #"https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html"

#Cellsummary2 <- Cellsummary %>%
#  dplyr::select(-Glom, -Cell) %>%
#  pivot_wider (names_from = Glom,
#               values_from = c(Cell_type))

```

# Plot Default cell matrix, glom, race

```{r}
#view(res.glom$prop_of_all)
cells_glom<-rownames(res.glom$prop_of_all)
prop_glom=data.frame(t(res.glom$prop_of_all))
prop_glom$SegmentDisplayName<-rownames(prop_glom)
prop_glom<-merge(data.notes,prop_glom,by="SegmentDisplayName")
prop_glom<-merge(gf.data,prop_glom,by="PID")

#reshape for boxplot
prop_glom_long<-prop_glom%>%pivot_longer(cols = all_of(cells_glom), names_to = "Cell", values_to = "Value")

ggplot(prop_glom_long, aes(x = Cell, y = Value, fill = as.factor(black))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of Glom tissue, AA ancestry vs non-AA ancestry",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long$Value) + 0.5) 

```

# Table of anova results, default cell matrix, glom, race

```{r}
#| echo = FALSE, 
#| warning=FALSE

prop_glom_long_p<-prop_glom_long%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_glom_long_p[, c(cells_glom)], function(x) {
  anova_result <- aov(x ~ prop_glom_long_p$black)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

#Plot only the significant results at Dr. Israni's request

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
prop_glom_long_sub<-prop_glom_long%>%
  filter(Cell %in% c("mDCs","fibroblasts"))

ggplot(prop_glom_long_sub, aes(x = Cell, y = Value, fill = as.factor(black))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of Glom tissue, AA ancestry vs non-AA ancestry, statistically significant results",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long_sub$Value) + 0.5) 

#Making a table comparing the proportion of cell types for significant results

#Since the F result is not an "effect estimate" I will make descriptive tables for the mean per group and pvalue where possible 
#One option would be "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" but there are no examples handling continuous variables there
#Maybe use '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"?

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
prop_glom_long_sub$aa_status[prop_glom_long_sub$black == 0] <- "Non African-American Ancestry"
prop_glom_long_sub$aa_status[prop_glom_long_sub$black == 1] <- "African-American Ancestry"

#view(prop_long_p_sub)

# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
prop_glom_long_sub$cell_type = paste0(prop_glom_long_sub$Cell,".",prop_glom_long_sub$aa_status) 
#view(prop_long_p_sub)

#based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"

Cellsummary <- prop_glom_long_sub %>%
  dplyr::select(-GF, -Slide) %>%
  dplyr::group_by(aa_status, Cell) %>%
  dplyr::summarise(MeanCell= mean(Value, na.rm=TRUE),
                   SeCell = sd(Value, na.rm=TRUE)/sqrt(n()),
                   Count = n())

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#Cellsummary$glom_status[Cellsummary$Glom == 0] <- "NoGlom"
#Cellsummary$glom_status[Cellsummary$Glom == 1] <- "Glom"
  
# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
#Cellsummary$Cell_type = paste0(Cellsummary$Cell,".",Cellsummary$glom_status) 

#Based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" to pivot so the case and control groups are side by side
Cellsummary2 <- Cellsummary %>%
  pivot_wider (names_from = aa_status,
               values_from = c(MeanCell, SeCell, Count)) 

view(Cellsummary2)
knitr::kable(Cellsummary2, format = "html")

#It is still staggered, check later #"https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html"

#Cellsummary2 <- Cellsummary %>%
#  dplyr::select(-Glom, -Cell) %>%
#  pivot_wider (names_from = Glom,
#               values_from = c(Cell_type))

```

# Plot Default cell matrix, nonglom, race

```{r}

#view(res.nonglom$prop_of_all)
cells_nonglom<-rownames(res.nonglom$prop_of_all)
prop_nonglom=data.frame(t(res.nonglom$prop_of_all))
prop_nonglom$SegmentDisplayName<-rownames(prop_nonglom)
prop_nonglom<-merge(data.notes,prop_nonglom,by="SegmentDisplayName")
prop_nonglom<-merge(gf.data,prop_nonglom,by="PID")

#reshape for boxplot
prop_nonglom_long<-prop_nonglom%>%pivot_longer(cols = all_of(cells_nonglom), names_to = "Cell", values_to = "Value")

ggplot(prop_nonglom_long, aes(x = Cell, y = Value, fill = as.factor(black))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of nonGlom tissue, AA ancestry vs non-AA ancestry",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_nonglom_long$Value) + 0.5) 

```

# Table of anova results, default cell matrix, nonglom, race

```{r}
#| echo = FALSE, 
#| warning=FALSE

prop_nonglom_long_p<-prop_nonglom_long%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_nonglom_long_p[, c(cells_glom)], function(x) {
  anova_result <- aov(x ~ prop_nonglom_long_p$black)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

#Plot only the significant results at Dr. Israni's request

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
prop_nonglom_long_sub<-prop_nonglom_long%>%
  filter(Cell %in% c("pDCs","mDCs"))

ggplot(prop_nonglom_long_sub, aes(x = Cell, y = Value, fill = as.factor(black))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of nonGlom tissue, AA ancestry vs non-AA ancestry, statistically significant results",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long_sub$Value) + 0.5)

#Making a table comparing the proportion of cell types for significant results

#Since the F result is not an "effect estimate" I will make descriptive tables for the mean per group and pvalue where possible 
#One option would be "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" but there are no examples handling continuous variables there
#Maybe use '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"?

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
prop_nonglom_long_sub$aa_status[prop_nonglom_long_sub$black == 0] <- "Non African-American Ancestry"
prop_nonglom_long_sub$aa_status[prop_nonglom_long_sub$black == 1] <- "African-American Ancestry"

#view(prop_long_p_sub)

# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
prop_nonglom_long_sub$cell_type = paste0(prop_nonglom_long_sub$Cell,".",prop_nonglom_long_sub$aa_status) 
#view(prop_long_p_sub)

#based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"

Cellsummary <- prop_nonglom_long_sub %>%
  dplyr::select(-GF, -Slide) %>%
  dplyr::group_by(aa_status, Cell) %>%
  dplyr::summarise(MeanCell= mean(Value, na.rm=TRUE),
                   SeCell = sd(Value, na.rm=TRUE)/sqrt(n()),
                   Count = n())

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#Cellsummary$glom_status[Cellsummary$Glom == 0] <- "NoGlom"
#Cellsummary$glom_status[Cellsummary$Glom == 1] <- "Glom"
  
# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
#Cellsummary$Cell_type = paste0(Cellsummary$Cell,".",Cellsummary$glom_status) 

#Based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" to pivot so the case and control groups are side by side
Cellsummary2 <- Cellsummary %>%
  pivot_wider (names_from = aa_status,
               values_from = c(MeanCell, SeCell, Count)) 

view(Cellsummary2)
knitr::kable(Cellsummary2, format = "html")

#It is still staggered, check later #"https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html"

#Cellsummary2 <- Cellsummary %>%
#  dplyr::select(-Glom, -Cell) %>%
#  pivot_wider (names_from = Glom,
#               values_from = c(Cell_type))

```

# Plot Default cell matrix, glom, test diagnosis (No abnormality vs AR_AM)

```{r}
#view(res.glom$prop_of_all)
cells_glom<-rownames(res.glom$prop_of_all)
prop_glom=data.frame(t(res.glom$prop_of_all))
prop_glom$SegmentDisplayName<-rownames(prop_glom)
prop_glom<-merge(data.notes,prop_glom,by="SegmentDisplayName")
prop_glom<-merge(gf.data,prop_glom,by="PID")

#reshape for boxplot

prop_glom_long<-prop_glom%>%pivot_longer(cols = all_of(cells_glom), names_to = "Cell", values_to = "Value")

prop_glom_long_ARAM<-prop_glom_long%>%filter(!is.na(AR_AM))

ggplot(prop_glom_long_ARAM, aes(x = Cell, y = Value, fill = as.factor(AR_AM))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of Glom tissues, Antibody-mediated Acute Rejection vs No Acute Rejection",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long$Value) + 0.5) 

```

# Table of anova results, default cell matrix, glom, Test diagnosis (No abnormality vs AR_AM)

```{r}
#| echo = FALSE, 
#| warning=FALSE

prop_glom_long_p<-prop_glom_long_ARAM%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_glom_long_p[, c(cells_glom)], function(x) {
  anova_result <- aov(x ~ prop_glom_long_p$AR_AM)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

#Plot only the significant results at Dr. Israni's request

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
prop_glom_long_sub<-prop_glom_long_ARAM%>%
  filter(Cell %in% c("mast","plasma","T.CD4.memory","neutrophils"))

ggplot(prop_glom_long_sub, aes(x = Cell, y = Value, fill = as.factor(AR_AM))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of Glom tissue, Antibody-mediated Acute Rejection vs no Acute Rejection, statistically significant results",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long_sub$Value) + 0.5) 

#Making a table comparing the proportion of cell types for significant results

#Since the F result is not an "effect estimate" I will make descriptive tables for the mean per group and pvalue where possible 
#One option would be "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" but there are no examples handling continuous variables there
#Maybe use '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"?

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
prop_glom_long_sub$ar_am_status[prop_glom_long_sub$AR_AM == 0] <- "No Acute Rejection"
prop_glom_long_sub$ar_am_status[prop_glom_long_sub$AR_AM == 1] <- "Antibody-mediated Acute Rejection"

#view(prop_long_p_sub)

# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
prop_glom_long_sub$cell_type = paste0(prop_glom_long_sub$Cell,".",prop_glom_long_sub$ar_am_status) 
#view(prop_long_p_sub)

#based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"

Cellsummary <- prop_glom_long_sub %>%
  dplyr::select(-GF, -Slide) %>%
  dplyr::group_by(ar_am_status, Cell) %>%
  dplyr::summarise(MeanCell= mean(Value, na.rm=TRUE),
                   SeCell = sd(Value, na.rm=TRUE)/sqrt(n()),
                   Count = n())

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#Cellsummary$glom_status[Cellsummary$Glom == 0] <- "NoGlom"
#Cellsummary$glom_status[Cellsummary$Glom == 1] <- "Glom"
  
# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
#Cellsummary$Cell_type = paste0(Cellsummary$Cell,".",Cellsummary$glom_status) 

#Based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" to pivot so the case and control groups are side by side
Cellsummary2 <- Cellsummary %>%
  pivot_wider (names_from = ar_am_status,
               values_from = c(MeanCell, SeCell, Count)) 

view(Cellsummary2)
knitr::kable(Cellsummary2, format = "html")

#It is still staggered, check later #"https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html"

#Cellsummary2 <- Cellsummary %>%
#  dplyr::select(-Glom, -Cell) %>%
#  pivot_wider (names_from = Glom,
#               values_from = c(Cell_type))

```

# Plot Default cell matrix, nonglom, test diagnosis (No abnormality vs AR_AM)

```{r}

#view(res.nonglom$prop_of_all)
cells_nonglom<-rownames(res.nonglom$prop_of_all)
prop_nonglom=data.frame(t(res.nonglom$prop_of_all))
prop_nonglom$SegmentDisplayName<-rownames(prop_nonglom)
prop_nonglom<-merge(data.notes,prop_nonglom,by="SegmentDisplayName")
prop_nonglom<-merge(gf.data,prop_nonglom,by="PID")

#reshape for boxplot
prop_nonglom_long<-prop_nonglom%>%pivot_longer(cols = all_of(cells_nonglom), names_to = "Cell", values_to = "Value")

prop_nonglom_long_ARAM<-prop_nonglom_long%>%filter(!is.na(AR_AM))

ggplot(prop_nonglom_long_ARAM, aes(x = Cell, y = Value, fill = as.factor(AR_AM))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of nonGlom tissues, Antibody-mediated Acute Rejection vs No Acute Rejection",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_nonglom_long$Value) + 0.5) 
```

# Table of anova results, default cell matrix, nonglom, test diagnosis (No abnormality vs AR_AM)

```{r}
#| echo = FALSE, 
#| warning=FALSE

prop_nonglom_long_p<-prop_nonglom_long_ARAM%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_nonglom_long_p[, c(cells_glom)], function(x) {
  anova_result <- aov(x ~ prop_nonglom_long_p$AR_AM)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

#Plot only the significant results at Dr. Israni's request

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
prop_nonglom_long_sub<-prop_nonglom_long_ARAM%>%
  filter(Cell %in% c("macrophages","mast","T.CD4.memory","NK","neutrophils","fibroblasts"))

ggplot(prop_nonglom_long_sub, aes(x = Cell, y = Value, fill = as.factor(AR_AM))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of nonGlom tissue, Antibody-mediated Acute Rejection vs No Acute Rejection, statistically significant results",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long_sub$Value) + 0.5)

#Making a table comparing the proportion of cell types for significant results

#Since the F result is not an "effect estimate" I will make descriptive tables for the mean per group and pvalue where possible 
#One option would be "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" but there are no examples handling continuous variables there
#Maybe use '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"?

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
prop_nonglom_long_sub$ar_am_status[prop_nonglom_long_sub$AR_AM == 0] <- "No Acute Rejection"
prop_nonglom_long_sub$ar_am_status[prop_nonglom_long_sub$AR_AM == 1] <- "Antibody-mediated Acute Rejection"

#view(prop_long_p_sub)

# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
prop_nonglom_long_sub$cell_type = paste0(prop_nonglom_long_sub$Cell,".",prop_nonglom_long_sub$ar_am_status) 
#view(prop_long_p_sub)

#based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"

Cellsummary <- prop_nonglom_long_sub %>%
  dplyr::select(-GF, -Slide) %>%
  dplyr::group_by(ar_am_status, Cell) %>%
  dplyr::summarise(MeanCell= mean(Value, na.rm=TRUE),
                   SeCell = sd(Value, na.rm=TRUE)/sqrt(n()),
                   Count = n())

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#Cellsummary$glom_status[Cellsummary$Glom == 0] <- "NoGlom"
#Cellsummary$glom_status[Cellsummary$Glom == 1] <- "Glom"
  
# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
#Cellsummary$Cell_type = paste0(Cellsummary$Cell,".",Cellsummary$glom_status) 

#Based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" to pivot so the case and control groups are side by side
Cellsummary2 <- Cellsummary %>%
  pivot_wider (names_from = ar_am_status,
               values_from = c(MeanCell, SeCell, Count)) 

view(Cellsummary2)
knitr::kable(Cellsummary2, format = "html")

#It is still staggered, check later #"https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html"

#Cellsummary2 <- Cellsummary %>%
#  dplyr::select(-Glom, -Cell) %>%
#  pivot_wider (names_from = Glom,
#               values_from = c(Cell_type))

```

# Plot Default cell matrix, glom, test diagnosis (No abnormality vs AR_Cellular)

```{r}
#view(res.glom$prop_of_all)
cells_glom<-rownames(res.glom$prop_of_all)
prop_glom=data.frame(t(res.glom$prop_of_all))
prop_glom$SegmentDisplayName<-rownames(prop_glom)
prop_glom<-merge(data.notes,prop_glom,by="SegmentDisplayName")
prop_glom<-merge(gf.data,prop_glom,by="PID")

#reshape for boxplot

prop_glom_long<-prop_glom%>%pivot_longer(cols = all_of(cells_glom), names_to = "Cell", values_to = "Value")

prop_glom_long_ARCellular<-prop_glom_long%>%filter(!is.na(AR_Cellular))

ggplot(prop_glom_long_ARCellular, aes(x = Cell, y = Value, fill = as.factor(AR_Cellular))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of Glom tissue, Acute Cellular Rejection vs No Acute Rejection",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long$Value) + 0.5) 

```

# Table of anova results, default cell matrix, glom, Test diagnosis (No abnormality vs AR_Cellular)

```{r}
#| echo = FALSE, 
#| warning=FALSE

prop_glom_long_p<-prop_glom_long_ARCellular%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_glom_long_p[, c(cells_glom)], function(x) {
  anova_result <- aov(x ~ prop_glom_long_p$AR_Cellular)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

#Plot only the significant results at Dr. Israni's request

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
prop_glom_long_sub<-prop_glom_long_ARCellular%>%
  filter(Cell %in% c("macrophages","neutrophils"))

ggplot(prop_glom_long_sub, aes(x = Cell, y = Value, fill = as.factor(AR_Cellular))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of Glom tissue, Acute Cellular Rejection vs No Acute Rejection, statistically significant results",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long_sub$Value) + 0.5) 

#Making a table comparing the proportion of cell types for significant results

#Since the F result is not an "effect estimate" I will make descriptive tables for the mean per group and pvalue where possible 
#One option would be "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" but there are no examples handling continuous variables there
#Maybe use '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"?

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
prop_glom_long_sub$ar_cel_status[prop_glom_long_sub$AR_Cellular == 0] <- "No Acute Rejection"
prop_glom_long_sub$ar_cel_status[prop_glom_long_sub$AR_Cellular == 1] <- "Acute Cellular Rejection"

#view(prop_long_p_sub)

# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
prop_glom_long_sub$cell_type = paste0(prop_glom_long_sub$Cell,".",prop_glom_long_sub$ar_cel_status) 
#view(prop_long_p_sub)

#based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"

Cellsummary <- prop_glom_long_sub %>%
  dplyr::select(-GF, -Slide) %>%
  dplyr::group_by(ar_cel_status, Cell) %>%
  dplyr::summarise(MeanCell= mean(Value, na.rm=TRUE),
                   SeCell = sd(Value, na.rm=TRUE)/sqrt(n()),
                   Count = n())

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#Cellsummary$glom_status[Cellsummary$Glom == 0] <- "NoGlom"
#Cellsummary$glom_status[Cellsummary$Glom == 1] <- "Glom"
  
# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
#Cellsummary$Cell_type = paste0(Cellsummary$Cell,".",Cellsummary$glom_status) 

#Based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" to pivot so the case and control groups are side by side
Cellsummary2 <- Cellsummary %>%
  pivot_wider (names_from = ar_cel_status,
               values_from = c(MeanCell, SeCell, Count)) 

view(Cellsummary2)
knitr::kable(Cellsummary2, format = "html")

#It is still staggered, check later #"https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html"

#Cellsummary2 <- Cellsummary %>%
#  dplyr::select(-Glom, -Cell) %>%
#  pivot_wider (names_from = Glom,
#               values_from = c(Cell_type))

```

# Plot Default cell matrix, nonglom, test diagnosis (No abnormality vs AR_Cellular)

```{r}

#view(res.nonglom$prop_of_all)
cells_nonglom<-rownames(res.nonglom$prop_of_all)
prop_nonglom=data.frame(t(res.nonglom$prop_of_all))
prop_nonglom$SegmentDisplayName<-rownames(prop_nonglom)
prop_nonglom<-merge(data.notes,prop_nonglom,by="SegmentDisplayName")
prop_nonglom<-merge(gf.data,prop_nonglom,by="PID")

#reshape for boxplot
prop_nonglom_long<-prop_nonglom%>%pivot_longer(cols = all_of(cells_nonglom), names_to = "Cell", values_to = "Value")

prop_nonglom_long_ARCellular<-prop_nonglom_long%>%filter(!is.na(AR_Cellular))

ggplot(prop_nonglom_long_ARCellular, aes(x = Cell, y = Value, fill = as.factor(AR_Cellular))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of nonGlom tissue, Acute Cellular Rejection vs No Acute Rejection",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_nonglom_long$Value) + 0.5) 
```

# Table of anova results, default cell matrix, nonglom, test diagnosis (No abnormality vs AR_Cellular)
```{r}
#| echo = FALSE, 
#| warning=FALSE

prop_nonglom_long_p<-prop_nonglom_long_ARCellular%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_nonglom_long_p[, c(cells_nonglom)], function(x) {
  anova_result <- aov(x ~ prop_nonglom_long_p$AR_Cellular)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

#Plot only the significant results at Dr. Israni's request

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
prop_nonglom_long_sub<-prop_nonglom_long_ARCellular%>%
  filter(Cell %in% c("macrophages","mast","T.CD8.memory","NK","neutrophils","endothelial.cells","fibroblasts"))

ggplot(prop_nonglom_long_sub, aes(x = Cell, y = Value, fill = as.factor(AR_Cellular))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of nonGlom tissue, Acute Cellular Rejection vs No Acute Rejection, statistically significant results",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long_sub$Value) + 0.5) 

#Making a table comparing the proportion of cell types for significant results

#Since the F result is not an "effect estimate" I will make descriptive tables for the mean per group and pvalue where possible 
#One option would be "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" but there are no examples handling continuous variables there
#Maybe use '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"?

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
prop_nonglom_long_sub$ar_cel_status[prop_nonglom_long_sub$AR_Cellular == 0] <- "No Acute Rejection"
prop_nonglom_long_sub$ar_cel_status[prop_nonglom_long_sub$AR_Cellular == 1] <- "Acute Cellular Rejection"

#view(prop_long_p_sub)

# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
prop_nonglom_long_sub$cell_type = paste0(prop_nonglom_long_sub$Cell,".",prop_nonglom_long_sub$ar_cel_status) 
#view(prop_long_p_sub)

#based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"

Cellsummary <- prop_nonglom_long_sub %>%
  dplyr::select(-GF, -Slide) %>%
  dplyr::group_by(ar_cel_status, Cell) %>%
  dplyr::summarise(MeanCell= mean(Value, na.rm=TRUE),
                   SeCell = sd(Value, na.rm=TRUE)/sqrt(n()),
                   Count = n())

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#Cellsummary$glom_status[Cellsummary$Glom == 0] <- "NoGlom"
#Cellsummary$glom_status[Cellsummary$Glom == 1] <- "Glom"
  
# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
#Cellsummary$Cell_type = paste0(Cellsummary$Cell,".",Cellsummary$glom_status) 

#Based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" to pivot so the case and control groups are side by side
Cellsummary2 <- Cellsummary %>%
  pivot_wider (names_from = ar_cel_status,
               values_from = c(MeanCell, SeCell, Count)) 

view(Cellsummary2)
knitr::kable(Cellsummary2, format = "html")

#It is still staggered, check later #"https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html"

#Cellsummary2 <- Cellsummary %>%
#  dplyr::select(-Glom, -Cell) %>%
#  pivot_wider (names_from = Glom,
#               values_from = c(Cell_type))

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

```{r}

```

```{r}

```

```{r}

```

# Plot Default cell matrix, glom, ACR_AMR

```{r}
#view(res.glom$prop_of_all)
cells_glom<-rownames(res.glom$prop_of_all)
prop_glom=data.frame(t(res.glom$prop_of_all))
prop_glom$SegmentDisplayName<-rownames(prop_glom)
prop_glom<-merge(data.notes,prop_glom,by="SegmentDisplayName")
prop_glom<-merge(gf.data,prop_glom,by="PID")

#reshape for boxplot

prop_glom_long<-prop_glom%>%pivot_longer(cols = all_of(cells_glom), names_to = "Cell", values_to = "Value")

prop_glom_long_ACR_AMR<-prop_glom_long%>%filter(!is.na(ACR_AMR))

ggplot(prop_glom_long_ACR_AMR, aes(x = Cell, y = Value, fill = as.factor(ACR_AMR))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of Glom tissue, Acute Cellular Rejection vs Antibody Mediated Rejection",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long$Value) + 0.5) 

```

# Table of anova results, default cell matrix, glom, Test diagnosis (No abnormality vs AR_Cellular)

```{r}
#| echo = FALSE, 
#| warning=FALSE

prop_glom_long_p<-prop_glom_long_ACR_AMR%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_glom_long_p[, c(cells_glom)], function(x) {
  anova_result <- aov(x ~ prop_glom_long_p$ACR_AMR)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

#Plot only the significant results at Dr. Israni's request

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
prop_glom_long_sub<-prop_glom_long_ACR_AMR%>%
  filter(Cell %in% c("mast","B.naive","B.memory","plasma","mDCs","monocytes.NC.I","endothelial.cells"))

ggplot(prop_glom_long_sub, aes(x = Cell, y = Value, fill = as.factor(ACR_AMR))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of Glom tissue, Acute Cellular Rejection vs Antibody Mediated Rejection, statistically significant results",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long_sub$Value) + 0.5) 

#Making a table comparing the proportion of cell types for significant results

#Since the F result is not an "effect estimate" I will make descriptive tables for the mean per group and pvalue where possible 
#One option would be "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" but there are no examples handling continuous variables there
#Maybe use '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"?

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#prop_glom_long_sub$ar_cel_status[prop_glom_long_sub$AR_Cellular == 0] <- "No Acute Rejection"
#prop_glom_long_sub$ar_cel_status[prop_glom_long_sub$AR_Cellular == 1] <- "Acute Cellular Rejection"

#view(prop_long_p_sub)

# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
prop_glom_long_sub$cell_type = paste0(prop_glom_long_sub$Cell,".",prop_glom_long_sub$ACR_AMR) 
#view(prop_long_p_sub)

#based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"

Cellsummary <- prop_glom_long_sub %>%
  dplyr::select(-GF, -Slide) %>%
  dplyr::group_by(ACR_AMR, Cell) %>%
  dplyr::summarise(MeanCell= mean(Value, na.rm=TRUE),
                   SeCell = sd(Value, na.rm=TRUE)/sqrt(n()),
                   Count = n())

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#Cellsummary$glom_status[Cellsummary$Glom == 0] <- "NoGlom"
#Cellsummary$glom_status[Cellsummary$Glom == 1] <- "Glom"
  
# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
#Cellsummary$Cell_type = paste0(Cellsummary$Cell,".",Cellsummary$glom_status) 

#Based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" to pivot so the case and control groups are side by side
Cellsummary2 <- Cellsummary %>%
  pivot_wider (names_from = ACR_AMR,
               values_from = c(MeanCell, SeCell, Count)) 

view(Cellsummary2)
knitr::kable(Cellsummary2, format = "html")

#It is still staggered, check later #"https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html"

#Cellsummary2 <- Cellsummary %>%
#  dplyr::select(-Glom, -Cell) %>%
#  pivot_wider (names_from = Glom,
#               values_from = c(Cell_type))

```

# Plot Default cell matrix, nonglom, ACR_AMR

```{r}

#view(res.nonglom$prop_of_all)
cells_nonglom<-rownames(res.nonglom$prop_of_all)
prop_nonglom=data.frame(t(res.nonglom$prop_of_all))
prop_nonglom$SegmentDisplayName<-rownames(prop_nonglom)
prop_nonglom<-merge(data.notes,prop_nonglom,by="SegmentDisplayName")
prop_nonglom<-merge(gf.data,prop_nonglom,by="PID")

#reshape for boxplot
prop_nonglom_long<-prop_nonglom%>%pivot_longer(cols = all_of(cells_nonglom), names_to = "Cell", values_to = "Value")

prop_nonglom_long_ACR_AMR<-prop_nonglom_long%>%filter(!is.na(ACR_AMR))

ggplot(prop_nonglom_long_ACR_AMR, aes(x = Cell, y = Value, fill = as.factor(ACR_AMR))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of nonGlom tissue, ACR vs AMR",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_nonglom_long$Value) + 0.5) 
```

# Table of anova results, default cell matrix, nonglom, test diagnosis (No abnormality vs AR_Cellular)
```{r}
#| echo = FALSE, 
#| warning=FALSE

prop_nonglom_long_p<-prop_nonglom_long_ACR_AMR%>%pivot_wider(names_from = Cell,values_from = Value)
anova_results <- lapply(prop_nonglom_long_p[, c(cells_nonglom)], function(x) {
  anova_result <- aov(x ~ prop_nonglom_long_p$ACR_AMR)
  summary(anova_result)[[1]][["Pr(>F)"]][1]
})%>%as.data.frame()
anova_results<-anova_results%>%pivot_longer(cols = 1:ncol(anova_results), names_to = "Cell", values_to = "p_value")
print(anova_results[which(anova_results$p_value<0.05),])

#Plot only the significant results at Dr. Israni's request

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
prop_nonglom_long_sub<-prop_nonglom_long_ACR_AMR%>%
  filter(Cell %in% c("B.naive","T.CD8.naive","monocytes.C","neutrophils","endothelial.cells"))

ggplot(prop_nonglom_long_sub, aes(x = Cell, y = Value, fill = as.factor(ACR_AMR))) +
  geom_boxplot(position = position_dodge(0.9)) +
  labs(title = "Cell Type Comparisons of nonGlom tissue, ACR vs AMR",
       x = "Cell Type",
       y = "Value") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1)) +
  stat_compare_means(method = "anova", label = "p.signif", 
                     label.y = max(prop_glom_long_sub$Value) + 0.5) 

#Making a table comparing the proportion of cell types for significant results

#Since the F result is not an "effect estimate" I will make descriptive tables for the mean per group and pvalue where possible 
#One option would be "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" but there are no examples handling continuous variables there
#Maybe use '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"?

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#prop_nonglom_long_sub$ar_cel_status[prop_nonglom_long_sub$AR_Cellular == 0] <- "No Acute Rejection"
#prop_nonglom_long_sub$ar_cel_status[prop_nonglom_long_sub$AR_Cellular == 1] <- "Acute Cellular Rejection"

#view(prop_long_p_sub)

# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
prop_nonglom_long_sub$cell_type = paste0(prop_nonglom_long_sub$Cell,".",prop_nonglom_long_sub$ACR_AMR) 
#view(prop_long_p_sub)

#based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_37_ggplot2publicationscourse.R'"

Cellsummary <- prop_nonglom_long_sub %>%
  dplyr::select(-GF, -Slide) %>%
  dplyr::group_by(ACR_AMR, Cell) %>%
  dplyr::summarise(MeanCell= mean(Value, na.rm=TRUE),
                   SeCell = sd(Value, na.rm=TRUE)/sqrt(n()),
                   Count = n())

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#Cellsummary$glom_status[Cellsummary$Glom == 0] <- "NoGlom"
#Cellsummary$glom_status[Cellsummary$Glom == 1] <- "Glom"
  
# Use the paste0 command to create unique variable combinations
#https://www.r-bloggers.com/2022/08/r-program-to-concatenate-two-strings/
#Cellsummary$Cell_type = paste0(Cellsummary$Cell,".",Cellsummary$glom_status) 

#Based on "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_2_Resources_3_01_Programs_01_R coding/Bioinf_2_Resources_3_03_Bioinformatics_Working with data in R LATIS/R_23_Creating tables with dplyr.R'" to pivot so the case and control groups are side by side
Cellsummary2 <- Cellsummary %>%
  pivot_wider (names_from = ACR_AMR,
               values_from = c(MeanCell, SeCell, Count)) 

view(Cellsummary2)
knitr::kable(Cellsummary2, format = "html")

#It is still staggered, check later #"https://rstudio-conf-2020.github.io/r-for-excel/pivot-tables.html"

#Cellsummary2 <- Cellsummary %>%
#  dplyr::select(-Glom, -Cell) %>%
#  pivot_wider (names_from = Glom,
#               values_from = c(Cell_type))

```