---
title: "UTMB MTX data prep"
output: html_document
date: "2025-06-20"
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

#You will NEED to go the R markdown file "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/R_43_Example_RMarkdown_basic.Rmd" to follow along

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

#Using left_join() (from dplyr):
#The dplyr package (part of the tidyverse) offers a more readable alternative.
#The left_join() function is specifically designed for this type of join.
#Syntax: left_join(x, y, by = "common_column"), where x is the left data frame and y is the right, and by specifies the join column(s

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

#install.packages("rmarkdown")
#install.packages("dplyr")
#install.packages("ggplot2")
#install.packages("GeomxTools")
#install.packages("tidyverse")
#install.packages("readxl")
#install.packages("ggpubr")
#https://stackoverflow.com/questions/19414605/export-data-from-r-to-excel
#install.packages("openxlsx")
```

## Load data {.hidden .unlisted .unnumbered}

```{r}
#| echo = FALSE, 
#| results='hide', warning=FALSE

#setwd("/Users/Scottiejj/Desktop/Spatial RoI/")
#setwd("/Users/Scottiejj/Desktop/Spatial RoI/")
#setwd("/Users/guillaumeonyeaghala/Desktop/GeoMX Project Summer 2024/Analysis_GO/")
setwd('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/')
#library(SpatialDecon)
#library(GeomxTools)
library(rmarkdown)
library(tidyverse)
library(openxlsx)
library(readxl)
library(dplyr)
library(ggplot2)
library(ggpubr)

```

#Merging the PK and mtx sample ID data for Kajal to pull samples {.hidden .unlisted .unnumbered}
```{r}

#Importing PK demographic and clinical data from Moataz

#gf.data<-read_xlsx("00_clinical data for biopsies, 6-18-2024.xlsx",sheet = 1)
#gf.data2<-read_xlsx("00_cgd_demo_data_20241029.xlsx",sheet = 'cgd_demo_data_20241029')

pk.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_Democlinical_prep.xlsx",sheet = 'Demographics_mtz')

sample.id.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_04_Shotgun_list_HUMANN_sorting_collection_date_time_V10_prep.xlsx",sheet = 'mgx_mtx_match')

#subsetting the dataset to only the significant cell types
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/

#prop_long_p_sub<-prop_long%>%
#  filter(Cell %in% c("macrophages","B.memory","plasma","T.CD4.naive","T.CD8.naive","T.CD8.memory",
#                     "pDCs","mDCs","monocytes.C","monocytes.NC.I","neutrophils","Treg","edothelial.cells","fibroblasts"))

sample.id.data_sub<-sample.id.data%>%
  filter(Month %in% c("PK"))

#renaming the pid column and then subsetting the dataset to only the PID and graft failure variables
#names(gf.data)[1]<-"PID"
#names(gf.data)[2]<-"PID"
#gf.data<-gf.data%>%dplyr::rename(GF=graftfail_usrds)%>%select(PID,GF)%>%mutate(GF=as.factor(GF))

names(pk.data)
names(pk.data)[1]<-"PID"
pk.data_sub<-pk.data%>%dplyr::select(PID,date_PKvisit,date_tx,predom_race,donar_status,MPA_AUC5_12_0_12,MPA_EHR_peaks_time,MPA_EHR_Peaks_count)

#Pooling the two datasets (https://www.datacamp.com/doc/r/merging)

# merge two data frames by ID
#total <- merge(data frameA,data frameB,by="ID")

sample_pull_test <-merge(pk.data_sub,sample.id.data_sub,by="PID")

#Exporting the dataset for Kajal as a tab separated text file

#write.table(sample_pull_test, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_innovation_mpa_samples", sep = "\t", row.names = FALSE)
```

#Combining the prospective and cross-sectional REDCAP Data {.hidden .unlisted .unnumbered}

```{r}

ps.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_01_mission_demo_ps_toshare_04012024.xlsx",sheet = 'mission_demo_ps_toshare_0401202')

names (ps.data)

#Subset the variables to combine (save this set for ps specific analyses)
#ps.data.2<-ps.data%>%select(record_id, site, age, sex, ethnicity, predom_race, donor_type, birth_yr_ld, sex_ld, ethnicity_ld, race_ld, birth_yr_dd, sex_dd, ethnicity_dd, race_dd, height_baseline, weight_baseline_kg, bmi_baseline, smoking, diabetes_pretx, age_at_diabetic_pretx, highbp, myocardialinf, strok, hyperlipidemia_drug, height_donor_ld, weight_donor_ld, donor_scr_avail, donor_scr_value, dt_admissn_scr_ld, donor_relationship, donor_relation_type, relation_type_other, donor_bp, cmv_donor_igg, eb_donor, hepb_donor, hepc_donor, height_donor_dd, weight_donor_dd, scr_donor_avail_dd, donor_scr_value_dd, dt_final_scr_dd, donor_death_cause, cause_death_other, donor_bp_dd, dcd, ecd, dd_donor_diabetes, kdpi, cmv_dd_donor, eb_dd_donor, hepb_dd_donor, hepc_dd_donor, donor_scr_availf, tx_dt, dt_hosp_discharge_posttx, esrd_cause, esrd_cause_glom_dis, esrd_cause_glom_dis_other, esrd_other, cause_esrd_notes, esrd_biopsy_confirm, prev_tx, donor_type_prevtx, preemptive_tx, initial_dialysis, spk, cmv_recipient, ebv_recipient, hepb_recipient, hepc_recipient, med_dt_baseline, htn_baseline, lipid_baseline, acidreducer_baseline, antiviral_baseline, antifungal_baseline, antibiotics_baseline, probiotx_baseline , immunospu_baseline, cold_ischem_time, abx_preop, pra_test, pra_dt, cpra, pra_method, date_of_pk_visit, height_at_pk,  weight_at_pk, a1, a2, b1, b2, dr1, dr2, a1_donor, a2_donor, b1_donor, b2_donor, dr1_donor, dr2_donor, hlamismatch, nummismatch, dialysis_post_tx, dt_dialysis_posttx, reason_dialysis_posttx, other_diaysis_posttx, dt_last_dialysis, dialysis_post_tx_count, data_entered_byc1, c1_postoperative_information_com, scr_day5_avail, scr_day7_avail, date_creatinine_day5, creatinine_day5, date_creatinine_day7, creatinine_day7, txn_dt_pk, pk_event_dt, days_after_posttx_pk, date_lab_pk, time_lab_pk, glucose, urea_nitrogen_bun, scr_value_pk, sodium, potassium, chloride, carbondioxide, bun, total_protein, alb_value, alkaline_phosphatase_alp, ast, alt_value, bilirubin_total, urine_crcl, data_entered_byf3, f3_lab_data_for_mmf_pk_assessmen, bun_num, urine_crcl_num, max_dt_lastvisit, max_dt_biosample, cmv_donor, cmvnew, cmv_grp, age_donor_group, age_recip_group, pcausenew, coldtime_cat, birth_yr_donor, age_donor, age_at_tx, crosstorbcell, PRA_pos, cv_base, ca_base, dial_7days, dgf, commoncloseoutdate, lastdate, maxfupdate, fup_days, fup_years, age_recip_group_report)

ps.data.2<-ps.data%>%select(record_id, site, age, sex, ethnicity, predom_race, donor_type, birth_yr_ld, sex_ld, ethnicity_ld, race_ld, birth_yr_dd, sex_dd, ethnicity_dd, race_dd, height_baseline, weight_baseline_kg, bmi_baseline, smoking, diabetes_pretx, age_at_diabetic_pretx, myocardialinf, strok, height_donor_ld, weight_donor_ld, donor_scr_avail, donor_scr_value, dt_admissn_scr_ld, donor_relationship, donor_relation_type, relation_type_other, donor_bp, cmv_donor_igg, eb_donor, hepb_donor, hepc_donor, height_donor_dd, weight_donor_dd, scr_donor_avail_dd, donor_scr_value_dd, dt_final_scr_dd, donor_death_cause, cause_death_other, donor_bp_dd, dcd, ecd, dd_donor_diabetes, kdpi, cmv_dd_donor, donor_scr_availf, tx_dt, dt_hosp_discharge_posttx, esrd_cause, esrd_cause_glom_dis, esrd_cause_glom_dis_other, esrd_other, cause_esrd_notes, esrd_biopsy_confirm, prev_tx, donor_type_prevtx, preemptive_tx, initial_dialysis, spk, cmv_recipient, ebv_recipient, hepb_recipient, hepc_recipient, med_dt_baseline, htn_baseline, lipid_baseline, acidreducer_baseline, antiviral_baseline, antifungal_baseline, antibiotics_baseline, probiotx_baseline, immunospu_baseline, cold_ischem_time, pra_test, pra_dt, cpra, pra_method, a1, a2, b1, b2, dr1, dr2, a1_donor, a2_donor, b1_donor, b2_donor, dr1_donor, dr2_donor, hlamismatch, nummismatch, dialysis_post_tx, dt_dialysis_posttx, reason_dialysis_posttx, other_diaysis_posttx, dt_last_dialysis, dialysis_post_tx_count, data_entered_byc1, c1_postoperative_information_com, date_lab_pk, time_lab_pk, glucose, urea_nitrogen_bun, scr_value_pk, sodium, potassium, chloride, carbondioxide, bun, total_protein, alb_value, alkaline_phosphatase_alp, ast, alt_value, bilirubin_total, urine_crcl, data_entered_byf3, f3_lab_data_for_mmf_pk_assessmen, bun_num, urine_crcl_num, max_dt_lastvisit, max_dt_biosample, cmv_donor, cmvnew, cmv_grp, age_donor_group, age_recip_group, pcausenew, coldtime_cat, birth_yr_donor, age_donor, age_at_tx, crosstorbcell, PRA_pos, cv_base, ca_base, dial_7days, dgf, commoncloseoutdate, lastdate, maxfupdate, fup_days, fup_years, age_recip_group_report)

#adding indicator variable for cohort type

ps.data.2$cohort_type<-"Prospective"

#Cross-sectional cohort
  
cs.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_01_mission_demo_cs_toshare_04012024.xlsx",sheet = 'mission_demo_cs_toshare_0401202')

names(cs.data)

#Subset the variables to combine
cs.data.2<-cs.data%>%select(record_id, site, age, sex, ethnicity, predom_race, donor_type, birth_yr_ld, sex_ld, ethnicity_ld, race_ld, birth_yr_dd, sex_dd, ethnicity_dd, race_dd, height_baseline, weight_baseline_kg, bmi_baseline, smoking, diabetes_pretx, age_at_diabetic_pretx, myocardialinf, strok, height_donor_ld, weight_donor_ld, donor_scr_avail, donor_scr_value, dt_admissn_scr_ld, donor_relationship, donor_relation_type, relation_type_other, donor_bp, cmv_donor_igg, eb_donor, hepb_donor, hepc_donor, height_donor_dd, weight_donor_dd, scr_donor_avail_dd, donor_scr_value_dd, dt_final_scr_dd, donor_death_cause, cause_death_other, donor_bp_dd, dcd, ecd, dd_donor_diabetes, kdpi, cmv_dd_donor, donor_scr_availf, tx_dt, dt_hosp_discharge_posttx, esrd_cause, esrd_cause_glom_dis, esrd_cause_glom_dis_other, esrd_other, cause_esrd_notes, esrd_biopsy_confirm, prev_tx, donor_type_prevtx, preemptive_tx, initial_dialysis, spk, cmv_recipient, ebv_recipient, hepb_recipient, hepc_recipient, med_dt_baseline, htn_baseline, lipid_baseline, acidreducer_baseline, antiviral_baseline, antifungal_baseline, antibiotics_baseline, probiotx_baseline, immunospu_baseline, cold_ischem_time, pra_test, pra_dt, cpra, pra_method, a1, a2, b1, b2, dr1, dr2, a1_donor, a2_donor, b1_donor, b2_donor, dr1_donor, dr2_donor, hlamismatch, nummismatch, dialysis_post_tx, dt_dialysis_posttx, reason_dialysis_posttx, other_diaysis_posttx, dt_last_dialysis, dialysis_post_tx_count, data_entered_byc1, c1_postoperative_information_com, date_lab_pk, time_lab_pk, glucose, urea_nitrogen_bun, scr_value_pk, sodium, potassium, chloride, carbondioxide, bun, total_protein, alb_value, alkaline_phosphatase_alp, ast, alt_value, bilirubin_total, urine_crcl, data_entered_byf3, f3_lab_data_for_mmf_pk_assessmen, bun_num, urine_crcl_num, max_dt_lastvisit, max_dt_biosample, cmv_donor, cmvnew, cmv_grp, age_donor_group, age_recip_group, pcausenew, coldtime_cat, birth_yr_donor, age_donor, age_at_tx, crosstorbcell, PRA_pos, cv_base, ca_base, dial_7days, dgf, commoncloseoutdate, lastdate, maxfupdate, fup_days, fup_years, age_recip_group_report)

#adding indicator variable for cohort type

cs.data.2$cohort_type<-"Cross-sectional"

#Pooling the two datasets (https://www.datacamp.com/doc/r/merging)

mission.data.comb <-rbind(ps.data.2, cs.data.2)

```

#Importing the diet and diarrhea data (It is only on prospective participants) {.hidden .unlisted .unnumbered}

```{r}

#Diet data
diet.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_02_F03_prospective_master_mission_prospective_diet_master_24honly_updated_PC variables.xlsx",sheet = 'B_F03_prospective_master_missio')

#renaming the pid column and then subsetting the dataset to only the PID and graft failure variables
#names(gf.data)[1]<-"PID"
#names(gf.data)[2]<-"PID"

names(diet.data)
names(diet.data)[2]<-"record_id"

#adding indicator variable for cohort type

diet.data$diet_check<-"NDSR"

#Diarrhea data
mosio.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_02_E03_MOSIO Data for analysis_05092024_postexclusion_days_Mergeprep.xlsx",sheet = 'Sheet1')

#renaming the pid column and then subsetting the dataset to only the PID and graft failure variables
#names(gf.data)[1]<-"PID"
#names(gf.data)[2]<-"PID"

names(mosio.data)
names(mosio.data)[1]<-"record_id"

#adding indicator variable for cohort type

mosio.data$mosio_check<-"MOSIO"

#merging the diet and mosio data to the combined mission dataset (https://www.datacamp.com/doc/r/merging)

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")
#(need a left join to keep all observations from misison.data.comb)

mission.diet.comb <-merge(mission.data.comb,diet.data,by="record_id",all.x=TRUE)
mission.diet.mosio.comb <-merge(mission.diet.comb,mosio.data,by="record_id",all.x=TRUE)

```

#Importing the PK variables provided by Moataz {.hidden .unlisted .unnumbered}

```{r}

#Importing PK demographic and clinical data from Moataz

#gf.data<-read_xlsx("00_clinical data for biopsies, 6-18-2024.xlsx",sheet = 1)
#gf.data2<-read_xlsx("00_cgd_demo_data_20241029.xlsx",sheet = 'cgd_demo_data_20241029')

pk.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_Democlinical_prep.xlsx",sheet = 'Demographics_mtz')

#renaming the pid column
#names(gf.data)[1]<-"PID"
#names(gf.data)[2]<-"PID"

names(pk.data)
names(pk.data)[1]<-"record_id"

#adding indicator variable for cohort type

pk.data$pk_check<-"PK"

#merging thepk data to the combined mission dataset (https://www.datacamp.com/doc/r/merging)

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")
#(need a left join to keep all observations from misison.data.comb)

mission.diet.mosio.pk.comb <-merge(mission.diet.mosio.comb,pk.data,by="record_id",all.x=TRUE)

```

#Switching from record_id to sample_id for all the mgx and mtx samples {.hidden .unlisted .unnumbered}

```{r}

#Importing the sample key data from the mgx and mtx data
#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. 
#In the mtx output, we have a PK sample for 4022 and an M1 sample for 4016 I will rename the M1 to PK in the month column to ease the merging process, the original data is in "Month_Notes)

sample.id.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_04_Shotgun_list_HUMANN_sorting_collection_date_time_V10_prep.xlsx",sheet = 'mgx_mtx_match')

#renaming the pid column (no need, record_id is already in the dataset)
#names(gf.data)[1]<-"PID"
#names(gf.data)[2]<-"PID"

names(sample.id.data)

#merging the combined mission dataset to the sample ID list for a master dataset (https://www.datacamp.com/doc/r/merging)

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")
#(need a left join to keep all observations from misison.data.comb)

mgx.mtx.mission.diet.mosio.pk.comb <-merge(sample.id.data,mission.diet.mosio.comb,by="record_id",all.x=TRUE)

#Exporting the dataset as a tab separated text file

#write.table(mgx.mtx.mission.diet.mosio.pk.comb, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_all_samples_democlinical_test", sep = "\t", row.names = FALSE)

```

#Importing the BGUS BLAST data and merging it with the PK data for analysis (first by sample ID, then by record ID) {.hidden .unlisted .unnumbered}

```{r}

#Importing BLAST BGUS data

bgus.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_05_00_Summarized_BGUS_Counts_prep.xlsx",sheet = '00_Summarized_BGUS_Counts')

#Importing the total reads data from MTX samples for relative abundance calculations

mtx.reads.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_05_00_Shotgun_list_HUMANN_sorting_collection_date_time_V10_readsprep.xlsx",sheet = 'BGUS_reads_match')

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")

bgus.reads.data <-merge(mtx.reads.data,bgus.data,by="id_match")

#Importing the sample key data from the mgx and mtx data
#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. 
#In the mtx output, we have a PK sample for 4022 and an M1 sample for 4016 I will rename the M1 to PK in the month column to ease the merging process, the original data is in "Month_Notes)

sample.id.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_04_Shotgun_list_HUMANN_sorting_collection_date_time_V10_prep.xlsx",sheet = 'mgx_mtx_match')

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")

bgus.reads.data.2 <-merge(bgus.reads.data,sample.id.data,by="id_match")

#Importing PK demographic and clinical data from Moataz

#gf.data<-read_xlsx("00_clinical data for biopsies, 6-18-2024.xlsx",sheet = 1)
#gf.data2<-read_xlsx("00_cgd_demo_data_20241029.xlsx",sheet = 'cgd_demo_data_20241029')

pk.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_Democlinical_prep.xlsx",sheet = 'Demographics_mtz')

#renaming the pid column
#names(gf.data)[1]<-"PID"
#names(gf.data)[2]<-"PID"

names(pk.data)
names(pk.data)[1]<-"record_id"

#adding indicator variable for cohort type

pk.data$pk_check<-"PK"

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")

names(bgus.reads.data.2)

bgus.reads.data.2$record_id
pk.data$record_id

pk.bgus.reads.data.2 <-merge(pk.data,bgus.reads.data.2,by="record_id")
```

#df_input_metadata for analysis (include the BLAST BGUS data into the PK democlinical dataset) {.hidden .unlisted .unnumbered}

##Merge the MPAEHR metadata file with the metatranscriptomics mapping file {.hidden .unlisted .unnumbered}

###Pull in the MPAEHR metadata file {.hidden .unlisted .unnumbered}

``` {r}

###Pull in the MPAEHR metadata file option 1

#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis
#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file
#democlinical<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. I will rename the M4 to PK to ease the merging process

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
#democlinical$Month[democlinical$Month=="M4"] <- "PK" 

###Pull in the MPAEHR metadata file option 2

#Importing PK demographic and clinical data from Moataz

#gf.data<-read_xlsx("00_clinical data for biopsies, 6-18-2024.xlsx",sheet = 1)
#gf.data2<-read_xlsx("00_cgd_demo_data_20241029.xlsx",sheet = 'cgd_demo_data_20241029')

pk.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_Democlinical_prep.xlsx",sheet = 'Demographics_mtz')

#renaming the pid column
#names(gf.data)[1]<-"PID"
#names(gf.data)[2]<-"PID"

names(pk.data)
names(pk.data)[1]<-"record_id"

#adding indicator variable for cohort type

pk.data$pk_check<-"PK"
```

##Merge the MPAEHR metadata file with the metatranscriptomics mapping file {.hidden .unlisted .unnumbered}

###Import the mapping file and merging with the clinical data{.hidden .unlisted .unnumbered}

``` {r}

###Import the mapping file and merging with the clinical data Option 1

#mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

#freq <-table(mtxmapping$Month)
#freq

#Filter it to only the PK observations
#mtxmapping.pk<-mtxmapping%>%filter(grepl("PK|pk",Month))

#Now match the PIDs from mtxmapping.pk to those in the democlinical file
#mtxmapping.pk<-mtxmapping.pk%>%filter(PID%in%democlinical$PID)
#democlinical.pk<-democlinical%>%filter(PID%in%mtxmapping.pk$PID)

#Merging both datasets to make the demographic dataset for the metatranscriptomics analysis
#mtxmapping.pk.all<-merge(mtxmapping.pk,democlinical.pk,by="PID") 

###Import the mapping file and merging with the clinical data Option 2

#Importing BLAST BGUS data

bgus.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_05_00_Summarized_BGUS_Counts_prep.xlsx",sheet = '00_Summarized_BGUS_Counts')

#Importing the total reads data from MTX samples for relative abundance calculations

mtx.reads.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_05_00_Shotgun_list_HUMANN_sorting_collection_date_time_V10_readsprep.xlsx",sheet = 'BGUS_reads_match')

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")

bgus.reads.data <-merge(mtx.reads.data,bgus.data,by="id_match")

#Importing the sample key data from the mgx and mtx data
#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. 
#In the mtx output, we have a PK sample for 4022 and an M1 sample for 4016 I will rename the M1 to PK in the month column to ease the merging process, the original data is in "Month_Notes)

sample.id.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_04_Shotgun_list_HUMANN_sorting_collection_date_time_V10_prep.xlsx",sheet = 'mgx_mtx_match')

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")

bgus.reads.data.2 <-merge(bgus.reads.data,sample.id.data,by="id_match")

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")

names(bgus.reads.data.2)

bgus.reads.data.2$record_id
pk.data$record_id

pk.bgus.reads.data.2 <-merge(pk.data,bgus.reads.data.2,by="record_id")

#Renaming the dataframe for ease of use for the rest of the code

mtxmapping.pk.all<-pk.bgus.reads.data.2 

#Check which PS observations I am missing in the file 

bgus.data.check <-merge(bgus.data,sample.id.data,by="id_match")
bgus.reads.data.3 <-merge(bgus.data.check,mtx.reads.data,by="id_match")
pk.bgus.reads.data.3 <-merge(pk.data,bgus.reads.data.3,by="record_id")
mtxmapping.pk.all<-pk.bgus.reads.data.3 

#Exporting the dataset as a tab separated text file to compare with the PIDs from Moataz's PK demographic file: We are Missing 4016 (FR45043.5.r1_kneaddata_Abundance-CPM for PK), 4017(FR45049.4.r1_kneaddata_Abundance-CPM for PK), and 4018 (FR45081.1.r1_kneaddata_Abundance-CPM for M6, no PK or M4 available), and the BGUS data also included the PK myfortic 3001, 3003 and 3005, which is why the total number seemed to match at first

#write.table(mtxmapping.pk.all, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_mtx_mapping_test", sep = "\t", row.names = FALSE)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

