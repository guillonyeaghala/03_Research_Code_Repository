*Importing the GEN03 Clinical datasets for the analysis;

*Linking a folder to stata with the dataset using the "cd" command;
*Change to the dataset directory;
cd H:\ROOK_PC\HHRI_GEN03_Analysis_Project\Datasets
cd G:\ROOK_PC\HHRI_GEN03_Analysis_Project\Datasets
cd D:\ROOK_PC\HHRI_GEN03_Analysis_Project\Datasets
cd D:\Professional Folder\HHRI\14_GEN03_CYP3A5 and TAC Dosing\Datasets
cd D:\ROOK_PC\HHRI_MISSION_2021_OLD\HHRI_GEN03_Analysis_Project\Datasets

*importing a csv spreadsheet/dataset into stata using the "insheet" command
*Trying to import as a csv because the import excel command did not import the variable names correctly
insheet using "GEN03 Clinical Data Excel_V2.csv", clear

describe

generate subject_id=.
replace subject_id = SUBJECT_ID
sort subject_id
save GEN03_KEVIN_DEMO, replace

*importing the AR gen03 dataset and merging it to the demographic data (changed the format to rgsn for GEN03)
*Importing Acute rejection at 12 months (from W:\dekaf data from david\unzipped from david\dbGap_datasets\Outcomes, there is an excel spreadsheet listed as Outcomes which details eeach outcome from david);

insheet using "GEN03_phenotype_AR12mo_GO.csv", clear

describe
sort subject_id

*Merging to the demo dataset;

save GEN03_KEVIN_AR, replace

use GEN03_KEVIN_DEMO
merge subject_id using GEN03_KEVIN_AR
save GEN03_KEVIN_DEMO_AR, replace

*importing the Serum creatinine gen03 dataset and merging it to the demographic data(changed the format to rgsn for GEN03);
*Importing Serum Creatinine (from W:\dekaf data from david\unzipped from david\dbGap_datasets\Outcomes, there is an excel spreadsheet listed as Outcomes which details eeach outcome from david);

insheet using "GEN03_phenotype_SCr_25_GO.csv", clear

describe
sort subject_id

*Merging to the demo dataset;

save GEN03_KEVIN_SCR, replace

use GEN03_KEVIN_DEMO_AR
sort subject_id
drop _merge
merge subject_id using GEN03_KEVIN_SCR
save GEN03_KEVIN_DEMO_AR_SCR, replace

*importing the GFR gen03 dataset and merging it to the demographic data(changed the format to rgsn for GEN03);
*Importing eGFR (from W:\dekaf data from david\unzipped from david\dbGap_datasets\Outcomes, there is an excel spreadsheet listed as Outcomes which details eeach outcome from david);

insheet using "GEN03_phenotype_GFR_25_GO.csv", clear

describe
sort subject_id

*Merging to the demo dataset;

save GEN03_KEVIN_GFR, replace

use GEN03_KEVIN_DEMO_AR_SCR
sort subject_id
drop _merge
merge subject_id using GEN03_KEVIN_GFR
save GEN03_KEVIN_DEMO_AR_SCR_GFR, replace

*Creating a suitable ID variable for the Data pulled by Kevin to merge to the clinical data;
*"https://www.extendoffice.com/documents/excel/1783-excel-remove-text-before-character.html"
*And I use the formula =CONCAT(4,1,MID(A2,4,3),RIGHT(A2,1)), with A2 being the GEN03 ID
*Do the same with the "Tacrolimus through data" and the genotype data;

*Importing the number of throughs from Kevin's dataset;
 
insheet using "Tacrolimus_PGx_Data_Collection_Full_03222021_troughs_GO.csv", clear

describe
sort subject_id

*Merging to the demo dataset;

save GEN03_KEVIN_N, replace

use GEN03_KEVIN_DEMO_AR_SCR_GFR
sort subject_id
drop _merge
merge subject_id using GEN03_KEVIN_N
save GEN03_KEVIN_DEMO_AR_SCR_GFR_N, replace

*Merging the genotype data send by david

insheet using "Tacrolimus_PGx_Data_Collection_Full_03222021_tacgeno_GO.csv", clear

describe
sort subject_id

