###########################################################################################################################################
###########################################################################################################################################
###########################################################################################################################################



###########################################################################################################################################
###########################################################################################################################################
###########################################################################################################################################


#My gameplan for using edgeR in the miRNA dataset is reliant on the fact that the format for the OTUtable as a phyloseq object is similar
 as seen below 

#You can export the table as a txt file then import that into excel as well (https://github.com/joey711/phyloseq/issues/613)
write.table(pseq.absolute.df, file = "/Users/guillaumeonyeaghala/Desktop/dadaoralspeciesabsolute.txt")

#I looked up the absolute abundance file which I moved to "D:\TEMP_Nsolver_miRNA\Datasets\asmicoralabsolute.txt" and I formatted the
normalized miRNA data in the same format (I will use the Sample_ID variable as my merging variable for the phyloseq object). I imported the txt file into the excel spreadsheet to match formats, then saved the miRNA spreadsheet as a txt file to import in R

#Next, I need to find the corresponding mapping file located at "asmicoralmap  <- import_qiime_sample_data("/Volumes/ROOK_PC/MAC_Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/Aspirin_Oral_Map.txt")", and use it to create a phyloseq object as seen in the section above

#Testing the import of the miRNA files for phyloseq

#Load phyloseq

#To use phyloseq in a new R session, it will have to be loaded. This can be done in your package manager, or at the command line using the library() command:

#Using the Phyloseq package and converting the DADA2 taxa names to something usable
source("http://www.bioconductor.org/biocLite.R") 
biocLite("microbiome")
biocLite("phyloseq")

library(microbiome) 
library("phyloseq")
library(knitr)

if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install()

BiocManager::install(c("phyloseq", "microbiome", "knitr", "ggplot2"))
library("microbiome") 
library("phyloseq")
library("knitr")
library("ggplot2")


install.packages('xlsx')
library('xlsx')
#could not load xlsx but right now I do not need this package

#Change the folder to "D:\Professional Folder\HHRI\19_MISSION_Nsolver_miRNA\Datasets\Israni4_Project_015_normalized_phyloseq.txt"
#Change the folder to ""D:\Professional Folder\HHRI\19_MISSION_Nsolver_miRNA\Documents\4_miRNA and new Betaglucuronidase measurements"
miRNAdata <- read.table('D:/Professional Folder/HHRI/19_MISSION_Nsolver_miRNA/Documents/4_miRNA and new Betaglucuronidase measurements/2_32_Israni4_Project_015_normalized_V3_litmiRNAonly_noESRD_notumorV2.txt',sep='\t',head=T,row=1,check=F,comment='')
miRNAmap  <- import_qiime_sample_data('D:/Professional Folder/HHRI/19_MISSION_Nsolver_miRNA/Documents/4_miRNA and new Betaglucuronidase measurements/2_22_Beta_glucoronidase_gene_families_renamed_test_mosio_M1M2_nomissing_v5.txt')

head(miRNAdata)
head(miRNAmap)

#https://stats.stackexchange.com/questions/13855/is-there-an-r-equivalent-of-sas-proc-freq
describe(miRNAmap$betaglucuronidase_activity_tert_ext)
describe(miRNAmap$betaglucuronidasegrams_tert_ext)

install.packages("summarytools")
library(summarytools)

freq(miRNAmap$betaglucuronidase_activity_tert_ext)
freq(miRNAmap$betaglucuronidasegrams_tert_ext)

# We now construct a phyloseq object directly from the miRNA data.
miRNAphyloseq <- phyloseq(otu_table(miRNAdata, taxa_are_rows=TRUE), 
               sample_data(miRNAmap))

miRNAphyloseq

head(otu_table(miRNAphyloseq))
head(tax_table(miRNAphyloseq)) #This should be empty since this is not microbiome data and we do not have a taxa table

# prune miRNAs that are not present in at least one sample
miRNAphyloseq
miRNAphyloseq2 <- prune_taxa(taxa_sums(miRNAphyloseq) > 0, miRNAphyloseq)
miRNAphyloseq2

###############################################################################################################################################################################
###############################################################################################################################################################################
###############################################################################################################################################################################

# Take a look at "https://joey711.github.io/phyloseq/import-data.html" on how to make pretend data to coerce into a phyloseq object
# I will create a pretend taxonomy table to add to the phyloseq object if I need to implement it in the phyloseq to edgeR section