mtxmapping.pk.all$metagx_id<-substring(mtxmapping.pk.all$metatx_id,1,7)
mtxmapping.pk.all$metatx_id_2<-substring(mtxmapping.pk.all$metatx_id,1,7)

#Generate PS and CS subdatasets for further analyses

#mgxmapping.pk.ps<-mgxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mgxmapping.pk.cs<-mgxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxpathmapping.pk.ps<-mtxpathmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxpathmapping.pk.cs<-mtxpathmapping.pk.all%>%filter(Cohort.x =="CS") #47

mgxmapping.pk.all<-mtxmapping.pk.all #81
mgxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort =="PS")  #33
mgxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort =="CS") #47

mtxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort =="PS")  #33
mtxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort =="CS") #47

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_all.txt", sep = "\t", row.names = FALSE)

```

#df_input_data (mtx_species) {.hidden .unlisted .unnumbered}
##creating PK only, PK_PS and PK_CS metatranscriptomics files {.hidden .unlisted .unnumbered}

``` {r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.species.xlsx",sheet = "Sheet1")

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.species.xlsx", sheet = "Sheet1")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

# Restore the object
mgxspecies <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

#Now transpose the data (unlist the column names and the row names to save them as vectors)

gene_list<-unlist((mgxspecies[,1]))
sample_list<-unlist((mgxspecies[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mgx.all<-data.frame(t(mgxspecies))

#Trying to get rid of the colnames that snuck in the data as the first row
mgx.all <- mgx.all %>% filter(!str_detect(1, "clade_name")) #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
str(mgx.all)
names(mgx.all)
rownames(mgx.all)<-colnames(mgxspecies) 
mgx.all<-mgx.all%>%filter(!grepl("s__Blautia_wexlerae",X1)) #option 2 with !grepl(this version worked)

colnames(mgx.all)<-gene_list
names(mgx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

mgx.all$metagx_id_str <- rownames(mgx.all)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgx.all<-mgx.all%>%relocate(metagx_id, .before=1)

#Create subset mtx files

mgx.pk<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.all$metagx_id)
mgx.ps<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.ps$metagx_id)
mgx.cs<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.cs$metagx_id)

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

```

#df_input_data (mtx_genefamilies) {.hidden .unlisted .unnumbered}
##creating PK only, PK_PS and PK_CS metatranscriptomics files {.hidden .unlisted .unnumbered}

``` {r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mtxec4<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.genefamilies.cpm.l4ec.names.xlsx",sheet = "Sheet1")

#mtxec4<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.genefamilies.cpm.l4ec.names.xlsx",sheet = "Sheet1")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")
#saveRDS(mtxec4, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mtxec4.rds")

# Restore the object
mtxec4 <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mtxec4.rds")

#Filter the dataset based on partial string using the stringr package
#https://stackoverflow.com/questions/59829668/filter-according-to-partial-match-of-string-variable-in-r

mtxec4.reduced <- mtxec4 %>% filter(!str_detect(Gene_family, "UNGROUPED|UNMAPPED|NO_NAME"))

#Note that the subgene families use the "g_" string so you can use that to reduce the EC data to the major family. Same with the "unclassified term"
mtxec4.reduced <- mtxec4 %>% filter(!str_detect(Gene_family, "UNGROUPED|UNMAPPED|NO_NAME|g_|unclassified"))

#Now transpose the data (unlist the column names and the row names to save them as vectors)

gene_list<-unlist((mtxec4.reduced[,1]))
sample_list<-unlist((mtxec4.reduced[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mtx.all<-data.frame(t(mtxec4.reduced))

#Trying to get rid of the colnames that snuck in the data
mtx.all <- mtx.all %>% filter(!str_detect(1, "Gene_family")) #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
str(mtx.all)
names(mtx.all)
rownames(mtx.all)<-colnames(mtxec4.reduced) 
mtx.all<-mtx.all%>%filter(!grepl("1.1.1.1: Alcohol_dehydrogenase",X1))  #option 2 with !grepl(this version worked)

colnames(mtx.all)<-gene_list
names(mtx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

#mgx.all$metagx_id_str <- rownames(mgx.all)
mtx.all$metatx_id_str <- rownames(mtx.all)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

#mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)
mtx.all$metatx_id_2<-substring(mtx.all$metatx_id_str,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mtx.all<-mtx.all%>%relocate(metatx_id_2, .before=1)

#Create subset mtx files
mtx.pk<-mtx.all%>%filter(metatx_id_2%in%mtxmapping.pk.all$metatx_id_2)
mtx.ps<-mtx.all%>%filter(metatx_id_2%in%mtxmapping.pk.ps$metatx_id_2)
mtx.cs<-mtx.all%>%filter(metatx_id_2%in%mtxmapping.pk.cs$metatx_id_2)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)


```

#df_input_data (mtx_pathabundance) {.hidden .unlisted .unnumbered}
##creating PK only, PK_PS and PK_CS metatranscriptomics files {.hidden .unlisted .unnumbered}

``` {r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mtxpath<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.pathabund.xlsx",sheet = "Sheet1")

#mtxpath<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.pathabund.xlsx",sheet = "Sheet1")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")
#saveRDS(mtxec4, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mtxec4.rds")
#saveRDS(mtxpath, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mtxpath.rds")

# Restore the object
mtxpath <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mtxpath.rds")


#Filter the dataset based on partial string using the stringr package
#https://stackoverflow.com/questions/59829668/filter-according-to-partial-match-of-string-variable-in-r

#mtxec4.reduced <- mtxec4 %>% filter(!str_detect(Gene_family, "UNGROUPED|UNMAPPED|NO_NAME"))
mtxpath.reduced <- mtxpath%>% filter(!str_detect(Pathway, "UNGROUPED|UNMAPPED|NO_NAME|UNINTEGRATED"))

#Note that the subgene families use the "g_" string so you can use that to reduce the EC data to the major family. Same with the "unclassified term"

#mtxec4.reduced <- mtxec4 %>% filter(!str_detect(Gene_family, "UNGROUPED|UNMAPPED|NO_NAME|g_|unclassified"))
mtxpath.reduced <- mtxpath%>% filter(!str_detect(Pathway, "UNGROUPED|UNMAPPED|NO_NAME|UNINTEGRATED|g_|unclassified"))

#Now transpose the data

#gene_list<-unlist((mtxec4.reduced[,1]))
#sample_list<-unlist((mtxec4.reduced[1,]))

gene_list<-unlist((mtxpath.reduced[,1]))
sample_list<-unlist((mtxpath.reduced[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

#mtx.all<-data.frame(t(mtxec4.reduced))
mtx.path.all<-data.frame(t(mtxpath.reduced))

#Trying to get rid of the colnames that snuck in the data
#mtx.all <- mtx.all %>% filter(!str_detect(1, "Gene_family")) #option 1 with str_detect, didn't work
mtx.path.all <- mtx.path.all %>% filter(!str_detect(1, "Pathway"))  #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
#str(mtx.all)
#names(mtx.all)
#rownames(mtx.all)<-colnames(mtxec4.reduced)

str(mtx.path.all)
names(mtx.path.all)
rownames(mtx.path.all)<-colnames(mtxpath.reduced)


#mtx.all<-mtx.all%>%filter(!grepl("1.1.1.1: Alcohol_dehydrogenase",X1))
mtx.path.all<-mtx.path.all%>%filter(!grepl("1CMET2-PWY: folate transformations III (E. coli)",X1)) 
mtx.path.all <- mtx.path.all %>% filter(!str_detect(1, "1CMET2-PWY: folate transformations III (E. coli)")) 
mtx.path.all <- mtx.path.all %>% filter(!str_detect(1, "Pathway"))

colnames(mtx.path.all)<-gene_list
names(mtx.path.all)

#colnames(mtx.all)<-gene_list
#names(mtx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file
mtx.path.all$metatx_path_id <- rownames(mtx.path.all)
#mtx.all$metatx_id <- rownames(mtx.all)

mtx.path.all<-mtx.path.all%>%filter(!grepl("Pathway",metatx_path_id))
#mtx.all<-mtx.all%>%filter(!grepl("1.1.1.1: Alcohol_dehydrogenase",X1))

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

#mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)
mtx.path.all$metatx_id_2<-substring(mtx.path.all$metatx_path_id,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mtx.path.all<-mtx.path.all%>%relocate(metatx_id_2, .before=1)

#Create subset mtx files

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#mtx.path.pk<-mtx.path.all%>%filter(metatx_path_id%in%mtxpathmapping.pk.all$metatx_path_id)
#mtx.path.ps<-mtx.path.all%>%filter(metatx_path_id%in%mtxpathmapping.pk.ps$metatx_path_id)
#mtx.path.cs<-mtx.path.all%>%filter(metatx_path_id%in%mtxpathmapping.pk.cs$metatx_path_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mtx.path.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_path_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.path.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_path_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.path.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_path_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.path.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_path_all.txt", sep = "\t", row.names = FALSE)

```

#Association between the BGUS BLAST data and MPA PK data {.hidden .unlisted .unnumbered}
## The necessary demoraphic and clinical data was included in mtxmapping.pk.all / pk.bgus.reads.data.3 {.hidden .unlisted .unnumbered}
### Full Cohort {.hidden .unlisted .unnumbered}

```{r}

#Calculating the relative abundance of BGUS reads in the data

pk.bgus.reads.data.3$BGUS_reads_adjusted <- pk.bgus.reads.data.3$BGUS_reads_sum/pk.bgus.reads.data.3$Total_reads

#Log transforming the reads "https://bookdown.org/rwnahhas/IntroToR/transform-a-numeric-variable.html"

hist(pk.bgus.reads.data.3$BGUS_reads_adjusted)

pk.bgus.reads.data.3$log_BGUS_reads_adjusted <- log(pk.bgus.reads.data.3$BGUS_reads_adjusted)

pk.bgus.reads.data.3$log2_BGUS_reads_adjusted <- log2(pk.bgus.reads.data.3$BGUS_reads_adjusted)

#also testing centered log ratios (https://stats.stackexchange.com/questions/305965/can-i-use-the-clr-centered-log-ratio-transformation-to-prepare-data-for-pca)
#Need to install the compositions package (https://search.r-project.org/CRAN/refmans/compositions/html/clr.html)
#bgus.data$clr_BGUS_reads_adjusted <- clr(bgus.data$BGUS_reads_adjusted)

#hist(bgus.data.2$log_BGUS_reads_adjusted)
#hist(bgus.data.2$log2_BGUS_reads_adjusted)
#hist(bgus.data.2$clr_BGUS_reads_adjusted)

#Association between cohort and bgus reads

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between cohort and MPA AUC

summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between cohort and MPA  5 to 12 AUC

summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between cohort and MPA EHR

summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between cohort and MPAG AUC 0 to 12 hours

summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between cohort and MPAG AUC 5 to 12 hours

summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between cohort and Acyl MPAG AUC 0 to 12 hours

summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between cohort and Acyl MPAG AUC 5 to 12 hours

summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12  ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between cohort and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPA_EHR_Peaks_count ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_EHR_Peaks_count ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA AUC and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA 5 to 12 AUC and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA EHR and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPAG AUC 0 to 12 hours and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPAG AUC 5 to 12 hours and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between Acyl MPAG AUC 0 to 12 hours and MPA peaks

summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between Acyl MPAG AUC 5 to 12 hours and MPA peaks

summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA peaks and cohort peaks

summary(lm(pk.bgus.reads.data.3$MPA_EHR_Peaks_count ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_EHR_Peaks_count ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Full cohort

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_0_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_0_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_0_12 * cohort, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_0_12 * cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA AUC 5 to 12

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_5_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_5_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_5_12 * cohort, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_5_12 * cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 * cohort, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 * cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 0 to 12 hours

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12 * cohort, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12 * cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 5 to 12 hours

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12 * cohort, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12 * cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Acyl MPAG AUC 0 to 12 hours

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12 * cohort, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12 * cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Acyl MPAG AUC 5 to 12 hours

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12 * MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12 * cohort, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12 * cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))
```

#Bringing in BGUS PWY data {.hidden .unlisted .unnumbered}

```{r}

#Association with BGUS pathway data

names(pk.bgus.reads.data.3)
names(mtx.path.all)

#Changing the variable names to something usable
mtx.path.all$GALACTUROCAT_PWY <-mtx.path.all[,43]
mtx.path.all$GLUCUROCAT_PWY <-mtx.path.all[,49]

#Subset the variables to combine
mtx.path.all.2<-mtx.path.all%>%select(metatx_id_2, GALACTUROCAT_PWY, GLUCUROCAT_PWY)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

#mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)

pk.bgus.reads.data.3$metatx_id_2<-substring(pk.bgus.reads.data.3$metatx_id,1,7)

pk.bgus.reads.data.4 <-merge(pk.bgus.reads.data.3,mtx.path.all.2,by="metatx_id_2")

#Association between BGUS reads and BGUS pathways in full cohort 

summary(lm(pk.bgus.reads.data.4$BGUS_reads_adjusted ~ GLUCUROCAT_PWY, data=pk.bgus.reads.data.4))

pk.bgus.reads.data.4$GLUCUROCAT_PWY_2 <-as.numeric(pk.bgus.reads.data.4$GLUCUROCAT_PWY)
pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 <- pk.bgus.reads.data.4$GLUCUROCAT_PWY_2/pk.bgus.reads.data.4$Total_reads

summary(lm(pk.bgus.reads.data.4$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=pk.bgus.reads.data.4))
summary(lm(pk.bgus.reads.data.4$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=pk.bgus.reads.data.4, subset = MPA_EHR_Peaks_count %in% c("0","2")))

cor.test(pk.bgus.reads.data.4$BGUS_reads_adjusted,pk.bgus.reads.data.4$GLUCUROCAT_PWY_3)
#cor.test(pk.bgus.reads.data.4$BGUS_reads_adjusted,pk.bgus.reads.data.4$GLUCUROCAT_PWY_3, subset = pk.bgus.reads.data.4$MPA_EHR_Peaks_count %in% c("0","2"))

pk.bgus.reads.data.4$log_GLUCUROCAT_PWY <- log(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 + 0.1)
summary(lm(pk.bgus.reads.data.4$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=pk.bgus.reads.data.4))
summary(lm(pk.bgus.reads.data.4$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=pk.bgus.reads.data.4, subset = MPA_EHR_Peaks_count %in% c("0","2")))



#subsetting the dataset 
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
bgus.data.ps<-pk.bgus.reads.data.4%>%
  filter(cohort %in% c("PROS"))

bgus.data.cs<-pk.bgus.reads.data.4%>%
  filter(cohort %in% c("CS"))

#Writing the file to Excel
#https://stackoverflow.com/questions/19414605/export-data-from-r-to-excel
#for writing a data.frame or list of data.frames to an xlsx file
write.xlsx(pk.bgus.reads.data.4, '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')




##Plotting 2 variables side by sides
#https://stackoverflow.com/questions/67536892/how-to-plot-two-histograms-of-different-variables-in-one-ggplot-with-legend-and
#https://www.sthda.com/english/wiki/ggplot2-axis-scales-and-transformations
#https://stackoverflow.com/questions/63003022/change-scale-on-x-axis-in-ggplot-in-r

#names(bgus.data.2)

#bgus.data.2$Uniref90_Beta_glucuronidase_cpm <- bgus.data.2$3_2_1_31_Beta_glucuronidase
#bgus.data.2$Blast_Beta_glucuronidase_cpm <- bgus.data.2$Blast_BGUS_cpm

#TwoHistos <- ggplot (bgus.data.2) +
#  labs(color="Variable name",x="BGUS transcripts in Copies per Million", y="Number of Samples")+
#  geom_histogram(aes(x=Uniref90_Beta_glucuronidase_cpm, fill= "Uniref90_Beta_glucuronidase_cpm"),  alpha = 0.2 ) + 
#  geom_histogram(aes(x=Blast_Beta_glucuronidase_cpm, fill= "Blast_Beta_glucuronidase_cpm"), alpha = 0.2) + 
#  scale_fill_manual(values = c("blue","red"))
#TwoHistos 

#TwoHistos + scale_x_continuous(n.breaks = 5)

#summary(bgus.data.2$Uniref90_Beta_glucuronidase_cpm)
#summary(bgus.data.2$Blast_Beta_glucuronidase_cpm)

#Pearson correlation figures for the abstract

cor.test(bgus.data.ps$BGUS_reads_adjusted,bgus.data.ps$MPA_EHR_Peaks_count)
#cor.test(bgus.data.ps$MPA_AUC5.12_0.12,bgus.data.ps$MPA_EHR_Peaks_count)
#cor.test(bgus.data.ps$MPA_AUC5.12_0.12,bgus.data.ps$BGUS_reads_adjusted)

#https://scientificallysound.org/2016/04/04/r-pearson-correlation/

scatter_plot <- ggplot(bgus.data.ps, aes(bgus.data.ps$BGUS_reads_adjusted, bgus.data.ps$MPA_EHR_Peaks_count))
scatter_plot

scatter_plot + geom_point() + labs(x = "βGUS transcripts", y = "Number of EHR peaks") + geom_smooth(method="lm")
```

### Prospective Cohort {.hidden .unlisted .unnumbered}

```{r}


#Stratified analyses, PS

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA AUC 5 to 12

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 0 to 12 hours

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 5 to 12 hours

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Acyl MPAG AUC 0 to 12 hours

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Acyl MPAG AUC 5 to 12 hours

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between BGUS reads and BGUS pathways 

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=bgus.data.ps))
summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

cor.test(bgus.data.ps$BGUS_reads_adjusted,bgus.data.ps$GLUCUROCAT_PWY_3)

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=bgus.data.ps))
summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

```

### Cross-sectional cohort {.hidden .unlisted .unnumbered}

```{r}

#Stratified analyses, CS

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Including Rory's suggested analysis

##MPA AUC 5 to 12

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 0 to 12 hours

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 5 to 12 hours

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Acyl MPAG AUC 0 to 12 hours

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Acyl MPAG AUC 5 to 12 hours

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log2_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between BGUS reads and BGUS pathways 

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=bgus.data.cs))
summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

cor.test(bgus.data.cs$BGUS_reads_adjusted,bgus.data.cs$GLUCUROCAT_PWY_3)

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=bgus.data.cs))
summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

```

#Data visualizations {.hidden .unlisted .unnumbered}

###read in ggplot data {.hidden .unlisted .unnumbered}

```{r}

#if (!requireNamespace("BiocManager", quietly = TRUE))
#    install.packages("BiocManager")
#BiocManager::install()
#BiocManager::install(c("phyloseq", "ggplot2"))
#BiocManager::install(c("seqgroup"))		
#install.packages("gitcreds")
#install.packages("credentials")
#install.packages("devtools")
#install.packages('readxl')
#install.packages('readr')
#install.packages('dplyr')
#install.packages("tidyverse")
#install.packages("igraph")
#install.packages("tidyverse")
#install.packages("igraph")
#install.packages("kableExtra")

```

###ggplot libraries {.hidden .unlisted .unnumbered}

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
library("kableExtra")
```

###setting up GGPLOT2 {.hidden .unlisted .unnumbered}

```{r}

#mgxmtxpk <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_mtx_mapping_pk_all_2.xlsx", sheet = "mgxmapping_pk_all")

mgxmtxpk <- read_excel("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_mgx_mtx_mapping_pk_all_2.xlsx", sheet = "mgxmapping_pk_all")
head(mgxmtxpk)

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')
```

###GGPLOT tutorial {.hidden .unlisted .unnumbered}

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

setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

#Loading dataset
#load("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/R_33_MIDUS_subset-1.Rdata")
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

#Dealing with missing variables (like the NA on the previous graph).
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

#rm(list=ls())

#Installing the packages
#install.packages("ggplot2")
#library(ggplot2)

#install.packages("dplyr")
#library(dplyr)

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

#setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

#Loading dataset
#load("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/R_33_MIDUS_subset-1.Rdata")

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

###Testing plots for mtx data (WTC2025 submission) {.hidden .unlisted .unnumbered}

```{r}

#mgxmtxpk <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_mtx_mapping_pk_all_2.xlsx", sheet = "mgxmapping_pk_all")

mgxmtxpk <- read_excel("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_mgx_mtx_mapping_pk_all_2.xlsx", sheet = "mgxmapping_pk_all")
head(mgxmtxpk)

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

#ggplot2::ggplot(mgxmtxpk, aes(x= Beta_glucuronidase)) + 
#  geom_histogram() #Second layer: Set the shape of each data point

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
#p<-ggplot(mgxmtxpk.plot_long, aes(x=EC4_protein_CPM_Value,color=Cohort.x))+
#  geom_histogram(position = "dodge") +
#  facet_wrap(EC4_protein ~ .)
#p

```

###testing visualizations for the metatx manuscript {.hidden .unlisted .unnumbered}

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

setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

#Plotting One continuous and one categorical variable
        
ggplot2::ggplot(pk.bgus.reads.data.4, aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_0_12)) + 
  geom_point() #set the categorical variable on X axis and the continuous variable as the Y axis

ggplot2::ggplot(pk.bgus.reads.data.4, aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_0_12)) + 
  geom_bar(stat="summary", fun="mean") #The previous view was not very informative, maybe we can use a bargraph instead. We will need to specify the statistic with the stat option and the function for the statistic with the fun option.

ggplot2::ggplot(pk.bgus.reads.data.4, aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(color=cohort)) #Geom_bar has two different coloring attributes: color and fill. Here we are using aes as we are mapping a color from the data

ggplot2::ggplot(pk.bgus.reads.data.4, aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort)) #Geom_bar has two different coloring attributes: color and fill. Fill controls the box color itself.

#Save the filter and ggplot base layer as an object

wmh <- pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_0_12)) 

wmh + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort))

wmh + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") #This can be changed to cluster the bars

#We can do a line graph

wmh + 
  geom_line() #this isn't useful, needs a summary

wmh + 
  geom_line(stat="summary", fun="mean") #We will need to specify the statistic with the stat option and the function for the statistic with the fun option.

wmh + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort)) #We can map to a specific categorical variable as well

wmh + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort)) #We can also make it more understandable by coloring each strata

#Back to bar graphs

wmh + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1") #We can use the scale option to adjust a specific mapping option (ie the option within aes, the aesthetic)

wmh + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", 
                    name = "Study Cohort") #We can rename our legend here too

wmh + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPA AUC between 0 and 12 hours",
        title = "MPA Area under the curve and MPA EHR peaks by study cohorts")

#Figure for MPA AUC 0 to 12 hours

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_0_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPA AUC between 0 and 12 hours",
        title = "MPA Area under the curve and MPA EHR peaks by study cohorts")

#Figure for MPA AUC 5 to 12 hours

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_5_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_5_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPA AUC between 5 and 12 hours",
        title = "MPA Area under the curve and MPA EHR peaks by study cohorts")

#Figure for MPA EHR

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC5_12_0_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC5_12_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPA EHR between 5 and 12 hours",
        title = "MPA EHR and MPA EHR peaks by study cohorts")

#Figure for MPAG AUC 0 to 12 hrs

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPAG_AUC_0_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPAG_AUC_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPAG AUC between 0 and 12 hours",
        title = "MPAG AUC and MPA EHR peaks by study cohorts")