*Checking for duplicates (https://www.stata.com/support/faqs/data-management/duplicate-observations/);

*Merging to the demo dataset;

save GEN03_KEVIN_GENO, replace

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N
sort subject_id
drop _merge
merge subject_id using GEN03_KEVIN_GENO
save GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO, replace

drop if check ==.
save GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO, replace
 
*Attempting to merge the long dataset as a setup for the mixed effect models;

insheet using "Tacrolimus_PGx_Data_Collection_Full_03222021_troughslong_GO.csv", clear

describe
sort subject_id

save GEN03_KEVIN_LONG, replace

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO
sort subject_id
drop _merge
merge subject_id using GEN03_KEVIN_LONG
save GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG, replace

*First, recode the phenotypes using the directions from Dr. Jacobson;
*"H:\Professional Folder\HHRI\HHRI GEN03 Analysis Project\Documents\Ultrapid Metabolism Project\Genotypes to Phenotypes AXELROD table"

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO, clear

describe

tabulate rs10264272_t_cyp3a5_6, missing	
tabulate rs41303343_ta_cyp3a5_7, missing 	
tabulate rs776746_g_cyp3a5_3, missing	
tabulate rs35599367_a_cyp3a4_22, missing

*Recoding rs776746_g_cyp3a5_3 to deal with the imputed values;

generate rs776746_g_cyp3a5_3_v2 = .
replace rs776746_g_cyp3a5_3_v2 = 0 if rs776746_g_cyp3a5_3 <= 1
replace rs776746_g_cyp3a5_3_v2 = 1 if rs776746_g_cyp3a5_3 <= 1.8 & rs776746_g_cyp3a5_3 >=1
replace rs776746_g_cyp3a5_3_v2 = 2 if rs776746_g_cyp3a5_3 <= 2 & rs776746_g_cyp3a5_3 >=1.8
tabulate rs776746_g_cyp3a5_3_v2 rs776746_g_cyp3a5_3, missing

/*Testing a second version of the phenotype version where I do not add up the alleles (i.e., do not assume an additive gene effect)
*Seems like a vertical bar is the substitute for an or statement (https://www.stata.com/statalist/archive/2002-06/msg00188.html)

generate pheno1v2=.
replace pheno1v2 =0 if rs776746_g_cyp3a5_3_v2==0 | rs10264272_t_cyp3a5_6==0 | rs41303343_ta_cyp3a5_7==0
replace pheno1v2 =1 if rs776746_g_cyp3a5_3_v2==1 | rs10264272_t_cyp3a5_6==1 | rs41303343_ta_cyp3a5_7==1
replace pheno1v2 =2 if rs776746_g_cyp3a5_3_v2==2 | rs10264272_t_cyp3a5_6==2 | rs41303343_ta_cyp3a5_7==2
tab pheno1v2, missing

generate pheno2v2=.
replace pheno2v2 =0 if rs776746_g_cyp3a5_3_v2==0 | rs10264272_t_cyp3a5_6==0 | rs41303343_ta_cyp3a5_7==0 | rs35599367_a_cyp3a4_22==0
replace pheno2v2 =1 if rs776746_g_cyp3a5_3_v2==1 | rs10264272_t_cyp3a5_6==1 | rs41303343_ta_cyp3a5_7==1 | rs35599367_a_cyp3a4_22==1
replace pheno2v2 =2 if rs776746_g_cyp3a5_3_v2==2 | rs10264272_t_cyp3a5_6==2 | rs41303343_ta_cyp3a5_7==2 | rs35599367_a_cyp3a4_22==2
tab pheno2v2, missing */

*We need to create 2 phenotypes (based on thee document Dr. Jacobson provided)

*Phenotype 1;
*Essentially, we are counting the number of LOF alleles for each participant;
 
generate pheno1=.
replace pheno1 = rs776746_g_cyp3a5_3_v2 + rs10264272_t_cyp3a5_6 + rs41303343_ta_cyp3a5_7
tabulate pheno1, missing

generate pheno1v2= " "
replace pheno1v2 = "RM" if pheno1==0
replace pheno1v2 = "IM" if pheno1==1
replace pheno1v2 = "PM" if pheno1==2
tab pheno1v2, missing
tab pheno1 pheno1v2, missing

*Phenotype 2;

generate pheno2=.
replace pheno2 = rs776746_g_cyp3a5_3_v2 + rs10264272_t_cyp3a5_6 + rs41303343_ta_cyp3a5_7 + rs35599367_a_cyp3a4_22
tabulate pheno2, missing

generate pheno2v2= " "
replace pheno2v2 = "RM" if pheno2==0
replace pheno2v2 = "IM" if pheno2==1
replace pheno2v2 = "PM" if pheno2==2
replace pheno2v2 = "PM" if pheno2==3
tab pheno2v2, missing
tab pheno2 pheno2v2, missing

*Creating a combined phenotype for RM and IM at dr. jacobson's request (10/12/21)
generate pheno1comb= " "
replace pheno1comb = "NP" if pheno1==0
replace pheno1comb = "NP" if pheno1==1
replace pheno1comb = "P" if pheno1==2
tab pheno1comb, missing
tab pheno1comb pheno1v2, missing

*Creating the demographic info for Kevin's presentation

*% Male – already collected in the initial analysis

encode(gender), generate(sex)
tabstat sex, by(pheno1v2) stat(n)
tabulate gender pheno1v2, missing

*Age at Tx – already collected in the initial

tabstat age_at_tx, by(pheno1v2) stat(n mean sd)

*Weight at Tx – have BMI, good enough for now I think

tabstat bmi_base, by(pheno1v2) stat(n mean sd)

*Race Category, checking based on Pam's comment 01/05/2022

tabulate racecatc pheno1v2, missing

*Cause of kidney disease – already collected in the initial analysis

tabulate pcause pheno1v2, missing

*Deceased Donor Kidney  - need, percent for each phenotype should be adequate (Tab 1, Column N numbers 2, 3, and 4 are all deceased donor kidneys)

tabulate donorstatus pheno1v2, missing

*Previous Transplant  - need, percent for each phenotype should be adequate (Tab 1, Column M number 1 indicates first transplant so would include any number > 1 – only needs to be a yes/no don’t need to know if this is second or third transplant)

tabulate transplantnumber pheno1v2, missing

*Weight-Based Tacrolimus Total Daily Dose – already collected in the initial analysis

tabstat weightbasedtactotaldailydose, by(pheno1v2) stat(n mean sd)

*Actual Initial Tacrolimus Total Daily Dose – already collected in the initial analysis

tabstat actualinitialtactotaldailydose, by(pheno1v2) stat(n mean sd)

*Trough Levels Collected – already collected in the initial analysis

tabstat troughlevelsn, by(pheno1v2) stat(n sum mean sd)
tabstat troughlevelsn, by(pheno1comb) stat(n sum mean sd)

*Troughs Collected During Months 0-3 – already collected in the initial analysis

tabstat troughs03monthsn, by(pheno1v2) stat(n mean sd)

*Troughs Collected During Months 3-6 – already collected in the initial analysis

tabstat troughs36mon, by(pheno1v2) stat(n mean sd)

*Number of Dose Adjustments – already collected in the initial analysis

tabstat doseadjustmentsn, by(pheno1v2) stat(n sum mean sd)
tabstat doseadjustmentsn, by(pheno1comb) stat(n sum mean sd)

*sCr at 1 Mo/3 Mo/6 Mo – already collected in the initial analysis

tabstat scr1mo scr3mo scr6mo, by(pheno1v2) stat(n mean sd)

*runing some models (use genmod to specify the family);
*https://www.statalist.org/forums/forum/general-stata-discussion/general/428376-replicating-proc-genmod-in-stata;

*Family options (https://www.stata.com/manuals/semgsemfamily-and-linkoptions.pdf)
*Specifying base levels in models (https://www.stata.com/manuals13/u11.pdf#u11.4.3.2Baselevels)

*Had to also include actual tac daily dose for the revisions, and only got significant results after adding previous kidney transplant history.


encode(gender), generate(sex)
encode(pheno1v2), generate(cat3pheno)
encode(pheno1comb), generate(cat2pheno)
encode(donorstatus), generate(catdonorstatus)
encode (racecatc), generate(racecatmodel)
encode (diab_base), generate(diabmodel)
encode (pcausenew), generate(pcausemodel)

glm troughlevelsn pheno1v2, family(poisson) scale(dev) irls
glm troughlevelsn pheno1v2, family(poisson) eform 
glm troughlevelsn ib2.pheno1v2, family(poisson) eform 
glm troughlevelsn pheno1v2 sex age_at_tx bmi_base, family(poisson) eform
glm troughlevelsn ib2.pheno1v2 sex age_at_tx bmi_base, family(poisson) eform
glm troughlevelsn ib0.cat2pheno sex age_at_tx bmi_base, family(poisson) eform
glm troughlevelsn ib0.cat2pheno sex age_at_tx bmi_base actualinitialtactotaldailydose , family(poisson) eform
glm troughlevelsn ib0.cat3pheno sex age_at_tx bmi_base actualinitialtactotaldailydose , family(poisson) eform
glm troughlevelsn ib0.cat2pheno sex age_at_tx bmi_base weightbasedtactotaldailydose, family(poisson) eform
glm troughlevelsn ib0.cat2pheno sex age_at_tx bmi_base priorktx actualinitialtactotaldailydose, family(poisson) eform

glm troughlevelsn cat3pheno, family(poisson) scale(dev) irls
tabstat troughlevelsn, by(pheno1comb) stat(n mean sd semean)

glm doseadjustmentsn pheno1v2, family(poisson) scale(dev) irls
glm doseadjustmentsn pheno1v2, family(poisson) eform
glm doseadjustmentsn ib2.pheno1v2, family(poisson) eform
glm doseadjustmentsn pheno1v2 sex age_at_tx bmi_base, family(poisson) eform
glm doseadjustmentsn ib2.pheno1v2 sex age_at_tx bmi_base, family(poisson) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base, family(poisson) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base actualinitialtactotaldailydose, family(poisson) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base actualinitialtactotaldailydose catdonorstatus, family(poisson) eform
glm doseadjustmentsn ib0.cat3pheno sex age_at_tx bmi_base actualinitialtactotaldailydose, family(poisson) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base actualinitialtactotaldailydose, family(gaussian) link(log) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base actualinitialtactotaldailydose, family(gaussian) link(identity) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base actualinitialtactotaldailydose, family(gaussian) link(identity) 
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base weightbasedtactotaldailydose, family(poisson) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx racecatmodel diabmodel bmi_base pcausemodel actualinitialtactotaldailydose, family(poisson) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base racecatmodel priorktx actualinitialtactotaldailydose, family(poisson) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base priorktx actualinitialtactotaldailydose, family(poisson) eform

tab cat2pheno, missing
tabstat doseadjustmentsn, by(pheno1comb) stat(n mean sd semean)

save GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO, replace

*Dr. Israni wanted to stratify the models by living vs deceased donors.

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO, clear

describe

tabulate catdonorstatus donorstatus, missing
*Deceased donors
drop if catdonorstatus != 1 

tabulate catdonorstatus donorstatus, missing

encode(gender), generate(sex)
encode(pheno1v2), generate(cat3pheno)
encode(pheno1comb), generate(cat2pheno)
encode(donorstatus), generate(catdonorstatus)
encode (racecatc), generate(racecatmodel)
encode (diab_base), generate(diabmodel)
encode (pcausenew), generate(pcausemodel)

glm troughlevelsn ib0.cat2pheno sex age_at_tx bmi_base priorktx actualinitialtactotaldailydose, family(poisson) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base priorktx actualinitialtactotaldailydose, family(poisson) eform


use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO, clear

describe

tabulate catdonorstatus donorstatus, missing
*Living Donors
drop if catdonorstatus != 2

tabulate catdonorstatus donorstatus, missing

encode(gender), generate(sex)
encode(pheno1v2), generate(cat3pheno)
encode(pheno1comb), generate(cat2pheno)
encode(donorstatus), generate(catdonorstatus)
encode (racecatc), generate(racecatmodel)
encode (diab_base), generate(diabmodel)
encode (pcausenew), generate(pcausemodel)

glm troughlevelsn ib0.cat2pheno sex age_at_tx bmi_base priorktx actualinitialtactotaldailydose, family(poisson) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base priorktx actualinitialtactotaldailydose, family(poisson) eform

*Dr. Israni wanted to stratify the models by living vs deceased donors.





/*Long dataset

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG, clear

sort SUBJECT_ID

describe

tabulate rs10264272_t_cyp3a5_6, missing	
tabulate rs41303343_ta_cyp3a5_7, missing 	
tabulate rs776746_g_cyp3a5_3, missing	
tabulate rs35599367_a_cyp3a4_22, missing

*Recoding rs776746_g_cyp3a5_3 to deal with the imputed values;

generate rs776746_g_cyp3a5_3_v2 = .
replace rs776746_g_cyp3a5_3_v2 = 0 if rs776746_g_cyp3a5_3 <= 1
replace rs776746_g_cyp3a5_3_v2 = 1 if rs776746_g_cyp3a5_3 <= 1.8 & rs776746_g_cyp3a5_3 >=1
replace rs776746_g_cyp3a5_3_v2 = 2 if rs776746_g_cyp3a5_3 <= 2 & rs776746_g_cyp3a5_3 >=1.8
tabulate rs776746_g_cyp3a5_3_v2 rs776746_g_cyp3a5_3, missing

*We need to create 2 phenotypes (based on thee document Dr. Jacobson provided)

*Phenotype 1;
*Essentially, we are counting the number of LOF alleles for each participant;
 
generate pheno1=.
replace pheno1 = rs776746_g_cyp3a5_3_v2 + rs10264272_t_cyp3a5_6 + rs41303343_ta_cyp3a5_7
tabulate pheno1, missing

generate pheno1v2= " "
replace pheno1v2 = "RM" if pheno1==0
replace pheno1v2 = "IM" if pheno1==1
replace pheno1v2 = "PM" if pheno1==2
tab pheno1v2, missing
tab pheno1 pheno1v2, missing

*Tacrolimus Level – median and range for level between phenotypes (should just be for eyeballing, actual interpretation of levels would need more context)
*https://www.fsb.miamioh.edu/lij14/311_stata_bar.pdf

encode(tacrolimuslevel), generate(taclevel)
graph hbar (mean)taclevel, over(troughnumber, label(labsize(small))) over(pheno1v2) asyvars ytitle("test test") legend(rows(1) stack size(small))
graph hbar (mean)taclevel, over(pheno1v2, label(labsize(small))) over(troughnumber) asyvars ytitle("test test") legend(rows(1) stack size(small))
graph bar (mean)taclevel, over(pheno1v2, label(labsize(small))) over(troughnumber) asyvars ytitle("test test") legend(rows(1) stack size(small))

*In Goal – median and range for percent of troughs within goal range between phenotypes
*Percent from Goal –absolute value median and range percent from goal between phenotypes (exclude if in goal)
*Dose Adjustment – this was already collected in the initial analysis so should be congruent with what is in Tab 1, not sure we need to run it again?

*/

*I needed to update the tac level and the percent from goal variables so they would be imported as numeric variables

*Attempting to merge the long dataset as a setup for the mixed effect models;

insheet using "Tacrolimus_PGx_Data_Collection_Full_03222021_troughslong_GO2.csv", clear

describe
sort subject_id

save GEN03_KEVIN_LONG_2, replace

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO
sort subject_id
drop _merge
merge subject_id using GEN03_KEVIN_LONG_2
save GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_2, replace

*Long dataset

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_2, clear

sort SUBJECT_ID

describe

tabulate rs10264272_t_cyp3a5_6, missing	
tabulate rs41303343_ta_cyp3a5_7, missing 	
tabulate rs776746_g_cyp3a5_3, missing	
tabulate rs35599367_a_cyp3a4_22, missing

*Recoding rs776746_g_cyp3a5_3 to deal with the imputed values;

generate rs776746_g_cyp3a5_3_v2 = .
replace rs776746_g_cyp3a5_3_v2 = 0 if rs776746_g_cyp3a5_3 <= 1
replace rs776746_g_cyp3a5_3_v2 = 1 if rs776746_g_cyp3a5_3 <= 1.8 & rs776746_g_cyp3a5_3 >=1
replace rs776746_g_cyp3a5_3_v2 = 2 if rs776746_g_cyp3a5_3 <= 2 & rs776746_g_cyp3a5_3 >=1.8
tabulate rs776746_g_cyp3a5_3_v2 rs776746_g_cyp3a5_3, missing

*We need to create 2 phenotypes (based on thee document Dr. Jacobson provided)

*Phenotype 1;
*Essentially, we are counting the number of LOF alleles for each participant;
 
generate pheno1=.
replace pheno1 = rs776746_g_cyp3a5_3_v2 + rs10264272_t_cyp3a5_6 + rs41303343_ta_cyp3a5_7
tabulate pheno1, missing

generate pheno1v2= " "
replace pheno1v2 = "RM" if pheno1==0
replace pheno1v2 = "IM" if pheno1==1
replace pheno1v2 = "PM" if pheno1==2
tab pheno1v2, missing
tab pheno1 pheno1v2, missing

*Creating a combined phenotype for RM and IM at dr. jacobson's request (10/12/21)
generate pheno1comb= " "
replace pheno1comb = "NP" if pheno1==0
replace pheno1comb = "NP" if pheno1==1
replace pheno1comb = "P" if pheno1==2
tab pheno1comb, missing
tab pheno1comb pheno1v2, missing

*Dr. Israni requested we exclude all observations  for throughs not within 11-13 hours;

tabulate troughtimewithin1113hours, missing

drop if troughtimewithin1113hours != 1

tabulate troughtimewithin1113hours, missing

save GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_calc

*One reviewer wanted descriptive statistics for the first 10 days post transplant

drop if postopday > 10

tabulate pheno1v2, summarize(tacrolimuslevel)
tabulate pheno1v2, summarize(tacrolimustotaldailydose)
table pheno1v2, c(median tacrolimuslevel min tacrolimuslevel max tacrolimuslevel)
table pheno1v2, c(median tacrolimustotaldailydose min tacrolimustotaldailydose max tacrolimustotaldailydose)

*Redoing the calculations for each month :(

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_calc, clear

drop if postopday > 10

tabulate pheno1v2, summarize(tacrolimuslevel)
tabulate pheno1v2, summarize(tacrolimustotaldailydose)
table pheno1v2, c(median tacrolimuslevel min tacrolimuslevel max tacrolimuslevel)
table pheno1v2, c(median tacrolimustotaldailydose min tacrolimustotaldailydose max tacrolimustotaldailydose)
ttest tacrolimuslevel, by(pheno1v2)
anova tacrolimuslevel pheno1
anova tacrolimustotaldailydose pheno1

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_calc, clear

drop if postopday > 30

tabulate pheno1v2, summarize(tacrolimuslevel)
tabulate pheno1v2, summarize(tacrolimustotaldailydose)
table pheno1v2, c(median tacrolimuslevel min tacrolimuslevel max tacrolimuslevel)
table pheno1v2, c(median tacrolimustotaldailydose min tacrolimustotaldailydose max tacrolimustotaldailydose)
ttest tacrolimuslevel, by(pheno1v2)
anova tacrolimuslevel pheno1
anova tacrolimustotaldailydose pheno1

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_calc, clear

drop if postopday > 60

tabulate pheno1v2, summarize(tacrolimuslevel)
tabulate pheno1v2, summarize(tacrolimustotaldailydose)
table pheno1v2, c(median tacrolimuslevel min tacrolimuslevel max tacrolimuslevel)
table pheno1v2, c(median tacrolimustotaldailydose min tacrolimustotaldailydose max tacrolimustotaldailydose)
ttest tacrolimuslevel, by(pheno1v2)
anova tacrolimuslevel pheno1
anova tacrolimustotaldailydose pheno1

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_calc, clear

drop if postopday > 90

tabulate pheno1v2, summarize(tacrolimuslevel)
tabulate pheno1v2, summarize(tacrolimustotaldailydose)
table pheno1v2, c(median tacrolimuslevel min tacrolimuslevel max tacrolimuslevel)
table pheno1v2, c(median tacrolimustotaldailydose min tacrolimustotaldailydose max tacrolimustotaldailydose)
ttest tacrolimuslevel, by(pheno1v2)
anova tacrolimuslevel pheno1
anova tacrolimustotaldailydose pheno1

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_calc, clear

drop if postopday > 120

tabulate pheno1v2, summarize(tacrolimuslevel)
tabulate pheno1v2, summarize(tacrolimustotaldailydose)
table pheno1v2, c(median tacrolimuslevel min tacrolimuslevel max tacrolimuslevel)
table pheno1v2, c(median tacrolimustotaldailydose min tacrolimustotaldailydose max tacrolimustotaldailydose)
ttest tacrolimuslevel, by(pheno1v2)
anova tacrolimuslevel pheno1
anova tacrolimustotaldailydose pheno1

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_calc, clear

drop if postopday > 150

tabulate pheno1v2, summarize(tacrolimuslevel)
tabulate pheno1v2, summarize(tacrolimustotaldailydose)
table pheno1v2, c(median tacrolimuslevel min tacrolimuslevel max tacrolimuslevel)
table pheno1v2, c(median tacrolimustotaldailydose min tacrolimustotaldailydose max tacrolimustotaldailydose)
ttest tacrolimuslevel, by(pheno1v2)
anova tacrolimuslevel pheno1
anova tacrolimustotaldailydose pheno1

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_calc, clear

drop if postopday > 180

tabulate pheno1v2, summarize(tacrolimuslevel)
tabulate pheno1v2, summarize(tacrolimustotaldailydose)
table pheno1v2, c(median tacrolimuslevel min tacrolimuslevel max tacrolimuslevel)
table pheno1v2, c(median tacrolimustotaldailydose min tacrolimustotaldailydose max tacrolimustotaldailydose)
ttest tacrolimuslevel, by(pheno1v2)
anova tacrolimuslevel pheno1
anova tacrolimustotaldailydose pheno1

*More than 2  groups, use Anova over ttest for a continuous variable (chisquare is for categorical variables)

ttest tacrolimuslevel, by(pheno1v2)
anova tacrolimuslevel pheno1
anova tacrolimustotaldailydose pheno1

*Calculating the number of dose adjustments in the dataset per Dr. Jacobson's comment 01/05/2022

tabulate doseadjustment, missing

*Tacrolimus Level – median and range for level between phenotypes (should just be for eyeballing, actual interpretation of levels would need more context)
*https://www.fsb.miamioh.edu/lij14/311_stata_bar.pdf

graph bar (mean)tacrolimuslevel, over(pheno1v2, label(labsize(small))) over(troughnumber) asyvars ytitle("test test") legend(rows(1) stack size(small)) 
graph bar (mean)tacrolimuslevel, over(pheno1v2, label(labsize(small))) over(troughnumber) asyvars ytitle("test test") legend(rows(1) stack size(small)) blabel(bar)

*Generate month to create a somewhat cleaner graph
********Will need to generate 15 day intervals as well;

generate month =.
replace month = int(postopday/30.5)
generate day_15 =.
replace day_15 = int(postopday/15)

tabulate month
graph bar (mean)tacrolimuslevel, over(pheno1v2, label(labsize(small))) over(month) asyvars ytitle("test test") legend(rows(1) stack size(small)) 
graph hbar (mean)tacrolimuslevel, over(pheno1v2, label(labsize(small))) over(month) asyvars ytitle("Average TAC levels per phenotype in the first 6 months post transplant") legend(rows(1) stack size(small)) blabel(bar)

tabstat tacrolimuslevel, by(pheno1v2) by(troughlevel) stat(median range)

*Use tabulate summarize instead
*https://www.stata.com/manuals/rtabulatesummarize.pdf#rtabulate,summarize()
*Tacrolimus levels is equivalent to the tacrolimus level for troughs 
*Dr Jacobson has requested  that we include the tacrolimus troughs and tacrolimus doses as supplemental data

tabulate pheno1v2 troughnumber, summarize(tacrolimuslevel)
tabulate pheno1v2 month, summarize(tacrolimuslevel)
tabulate pheno1v2 month, summarize(tacrolimustotaldailydose)

*Dr jacobson also wanted pvalues for the supplementat data (https://stats.idre.ucla.edu/stata/output/t-test/)
*More than 2  groups, use Anova over ttest for a continuous variable (chisquare is for categorical variables)

ttest tacrolimuslevel, by(pheno1v2)
anova tacrolimuslevel pheno1
anova tacrolimustotaldailydose pheno1

*https://www.stata.com/manuals/rtable.pdf#rtable

table pheno1v2 month, c(median tacrolimuslevel min tacrolimuslevel max tacrolimuslevel)

*In Goal – median and range for percent of troughs within goal range between phenotypes
drop if ingoal=="In Goal"
generate ingoal2=.
replace ingoal2=0 if ingoal==0
replace ingoal2=1 if ingoal==1
table pheno1v2 month, c(mean ingoal2)

*Percent from Goal –absolute value median and range percent from goal between phenotypes (exclude if in goal)
drop if ingoal==1
generate absolutegoal=.
replace absolutegoal=abs(percentgoal2)
table pheno1v2 month, c(median absolutegoal min absolutegoal max absolutegoal)

*Dose Adjustment – this was already collected in the initial analysis so should be congruent with what is in Tab 1, not sure we need to run it again?

*Next step: use proc mixed to calculate the mean levels for these values accounting for clustering, similar to LSmeans in SAS? (Look at mixed, margins and collapse)

save GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_2_FIG, replace

*Long dataset

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_2_FIG, clear

collapse (mean) tacrolimuslevel, by(day_15 pheno1v2) /* Collapse is used to create aggregate variables, and you can aggregate on more than one variable */

export excel using "C:\Users\Guillaume Onyeaghala\Desktop\Kevin_TAC_Figure_Mean_V2.xlsx", firstrow(variables)

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_2_FIG, clear

collapse (sd) tacrolimuslevel, by(day_15 pheno1v2) /* Collapse is used to create aggregate variables, and you can aggregate on more than one variable */

export excel using "C:\Users\Guillaume Onyeaghala\Desktop\Kevin_TAC_Figure_SD.xlsx", firstrow(variables)

*Generating a histogram for percent change in dose

generate percentchange_n = real(percentchange)

graph bar (mean)percentchange_n, over(pheno1v2, label(labsize(small))) over(day_15) asyvars ytitle("Percent change in TAC Dose") legend(rows(1) stack size(small)) 

tabulate pheno1v2 month, summarize(percentchange_n)

*Generating a histogram for percent change in dose but the absolute value

generate percentchange_abs = abs(percentchange_n)

graph bar (mean)percentchange_abs, over(pheno1v2, label(labsize(small))) over(day_15) asyvars ytitle("Absolute Percent change in TAC Dose") legend(rows(1) stack size(small)) 

tabulate pheno1v2 month, summarize(percentchange_abs)

*Generating data for the time to first theurapeutic dose;
*https://www.stata.com/support/faqs/data-management/first-and-last-occurrences/;

by SUBJECT_ID(postopday), sort: egen firsttime = min(cond(ingoal ==1, postopday, .))
by SUBJECT_ID(postopday), sort: egen lasttime = max(cond(ingoal ==1, postopday, .))

table pheno1v2, c(median firsttime min firsttime max firsttime)

*Since we threw out the observations where the data was not collected in that window, some of the calculations may warrant a second look later. 

list SUBJECT_ID pheno1v2 ingoal postopday firsttime if pheno1v2 =="RM"

graph bar (mean)firsttime, over(pheno1v2, label(labsize(small))) asyvars ytitle("Visualizing time to firt dose") legend(rows(1) stack size(small)) 
graph bar firsttime, over(pheno1v2, label(labsize(small))) asyvars ytitle("Visualizing time to firt dose") legend(rows(1) stack size(small)) 
histogram firsttime, normal by(pheno1v2) ytitle("Visualizing time to first dose")
histogram postopday, normal by(pheno1v2) ytitle("Visualizing distribution of the follow up time")

*Trying to generate a value to determine the time in theurapeutic range

by SUBJECT_ID(postopday), sort: gen byte track = ingoal == 0 & ingoal[_n - 1] == 1 
by SUBJECT_ID(postopday), sort: gen byte track2 = ingoal == 1 & ingoal[_n - 1] == 0 
tabulate SUBJECT_ID, summarize (track)
tabstat track, by(SUBJECT_ID) stat(n mean sd)

by SUBJECT_ID(postopday), sort: egen endtherapeutic = min(cond(track == 1, postopday, .))

generate timeinrange = .
replace timeinrange= endtherapeutic - firsttime
table pheno1v2, c(median timeinrange min timeinrange max timeinrange)

*Making a duplicate postopday variable to make things easier to visualize in the dataset
generate postopday2 = .
replace postopday2 = postopday

*After getting feedback from Dr. Jacobson, we need to sum all the intervals where the person fell out of range
*My strategy is calculate the time interval between each observation, then sum up these intervals wherever the participant is in goal range;
by SUBJECT_ID(postopday), sort: gen byte interval = postopday - postopday[_n - 1]
by SUBJECT_ID(postopday), sort: egen testtherapeuticrange = sum(cond(ingoal == 1, interval, .))
by SUBJECT_ID(postopday), sort: egen testtherapeuticrangev2 = sum(cond(ingoal == 1, interval[_n + 1], .))

*Attempting to apply the rosendaal method to the ttr calculation (https://www.inrpro.com/rosendaal.asp)
by SUBJECT_ID(postopday), sort: gen rosendaaltotalshift = tacrolimuslevel - tacrolimuslevel[_n - 1]

generate rosendaalwithinrange = .
replace rosendaalwithinrange = rosendaaltotalshift if tacrolimuslevel <= lowerboundtacrolimusgoal | tacrolimuslevel >= upperboundtacrolimusgoal
replace rosendaalwithinrange = upperboundtacrolimusgoal-tacrolimuslevel[_n - 1]  if track == 1 & tacrolimuslevel >= upperboundtacrolimusgoal
replace rosendaalwithinrange = tacrolimuslevel[_n - 1]-lowerboundtacrolimusgoal  if track == 1 & tacrolimuslevel <= lowerboundtacrolimusgoal
replace rosendaalwithinrange = rosendaaltotalshift if track==0

generate rosendaalratio = .
replace rosendaalratio = abs(rosendaalwithinrange/rosendaaltotalshift)
list SUBJECT_ID tacrolimuslevel lowerboundtacrolimusgoal upperboundtacrolimusgoal rosendaaltotalshift rosendaalwithinrange if rosendaalratio ==.

generate rosendaalinterval = .
replace rosendaalinterval = interval*rosendaalratio if track==1
replace rosendaalinterval = interval*rosendaalratio if track2==1
replace rosendaalinterval = interval if rosendaalratio >= 1
replace rosendaalinterval = interval if rosendaalratio == .
by SUBJECT_ID(postopday), sort: egen testtherapeuticrangerosendaal = sum(cond(ingoal == 1, rosendaalinterval[_n + 1], .))

*I had to edit the code for the ratio because some observations had a rosendaal ratio greater than 1 and should be manually checked to adjust the values)
list postopday tacrolimuslevel lowerboundtacrolimusgoal upperboundtacrolimusgoal rosendaalratio if rosendaalratio > 1
table pheno1v2, c(median testtherapeuticrangerosendaal min testtherapeuticrangerosendaal max testtherapeuticrangerosendaal)

table pheno1v2 if month ==0, c(median testtherapeuticrangerosendaal min testtherapeuticrangerosendaal max testtherapeuticrangerosendaal) 
table pheno1v2 if month ==1, c(median testtherapeuticrangerosendaal min testtherapeuticrangerosendaal max testtherapeuticrangerosendaal) 
table pheno1v2 if month ==2, c(median testtherapeuticrangerosendaal min testtherapeuticrangerosendaal max testtherapeuticrangerosendaal) 
table pheno1v2 if month <=2, c(median testtherapeuticrangerosendaal min testtherapeuticrangerosendaal max testtherapeuticrangerosendaal) 

generate rosendaalinterval2 = .
replace rosendaalinterval2 = interval*rosendaalratio if rosendaalratio < 1
replace rosendaalinterval2 = interval if rosendaalratio >= 1
by SUBJECT_ID(postopday), sort: egen testtherapeuticrangerosendaal2 = sum(cond(ingoal == 1, rosendaalinterval2[_n + 1], .))

table pheno1v2, c(median testtherapeuticrangerosendaal2 min testtherapeuticrangerosendaal2 max testtherapeuticrangerosendaal2)

by SUBJECT_ID(postopday), sort: egen testtherapeuticrangerosendaal2m1 = sum(cond(ingoal == 1 & month==0, rosendaalinterval2[_n + 1], .))
table pheno1v2 if month ==0, c(median testtherapeuticrangerosendaal2m1 min testtherapeuticrangerosendaal2m1 max testtherapeuticrangerosendaal2m1) 
table pheno1v2 if month ==0, c(mean testtherapeuticrangerosendaal2m1) 

*Creating the correct value for TTR based on Kevin's feedback

by SUBJECT_ID(postopday), sort: egen totaldays = sum(interval)

generate ttrpercent = .
replace ttrpercent = testtherapeuticrangerosendaal/totaldays
table pheno1v2, c(median ttrpercent min ttrpercent max ttrpercent)

by SUBJECT_ID(postopday), sort: egen totaldaysm1 = sum(cond(month == 0, interval, .))

generate ttrpercentm1 = .
replace ttrpercentm1 = testtherapeuticrangerosendaal2m1/totaldaysm1
table pheno1v2, c(median ttrpercentm1 min ttrpercentm1 max ttrpercentm1)

*Calculating the tacrolimus dose adusted values

generate tacdoseadjusted = .
replace tacdoseadjusted = tacrolimuslevel/tacrolimustotaldailydose

table pheno1v2, c(mean ttrpercent min ttrpercent max ttrpercent)
tabstat tacdoseadjusted, by(pheno1v2) stat(n mean sd)


*Calculating the steroid tapervalue (will do if necessary at a later date as Kevin did not provide the prednisone values for the low and unique steroid tapers)

save GEN03_KEVIN_longitudinal_mixed, replace

*Testing mixed effect regression setup based on PUBH8342 (lectures 8-12) & PUBH6363 class notes\\

use GEN03_KEVIN_longitudinal_mixed, clear

generate  initialweightbasedtd = .
replace initialweightbasedtd = actualinitialtactotaldailydose / weightkg


generate  idealweightbasedtd = .
replace idealweightbasedtd = actualinitialtactotaldailydose / ibw

table pheno1v2, c(median initialweightbasedtd min initialweightbasedtd max initialweightbasedtd)
table pheno1v2, c(median idealweightbasedtd min initialweightbasedtd max initialweightbasedtd)

*From lecture 9: Covariance Pattern Modeling

/*
Clustering variable (SUBJECT_ID)
timevariable (postopday, use to create troughnumber and month)
Longitudinal time outcomes = tacrolimuslevel
Longitudinal time outcomes = percentfromgoal2 (numeric equivalent to the percentfromgoal generated by Kevin)
Longitudinal time outcomes = percentchange

*/

mixed tacrolimuslevel ib0.pheno1 c.postopday || SUBJECT_ID:, noconstant residuals(independent, t(postopday))
mixed tacrolimuslevel ib0.pheno1 c.postopday || SUBJECT_ID:, noconstant residuals(exchangeable, t(postopday))
mixed tacrolimuslevel ib0.pheno1 c.postopday || SUBJECT_ID:, noconstant residuals(ar 1, t(postopday))
mixed tacrolimuslevel ib0.pheno1 c.postopday || SUBJECT_ID:, noconstant residuals(toeplitz, t(postopday))

mixed tacdoseadjusted ib0.pheno1 c.postopday || SUBJECT_ID:, noconstant residuals(independent, t(postopday))
mixed tacdoseadjusted ib0.pheno1 c.postopday || SUBJECT_ID:, noconstant residuals(exchangeable, t(postopday))
mixed tacdoseadjusted ib0.pheno1 c.postopday || SUBJECT_ID:, noconstant residuals(ar 1, t(postopday))
mixed tacdoseadjusted ib0.pheno1 c.postopday || SUBJECT_ID:, noconstant residuals(toeplitz, t(postopday))

*Adding interactions
mixed tacrolimuslevel ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(independent, t(postopday))
mixed tacrolimuslevel ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(exchangeable, t(postopday))
mixed tacrolimuslevel ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(ar 1, t(postopday))
mixed tacrolimuslevel ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(toeplitz, t(postopday))

mixed tacdoseadjusted ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(independent, t(postopday))
mixed tacdoseadjusted ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(exchangeable, t(postopday))
mixed tacdoseadjusted ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(ar 1, t(postopday))
mixed tacdoseadjusted ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(toeplitz, t(postopday))

*Adding margins (need to use a categorical variable)
mixed tacrolimuslevel ib0.pheno1##ib0.month || SUBJECT_ID:, noconstant residuals(independent, t(postopday))
margins ib0.pheno1##ib0.month
marginsplot, xdimension(month) plotdimension(pheno1)

mixed tacdoseadjusted ib0.pheno1##ib0.month || SUBJECT_ID:, noconstant residuals(independent, t(postopday))
margins ib0.pheno1##ib0.month
marginsplot, xdimension(month) plotdimension(pheno1)

mixed tacrolimuslevel ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(exchangeable, t(postopday))
mixed tacrolimuslevel ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(ar 1, t(postopday))
mixed tacrolimuslevel ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(toeplitz, t(postopday))

mixed tacdoseadjusted ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(exchangeable, t(postopday))
mixed tacdoseadjusted ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(ar 1, t(postopday))
mixed tacdoseadjusted ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(toeplitz, t(postopday))

*Calculate margins separately, and get P-Values
mixed tacrolimuslevel ib0.pheno1##ib0.month || SUBJECT_ID:, noconstant residuals(independent, t(postopday))
margins pheno1, over(month)
margins r.pheno1, over(month)
marginsplot, xdimension(month) plotdimension(pheno1)

mixed tacdoseadjusted ib0.pheno1##ib0.month || SUBJECT_ID:, noconstant residuals(independent, t(postopday))
margins pheno1, over(month)
margins r.pheno1, over(month)
marginsplot, xdimension(month) plotdimension(pheno1)

mixed tacrolimuslevel ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(exchangeable, t(postopday))
mixed tacrolimuslevel ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(ar 1, t(postopday))
mixed tacrolimuslevel ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(toeplitz, t(postopday))

mixed tacdoseadjusted ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(exchangeable, t(postopday))
mixed tacdoseadjusted ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(ar 1, t(postopday))
mixed tacdoseadjusted ib0.pheno1##c.postopday || SUBJECT_ID:, noconstant residuals(toeplitz, t(postopday))

*From lecture 11: Mixed model regression (refer to these materials for the interpretation)

*Random Intercept, No Time Effect
mixed tacrolimuslevel || SUBJECT_ID:

*Random Intercept, Fixed Time
mixed tacrolimuslevel postopday|| SUBJECT_ID:

*Random Intercept, Random Linear Time
mixed tacrolimuslevel postopday|| SUBJECT_ID: postopday
mixed tacrolimuslevel postopday|| SUBJECT_ID: postopday, cov(independent)
mixed tacrolimuslevel postopday|| SUBJECT_ID: postopday, cov(exchangeable)
mixed tacrolimuslevel postopday|| SUBJECT_ID: postopday, cov(unstr)

*Adding predictors

encode(gender), generate(sex)
encode(donorstatus), generate(donorstat)
encode(steroiddose), generate(steroid)

mixed tacrolimuslevel postopday ib0.pheno1|| SUBJECT_ID: postopday
mixed tacrolimuslevel postopday ib0.pheno1|| SUBJECT_ID: postopday, cov(independent)
mixed tacrolimuslevel postopday ib0.pheno1|| SUBJECT_ID: postopday, cov(exchangeable)
mixed tacrolimuslevel postopday ib0.pheno1|| SUBJECT_ID: postopday, cov(unstr)

mixed tacdoseadjusted postopday ib2.pheno1|| SUBJECT_ID: postopday
mixed tacdoseadjusted postopday ib2.pheno1|| SUBJECT_ID: postopday, cov(independent)
mixed tacdoseadjusted postopday ib2.pheno1|| SUBJECT_ID: postopday, cov(exchangeable)
mixed tacdoseadjusted postopday ib2.pheno1|| SUBJECT_ID: postopday, cov(unstr)

mixed tacdoseadjusted postopday ib2.pheno1 sex age_at_tx bmi_base steroid || SUBJECT_ID: postopday
mixed tacdoseadjusted postopday ib2.pheno1 sex age_at_tx bmi_base steroid || SUBJECT_ID: postopday, cov(independent)
mixed tacdoseadjusted postopday ib2.pheno1 sex age_at_tx bmi_base steroid || SUBJECT_ID: postopday, cov(exchangeable)
mixed tacdoseadjusted postopday ib2.pheno1 sex age_at_tx bmi_base steroid || SUBJECT_ID: postopday, cov(unstr)

*Checking interactions

mixed tacrolimuslevel ib0.month ib0.pheno1 ib0.pheno1##ib0.month|| SUBJECT_ID: postopday
mixed tacrolimuslevel ib0.month ib0.pheno1 ib0.pheno1##ib0.month|| SUBJECT_ID: postopday, cov(independent)
mixed tacrolimuslevel ib0.month ib0.pheno1 ib0.pheno1##ib0.month|| SUBJECT_ID: postopday, cov(exchangeable)
mixed tacrolimuslevel ib0.month  ib0.pheno1 ib0.pheno1##ib0.month|| SUBJECT_ID: postopday, cov(unstr)

mixed tacdoseadjusted ib0.month ib2.pheno1 ib2.pheno1##ib0.month|| SUBJECT_ID: postopday
mixed tacdoseadjusted ib0.month ib2.pheno1 ib2.pheno1##ib0.month|| SUBJECT_ID: postopday, cov(independent)
mixed tacdoseadjusted ib0.month ib2.pheno1 ib2.pheno1##ib0.month|| SUBJECT_ID: postopday, cov(exchangeable)
mixed tacdoseadjusted ib0.month  ib2.pheno1 ib2.pheno1##ib0.month|| SUBJECT_ID: postopday, cov(unstr)

mixed tacdoseadjusted ib0.month ib2.pheno1 ib2.pheno1##ib0.month sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday
mixed tacdoseadjusted ib0.month ib2.pheno1 ib2.pheno1##ib0.month sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday, cov(independent)
mixed tacdoseadjusted ib0.month ib2.pheno1 ib2.pheno1##ib0.month sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday, cov(exchangeable)
mixed tacdoseadjusted ib0.month ib2.pheno1 ib2.pheno1##ib0.month sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday, cov(unstr)

*Creating a combined phenotype for RM and IM at dr. jacobson's request (10/12/21)
*Adding baseline TAC level to the secondary outcomes based on reviewer feedback (07/05/2022)
*The model only remained significant when I added previous kidney transplant (as a variable) to the model

generate pheno1comb= " "
replace pheno1comb = "NP" if pheno1==0
replace pheno1comb = "NP" if pheno1==1
replace pheno1comb = "P" if pheno1==2
tab pheno1comb, missing
tab pheno1comb pheno1v2, missing

encode(gender), generate(sex)
encode(pheno1v2), generate(cat3pheno)
encode(pheno1comb), generate(cat2pheno)

mixed tacdoseadjusted ib0.month ib2.pheno1 ib2.pheno1##ib0.month sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday
mixed tacdoseadjusted ib0.month ib2.pheno1 ib2.pheno1##ib0.month sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday, cov(independent)
mixed tacdoseadjusted ib0.month ib2.pheno1 ib2.pheno1##ib0.month sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday, cov(exchangeable)
mixed tacdoseadjusted ib0.month ib2.pheno1 ib2.pheno1##ib0.month sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday, cov(unstr)

mixed tacdoseadjusted ib0.month ib0.cat2pheno ib0.cat2pheno##ib0.month sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday, cov(independent)
mixed tacdoseadjusted ib0.month ib0.cat2pheno ib0.cat2pheno##ib0.month sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday, cov(exchangeable)
mixed tacdoseadjusted ib0.month ib0.cat2pheno ib0.cat2pheno##ib0.month sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday, cov(unstr)

mixed tacdoseadjusted ib0.month ib0.cat2pheno ib0.cat2pheno##ib0.month actualinitialtactotaldailydose priorktx sex age_at_tx bmi_base steroid|| SUBJECT_ID: postopday, cov(independent)

tabstat tacdoseadjusted, by(pheno1comb) stat(n mean sd semean)


*Calculate margins

margins pheno1, over(month)

*Plot margins

marginsplot, xdimension(month) plotdimension(pheno1)

*Next steps: After you make sure the interpretation of effect estimates is correct, learn how to interpret the random effect parameters, and set the same analysis for the other longitudinal outcomes 
*Crude models then adjusted models

*Outcome = percent of tac troughs in goal (ingoal = 1), see PUBH 8342 lecture 12

*Longitudinal logistic example
meglm ingoal || SUBJECT_ID: , fam(binomial) link(logit)
estat icc

*Random intercept model with OR and covariates
encode(gender), generate(sex)
encode(donorstatus), generate(donorstat)
encode(steroiddose), generate(steroid)

meglm ingoal ib2.pheno1 ib0.month || SUBJECT_ID: , fam(binomial) link(logit)
meglm, or

meglm ingoal ib2.pheno1 ib0.month sex age_at_tx bmi_base steroid || SUBJECT_ID: , fam(binomial) link(logit)
meglm, or

meglm ingoal ib2.pheno1 ib0.month ib2.pheno1##ib0.month || SUBJECT_ID: , fam(binomial) link(logit)
meglm, or

meglm ingoal ib2.pheno1 ib0.month ib2.pheno1##ib0.month sex age_at_tx bmi_base steroid || SUBJECT_ID: , fam(binomial) link(logit)
meglm, or

*Generating the ingoal count value to merge with the collapsed dataset. 
by SUBJECT_ID(postopday), sort: egen ingoalsum = sum(cond(ingoal == 1, ingoal, .))
by SUBJECT_ID(postopday), sort: egen ingoalall = sum(troughtimewithin1113hours) 

generate ingoalratio = .
replace ingoalratio = ingoalsum/ingoalall

*Getting things ready for merging
keep SUBJECT_ID firsttime lasttime testtherapeuticrange testtherapeuticrangev2 testtherapeuticrangerosendaal testtherapeuticrangerosendaal2m1 totaldays ttrpercent totaldaysm1 ttrpercentm1 steroid ingoalsum ingoalall ingoalratio
collapse (mean) firsttime lasttime testtherapeuticrange testtherapeuticrangev2 testtherapeuticrangerosendaal testtherapeuticrangerosendaal2m1 totaldays ttrpercent totaldaysm1 ttrpercentm1 steroid ingoalsum ingoalall ingoalratio, by(SUBJECT_ID)

generate subject_id = .
replace subject_id = SUBJECT_ID
describe
sort subject_id

save GEN03_KEVIN_longitudinal_merge, replace

use GEN03_KEVIN_longitudinal_merge, clear 
sort SUBJECT_ID
save GEN03_KEVIN_longitudinal_merge, replace

*Merging to the demo dataset;

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO
sort SUBJECT_ID
drop _merge
merge SUBJECT_ID using GEN03_KEVIN_longitudinal_merge
save GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_longitudinal, replace

*Running models based on glm based on collapsed variables
use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_longitudinal, clear

*running some models (use genmod to specify the family);
*https://www.statalist.org/forums/forum/general-stata-discussion/general/428376-replicating-proc-genmod-in-stata;

*Creating a combined phenotype for RM and IM at dr. jacobson's request (10/12/21)
*Adding baseline TAC level to the secondary outcomes based on reviewer feedback (07/05/2022)
*The model only remained significant when I added previous kidney transplant (as a variable) to the model

generate pheno1comb= " "
replace pheno1comb = "NP" if pheno1==0
replace pheno1comb = "NP" if pheno1==1
replace pheno1comb = "P" if pheno1==2
tab pheno1comb, missing
tab pheno1comb pheno1v2, missing

encode(gender), generate(sex)
encode(pheno1v2), generate(cat3pheno)
encode(pheno1comb), generate(cat2pheno)
glm troughlevelsn pheno1v2, family(poisson) scale(dev) irls
glm troughlevelsn pheno1v2, family(poisson) eform 
glm troughlevelsn ib2.pheno1v2, family(poisson) eform 
glm troughlevelsn pheno1v2 sex age_at_tx bmi_base, family(poisson) eform
glm troughlevelsn ib2.pheno1v2 sex age_at_tx bmi_base, family(poisson) eform
glm troughlevelsn ib0.cat2pheno sex age_at_tx bmi_base, family(poisson) eform
glm troughlevelsn ib0.cat2pheno sex age_at_tx bmi_base actualinitialtactotaldailydose priorktx, family(poisson) eform

glm troughlevelsn cat3pheno, family(poisson) scale(dev) irls
tabstat troughlevelsn, by(pheno1comb) stat(n mean sd semean)

*Family options (https://www.stata.com/manuals/semgsemfamily-and-linkoptions.pdf)
*Specifying base levels in models (https://www.stata.com/manuals13/u11.pdf#u11.4.3.2Baselevels)

encode(gender), generate(sex)
encode(donorstatus), generate(donorstat)
encode(steroiddose), generate(steroid)
encode(pheno1comb), generate(cat2pheno)

glm troughlevelsn pheno1, family(poisson) scale(dev) irls
glm troughlevelsn pheno1, family(poisson) eform 
glm troughlevelsn ib2.pheno1, family(poisson) eform 
glm troughlevelsn pheno1 sex age_at_tx bmi_base, family(poisson) eform
glm troughlevelsn ib2.pheno1 sex age_at_tx bmi_base, family(poisson) eform
glm troughlevelsn ib0.cat2pheno sex age_at_tx bmi_base actualinitialtactotaldailydose priorktx, family(poisson) eform

glm doseadjustmentsn pheno1, family(poisson) scale(dev) irls
glm doseadjustmentsn pheno1, family(poisson) eform
glm doseadjustmentsn ib2.pheno1, family(poisson) eform
glm doseadjustmentsn pheno1 sex age_at_tx bmi_base, family(poisson) eform
glm doseadjustmentsn ib2.pheno1 sex age_at_tx bmi_base, family(poisson) eform
glm doseadjustmentsn ib0.cat2pheno sex age_at_tx bmi_base, family(poisson) eform

glm ingoalsum ib2.pheno1, family(poisson) eform
glm ingoalsum ib2.pheno1 sex age_at_tx bmi_base, family(poisson) eform
glm ingoalsum ib2.pheno1 sex age_at_tx bmi_base donorstat, family(poisson) eform
glm ingoalsum ib2.pheno1 sex age_at_tx bmi_base donorstat typeoftransplant, family(poisson) eform
glm ingoalsum ib2.pheno1 sex age_at_tx bmi_base donorstat typeoftransplant nummismatch, family(poisson) eform

glm ingoalratio ib2.pheno1
glm ingoalratio ib2.pheno1 sex age_at_tx bmi_base 
glm ingoalratio ib2.pheno1 sex age_at_tx bmi_base steroid

glm firsttime ib2.pheno1
glm firsttime ib2.pheno1 sex age_at_tx bmi_base 
glm firsttime ib2.pheno1 sex age_at_tx bmi_base steroid
glm firsttime ib0.cat2pheno sex age_at_tx bmi_base steroid
glm firsttime ib0.cat2pheno sex age_at_tx bmi_base steroid actualinitialtactotaldailydose priorktx

tabstat firsttime, by(pheno1comb) stat(n mean sd semean)

*Time in Therapeutic Range
glm ttrpercent ib2.pheno1
glm ttrpercent ib2.pheno1 sex age_at_tx bmi_base 
glm ttrpercent ib2.pheno1 sex age_at_tx bmi_base steroid
glm ttrpercent ib0.cat2pheno sex age_at_tx bmi_base steroid 
glm ttrpercent ib0.cat2pheno sex age_at_tx bmi_base steroid actualinitialtactotaldailydose priorktx

tabstat ttrpercent, by(pheno1comb) stat(n mean sd semean)

save GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_longitudinal, replace

*Importing the eGFR data for the model

insheet using "Tacrolimus PGx Data Collection ATC_GO_V3_2021.csv", clear

describe
keep subject_id gfr_calc scr1mo ecrcl1mo scr3mo ecrcl3mo scr6mo ecrcl6mo

generate SUBJECT_ID = .
replace SUBJECT_ID = subject_id
sort SUBJECT_ID
save GEN03_KEVIN_GFR, replace

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_longitudinal
sort SUBJECT_ID
drop _merge
merge SUBJECT_ID using GEN03_KEVIN_GFR
save GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_longitudinal_GFR, replace

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_longitudinal_GFR, clear

*Creating a combined phenotype for RM and IM at dr. jacobson's request (10/12/21)
*Adding baseline TAC level to the secondary outcomes based on reviewer feedback (07/05/2022)
*The model only remained significant when I added previous kidney transplant (as a variable) to the model

*eGFR at 6 months, mL/min/1.73m2

generate pheno1comb= " "
replace pheno1comb = "NP" if pheno1==0
replace pheno1comb = "NP" if pheno1==1
replace pheno1comb = "P" if pheno1==2
tab pheno1comb, missing
tab pheno1comb pheno1v2, missing

encode(gender), generate(sex)
encode(donorstatus), generate(donorstat)
encode(steroiddose), generate(steroid)
encode(pheno1comb), generate(cat2pheno)

glm gfr_calc ib2.pheno1
glm gfr_calc ib2.pheno1 sex age_at_tx bmi_base 
glm gfr_calc ib2.pheno1 sex age_at_tx bmi_base steroid

glm gfr_calc ib0.cat2pheno sex age_at_tx bmi_base 
glm gfr_calc ib0.cat2pheno sex age_at_tx bmi_base actualinitialtactotaldailydose priorktx
glm gfr_calc ib0.cat2pheno sex age_at_tx bmi_base actualinitialtactotaldailydose 

tabstat gfr_calc, by(pheno1comb) stat(n mean sd semean)

*Creating a combined phenotype for RM and IM at dr. jacobson's request (10/12/21)
*Time to first therapeutic trough in days

generate pheno1comb= " "
replace pheno1comb = "NP" if pheno1==0
replace pheno1comb = "NP" if pheno1==1
replace pheno1comb = "P" if pheno1==2
tab pheno1comb, missing
tab pheno1comb pheno1v2, missing

*set up survival data for Kevin's project to contrast with the glm model

generate ingoalcox = 1

stset firsttime, f(ingoalcox)
list

*Generate a KM survival estimate
sts list if pheno1v2=="RM"
sts list if pheno1v2=="IM"
sts list if pheno1v2=="PM"
sts list, by(pheno1v2)

sts list if pheno1v2=="NP"
sts list if pheno1v2=="P"
sts list, by(pheno1comb)

*graph the KM curves
sts graph, by(pheno1v2) risktable ci
sts graph, by(pheno1v2) risktable 

*Test equality of KM curves (you want the test to be NS, https://www.stata.com/manuals/stststest.pdf)
*https://sphweb.bumc.bu.edu/otlt/mph-modules/bs/bs704_survival/BS704_Survival5.html#:~:text=among%20independent%20groups.-,The%20Log%20Rank%20Test,identical%20(overlapping)%20or%20not
*https://people.umass.edu/biep640w/pdf/Stata%20for%20Survival%20Analysis.pdf

sts test pheno1v2
stphplot, by (pheno1v2)

*option 2: Goodness of fit which Uses residuals (If PH assumption is met, residuals should be independent of time, the p-value gives a somewhat easier interpretation than the subjective visual inspection)
stcox ib2.pheno1
estat phtest

*Run Cox Model (https://www.stata.com/features/overview/survival-example-session/)
*could use the encode function to convert the string variable:encode(gender), generate(sex)
*check lecture 14 in PUBH8342

*Creating a combined phenotype for RM and IM at dr. jacobson's request (10/12/21)
*Adding baseline TAC level to the secondary outcomes based on reviewer feedback (07/05/2022)
*The model only remained significant when I added previous kidney transplant (as a variable) to the model

encode(gender), generate(sex)
encode(donorstatus), generate(donorstat)
encode(steroiddose), generate(steroid)
encode(pheno1comb), generate(cat2pheno)

stsum, by(pheno1v2)

stcox ib2.pheno1
stcox ib2.pheno1 sex age_at_tx bmi_base
stcox ib0.cat2pheno sex age_at_tx bmi_base
stcox ib0.cat2pheno sex age_at_tx bmi_base actualinitialtactotaldailydose priorktx

tabstat firsttime, by(pheno1comb) stat(n min max median mean sd semean)

*Running some power calculations at Dr. Israni's request. First reporting the power to detect the increase in 2 more doses given our sample size.
*Second is the sample size needed to detect the same association with 80% power. 
*Found in D:\Professional Folder\WBOB\Power Calculations\5_Power and Sample Size STATA 15 on page 36

power twomeans  6.1 8.1, sd1(0.35) sd2(0.650) nratio(2.39)
power twomeans  6.1 8.1, sd1(0.35) sd2(0.650) n(78)
power twoproportions, ratio(1.28) n1(23) n2(55)
power twomeans  6.1 8.1, sd1(0.35) sd2(0.650) n1(55) n2(23)

*In order to use the relative risk power calculation, I will need to use the proportion of dose adjustments in each group

*Long dataset

use GEN03_KEVIN_DEMO_AR_SCR_GFR_N_GENO_LONG_2, clear

sort SUBJECT_ID

describe

tabulate rs10264272_t_cyp3a5_6, missing	
tabulate rs41303343_ta_cyp3a5_7, missing 	
tabulate rs776746_g_cyp3a5_3, missing	
tabulate rs35599367_a_cyp3a4_22, missing

*Recoding rs776746_g_cyp3a5_3 to deal with the imputed values;

generate rs776746_g_cyp3a5_3_v2 = .
replace rs776746_g_cyp3a5_3_v2 = 0 if rs776746_g_cyp3a5_3 <= 1
replace rs776746_g_cyp3a5_3_v2 = 1 if rs776746_g_cyp3a5_3 <= 1.8 & rs776746_g_cyp3a5_3 >=1
replace rs776746_g_cyp3a5_3_v2 = 2 if rs776746_g_cyp3a5_3 <= 2 & rs776746_g_cyp3a5_3 >=1.8
tabulate rs776746_g_cyp3a5_3_v2 rs776746_g_cyp3a5_3, missing

*We need to create 2 phenotypes (based on thee document Dr. Jacobson provided)

*Phenotype 1;
*Essentially, we are counting the number of LOF alleles for each participant;
 
generate pheno1=.
replace pheno1 = rs776746_g_cyp3a5_3_v2 + rs10264272_t_cyp3a5_6 + rs41303343_ta_cyp3a5_7
tabulate pheno1, missing

generate pheno1v2= " "
replace pheno1v2 = "RM" if pheno1==0
replace pheno1v2 = "IM" if pheno1==1
replace pheno1v2 = "PM" if pheno1==2
tab pheno1v2, missing
tab pheno1 pheno1v2, missing

*Creating a combined phenotype for RM and IM at dr. jacobson's request (10/12/21)
generate pheno1comb= " "
replace pheno1comb = "NP" if pheno1==0
replace pheno1comb = "NP" if pheno1==1
replace pheno1comb = "P" if pheno1==2
tab pheno1comb, missing
tab pheno1comb pheno1v2, missing

*Dr. Israni requested we exclude all observations  for throughs not within 11-13 hours;

tabulate troughtimewithin1113hours, missing

drop if troughtimewithin1113hours != 1

tabulate troughtimewithin1113hours, missing

*One reviewer wanted descriptive statistics for the first 10 days post transplant

drop if postopday > 10

tabulate pheno1v2, summarize(tacrolimuslevel)
tabulate pheno1v2, summarize(tacrolimustotaldailydose)
table pheno1v2, c(median tacrolimuslevel min tacrolimuslevel max tacrolimuslevel)
table pheno1v2, c(median tacrolimustotaldailydose min tacrolimustotaldailydose max tacrolimustotaldailydose)

*More than 2  groups, use Anova over ttest for a continuous variable (chisquare is for categorical variables)

ttest tacrolimuslevel, by(pheno1v2)
anova tacrolimuslevel pheno1
anova tacrolimustotaldailydose pheno1

*Calculating the number of dose adjustments in the dataset per Dr. Jacobson's comment 01/05/2022

tabulate doseadjustment pheno1comb, missing

*Back to power calculations (overall proportion is 482/1425)
*Proportion of dose adjustments in the Poor metabolizer group is 310 / 996
*Proportion of dose adjustments in the intermediate + extensive metabolizer group is 172 / 429

*Back to the power calculation book page 237

power twoproportions 0.31124, rrisk(1.28) n1(55) n2(23)
power twoproportions 0.31124, power(0.8) rrisk (1.28)

*trying to make our pcalc better. Issue is that then you have 88% power but it doesn't take into account the repeated measures
power twoproportions 0.31124, rrisk(1.28) n1(996) n2(429)
power twoproportions 0.31124, power(0.8) rrisk (1.28)
