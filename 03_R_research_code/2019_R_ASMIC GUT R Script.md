source("http://bioconductor.org/biocLite.R")
biocLite ()
biocLite("DESeq2")
biocLite("biomformat")
biocLite("metagenomeSeq")

install.packages('randomForest')
install.packages('ape')
install.packages('vegan')
install.packages('optparse')
install.packages('gtools')
install.packages('klaR')
install.packages('RColorBrewer')

library('biomformat')
library('vegan')

genus.biom <- read_biom('/Users/onyea005/Documents/asmic_gut_mapping/otu_table_L6_json.biom')
genus <- as.matrix(biom_data(genus.biom))  
genus <- t(genus)  
map <- read.table('/Users/onyea005/Documents/asmic_gut_mapping/mapping_test_3.txt', sep='\t', comment='', head=T, row.names=1)

# find the overlapping samples 
common.ids <- intersect(rownames(map), rownames(genus))  

dim(genus)
genus <- genus[,colMeans(genus > 0) >= .1] 
dim(genus)

colnames(genus)[1:10]
colnames(genus) <- sapply(strsplit(colnames(genus),';'),function(xx) paste(paste(substr(xx[-c(1,length(xx))],4,7),collapse=';'),substring(xx[length(xx)],4),sep=';'))
colnames(genus)[1:10]
genus[1:5,1:2]
colnames(map)
table(map$Treatment)
table(map$Collection)

install.packages('xlsx')
library(xlsx)
write.xlsx(genus, "c:/Users/onyea005/Documents/asmic_gut_mapping/genus_taxa_L6.xlsx")


grep('Prevotella$',colnames(genus))
prevotella <- genus[,grep('Prevotella$',colnames(genus))]
hist(prevotella, br=30)
cor.test(prevotella, map$Treatment)
fit <- lm(prevotella ~ map$Collection)  

pval <- anova(fit)['map$Collection','Pr(>F)'] 
pval

fit <- lm(prevotella ~ map$Treatment + map$Collection)  

for(i in 1:ncol(genus)) {     
fit <- lm(genus[,i] ~ map$Treatment + map$Collection)  
pvals[i] <- anova(fit)['map$Treatment','Pr(>F)'] }  

pvals <- anova(fit)['map$Collection','Pr(>F)'] 
pvals

for(i in 1:ncol(genus)) {
fit <- lm(genus[,i] ~ map$Treatment + map$Collection) 
pval[i] <-  ks.test(rstudent(fit), pnorm, mean=mean(rstudent(fit)), sd=sd(rstudent(fit)),exact=FALSE)$p.value }  

genus.biom <- read_biom('/Users/onyea005/Documents/asmic_gut_mapping/otu_table_L6_json2.biom')

genus.a <- as.matrix(biom_data(genus.biom)) 
genus.a <- t(genus.a) 
common.ids <- intersect(rownames(map), rownames(genus.a))  
# get just the overlapping samples 
genus.a <- genus.a[common.ids,] 
map <- map[common.ids,]
dim(genus.a)
dim(map)
genus.a <- genus.a[,colMeans(genus.a > 0) >= .1] 
colnames(genus.a) <- sapply(strsplit(colnames(genus.a),';'),function(xx) paste(paste(substr(xx[-c(1,length(xx))],4,7),collapse=';'),substring(xx[length(xx)],4),sep=';'))

biocLite("edgeR")
library(limma)
library(edgeR)

source('/Users/onyea005/Documents/mice_tutorial/mice8992-2016-master/src/wrap.edgeR.r')

result <- glm.edgeR(x=map$Treatment, Y=genus.a)

biocLite ()
biocLite("phyloseq")
biocLite("ggplot2")
library("phyloseq")
library("ggplot2")

asmicgutotu <- import_biom("/Users/onyea005/Documents/asmic_gut_mapping/otu_table.biom")
asmicguttree <- read_tree("/Users/onyea005/Documents/asmic_gut_mapping/97_otus.tree")
asmicgutmap  <- import_qiime_sample_data("/Users/onyea005/Documents/asmic_gut_mapping/mapping_test_3.txt")
asmicgutphyloseq <- merge_phyloseq(asmicgutotu,asmicguttree,asmicgutmap)