#Figure for MPAG AUC 5 to 12 hrs

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPAG_AUC_5_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPAG_AUC_5_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPAG AUC between 0 and 12 hours",
        title = "MPAG AUC and MPA EHR peaks by study cohorts")

#Figure for Acyl MPAG AUC 0 to 12 hrs

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=AcMPAG_AUC_0_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=AcMPAG_AUC_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "Acyl MPAG AUC between 0 and 12 hours",
        title = "Acyl MPAG AUC and MPA EHR peaks by study cohorts")

#Figure for Acyl MPAG AUC 5 to 12 hrs

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=AcMPAG_AUC_5_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=AcMPAG_AUC_5_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "Acyl MPAG AUC between 5 and 12 hours",
        title = "Acyl MPAG AUC and MPA EHR peaks by study cohorts")

```

#DPLYR Tables
###Testing out code from "/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/R_23_Creating tables with dplyr.R" to make a table {.hidden .unlisted .unnumbered}

```{r}

library(kableExtra)

#setworking directory
#setwd("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/")

#read in the data

#dat <- read.csv("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/R_23_Data_for_table.csv")
dat <- read.csv("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_R_23_Data_for_table.csv")

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

###Reapeating the coding for generating a descriptive table for the categorical variables {.hidden .unlisted .unnumbered}

```{r}
#Loading the dataset
#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_crcl.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_crcl.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

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
  dplyr::select(variable, value, count_PS, percent_PS, count_CS, percent_CS) #%>% #You can use the select command in dplyr to force a certain order for your chosen variables
  
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

###Testing out dplyr to create a summary table for the poster {.hidden .unlisted .unnumbered}

```{r}
#clear the environment
#rm(list = ls())

#installing packages
#install.packages("dplyr")
#install.packages("tidyr")
#install.packages("kableExtra")

#Installing the labelled package to edit the summary statistics
#https://stackoverflow.com/questions/76341658/how-to-add-column-labels-when-piping-in-r-with-dplyr-and-labelled
#install.packages("labelled")

```

###Load the packages {.hidden .unlisted .unnumbered}

```{r}
#Load packages

library(dplyr)
library(tidyr)
library(kableExtra)
library(labelled) 

```

###Using the Full MISSION cohort data for the table {.hidden .unlisted .unnumbered}

```{r}

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_crcl.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_D10_missiondemoshotgunpscsV3_PK_maaslin2_V11_sigtaxa_newPCs_bgus_crcl.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#mgxmtxpk <- read_excel("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_mtx_mapping_pk_all.xlsx", sheet = "mgxmapping_pk_all")

mgxmtxpk <- read_excel("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_mgx_mtx_mapping_pk_all_2.xlsx", sheet = "mgxmapping_pk_all")

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

###Summarizing the continuous data {.hidden .unlisted .unnumbered}

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

###Demographic table summary for the metatx manuscript draft {.hidden .unlisted .unnumbered}

```{r}

#Set working directory
setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

#Check the data type for all the variable we want to combine
names(pk.bgus.reads.data.4)
summary(pk.bgus.reads.data.4)

#Summarising the variables of interest
#https://dplyr.tidyverse.org/reference/summarise.html

##Build the summaries from dplyr (chaining command) for continuous variables

dattab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, age_PK, gender, predom_race, eGFR_Epi_RF_Cr2021_1_73m2, total_bilirubin_mg_dL, albumin_g_dL, MPA_AUC_0_12, MPA_AUC_5_12, MPA_AUC5_12_0_12, MPAG_AUC_0_12, MPAG_AUC_5_12, AcMPAG_AUC_0_12, AcMPAG_AUC_5_12) %>%
  labelled::set_variable_labels(age_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_Epi_RF_Cr2021_1_73m2 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", MPA_AUC_5_12 = "MPA AUC5-12 hr, mg.h/L",
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)",
                                MPAG_AUC_0_12 = "MPAG AUC0-12 hr, mg.h/L", MPAG_AUC_5_12 = "MPAG AUC5-12 hr, mg.h/L",
                                AcMPAG_AUC_0_12 = "Acyl MPAG AUC0-12 hr, mg.h/L", AcMPAG_AUC_5_12 = "Acyl MPAG AUC5-12 hr, mg.h/L")  %>% 
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_age = mean(age_PK),
                   sd_age = sd(age_PK),
                   mean_eGFR = mean(eGFR_Epi_RF_Cr2021_1_73m2),
                   sd_eGFR = sd(eGFR_Epi_RF_Cr2021_1_73m2),
                   mean_bilirubin = mean(total_bilirubin_mg_dL),
                   sd_bilirubin = sd(total_bilirubin_mg_dL),
                   mean_albumin = mean(albumin_g_dL),
                   sd_albumin = sd(albumin_g_dL),
                   mean_MPA_0_12_AUC = mean(MPA_AUC_0_12),
                   sd_MPA_0_12_AUC = sd(MPA_AUC_0_12),
                   mean_MPA_5_12_AUC = mean(MPA_AUC_5_12),
                   sd_MPA_5_12_AUC = sd(MPA_AUC_5_12),
                   mean_EHR = mean(MPA_AUC5_12_0_12),
                   sd_EHR = sd(MPA_AUC5_12_0_12),
                   mean_MPAG_0_12_AUC = mean(MPAG_AUC_0_12),
                   sd_MPAG_0_12_AUC = sd(MPAG_AUC_0_12),
                   mean_MPAG_5_12_AUC = mean(MPAG_AUC_5_12),
                   sd_MPAG_5_12_AUC = sd(MPAG_AUC_5_12),
                   mean_AcMPAG_0_12_AUC = mean(AcMPAG_AUC_0_12),
                   sd_AcMPAG_0_12_AUC = sd(AcMPAG_AUC_0_12),
                   mean_AcMPAG_5_12_AUC = mean(AcMPAG_AUC_5_12),
                   sd_AcMPAG_5_12_AUC = sd(AcMPAG_AUC_5_12)) #%>% #Reshape the summarize output so that the case and control columns are side by side  
#  pivot_wider (names_from = cohort,
#               values_from = c(mean_age, sd_age, mean_eGFR, sd_eGFR, mean_bilirubin, sd_bilirubin,
#                               mean_albumin, sd_albumin, mean_MPA_0_12_AUC, sd_MPA_0_12_AUC, mean_MPA_5_12_AUC, mean_EHR, sd_EHR,
#                               sd_MPA_5_12_AUC, mean_MPAG_0_12_AUC, sd_MPAG_0_12_AUC, mean_MPAG_5_12_AUC, sd_MPAG_5_12_AUC,
#                               mean_AcMPAG_0_12_AUC, sd_AcMPAG_0_12_AUC, mean_AcMPAG_5_12_AUC, sd_AcMPAG_5_12_AUC)) %>% #You #have to use the concatenate option here so that the values appear side by side, not staggered.
#  dplyr::select(variable, value, count_Case, percent_Case, count_Control, percent_Control) %>% #You can use the select #command in dplyr to force a certain order for your chosen variables
#  dplyr::mutate(variable = factor(variable, levels = c("AgeYear", "sex", "grade"))) %>% #Here we are forcing R to #display the row within the variable "variable" in a specific order
#  dplyr::arrange(variable)

dattab2 <- data.frame(t(dattab))

#renaming the columns
#https://www.datanovia.com/en/lessons/rename-data-frame-columns-in-r/

names(dattab2)[1]<- "Cross_sectional_cohort"  
names(dattab2)[2]<- "Prospective_cohort"

#Trying to get rid of the colnames that snuck in the data
#mtx.all <- mtx.all %>% filter(!str_detect(1, "Gene_family")) #option 1 with str_detect, didn't work
#mtx.path.all <- mtx.path.all %>% filter(!str_detect(1, "Pathway"))  #option 1 with str_detect, didn't work
#dattab2.all <-dattab2 %>% filter(!str_detect(1, "Cohort"))

#mtx.path.all<-mtx.path.all%>%filter(!grepl("1CMET2-PWY: folate transformations III (E. coli)",X1)) 
dattab2.all <-dattab2 %>% filter(!grepl("CS",Cross_sectional_cohort))

##Beautify the table using the kableExtra package

dattab2.all %>%
  kable(digits=2) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F) %>% #We can use preset table settings within R to set our table 
  pack_rows("Age", start_row = 1, end_row = 2) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("eGFR, ml/min/1.73m2", start_row = 3, end_row = 4) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum
  pack_rows("Total bilirubin, mg/dL", start_row = 5, end_row = 6) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum  
  pack_rows("Albumin, g/dL", start_row = 7, end_row = 8) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum    
  pack_rows("MPA AUC0-12 hr, mg.h/L", start_row = 9, end_row = 10) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum    
  pack_rows("MPA AUC5-12 hr, mg.h/L", start_row = 11, end_row = 12) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("MPA EHR (AUC5-12 hr/AUC0-12 hr)", start_row = 13, end_row = 14) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum
  pack_rows("MPAG AUC0-12 hr, mg.h/L", start_row = 15, end_row = 16) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("MPAG AUC5-12 hr, mg.h/L", start_row = 17, end_row = 18) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("AcMPAG AUC0-12 hr, mg.h/L", start_row = 19, end_row = 20) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("AcMPAG AUC5-12 hr, mg.h/L", start_row = 21, end_row = 22) #%>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum    


##Build the summaries from dplyr (chaining command) for categorical variables, 1 categorical variable at the time

### Gender
  
dattab3 <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, age_PK, gender, predom_race, eGFR_Epi_RF_Cr2021_1_73m2, total_bilirubin_mg_dL, albumin_g_dL, MPA_AUC_0_12, MPA_AUC_5_12, MPA_AUC5_12_0_12, MPAG_AUC_0_12, MPAG_AUC_5_12, AcMPAG_AUC_0_12, AcMPAG_AUC_5_12) %>%
  labelled::set_variable_labels(age_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_Epi_RF_Cr2021_1_73m2 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", MPA_AUC_5_12 = "MPA AUC5-12 hr, mg.h/L",
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)",
                                MPAG_AUC_0_12 = "MPAG AUC0-12 hr, mg.h/L", MPAG_AUC_5_12 = "MPAG AUC5-12 hr, mg.h/L",
                                AcMPAG_AUC_0_12 = "Acyl MPAG AUC0-12 hr, mg.h/L", AcMPAG_AUC_5_12 = "Acyl MPAG AUC5-12 hr, mg.h/L")  %>% 
  dplyr::select(cohort, gender) %>% #Select only the categorical variables here
  tidyr::pivot_longer( cols = c("gender"),
               names_to = "variable",
               values_to = "value") %>% #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 
  

dattab4 <- data.frame(t(dattab3)) #Tricking the dataframe into the same format used for the continuous summaries 

dattab4 <- dattab4 %>%
  dplyr::select(X2, X4) #Trimming down to a single category per variable so the number of columns matches that of the continuous variable summary
  
#renaming the columns
#https://www.datanovia.com/en/lessons/rename-data-frame-columns-in-r/

names(dattab4)[1]<- "Cross_sectional_cohort"  
names(dattab4)[2]<- "Prospective_cohort"


#Trying to get rid of the colnames that snuck in the data
#mtx.all <- mtx.all %>% filter(!str_detect(1, "Gene_family")) #option 1 with str_detect, didn't work
#mtx.path.all <- mtx.path.all %>% filter(!str_detect(1, "Pathway"))  #option 1 with str_detect, didn't work
#dattab2.all <-dattab2 %>% filter(!str_detect(1, "Cohort"))

#mtx.path.all<-mtx.path.all%>%filter(!grepl("1CMET2-PWY: folate transformations III (E. coli)",X1)) 
dattab4.all <-dattab4 %>% filter(!grepl("CS",Cross_sectional_cohort))
dattab4.all <-dattab4.all %>% filter(!grepl("gender",Cross_sectional_cohort))
dattab4.all <-dattab4.all %>% filter(!grepl("Male",Cross_sectional_cohort))

#race

dattab5 <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, age_PK, gender, predom_race, eGFR_Epi_RF_Cr2021_1_73m2, total_bilirubin_mg_dL, albumin_g_dL, MPA_AUC_0_12, MPA_AUC_5_12, MPA_AUC5_12_0_12, MPAG_AUC_0_12, MPAG_AUC_5_12, AcMPAG_AUC_0_12, AcMPAG_AUC_5_12) %>%
  labelled::set_variable_labels(age_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_Epi_RF_Cr2021_1_73m2 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", MPA_AUC_5_12 = "MPA AUC5-12 hr, mg.h/L",
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)",
                                MPAG_AUC_0_12 = "MPAG AUC0-12 hr, mg.h/L", MPAG_AUC_5_12 = "MPAG AUC5-12 hr, mg.h/L",
                                AcMPAG_AUC_0_12 = "Acyl MPAG AUC0-12 hr, mg.h/L", AcMPAG_AUC_5_12 = "Acyl MPAG AUC5-12 hr, mg.h/L")  %>% 
  dplyr::select(cohort, predom_race) %>% #Select only the categorical variables here
  tidyr::pivot_longer( cols = c("predom_race"),
               names_to = "variable",
               values_to = "value") %>% #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 
  

dattab6 <- data.frame(t(dattab5)) #Tricking the dataframe into the same format used for the continuous summaries 

dattab6 <- dattab6 %>%
  dplyr::select(X5, X9) #Trimming down to a single category per variable so the number of columns matches that of the continuous variable summary
  
#renaming the columns
#https://www.datanovia.com/en/lessons/rename-data-frame-columns-in-r/

names(dattab6)[1]<- "Cross_sectional_cohort"  
names(dattab6)[2]<- "Prospective_cohort"


#Trying to get rid of the colnames that snuck in the data
#mtx.all <- mtx.all %>% filter(!str_detect(1, "Gene_family")) #option 1 with str_detect, didn't work
#mtx.path.all <- mtx.path.all %>% filter(!str_detect(1, "Pathway"))  #option 1 with str_detect, didn't work
#dattab2.all <-dattab2 %>% filter(!str_detect(1, "Cohort"))

#mtx.path.all<-mtx.path.all%>%filter(!grepl("1CMET2-PWY: folate transformations III (E. coli)",X1)) 
dattab6.all <-dattab6 %>% filter(!grepl("CS",Cross_sectional_cohort))
dattab6.all <-dattab6.all %>% filter(!grepl("predom_race",Cross_sectional_cohort))
dattab6.all <-dattab6.all %>% filter(!grepl("White",Cross_sectional_cohort))

#Number in each cohort

dattab7 <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, age_PK, gender, predom_race, eGFR_Epi_RF_Cr2021_1_73m2, total_bilirubin_mg_dL, albumin_g_dL, MPA_AUC_0_12, MPA_AUC_5_12, MPA_AUC5_12_0_12, MPAG_AUC_0_12, MPAG_AUC_5_12, AcMPAG_AUC_0_12, AcMPAG_AUC_5_12) %>%
  labelled::set_variable_labels(age_PK = "Age at PK assessment", gender = "Gender", 
                                predom_race = "Ancestry", eGFR_Epi_RF_Cr2021_1_73m2 = "eGFR, ml/min/1.73m2",
                                total_bilirubin_mg_dL = "Total bilirubin, mg/dL", albumin_g_dL = "Albumin, g/dL",
                                MPA_AUC_0_12 = "MPA AUC0-12 hr, mg.h/L", MPA_AUC_5_12 = "MPA AUC5-12 hr, mg.h/L",
                                MPA_AUC5_12_0_12 = "MPA EHR (AUC5-12 hr/AUC0-12 hr)",
                                MPAG_AUC_0_12 = "MPAG AUC0-12 hr, mg.h/L", MPAG_AUC_5_12 = "MPAG AUC5-12 hr, mg.h/L",
                                AcMPAG_AUC_0_12 = "Acyl MPAG AUC0-12 hr, mg.h/L", AcMPAG_AUC_5_12 = "Acyl MPAG AUC5-12 hr, mg.h/L")  %>% 
  dplyr::select(cohort) %>% #Select only the categorical variables here
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
  dplyr::mutate(percent = (count/sum(count))*100) #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above

dattab8 <- data.frame(t(dattab7)) #Tricking the dataframe into the same format used for the continuous summaries 

dattab8 <- dattab8 %>%
  dplyr::select(X1, X2) #Trimming down to a single category per variable so the number of columns matches that of the continuous variable summary
  
#renaming the columns
#https://www.datanovia.com/en/lessons/rename-data-frame-columns-in-r/

names(dattab8)[1]<- "Cross_sectional_cohort"  
names(dattab8)[2]<- "Prospective_cohort"


#Trying to get rid of the colnames that snuck in the data
#mtx.all <- mtx.all %>% filter(!str_detect(1, "Gene_family")) #option 1 with str_detect, didn't work
#mtx.path.all <- mtx.path.all %>% filter(!str_detect(1, "Pathway"))  #option 1 with str_detect, didn't work
#dattab2.all <-dattab2 %>% filter(!str_detect(1, "Cohort"))

#mtx.path.all<-mtx.path.all%>%filter(!grepl("1CMET2-PWY: folate transformations III (E. coli)",X1)) 
dattab8.all <-dattab8 %>% filter(!grepl("CS",Cross_sectional_cohort))

##Append multiple datasets together before using KableExtra

#Pooling the two datasets (https://www.datacamp.com/doc/r/merging)

dattab9 <-rbind(dattab8.all, dattab6.all, dattab4.all, dattab2.all)

summary(dattab9)

#Including the indext data as row names to keep it into the kable output
#https://stackoverflow.com/questions/49184723/r-adding-row-index-values-as-rownames
#dattab7$variable <- rownames(dattab7)

#Moving the variable column first
#dattab7<-dattab7%>%relocate(variable, .before=1)

#Converting the specific colums os interest to numeric

dattab9$Cross_sectional_cohort<-as.numeric(dattab9$Cross_sectional_cohort)
dattab9$Prospective_cohort<-as.numeric(dattab9$Prospective_cohort)

dattab9.all <- dattab9 %>%
  kable(digits=2) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F) %>% #We can use preset table settings within R to set our table 
  pack_rows("Number of Participants", start_row = 1, end_row = 2) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum
  pack_rows("Ancestry (White)", start_row = 3, end_row = 4) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum  
  pack_rows("Gender (Male)", start_row = 5, end_row = 6) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum    
  pack_rows("Age", start_row = 7, end_row = 8) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("eGFR, ml/min/1.73m2", start_row = 9, end_row = 10) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum
  pack_rows("Total bilirubin, mg/dL", start_row = 11, end_row = 12) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum  
  pack_rows("Albumin, g/dL", start_row = 13, end_row = 14) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum    
  pack_rows("MPA AUC0-12 hr, mg.h/L", start_row = 15, end_row = 16) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum    
  pack_rows("MPA AUC5-12 hr, mg.h/L", start_row = 17, end_row = 18) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("MPA EHR (AUC5-12 hr/AUC0-12 hr)", start_row = 19, end_row = 20) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum
  pack_rows("MPAG AUC0-12 hr, mg.h/L", start_row = 21, end_row = 22) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("MPAG AUC5-12 hr, mg.h/L", start_row = 23, end_row = 24) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("AcMPAG AUC0-12 hr, mg.h/L", start_row = 25, end_row = 26) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("AcMPAG AUC5-12 hr, mg.h/L", start_row = 27, end_row = 28)#The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 

view(dattab9.all)

```

#Output for presentations / Manuscripts

###MISSION Study demographic information

```{r}

#Demegraphic table placeholder

dattab9.all <- dattab9 %>%
  kable(digits=2) %>% #Kable is used to set the table under an HTML format. We can also set the number of digits here, and manually rename each colum, using blank quotes to mask specific column names too.
  kable_classic(full_width = F) %>% #We can use preset table settings within R to set our table 
  pack_rows("Number of Participants", start_row = 1, end_row = 2) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum
  pack_rows("Ancestry (White)", start_row = 3, end_row = 4) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum  
  pack_rows("Gender (Male)", start_row = 5, end_row = 6) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum    
  pack_rows("Age", start_row = 7, end_row = 8) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("eGFR, ml/min/1.73m2", start_row = 9, end_row = 10) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum
  pack_rows("Total bilirubin, mg/dL", start_row = 11, end_row = 12) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum  
  pack_rows("Albumin, g/dL", start_row = 13, end_row = 14) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum    
  pack_rows("MPA AUC0-12 hr, mg.h/L", start_row = 15, end_row = 16) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum    
  pack_rows("MPA AUC5-12 hr, mg.h/L", start_row = 17, end_row = 18) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("MPA EHR (AUC5-12 hr/AUC0-12 hr)", start_row = 19, end_row = 20) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum
  pack_rows("MPAG AUC0-12 hr, mg.h/L", start_row = 21, end_row = 22) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("MPAG AUC5-12 hr, mg.h/L", start_row = 23, end_row = 24) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("AcMPAG AUC0-12 hr, mg.h/L", start_row = 25, end_row = 26) %>% #The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum   
  pack_rows("AcMPAG AUC5-12 hr, mg.h/L", start_row = 27, end_row = 28)#The pakrows option will let us delineate the strata within your table, and you can manually tell R where to start or end the stratum 

dattab9.all

#Writing the file to Excel
#https://stackoverflow.com/questions/19414605/export-data-from-r-to-excel
#for writing a data.frame or list of data.frames to an xlsx file
write.xlsx(dattab9, '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_wtc_demotable.xlsx')
```

#Description of the PK data 

### Continuous Demographic test statistics