#Phyloseq to EdgeR Example 1
#Differential Abundances
#For differential abundances we use RNAseq pipeline EdgeR and limma voom.

if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install()

BiocManager::install(c("edgeR"))
library("edgeR") 

###Betaglucuronidase tertiles

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseq2), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseq)) 

head(miRNAmap)
head(sample_data(miRNAphyloseq))

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betaglucuronidasegrams_tert, data=data.frame(as(sample_data(miRNAphyloseq),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)


#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/7_betaglucgrams_tert_diffexpression_lit.txt")

# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt

###Betaglucuronidase tertiles T1 vs T3 (Need to figure out a way to restrict to only the observations with extreme values)

head(miRNAmap)
head(sample_data(miRNAphyloseq))
miRNAphyloseqV2 <- subset_samples(miRNAphyloseq, betaglucuronidasegrams_tert_ext != "NA") 

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseqV2), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseqV2)) 

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betaglucuronidasegrams_tert_ext, data=data.frame(as(sample_data(miRNAphyloseqV2),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)

#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/8_betaglucgrams_tert_ext_diffexpression_lit.txt")

# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt

###Betaglucuronidase quartiles

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseq2), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseq)) 

head(miRNAmap)
head(sample_data(miRNAphyloseq))

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betaglucuronidasegrams_quart, data=data.frame(as(sample_data(miRNAphyloseq),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)


#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/9_betaglucgrams_quart_diffexpression_litnoCRC.txt")

# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt

###Here is a bar plot showing the log2-fold-change, showing Genus and Phylum. Uses some ggplot2 commands.
#https://joey711.github.io/phyloseq-extensions/edgeR.html

library("ggplot2")

theme_set(theme_bw())
scale_fill_discrete <- function(palname = "Set1", ...) {
    scale_fill_brewer(palette = palname, ...)
}

# Phylum order
test <-as.data.frame(lrt)

write.table(test, file = "C:/Users/Guillaume Onyeaghala/Desktop/ggplot-template-quartiles.txt")

#reimport the table after fixing the column names to make a figure of the top 25 pvalues

test2 <- read.table('C:/Users/Guillaume Onyeaghala/Desktop/ggplot-template-quartiles-fixedV2.txt',head=T,row=1)

head(test2)
x = tapply(test2$PValue, test2$miRNA_names, function(x) max(x))
x = sort(x, TRUE)

ggplot(test2, aes(x = miRNA_names, y = PValue, color = miRNA_names)) + geom_point(size=6) + 
  theme(axis.text.x = element_text(angle = -90, hjust = 0, vjust = 0.5))

###Betaglucuronidase quartiles Q1 vs Q4 (Need to figure out a way to restrict to only the observations with extreme values)

head(miRNAmap)
head(sample_data(miRNAphyloseq))
miRNAphyloseqV3 <- subset_samples(miRNAphyloseq, betaglucuronidasegrams_quart_ext != "NA") 

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseqV3), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseqV2)) 

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betaglucuronidasegrams_quart_ext, data=data.frame(as(sample_data(miRNAphyloseqV3),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)

#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/10_betaglucgrams_quart_ext_diffexpression_litV2.txt")

# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt

###Betaglucuronidase quintiles

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseq2), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseq)) 

