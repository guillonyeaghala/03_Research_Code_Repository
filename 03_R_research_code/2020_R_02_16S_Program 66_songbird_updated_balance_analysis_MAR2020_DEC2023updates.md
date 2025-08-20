############################################################################################################################

#Updated Balance analysis based on Dr. Demmer feedback for using a pseudocount table and adding a few family level organism in the mix

############################################################################################################################

#Testing out the analysis using balances

library(data.table)
library(lme4)

#The lme4 package suppresses p-values, so it's been sugested to install a package called afex to get them
#https://stats.stackexchange.com/questions/22988/how-to-obtain-the-p-value-check-significance-of-an-effect-in-a-lme4-mixed-mode

install.packages("afex")
library("afex")

#Testing the balance at 5%

Balance <- read.table('/Volumes/ROOK_PC/MAC_Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/Balance_Contents_Test.txt', sep='\t', comment='', head=T, row.names=1)
Balance

Balance$Collection.factor <-as.factor(Balance$Collection)
fre.balance<- lmer(Current_Natural_Log_Ratio~ Contents+Collection.factor+Contents*Collection.factor + (1 | Subject_ID), data = Balance, subset = Collection %in% c(1, 3))
summary(fre.balance)
anova(fre.balance)

#Okay, i created variables for the balance at 10, 15, 20, and 25% at the same time to save time instead of importing multiple folders

BalanceQ <- read.table('/Volumes/ROOK_PC/MAC_Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/Balance_Contents_Test_Quantiles.txt', sep='\t', comment='', head=T, row.names=1)
BalanceQ

#Testing the balance at 10%

BalanceQ$Collection.factor <-as.factor(BalanceQ$Collection)
fre.balance<- lmer(Current_Natural_Log_Ratio_10~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceQ, subset = Collection %in% c(1, 3))
summary(fre.balance)
anova(fre.balance)

fre.balance<- lmer(Current_Natural_Log_Ratio_10~ Contents+Collection.factor+Contents*Collection.factor + (1 | Subject_ID), data = BalanceQ, subset = Collection %in% c(1, 3))
summary(fre.balance)
anova(fre.balance)

Balance at 15%

BalanceQ$Collection.factor <-as.factor(BalanceQ$Collection)
fre.balance<- lmer(Current_Natural_Log_Ratio_15~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceQ, subset = Collection %in% c(1, 3))
summary(fre.balance)
anova(fre.balance)

fre.balance<- lmer(Current_Natural_Log_Ratio_15~ Contents+Collection.factor+Contents*Collection.factor + (1 | Subject_ID), data = BalanceQ, subset = Collection %in% c(1, 3))
summary(fre.balance)
anova(fre.balance)

Balance at 20%

BalanceQ$Collection.factor <-as.factor(BalanceQ$Collection)
fre.balance<- lmer(Current_Natural_Log_Ratio_20~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceQ, subset = Collection %in% c(1, 3))
summary(fre.balance)
anova(fre.balance)

fre.balance<- lmer(Current_Natural_Log_Ratio_20~ Contents+Collection.factor+Contents*Collection.factor + (1 | Subject_ID), data = BalanceQ, subset = Collection %in% c(1, 3))
summary(fre.balance)
anova(fre.balance)

Balance at 25%

BalanceQ$Collection.factor <-as.factor(BalanceQ$Collection)
fre.balance<- lmer(Current_Natural_Log_Ratio_25~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceQ, subset = Collection %in% c(1, 3))
summary(fre.balance)
anova(fre.balance)

fre.balance<- lmer(Current_Natural_Log_Ratio_25~ Contents+Collection.factor+Contents*Collection.factor + (1 | Subject_ID), data = BalanceQ, subset = Collection %in% c(1, 3))
summary(fre.balance)
anova(fre.balance)

############################################################################################################################

#Testing out the analysis using balances on post intvervention samples only

library(data.table)
library(lme4)

#The lme4 package suppresses p-values, so it's been sugested to install a package called afex to get them
#https://stats.stackexchange.com/questions/22988/how-to-obtain-the-p-value-check-significance-of-an-effect-in-a-lme4-mixed-mode

install.packages("afex")
library("afex")

#Okay, i created variables for the balance at 10, 15, 20, and 25% at the same time to save time instead of importing multiple folders

#Using data collapsed at the genus level

