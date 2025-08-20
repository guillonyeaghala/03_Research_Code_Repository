*You will need to upload the files to the SAS studio server directly;
*Right click on the folder location and select properties to copy the path name;

libname mylib "/home/u63327094/MPAG_Bioaccumulation_Analysis";

*This is the XLSX file;
proc import 
datafile= '/home/u63327094/MPAG_Bioaccumulation_Analysis/47_41_MAP_MISSION_16S_sample_pull_mappingtemplate_MAR2023_PKMAP_nodups_alpha_beta_MPA_bugs_V2.xlsx'
out=mylib.MPAG16S
dbms=XLSX	
replace;
getnames=yes;
run;

*Load data; 
data MPAG16S; 
set mylib.MPAG16S;
run; 

*Check variable names;
*https://chemicalstatistician.wordpress.com/2015/01/06/get-a-list-of-the-variable-names-of-a-sas-data-set/;

proc contents data=MPAG16S order=varnum;
run;

proc print data = MPAG16S;
run;

data MPAG16Slong;
set MPAG16S;
keep
Sample_id
Patient_ID
Study_arm_2
Collection_Point
Time_point
moataz_id
MPA_Cmax_dose_adj
MPA_Ctrough_dose_adj
MPA_AUC_0_12_dose_adj
MPA_EHR_5_12
MPA_AUC_0_12_dose_adj_cat
MPA_EHR_5_12_cat
AUC_sequencing
Invitro_PID
Invitro_Date
Invitro_SampleID
T6_T24_data
T6_MPAG_ug_mL
T24_MPAG_ug_mL
T6_T24_Ratio
T6_2_MPAG_Mol_L
T24_2_MPAG_Mol_L
C10X_data
C10X_0_Mins_ug_mL
C10X_30_Mins_ug_mL
C10X_60_Mins_ug_mL
C10X_90_Mins_ug_mL
C10X_120_Mins_ug_mL
Lysis_data
T6_BEAD_MPAG_Lysis
T24_BEAD_MPAG_Lysis
Unit
Myfortic
sex_2
sex
ethnicity
predom_race
residence
country_of_birth
donor_type
birth_yr_ld
sex_ld
ethnicity_ld
race_ld
birth_yr_dd
sex_dd
ethnicity_dd
race_dd
height_baseline
weight_baseline_kg
bmi_baseline
smoking
pets
pets_count
pets_type
diabetes_pretx
age_at_diabetic_pretx
tx_dt
dt_hosp_discharge_posttx
esrd_cause
esrd_cause_glom_dis
esrd_cause_glom_dis_other
esrd_other
cause_esrd_notes
esrd_biopsy_confirm
prev_tx
donor_type_prevtx
preemptive_tx
initial_dialysis
htn_baseline
lipid_baseline
acidreducer_baseline
antiviral_baseline
antifungal_baseline
antibiotics_baseline
probiotx_baseline
immunospu_baseline
label_feces_sample
label_feces_dna1
alpha_merge
shannon
beta_merge
bc_pcoa_axis1
bc_pcoa_axis2
bc_pcoa_axis3
abundance_merge
Bifidobacterium_adolescentis
Bifidobacterium_bifidum
Bifidobacterium_breve
Bifidobacterium_pseudolongum
Collinsella_aerofaciens
Bacteroides_fragilis
Bacteroides_ovatus
Bacteroides_uniformis
Clostridium_butyricum
Clostridium_paraputrificum
Clostridium_perfringens
Clostridium_clostridioforme
Marvinbryantia_formatexigens
Roseburia_inulinivorans
Ruminococcus_gnavus
Clostridium_bartlettii
Clostridium_bifermentans
Faecalibacterium_prausnitzii
Subdoligranulum_variabile
;
run;

*Creating a prefix variables for transposing;
data MPAG16Slong;
set MPAG16Slong;
If Time_point = 1 then Mission_Collection = "M1";
If Time_point = 2 then Mission_Collection = "M2";
If Time_point = 4 then Mission_Collection = "M4";
If Time_point = 6 then Mission_Collection = "M6";
run;

proc freq data = MPAG16Slong;
tables Mission_collection;
run;

proc print data = MPAG16Slong;
where Mission_collection = " ";
run;

proc print data = MPAG16Slong;
where Patient_ID  = 3504;
run;

proc print data = MPAG16Slong;
where Patient_ID  = 4006;
run;

data MPAG16Slong;
set MPAG16Slong;
if Mission_collection = " " then delete;
if Sample_id  = "FD32021-3" then delete;
if Sample_id = "FD45027-1" then delete;
run;