head(miRNAmap)
head(sample_data(miRNAphyloseq))

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betaglucuronidase_activity_quint, data=data.frame(as(sample_data(miRNAphyloseq2),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)


#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/11_betagluc_quint_diffexpression_lit.txt")

# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt

###Betaglucuronidase quintiles Q1 vs Q5 (Need to figure out a way to restrict to only the observations with extreme values)

head(miRNAmap)
head(sample_data(miRNAphyloseq))
miRNAphyloseqV4 <- subset_samples(miRNAphyloseq, betaglucuronidase_activity_quint_ext != "NA") 

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseqV4), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseqV2)) 

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betaglucuronidase_activity_quint_ext, data=data.frame(as(sample_data(miRNAphyloseqV4),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)

#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/12_betagluc_quint_ext_diffexpression_lit.txt")

# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt

###############################################################################################################################################################################################################################
###############################################################################################################################################################################################################################
###############################################################################################################################################################################################################################

#Running Differential abundances by the pfam metatranscript copies as categorical variables

#My gameplan for using edgeR in the miRNA dataset is reliant on the fact that the format for the OTUtable as a phyloseq object is similar
 as seen below 

#You can export the table as a txt file then import that into excel as well (https://github.com/joey711/phyloseq/issues/613)
write.table(pseq.absolute.df, file = "/Users/guillaumeonyeaghala/Desktop/dadaoralspeciesabsolute.txt")

#I looked up the absolute abundance file which I moved to "D:\TEMP_Nsolver_miRNA\Datasets\asmicoralabsolute.txt" and I formatted the
normalized miRNA data in the same format (I will use the Sample_ID variable as my merging variable for the phyloseq object). I imported the txt file into the excel spreadsheet to match formats, then saved the miRNA spreadsheet as a txt file to import in R

#Next, I need to find the corresponding mapping file located at "asmicoralmap  <- import_qiime_sample_data("/Volumes/ROOK_PC/MAC_Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/Aspirin_Oral_Map.txt")", and use it to create a phyloseq object as seen in the section above

#Testing the import of the miRNA files for phyloseq

#Load phyloseq

#To use phyloseq in a new R session, it will have to be loaded. This can be done in your package manager, or at the command line using the library() command:

#Using the Phyloseq package and converting the DADA2 taxa names to something usable
source("http://www.bioconductor.org/biocLite.R") 
biocLite("microbiome")
biocLite("phyloseq")

library(microbiome) 
library("phyloseq")
library(knitr)

if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install()

BiocManager::install(c("phyloseq", "microbiome", "knitr", "ggplot2"))
library("microbiome") 
library("phyloseq")
library("knitr")
library("ggplot2")


install.packages('xlsx')
library('xlsx')
#could not load xlsx but right now I do not need this package

#Change the folder to "D:\Professional Folder\HHRI\19_MISSION_Nsolver_miRNA\Datasets\Israni4_Project_015_normalized_phyloseq.txt"
miRNAdata <- read.table('D:/Professional Folder/HHRI/19_MISSION_Nsolver_miRNA/Datasets/Israni4_Project_015_normalized_phyloseq.txt',sep='\t',head=T,row=1,check=F,comment='')
miRNAmap  <- import_qiime_sample_data('D:/Professional Folder/HHRI/19_MISSION_Nsolver_miRNA/Datasets/Beta_glucoronidase_gene_families_renamed_test_mosio_M1M2_nomissing_v4.txt')

head(miRNAdata)
head(miRNAmap)

#https://stats.stackexchange.com/questions/13855/is-there-an-r-equivalent-of-sas-proc-freq
describe(miRNAmap$betaglucuronidase_activity_tert_ext)
describe(miRNAmap$betaglucuronidase_pfam_tert_ext)

install.packages("summarytools")
library(summarytools)

freq(miRNAmap$betaglucuronidase_activity_tert_ext)
freq(miRNAmap$betagluc_pfam_tert_ext)

# We now construct a phyloseq object directly from the miRNA data.
miRNAphyloseq <- phyloseq(otu_table(miRNAdata, taxa_are_rows=TRUE), 
               sample_data(miRNAmap))

miRNAphyloseq

head(otu_table(miRNAphyloseq))
head(tax_table(miRNAphyloseq)) #This should be empty since this is not microbiome data and we do not have a taxa table

# prune miRNAs that are not present in at least one sample
miRNAphyloseq
miRNAphyloseq2 <- prune_taxa(taxa_sums(miRNAphyloseq) > 0, miRNAphyloseq)
miRNAphyloseq2

# Take a look at "https://joey711.github.io/phyloseq/import-data.html" on how to make pretend data to coerce into a phyloseq object
# I will create a pretend taxonomy table to add to the phyloseq object if I need to implement it in the phyloseq to edgeR section

#Phyloseq to EdgeR Example 1
#Differential Abundances
#For differential abundances we use RNAseq pipeline EdgeR and limma voom.

if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install()

BiocManager::install(c("edgeR"))
library("edgeR") 

###Betaglucuronidase tertiles

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseq2), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseq)) 