```{r}

#Set working directory
setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

#Check the data type for all the variable we want to combine
names(pk.bgus.reads.data.4)
summary(pk.bgus.reads.data.4)

#Variables to check
#select(cohort, age_PK, gender, predom_race, eGFR_Epi_RF_Cr2021_1_73m2, total_bilirubin_mg_dL, albumin_g_dL, MPA_AUC_0_12, MPA_AUC_5_12, MPA_AUC5_12_0_12, MPAG_AUC_0_12, MPAG_AUC_5_12, AcMPAG_AUC_0_12, AcMPAG_AUC_5_12)

#Continuous variables

#https://www.rdocumentation.org/packages/stats/versions/3.6.2/topics/t.test

## Formula interface

#age
##Build the summaries from dplyr (chaining command) for continuous variables

agetab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, age_PK)  %>% 
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_age = mean(age_PK),
                   sd_age = sd(age_PK))

agetab

t.test(age_PK ~ cohort, data = pk.bgus.reads.data.4)

#BMI

pk.bgus.reads.data.4$bmi <- pk.bgus.reads.data.4$weight_at_pk_Kg/((pk.bgus.reads.data.4$height_at_pk_cm/100)*(pk.bgus.reads.data.4$height_at_pk_cm/100))

summary(pk.bgus.reads.data.4$bmi) 

bmitab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, bmi)  %>% 
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_bmi = mean(bmi),
                   sd_bmi = sd(bmi))

bmitab

t.test(bmi ~ cohort, data = pk.bgus.reads.data.4)

#EGFR

gfrtab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, eGFR_Epi_RF_Cr2021_1_73m2)  %>% 
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_gfr = mean(eGFR_Epi_RF_Cr2021_1_73m2),
                   sd_gfr = sd(eGFR_Epi_RF_Cr2021_1_73m2))

gfrtab

t.test(eGFR_Epi_RF_Cr2021_1_73m2 ~ cohort, data = pk.bgus.reads.data.4)

#Bilirubin

bilitab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, total_bilirubin_mg_dL)  %>% 
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_bili = mean(total_bilirubin_mg_dL),
                   sd_bili = sd(total_bilirubin_mg_dL))

bilitab

t.test(total_bilirubin_mg_dL ~ cohort, data = pk.bgus.reads.data.4)

#Albumin

albtab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, albumin_g_dL)  %>% 
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_bili = mean(albumin_g_dL),
                   sd_bili = sd(albumin_g_dL))

albtab

t.test(albumin_g_dL ~ cohort, data = pk.bgus.reads.data.4)

#MPA AUC 0-12

MPA_AUC_0_12_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, MPA_AUC_0_12)  %>% 
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_MPA_0_12 = mean(MPA_AUC_0_12),
                   sd_MPA_0_12 = sd(MPA_AUC_0_12))


MPA_AUC_0_12_tab

t.test(MPA_AUC_0_12 ~ cohort, data = pk.bgus.reads.data.4)

#MPA AUC 5-12

MPA_AUC_5_12_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, MPA_AUC_5_12)  %>% 
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_MPA_5_12 = mean(MPA_AUC_5_12),
                   sd_MPA_5_12 = sd(MPA_AUC_5_12))

MPA_AUC_5_12_tab

t.test(MPA_AUC_5_12 ~ cohort, data = pk.bgus.reads.data.4)

#MPA EHR

MPA_AUC5_12_0_12_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, MPA_AUC5_12_0_12)  %>% 
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_EHR = mean(MPA_AUC5_12_0_12),
                   sd_EHR = sd(MPA_AUC5_12_0_12))

MPA_AUC5_12_0_12_tab

t.test(MPA_AUC5_12_0_12 ~ cohort, data = pk.bgus.reads.data.4)

#Time since transplant

tx_to_pk_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, tx_to_pk_days)  %>% 
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_tx_to_pk = mean(tx_to_pk_days),
                   sd_tx_to_pk = sd(tx_to_pk_days))

tx_to_pk_tab

t.test(tx_to_pk_days ~ cohort, data = pk.bgus.reads.data.4)

#Standardized creatinine clearance (per pam on '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/E_02_Microbiome signature and MOSIO_07102024.pptx')

summary(pk.bgus.reads.data.4$final_St_CrCl_ml_min_1_73)
class(pk.bgus.reads.data.4$final_St_CrCl_ml_min_1_73)

#removing missing crcl values
#https://tidyr.tidyverse.org/reference/drop_na.html

crcl_tab <-
pk.bgus.reads.data.4 %>%
  tidyr::drop_na(final_St_CrCl_ml_min_1_73)

crcl_tab_2 <-
crcl_tab %>%
  dplyr::select(cohort, final_St_CrCl_ml_min_1_73)  %>% 
  dplyr::group_by(cohort) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_crcl = mean(final_St_CrCl_ml_min_1_73),
                   sd_crcl = sd(final_St_CrCl_ml_min_1_73))

crcl_tab_2

t.test(final_St_CrCl_ml_min_1_73 ~ cohort, data = pk.bgus.reads.data.4)

```

###Categorical Demographic test statistics

```{r}

#DPLYR tutorial

library(kableExtra)

#setworking directory
#setwd("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/")

#read in the data

#dat <- read.csv("/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/R_23_Data_for_table.csv")
dat <- read.csv("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_R_23_Data_for_table.csv")

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

#Set working directory
setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

#adding BMI to the dataset
pk.bgus.reads.data.4$bmi <- pk.bgus.reads.data.4$weight_at_pk_Kg/((pk.bgus.reads.data.4$height_at_pk_cm/100)*(pk.bgus.reads.data.4$height_at_pk_cm/100))

summary(pk.bgus.reads.data.4$bmi) 

#Check the data type for all the variable we want to combine
names(pk.bgus.reads.data.4)
summary(pk.bgus.reads.data.4)

#Adding the pcause new variable for demographics
# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")
#(need a left join to keep all observations from misison.data.comb)

pcausetab <-
mission.data.comb %>%
  dplyr::select(record_id, pcausenew)

pk.bgus.reads.data.5 <-merge(pk.bgus.reads.data.4,pcausetab,by="record_id",all.x=TRUE)

#Categorical variables

#Checking frequencies
#https://www.geeksforgeeks.org/frequency-table-in-r/
#https://cran.r-project.org/web/packages/vcdExtra/vignettes/creating.html

#freq <- table(data.notes2$PID, data.notes2$Slide)
#freq
#dim(freq) #28 unique PIDs across 7 slides, 363 RoIs

#https://www.rdocumentation.org/packages/stats/versions/3.6.2/topics/chisq.test

#race

freq <- table(pk.bgus.reads.data.4$predom_race,pk.bgus.reads.data.4$cohort)
freq

race_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, predom_race) %>%
  labelled::set_variable_labels(predom_race = "Ancestry")  %>% 
  dplyr::select(cohort, predom_race) 

race_tab_2 <- race_tab %>%
    tidyr::pivot_longer( cols = c("predom_race"),
               names_to = "variable",
               values_to = "value") #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  
race_tab_3 <- race_tab_2 %>%  
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 

race_tab_4 <- race_tab_3 %>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered. The names from will automatically rename the variables
  dplyr::select(variable, value, count_CS, percent_CS, count_PROS, percent_PROS) #You can use the select command in dplyr to force a certain order for your chosen variables    

race_tab_4

chisq.test(pk.bgus.reads.data.4$predom_race,pk.bgus.reads.data.4$cohort)

#Gender

gender_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, gender) %>%
  labelled::set_variable_labels(gender = "Sex")  %>% 
  dplyr::select(cohort, gender) 

gender_tab_2 <- gender_tab %>%
    tidyr::pivot_longer( cols = c("gender"),
               names_to = "variable",
               values_to = "value") #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  
gender_tab_3 <- gender_tab_2 %>%  
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 

gender_tab_4 <- gender_tab_3 %>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered. The names from will automatically rename the variables
  dplyr::select(variable, value, count_CS, percent_CS, count_PROS, percent_PROS) #You can use the select command in dplyr to force a certain order for your chosen variables    

gender_tab_4

chisq.test(pk.bgus.reads.data.4$gender,pk.bgus.reads.data.4$cohort)

#ethnicity

ethnicity_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, ethnicity) %>%
  labelled::set_variable_labels(ethnicity = "ethnicity")  %>% 
  dplyr::select(cohort, ethnicity) 

ethnicity_tab_2 <- ethnicity_tab %>%
    tidyr::pivot_longer( cols = c("ethnicity"),
               names_to = "variable",
               values_to = "value") #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  
ethnicity_tab_3 <- ethnicity_tab_2 %>%  
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 

ethnicity_tab_4 <- ethnicity_tab_3 %>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered. The names from will automatically rename the variables
  dplyr::select(variable, value, count_CS, percent_CS, count_PROS, percent_PROS) #You can use the select command in dplyr to force a certain order for your chosen variables    

ethnicity_tab_4

chisq.test(pk.bgus.reads.data.4$ethnicity,pk.bgus.reads.data.4$cohort)

#Donor status

donor_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, donar_status) %>%
  labelled::set_variable_labels(donar_status = "donor status")  %>% 
  dplyr::select(cohort, donar_status) 

donor_tab_2 <- donor_tab %>%
    tidyr::pivot_longer( cols = c("donar_status"),
               names_to = "variable",
               values_to = "value") #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  
donor_tab_3 <- donor_tab_2 %>%  
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 

donor_tab_4 <- donor_tab_3 %>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered. The names from will automatically rename the variables
  dplyr::select(variable, value, count_CS, percent_CS, count_PROS, percent_PROS) #You can use the select command in dplyr to force a certain order for your chosen variables    

donor_tab_4

chisq.test(pk.bgus.reads.data.4$donar_status,pk.bgus.reads.data.4$cohort)

#Primary cause of kidney disease

pcause_tab <-
pk.bgus.reads.data.5 %>%
  dplyr::select(cohort, pcausenew) %>%
  labelled::set_variable_labels(pcausenew = "Primary cause of kidney disease")  %>% 
  dplyr::select(cohort, pcausenew) 

pcause_tab_2 <- pcause_tab %>%
    tidyr::pivot_longer( cols = c("pcausenew"),
               names_to = "variable",
               values_to = "value") #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  
pcause_tab_3 <- pcause_tab_2 %>%  
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 

pcause_tab_4 <- pcause_tab_3 %>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered. The names from will automatically rename the variables
  dplyr::select(variable, value, count_CS, percent_CS, count_PROS, percent_PROS) #You can use the select command in dplyr to force a certain order for your chosen variables    

pcause_tab_4

chisq.test(pk.bgus.reads.data.5$pcausenew,pk.bgus.reads.data.5$cohort)

#PPI use

ppi_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, PPI) %>%
  labelled::set_variable_labels(PPI = "PPI use")  %>% 
  dplyr::select(cohort, PPI) 

ppi_tab_2 <- ppi_tab %>%
    tidyr::pivot_longer( cols = c("PPI"),
               names_to = "variable",
               values_to = "value") #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  
ppi_tab_3 <- ppi_tab_2 %>%  
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 

ppi_tab_4 <- ppi_tab_3 %>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered. The names from will automatically rename the variables
  dplyr::select(variable, value, count_CS, percent_CS, count_PROS, percent_PROS) #You can use the select command in dplyr to force a certain order for your chosen variables    

ppi_tab_4

chisq.test(pk.bgus.reads.data.4$PPI,pk.bgus.reads.data.4$cohort)

#Antifungal use

antifungal_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, antifungal_PK) %>%
  labelled::set_variable_labels(antifungal_PK = "PPI use")  %>% 
  dplyr::select(cohort, antifungal_PK) 

antifungal_tab_2 <- antifungal_tab %>%
    tidyr::pivot_longer( cols = c("antifungal_PK"),
               names_to = "variable",
               values_to = "value") #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  
antifungal_tab_3 <- antifungal_tab_2 %>%  
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 

antifungal_tab_4 <- antifungal_tab_3 %>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered. The names from will automatically rename the variables
  dplyr::select(variable, value, count_CS, percent_CS, count_PROS, percent_PROS) #You can use the select command in dplyr to force a certain order for your chosen variables    

antifungal_tab_4

chisq.test(pk.bgus.reads.data.4$antifungal_PK,pk.bgus.reads.data.4$cohort)

#Antiviral use

antiviral_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, antiviral_PK) %>%
  labelled::set_variable_labels(antiviral_PK = "Antiviral")  %>% 
  dplyr::select(cohort, antiviral_PK) 

antiviral_tab_2 <- antiviral_tab %>%
    tidyr::pivot_longer( cols = c("antiviral_PK"),
               names_to = "variable",
               values_to = "value") #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  
antiviral_tab_3 <- antiviral_tab_2 %>%  
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 

antiviral_tab_4 <- antiviral_tab_3 %>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered. The names from will automatically rename the variables
  dplyr::select(variable, value, count_CS, percent_CS, count_PROS, percent_PROS) #You can use the select command in dplyr to force a certain order for your chosen variables    

antiviral_tab_4

chisq.test(pk.bgus.reads.data.4$antiviral_PK,pk.bgus.reads.data.4$cohort)

#Antibiotic use

antibiotic_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, antibiotics_pk) %>%
  labelled::set_variable_labels(antibiotics_pk = "Antibiotics")  %>% 
  dplyr::select(cohort, antibiotics_pk) 

antibiotic_tab_2 <- antibiotic_tab %>%
    tidyr::pivot_longer( cols = c("antibiotics_pk"),
               names_to = "variable",
               values_to = "value") #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  
antibiotic_tab_3 <- antibiotic_tab_2 %>%  
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 

antibiotic_tab_4 <- antibiotic_tab_3 %>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered. The names from will automatically rename the variables
  dplyr::select(variable, value, count_CS, percent_CS, count_PROS, percent_PROS) #You can use the select command in dplyr to force a certain order for your chosen variables    

antibiotic_tab_4

chisq.test(pk.bgus.reads.data.4$antibiotics_pk,pk.bgus.reads.data.4$cohort)

#Steroid use

steroid_tab <-
pk.bgus.reads.data.4 %>%
  dplyr::select(cohort, steroid_at_PK) %>%
  labelled::set_variable_labels(steroid_at_PK = "Steroids")  %>% 
  dplyr::select(cohort, steroid_at_PK) 

steroid_tab_2 <- steroid_tab %>%
    tidyr::pivot_longer( cols = c("steroid_at_PK"),
               names_to = "variable",
               values_to = "value") #This command is from the tidyR package, which is part of the reshaping data in R lesson (wide to long transformation). You can use the semicolon abbreviation lets us select every column between the tow without needing to type them out. Otherwise you need to use the cols = c() nomenclature to concatenate multiple columns (#https://dcl-wrangle.stanford.edu/pivot-advanced.html)
  
steroid_tab_3 <- steroid_tab_2 %>%  
  dplyr::group_by(cohort, variable, value) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(count=n()) %>%  
#To calculate the percent you have to adjust the grouping variables
  dplyr::group_by(cohort, variable) %>% #use mutate to add a new column which will have the desired calculated metric (percent based on value), and R knows the count value because we generated it above
  dplyr::mutate(percent = (count/sum(count))*100) 

steroid_tab_4 <- steroid_tab_3 %>%
#Reshape the summarize output so that the case and control columns are side by side  
  pivot_wider (names_from = cohort,
               values_from = c(count, percent)) %>% #You have to use the concatenate option here so that the values appear side by side, not staggered. The names from will automatically rename the variables
  dplyr::select(variable, value, count_CS, percent_CS, count_PROS, percent_PROS) #You can use the select command in dplyr to force a certain order for your chosen variables    

steroid_tab_4

chisq.test(pk.bgus.reads.data.4$steroid_at_PK,pk.bgus.reads.data.4$cohort)

#Saving the dataset after adding the additional variables for demographics

write.xlsx(pk.bgus.reads.data.5, '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data_2.xlsx')
```


###MPA AUC from 0 to 12 hours

```{r}

#Figure for MPA AUC 0 to 12 hours

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_0_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPA AUC between 0 and 12 hours",
        title = "MPA Area under the curve and MPA EHR peaks by study cohorts")

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_grey(name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPA AUC between 0 and 12 hours",
        title = "MPA Area under the curve and MPA EHR peaks by study cohorts")

#Association between cohort and MPA AUC

summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA AUC and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))
```

### MPA AUC from 5 to 12 hours

```{r}

#Figure for MPA AUC 5 to 12 hours

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_5_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_5_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPA AUC between 5 and 12 hours",
        title = "MPA Area under the curve and MPA EHR peaks by study cohorts")

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC_5_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_grey(name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPA AUC between 5 and 12 hours",
        title = "MPA Area under the curve (5-12 hours) and MPA EHR peaks by study cohorts")

#Association between cohort and MPA  5 to 12 AUC

summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA 5 to 12 AUC and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

```

### MPA EHR

```{r}

#Figure for MPA EHR

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC5_12_0_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC5_12_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPA EHR between 5 and 12 hours",
        title = "MPA EHR and MPA EHR peaks by study cohorts")

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPA_AUC5_12_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_grey(name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPA EHR between 5 and 12 hours",
        title = "MPA EHR and MPA EHR peaks by study cohorts")

#Association between cohort and MPA EHR

summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA EHR and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between cohort and MPA EHR adjusted for demo covariates (association goes away)

summary(lm(pk.bgus.reads.data.5$MPA_AUC5_12_0_12 ~ cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5))
summary(lm(pk.bgus.reads.data.5$MPA_AUC5_12_0_12 ~ cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

```

### MPA PK parameter adjustments for table 2

```{r}

#Generating the summary statistics per MPA EHR category
#https://pubs.wsb.wisc.edu/academics/analytics-using-r-2019/convert-numerical-data-to-categorical.html

#To define the new categorical variable we use the following code:

pk.bgus.reads.data.5 <- within(pk.bgus.reads.data.5, {   
  MPA_EHR_Peaks_count_cat <- NA # need to initialize variable
  MPA_EHR_Peaks_count_cat[MPA_EHR_Peaks_count < 1] <- "Low"
  MPA_EHR_Peaks_count_cat[MPA_EHR_Peaks_count >= 1 & MPA_EHR_Peaks_count < 2] <- "Middle"
  MPA_EHR_Peaks_count_cat[MPA_EHR_Peaks_count >= 2] <- "High"
   } )

#The next line defines the variable as a factor variable and sets the ordering of the buckets with the levels() parameter. The category noted first is called the Reference category. This ordering is important for some analytic methods that you will complete.

pk.bgus.reads.data.5$MPA_EHR_Peaks_count_cat <- factor(pk.bgus.reads.data.5$MPA_EHR_Peaks_count_cat, levels = c("Low", "Middle", "High"))
  
summary(pk.bgus.reads.data.5$MPA_EHR_Peaks_count_cat)

##Build the summaries from dplyr (chaining command) for continuous variables

AUC_0_12_tab <-
pk.bgus.reads.data.5 %>%
  dplyr::select(MPA_EHR_Peaks_count_cat, MPA_AUC_0_12)  %>% 
  dplyr::group_by(MPA_EHR_Peaks_count_cat) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_AUC_0_12 = mean(MPA_AUC_0_12),
                   sd_AUC_0_12 = sd(MPA_AUC_0_12))

AUC_0_12_tab

#Association between cohort and MPA AUC

summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA AUC and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA AUC and MPA peaks adjusted for demo covariates

summary(lm(pk.bgus.reads.data.5$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5))
summary(lm(pk.bgus.reads.data.5$MPA_AUC_0_12 ~ MPA_EHR_Peaks_count + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##Build the summaries from dplyr (chaining command) for continuous variables

AUC_5_12_tab <-
pk.bgus.reads.data.5 %>%
  dplyr::select(MPA_EHR_Peaks_count_cat, MPA_AUC_5_12)  %>% 
  dplyr::group_by(MPA_EHR_Peaks_count_cat) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_AUC_5_12 = mean(MPA_AUC_5_12),
                   sd_AUC_5_12 = sd(MPA_AUC_5_12))

AUC_5_12_tab

#Association between cohort and MPA  5 to 12 AUC

summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA 5 to 12 AUC and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA 5 to 12 AUC and MPA peaks adjusted for demo covariates

summary(lm(pk.bgus.reads.data.5$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5))
summary(lm(pk.bgus.reads.data.5$MPA_AUC_5_12 ~ MPA_EHR_Peaks_count + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##Build the summaries from dplyr (chaining command) for continuous variables

EHR_tab <-
pk.bgus.reads.data.5 %>%
  dplyr::select(MPA_EHR_Peaks_count_cat, MPA_AUC5_12_0_12)  %>% 
  dplyr::group_by(MPA_EHR_Peaks_count_cat) %>% #This step lets us count the number of times that each combination of variables appears
  dplyr::summarise(mean_EHR = mean(MPA_AUC5_12_0_12),
                   sd_EHR = sd(MPA_AUC5_12_0_12))

EHR_tab

#Association between cohort and MPA EHR

summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA EHR and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPA peaks and MPA EHR adjusted for demo covariates

summary(lm(pk.bgus.reads.data.5$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5))
summary(lm(pk.bgus.reads.data.5$MPA_AUC5_12_0_12 ~ MPA_EHR_Peaks_count + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

```

### MPAG AUC from 0 to 12 hours

```{r}

#Figure for MPAG AUC 0 to 12 hrs

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPAG_AUC_0_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPAG_AUC_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPAG AUC between 0 and 12 hours",
        title = "MPAG AUC and MPA EHR peaks by study cohorts")

#Association between cohort and MPAG AUC 0 to 12 hours

summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPAG AUC 0 to 12 hours and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))


```

### MPAG AUC from 5 to 12 hours

```{r}

#Figure for MPAG AUC 5 to 12 hrs

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPAG_AUC_5_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=MPAG_AUC_5_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "MPAG AUC between 0 and 12 hours",
        title = "MPAG AUC and MPA EHR peaks by study cohorts")

#Association between cohort and MPAG AUC 5 to 12 hours

summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between MPAG AUC 5 to 12 hours and MPA peaks

summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$MPAG_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))
```

### Acyl MPAG from 0 to 12 hours

```{r}

#Figure for Acyl MPAG AUC 0 to 12 hrs

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=AcMPAG_AUC_0_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=AcMPAG_AUC_0_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "Acyl MPAG AUC between 0 and 12 hours",
        title = "Acyl MPAG AUC and MPA EHR peaks by study cohorts")

#Association between cohort and Acyl MPAG AUC 0 to 12 hours

summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between Acyl MPAG AUC 0 to 12 hours and MPA peaks

summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_0_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))


```

### Acyl MPAG from 5 to 12 hours

```{r}

#Figure for Acyl MPAG AUC 5 to 12 hrs

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=AcMPAG_AUC_5_12)) + 
  geom_line(stat="summary", fun="mean", 
            aes(group=cohort, color=cohort))

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=AcMPAG_AUC_5_12)) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "Acyl MPAG AUC between 5 and 12 hours",
        title = "Acyl MPAG AUC and MPA EHR peaks by study cohorts")

#Association between cohort and Acyl MPAG AUC 5 to 12 hours

summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12  ~ cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12 ~ cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Association between Acyl MPAG AUC 5 to 12 hours and MPA peaks

summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interactions
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3))
summary(lm(pk.bgus.reads.data.3$AcMPAG_AUC_5_12 ~ MPA_EHR_Peaks_count*cohort, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

```

### Descriptive figures for BGUS reads per study arm

```{r}

#Figure for Acyl MPAG AUC 5 to 12 hrs

pk.bgus.reads.data.4 %>%
  ggplot2::ggplot(aes(x= MPA_EHR_Peaks_count, y=BGUS_reads_adjusted )) + 
  geom_bar(stat="summary", fun="mean", aes(fill=cohort),
           position = "dodge") + 
            scale_fill_brewer(palette = "Pastel1", name = "Study Cohort") + 
  labs (x = "Number of detected MPA peaks in the EHR window", 
        y = "Relative abundance of BGUS transcripts",
        title = "BGUS transcripts and MPA EHR peaks by study cohorts")

#convert the EHR peaks variable to a factor variable
#https://bookdown.org/ccolonescu/RPoE4/indvars.html

summary(pk.bgus.reads.data.4$MPA_EHR_Peaks_count)

pk.bgus.reads.data.4$MPA_EHR_Peaks_factor <-as.factor(pk.bgus.reads.data.4$MPA_EHR_Peaks_count)

#Checking anova on BGUS reads relative abundance
#https://statsandr.com/blog/anova-in-r/

res_aov <- aov(BGUS_reads_adjusted ~ pk.bgus.reads.data.4$MPA_EHR_Peaks_factor ,
  data = pk.bgus.reads.data.4
)

summary(res_aov)

#Breakdown of observations per group
freq <- table(pk.bgus.reads.data.4$cohort, pk.bgus.reads.data.4$MPA_EHR_Peaks_factor)
freq
```

#MTX Path analysis update

##Full MISSION cohort

###Data prep