Balance <- read.table('/Volumes/ROOK_PC/MAC_Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/qurrro_balance_v2.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance)

#Testing the balance using crude models

summary(lm(Balance$X10_Balance ~ Contents, data=Balance))
summary(lm(Balance$X15_Balance ~ Contents, data=Balance))
summary(lm(Balance$X20_Balance ~ Contents, data=Balance))
summary(lm(Balance$X25_Balance ~ Contents, data=Balance))

#Testing the balance using adjusted models

summary(lm(Balance$A_10_Balance ~ Contents, data=Balance))
summary(lm(Balance$A_15_Balance ~ Contents, data=Balance))
summary(lm(Balance$A_20_Balance ~ Contents, data=Balance))
summary(lm(Balance$A_25_Balance ~ Contents, data=Balance))

#Using data not collaposed at the genus level

Balance <- read.table('/Volumes/ROOK_PC/MAC_Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/qurrro_balance_v3.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance)

#Testing the balance using crude models

summary(lm(Balance$X10_Balance ~ Contents, data=Balance))
summary(lm(Balance$X15_Balance ~ Contents, data=Balance))
summary(lm(Balance$X20_Balance ~ Contents, data=Balance))
summary(lm(Balance$X25_Balance ~ Contents, data=Balance))

#Testing the balance using adjusted models

summary(lm(Balance$A10_Balance ~ Contents, data=Balance))
summary(lm(Balance$A15_Balance ~ Contents, data=Balance))
summary(lm(Balance$A20_Balance ~ Contents, data=Balance))
summary(lm(Balance$A25_Balance ~ Contents, data=Balance))

#Looking at specific balances
summary(lm(Balance$Porphyromonas_Aggregatibacter ~ Contents, data=Balance))
summary(lm(Balance$Peptostreptococcus_Campylobacter ~ Contents, data=Balance))
summary(lm(Balance$Parainfluenzae_Campylobacter ~ Contents, data=Balance))
summary(lm(Balance$Pallens_Campylobacter ~ Contents, data=Balance))
summary(lm(Balance$Nanceiensis_Campylobacter ~ Contents, data=Balance))
summary(lm(Balance$Mucilaginosa_Campylobacter ~ Contents, data=Balance))
summary(lm(Balance$Moorei_Campylobacter ~ Contents, data=Balance))
summary(lm(Balance$Dispar_Campylobacter ~ Contents, data=Balance))
summary(lm(Balance$Dentocariosa_Campylobacter ~ Contents, data=Balance))
summary(lm(Balance$Veillonella_Campylobacter ~ Contents, data=Balance))
summary(lm(Balance$Veillonella_Aggregatibacter ~ Contents, data=Balance))
summary(lm(Balance$Porphyromonas_Campylobacter ~ Contents, data=Balance))


############################################################################################################################

#Running the linear mixed effect on the balances of pre-specified taxa

library(data.table)
library(lme4)

#The lme4 package suppresses p-values, so it's been sugested to install a package called afex to get them
#https://stats.stackexchange.com/questions/22988/how-to-obtain-the-p-value-check-significance-of-an-effect-in-a-lme4-mixed-mode

install.packages("afex")
library("afex")

#Importing the file in which I have imported the taxa specific balances

BalanceT <- read.table('/Volumes/ROOK_PC/MAC_Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/Balance_Contents_Test_Taxa.txt', sep='\t', comment='', head=T, row.names=1)
BalanceT

#Streptococcus
BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Streptococcus_Balance~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Streptococcus_Balance~ Contents*Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

#Veillonella

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Veillonella_Balance~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Veillonella_Balance~ Contents*Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

#Haemophilus

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Haemophilus_Balance~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Haemophilus_Balance~ Contents*Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

#Neisseria

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Neisseria_Balance~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Neisseria_Balance~ Contents*Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

#Prevotella

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Prevotella_Balance~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Prevotella_Balance~ Contents*Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

#Actinomyces

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Actinomyces_Balance~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Actinomyces_Balance~ Contents*Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

#Porphyromonas

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Actinomyces_Balance~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Actinomyces_Balance~ Contents*Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

#Campylobacter

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Campylobacter_Balance~ Contents+Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)

BalanceT$Collection.factor <-as.factor(BalanceT$Collection)
fre.balance<- lmer(Campylobacter_Balance~ Contents*Collection.factor+ (1 | Subject_ID), data = BalanceT, subset = Collection %in% c(1, 3))
summary(fre.balance)


##############################################################################################################################

#Balances for Corynebacterium as the denominator at visit 3

#Testing out the analysis using balances

library(data.table)
library(lme4)

#The lme4 package suppresses p-values, so it's been sugested to install a package called afex to get them
#https://stats.stackexchange.com/questions/22988/how-to-obtain-the-p-value-check-significance-of-an-effect-in-a-lme4-mixed-mode

install.packages("afex")
library("afex")

Balance_Cory_Denom <- read.table('/Users/guillaumeonyeaghala/Downloads/Aspirin_Oral_Map_Corynebacterium.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_Cory_Denom)

summary(lm(Balance_Cory_Denom$Actynomyces ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Aggregatibacter ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Akkermansia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Allistipes ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Atopobium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bacteroides ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Barnesiella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bifidobacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Blautia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bulleidia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Campylobacter ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Capnocytophaga ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Clostridiales ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Dorea ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Eikenella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Faecalibacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Fusobacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Granulicatella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Haemophilus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Lachnospiraceae ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Lactobacillus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Neisseria ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Parabacteroides ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Parviromonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Peptostreptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Porphyromonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Prevotella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Prevotella2 ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Propionibacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Pseudomonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Roseburia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Rothia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Ruminococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Ruminococcaceae ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Selomonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Shigella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Slackia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Staphylococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Streptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Suterella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Tannerella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Treponema ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Veillonella ~ Contents, data=Balance_Cory_Denom))

##############################################################################################################################

#Balances for Ruminococcaceae as the denominator at visit 3

#Testing out the analysis using balances

library(data.table)
library(lme4)

#The lme4 package suppresses p-values, so it's been sugested to install a package called afex to get them
#https://stats.stackexchange.com/questions/22988/how-to-obtain-the-p-value-check-significance-of-an-effect-in-a-lme4-mixed-mode

install.packages("afex")
library("afex")

Balance_Cory_Denom <- read.table('/Users/guillaumeonyeaghala/Downloads/Aspirin_Oral_Map_Ruminococcaceae.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_Cory_Denom)

summary(lm(Balance_Cory_Denom$Actinomyces ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Aggregatibacter ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Akkermansia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Alistipes ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Atopobium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bacteroides ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Barnesiella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bifidobacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Blautia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bulleidia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Butyrovibrio ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Campylobacter ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Capnocytophaga ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Corynebacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Dorea ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Eikenella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Fusobacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Granulicatella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Haemophilus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Lachnospiraceae ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Lactobacillus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Neisseria ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Parabacteroides ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Parvimonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Peptostreptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Porphyromonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Prevotella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Prevotella2 ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Propionibacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Pseudomonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Roseburia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Rothia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Selomonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Shigella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Slackia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Staphylococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Streptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Suterella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Tannerella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Treponema ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Veillonella ~ Contents, data=Balance_Cory_Denom))

#Balances for Eikenella as the denominator at visit 3

#Testing out the analysis using balances

library(data.table)
library(lme4)

#The lme4 package suppresses p-values, so it's been sugested to install a package called afex to get them
#https://stats.stackexchange.com/questions/22988/how-to-obtain-the-p-value-check-significance-of-an-effect-in-a-lme4-mixed-mode

install.packages("afex")
library("afex")

Balance_Cory_Denom <- read.table('/Users/guillaumeonyeaghala/Downloads/Aspirin_Oral_Map_Eikenella.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_Cory_Denom)

summary(lm(Balance_Cory_Denom$Actinomyces ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Aggregatibacter ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Akkermansia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Alistipes ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Atopobium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bacteroides ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Barnesiella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bifidobacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Blautia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bulleidia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Butyrivibrio ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Campylobacter ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Capnocytophaga ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Clostridiales ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Corynebacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Dorea ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Faecalibacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Fusobacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Granulicatella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Haemophilus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Lachnospiraceae ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Lactobacillus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Neisseria ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Parabacteroides ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Parvimonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Peptostreptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Porphyromonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Prevotella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Prevotella2 ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Propionibacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Pseudomonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Roseburia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Rothia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Ruminococcaceae~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Ruminococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Selenomonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Shigella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Slackia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Staphylococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Streptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Sutterella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Tannerella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Treponema ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Veillonella ~ Contents, data=Balance_Cory_Denom))

##############################################################################################################################
##############################################################################################################################
##############################################################################################################################

#Updated regression analyses on filtered datasets with pseudocounts, and added v1 pseudocounts for LME testing

#Balances for Aggregatibacter as the denominator at visit 3

#Testing out the analysis using balances

library(data.table)
library(lme4)

#The lme4 package suppresses p-values, so it's been sugested to install a package called afex to get them
#https://stats.stackexchange.com/questions/22988/how-to-obtain-the-p-value-check-significance-of-an-effect-in-a-lme4-mixed-mode

install.packages("afex")
library("afex")

Balance_Cory_Denom <- read.table('/Volumes/OBSIDIANJR/Balance Analysis/14_MAR2020_Balances_regressions/Aspirin_Oral_Balance_V3_Aggregatibacter.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_Cory_Denom)

summary(lm(Balance_Cory_Denom$Actinomyces ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bifidobacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bacteroides ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Butyrivibrio ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Campylobacter ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Capnocytophaga ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Corynebacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Fusobacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Gemella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Granullicatella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Haemophilus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Lachnospiraceae ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Neisseria ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Parvimonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Peptostreptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Porphyromonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Prevotella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Rothia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Ruminococcaceae~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Selenomonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Streptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Tannerella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Treponema ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Veillonella ~ Contents, data=Balance_Cory_Denom))

##############################################################################################################################

#Testing the LME without an interaction term

Balance_T <- read.table('/Volumes/OBSIDIANJR/Balance Analysis/14_MAR2020_Balances_regressions/Aspirin_Oral_Balance_V1V3_Aggregatibacter.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_T)

Balance_T$Collection.factor <-as.factor(Balance_T$Collection)

fre.balance<- lmer(Actinomyces~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Bifidobacterium~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Butyrivibrio~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Campylobacter~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Capnocytophaga~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Corynebacterium~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Fusobacterium~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Gemella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Granulicatella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Haemophilus~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Lachnospiraceae~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Neisseria~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Parvimonas~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Peptostreptococcus~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Porphyromonas~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Prevotella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Rothia~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Ruminococcaceae~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Selenomonas~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Streptococcus~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Tannerella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Treponema~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Veillonella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

##############################################################################################################################

Balance_T <- read.table('/Volumes/OBSIDIANJR/Balance Analysis/14_MAR2020_Balances_regressions/Aspirin_Oral_Balance_V1V3_Aggregatibacter.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_T)

Balance_T$Collection.factor <-as.factor(Balance_T$Collection)

fre.balance<- lmer(Actinomyces~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Bifidobacterium~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Butyrivibrio~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Campylobacter~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Capnocytophaga~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Corynebacterium~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Fusobacterium~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Gemella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Granulicatella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Haemophilus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Lachnospiraceae~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Neisseria~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Parvimonas~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Peptostreptococcus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Porphyromonas~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Prevotella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Rothia~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Ruminococcaceae~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Selenomonas~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Streptococcus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Tannerella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Treponema~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Veillonella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

##############################################################################################################################
##############################################################################################################################
##############################################################################################################################

#Updated regression analyses on filtered datasets with pseudocounts, and added v1 pseudocounts for LME testing

#Balances for Ruminococcaceae as the denominator at visit 3

#Testing out the analysis using balances

library(data.table)
library(lme4)

#The lme4 package suppresses p-values, so it's been sugested to install a package called afex to get them
#https://stats.stackexchange.com/questions/22988/how-to-obtain-the-p-value-check-significance-of-an-effect-in-a-lme4-mixed-mode

install.packages("afex")
library("afex")

Balance_Cory_Denom <- read.table('/Volumes/OBSIDIANJR/Balance Analysis/14_MAR2020_Balances_regressions/Aspirin_Oral_Balance_V3_Ruminococcaceae.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_Cory_Denom)

summary(lm(Balance_Cory_Denom$Actinomyces ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Aggregatibacter ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bifibacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bacteroides ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Butyrivibrio ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Campylobacter ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Capnocytophaga ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Corynebacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Fusobacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Gemella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Granulicatella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Haemophilus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Lachnospiraceae ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Neisseria ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Parvimonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Peptostreptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Porphyromonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Prevotella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Rothia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Selenomonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Streptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Tannerella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Treponema ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Veillonella ~ Contents, data=Balance_Cory_Denom))

##############################################################################################################################

#Testing the LME without an interaction term

Balance_T <- read.table('/Volumes/OBSIDIANJR/Balance Analysis/14_MAR2020_Balances_regressions/Aspirin_Oral_Balance_V1V3_Ruminococcaceae.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_T)

Balance_T$Collection.factor <-as.factor(Balance_T$Collection)

fre.balance<- lmer(Actinomyces~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Aggregatibacter~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Bifidobacterium~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Butyrivibrio~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Campylobacter~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Capnocytophaga~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Corynebacterium~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Fusobacterium~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Gemella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Granullicatella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Haemophilus~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Lachnospiraceae~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Neisseria~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Parvimonas~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Peptostreptococcus~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Porphyromonas~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Prevotella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Rothia~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Ruminococcaceae~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Selenomonas~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Streptococcus~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Tannerella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Treponema~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Veillonella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

##############################################################################################################################

Balance_T <- read.table('/Volumes/OBSIDIANJR/Balance Analysis/14_MAR2020_Balances_regressions/Aspirin_Oral_Balance_V1V3_Ruminococcaceae.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_T)

Balance_T$Collection.factor <-as.factor(Balance_T$Collection)

fre.balance<- lmer(Actinomyces~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Aggregatibacter~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Bifidobacterium~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Butyrivibrio~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Campylobacter~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Capnocytophaga~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Corynebacterium~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Fusobacterium~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Gemella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Granullicatella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Haemophilus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Lachnospiraceae~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Neisseria~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Parvimonas~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Peptostreptococcus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Porphyromonas~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Prevotella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Rothia~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Selenomonas~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Streptococcus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Tannerella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Treponema~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Veillonella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

##############################################################################################################################
##############################################################################################################################
##############################################################################################################################

#Updated regression analyses on filtered datasets with pseudocounts, and added v1 pseudocounts for LME testing

#Balances for Corynebacterium as the denominator at visit 3

#Testing out the analysis using balances

library(data.table)
library(lme4)

#The lme4 package suppresses p-values, so it's been sugested to install a package called afex to get them
#https://stats.stackexchange.com/questions/22988/how-to-obtain-the-p-value-check-significance-of-an-effect-in-a-lme4-mixed-mode

install.packages("afex")
library("afex")

Balance_Cory_Denom <- read.table('/Volumes/OBSIDIANJR/Balance Analysis/14_MAR2020_Balances_regressions/Aspirin_Oral_Balance_V3_Corynebacterium.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_Cory_Denom)

summary(lm(Balance_Cory_Denom$Actinomyces ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bifidobacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Bacteroides ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Butyrivibrio ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Campylobacter ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Capnocytophaga ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Aggregatibacter ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Fusobacterium ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Gemella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Granullicatella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Haemophilus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Lachnospiraceae ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Neisseria ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Parvimonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Peptostreptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Porphyromonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Prevotella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Rothia ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Ruminococcaceae~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Selenomonas ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Streptococcus ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Tannerella ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Treponema ~ Contents, data=Balance_Cory_Denom))
summary(lm(Balance_Cory_Denom$Veillonella ~ Contents, data=Balance_Cory_Denom))

##############################################################################################################################

#Testing the LME without an interaction term

Balance_T <- read.table('/Volumes/OBSIDIANJR/Balance Analysis/14_MAR2020_Balances_regressions/Aspirin_Oral_Balance_V1V3_Corynebacterium.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_T)

Balance_T$Collection.factor <-as.factor(Balance_T$Collection)

fre.balance<- lmer(Actinomyces~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Bifidobacterium~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Butyrivibrio~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Campylobacter~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Capnocytophaga~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Aggregatibacter~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Fusobacterium~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Gemella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Granulicatella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Haemophilus~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Lachnospiraceae~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Neisseria~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Parvimonas~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Peptostreptococcus~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Porphyromonas~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Prevotella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Rothia~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Ruminococcaceae~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Selenomonas~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Streptococcus~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Tannerella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Treponema~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Veillonella~ Contents+Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

##############################################################################################################################

Balance_T <- read.table('/Volumes/OBSIDIANJR/Balance Analysis/14_MAR2020_Balances_regressions/Aspirin_Oral_Balance_V1V3_Aggregatibacter.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_T)

Balance_T$Collection.factor <-as.factor(Balance_T$Collection)

fre.balance<- lmer(Actinomyces~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Bifidobacterium~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Butyrivibrio~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Campylobacter~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Capnocytophaga~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Corynebacterium~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Fusobacterium~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Gemella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Granulicatella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Haemophilus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Lachnospiraceae~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Neisseria~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Parvimonas~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Peptostreptococcus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Porphyromonas~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Prevotella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Rothia~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Ruminococcaceae~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Selenomonas~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Streptococcus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Tannerella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Treponema~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(Veillonella~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

##############################################################################################################################

#Testing out the analysis using balances, Dr. Demmer requested that we avoid using Ruminococcaceae as a denominator. I looked at the suggested corynebacterium to treponema ratio, and altho it was different in the ASA vs PCB group, there were only 8 observations (likely owing to these plaque organisms not being easily detectable on saliva/tongueswabs)

library(data.table)
library(lme4)

#The lme4 package suppresses p-values, so it's been sugested to install a package called afex to get them
#https://stats.stackexchange.com/questions/22988/how-to-obtain-the-p-value-check-significance-of-an-effect-in-a-lme4-mixed-mode

install.packages("afex")
library("afex")

#Testing the LME without an interaction term

Balance_T <- read.table('/Volumes/OBSIDIANJR/Balance Analysis/14_MAR2020_Balances_regressions/Aspirin_Oral_Balance_V1V3_Corynebacterium.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_T)

Balance_T <- read.table('/Volumes/RAVPOWER2/Temp_ASMICORAL/Aspirin_Oral_Map_Balances_DEC2023.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_T)

Balance_T$Collection.factor <-as.factor(Balance_T$Collection)

fre.balance<- lmer(d_prevotella_n_neiseria~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_prevotella_n_streptococcus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_prevotella_n_actinomyces~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_prevotella_n_rothia~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_veillonella_n_rothia~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_veillonella_n_actinomyces~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_veillonella_n_streptococcus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_veillonella_n_neisseria~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_fusobacterium_n_neisseria~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_fusobacterium_n_streptococcus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_fusobacterium_n_actinyomyces~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_fusobacterium_n_rothia~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_porphyromonas_n_rothia~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_porphyromonas_n_actinomyces~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_porphyromonas_n_streptococcus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_porphyromonas_n_neisseria~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_treponema_n_rothia~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_treponema_n_actinomyces~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_treponema_n_streptococcus~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_treponema_n_neisseria~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_treponema_n_corynebacterium~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

*Generating additional metrics for the balance table (from /Volumes/RAVPOWER2/Temp_ASMICORAL/3_Dissertation Analysis/3_Dissertation Outputs/22_AACR ASMIC ORAL POSTER/30_AACR_FEB2020_Outputs_Updated.xlsx)
*https://rdrr.io/cran/samplingbook/man/stratamean.html
stratamean(y=Balance_T$d_fusobacterium_n_neisseria, h=as.vector(Balance_T$Class), eae=TRUE)

##############################################################################################################################

#Testing out the analysis using balances, Dr. Demmer requested that we avoid using Ruminococcaceae as a denominator. I looked at the suggested corynebacterium to treponema ratio, and altho it was different in the ASA vs PCB group, there were only 8 observations (likely owing to these plaque organisms not being easily detectable on saliva/tongueswabs). Repeating the analysis after having added a pseudocount to all observations at Dr.Demmer's request

library(data.table)
library(lme4)

#The lme4 package suppresses p-values, so it's been sugested to install a package called afex to get them
#https://stats.stackexchange.com/questions/22988/how-to-obtain-the-p-value-check-significance-of-an-effect-in-a-lme4-mixed-mode

install.packages("afex")
library("afex")

#Testing the LME without an interaction term

Balance_T <- read.table('/Volumes/RAVPOWER2/Temp_ASMICORAL/Aspirin_Oral_Map_Balances_DEC2023.txt', sep='\t', comment='', head=T, row.names=1)
names(Balance_T)

Balance_T$Collection.factor <-as.factor(Balance_T$Collection)

fre.balance<- lmer(d_treponema_n_corynebacterium_2~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_porphyromonas_n_actinomyces_2~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)

fre.balance<- lmer(d_fusobacterium_n_neisseria_2~ Contents*Collection.factor+ (1 | Subject_ID), data = Balance_T, subset = Collection %in% c(1, 2))
summary(fre.balance)