*Now selecting only sample with AUC and PK measurements;
proc freq data = MPAG16Slong;
Tables AUC_sequencing;
run;

data MPAG16Slong;
set MPAG16Slong;
If AUC_sequencing ne "Both" then delete;
run;

proc freq data = MPAG16Slong;
Tables MPA_AUC_0_12_dose_adj_cat MPA_EHR_5_12_cat;
run;

*Correlation with free floating stool MPA;
proc corr data = MPAG16Slong;
var 
MPA_Cmax_dose_adj 
MPA_Ctrough_dose_adj 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
T6_T24_Ratio
T6_2_MPAG_Mol_L
T24_2_MPAG_Mol_L;
where T6_T24_data = "Yes";
run;

data MPAGT6T24;
set MPAG16Slong;
where T6_T24_data = "Yes";
run;

*Creating median variables for this subanalysis;
proc rank data=MPAGT6T24 out=MPAGT6T24 groups=2;
var T6_T24_Ratio
T6_2_MPAG_Mol_L
T24_2_MPAG_Mol_L
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12;
ranks rT6_T24_Ratio
rT6_2_MPAG_Mol_L
rT24_2_MPAG_Mol_L
rMPA_AUC_0_12_dose_adj 
rMPA_EHR_5_12;
run;

proc sort data = MPAGT6T24;
by rMPA_AUC_0_12_dose_adj;
run;

proc corr data = MPAGT6T24;
var 
MPA_Cmax_dose_adj 
MPA_Ctrough_dose_adj 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
T6_T24_Ratio
T6_2_MPAG_Mol_L
T24_2_MPAG_Mol_L;
by rMPA_AUC_0_12_dose_adj;
run;

proc sort data = MPAGT6T24;
by rMPA_EHR_5_12;
run;

proc corr data = MPAGT6T24;
var 
MPA_Cmax_dose_adj 
MPA_Ctrough_dose_adj 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
T6_T24_Ratio
T6_2_MPAG_Mol_L
T24_2_MPAG_Mol_L;
by rMPA_EHR_5_12;
run;

proc sort data = MPAGT6T24;
by rT6_T24_Ratio;
run;

proc corr data = MPAGT6T24;
var 
MPA_Cmax_dose_adj 
MPA_Ctrough_dose_adj 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
T6_T24_Ratio
T6_2_MPAG_Mol_L
T24_2_MPAG_Mol_L;
by rT6_T24_Ratio;
run;

*Export the data to excel for network testing using iGraph;

proc export data=MPAGT6T24
    outfile="/home/u63327094/MPAG_Bioaccumulation_Analysis/MPAG_T6T24_bacteria.xlsx"
    dbms=xlsx
    replace;
run;

*Outputting an rtf document for the correlation in the nine 10X samples;

title1;

options orientation=portrait center number pageno=1 nodate;

ods rtf file="/home/u63327094/MPAG_Bioaccumulation_Analysis/MPAG_10X_correlation.rtf";

*Correlation with oversaturated invitro MPAG turnover;
proc corr data = MPAG16Slong;
var 
MPA_Cmax_dose_adj 
MPA_Ctrough_dose_adj 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
C10X_0_Mins_ug_mL
C10X_30_Mins_ug_mL
C10X_60_Mins_ug_mL
C10X_90_Mins_ug_mL
C10X_120_Mins_ug_mL;
where C10X_data = "Yes";
run;

data MPAG10X;
set MPAG16Slong;
where C10X_data = "Yes";
run;

*Creating median variables for this subanalysis;
proc rank data=MPAG10X out=MPAG10X groups=2;
var C10X_0_Mins_ug_mL
C10X_30_Mins_ug_mL
C10X_60_Mins_ug_mL
C10X_90_Mins_ug_mL
C10X_120_Mins_ug_mL
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12;
ranks rC10X_0_Mins_ug_mL
rC10X_30_Mins_ug_mL
rC10X_60_Mins_ug_mL
rC10X_90_Mins_ug_mL
rC10X_120_Mins_ug_mL
rMPA_AUC_0_12_dose_adj 
rMPA_EHR_5_12;
run;

*Outputting an rtf document for the stratfied correlation in the nine 10X samples;

title1;

options orientation=portrait center number pageno=1 nodate;

ods rtf file="/home/u63327094/MPAG_Bioaccumulation_Analysis/MPAG_10X_stratified_correlation.rtf";