head(miRNAmap)
head(sample_data(miRNAphyloseq))

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betagluc_pfam_tert, data=data.frame(as(sample_data(miRNAphyloseq),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)

#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/1_betagluc_pfam_tert_diffexpression.txt")

# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt

###Betaglucuronidase tertiles T1 vs T3 (Need to figure out a way to restrict to only the observations with extreme values)

head(miRNAmap)
head(sample_data(miRNAphyloseq))
miRNAphyloseqV2 <- subset_samples(miRNAphyloseq, betagluc_pfam_tert_ext != "NA") 

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseqV2), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseqV2)) 

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betagluc_pfam_tert_ext, data=data.frame(as(sample_data(miRNAphyloseqV2),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)

#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/2_betagluc_pfam_tertext_diffexpression.txt")


# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt

###Betaglucuronidase quartiles

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseq2), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseq)) 

head(miRNAmap)
head(sample_data(miRNAphyloseq))

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betagluc_pfam_quart, data=data.frame(as(sample_data(miRNAphyloseq2),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

###write.table(decideTests(fit), file = "C:/Users/Guillaume Onyeaghala/Desktop/0_betagluc_pfam_quart_diffexpressiondecidetest.txt")

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)

#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/3_betagluc_pfam_quart_diffexpression.txt")

# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt

###Betaglucuronidase quartiles Q1 vs Q4 (Need to figure out a way to restrict to only the observations with extreme values)

head(miRNAmap)
head(sample_data(miRNAphyloseq))
miRNAphyloseqV3 <- subset_samples(miRNAphyloseq, betagluc_pfam_quart_ext != "NA") 

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseqV3), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseqV2)) 

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betagluc_pfam_quart_ext, data=data.frame(as(sample_data(miRNAphyloseqV3),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)

#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/4_betagluc_pfam_quart_ext_diffexpression.txt")

# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt

###Betaglucuronidase quintiles

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseq2), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseq)) 

head(miRNAmap)
head(sample_data(miRNAphyloseq))

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betagluc_pfam_quint, data=data.frame(as(sample_data(miRNAphyloseq2),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)


#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/5_betagluc_pfam_quint_diffexpression.txt")

# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt

###Betaglucuronidase quintiles Q1 vs Q5 (Need to figure out a way to restrict to only the observations with extreme values)

head(miRNAmap)
head(sample_data(miRNAphyloseq))
miRNAphyloseqV4 <- subset_samples(miRNAphyloseq, betagluc_pfam_quint_ext != "NA") 

#Since it is a phyloseq object, you can just pull the otu table and the sample data information from the one dataset
m = as(otu_table(miRNAphyloseqV4), "matrix") 

# Add one to protect against overflow, log(0) issues. 
m = m + 1 

# Define gene annotations (`genes`) as tax_table (#Skip this step as we did not include a pretend taxonomy table)
taxonomy = tax_table(asmicoralphyloseq.4, errorIfNULL=FALSE) 
if( !is.null(taxonomy) ){   taxonomy = data.frame(as(taxonomy, "matrix")) }  

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

# Check for division by zero inside `calcNormFactors` 
if( !all(is.finite(z$samples$norm.factors)) ){   stop("Something wrong with edgeR::calcNormFactors on this data, non-finite $norm.factors, consider changing `method` argument") }  
plotMDS(z, col = as.numeric(factor(sample_data(miRNAphyloseq)$X_SampleID)), labels = sample_names(miRNAphyloseqV2)) 

# Creat a model based on Treatment and Collection
mm <- model.matrix(~ betagluc_pfam_quint_ext, data=data.frame(as(sample_data(miRNAphyloseqV4),"matrix")))

# specify model with no intercept for easier contrasts 
#Check the mean variance trend
y <- voom(d, mm, plot = T)
fit <- lmFit(y, mm)
head(coef(fit))
summary(fit)

###Go to the limma user guide "D:\TEMP_Nsolver_miRNA\Documents_NSolver Analysis tutorial\5_0_Limma_User Guide.pdf" at page 126 and 127 to see how to publish pvalues for your lmfit model
#Convert the variables to continous didn't seem to change much (https://statisticsglobe.com/convert-discrete-factor-continuous-variable-r) (https://www.guru99.com/r-factor-categorical-continuous.html)

fit<-eBayes(fit)
summary(decideTests(fit))

sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)

#Check the guidelines at "https://joey711.github.io/phyloseq-extensions/edgeR.html" to publish the logFc values

topTags(fit) #this won't work without the exact test or glmLRT

# Perform test

# Now turn into a DGEList 
d = DGEList(counts=m, remove.zeros = TRUE)  

# Calculate the normalization factors 
z = calcNormFactors(d, method="RLE") 