```{r}

#Read in the data
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data_2.xlsx')

pk.bgus.reads.data.4$MPA_EHR_Peaks_factor <-as.factor(pk.bgus.reads.data.4$MPA_EHR_Peaks_count)

#Association between BGUS reads and BGUS pathways in full cohort 

summary(lm(pk.bgus.reads.data.4$BGUS_reads_adjusted ~ GLUCUROCAT_PWY, data=pk.bgus.reads.data.4))

pk.bgus.reads.data.4$GLUCUROCAT_PWY_2 <-as.numeric(pk.bgus.reads.data.4$GLUCUROCAT_PWY)
pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 <- pk.bgus.reads.data.4$GLUCUROCAT_PWY_2/pk.bgus.reads.data.4$Total_reads

summary(lm(pk.bgus.reads.data.4$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=pk.bgus.reads.data.4))
summary(lm(pk.bgus.reads.data.4$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=pk.bgus.reads.data.4, subset = MPA_EHR_Peaks_count %in% c("0","2")))

cor.test(pk.bgus.reads.data.4$BGUS_reads_adjusted,pk.bgus.reads.data.4$GLUCUROCAT_PWY_3)
#cor.test(pk.bgus.reads.data.4$BGUS_reads_adjusted,pk.bgus.reads.data.4$GLUCUROCAT_PWY_3, subset = pk.bgus.reads.data.4$MPA_EHR_Peaks_count %in% c("0","2"))

pk.bgus.reads.data.4$log_GLUCUROCAT_PWY <- log(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 + 0.1)
summary(lm(pk.bgus.reads.data.4$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=pk.bgus.reads.data.4))
summary(lm(pk.bgus.reads.data.4$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=pk.bgus.reads.data.4, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Breakdown of observations per group
#freq <- table(pk.bgus.reads.data.4$cohort, pk.bgus.reads.data.4$MPA_EHR_Peaks_factor)
#freq

#convert the EHR peaks variable to a factor variable
#https://bookdown.org/ccolonescu/RPoE4/indvars.html

summary(pk.bgus.reads.data.4$MPA_EHR_Peaks_count)

pk.bgus.reads.data.4$MPA_EHR_Peaks_factor <-as.factor(pk.bgus.reads.data.4$MPA_EHR_Peaks_count)

```

###BGUS pathway accross PK groups defined by number of EHR Peaks

```{r}

#Full cohort, association with number of MPA EHR peaks observed

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.4))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.4))

#Checking anova on BGUS pathway
#https://statsandr.com/blog/anova-in-r/

res_aov <- aov(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_factor,
  data = pk.bgus.reads.data.4)

summary(res_aov)

res_aov <- aov(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_factor,
  data = pk.bgus.reads.data.4)

summary(res_aov)

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))


```

###PK parameters

```{r}


#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=pk.bgus.reads.data.4))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=pk.bgus.reads.data.4))

#Testing out 95% CI coding
#https://www.r-bloggers.com/2023/09/creating-confidence-intervals-for-a-linear-model-in-r-using-base-r-and-the-iris-dataset/

#predict(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=pk.bgus.reads.data.4),
#        interval = "confidence",
#        level = 0.95)

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4), 
           subset = MPA_EHR_Peaks_count %in% c("0","2"))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4), 
           subset = MPA_EHR_Peaks_count %in% c("0","2"))

##MPA AUC 5 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=pk.bgus.reads.data.4))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=pk.bgus.reads.data.4))

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.4))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.4))

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12+ cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12+ cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL  + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 0 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.4))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.4))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 5 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.4))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.4))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Acyl MPAG AUC 0 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.4))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.4))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Acyl MPAG AUC 5 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.4))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.4))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

#subsetting the dataset 
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
bgus.data.ps<-pk.bgus.reads.data.4%>%
  filter(cohort %in% c("PROS"))

bgus.data.cs<-pk.bgus.reads.data.4%>%
  filter(cohort %in% c("CS"))

```

###PK parameters adjusted for demographic covariates

```{r}


#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=pk.bgus.reads.data.4))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=pk.bgus.reads.data.4))

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4), 
           subset = MPA_EHR_Peaks_count %in% c("0","2"))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4), 
           subset = MPA_EHR_Peaks_count %in% c("0","2"))

##MPA AUC 5 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=pk.bgus.reads.data.4))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=pk.bgus.reads.data.4))

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL  + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.4))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.4))

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12+ cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.4$GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(pk.bgus.reads.data.4$log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12+ cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.4, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
```

###Prospective Cohort

```{r}

#Stratified analyses, PS

#Full cohort, association with number of MPA EHR peaks observed

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_count, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_count, data=bgus.data.ps))

#Checking anova on BGUS pathway
#https://statsandr.com/blog/anova-in-r/

res_aov <- aov(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_factor,
  data = bgus.data.ps)

summary(res_aov)

res_aov <- aov(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_factor,
  data = bgus.data.ps)

summary(res_aov)

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_count, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_count, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=bgus.data.ps))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA AUC 5 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=bgus.data.ps))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=bgus.data.ps))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 0 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_0_12, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_0_12, data=bgus.data.ps))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 5 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_5_12, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_5_12, data=bgus.data.ps))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_5_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_5_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Acyl MPAG AUC 0 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_0_12, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_0_12, data=bgus.data.ps))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Acyl MPAG AUC 5 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_5_12, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_5_12, data=bgus.data.ps))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_5_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_5_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

```

###Prospective Cohort adjusted for demographic covariates

```{r}

#Stratified analyses, PS

#Full cohort, association with number of MPA EHR peaks observed

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_count, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_count, data=bgus.data.ps))

#Checking anova on BGUS pathway
#https://statsandr.com/blog/anova-in-r/

res_aov <- aov(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_factor,
  data = bgus.data.ps)

summary(res_aov)

res_aov <- aov(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_factor,
  data = bgus.data.ps)

summary(res_aov)

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_count, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_count, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=bgus.data.ps))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA AUC 5 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=bgus.data.ps))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=bgus.data.ps))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
```

###Cross-Sectional Cohort

```{r}
#Stratified analyses, CS

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_count, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_count, data=bgus.data.cs))

#Checking anova on BGUS pathway
#https://statsandr.com/blog/anova-in-r/

res_aov <- aov(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_factor,
  data = bgus.data.cs)

summary(res_aov)

res_aov <- aov(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_factor,
  data = bgus.data.cs)

summary(res_aov)

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_count, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_count, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=bgus.data.cs))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA AUC 5 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=bgus.data.cs))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=bgus.data.cs))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 0 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_0_12, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_0_12, data=bgus.data.cs))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_0_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_0_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

#MPAG AUC 5 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_5_12, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_5_12, data=bgus.data.cs))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPAG_AUC_5_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPAG_AUC_5_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Acyl MPAG AUC 0 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_0_12, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_0_12, data=bgus.data.cs))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_0_12, data=bgus.data.ps, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Acyl MPAG AUC 5 to 12 hours

summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_5_12, data=bgus.data.ps))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_5_12, data=bgus.data.ps))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ AcMPAG_AUC_5_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ AcMPAG_AUC_5_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
```

###Cross-sectional Cohort adjusted for demographic covariates

```{r}

#Stratified analyses, PS

#Full cohort, association with number of MPA EHR peaks observed

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_count, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_count, data=bgus.data.cs))

#Checking anova on BGUS pathway
#https://statsandr.com/blog/anova-in-r/

res_aov <- aov(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_factor,
  data = bgus.data.cs)

summary(res_aov)

res_aov <- aov(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_factor,
  data = bgus.data.cs)

summary(res_aov)

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_EHR_Peaks_count, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_EHR_Peaks_count, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=bgus.data.cs))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA AUC 5 to 12

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=bgus.data.cs))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=bgus.data.cs))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs))

#Looking only at top and bottom groups
summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(GLUCUROCAT_PWY_3 ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
summary(lm(log_GLUCUROCAT_PWY ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs, 
           subset = MPA_EHR_Peaks_count %in% c("0","2")))
```

#Random forest tutorial

###Loading the packages (diamonds is included in ggplot2)
```{r}

library(ggplot2)
library(dplyr)
library(tidyverse)
#Need the package with the createDataPartition function
#https://www.rdocumentation.org/packages/caret/versions/7.0-1/topics/createDataPartition
#install.packages("caret")
library(caret)
#install.packages("randomForest")
library(randomForest)
#install.packages("Metrics")
library(Metrics)


#https://www.statisticalaid.com/random-forest-in-r/

diamond<-diamonds
head(diamond)  

#convert variables to numeric
diamond$cut <- as.integer(diamond$cut)
diamond$color <- as.integer(diamond$color)
diamond$clarity <- as.integer(diamond$clarity)

head(diamond)  

#To use random forest, all you need is to define the features (X vector) and the target you are trying to predict (Y vector)

#Create features and targets
X <- diamond %>%
  select(carat,depth,table, x, y, z, cut, color, clarity)

y<- diamond$price

#Training the model and making predictions
#At this point, we have to split our data: in this example, we will use 75% of the rows as training and 25% of the rows as test datasets

#Split the data into training and test datasets
#https://www.rdocumentation.org/packages/caret/versions/7.0-1/topics/createDataPartition

index<-createDataPartition(y,p=0.75,list=FALSE)

X_train <- X[index,]
X_test <- X[-index,] #The "-" signs lets you exclude the values you don't want (from tidyverse)
y_train <- y[index]
y_test <- y[-index]

#Train the model
#For ntree options (number of bootstrap replicates) check https://stackoverflow.com/questions/13956435/setting-values-for-ntree-and-mtry-for-random-forest-regression-model
regr <-randomForest(x=X_train, y=y_train, maxnodes = 10, ntree=10)


#We now have a model that has been pretrained and can predict values for the test data. The model's accuracy is then evaluated by comparing the predict value to the actual value in the test data. We will present this comparison in the form of a table and plot the price and carat value to make it more illustrative

#Make predictions
predictions<-predict(regr, X_test)
result <- X_test
result['price'] <- y_test
result['prediction']<-predictions

head(result)

#Visualization
#Build scatterplot

ggplot () + 
  geom_point(aes(x= X_test$carat, y=y_test, color="red", alpha = 0.5)) + 
  geom_point(aes(x= X_test$carat, y=predictions, color= "blue", alpha = 0.5)) +
  labs(x="Carats", y= "Price", color = "", alpha = "transparency") + 
  scale_color_manual(labels = c("Predicted","Real"), values=c("blue","red"))

#To estimate our model more precisely, we will look at mean absolute error, mean square error and R-squared scores

##Precision, recall
print(paste0('MAE:', mae(y_test,predictions)))
print(paste0('MSE:', caret::postResample(predictions,y_test)['RMSE']^2))
print(paste0('R2:', caret::postResample(predictions,y_test)['Rsquared']))

##We will now build a custom RF model to determine the best parameters and compare the outputs from various combinations in order to tune the parameter ntree(number of trees in the forest) and maxnodes(maximum number of terminal nodes)

#Tuning the parameters

#If training takes too long try setting up a lower value of N
N=100 #length(X_train)
X_train_ = X_train[1:N,]
y_train_ = y_train[1:N]

seed <- 7 #I believe setting the seed insure reproducible findings
metric<-'RMSE'

customRF <- list(type= "Regression", library = "randomForest", loop=NULL)

customRF$parameters <- data.frame(parameter = c("maxnodes", "ntree"), class = rep("numeric",2),label = c("maxnodes","ntree"))

customRF$grid <- function(x,y,len=NULL,search = "grid"){}

customRF$fit <- function(x,y,wts,param,lev,last,weights,classProbs,...){
  randomForest(x,y,maxnodes=param$maxnodes, ntree = param$ntree, ...)
}

customRF$predict <-function(modelFit, newdata, preProc = NULL, submodels=NULL)
  predict(modelFit,newdata)
customRF$prob <-function(modelFit, newdata, preProc = NULL, submodels=NULL)
  predict(modelFit,newdata,type="prob")

customRF$sort <-function(x) x[order(x[,1]),]
customRF$levels <- function(x) x$classes

#Set grid search parameters
control <-trainControl(method="repeatedcv", number = 10, repeats = 3, search = "grid")

#Outline the grid parameters
tunegrid <- expand.grid(.maxnodes=c(70,80,90,100),.ntree=c(900,1000,1100))
set.seed(seed)

#train the model
rf_gridsearch <-train(x=X_train_, y=y_train_, method=customRF, metric=metric,tuneGrid=tunegrid,trcontrol = control)

#Visualization of random forest

#lets visualize the impact of tuned parameters on RMSE. the best performing model should be the one with the lowest RMSE

plot(rf_gridsearch)
rf_gridsearch$bestTune

#Visualizing variable importance
varImpPlot(rf_gridsearch$finalModel, main="Feature Importance")

#from the plot you can see that that the size of the diamond (x, y and z refer to length, width and depth) and the weight (carat) contain the major part of the predictive power.

```

###Repeat random forest tutorial trying to determine which microbiome taxa species are most predictive of the BGUS pathway abundance in the data

```{r}

###Import the mapping file and merging with the clinical data Option 1

#mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

#freq <-table(mtxmapping$Month)
#freq

#Filter it to only the PK observations
#mtxmapping.pk<-mtxmapping%>%filter(grepl("PK|pk",Month))

#Now match the PIDs from mtxmapping.pk to those in the democlinical file
#mtxmapping.pk<-mtxmapping.pk%>%filter(PID%in%democlinical$PID)
#democlinical.pk<-democlinical%>%filter(PID%in%mtxmapping.pk$PID)

#Merging both datasets to make the demographic dataset for the metatranscriptomics analysis
#mtxmapping.pk.all<-merge(mtxmapping.pk,democlinical.pk,by="PID") 

###Import the mapping file and merging with the clinical data Option 2

#Importing BLAST BGUS data

bgus.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_05_00_Summarized_BGUS_Counts_prep.xlsx",sheet = '00_Summarized_BGUS_Counts')

#Importing the total reads data from MTX samples for relative abundance calculations

mtx.reads.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_05_00_Shotgun_list_HUMANN_sorting_collection_date_time_V10_readsprep.xlsx",sheet = 'BGUS_reads_match')

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")

bgus.reads.data <-merge(mtx.reads.data,bgus.data,by="id_match")

#Importing the sample key data from the mgx and mtx data
#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. 
#In the mtx output, we have a PK sample for 4022 and an M1 sample for 4016 I will rename the M1 to PK in the month column to ease the merging process, the original data is in "Month_Notes)

sample.id.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_04_Shotgun_list_HUMANN_sorting_collection_date_time_V10_prep.xlsx",sheet = 'mgx_mtx_match')

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")

bgus.reads.data.2 <-merge(bgus.reads.data,sample.id.data,by="id_match")

# merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")

pk.data<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_Democlinical_prep.xlsx",sheet = 'Demographics_mtz')

#renaming the pid column
#names(gf.data)[1]<-"PID"
#names(gf.data)[2]<-"PID"

names(pk.data)
names(pk.data)[1]<-"record_id"

#adding indicator variable for cohort type

pk.data$pk_check<-"PK"

names(bgus.reads.data.2)

bgus.reads.data.2$record_id
pk.data$record_id

pk.bgus.reads.data.2 <-merge(pk.data,bgus.reads.data.2,by="record_id")

#Renaming the dataframe for ease of use for the rest of the code

mtxmapping.pk.all<-pk.bgus.reads.data.2 

#Check which PS observations I am missing in the file 

bgus.data.check <-merge(bgus.data,sample.id.data,by="id_match")
bgus.reads.data.3 <-merge(bgus.data.check,mtx.reads.data,by="id_match")
pk.bgus.reads.data.3 <-merge(pk.data,bgus.reads.data.3,by="record_id")
mtxmapping.pk.all<-pk.bgus.reads.data.3 

#Exporting the dataset as a tab separated text file to compare with the PIDs from Moataz's PK demographic file: We are Missing 4016, 4017, and 4018, and the BGUS data also included the PK myfortic 3001, 3003 and 3005, which is why the total number seemed to match at first

#write.table(mtxmapping.pk.all, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_mtx_mapping_test", sep = "\t", row.names = FALSE)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

mtxmapping.pk.all$metagx_id<-substring(mtxmapping.pk.all$metatx_id,1,7)
mtxmapping.pk.all$metatx_id_2<-substring(mtxmapping.pk.all$metatx_id,1,7)

#Generate PS and CS subdatasets for further analyses

#mgxmapping.pk.ps<-mgxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mgxmapping.pk.cs<-mgxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxpathmapping.pk.ps<-mtxpathmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxpathmapping.pk.cs<-mtxpathmapping.pk.all%>%filter(Cohort.x =="CS") #47

mgxmapping.pk.all<-mtxmapping.pk.all #81
mgxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort =="PS")  #33
mgxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort =="CS") #47

mtxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort =="PS")  #33
mtxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort =="CS") #47

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#df_input_data (mtx_species) {.hidden .unlisted .unnumbered}
##creating PK only, PK_PS and PK_CS metatranscriptomics files {.hidden .unlisted .unnumbered}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.species.xlsx",sheet = "Sheet1")

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.species.xlsx", sheet = "Sheet1")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

# Restore the object
mgxspecies <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

#Now transpose the data (unlist the column names and the row names to save them as vectors)

gene_list<-unlist((mgxspecies[,1]))
sample_list<-unlist((mgxspecies[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mgx.all<-data.frame(t(mgxspecies))

#Trying to get rid of the colnames that snuck in the data as the first row
#mgx.all <- mgx.all %>% filter(!str_detect(1, "clade_name")) #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
str(mgx.all)
names(mgx.all)
rownames(mgx.all)<-colnames(mgxspecies) 
mgx.all<-mgx.all%>%filter(!grepl("s__Blautia_wexlerae",X1)) #option 2 with !grepl(this version worked)

colnames(mgx.all)<-gene_list
names(mgx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

mgx.all$metagx_id_str <- rownames(mgx.all)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgx.all<-mgx.all%>%relocate(metagx_id, .before=1)

#Create subset mtx files

mgx.pk<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.all$metagx_id)
mgx.ps<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.ps$metagx_id)
mgx.cs<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.cs$metagx_id)

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

#Set working directory
setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
#pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data_2.xlsx')

#Check the data type for all the variable we want to combine
names(pk.bgus.reads.data.4)
summary(pk.bgus.reads.data.4)

#Keep the variables you need for the random forest tutorial

pk_rf <- pk.bgus.reads.data.4 %>%
  select(metatx_id_2,GLUCUROCAT_PWY_3)

#Renaming the merging variable

#pk_rf <- pk_rf %>% 
#  rename(metatx_id_2 = metagx_id)

pk_rf$metagx_id <-pk_rf$metatx_id_2

view(pk_rf)
names(pk_rf)

## merge two data frames by ID 
#total <- merge(data frameA,data frameB,by="ID")

pk_rf_2 <-merge(pk_rf,mgx.all,by="metagx_id")

#Remove the metagx_id for the RF portion

pk_rf_3 <- pk_rf_2 %>%
  select(-metagx_id,-metatx_id_2)

```

###Resuming the RF tutorial

```{r}

#Check variable types
summary(pk_rf_3)

#Convert the full dataframe to numeric
#https://www.geeksforgeeks.org/r-language/how-to-convert-entire-dataframe-to-numeric-while-preserving-decimals-in-r/

# Convert columns to numeric using type.convert()
pk_rf_4 <- as.data.frame(lapply(pk_rf_3, type.convert, as.is = TRUE))

# Print the converted data frame
print(pk_rf_4)

# Check the data types of the columns
str(pk_rf_4)

#To use random forest, all you need is to define the features (X vector) and the target you are trying to predict (Y vector)

#Create features and targets
X <- pk_rf_4 %>%
  select(-GLUCUROCAT_PWY_3)

y<- pk_rf_4$GLUCUROCAT_PWY_3

#Training the model and making predictions
#At this point, we have to split our data: in this example, we will use 75% of the rows as training and 25% of the rows as test datasets

#Split the data into training and test datasets
#https://www.rdocumentation.org/packages/caret/versions/7.0-1/topics/createDataPartition

index<-createDataPartition(y,p=0.75,list=FALSE)

X_train <- X[index,]
X_test <- X[-index,] #The "-" signs lets you exclude the values you don't want (from tidyverse)
y_train <- y[index]
y_test <- y[-index]

#Train the model
#For ntree options (number of bootstrap replicates) check https://stackoverflow.com/questions/13956435/setting-values-for-ntree-and-mtry-for-random-forest-regression-model
regr <-randomForest(x=X_train, y=y_train, maxnodes = 10, ntree=10)


#We now have a model that has been pretrained and can predict values for the test data. The model's accuracy is then evaluated by comparing the predict value to the actual value in the test data. We will present this comparison in the form of a table and plot the price and carat value to make it more illustrative

#Make predictions
predictions<-predict(regr, X_test)
result <- X_test
result['BGUS_pathway_value'] <- y_test
result['BGUS_pathway_prediction']<-predictions

head(result)

#Visualization
#Build scatterplot

ggplot () + 
  geom_point(aes(x= X_test$s__Blautia_wexlerae, y=y_test, color="red", alpha = 0.5)) + 
  geom_point(aes(x= X_test$s__Blautia_wexlerae, y=predictions, color= "blue", alpha = 0.5)) +
  labs(x="Blautia_W", y= "BGUS_pathway_transcrupts", color = "", alpha = "transparency") + 
  scale_color_manual(labels = c("Predicted","Real"), values=c("blue","red"))

#To estimate our model more precisely, we will look at mean absolute error, mean square error and R-squared scores

##Precision, recall
print(paste0('MAE:', mae(y_test,predictions)))
print(paste0('MSE:', caret::postResample(predictions,y_test)['RMSE']^2))
print(paste0('R2:', caret::postResample(predictions,y_test)['Rsquared']))

##We will now build a custom RF model to determine the best parameters and compare the outputs from various combinations in order to tune the parameter ntree(number of trees in the forest) and maxnodes(maximum number of terminal nodes)

#Tuning the parameters

#If training takes too long try setting up a lower value of N
#N=60 #length(X_train)
N=1000 #length(X_train)
X_train_ = X_train[1:N,]
y_train_ = y_train[1:N]

seed <- 7 #I believe setting the seed insure reproducible findings
metric<-'RMSE'

customRF <- list(type= "Regression", library = "randomForest", loop=NULL)

customRF$parameters <- data.frame(parameter = c("maxnodes", "ntree"), class = rep("numeric",2),label = c("maxnodes","ntree"))

customRF$grid <- function(x,y,len=NULL,search = "grid"){}

customRF$fit <- function(x,y,wts,param,lev,last,weights,classProbs,...){
  randomForest(x,y,maxnodes=param$maxnodes, ntree = param$ntree, ...)
}

customRF$predict <-function(modelFit, newdata, preProc = NULL, submodels=NULL)
  predict(modelFit,newdata)
customRF$prob <-function(modelFit, newdata, preProc = NULL, submodels=NULL)
  predict(modelFit,newdata,type="prob")

customRF$sort <-function(x) x[order(x[,1]),]
customRF$levels <- function(x) x$classes

#Set grid search parameters
control <-trainControl(method="repeatedcv", number = 10, repeats = 3, search = "grid")

#Outline the grid parameters
tunegrid <- expand.grid(.maxnodes=c(70,80,90,100),.ntree=c(900,1000,1100))
set.seed(seed)

#train the model
rf_gridsearch <-train(x=X_train_, y=y_train_, method=customRF, metric=metric,tuneGrid=tunegrid,trcontrol = control)

#Visualization of random forest

#lets visualize the impact of tuned parameters on RMSE. the best performing model should be the one with the lowest RMSE

plot(rf_gridsearch)
rf_gridsearch$bestTune

#Visualizing variable importance
varImpPlot(rf_gridsearch$finalModel, main="Feature Importance")

```