proc sort data = MPAG10X;
by rMPA_AUC_0_12_dose_adj;
run;

proc corr data = MPAG10X;
var 
MPA_Cmax_dose_adj 
MPA_Ctrough_dose_adj 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
C10X_0_Mins_ug_mL
C10X_30_Mins_ug_mL
C10X_60_Mins_ug_mL
C10X_90_Mins_ug_mL
C10X_120_Mins_ug_mL;
by rMPA_AUC_0_12_dose_adj;
run;

proc sort data = MPAG10X;
by rMPA_EHR_5_12;
run;

proc corr data = MPAG10X;
var 
MPA_Cmax_dose_adj 
MPA_Ctrough_dose_adj 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
C10X_0_Mins_ug_mL
C10X_30_Mins_ug_mL
C10X_60_Mins_ug_mL
C10X_90_Mins_ug_mL
C10X_120_Mins_ug_mL;
by rMPA_EHR_5_12;
run;

*Export the data to excel for network testing using iGraph;

proc export data=MPAG10X
    outfile="/home/u63327094/MPAG_Bioaccumulation_Analysis/MPAG_10X_bacteria.xlsx"
    dbms=xlsx
    replace;
run;

*Correlation with Bioacumulated MPA levels;

proc corr data = MPAG16Slong;
var 
MPA_Cmax_dose_adj 
MPA_Ctrough_dose_adj 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
T6_BEAD_MPAG_Lysis
T24_BEAD_MPAG_Lysis;
where Lysis_data = "Yes";
run;

data MPAG_Lysis;
set MPAG16Slong;
where Lysis_data = "Yes";
run;

*Creating median variables for this subanalysis;
proc rank data=MPAG_Lysis out=MPAG_Lysis groups=2;
var T6_BEAD_MPAG_Lysis
T24_BEAD_MPAG_Lysis
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12;
ranks rT6_BEAD_MPAG_Lysis
rT24_BEAD_MPAG_Lysis
rMPA_AUC_0_12_dose_adj 
rMPA_EHR_5_12;
run;

proc sort data = MPAG_Lysis;
by rMPA_AUC_0_12_dose_adj;
run;

proc corr data = MPAG_Lysis;
var 
MPA_Cmax_dose_adj 
MPA_Ctrough_dose_adj 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
T6_BEAD_MPAG_Lysis
T24_BEAD_MPAG_Lysis;
by rMPA_AUC_0_12_dose_adj;
run;

proc sort data = MPAG_Lysis;
by rMPA_EHR_5_12;
run;

proc corr data = MPAG_Lysis;
var 
MPA_Cmax_dose_adj 
MPA_Ctrough_dose_adj 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
T6_BEAD_MPAG_Lysis
T24_BEAD_MPAG_Lysis;
by rMPA_EHR_5_12;
run;

*Export the data to excel for network testing using iGraph;

proc export data=MPAG_Lysis
    outfile="/home/u63327094/MPAG_Bioaccumulation_Analysis/MPAG_Lysis_bacteria.xlsx"
    dbms=xlsx
    replace;
run;

*Transposing the dataset from long to wide;
*https://stats.oarc.ucla.edu/sas/modules/how-to-reshape-data-long-to-wide-using-proc-transpose/;


proc sort data = TAC16Slong;
by Patient_ID;
run;

data TAC16Slong; 
set TAC16Slong;
if Study_arm_2	ne "Prospective" then delete;
run;

proc transpose data=TAC16Slong out=shannon (drop=_name_ _label_) prefix=shannon_M;
    by Patient_ID;
    id Time_point;
    var shannon;
    run;
    
proc transpose data=TAC16Slong out=bc_pcoa_axis1 (drop=_name_ _label_) prefix=bc_pcoa_axis1_M;
    by Patient_ID;
    id Time_point;
    var bc_pcoa_axis1;
    run;    

proc transpose data=TAC16Slong out=bc_pcoa_axis2 (drop=_name_ _label_) prefix=bc_pcoa_axis2_M;
    by Patient_ID;
    id Time_point;
    var bc_pcoa_axis2;
    run; 

proc transpose data=TAC16Slong out=bc_pcoa_axis3 (drop=_name_ _label_) prefix=bc_pcoa_axis3_M;
    by Patient_ID;
    id Time_point;
    var bc_pcoa_axis3;
    run; 
    
proc transpose data=TAC16Slong out=Bacteroides_ovatus (drop=_name_ _label_) prefix=Bacteroides_ovatus_M;
    by Patient_ID;
    id Time_point;
    var Bacteroides_ovatus;
    run; 
    