z = estimateGLMCommonDisp(z, mm)
z = estimateGLMTrendedDisp(z, mm)
z = estimateGLMTagwiseDisp(z, mm)

fit2 <- glmFit(z, mm)
lrt <-glmLRT(fit2) 
topTags(lrt)

write.table(topTags(lrt), file = "C:/Users/Guillaume Onyeaghala/Desktop/6_betagluc_pfam_quint_ext_diffexpression.txt")

# Extract values from test results 

tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt


###############################################################################################################################################################################################################################
###############################################################################################################################################################################################################################
###############################################################################################################################################################################################################################

#Phyloseq to EdgeR Example 2
#(http://joey711.github.io/phyloseq-extensions/edgeR.html)
#For this example, instead of using the the publicly available data from a study on colorectal cancer (kosticDB) from the website, we will use datasets that are included in phyloseq

#Independent filtering

#Now let?s use a function to convert the phyloseq data object into an edgeR ?DGE? data object. In order to do that, we will need to run the custom function below. 
phyloseq_to_edgeR = function(physeq, group, method="RLE", ...){   
require("edgeR")   
require("phyloseq")   
# Enforce orientation.   
if( !taxa_are_rows(physeq) ){ physeq <- t(physeq) }   
x = as(otu_table(physeq), "matrix")   
# Add one to protect against overflow, log(0) issues.   
x = x + 1   
# Check `group` argument   
if( identical(all.equal(length(group), 1), TRUE) & nsamples(physeq) > 1 ){     
# Assume that group was a sample variable name (must be categorical)     
group = get_variable(physeq, group)   }   
# Define gene annotations (`genes`) as tax_table   
taxonomy = tax_table(physeq, errorIfNULL=FALSE)   
if( !is.null(taxonomy) ){     taxonomy = data.frame(as(taxonomy, "matrix"))   }    
# Now turn into a DGEList   
y = DGEList(counts=x, group=group, genes=taxonomy, remove.zeros = TRUE, ...)   
# Calculate the normalization factors   
z = calcNormFactors(y, method=method)   
# Check for division by zero inside `calcNormFactors`   
if( !all(is.finite(z$samples$norm.factors)) ){     stop("Something wrong with edgeR::calcNormFactors on this data,          non-finite $norm.factors, consider changing `method` argument")   }   
# Estimate dispersions   
return(estimateTagwiseDisp(estimateCommonDisp(z))) }

#However, as we showed in example #1 you do not need a wrapper R function as you can walk through each of those automated steps manually
asmicoralphyloseqB
head(sample_data(asmicoralphyloseqB))

#We will use Contents as the categorical variable for our differential abundance analyses
#Now we can run our tests to display our differentially abundant groups
dge = phyloseq_to_edgeR(kosticB, group="DIAGNOSIS")
dge = phyloseq_to_edgeR(asmicoralphyloseqB, group="Contents")

# Perform binary test 
et = exactTest(dge) 
# Extract values from test results 
tt = topTags(et, n=nrow(dge$table), adjust.method="BH", sort.by="PValue") 
tt
res = tt@.Data[[1]] 
alpha = 0.001 
sigtab = res[(res$FDR < alpha), ] 
sigtab = cbind(as(sigtab, "data.frame"), as(tax_table(asmicoralphyloseqB)[rownames(sigtab), ], "matrix")) 
dim(sigtab)
head(sigtab)

#Here is a bar plot showing the log2-fold-change, showing Genus and Phylum. Uses some ggplot2 commands.
library("ggplot2") 
theme_set(theme_bw()) 
scale_fill_discrete <- function(palname = "Set1", ...) {     scale_fill_brewer(palette = palname, ...) } 
sigtabgen = subset(sigtab, !is.na(Genus)) 

# Phylum order 
x = tapply(sigtabgen$logFC, sigtabgen$Phylum, function(x) max(x)) 
x = sort(x, TRUE) 
sigtabgen$Phylum = factor(as.character(sigtabgen$Phylum), levels = names(x)) 

# Genus order 
x = tapply(sigtabgen$logFC, sigtabgen$Genus, function(x) max(x)) 
x = sort(x, TRUE) 
sigtabgen$Genus = factor(as.character(sigtabgen$Genus), levels = names(x)) 
ggplot(sigtabgen, aes(x = Genus, y = logFC, color = Phylum)) + geom_point(size=6) +    theme(axis.text.x = element_text(angle = -90, hjust = 0, vjust = 0.5))