###Abdelrahman sample code to test for adjustments{.hidden .unlisted .unnumbered}

```{r}
#| fake R chunks, echo = FALSE, 
#| results='hide', warning=FALSE

#library(compositions)
 
#Filter species to 15% or 10% relative abundance

# ================== Step 1: Filter out rare taxa =========================== #
 
# Calculate counts of cells equal to zero for each column

 cells_eq_zero <- colSums(microbiome.species.cna == 0, na.rm = TRUE)
 
# Create a data frame with column names and counts of cells equal to zero

 count_table <- data.frame(

   Species_Name = names(cells_eq_zero),

   Counts_of_0 = cells_eq_zero

 )

 count_table <- count_table[order(count_table$Counts_of_0, decreasing = T),]
 
rownames(count_table) <- NULL
 
table(count_table$Counts_of_0)
 
# count number > zeros
 
cells_gt_zero <- colSums(microbiome.species.cna > 0, na.rm = TRUE)
 
# Create a data frame with column names and counts of cells equal to zero

 count_table2 <- data.frame(

   Species_Name = names(cells_gt_zero),

   Counts_of_gt_0 = cells_gt_zero

 )

 count_table2 <- count_table2[order(count_table2$Counts_of_gt_0, decreasing = T),]
 
# Keep taxa present in at least 13 (>=15% of the population) samples
 
species_to_keep <- names(cells_gt_zero[cells_gt_zero >= 13])

# Filter the columns (taxa), not rows
 
microbiome_filtered <- microbiome.species.cna[, species_to_keep]

# Step 4: Define pseudocount as 1/10 of that value

offset <- lowest_non_zero * 0.1
 
microbiome_filtered_peudo <- microbiome_filtered

microbiome_filtered_peudo[microbiome_filtered_peudo == 0] <- offset #replacing the zeros with the offset value
 
# Step 2: Re-apply closure (renormalize rows to sum to 1 since th)

microbiome_renorm <- microbiome_filtered_peudo / rowSums(microbiome_filtered_peudo)

# Step 4: Perform CLR transformation
 
microbiome_clr_2 <- clr(microbiome_renorm)
 
```

###Abdelrahman adjustments on mtx analysis data

```{r}

#install.packages("compositions")
library(compositions)
 
#Filter species to 15% or 10% relative abundance

# ================== Step 1: Filter out rare taxa =========================== #
 
# Calculate counts of cells equal to zero for each column

 #cells_eq_zero <- colSums(microbiome.species.cna == 0, na.rm = TRUE)
 
# Create a data frame with column names and counts of cells equal to zero

# count_table <- data.frame(

#   Species_Name = names(cells_eq_zero),

#   Counts_of_0 = cells_eq_zero

# )

# count_table <- count_table[order(count_table$Counts_of_0, decreasing = T),]
 
#rownames(count_table) <- NULL
 
#table(count_table$Counts_of_0)
 
# count number > zeros
 
#cells_gt_zero <- colSums(microbiome.species.cna > 0, na.rm = TRUE)
 
# Create a data frame with column names and counts of cells equal to zero

 #count_table2 <- data.frame(

#   Species_Name = names(cells_gt_zero),

 #  Counts_of_gt_0 = cells_gt_zero

 #)

 #count_table2 <- count_table2[order(count_table2$Counts_of_gt_0, decreasing = T),]
 
# Keep taxa present in at least 13 (>=15% of the population) samples
 
#species_to_keep <- names(cells_gt_zero[cells_gt_zero >= 13])

# Filter the columns (taxa), not rows
 
#microbiome_filtered <- microbiome.species.cna[, species_to_keep]

# Step 4: Define pseudocount as 1/10 of that value

#offset <- lowest_non_zero * 0.1
 
#microbiome_filtered_peudo <- microbiome_filtered

#microbiome_filtered_peudo[microbiome_filtered_peudo == 0] <- offset #replacing the zeros with the offset value
 
# Step 2: Re-apply closure (renormalize rows to sum to 1 since th)

#microbiome_renorm <- microbiome_filtered_peudo / rowSums(microbiome_filtered_peudo)

# Step 4: Perform CLR transformation

#microbiome_clr_2 <- clr(microbiome_renorm)

pk_rf_comp <- pk_rf_4 %>%
  select(-GLUCUROCAT_PWY_3) 

pk_rf_comp_2 <- clr(pk_rf_comp)

```

###Abdelrahman sample code to test for adjustments {.hidden .unlisted .unnumbered}

```{r}
#| fake R chunks, echo = FALSE, 
#| results='hide', warning=FALSE

rf_AUCmodel <- randomForest(log(AUC0.tlast_h.mg.L) ~ ., 

                             data = microbiome_clr.pk_015prev_comm.spp[,-c(1,3:12)],

                             importance = TRUE, ntree = 1000)

 plot(rf_AUCmodel, main = "OOB Error vs Number of Trees") # this is to determine the reasonable ntree number. check for a plateau to set the limit of ntree value.
 
importance_vals_auc <- importance(rf_AUCmodel, type = 1)  # %IncMSE (permutation importance)
 
importanceAUC_df <- data.frame(Taxon = rownames(importance_vals_auc),

                                Importance = importance_vals_auc[, 1]) %>%

   arrange(desc(Importance))
 
top_rfAUC <- importanceAUC_df %>% top_n(25, Importance)
 
ggplot(top_rfAUC, aes(x = reorder(Taxon, Importance), y = Importance)) +

   geom_col(fill = "steelblue") +

   coord_flip() + theme_minimal() +

   labs(title = "Top 20 Microbiome Predictors of AUC0-12", 

        x = "Microbiome Species", y = "%IncMSE")

```

###Abdelrahman adjustments rf 

```{r}

y<- pk_rf_4$GLUCUROCAT_PWY_3

#rf_AUCmodel <- randomForest(log(AUC0.tlast_h.mg.L) ~ ., 
#
#                             data = microbiome_clr.pk_015prev_comm.spp[,-c(1,3:12)],
#
#                             importance = TRUE, ntree = 1000)

rf_AUCmodel <- randomForest(y ~ ., 

                             data = pk_rf_comp_2,

                             importance = TRUE, ntree = 1000)

 plot(rf_AUCmodel, main = "OOB Error vs Number of Trees") # this is to determine the reasonable ntree number. check for a plateau to set the limit of ntree value.
 
importance_vals_auc <- importance(rf_AUCmodel, type = 1)  # %IncMSE (permutation importance)
 
importanceAUC_df <- data.frame(Taxon = rownames(importance_vals_auc),

                                Importance = importance_vals_auc[, 1]) %>%

   arrange(desc(Importance))
 
top_rfAUC <- importanceAUC_df %>% top_n(25, Importance)
 
ggplot(top_rfAUC, aes(x = reorder(Taxon, Importance), y = Importance)) +

   geom_col(fill = "Grey") +

   coord_flip() + theme_minimal() +
  
  scale_fill_grey() + 

   labs(title = "Top 25 Microbiome Predictors of GLUCUROCAT PWY", 

        x = "Microbiome Species", y = "%IncMSE")

```

#Run the differential abundance analysis from "'/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/C_02_16S_Program 102_R_44_RmarkdownMPAEHRanalysis_V2.Rmd'"

##Full Cohort Metatranscriptomics data prep

###df_input_metadata

``` {r}
#Merge the MPAEHR_metadata file with the metatranscriptomics mapping file

#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis
#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file

#democlinical<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

democlinical<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. I will rename the M4 to PK to ease the merging process

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
democlinical$Month[democlinical$Month=="M4"] <- "PK" 

#Import the mapping file
#mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

freq <-table(mtxmapping$Month)
freq

#Filter it to only the PK observations

mtxmapping.pk<-mtxmapping%>%filter(grepl("PK|pk",Month))

#Now match the PIDs from mtxmapping.pk to those in the democlinical file

mtxmapping.pk<-mtxmapping.pk%>%filter(PID%in%democlinical$PID)
democlinical.pk<-democlinical%>%filter(PID%in%mtxmapping.pk$PID)

#Merging both datasets to make the demographic dataset for the metatranscriptomics analysis

mtxmapping.pk.all<-merge(mtxmapping.pk,democlinical.pk,by="PID") 

#This is the equivalent dataset
#Set working directory

setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

#mgxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metagx_id, .before=1)
#mgxmapping.pk.all<-mgxmapping.pk.all%>%filter(!PID==4036)
#mtxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_id, .before=1)
#mtxpathmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_path_id, .before=1)

#Generate PS and CS subdatasets for further analyses

#mgxmapping.pk.ps<-mgxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mgxmapping.pk.cs<-mgxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxpathmapping.pk.ps<-mtxpathmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxpathmapping.pk.cs<-mtxpathmapping.pk.all%>%filter(Cohort.x =="CS") #47

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_all.txt", sep = "\t", row.names = FALSE)

write.table(pk.bgus.reads.data.4, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

```

###df_input_data (mtx_species)

```{r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.species.xlsx",sheet = "Sheet1")

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.species.xlsx", sheet = "Sheet1")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

# Restore the object
mgxspecies <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

#Now transpose the data (unlist the column names and the row names to save them as vectors)

gene_list<-unlist((mgxspecies[,1]))
sample_list<-unlist((mgxspecies[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mgx.all<-data.frame(t(mgxspecies))

#Trying to get rid of the colnames that snuck in the data as the first row
mgx.all <- mgx.all %>% filter(!str_detect(1, "clade_name")) #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
str(mgx.all)
names(mgx.all)
rownames(mgx.all)<-colnames(mgxspecies) 
mgx.all<-mgx.all%>%filter(!grepl("s__Blautia_wexlerae",X1)) #option 2 with !grepl(this version worked)

colnames(mgx.all)<-gene_list
names(mgx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

mgx.all$metagx_id_str <- rownames(mgx.all)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

#mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)
mgx.all$metatx_id_2<-substring(mgx.all$metagx_id_str,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgx.all<-mgx.all%>%relocate(metatx_id_2, .before=1)

#Create subset mtx files

#mgx.pk<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.all$metagx_id)
#mgx.ps<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.ps$metagx_id)
#mgx.cs<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.cs$metagx_id)

#There is one observation that was missing from the mgx data provided by Dr. Staley, I believe it is for PID 4036?
mgx.pk<-mgx.all%>%filter(metatx_id_2%in%pk.bgus.reads.data.4$metatx_id_2)
pk.bgus.reads.data.4<-pk.bgus.reads.data.4%>%filter(metatx_id_2%in%mgx.pk$metatx_id_2)

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

write.table(mgx.pk, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk.txt", sep = "\t", row.names = FALSE)
write.table(pk.bgus.reads.data.4, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

```

###Prepping the MISSION mgx (from MTX) data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
#install.packages("vegan")
library(vegan)
#if (!require("BiocManager", quietly = TRUE))
#    install.packages("BiocManager")

#BiocManager::install("Maaslin2")
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '//Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
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

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
#vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

#df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
#                                           cut(MPA_AUC5_12_0_12, 
#                                               vTert, 
#                                               include.lowest = T, 
#                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
#df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)
df_input_metadata_mission$EHRpeakbinary<-ifelse(df_input_metadata_mission$MPA_EHR_Peaks_count==0,"LOW","HIGH")

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
#df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

#Calculating the ratio of MPAG AUC over MPA AUC during the EHR window
#df_input_metadata_mission$MPAG_MPA_ratio<-df_input_metadata_mission$MPAG_AUC_0_12_dose_adj/df_input_metadata_mission$MPA_AUC_0_12_dose_adj
```

###Run MGX models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_cohort", 
                            fixed_effects  = c("cohort"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_0_12", 
                            fixed_effects  = c("MPA_AUC_0_12","cohort"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_5_12", 
                            fixed_effects  = c("MPA_AUC_5_12","cohort"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","cohort"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak", 
                            fixed_effects  = c("MPA_EHR_Peaks_count","cohort"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak_Binary", 
                            fixed_effects  = c("EHRpeakbinary","cohort"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_GLUCUROCAT_PWY_3", 
                            fixed_effects  = c("GLUCUROCAT_PWY_3","cohort"))
```

#Full Cohort, O peak vs 2+ peak

###df_input_metadata

``` {r}
#Merge the MPAEHR_metadata file with the metatranscriptomics mapping file

#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis
#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file

#democlinical<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

democlinical<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. I will rename the M4 to PK to ease the merging process

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
democlinical$Month[democlinical$Month=="M4"] <- "PK" 

#Import the mapping file
#mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

freq <-table(mtxmapping$Month)
freq

#Filter it to only the PK observations

mtxmapping.pk<-mtxmapping%>%filter(grepl("PK|pk",Month))

#Now match the PIDs from mtxmapping.pk to those in the democlinical file

mtxmapping.pk<-mtxmapping.pk%>%filter(PID%in%democlinical$PID)
democlinical.pk<-democlinical%>%filter(PID%in%mtxmapping.pk$PID)

#Merging both datasets to make the demographic dataset for the metatranscriptomics analysis

mtxmapping.pk.all<-merge(mtxmapping.pk,democlinical.pk,by="PID") 

#This is the equivalent dataset
#Set working directory

setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

#subsetting the dataset 
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
pk.bgus.reads.data.sub<-pk.bgus.reads.data.4%>%
  filter(MPA_EHR_Peaks_count %in% c("0","2"))

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

#mgxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metagx_id, .before=1)
#mgxmapping.pk.all<-mgxmapping.pk.all%>%filter(!PID==4036)
#mtxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_id, .before=1)
#mtxpathmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_path_id, .before=1)

#Generate PS and CS subdatasets for further analyses

#mgxmapping.pk.ps<-mgxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mgxmapping.pk.cs<-mgxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxpathmapping.pk.ps<-mtxpathmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxpathmapping.pk.cs<-mtxpathmapping.pk.all%>%filter(Cohort.x =="CS") #47

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(pk.bgus.reads.data.4, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

```

###df_input_data (mtx_species)

```{r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.species.xlsx",sheet = "Sheet1")

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.species.xlsx", sheet = "Sheet1")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

# Restore the object
mgxspecies <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

#Now transpose the data (unlist the column names and the row names to save them as vectors)

gene_list<-unlist((mgxspecies[,1]))
sample_list<-unlist((mgxspecies[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mgx.all<-data.frame(t(mgxspecies))

#Trying to get rid of the colnames that snuck in the data as the first row
mgx.all <- mgx.all %>% filter(!str_detect(1, "clade_name")) #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
str(mgx.all)
names(mgx.all)
rownames(mgx.all)<-colnames(mgxspecies) 
mgx.all<-mgx.all%>%filter(!grepl("s__Blautia_wexlerae",X1)) #option 2 with !grepl(this version worked)

colnames(mgx.all)<-gene_list
names(mgx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

mgx.all$metagx_id_str <- rownames(mgx.all)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

#mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)
mgx.all$metatx_id_2<-substring(mgx.all$metagx_id_str,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgx.all<-mgx.all%>%relocate(metatx_id_2, .before=1)

#Create subset mtx files

#mgx.pk<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.all$metagx_id)
#mgx.ps<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.ps$metagx_id)
#mgx.cs<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.cs$metagx_id)

#There is one observation that was missing from the mgx data provided by Dr. Staley, I believe it is for PID 4036?
mgx.pk.sub<-mgx.all%>%filter(metatx_id_2%in%pk.bgus.reads.data.sub$metatx_id_2)
pk.bgus.reads.data.sub<-pk.bgus.reads.data.sub%>%filter(metatx_id_2%in%mgx.pk.sub$metatx_id_2)

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

write.table(mgx.pk.sub, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk_sub.txt", sep = "\t", row.names = FALSE)
write.table(pk.bgus.reads.data.sub, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_sub.txt", sep = "\t", row.names = FALSE)

```

###Prepping the MISSION mgx (from MTX) data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
#install.packages("vegan")
library(vegan)
#if (!require("BiocManager", quietly = TRUE))
#    install.packages("BiocManager")

#BiocManager::install("Maaslin2")
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk_sub.txt', header = TRUE, sep = "\t", row.names = 1)
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

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_sub.txt', 
                                       header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
#vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

#df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
#                                           cut(MPA_AUC5_12_0_12, 
#                                               vTert, 
#                                               include.lowest = T, 
#                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
#df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)
df_input_metadata_mission$EHRpeakbinary<-ifelse(df_input_metadata_mission$MPA_EHR_Peaks_count==0,"LOW","HIGH")

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
#df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

#Calculating the ratio of MPAG AUC over MPA AUC during the EHR window
#df_input_metadata_mission$MPAG_MPA_ratio<-df_input_metadata_mission$MPAG_AUC_0_12_dose_adj/df_input_metadata_mission$MPA_AUC_0_12_dose_adj
```

###Run MGX models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_cohort_sub", 
                            fixed_effects  = c("cohort"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_0_12_sub", 
                            fixed_effects  = c("MPA_AUC_0_12","cohort"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_5_12_sub", 
                            fixed_effects  = c("MPA_AUC_5_12","cohort"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_sub", 
                            fixed_effects  = c("MPA_AUC5_12_0_12","cohort"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak_sub", 
                            fixed_effects  = c("MPA_EHR_Peaks_count","cohort"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak_Binary_sub", 
                            fixed_effects  = c("EHRpeakbinary","cohort"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_GLUCUROCAT_PWY_3_sub", 
                            fixed_effects  = c("GLUCUROCAT_PWY_3","cohort"))
```

#PS cohort

###df_input_metadata

``` {r}
#Merge the MPAEHR_metadata file with the metatranscriptomics mapping file

#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis
#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file

#democlinical<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

democlinical<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. I will rename the M4 to PK to ease the merging process

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
democlinical$Month[democlinical$Month=="M4"] <- "PK" 

#Import the mapping file
#mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

freq <-table(mtxmapping$Month)
freq

#Filter it to only the PK observations

mtxmapping.pk<-mtxmapping%>%filter(grepl("PK|pk",Month))

#Now match the PIDs from mtxmapping.pk to those in the democlinical file

mtxmapping.pk<-mtxmapping.pk%>%filter(PID%in%democlinical$PID)
democlinical.pk<-democlinical%>%filter(PID%in%mtxmapping.pk$PID)

#Merging both datasets to make the demographic dataset for the metatranscriptomics analysis

mtxmapping.pk.all<-merge(mtxmapping.pk,democlinical.pk,by="PID") 

#This is the equivalent dataset
#Set working directory

setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

#subsetting the dataset 
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
pk.bgus.reads.data.ps<-pk.bgus.reads.data.4%>%
  filter(cohort %in% c("PROS"))

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

#mgxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metagx_id, .before=1)
#mgxmapping.pk.all<-mgxmapping.pk.all%>%filter(!PID==4036)
#mtxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_id, .before=1)
#mtxpathmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_path_id, .before=1)

#Generate PS and CS subdatasets for further analyses

#mgxmapping.pk.ps<-mgxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mgxmapping.pk.cs<-mgxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxpathmapping.pk.ps<-mtxpathmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxpathmapping.pk.cs<-mtxpathmapping.pk.all%>%filter(Cohort.x =="CS") #47

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(pk.bgus.reads.data.4, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

```

###df_input_data (mtx_species)

```{r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.species.xlsx",sheet = "Sheet1")

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.species.xlsx", sheet = "Sheet1")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

# Restore the object
mgxspecies <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

#Now transpose the data (unlist the column names and the row names to save them as vectors)

gene_list<-unlist((mgxspecies[,1]))
sample_list<-unlist((mgxspecies[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mgx.all<-data.frame(t(mgxspecies))

#Trying to get rid of the colnames that snuck in the data as the first row
mgx.all <- mgx.all %>% filter(!str_detect(1, "clade_name")) #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
str(mgx.all)
names(mgx.all)
rownames(mgx.all)<-colnames(mgxspecies) 
mgx.all<-mgx.all%>%filter(!grepl("s__Blautia_wexlerae",X1)) #option 2 with !grepl(this version worked)

colnames(mgx.all)<-gene_list
names(mgx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

mgx.all$metagx_id_str <- rownames(mgx.all)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

#mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)
mgx.all$metatx_id_2<-substring(mgx.all$metagx_id_str,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgx.all<-mgx.all%>%relocate(metatx_id_2, .before=1)

#Create subset mtx files

#mgx.pk<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.all$metagx_id)
#mgx.ps<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.ps$metagx_id)
#mgx.cs<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.cs$metagx_id)

#There is one observation that was missing from the mgx data provided by Dr. Staley, I believe it is for PID 4036?
mgx.pk.ps<-mgx.all%>%filter(metatx_id_2%in%pk.bgus.reads.data.ps$metatx_id_2)
pk.bgus.reads.data.ps<-pk.bgus.reads.data.ps%>%filter(metatx_id_2%in%mgx.pk.ps$metatx_id_2)

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

write.table(mgx.pk.ps, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk_ps.txt", sep = "\t", row.names = FALSE)
write.table(pk.bgus.reads.data.ps, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

```

###Prepping the MISSION mgx (from MTX) data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
#install.packages("vegan")
library(vegan)
#if (!require("BiocManager", quietly = TRUE))
#    install.packages("BiocManager")

#BiocManager::install("Maaslin2")
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk_ps.txt', header = TRUE, sep = "\t", row.names = 1)
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

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Fix the error by saving the file as an excel file and then back to text.
df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_ps.txt', 
                                       header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
#vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

#df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
#                                           cut(MPA_AUC5_12_0_12, 
#                                               vTert, 
#                                               include.lowest = T, 
#                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
#df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)
df_input_metadata_mission$EHRpeakbinary<-ifelse(df_input_metadata_mission$MPA_EHR_Peaks_count==0,"LOW","HIGH")

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
#df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

#Calculating the ratio of MPAG AUC over MPA AUC during the EHR window
#df_input_metadata_mission$MPAG_MPA_ratio<-df_input_metadata_mission$MPAG_AUC_0_12_dose_adj/df_input_metadata_mission$MPA_AUC_0_12_dose_adj
```

###Run MGX models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")


fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_0_12_ps", 
                            fixed_effects  = c("MPA_AUC_0_12"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_5_12_ps", 
                            fixed_effects  = c("MPA_AUC_5_12"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_ps", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak_ps", 
                            fixed_effects  = c("MPA_EHR_Peaks_count"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak_Binary_ps", 
                            fixed_effects  = c("EHRpeakbinary"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_GLUCUROCAT_PWY_3_ps", 
                            fixed_effects  = c("GLUCUROCAT_PWY_3"))
```

#PS cohort sub

###df_input_metadata