proc transpose data=TAC16Slong out=Anaerostipes_spp (drop=_name_ _label_) prefix=Anaerostipes_spp_M;
    by Patient_ID;
    id Time_point;
    var Anaerostipes_spp;
    run;
    
proc transpose data=TAC16Slong out=Blautia_spp (drop=_name_ _label_) prefix=Blautia_spp_M;
    by Patient_ID;
    id Time_point;
    var Blautia_spp;
    run;
    
proc transpose data=TAC16Slong out=Clostridium_aldenense (drop=_name_ _label_) prefix=Clostridium_aldenense_M;
    by Patient_ID;
    id Time_point;
    var Clostridium_aldenense;
    run;
    
proc transpose data=TAC16Slong out=Clostridium_bolteae (drop=_name_ _label_) prefix=Clostridium_bolteae_M;
    by Patient_ID;
    id Time_point;
    var Clostridium_bolteae;
    run;
    
proc transpose data=TAC16Slong out=Clostridium_citroniae (drop=_name_ _label_) prefix=Clostridium_citroniae_M;
    by Patient_ID;
    id Time_point;
    var Clostridium_citroniae;
    run;
    
proc transpose data=TAC16Slong out=Clostridium_clostridioforme (drop=_name_ _label_) prefix=Clostridium_clostridioforme_M;
    by Patient_ID;
    id Time_point;
    var Clostridium_clostridioforme;
    run;
    
proc transpose data=TAC16Slong out=Clostridium_hathewayi (drop=_name_ _label_) prefix=Clostridium_hathewayi_M;
    by Patient_ID;
    id Time_point;
    var Clostridium_hathewayi;
    run;
    
proc transpose data=TAC16Slong out=Clostridium_symbiosum (drop=_name_ _label_) prefix=Clostridium_symbiosum_M;
    by Patient_ID;
    id Time_point;
    var Clostridium_symbiosum;
    run;
    
proc transpose data=TAC16Slong out=Coprococcus_spp (drop=_name_ _label_) prefix=Coprococcus_spp_M;
    by Patient_ID;
    id Time_point;
    var Coprococcus_spp;
    run;
    
proc transpose data=TAC16Slong out=Dorea_formicigenerans (drop=_name_ _label_) prefix=Dorea_formicigenerans_M;
    by Patient_ID;
    id Time_point;
    var Dorea_formicigenerans;
    run;
    
proc transpose data=TAC16Slong out=Ruminococcus_gnavus (drop=_name_ _label_) prefix=Ruminococcus_gnavus_M;
    by Patient_ID;
    id Time_point;
    var Ruminococcus_gnavus;
    run;
    
proc transpose data=TAC16Slong out=Ruminococcaceae_spp (drop=_name_ _label_) prefix=Ruminococcaceae_spp_M;
    by Patient_ID;
    id Time_point;
    var Ruminococcaceae_spp;
    run;
    
proc transpose data=TAC16Slong out=Faecalibacterium_prausnitzii (drop=_name_ _label_) prefix=Faecalibacterium_prausnitzii_M;
    by Patient_ID;
    id Time_point;
    var Faecalibacterium_prausnitzii;
    run;
    
proc transpose data=TAC16Slong out=Erysipelotrichaceae_spp (drop=_name_ _label_) prefix=Erysipelotrichaceae_spp_M;
    by Patient_ID;
    id Time_point;
    var Erysipelotrichaceae_spp;
    run;
    
/*proc transpose data=TAC16Slong out=Total_Tac_ketoreducing_bacteria (drop=_name_ _label_) prefix=Total_Tac_ketoreducing_bacteria_M;
    by Patient_ID;
    id Time_point;
    var Total_Tac_ketoreducing_bacteria;
    run;*/

data TAC16Swide;
merge shannon
bc_pcoa_axis1
bc_pcoa_axis2
bc_pcoa_axis3
Bacteroides_ovatus
Anaerostipes_spp
Blautia_spp
Clostridium_aldenense
Clostridium_bolteae
Clostridium_citroniae
Clostridium_clostridioforme
Clostridium_hathewayi
Clostridium_symbiosum
Coprococcus_spp
Dorea_formicigenerans
Ruminococcus_gnavus
Ruminococcaceae_spp
Faecalibacterium_prausnitzii
Erysipelotrichaceae_spp;
By Patient_ID;
run;

proc print data = TAC16Swide;
run;