#You can export the table as a txt file then import that into excel as well (https://github.com/joey711/phyloseq/issues/613)
write.table(sigtabgen, file = "/Users/onyea005/Documents/asmic_oral/asmic_oral_mapping/V1V3EdgeR.txt")

###########################################################################################################################################
###########################################################################################################################################
###########################################################################################################################################

Phyloseq to DESeq2 Analysis ASMIC ORAL Full Version
#Load phyloseq & DESeq2 and import data

if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install()

BiocManager::install(c("phyloseq", "microbiome", "knitr", "ggplot2", "DESeq2"))
library("microbiome") 
library("phyloseq")
library("knitr")
library("ggplot2")
library("DESeq2")

install.packages('xlsx')
library('xlsx')
#could not load xlsx but right now I do not need this package

#Change the folder to "D:\Professional Folder\HHRI\19_MISSION_Nsolver_miRNA\Datasets\Israni4_Project_015_normalized_phyloseq.txt"
miRNAdata <- read.table('D:/Professional Folder/HHRI/19_MISSION_Nsolver_miRNA/Datasets/Israni4_Project_015_normalized_phyloseq.txt',sep='\t',head=T,row=1,check=F,comment='')
miRNAmap  <- import_qiime_sample_data('D:/Professional Folder/HHRI/19_MISSION_Nsolver_miRNA/Datasets/Beta_glucoronidase_gene_families_renamed_test_mosio_M1M2_nomissing_v4.txt')

miRNAdata <- read.table('D:/Professional Folder/HHRI/19_MISSION_Nsolver_miRNA/Documents/4_miRNA and new Betaglucuronidase measurements/2_32_Israni4_Project_015_normalized_V3_litmiRNAonly_noESRD_notumorV2.txt',sep='\t',head=T,row=1,check=F,comment='')
miRNAmap  <- import_qiime_sample_data('D:/Professional Folder/HHRI/19_MISSION_Nsolver_miRNA/Documents/4_miRNA and new Betaglucuronidase measurements/2_22_Beta_glucoronidase_gene_families_renamed_test_mosio_M1M2_nomissing_v5.txt')


head(miRNAdata)
head(miRNAmap)

# We now construct a phyloseq object directly from the miRNA data.
miRNAphyloseq <- phyloseq(otu_table(miRNAdata, taxa_are_rows=TRUE), 
               sample_data(miRNAmap))

miRNAphyloseq

head(otu_table(miRNAphyloseq))
head(tax_table(miRNAphyloseq)) #This should be empty since this is not microbiome data and we do not have a taxa table

# prune miRNAs that are not present in at least one sample
miRNAphyloseq
miRNAphyloseq2 <- prune_taxa(taxa_sums(miRNAphyloseq) > 0, miRNAphyloseq)
miRNAphyloseq2

#The following two lines actually do all the complicated DESeq2 work. The function phyloseq_to_deseq2 converts your phyloseq-format microbiome data into a DESeqDataSetwith dispersions estimated, using the experimental design formula, also shown (the ~SampleType). The DESeq function does the rest of the testing, in this case with default testing framework, but you can actually use alternatives.

head(miRNAmap)
head(sample_data(miRNAphyloseq))
sample_variables(miRNAphyloseq)
class(miRNAmap$total_glucuronidase)
class(sample_variables(miRNAphyloseq)$total_glucuronidase)

#Convert file from Phyloseq to DESeq2
diagdds = phyloseq_to_deseq2(miRNAphyloseq2, ~ betaglucuronidase_activity_grams) 

# calculate geometric means prior to estimate size factors 
gm_mean = function(x, na.rm=TRUE){ exp(sum(log(x[x > 0]), na.rm=na.rm) / length(x)) } 
geoMeans = apply(counts(diagdds), 1, gm_mean) 
diagdds = estimateSizeFactors(diagdds, geoMeans = geoMeans) 
diagdds = DESeq(diagdds, fitType="local")

#Note: The default multiple-inference correction is Benjamini-Hochberg, and occurs within the DESeq function.

#Investigate test results table
#The following results function call creates a table of the results of the tests. Very fast. The hard work was already stored with the rest of the DESeq2-related data in our latest version of the diagdds object (see above). I then order by the adjusted p-value, removing the entries with an NA value. The rest of this example is just formatting the results table with taxonomic information for nice(ish) display in the HTML output.