``` {r}
#Merge the MPAEHR_metadata file with the metatranscriptomics mapping file

#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis
#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file

#democlinical<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

democlinical<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. I will rename the M4 to PK to ease the merging process

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
democlinical$Month[democlinical$Month=="M4"] <- "PK" 

#Import the mapping file
#mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

freq <-table(mtxmapping$Month)
freq

#Filter it to only the PK observations

mtxmapping.pk<-mtxmapping%>%filter(grepl("PK|pk",Month))

#Now match the PIDs from mtxmapping.pk to those in the democlinical file

mtxmapping.pk<-mtxmapping.pk%>%filter(PID%in%democlinical$PID)
democlinical.pk<-democlinical%>%filter(PID%in%mtxmapping.pk$PID)

#Merging both datasets to make the demographic dataset for the metatranscriptomics analysis

mtxmapping.pk.all<-merge(mtxmapping.pk,democlinical.pk,by="PID") 

#This is the equivalent dataset
#Set working directory

setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

#subsetting the dataset 
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
pk.bgus.reads.data.ps<-pk.bgus.reads.data.4%>%
  filter(cohort %in% c("PROS"))

pk.bgus.reads.data.ps.sub<-pk.bgus.reads.data.ps%>%
  filter(MPA_EHR_Peaks_count %in% c("0","2"))

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

#mgxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metagx_id, .before=1)
#mgxmapping.pk.all<-mgxmapping.pk.all%>%filter(!PID==4036)
#mtxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_id, .before=1)
#mtxpathmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_path_id, .before=1)

#Generate PS and CS subdatasets for further analyses

#mgxmapping.pk.ps<-mgxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mgxmapping.pk.cs<-mgxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxpathmapping.pk.ps<-mtxpathmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxpathmapping.pk.cs<-mtxpathmapping.pk.all%>%filter(Cohort.x =="CS") #47

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(pk.bgus.reads.data.4, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

```

###df_input_data (mtx_species)

```{r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.species.xlsx",sheet = "Sheet1")

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.species.xlsx", sheet = "Sheet1")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

# Restore the object
mgxspecies <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

#Now transpose the data (unlist the column names and the row names to save them as vectors)

gene_list<-unlist((mgxspecies[,1]))
sample_list<-unlist((mgxspecies[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mgx.all<-data.frame(t(mgxspecies))

#Trying to get rid of the colnames that snuck in the data as the first row
mgx.all <- mgx.all %>% filter(!str_detect(1, "clade_name")) #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
str(mgx.all)
names(mgx.all)
rownames(mgx.all)<-colnames(mgxspecies) 
mgx.all<-mgx.all%>%filter(!grepl("s__Blautia_wexlerae",X1)) #option 2 with !grepl(this version worked)

colnames(mgx.all)<-gene_list
names(mgx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

mgx.all$metagx_id_str <- rownames(mgx.all)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

#mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)
mgx.all$metatx_id_2<-substring(mgx.all$metagx_id_str,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgx.all<-mgx.all%>%relocate(metatx_id_2, .before=1)

#Create subset mtx files

#mgx.pk<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.all$metagx_id)
#mgx.ps<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.ps$metagx_id)
#mgx.cs<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.cs$metagx_id)

#There is one observation that was missing from the mgx data provided by Dr. Staley, I believe it is for PID 4036?
mgx.pk.ps.sub<-mgx.all%>%filter(metatx_id_2%in%pk.bgus.reads.data.ps.sub$metatx_id_2)
pk.bgus.reads.data.ps.sub<-pk.bgus.reads.data.ps.sub%>%
    filter(metatx_id_2%in%mgx.pk.ps.sub$metatx_id_2)

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

write.table(mgx.pk.ps.sub, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk_ps_sub.txt", sep = "\t", row.names = FALSE)
write.table(pk.bgus.reads.data.ps.sub, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_ps_sub.txt", sep = "\t", row.names = FALSE)

```

###Prepping the MISSION mgx (from MTX) data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
#install.packages("vegan")
library(vegan)
#if (!require("BiocManager", quietly = TRUE))
#    install.packages("BiocManager")

#BiocManager::install("Maaslin2")
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk_ps_sub.txt', header = TRUE, sep = "\t", row.names = 1)
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

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Fix the error by saving the file as an excel file and then back to text.
df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_ps_sub.txt', 
                                       header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
#vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

#df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
#                                           cut(MPA_AUC5_12_0_12, 
#                                               vTert, 
#                                               include.lowest = T, 
#                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
#df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)
df_input_metadata_mission$EHRpeakbinary<-ifelse(df_input_metadata_mission$MPA_EHR_Peaks_count==0,"LOW","HIGH")

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
#df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

#Calculating the ratio of MPAG AUC over MPA AUC during the EHR window
#df_input_metadata_mission$MPAG_MPA_ratio<-df_input_metadata_mission$MPAG_AUC_0_12_dose_adj/df_input_metadata_mission$MPA_AUC_0_12_dose_adj
```

###Run MGX models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")


fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_0_12_ps_sub", 
                            fixed_effects  = c("MPA_AUC_0_12"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_5_12_ps_sub", 
                            fixed_effects  = c("MPA_AUC_5_12"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_ps_sub", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak_ps_sub", 
                            fixed_effects  = c("MPA_EHR_Peaks_count"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak_Binary_ps_sub", 
                            fixed_effects  = c("EHRpeakbinary"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_GLUCUROCAT_PWY_3_ps_sub", 
                            fixed_effects  = c("GLUCUROCAT_PWY_3"))
```

#CS cohort

###df_input_metadata

``` {r}
#Merge the MPAEHR_metadata file with the metatranscriptomics mapping file

#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis
#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file

#democlinical<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

democlinical<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. I will rename the M4 to PK to ease the merging process

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
democlinical$Month[democlinical$Month=="M4"] <- "PK" 

#Import the mapping file
#mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

freq <-table(mtxmapping$Month)
freq

#Filter it to only the PK observations

mtxmapping.pk<-mtxmapping%>%filter(grepl("PK|pk",Month))

#Now match the PIDs from mtxmapping.pk to those in the democlinical file

mtxmapping.pk<-mtxmapping.pk%>%filter(PID%in%democlinical$PID)
democlinical.pk<-democlinical%>%filter(PID%in%mtxmapping.pk$PID)

#Merging both datasets to make the demographic dataset for the metatranscriptomics analysis

mtxmapping.pk.all<-merge(mtxmapping.pk,democlinical.pk,by="PID") 

#This is the equivalent dataset
#Set working directory

setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

#subsetting the dataset 
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
pk.bgus.reads.data.cs<-pk.bgus.reads.data.4%>%
  filter(cohort %in% c("CS"))

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

#mgxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metagx_id, .before=1)
#mgxmapping.pk.all<-mgxmapping.pk.all%>%filter(!PID==4036)
#mtxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_id, .before=1)
#mtxpathmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_path_id, .before=1)

#Generate PS and CS subdatasets for further analyses

#mgxmapping.pk.ps<-mgxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mgxmapping.pk.cs<-mgxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxpathmapping.pk.ps<-mtxpathmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxpathmapping.pk.cs<-mtxpathmapping.pk.all%>%filter(Cohort.x =="CS") #47

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(pk.bgus.reads.data.4, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

```

###df_input_data (mtx_species)

```{r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.species.xlsx",sheet = "Sheet1")

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.species.xlsx", sheet = "Sheet1")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

# Restore the object
mgxspecies <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

#Now transpose the data (unlist the column names and the row names to save them as vectors)

gene_list<-unlist((mgxspecies[,1]))
sample_list<-unlist((mgxspecies[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mgx.all<-data.frame(t(mgxspecies))

#Trying to get rid of the colnames that snuck in the data as the first row
mgx.all <- mgx.all %>% filter(!str_detect(1, "clade_name")) #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
str(mgx.all)
names(mgx.all)
rownames(mgx.all)<-colnames(mgxspecies) 
mgx.all<-mgx.all%>%filter(!grepl("s__Blautia_wexlerae",X1)) #option 2 with !grepl(this version worked)

colnames(mgx.all)<-gene_list
names(mgx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

mgx.all$metagx_id_str <- rownames(mgx.all)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

#mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)
mgx.all$metatx_id_2<-substring(mgx.all$metagx_id_str,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgx.all<-mgx.all%>%relocate(metatx_id_2, .before=1)

#Create subset mtx files

#mgx.pk<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.all$metagx_id)
#mgx.ps<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.ps$metagx_id)
#mgx.cs<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.cs$metagx_id)

#There is one observation that was missing from the mgx data provided by Dr. Staley, I believe it is for PID 4036?
mgx.pk.cs<-mgx.all%>%filter(metatx_id_2%in%pk.bgus.reads.data.cs$metatx_id_2)
pk.bgus.reads.data.cs<-pk.bgus.reads.data.cs%>%filter(metatx_id_2%in%mgx.pk.cs$metatx_id_2)

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

write.table(mgx.pk.cs, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk_cs.txt", sep = "\t", row.names = FALSE)
write.table(pk.bgus.reads.data.cs, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

```

###Prepping the MISSION mgx (from MTX) data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
#install.packages("vegan")
library(vegan)
#if (!require("BiocManager", quietly = TRUE))
#    install.packages("BiocManager")

#BiocManager::install("Maaslin2")
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk_cs.txt', header = TRUE, sep = "\t", row.names = 1)
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

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Fix the error by saving the file as an excel file and then back to text.
df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_cs.txt', 
                                       header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
#vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

#df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
#                                           cut(MPA_AUC5_12_0_12, 
#                                               vTert, 
#                                               include.lowest = T, 
#                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
#df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)
df_input_metadata_mission$EHRpeakbinary<-ifelse(df_input_metadata_mission$MPA_EHR_Peaks_count==0,"LOW","HIGH")

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
#df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

#Calculating the ratio of MPAG AUC over MPA AUC during the EHR window
#df_input_metadata_mission$MPAG_MPA_ratio<-df_input_metadata_mission$MPAG_AUC_0_12_dose_adj/df_input_metadata_mission$MPA_AUC_0_12_dose_adj
```

###Run MGX models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")


fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_0_12_ps", 
                            fixed_effects  = c("MPA_AUC_0_12"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_5_12_cs", 
                            fixed_effects  = c("MPA_AUC_5_12"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_cs", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak_cs", 
                            fixed_effects  = c("MPA_EHR_Peaks_count"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak_Binary_cs", 
                            fixed_effects  = c("EHRpeakbinary"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_GLUCUROCAT_PWY_3_cs", 
                            fixed_effects  = c("GLUCUROCAT_PWY_3"))
```

#CS cohort sub

###df_input_metadata

``` {r}
#Merge the MPAEHR_metadata file with the metatranscriptomics mapping file

#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis
#/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file

#democlinical<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

democlinical<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.xlsx",sheet = "resid_test_3")

#Note that 4022 and 4016 have their M4 Samples instead of the PK samples for the metagenomics output. I will rename the M4 to PK to ease the merging process

#Recoding a variable for ease of use (https://rpubs.com/prlicari13/541675)
democlinical$Month[democlinical$Month=="M4"] <- "PK" 

#Import the mapping file
#mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

mtxmapping<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_06_metatx_mapping_toshare_2.xlsx",sheet = "Metatx_list_HUMANN_clean")

freq <-table(mtxmapping$Month)
freq

#Filter it to only the PK observations

mtxmapping.pk<-mtxmapping%>%filter(grepl("PK|pk",Month))

#Now match the PIDs from mtxmapping.pk to those in the democlinical file

mtxmapping.pk<-mtxmapping.pk%>%filter(PID%in%democlinical$PID)
democlinical.pk<-democlinical%>%filter(PID%in%mtxmapping.pk$PID)

#Merging both datasets to make the demographic dataset for the metatranscriptomics analysis

mtxmapping.pk.all<-merge(mtxmapping.pk,democlinical.pk,by="PID") 

#This is the equivalent dataset
#Set working directory

setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

#subsetting the dataset 
#https://dplyr.tidyverse.org/reference/filter.html
#https://sparkbyexamples.com/r-programming/r-dplyr-filter/
pk.bgus.reads.data.cs<-pk.bgus.reads.data.4%>%
  filter(cohort %in% c("CS"))

pk.bgus.reads.data.cs.sub<-pk.bgus.reads.data.cs%>%
  filter(MPA_EHR_Peaks_count %in% c("0","2"))

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

#mgxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metagx_id, .before=1)
#mgxmapping.pk.all<-mgxmapping.pk.all%>%filter(!PID==4036)
#mtxmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_id, .before=1)
#mtxpathmapping.pk.all<-mtxmapping.pk.all%>%relocate(metatx_path_id, .before=1)

#Generate PS and CS subdatasets for further analyses

#mgxmapping.pk.ps<-mgxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mgxmapping.pk.cs<-mgxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxmapping.pk.ps<-mtxmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxmapping.pk.cs<-mtxmapping.pk.all%>%filter(Cohort.x =="CS") #47

#mtxpathmapping.pk.ps<-mtxpathmapping.pk.all%>%filter(Cohort.x =="PS")  #35
#mtxpathmapping.pk.cs<-mtxpathmapping.pk.all%>%filter(Cohort.x =="CS") #47

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtxpathmapping.pk.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxpathmapping_pk_all.txt", sep = "\t", row.names = FALSE)

#write.table(pk.bgus.reads.data.4, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_ps.txt", sep = "\t", row.names = FALSE)

```

###df_input_data (mtx_species)

```{r}

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.species.xlsx",sheet = "Sheet1")

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.species.xlsx", sheet = "Sheet1")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

# Restore the object
mgxspecies <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

#Now transpose the data (unlist the column names and the row names to save them as vectors)

gene_list<-unlist((mgxspecies[,1]))
sample_list<-unlist((mgxspecies[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mgx.all<-data.frame(t(mgxspecies))

#Trying to get rid of the colnames that snuck in the data as the first row
mgx.all <- mgx.all %>% filter(!str_detect(1, "clade_name")) #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
str(mgx.all)
names(mgx.all)
rownames(mgx.all)<-colnames(mgxspecies) 
mgx.all<-mgx.all%>%filter(!grepl("s__Blautia_wexlerae",X1)) #option 2 with !grepl(this version worked)

colnames(mgx.all)<-gene_list
names(mgx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

mgx.all$metagx_id_str <- rownames(mgx.all)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

#mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)
mgx.all$metatx_id_2<-substring(mgx.all$metagx_id_str,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgx.all<-mgx.all%>%relocate(metatx_id_2, .before=1)

#Create subset mtx files

#mgx.pk<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.all$metagx_id)
#mgx.ps<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.ps$metagx_id)
#mgx.cs<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.cs$metagx_id)

#There is one observation that was missing from the mgx data provided by Dr. Staley, I believe it is for PID 4036?
mgx.pk.cs.sub<-mgx.all%>%filter(metatx_id_2%in%pk.bgus.reads.data.cs.sub$metatx_id_2)
pk.bgus.reads.data.cs.sub<-pk.bgus.reads.data.cs.sub%>%
    filter(metatx_id_2%in%mgx.pk.cs.sub$metatx_id_2)

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

write.table(mgx.pk.cs.sub, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk_cs_sub.txt", sep = "\t", row.names = FALSE)
write.table(pk.bgus.reads.data.cs.sub, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_cs_sub.txt", sep = "\t", row.names = FALSE)

```

###Prepping the MISSION mgx (from MTX) data for the MAASLIN analysis

```{r}

getwd()
setwd("~/Desktop")
library(tidyverse)
#install.packages("vegan")
library(vegan)
#if (!require("BiocManager", quietly = TRUE))
#    install.packages("BiocManager")

#BiocManager::install("Maaslin2")
library(Maaslin2)
?Maaslin2

#Start with "/Volumes/RAVPOWER2/Temp_DEC2023_Tac grant reviews/Z_TAC_merged_abundance_table_kneaddata_species.xlsx" for your df_input_data
#Start with "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep.xlsx" for your df_input_metadata

#In the Transposed "tab", create 3 empty columns and copy the sample-id, study_arm and PK londigutinal columns from the Z_missiondemoshotgun_prep.xlsx spreadsheet. Use study_arm to sort & remove observations that are cross-sectional. Then use PKlongitudinal to sort and remove observations that are not longitudinal samples from PK participants. Save the resulting data set as a _masslin2 tsv file. Apply the same filtering to the Z_missiondemoshotgun_prep.xlsx spreadsheet and save it as well (Save as tab delimited and then manually rename the extensionto .tsv)

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_TAC_merged_abundance_table_kneaddata_species_masslin2.tsv', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_CS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_PS_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#ASN 2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MOSIO/D12_merged_abundance_table_kneaddata_species_prep_APR2024_V2_maaslin2.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

#df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt', header = TRUE, sep = "\t", row.names = 1)
#df_input_data_mission[1:5, 1:5]

df_input_data_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk_cs_sub.txt', header = TRUE, sep = "\t", row.names = 1)
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

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/Z_missiondemoshotgun_prep_masslin2.tsv', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Checking the EHR_class_40perc variable
#https://bookdown.org/rwnahhas/IntroToR/summarizing-categorical-data.html

#table(mydat$income, exclude = NULL)
#table(df_input_metadata_mission$EHR_class_40perc, exclude = NULL)

#print (df_input_metadata_mission$EHR_class_40perc)

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#turns out that the R import shifted two variables, so I will use a modified version for the maaslinmodel

#write.csv(df_input_metadata_mission, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/EHR_class_check.csv")

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_CS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_PS_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

### I copied the data under the "all data combined tab" from Moataz's dataset to the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V2.txt" to create the "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt" which has the demographic covariates I may want in the model.

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V3.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

# In order to facilitate the model converging, I computed the residuals for all the covariates regressed on the cohort variable; This was done in "/home/u63327094/analysis_MISSION_06_16S_Shotgun_TAC_analysis/Program_TAC_16S_shotgun_analysis.sas" to generate the dataset found at "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt"

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V4_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran cohort as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V5_residuals.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Same as above but ran MPA EHR as the output so I would only need 1 residual for the full model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V7_nopc.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

##Weihua suggested that we use pca instead of residuals, so I pca comp on the relevant covariates requested by Pam and Ajay to include the first two PCs into the model

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#ASN2024 poster prep
#/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V8_democlinicalpca.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgxmapping_pk_all.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Fix the error by saving the file as an excel file and then back to text.
df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_cs_sub.txt', 
                                       header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#Switching to the mosio analysis here

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E04_shotgunlistalphapsmosio_V2_M1.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#df_input_metadata_mission = read.table(file = '/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/Analysis_shotgun_microbiome_MOSIO/E05_shotgunlistalphapsmosio_V2_M1_mergeprep_dietpconly.txt', header = TRUE, sep = "\t", fill = TRUE, row.names = 1)

#recode the tertile variable so it is the lowest tertile vs everything else (smart to do so for each arm) https://stackoverflow.com/questions/62574146/how-to-create-tertile-in-r

# Find tertiles
#vTert = quantile(DF$Score, c(0:3/3))
#vTert = quantile(df_input_metadata_mission$MPA_AUC5_12_0_12, c(0:3/3))

# classify values
#DF$tert = with(DF, 
#               cut(Score, 
#                   vTert, 
#                   include.lowest = T, 
#                   labels = c("Low", "Medium", "High")))

#df_input_metadata_mission$EHRtertv2 = with(df_input_metadata_mission, 
#                                           cut(MPA_AUC5_12_0_12, 
#                                               vTert, 
#                                               include.lowest = T, 
#                                               labels = c("Low", "Medium", "High")))

#recoding the variable for a binary analysis based on the tertile
#https://www.sfu.ca/~mjbrydon/tutorials/BAinR/recode.html

#If Bank$Gender equals “Male” then the ifelse function returns a value of 1. Otherwise it returns a value of zero. In this way, a textual binary variable can be recoded as a numeric binary variable.

#Bank$Gender.Dummy<-ifelse(Bank$Gender=="Male",1,0)
#df_input_metadata_mission$EHRtertv2binary<-ifelse(df_input_metadata_mission$EHRtertv2=="Low",1,0)
df_input_metadata_mission$EHRpeakbinary<-ifelse(df_input_metadata_mission$MPA_EHR_Peaks_count==0,"LOW","HIGH")

#Calculating the ratio of MPA AUC over MPAG AUC during the EHR window
#df_input_metadata_mission$MPA_MPAG_ratio<-df_input_metadata_mission$MPA_AUC_5_12/df_input_metadata_mission$MPAG_AUC_5_12

#Calculating the ratio of MPAG AUC over MPA AUC during the EHR window
#df_input_metadata_mission$MPAG_MPA_ratio<-df_input_metadata_mission$MPAG_AUC_0_12_dose_adj/df_input_metadata_mission$MPA_AUC_0_12_dose_adj
```

###Run MGX models: Base your options on (https://docs.cosmosid.com/docs/running-maaslin2)

``` {r}

getwd()
setwd("~/Desktop")


fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_0_12_cs_sub", 
                            fixed_effects  = c("MPA_AUC_0_12"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_AUC_5_12_cs_sub", 
                            fixed_effects  = c("MPA_AUC_5_12"))  

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_cs_sub", 
                            fixed_effects  = c("MPA_AUC5_12_0_12"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak_cs_sub", 
                            fixed_effects  = c("MPA_EHR_Peaks_count"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_MPA_EHR_Peak_Binary_cs_sub", 
                            fixed_effects  = c("EHRpeakbinary"))

fit_data_mission = Maaslin2(input_data     = df_input_data_mission, 
                            input_metadata = df_input_metadata_mission, 
                            min_prevalence = 0.1,
                            min_abundance = 0.01,
                            normalization  = "none",
                            output         = "demo_output_GLUCUROCAT_PWY_3_cs_sub", 
                            fixed_effects  = c("GLUCUROCAT_PWY_3"))
```


#DESeq2 

###DESEQ2 Tutorial part 1 (implement other steps in https://www.yanh.org/2021/01/01/microbiome-r/)


```{r}

#https://bioconductor.org/packages/devel/bioc/vignettes/phyloseq/inst/doc/phyloseq-mixture-models.html#import-data-with-phyloseq-convert-to-deseq2
#https://microbiome.github.io/tutorials/deseq2.html


#Load example data
#https://www.bioconductor.org/packages/release/bioc/html/microbiome.html
#if (!require("BiocManager", quietly = TRUE))
#    install.packages("BiocManager")

#BiocManager::install("microbiome")

library(microbiome)
library(ggplot2)

#install.packages("magrittr")
library(magrittr)
library(dplyr)

# Probiotics intervention example data 
data(dietswap) #I will use a different tutorial since this one uses a premade phyloseq object

#https://www.yanh.org/2021/01/01/microbiome-r/

#Install and load relevant packages: phyloseq, tidyverse, vegan, DESeq2 , ANCOMBC, ComplexHeatmap. The following code are solely written in R. Now you can open the Rstudio and repeat it.

p1 <- c("tidyverse", "vegan", "BiocManager")
p2 <- c("phyloseq", "ANCOMBC", "DESeq2", "ComplexHeatmap")
load_package <- function(p) {
  if (!requireNamespace(p, quietly = TRUE)) {
    ifelse(p %in% p1, 
           install.packages(p, repos = "http://cran.us.r-project.org/"), 
           BiocManager::install(p))
  }
  library(p, character.only = TRUE, quietly = TRUE)
}
invisible(lapply(c(p1,p2), load_package))

```

###Shotgun Data prep from MGX data