*Restricting the analysis to samples with an M2 and M4 sample;
data TAC16SwideM2M4;
set TAC16Swide;
if Shannon_M2 = . or Shannon_M4 = . then delete;
run;

proc print data = TAC16SwideM2M4;
run;

*Trying to generate boxplots for the TAC keto-reducing commensals;
*One option would be export the wide dataset into excel and make figures there;

data TAC16SwideM2M4;
set TAC16SwideM2M4;
Total_Tac_ketoreducing_M2 = Bacteroides_ovatus_M2 + 
Anaerostipes_spp_M2 + 
Blautia_spp_M2 + 
Clostridium_aldenense_M2 + 
Clostridium_bolteae_M2 + 
Clostridium_citroniae_M2 + 
Clostridium_clostridioforme_M2 + 
Clostridium_hathewayi_M2 + 
Clostridium_symbiosum_M2 + 
Coprococcus_spp_M2 + 
Dorea_formicigenerans_M2 + 
Ruminococcus_gnavus_M2 + 
Ruminococcaceae_spp_M2 + 
Faecalibacterium_prausnitzii_M2 + 
Erysipelotrichaceae_spp_M2;
Total_Tac_ketoreducing_M4 = Bacteroides_ovatus_M4 + 
Anaerostipes_spp_M4 + 
Blautia_spp_M4 + 
Clostridium_aldenense_M4 + 
Clostridium_bolteae_M4 + 
Clostridium_citroniae_M4 + 
Clostridium_clostridioforme_M4 + 
Clostridium_hathewayi_M4 + 
Clostridium_symbiosum_M4 + 
Coprococcus_spp_M4 + 
Dorea_formicigenerans_M4 + 
Ruminococcus_gnavus_M4 + 
Ruminococcaceae_spp_M4 + 
Faecalibacterium_prausnitzii_M4 + 
Erysipelotrichaceae_spp_M4;
Bacteroides_ovatus_diff = Bacteroides_ovatus_M4 - Bacteroides_ovatus_M2;
Anaerostipes_spp_diff = Anaerostipes_spp_M4 - Anaerostipes_spp_M2;
Blautia_spp_diff = Blautia_spp_M4 - Blautia_spp_M2;
Clostridium_aldenens_diff  = Clostridium_aldenense_M4 - Clostridium_aldenense_M2;
Clostridium_bolteae_diff = Clostridium_bolteae_M4 - Clostridium_bolteae_M2;
Clostridium_citroniae_diff = Clostridium_citroniae_M4 - Clostridium_citroniae_M2;
Clostridium_clostridioforme_diff = Clostridium_clostridioforme_M4 - Clostridium_clostridioforme_M2;
Clostridium_hathewayi_diff = Clostridium_hathewayi_M4 - Clostridium_hathewayi_M2;
Clostridium_symbiosum_diff = Clostridium_symbiosum_M4 - Clostridium_symbiosum_M2;
Coprococcus_spp_diff = Coprococcus_spp_M4 - Coprococcus_spp_M2;
Dorea_formicigenerans_diff = Dorea_formicigenerans_M4 - Dorea_formicigenerans_M2; 
Ruminococcus_gnavus_diff = Ruminococcus_gnavus_M4 - Ruminococcus_gnavus_M2;
Ruminococcaceae_spp_diff = Ruminococcaceae_spp_M4 - Ruminococcaceae_spp_M2;
F_prausnitzii_diff = Faecalibacterium_prausnitzii_M4 - Faecalibacterium_prausnitzii_M2; 
E_spp_diff = Erysipelotrichaceae_spp_M4 - Erysipelotrichaceae_spp_M2;
run;

*Outputting an rtf document for the variability in relative abundance at M2;

title1;

options orientation=portrait center number pageno=1 nodate;

ods rtf file="/home/u63327094/TAC_16S_Analysis/RelativeAbundanceVariability_M2.rtf";

proc univariate data = TAC16SwideM2M4;
var Faecalibacterium_prausnitzii_M2 Ruminococcus_gnavus_M2 Ruminococcaceae_spp_M2 Clostridium_clostridioforme_M2 Bacteroides_ovatus_M2 Erysipelotrichaceae_spp_M2;
run;

*Outputting an rtf document for the variability in relative abundance at M4;

title1;

options orientation=portrait center number pageno=1 nodate;

ods rtf file="/home/u63327094/TAC_16S_Analysis/RelativeAbundanceVariability_M4.rtf";