res = results(diagdds) 
res = res[order(res$padj, na.last=NA), ] 
res
alpha = 0.1 
sigtab = res[(res$pvalue < alpha), ] 

#sigtab = cbind(as(sigtab, "data.frame"), as(tax_table(asmicoralphyloseq)[rownames(sigtab), ], "matrix")) 
head(sigtab)

#You can export the table as a txt file then import that into excel as well (https://github.com/joey711/phyloseq/issues/613)
write.table(sigtab, file = "/Users/onyea005/Documents/asmic_oral/asmic_oral_mapping/V1V3DESeq2.txt")
write.table(sigtab, file = "C:/Users/Guillaume Onyeaghala/Desktop/14_betaglucgramslitonly_DESeq2.txt")


#Let?s look at just the OTUs that were significantly enriched in the Treatment group. First, cleaning up the table a little for legibility.

posigtab = sigtab[sigtab[, "log2FoldChange"] > 0, ]
posigtab = posigtab[, c("baseMean", "log2FoldChange", "lfcSE", "padj", "Phylum", "Class", "Family", "Genus")]
posigtab

posigtab = sigtab[sigtab[, "log2FoldChange"] > 0, ]
posigtab

negsigtab = sigtab[sigtab[, "log2FoldChange"] < 0, ]
negsigtab

#You can export the table as a txt file then import that into excel as well (https://github.com/joey711/phyloseq/issues/613)
write.table(posigtab, file = "/Users/onyea005/Documents/asmic_oral/asmic_oral_mapping/V1V3DESeq2pos.txt")
write.table(posigtab, file = "C:/Users/Guillaume Onyeaghala/Desktop/15_betaglucgramslitonlypos_DESeq2.txt")
write.table(negsigtab, file = "C:/Users/Guillaume Onyeaghala/Desktop/16_betaglucgramslitonlyneg_DESeq2.txt")

#Plot Results

###Here is a bar plot showing the log2-fold-change, showing Genus and Phylum. Uses some ggplot2 commands.
#https://joey711.github.io/phyloseq-extensions/edgeR.html

library("ggplot2")

theme_set(theme_bw())
scale_fill_discrete <- function(palname = "Set1", ...) {
    scale_fill_brewer(palette = palname, ...)
}

# Phylum order
test <-as.data.frame(sigtab)

write.table(test, file = "C:/Users/Guillaume Onyeaghala/Desktop/ggplot-template-betaglucDESEq2.txt")

#reimport the table after fixing the column names to make a figure of the top 25 pvalues (by making a new column with miRNA names as a covariate instead of first column name)

test2 <- read.table('C:/Users/Guillaume Onyeaghala/Desktop/ggplot-template-DESEq2-fixedV2.txt',head=T,row=1)

head(test2)
x = tapply(test2$log2FoldChange, test2$miRNA_names, function(x) max(x))
x = sort(x, TRUE)

ggplot(test2, aes(x = miRNA_names, y = log2FoldChange, color = miRNA_names)) + geom_point(size=6) + 
  theme(axis.text.x = element_text(angle = -90, hjust = 0, vjust = 0.5))

#Here is a bar plot showing the log2-fold-change, showing Genus and Phylum. Uses some ggplot2 commands.

library("ggplot2") 
theme_set(theme_bw()) 
sigtabgen = subset(sigtab, !is.na(Genus)) 

# Phylum order 
x = tapply(sigtabgen$log2FoldChange, sigtabgen$Phylum, function(x) max(x)) 
x = sort(x, TRUE) 
sigtabgen$Phylum = factor(as.character(sigtabgen$Phylum), levels=names(x)) 

# Genus order 
x = tapply(sigtabgen$log2FoldChange, sigtabgen$Genus, function(x) max(x)) 
x = sort(x, TRUE) 
sigtabgen$Genus = factor(as.character(sigtabgen$Genus), levels=names(x)) 
ggplot(sigtabgen, aes(y=Genus, x=log2FoldChange, color=Phylum)) + geom_vline(xintercept = 0.0, color = "gray", size = 0.5) + geom_point(size=6) + theme(axis.text.x = element_text(angle = -90, hjust = 0, vjust=0.5))

###Here is a bar plot showing the log2-fold-change, showing Genus and Phylum. Uses some ggplot2 commands.
#https://joey711.github.io/phyloseq-extensions/edgeR.html

library("ggplot2")