```{r}

#Information on how to import the data in phyloseq
#https://joey711.github.io/phyloseq/import-data.html

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.species.xlsx",sheet = "Sheet1")

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.species.xlsx", sheet = "Sheet1")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

# Restore the object
mgxspecies <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies.rds")

#Now transpose the data (unlist the column names and the row names to save them as vectors)

gene_list<-unlist((mgxspecies[,1]))
sample_list<-unlist((mgxspecies[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mgx.all<-data.frame(t(mgxspecies))

#Trying to get rid of the colnames that snuck in the data as the first row
mgx.all <- mgx.all %>% filter(!str_detect(1, "clade_name")) #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
str(mgx.all)
names(mgx.all)
rownames(mgx.all)<-colnames(mgxspecies) 
mgx.all<-mgx.all%>%filter(!grepl("s__Blautia_wexlerae",X1)) #option 2 with !grepl(this version worked)

colnames(mgx.all)<-gene_list
names(mgx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

mgx.all$metagx_id_str <- rownames(mgx.all)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

#mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)
mgx.all$metatx_id_2<-substring(mgx.all$metagx_id_str,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgx.all<-mgx.all%>%relocate(metatx_id_2, .before=1)

#Create subset mtx files

#mgx.pk<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.all$metagx_id)
#mgx.ps<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.ps$metagx_id)
#mgx.cs<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.cs$metagx_id)

#There is one observation that was missing from the mgx data provided by Dr. Staley, I believe it is for PID 4036?
mgx.pk<-mgx.all%>%filter(metatx_id_2%in%pk.bgus.reads.data.4$metatx_id_2)
pk.bgus.reads.data.4<-pk.bgus.reads.data.4%>%filter(metatx_id_2%in%mgx.pk$metatx_id_2)

#Shotgun Data prep from MGX data

#The relative abundance data for all shotgun observations
#/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_07_01_Documents_UMGC_06_03312023_Shotgun_Metatranscriptomics_sequencing/10_toshare_Shotgun/Metaphlan_Full/merged_abundance_table_kneaddata_species.txt

#The total read count data to convert back the relative abundance data back to absolute count

#/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_07_01_Documents_UMGC_07_Analysis_shotgun_microbiome_MPAEHR/D10_missiondemoshotgunpscsV3_PK_maaslin2_V10_allreadcounts.xlsx

#In excel, prep the data from the text file for an id_match variable just like you did in "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_05_00_Shotgun_list_HUMANN_sorting_collection_date_time_V10_readsprep.xlsx"

### In brief, port the file to excel, transpose the sample IDs to a new tab, and use the LEFT command in excel to isolate a textstring for the sample name

### You need to manually change the following IDs so that they match the data in the mapping file: 
###BL32096_S1.r1_kneaddata to  FD32096
###F32002-4_S2.r1_kneaddata to FD32002
###F32003-4_S3.r1_kneaddata to FD32003
###FD450030-4_S166.r1_kneaddata	FD45003	to FR45030

###After you get the values only new PID in a separate column, use CTRL + H to change it back to "FR" instead of "FD" so it matches the meta_tx_id_2 variable in the pk.bgus.reads.data dataset
###Copy that new variable back as the column name in the excel tab you wish to import into R. Also modify the species name using the TEXTAFTER command in excel to isolate the species name from the "s__" delimiter

###the prepped dataset is saved in /Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_08_merged_abundance_table_kneaddata_species_prep.xlsx

#creating PK only, PK_PS and PK_CS metatranscriptomics files.

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/01_DEC2024_Metatranscriptomics data dump/metatranscriptomics.2024/mission.metatx.species.xlsx",sheet = "Sheet1")

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mission.metatx.species.xlsx", sheet = "Sheet1")

#mgxspecies<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_08_merged_abundance_table_kneaddata_species_prep.xlsx", sheet = "Sheet2")

#Having issues loading the file while knitting in markdown, so I will save it as an R object
#https://www.sthda.com/english/wiki/saving-data-into-r-data-format-rds-and-rdata

#saveRDS(mgxspecies, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies_shotgun.rds")

# Restore the object
mgxspecies <- readRDS(file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_07_mgxspecies_shotgun.rds")

#Now transpose the data (unlist the column names and the row names to save them as vectors)

gene_list<-unlist((mgxspecies[,1]))
sample_list<-unlist((mgxspecies[1,]))

#Gene data only
#I want the mtx data to be in columns
#Update the column names and row names

mgx.all<-data.frame(t(mgxspecies))

#Trying to get rid of the colnames that snuck in the data as the first row
#mgx.all <- mgx.all %>% filter(!str_detect(1, "clade_name")) #option 1 with str_detect, didn't work

#update the row names first so it matches the vector lenght above before filtering
str(mgx.all)
names(mgx.all)
rownames(mgx.all)<-colnames(mgxspecies) 
mgx.all<-mgx.all%>%filter(!grepl("Blautia_wexlerae",X1)) #option 2 with !grepl(this version worked)

colnames(mgx.all)<-gene_list
names(mgx.all)

#Now assign the rows as PIDs, need to use the same variable name as in the mtxmapping.pk file

mgx.all$metagx_id_str <- rownames(mgx.all)

#Creating a id_substring to match the ids from the sequencing files for mapping
#https://www.digitalocean.com/community/tutorials/substring-function-in-r

#mgx.all$metagx_id<-substring(mgx.all$metagx_id_str,1,7)
mgx.all$metatx_id_2<-substring(mgx.all$metagx_id_str,1,7)

#Relocate the metatx_id column to the first column so I can use it as the merging ID for MAASLIN2
#https://dplyr.tidyverse.org/reference/relocate.html#:~:text=Use%20relocate()%20to%20change,blocks%20of%20columns%20at%20once.

mgx.all<-mgx.all%>%relocate(metatx_id_2, .before=1)

#Merging the dataset to the total read count 

mgx.read.count<-read_xlsx("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_08_D10_missiondemoshotgunpscsV3_PK_maaslin2_V10_allreadcounts.xlsx",sheet = 'comb_reads')

mgx.read.count.2 <- mgx.read.count %>%
  select(read_counts, clade_name)

mgx.read.count.2$metatx_id_2<-mgx.read.count.2$clade_name

mgx.all.2<-mgx.all%>%filter(metatx_id_2%in%mgx.read.count.2$metatx_id_2)
mgx.read.count.3<-mgx.read.count.2%>%filter(metatx_id_2%in%mgx.all$metatx_id_2)

mgx.all.3<-merge(mgx.all.2,mgx.read.count.3,by="metatx_id_2") 

mgx.all.4 <- mgx.all.3 %>%
  select(-metagx_id_str, -clade_name)

#Multiplying all the relative abundance values by the total read count value
#https://stackoverflow.com/questions/28123098/multiply-many-columns-by-a-specific-other-column-in-r-with-data-table

mgx.all.5 <- mgx.all.4 %>%
  select(-metatx_id_2)

# Convert columns to numeric using apply()
mgx.all.6 <- as.data.frame(apply(mgx.all.5, 2, as.numeric))

#mutate_each() is now deprecated in favor of mutate_at(). The code could now be written as DT %>% mutate_at(vars(starts_with("inc")), ~.*deflator).

scale2 <- function(x, na.rm=FALSE) (x * mgx.all.6$read_counts)

mgx.all.test <- mgx.all.6 %>% mutate_at(c("Blautia_wexlerae"), scale2)
mgx.all.test2 <- mgx.all.6 %>% mutate_if(is.numeric, scale2)

mgx.all.7 <- mgx.all.test2 %>%
  select(-read_counts)

class(mgx.all.7$Blautia_wexlerae)

mgx.all.8 <- round(mgx.all.7, digits = 0)

#Add back the ID variable
mgx.all.8$metatx_id_2<-mgx.all.4$metatx_id_2

mgx.all.8<-mgx.all.8%>%relocate(metatx_id_2, .before=1)

#Create subset mtx files

#mgx.pk<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.all$metagx_id)
#mgx.ps<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.ps$metagx_id)
#mgx.cs<-mgx.all%>%filter(metagx_id%in%mgxmapping.pk.cs$metagx_id)

#Set working directory
setwd("/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/")

# read data from an Excel file or Workbook object into a data.frame
pk.bgus.reads.data.4 <- read.xlsx('/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_pk_bgus_reads_data.xlsx')

#There is one observation that was missing from the mgx data provided by Dr. Staley, I believe it is for PID 4036?
mgx.pk<-mgx.all.8%>%filter(metatx_id_2%in%pk.bgus.reads.data.4$metatx_id_2)
pk.bgus.reads.data.4<-pk.bgus.reads.data.4%>%filter(metatx_id_2%in%mgx.pk$metatx_id_2)

#mtx.pk<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.all$metatx_id)
#mtx.ps<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.ps$metatx_id)
#mtx.cs<-mtx.all%>%filter(metatx_id%in%mtxmapping.pk.cs$metatx_id)

#Now export these files so they can be imported back as txt files
#https://www.sthda.com/english/wiki/writing-data-from-r-to-txt-csv-files-r-base-functions

#write.table(mgx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mgx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mgx_all.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.ps, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_ps.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.cs, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_cs.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.pk, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_pk.txt", sep = "\t", row.names = FALSE)

#write.table(mtx.all, file = "/Volumes/RAVPOWER2/Temp_Analysis_DEC2024_WTC_Metatranscriptomics_Analysis/02_DEC2024 metatranscriptomics mapping file/mtx_all.txt", sep = "\t", row.names = FALSE)

write.table(mgx.pk, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgx_pk_shotgun.txt", sep = "\t", row.names = FALSE)
write.table(pk.bgus.reads.data.4, file = "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_08_01_Documents_UTMB_04_MTX_JUL_2025/15_UTMB_MTX_analysis/B_99_mgxmapping_pk_all.txt", sep = "\t", row.names = FALSE)

```

###DESeq tutorial part 2

```{r}

#Now back to Information on how to import the data in phyloseq
#https://joey711.github.io/phyloseq/import-data.html
#I have a OTU table, need to flip it back 

rownames(mgx.pk) <-mgx.pk$metatx_id_2
mgx.pk.2 <- mgx.pk %>%
  select(-metatx_id_2)
mgx.pk.3<-data.frame(t(mgx.pk.2))

#convert the whole dataframe to numeric
#https://www.geeksforgeeks.org/r-language/how-to-convert-entire-dataframe-to-numeric-while-preserving-decimals-in-r/#
  
# Convert columns to numeric using type.convert()
mgx.pk.4<- as.data.frame(lapply(mgx.pk.3, type.convert, as.is = TRUE))
# Convert columns to numeric using apply()
mgx.pk.4 <- as.data.frame(apply(mgx.pk.3, 2, as.numeric))
rownames(mgx.pk.4) <-rownames(mgx.pk.3) 

str(mgx.pk.4)

otumat <-matrix(mgx.pk.4)

str(otumat)

#Now we need a pretend taxonomy table

taxmat = matrix(sample(letters, 70, replace = TRUE), nrow = nrow(otumat), ncol = 7)
rownames(taxmat) <- rownames(otumat)
colnames(taxmat) <- c("Domain", "Phylum", "Class", "Order", "Family", "Genus", "Species")
taxmat

class(otumat)

class(taxmat)

#Checking how I did it for asmic oral to prep the OTU Matrix

#seqtab2 <- readRDS("/Users/guillaumeonyeaghala/Documents/Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/sequences_oral.rds")

seqtab2 <- readRDS("/Volumes/RAVPOWER2/ROOK_03_Professional/WBOB/ASMIC Study/18_Dissertation/3_Dissertation Analysis/1_Dissertation Processed Datasets/1_asmic_oral_mapping/sequences_oral.rds")

#The solution was to set any NAs in my data to 0
#https://stackoverflow.com/questions/64553944/deseq2-na-values-are-not-allowed-error-when-anyis-nacounts-false
mgx.pk.4[is.na(mgx.pk.4)] <- 0

#Note how these are just vanilla R matrices. Now let’s tell phyloseq how to combine them into a phyloseq object. In the previous lines, we didn’t even need to have phyloseq loaded yet. Now we do.

library("phyloseq")
#OTU = otu_table(otumat, taxa_are_rows = TRUE)
OTU = otu_table(mgx.pk.4, taxa_are_rows = TRUE)
TAX = tax_table(taxmat)
OTU

#coercing the sample dataset 

rownames(pk.bgus.reads.data.4) <-pk.bgus.reads.data.4$metatx_id_2
pk.bgus.reads.data.5 <- pk.bgus.reads.data.4 %>%
  select(-metatx_id_2)

pk.bgus.reads.data.6 <- pk.bgus.reads.data.5 #%>%
  #select(GLUCUROCAT_PWY_3)

str(pk.bgus.reads.data.6)

#sample.data <-sample_data(pk.bgus.reads.data.5)
#sample.data <-sample_data(pk.bgus.reads.data.6)
sample.data <-sample_data(pk.bgus.reads.data.5)

#Making a phyloseq object
physeq = phyloseq(OTU, sample.data)
physeq

head(otu_table(physeq))
str(sample_data(physeq))

```

###Associations in full cohort

```{r}

#Go to https://bioconductor.org/packages/devel/bioc/vignettes/phyloseq/inst/doc/phyloseq-mixture-models.html#import-data-with-phyloseq-convert-to-deseq2

#DESeq2 conversion and call
#First load DESeq2.

library("DESeq2"); packageVersion("DESeq2")

#make sure to remove any missing data

physeq <- subset_samples(physeq, GLUCUROCAT_PWY_3 != 0)
#physeq <- prune_samples(sample_sums(physeq) > 0.01, physeq)
physeq

diagdds = phyloseq_to_deseq2(physeq, ~ GLUCUROCAT_PWY_3)

# calculate geometric means prior to estimate size factors
gm_mean = function(x, na.rm=TRUE){
  exp(sum(log(x[x > 0]), na.rm=na.rm) / length(x))
}
geoMeans = apply(counts(diagdds), 1, gm_mean)
diagdds = estimateSizeFactors(diagdds, geoMeans = geoMeans)
diagdds = DESeq(diagdds, fitType="local")

#Investigate teh results table
res = results(diagdds)
res = res[order(res$padj, na.last=NA), ]
#alpha = 0.01
#sigtab = res[(res$padj < alpha), ]
#head(sigtab)

head(res)

```

###Alpha & Beta diversity phyloseq
```{r}

#https://www.yanh.org/2021/01/01/microbiome-r/
#Need the count data

#Alpha diversity metrics assess the species diversity within the ecosystems, telling you how diverse a sequenced community is

plot_richness(physeq, x="cohort", measures=c("Observed", "Shannon")) +
  geom_boxplot() +
  theme_classic() +
  theme(strip.background = element_blank(), axis.text.x.bottom = element_text(angle = -90))

#We could see tongue samples had the lowest alpha diversity. Next, some statistics: pairwise test with Wilcoxon rank-sum test, corrected by FDR method. For the number of observed tags,

rich = estimate_richness(physeq, measures = c("Observed", "Shannon"))
wilcox.observed <- pairwise.wilcox.test(rich$Observed, 
                                        sample_data(physeq)$cohort, 
                                        p.adjust.method = "BH")

tab.observed <- wilcox.observed$p.value %>%
  as.data.frame() #%>%
  #tibble::rownames_to_column(var = "group1") %>%
  #gather(key="group2", value="p.adj", -group1) %>%
  #na.omit()
tab.observed

#Similarly for Shannon Diversity

rich = estimate_richness(physeq, measures = c("Observed", "Shannon"))
wilcox.observed <- pairwise.wilcox.test(rich$Shannon, 
                                        sample_data(physeq)$cohort, 
                                        p.adjust.method = "BH")

tab.observed <- wilcox.observed$p.value %>%
  as.data.frame() #%>%
  #tibble::rownames_to_column(var = "group1") %>%
  #gather(key="group2", value="p.adj", -group1) %>%
  #na.omit()
tab.observed

#Beta diversity
#Beta diversity metrics assess the dissimilarity between ecosystem, telling you to what extend one community is different from another. Here are some demo code using Bray Curtis and binary Jaccard distance metrics. I personally do not recommend directly using the UniFrac metrics supported by phyloseq, especially when your tree is not dichotomous. This will happen when some children branches lose their parents in rarefaction.😭 You need to re-root your tree in e.g. QIIME 2 if you run into this case, see here. Besides, the UniFrac distance calculation by phyloseq has not been checked with the source code, see the posts in phyloseq issues and QIIME 2 forum. So it’s safer to use QIIME 2 version given that both QIIME and UniFrac concepts came from the Knight lab.

#distance is in overlapped use, so we specify the package by phyloseq::distance

dist = phyloseq::distance(physeq, method="bray")
ordination = ordinate(physeq, method="PCoA", distance=dist)
plot_ordination(physeq, ordination, color="cohort") + 
  theme_classic() +
  theme(strip.background = element_blank())

#PERMANOVA/ADONIS

metadata <- data.frame(sample_data(physeq))
test.adonis <- adonis2(dist ~ cohort, data = metadata)
test.adonis <- as.data.frame(test.adonis)
test.adonis

```

### AncomBC phyloseq (https://www.bioconductor.org/packages/devel/bioc/vignettes/ANCOMBC/inst/doc/ANCOMBC.html)

```{r}

#ANCOM-BC
#Here I will introduce another statistical method specially designed for Microbiome data. I like its theory behind - sequencing is a process of sampling. For a microbial community under adequate sequencing, the ratio of two specific taxa should be theoretically the “same” when calculated using either the profiled relative abundances or the absolute abundance. The theory can be found in their published paper. ANCOM is one recommended method by QIIME 2 and similar theory is also adopted by another QIIME 2 plugin- Geneiss. ANCOM is originally written in R and hosted in NIH, but the link has expired now. The q2-compositon plugin is one accessible version to my knowledge. In that QIIME has no official R API, I recommend using improved ANCOM version e.g. ANCOM 2 or ANCOM-BC. These two have not been included in QIIME 2 and maintained by the lab members of the original ANCOM paper. The key differences between ANCOM 2 and ANCOM are how they treat zeros and the introduced process of repeated data, and more can be found in the paper. Considering the structure of code, ANCOM 2 is more like a wrapped R function of ANCOM, especially relative to ANCOM-BC. ANCOM-BC inherited the methodology of pre-processing zeros in ANCOM but estimated the sampling bias by linear regression. Besides, it provides p values and according to the author, it is blazingly fast while controlling the FDR and maintaining adequate power. ANCOM-BC is maintained at bioconductor, and a vignette is here. It’s worth noting that ANCOM-BC is in development and now (Jan 2021) only support testing for co-variates and global test.

#ANCOM-BC is adopted here to repeat the test above. Here it takes gut label as a reference to other body site groups.

# ancombc
out <- ancombc(phyloseq = physeq, formula = "cohort", 
              p_adj_method = "holm",
              group = "cohort", struc_zero = TRUE, neg_lb = TRUE, tol = 1e-5, 
              max_iter = 100, conserve = TRUE, alpha = 0.05, global = TRUE)
res <- out$res

```

#Association between PK parameters and the relative abundance of BGUS transcripts 

###Full MISSION cohort

###Relative Abundance of BGUS transcripts accross PK groups defined by number of EHR Peaks

```{r}

#Full cohort, association with number of MPA EHR peaks observed

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))


summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

```

###PK parameters

```{r}


#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_0_12 + cohort*MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))


##MPA AUC 5 to 12

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL  + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC_5_12 + cohort*MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))



##MPA_EHR

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + cohort*MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))


#MPAG AUC 0 to 12 hours

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.3))


summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))



#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12 + cohort*MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))



#MPAG AUC 5 to 12 hours

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.3))


summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ MPAG_AUC_0_12 + cohort*MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))


#Acyl MPAG AUC 0 to 12 hours

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.3))


summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12 + cohort*MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))


#Acyl MPAG AUC 5 to 12 hours

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.3))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.3))


summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.3$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=pk.bgus.reads.data.3, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Testing for interaction

summary(lm(pk.bgus.reads.data.3$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12 + cohort*MPA_EHR_Peaks_count, data=pk.bgus.reads.data.3))

```

### PK parameters with adjustments

```{r}


#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.5$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Testing for interaction

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_0_12 + cohort*MPA_EHR_Peaks_count, data=pk.bgus.reads.data.5))


##MPA AUC 5 to 12

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.5$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$log_BGUS_reads_adjusted ~ MPA_AUC_5_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL  + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interaction

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC_5_12 + cohort*MPA_EHR_Peaks_count, data=pk.bgus.reads.data.5))



##MPA_EHR

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.5$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antiviral_PK + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(pk.bgus.reads.data.5$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + cohort + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=pk.bgus.reads.data.5, subset = MPA_EHR_Peaks_count %in% c("0","2")))

#Testing for interaction

summary(lm(pk.bgus.reads.data.5$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + cohort*MPA_EHR_Peaks_count, data=pk.bgus.reads.data.5))

```

###Prospective Cohort 

```{r}

#Stratified analyses, PS

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps))



summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA AUC 5 to 12

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps))



summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#MPAG AUC 0 to 12 hours

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.ps))


summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#MPAG AUC 5 to 12 hours

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.ps))


summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Acyl MPAG AUC 0 to 12 hours

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.ps))


summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Acyl MPAG AUC 5 to 12 hours

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.ps))


summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Association between BGUS reads and BGUS pathways 

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=bgus.data.ps))
summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

cor.test(bgus.data.ps$BGUS_reads_adjusted,bgus.data.ps$GLUCUROCAT_PWY_3)

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=bgus.data.ps))
summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

```

###Prospective Cohort adjusted
```{r}

##MPA AUC 0 to 12

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA AUC 5 to 12

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.ps$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.ps, subset = MPA_EHR_Peaks_count %in% c("0","2")))

```

###Cross-Sectional Cohort (not including the adjusted models since those covariates differed between the PS and CS cohorts)

```{r}
#Stratified analyses, CS

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs))



summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_EHR_Peaks_count, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Including Rory's suggested analysis

##MPA AUC 0 to 12

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.cs))


summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))


##MPA AUC 5 to 12

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs))


summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))


##MPA_EHR

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs))


summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))



#MPAG AUC 0 to 12 hours

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.cs))


summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPAG_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#MPAG AUC 5 to 12 hours

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.cs))


summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPAG_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Acyl MPAG AUC 0 to 12 hours

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.cs))


summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ AcMPAG_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Acyl MPAG AUC 5 to 12 hours

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.cs))


summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ AcMPAG_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))


#Association between BGUS reads and BGUS pathways 

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=bgus.data.cs))
summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ GLUCUROCAT_PWY_3, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

cor.test(bgus.data.cs$BGUS_reads_adjusted,bgus.data.ps$GLUCUROCAT_PWY_3)

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=bgus.data.cs))
summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ log_GLUCUROCAT_PWY, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))
```

###Cross-sectional Cohort adjusted
```{r}

##MPA AUC 0 to 12

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA AUC 5 to 12

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC_5_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC_5_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

##MPA_EHR

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$log_BGUS_reads_adjusted ~ MPA_AUC5_12_0_12, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

summary(lm(bgus.data.cs$BGUS_reads_adjusted ~ MPA_AUC5_12_0_12 + eGFR_Epi_RF_Cr2021_1_73m2 + albumin_g_dL + antibiotics_pk + steroid_at_PK, data=bgus.data.cs, subset = MPA_EHR_Peaks_count %in% c("0","2")))

```