proc univariate data = TAC16SwideM2M4;
var Faecalibacterium_prausnitzii_M4 Ruminococcus_gnavus_M4 Ruminococcaceae_spp_M4 Clostridium_clostridioforme_M4 Bacteroides_ovatus_M4 Erysipelotrichaceae_spp_M4;
run;

*Outputting an rtf document for the change in relative abundance between M2 and M4;

title1;

options orientation=portrait center number pageno=1 nodate;

ods rtf file="/home/u63327094/TAC_16S_Analysis/RelativeAbundanceChange_M2M4.rtf";

proc univariate data = TAC16SwideM2M4;
var F_prausnitzii_diff Ruminococcus_gnavus_diff Ruminococcaceae_spp_diff Clostridium_clostridioforme_diff Bacteroides_ovatus_diff E_spp_diff;
run;

*Export the data to excel for boxplots;

proc export data=TAC16SwideM2M4
    outfile="/home/u63327094/TAC_16S_Analysis/M2M4_tac_bacteria.xlsx"
    dbms=xlsx
    replace;
run;

*****************************************************************
*****************************************************************
*****************************************************************;

*You will need to upload the files to the SAS studio server directly;
*Right click on the folder location and select properties to copy the path name;

libname mylib "/home/u63327094/analysis_MPAG_Bioaccumulation_Analysis";

*Importing the ASN excel file to run descriptive statistics;

proc import 
datafile= '/home/u63327094/analysis_MPAG_Bioaccumulation_Analysis/48_11_MAP_MPAG_10X_bacteria.xlsx'
out=mylib.MPAG16SASN
dbms=XLSX	
replace;
getnames=yes;
run;

proc import 
datafile= '/home/u63327094/analysis_MPAG_Bioaccumulation_Analysis/48_11_MAP_MPAG_10X_bacteria_MPAbloodtest.xlsx'
out=mylib.MPAG16SASN2
dbms=XLSX	
replace;
getnames=yes;
run;

*Load data; 
data MPAG16S; 
set mylib.MPAG16SASN;
run; 

data MPAG16S; 
set mylib.MPAG16SASN2;
run; 

proc contents data = MPAG16S varnum;
run;

*Demographics Information;

proc means data = MPAG16S mean std;
var sex_2 
bmi_baseline 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 ;
run;

proc freq data = MPAG16S;
tables sex predom_race donor_type / nocol norow nopercent;
run;

*Correlation with Bioacumulated MPA levels (to add as a table to the poster);

proc corr data = MPAG16S;
var 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
C10X_0_Mins_ug_mL
C10X_T30_standardized
C10X_T60_standardized
C10X_T90_standardized
C10X_T120_standardized;
run;

proc corr data = MPAG16S;
var 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
C10X_0_Mins_ug_mL
C10X_30_Mins_ug_mL
C10X_60_Mins_ug_mL
C10X_90_Mins_ug_mL
C10X_120_Mins_ug_mL;
run;

proc corr data = MPAG16S;
var 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
C10X_0_Mins_ug_mL
C10X_T30_standardized
C10X_T60_standardized
C10X_T90_standardized
C10X_T120_standardized;
run;

*List of BGUS bacteria of interest;

*"Collinsella_aerofaciens", "Bacteroides_ovatus", "Bacteroides_uniformis", "Clostridium_clostridioforme",  "Roseburia_inulinivorans", "Ruminococcus_gnavus", "Faecalibacterium_prausnitzii";

proc corr data = MPAG16S;
var 
MPA_AUC_0_12_dose_adj 
MPA_EHR_5_12 
Collinsella_aerofaciens
Bacteroides_ovatus
Bacteroides_uniformis
Clostridium_clostridioforme
Roseburia_inulinivorans
Ruminococcus_gnavus
Faecalibacterium_prausnitzii;
run;

proc corr data = MPAG16S;
var 
C10X_0_Mins_ug_mL
C10X_T30_standardized
C10X_T60_standardized
C10X_T90_standardized
C10X_T120_standardized
Collinsella_aerofaciens
Bacteroides_ovatus
Bacteroides_uniformis
Clostridium_clostridioforme
Roseburia_inulinivorans
Ruminococcus_gnavus
Faecalibacterium_prausnitzii;
run;

*Correlations between invitro data and in-vivo MPA measurements;

proc corr data = MPAG16S;
var 
C10X_0_Mins_ug_mL
C10X_T30_standardized
C10X_T60_standardized
C10X_T90_standardized
C10X_T120_standardized
SrCr_mg_dL
MPA_urine_total_mg
MPA_renal_cl_L_hr
MPA_trough_T0
;
run;