theme_set(theme_bw())
scale_fill_discrete <- function(palname = "Set1", ...) {
    scale_fill_brewer(palette = palname, ...)
}

# Phylum order
test <-as.data.frame(lrt)

write.table(test, file = "C:/Users/Guillaume Onyeaghala/Desktop/ggplot-template-quartiles.txt")

#reimport the table after fixing the column names to make a figure of the top 25 pvalues

test2 <- read.table('C:/Users/Guillaume Onyeaghala/Desktop/ggplot-template-quartiles-fixedV2.txt',head=T,row=1)

head(test2)
x = tapply(test2$PValue, test2$miRNA_names, function(x) max(x))
x = sort(x, TRUE)

ggplot(test2, aes(x = miRNA_names, y = PValue, color = miRNA_names)) + geom_point(size=6) + 
  theme(axis.text.x = element_text(angle = -90, hjust = 0, vjust = 0.5))


#Now running this analysis on just collections 2

asmicoral = subset_samples(asmicoralphyloseq, Collection%in% c(2))
asmicoral

asmicoral <- subset_samples(asmicoral, Contents != "NA") 
asmicoral <- prune_samples(sample_sums(asmicoral) > 500, asmicoral) 
asmicoral

#Convert file from Phyloseq to DESeq2
diagdds = phyloseq_to_deseq2(asmicoral, ~ Contents) 

# calculate geometric means prior to estimate size factors 
gm_mean = function(x, na.rm=TRUE){ exp(sum(log(x[x > 0]), na.rm=na.rm) / length(x)) } 
geoMeans = apply(counts(diagdds), 1, gm_mean) 
diagdds = estimateSizeFactors(diagdds, geoMeans = geoMeans) 
diagdds = DESeq(diagdds, fitType="local")

#Note: The default multiple-inference correction is Benjamini-Hochberg, and occurs within the DESeq function.

#Investigate test results table
#The following results function call creates a table of the results of the tests. Very fast. The hard work was already stored with the rest of the DESeq2-related data in our latest version of the diagdds object (see above). I then order by the adjusted p-value, removing the entries with an NA value. The rest of this example is just formatting the results table with taxonomic information for nice(ish) display in the HTML output.

res = results(diagdds) 
res = res[order(res$padj, na.last=NA), ] 
res
alpha = 0.01 
sigtab = res[(res$padj < alpha), ] 
sigtab = cbind(as(sigtab, "data.frame"), as(tax_table(asmicoral)[rownames(sigtab), ], "matrix")) 
head(sigtab)

#Let?s look at just the OTUs that were significantly enriched in the Treatment group. First, cleaning up the table a little for legibility.

posigtab = sigtab[sigtab[, "log2FoldChange"] > 0, ]
posigtab = posigtab[, c("baseMean", "log2FoldChange", "lfcSE", "padj", "Genus")]

#Plot Results

#Here is a bar plot showing the log2-fold-change, showing Genus and Phylum. Uses some ggplot2 commands.

#First i need to recode the taxonomic ranks 

rank_names(asmicgutphyloseq)
colnames(tax_table(asmicgutphyloseq)) <- c("Kingdom", "Phylum", "Class", "Order", "Family", "Genus", "Species")
rank_names(asmicgutphyloseq)

library("ggplot2") 
theme_set(theme_bw()) 
sigtabgen = subset(sigtab, !is.na(Genus)) 

# Phylum order 
x = tapply(sigtabgen$log2FoldChange, sigtabgen$Phylum, function(x) max(x)) 
x = sort(x, TRUE) 
sigtabgen$Phylum = factor(as.character(sigtabgen$Phylum), levels=names(x)) 

# Genus order 
x = tapply(sigtabgen$log2FoldChange, sigtabgen$Genus, function(x) max(x)) 
x = sort(x, TRUE) 
sigtabgen$Genus = factor(as.character(sigtabgen$Genus), levels=names(x)) 
ggplot(sigtabgen, aes(y=Genus, x=log2FoldChange, color=Phylum)) + geom_vline(xintercept = 0.0, color = "gray", size = 0.5) + geom_point(size=6) + theme(axis.text.x = element_text(angle = -90, hjust = 0, vjust=0.5))

###########################################################################################################################################
###########################################################################################################################################
###########################################################################################################################################

###########################################################################################################################################
###########################################################################################################################################
###########################################################################################################################################