**********************************************************************************************************************************************;
**********************************************************************************************************************************************;
**********************************************************************************************************************************************;


*Merging the PK demographic dataset provided by MOataz with thee T0 invitro dataset at Ajay's request;

libname mylib "/home/u63327094/analysis_MPAG_Bioaccumulation_Analysis";

*This is the xlsx file;
proc import 
datafile= '/home/u63327094/analysis_MPAG_Bioaccumulation_Analysis/C_11_MISSION_demographics_updated_final_n_84_02152024.xlsx'
out=mylib.pkdemo
dbms=xlsx	
replace;
getnames=yes;
run;

proc import 
datafile= '/home/u63327094/analysis_MPAG_Bioaccumulation_Analysis/C_10_Results_P6_07_Compiled_InVitro_120mins_only_MAR2024.xlsx'
out=mylib.invitro120mins
dbms=xlsx	
replace;
getnames=yes;
run;

proc print data = mylib.pkdemo;
run;

proc contents data = mylib.pkdemo;
run;

proc print data = mylib.invitro120mins;
run;

proc contents data = mylib.invitro120mins;
run;

*Load data; 
data pkdemo; 
set mylib.pkdemo;
pkdemocheck = 1;
run; 

proc sort data = pkdemo;
by record_id;
run;

data invitro120mins; 
set mylib.invitro120mins;
invitrocheck = 1;
run; 

proc sort data = invitro120mins;
by record_id;
run;

data pkdemoinvitro;
merge pkdemo invitro120mins;
by record_id;
run;

proc print data=pkdemoinvitro;
run;

data pkdemoinvitro;
set pkdemoinvitro;
if invitrocheck ne 1 then delete;
run;

proc print data=pkdemoinvitro;
run;

*Correlations between invitro data and in-vivo MPA measurements;

/*
MPA_AUC_0_12	
MPA_AUC_5_12	
MPA_AUC_6_12	
MPA_AUC6_12_0_12	
MPA_AUC5_12_0_12
T0_Mean
T0_Mean_V2		
T30_Mean	
T60_Mean	
T90_Mean
T30diff	
T60diff	
T90diff
T30diff_V2	
T60diff_V2	
T90diff_V2
*/

**After discussing the results with Ajay, he requested that the analysis be rerun excluding any samples where the t0 was taken from the MPAG tube (set_4 indicator variable)
and then further restrict the analysis to samples where the to timepoint was ND (set_5);

proc contents data = pkdemoinvitro;
run;

proc print data=pkdemoinvitro;
run;

*ods excel file="/home/u63327094/analysis_MPAG_Bioaccumulation_Analysis/pk_demo_invitro_preliminary_n31.xlsx";
ods excel file="/home/u63327094/analysis_MPAG_Bioaccumulation_Analysis/pk_demo_invitro_preliminary_n27.xlsx";

proc sort data=pkdemoinvitro;
by cohort;
run;

proc means data = pkdemoinvitro;
var age_PK;
by cohort;
where set_4 = 1;
run;

proc freq data = pkdemoinvitro;
tables gender*cohort race*cohort donar_status*cohort/nocol norow nopercent;
where set_4 = 1;
run;
	
proc corr data = pkdemoinvitro;
var 
MPA_AUC_0_12
T0_Mean	
T0_Mean_V2
T30_Mean	
T60_Mean	
T90_Mean
/*T30diff*/	
T60diff	
T90diff
T30diff_V2	
/*T60diff_V2	
T90diff_V2*/
;
where set_4 = 1;
run;

proc corr data = pkdemoinvitro;
var 
MPA_AUC_5_12	
T0_Mean
T0_Mean_V2		
T30_Mean	
T60_Mean	
T90_Mean
/*T30diff*/	
T60diff	
T90diff
T30diff_V2	
/*T60diff_V2	
T90diff_V2*/
;
where set_4 = 1;
run;

proc corr data = pkdemoinvitro;
var 
MPA_AUC_6_12
T0_Mean
T0_Mean_V2		
T30_Mean	
T60_Mean	
T90_Mean
/*T30diff*/	
T60diff	
T90diff
T30diff_V2	
/*T60diff_V2	
T90diff_V2*/
;
where set_4 = 1;
run;

proc corr data = pkdemoinvitro;
var 
MPA_AUC5_12_0_12
T0_Mean
T0_Mean_V2		
T30_Mean	
T60_Mean	
T90_Mean
/*T30diff*/	
T60diff	
T90diff
T30diff_V2	
/*T60diff_V2	
T90diff_V2*/
;
where set_4 = 1;
run;

/*

*https://support.sas.com/documentation/cdl/en/procstat/63104/HTML/default/viewer.htm#procstat_corr_sect034.htm;

ods graphics on;
proc corr data=fish1 nomiss
          plots=scatter(nvar=2 alpha=.20 .30);
   var Height Width Length3 Weight3;
 run;
ods graphics off;

*/

ods graphics on;
proc corr data=pkdemoinvitro
          plots=scatter(nvar=2 alpha=.20);
   var T0_Mean_V2 MPA_AUC5_12_0_12 ;
where set_4 = 1;
run;
ods graphics off;

proc corr data = pkdemoinvitro;
var 
MPA_AUC6_12_0_12
T0_Mean
T0_Mean_V2		
T30_Mean	
T60_Mean	
T90_Mean
/*T30diff*/	
T60diff	
T90diff
T30diff_V2	
/*T60diff_V2	
T90diff_V2*/
;
where set_4 = 1;
run;

ods graphics on;
proc corr data=pkdemoinvitro
          plots=scatter(nvar=2 alpha=.20);
   var T0_Mean_V2 MPA_AUC6_12_0_12 ;
where set_4 = 1;
run;
ods graphics off;

ods excel close;

*Excluding all PIDs where the T0 was below detetable threshold as potentially bad assays;

*ods excel file="/home/u63327094/analysis_MPAG_Bioaccumulation_Analysis/pk_demo_invitro_preliminary_n20.xlsx";
ods excel file="/home/u63327094/analysis_MPAG_Bioaccumulation_Analysis/pk_demo_invitro_preliminary_n19.xlsx";
proc sort data=pkdemoinvitro;
by cohort;
run;

proc means data = pkdemoinvitro;
var age_PK;
by cohort;
where set_5 = 1;
run;

proc freq data = pkdemoinvitro;
tables gender*cohort race*cohort donar_status*cohort/nocol norow nopercent;
where set_5 = 1;
run;

proc corr data = pkdemoinvitro;
var 
MPA_AUC_0_12
T0_Mean	
T0_Mean_V2
T30_Mean	
T60_Mean	
T90_Mean
/*T30diff*/	
T60diff	
T90diff
T30diff_V2	
/*T60diff_V2	
T90diff_V2*/
;
where set_5 = 1;
run;

proc corr data = pkdemoinvitro;
var 
MPA_AUC_5_12	
T0_Mean
T0_Mean_V2		
T30_Mean	
T60_Mean	
T90_Mean
/*T30diff*/	
T60diff	
T90diff
T30diff_V2	
/*T60diff_V2	
T90diff_V2*/
;
where set_5 = 1;
run;

proc corr data = pkdemoinvitro;
var 
MPA_AUC_6_12	
T0_Mean
T0_Mean_V2		
T30_Mean	
T60_Mean	
T90_Mean
/*T30diff*/	
T60diff	
T90diff
T30diff_V2	
/*T60diff_V2	
T90diff_V2*/
;
where set_5 = 1;
run;

proc corr data = pkdemoinvitro;
var 
MPA_AUC5_12_0_12
T0_Mean
T0_Mean_V2		
T30_Mean	
T60_Mean	
T90_Mean
/*T30diff*/	
T60diff	
T90diff
T30diff_V2	
/*T60diff_V2	
T90diff_V2*/
;
where set_5 = 1;
run;

ods graphics on;
proc corr data=pkdemoinvitro
          plots=scatter(nvar=2 alpha=.20);
   var T0_Mean_V2 MPA_AUC5_12_0_12 ;
where set_5 = 1;
run;
ods graphics off;

proc corr data = pkdemoinvitro;
var 
MPA_AUC6_12_0_12
T0_Mean
T0_Mean_V2		
T30_Mean	
T60_Mean	
T90_Mean
/*T30diff*/	
T60diff	
T90diff
T30diff_V2	
/*T60diff_V2	
T90diff_V2*/
;
where set_5 = 1;
run;

ods graphics on;
proc corr data=pkdemoinvitro
          plots=scatter(nvar=2 alpha=.20);
   var T0_Mean_V2 MPA_AUC6_12_0_12 ;
where set_5 = 1;
run;
ods graphics off;

ods excel close;

**********************************************************************************************************************************************;
**********************************************************************************************************************************************;
**********************************************************************************************************************************************;