*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*We decided to go back to the Sarah's original dataset to pull the covariates and reconstruct the Summer 2017 analysis dataset;

*adding the new ARIC cancer cases(up to 2012);

libname mylib 'E:\Professional Folder\WBOB\ARIC Data\Updated ARIC cases\CANCER';

Data overall2012;
set mylib.OVERALL_CANCER_2012;
Run;

proc contents data=overall2012;
run;

*We are going tp use the individual cancer datasets, because the overall cancer dataset lists primary & secondary cancers, which is a lot of extraneous data;

Data bladder2012;
set mylib.BLADDER_CANCER_2012;
Run;

proc contents data=bladder2012;
run;

data bladder2012;
set bladder2012 (Keep= id incidence death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data bladder2012;
set bladder2012;
Bladdercancer12 = incidence;
Bladderdeath12 = death;
Bladderfu1 = incidence_t1;
Bladderfu2 = incidence_t2;
Bladderfu3 = incidence_t3;
Bladderfu4 = incidence_t4;
BladderDeathfu1 = death_t1;
BladderDeathfu2 = death_t2;
BladderDeathfu3 = death_t3;
BladderDeathfu4 = death_t4;
bladdercancerprev = prvcancr;
run;

Data breast2012;
set mylib.BREAST_CANCER_2012;
Run;

proc contents data=breast2012;
run;

data breast2012;
set breast2012 (Keep= id incidence death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data breast2012;
set breast2012;
breastcancer12 = incidence;
breastdeath12 = death;
breastfu1 = incidence_t1;
breastfu2 = incidence_t2;
breastfu3 = incidence_t3;
breastfu4 = incidence_t4;
breastDeathfu1 = death_t1;
breastDeathfu2 = death_t2;
breastDeathfu3 = death_t3;
breastDeathfu4 = death_t4;
breastcancerprev = prvcancr;
run;

Data colorectal2012;
set mylib.COLORECTAL_CANCER_2012;
Run;

proc contents data=colorectal2012;
run;

data colorectal2012;
set colorectal2012 (Keep= id colorectal_incidence colorectal_death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data colorectal2012;
set colorectal2012;
colorectalcancer12 = colorectal_incidence;
colorectaldeath12 = colorectal_death;
colorectalfu1 = incidence_t1;
colorectalfu2 = incidence_t2;
colorectalfu3 = incidence_t3;
colorectalfu4 = incidence_t4;
colorectalDeathfu1 = death_t1;
colorectalDeathfu2 = death_t2;
colorectalDeathfu3 = death_t3;
colorectalDeathfu4 = death_t4;
colorectalcancerprev = prvcancr;
run;

Data liver2012;
set mylib.LIVER_CANCER_2012;
Run;

proc contents data=liver2012;
run;

data liver2012;
set liver2012 (Keep= id incidence death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data liver2012;
set liver2012;
livercancer12 = incidence;
liverdeath12 = death;
liverfu1 = incidence_t1;
liverfu2 = incidence_t2;
liverfu3 = incidence_t3;
liverfu4 = incidence_t4;
liverDeathfu1 = death_t1;
liverDeathfu2 = death_t2;
liverDeathfu3 = death_t3;
liverDeathfu4 = death_t4;
livercancerprev = prvcancr;
run;

Data lung2012;
set mylib.LUNG_CANCER_2012;
Run;

proc contents data=lung2012;
run;

data lung2012;
set lung2012 (Keep= id incidence death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data lung2012;
set lung2012;
lungcancer12 = incidence;
lungdeath12 = death;
lungfu1 = incidence_t1;
lungfu2 = incidence_t2;
lungfu3 = incidence_t3;
lungfu4 = incidence_t4;
lungDeathfu1 = death_t1;
lungDeathfu2 = death_t2;
lungDeathfu3 = death_t3;
lungDeathfu4 = death_t4;
lungcancerprev = prvcancr;
run;

Data pancreas2012;
set mylib.PANCREAS_CANCER_2012;
Run;

proc contents data=pancreas2012;
run;

data pancreas2012;
set pancreas2012 (Keep= id incidence death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data pancreas2012;
set pancreas2012;
pancreascancer12 = incidence;
pancreasdeath12 = death;
pancreasfu1 = incidence_t1;
pancreasfu2 = incidence_t2;
pancreasfu3 = incidence_t3;
pancreasfu4 = incidence_t4;
pancreasDeathfu1 = death_t1;
pancreasDeathfu2 = death_t2;
pancreasDeathfu3 = death_t3;
pancreasDeathfu4 = death_t4;
pancreascancerprev = prvcancr;
run;

Data prostate2012;
set mylib.PROSTATE_CANCER_2012;
Run;

proc contents data=prostate2012;
run;

data prostate2012;
set prostate2012 (Keep= id incidence death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data prostate2012;
set prostate2012;
prostatecancer12 = incidence;
prostatedeath12 = death;
prostatefu1 = incidence_t1;
prostatefu2 = incidence_t2;
prostatefu3 = incidence_t3;
prostatefu4 = incidence_t4;
prostateDeathfu1 = death_t1;
prostateDeathfu2 = death_t2;
prostateDeathfu3 = death_t3;
prostateDeathfu4 = death_t4;
prostatecancerprev = prvcancr;
run;

*merging the cancer datasets;

proc sort data = bladder2012; by id; run;
proc sort data = breast2012; by id; run;
proc sort data = colorectal2012; by id; run;
proc sort data = liver2012; by id; run;
proc sort data = lung2012; by id; run;
proc sort data = pancreas2012; by id; run;
proc sort data = prostate2012; by id; run;

data ariccancers2012;
merge bladder2012 breast2012 colorectal2012 liver2012 lung2012 pancreas2012 prostate2012;
by id;
run;

proc contents data = ariccancers2012;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*The hematology data (WBC, eosinophils, lymphocytes.... )comes from the HTMA data (visit 1) Y:\DATA\VISIT_1 and HMTB data (visit 2) Y:\DATA\VISIT_2;
*Based on Dr. Prizment's B2M analysis;

libname blood 'E:\Professional Folder\WBOB\ARIC Data\Covariates_Visits_ARIC\SAS Datasets';

Data wbcvisit1;
set blood.HMTA;
Run;

proc contents data=wbcvisit1;
run;

data wbcvisit1;
set wbcvisit1 (keep = ID HMTA01 HMTA02 HMTA03 HMTA04 HMTA05 HMTA06 HMTA07 HMTA08 HMTA09 HMTA10 HMTA14);
run;

data wbcvisit1;
set wbcvisit1;
rename HMTA01 = HEMATOCRITV1 
HMTA02 = HEMOGLOBINV1
HMTA03 = WBCV1 
HMTA04 = PLATELETV1 
HMTA05 = NEUTROPHILSV1 
HMTA06 = BANDSV1
HMTA07 = LYMPHOCYTESV1 
HMTA08 = MONOCYTESV1 
HMTA09 = EOSINOPHILSV1 
HMTA10 = BASOPHILSV1 
HMTA14 = BLOODDATEV1;
run;

proc contents data = wbcvisit1;
run;

Data wbcvisit2;
set blood.HMTB;
Run;

proc contents data=wbcvisit2;
run;

data wbcvisit2;
set wbcvisit2 (keep = ID HMTB01 HMTB02 HMTB03 HMTB04 HMTB05 HMTB06 HMTB07 HMTB08 HMTB09 HMTB10 HMTB14);
run;

data wbcvisit2;
set wbcvisit2;
rename HMTB01 = HEMATOCRITV2 
HMTB02 = HEMOGLOBINV2
HMTB03 = WBCV2 
HMTB04 = PLATELETV2 
HMTB05 = NEUTROPHILSV2 
HMTB06 = BANDSV2
HMTB07 = LYMPHOCYTESV2 
HMTB08 = MONOCYTESV2 
HMTB09 = EOSINOPHILSV2 
HMTB10 = BASOPHILSV2 
HMTB14 = BLOODDATEV2;
run;

proc contents data = wbcvisit2;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*CRP data;

libname blood 'E:\Professional Folder\WBOB\ARIC Data\Covariates_Visits_ARIC\SAS Datasets';

Data crp;
set blood.analys;
Run;

proc contents data = crp;
run;

Data crpv2;
set blood.CRP4;
Run;

proc contents data = crpv2;
run;

data crpv2;
set crpv2 (keep= id CRP4);
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Covariate data from Sara's do file: we are using this specific SAS code to convert the STATA dataset into a SAS dataset after having exported it using the EXPORT funtion from STATA;

libname datapath 'E:\Professional Folder\WBOB\ARIC WBC Project\2_Summer 2017 RA Projects\SAS Datasets' ;
libname xptfile xport 'E:\Professional Folder\WBOB\ARIC WBC Project\2_Summer 2017 RA Projects\SAS Datasets' ;

proc copy in = xptfile out = datapath ;

proc format library = work ;
    value _MERGE
           1 = 'master only (1)'
           2 = 'using only (2)'
           3 = 'matched (3)'
           4 = 'missing updated (4)'
           5 = 'nonmissing conflict (5)' ;

quit ;

proc print data= DATAPATH.WBC(obs=50);
run;

proc contents data = datapath.wbc;
run;

proc freq data = datapath.wbc;
tables center;
run;

libname summer 'E:\Professional Folder\WBOB\ARIC WBC Project\2_Summer 2017 RA Projects\SAS Datasets';

data summer.original;
set datapath.wbc;
run;

data original;
set summer.original;
run;

proc contents data = original;
run;

proc freq data = original;
tables center racegrp ELEVEL01;
run;

proc freq data = original;
tables CENTER;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*We will be adding a few additional variables that were not used in the descriptive statistics table but were used in the statistical models section. We are adding
Waist circumference (ANTA07A + WSTHPR01), number of hours per week around smokers + occupation for environmental smoke (HOM52 + HOM55), and we are adding RHXA07 to RHXA44 to capture the various 
hormone exposure that can be uesd to determine the different HRT categories to include in the model; 

data original2;
set original (Keep= ID BMI01 ASPIRINC ANTICOAG BIRTHDAT CENTER CIGR01 CIGRYR01 CIGT01
CIGTYR01 CURDRK02 CURSMK01 DIABETES DIABTS02 DIABTS03 DRNKR01 DTIACY ELEVEL01 ELEVEL02 ETHANL03
EVRDRK01 EVRSMK01 FORDRK01 FORSMK01 GENDER GLUCOS01 GLUSIU01 HDL01 HDL201 HDL301 HOM32 HOM35 HOM48 HOM54 HOM55 HORMON01 HORMON02
INSSIU01 LDL02 LDLSIU02 LIPA01 LIPA02 LIPA06 LIPA07 MHXA47 MHXACY RACEGRP SPRT_I02 STATINCO TCHSIU01 TGLEFH01 TOTCAL03
V1AGE01 V1DATE01 WSTHPR01 HEMA11 HMTA01 HMTA02 HMTA03 HMTA04 HMTA05 HMTA06 HMTA07 HMTA08 HMTA09 HMTA10 HMTA11 HMTA12 HMTA13 HMTA14 HMTA13D HMTA13M HMTA13Y HMTACY HMTAFLAG 
HMTB01 HMTB02 HMTB03 HMTB04 HMTB05 HMTB06 HMTB07 HMTB08 HMTB09 HMTB10 HMTB11 HMTB12 HMTB13 HMTB14 HMTB15 HMTB14D HMTB14M HMTB14Y HMTBCY HMTBFLAG ANTA07A WSTHPR01 HOM52 HOM55
RHXA07 RHXA08 RHXA09 RHXA10 RHXA11 RHXA12 RHXA13 RHXA14 RHXA15 RHXA16 RHXA17 RHXA18 RHXA19 RHXA20 RHXA21 RHXA22 RHXA23 RHXA24 RHXA25
RHXA26 RHXA27 RHXA28 RHXA29 RHXA30 RHXA31 RHXA32 RHXA33 RHXA34 RHXA35 RHXA36 RHXA37 RHXA38 RHXA39 RHXA40 RHXA41 RHXA42 RHXA43 RHXA44);
run;

Data original2;
set original2;
rename HEMA11 = HEMA11Sarah 
HMTA01 = HMTA01Sarah
HMTA02 = HMTA02Sarah
HMTA03 = HMTA03Sarah 
HMTA04 = HMTA04Sarah 
HMTA05 = HMTA05Sarah 
HMTA06 = HMTA06Sarah 
HMTA07 = HMTA07Sarah 
HMTA08 = HMTA08Sarah 
HMTA09 = HMTA09Sarah 
HMTA10 = HMTA10Sarah 
HMTA11 = HMTA11Sarah 
HMTA12 = HMTA12Sarah 
HMTA13 = HMTA13Sarah 
HMTA14 = HMTA14Sarah 
HMTA13D = HMTA13DSarah 
HMTA13M = HMTA13MSarah 
HMTA13Y = HMTA13YSarah 
HMTACY = HMTACYSarah 
HMTAFLAG = HMTAFLAGSarah 
HMTB01 = HMTB01Sarah
HMTB02 = HMTB02Sarah 
HMTB03 = HMTB03Sarah 
HMTB04 = HMTB04Sarah
HMTB05 = HMTB05Sarah 
HMTB06 = HMTB06Sarah 
HMTB07 = HMTB07Sarah 
HMTB08 = HMTB08Sarah 
HMTB09 = HMTB09Sarah 
HMTB10 = HMTB10Sarah 
HMTB11 = HMTB11Sarah 
HMTB12 = HMTB12Sarah 
HMTB13 = HMTB13Sarah 
HMTB14 = HMTB14Sarah 
HMTB15 = HMTB15Sarah 
HMTB14D = HMTB14DSarah 
HMTB14M = HMTB14MSarah 
HMTB14Y = HMTB14YSarah 
HMTBCY = HMTBCYSarah 
HMTBFLAG = HMTBFLAGSarah;
run;

proc contents data = original2;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*adding CVD outcomes to the dataset (CVD based on a history of angina, prevalent CHD,  Rose intermittent claudication and stroke) + Hypertension;

data original3;
set original (Keep= ID RANGNA01 PRVCHD05 ROSEIC03 STROKE01 HYPERT05);
run;

data summer.CVD;
set original3;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Merging the cancer data and the WBC, general ARIC covariates and CRP data;
*We dropped the HTMA and HMTB (hematology assays version 1 and 2 from the WBCvisit 2 datasets, and used the values from Sarah's Final analysis dataset in Darek's file;
*Added HOM32 HOM35 and HOM48 (# of cigarettes/day) variables to the dataset to calculate Pck=tears of smoking;

proc sort data = ariccancers2012; by id; run;
proc sort data = wbcvisit1; by id; run;
proc sort data = wbcvisit2; by id; run;
proc sort data = original2; by id; run;
proc sort data = crpv2; by id; run;
proc sort data = original3; by id; run;

data mylib.WBCCA17V2;
merge ariccancers2012 wbcvisit1 wbcvisit2 original2 crpv2 original3;
by id;
run;

proc contents data = mylib.WBCCA17V2;
run;

proc print data = mylib.WBCCA17V2;
where NEUTROPHILSV1 = 0;
run;

proc print data = mylib.WBCCA17V2;
where NEUTROPHILSV1 = 0;
var  HEMATOCRITV1 HEMOGLOBINV1 WBCV1 PLATELETV1 NEUTROPHILSV1 BANDSV1 LYMPHOCYTESV1 MONOCYTESV1 EOSINOPHILSV1 BASOPHILSV1 BLOODDATEV1 HEMATOCRITV2 HEMOGLOBINV2 WBCV2 PLATELETV2 NEUTROPHILSV2 BANDSV2 LYMPHOCYTESV2 MONOCYTESV2 EOSINOPHILSV2 BASOPHILSV2 BLOODDATEV2 HMTAFLAGSarah HMTACYSarah HMTA01Sarah HMTA02Sarah HMTA03Sarah HMTA04Sarah HMTA05Sarah HMTA06Sarah HMTA07Sarah HMTA08Sarah HMTA09Sarah HMTA10Sarah HMTA11Sarah HMTA12Sarah HMTA13Sarah HMTA13MSarah HMTA13DSarah HMTA13YSarah HMTA14Sarah ;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Adding a total cancer count for incidence and mortality based on the overall cancer dataset;
*There were approximately 4000 missing dxdate 1 in the dataset, so we checked to make sure that no identified cancer case had a missing value for diagnosis date, and Darek will code the non cases with missing diagnosis dates as PY = 12/31/2012 - V1DATE01;

libname mylib 'E:\Professional Folder\WBOB\ARIC Data\Updated ARIC cases\CANCER';

Data overall2012;
set mylib.OVERALL_CANCER_2012 (keep = ID anycancer dxdate_1 cancerdeath deathcensdt prvcancr);
Run;

proc contents data=overall2012;
run;

proc print data = overall2012 (obs= 500);
where dxdate_1 = . and anycancer = 1;
run;

proc freq data = overall2012;
table anycancer;
run;

proc print data = overall2012 (obs= 500);
where anycancer = .;
run;

proc print data = overall2012 (obs= 500);
where anycancer = 1;
run;


proc freq data = overall2012;
table anycancer*prvcancr;
run;

proc print data = overall2012 (obs= 500);
where anycancer = 0;
run;

data WBCCA17V2;
set mylib.WBCCA17V2;
run;

proc contents data = wbca17v2;
run;

proc sort data = wbcca17v2; by id; run;
proc sort data = overall2012; by id; run;

data mylib.wbcca17v3;
merge wbcca17v2 overall2012; 
by id;
run;

proc contents data = mylib.wbcca17v3;
run; 

*Checking the number of breast cancer cases (post menopausal);

proc freq data = mylib.wbcca17v3;
tables breastcancer12 breastcancer12*RHXA07 gender;
run;

proc means data = mylib.wbcca17v3;
var breastfu1;
run;

libname mylib3 'E:\Professional Folder\WBOB\ARIC WBC Project\2_Summer 2017 RA Projects\SAS Datasets';

data mylib3.wbccancer;
set mylib.wbcca17v3;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*SER 2019 analysis;

*Exposures of interest;

*The hematology data (WBC, eosinophils, lymphocytes.... )comes from the HTMA data (visit 1) Y:\DATA\VISIT_1 and HMTB data (visit 2) Y:\DATA\VISIT_2;
*We also imported Sarah's data from STATA and merged it with the updated (2012) ARIC cancer outcomes data;

*HMTA01 = HEMATOCRITV1 
HMTA02 = HEMOGLOBINV1
HMTA03 = WBCV1 
HMTA04 = PLATELETV1 
HMTA05 = NEUTROPHILSV1 
HMTA06 = BANDSV1
HMTA07 = LYMPHOCYTESV1 
HMTA08 = MONOCYTESV1 
HMTA09 = EOSINOPHILSV1 
HMTA10 = BASOPHILSV1 
HMTA14 = BLOODDATEV1;

*HMTB01 = HEMATOCRITV2 
HMTB02 = HEMOGLOBINV2
HMTB03 = WBCV2 
HMTB04 = PLATELETV2 
HMTB05 = NEUTROPHILSV2 
HMTB06 = BANDSV2
HMTB07 = LYMPHOCYTESV2 
HMTB08 = MONOCYTESV2 
HMTB09 = EOSINOPHILSV2 
HMTB10 = BASOPHILSV2 
HMTB14 = BLOODDATEV2;

*HEMA11 = HEMA11Sarah 
HMTA01 = HMTA01Sarah
HMTA02 = HMTA02Sarah
HMTA03 = HMTA03Sarah 
HMTA04 = HMTA04Sarah 
HMTA05 = HMTA05Sarah 
HMTA06 = HMTA06Sarah 
HMTA07 = HMTA07Sarah 
HMTA08 = HMTA08Sarah 
HMTA09 = HMTA09Sarah 
HMTA10 = HMTA10Sarah 
HMTA11 = HMTA11Sarah 
HMTA12 = HMTA12Sarah 
HMTA13 = HMTA13Sarah 
HMTA14 = HMTA14Sarah 
HMTA13D = HMTA13DSarah 
HMTA13M = HMTA13MSarah 
HMTA13Y = HMTA13YSarah 
HMTACY = HMTACYSarah 
HMTAFLAG = HMTAFLAGSarah 
HMTB01 = HMTB01Sarah
HMTB02 = HMTB02Sarah 
HMTB03 = HMTB03Sarah 
HMTB04 = HMTB04Sarah
HMTB05 = HMTB05Sarah 
HMTB06 = HMTB06Sarah 
HMTB07 = HMTB07Sarah 
HMTB08 = HMTB08Sarah 
HMTB09 = HMTB09Sarah 
HMTB10 = HMTB10Sarah 
HMTB11 = HMTB11Sarah 
HMTB12 = HMTB12Sarah 
HMTB13 = HMTB13Sarah 
HMTB14 = HMTB14Sarah 
HMTB15 = HMTB15Sarah 
HMTB14D = HMTB14DSarah 
HMTB14M = HMTB14MSarah 
HMTB14Y = HMTB14YSarah 
HMTBCY = HMTBCYSarah 
HMTBFLAG = HMTBFLAGSarah;

*Outcomes of interest;

*Overall cancer incidence

Lung Cancer incidence
Lung Cancer incidence In Never Smokers

Colorectal Cancer incidence 
Colorectal Cancer incidence Adjusted for Hormone Use

Male Prostate Cancer incidence
Female Breast inciddence Cancer;

*Overall cancer mortality 

Lung Cancer mortality
Lung Cancer mortality In Never Smokers

Colorectal Cancer mortality 
Colorectal Cancer mortality Adjusted for Hormone Use

Male Prostate Cancer mortality
Female Breast mortality Cancer;

*Covariates of interest;

*Study Site
Gender
Race
Age, Mean (SD) 
Education
BMI, Mean (SD) (kg/m2)
Cigarette Use
Alcohol Intake After Removal of Non-Drinkers, Mean (SD) (g/wk) [Alcohol intake among participants who reported usually having at least one drink per week]
CVD [CVD defined as having a history of angina pectoris, coronary heart disease, intermittent claudication, or stroke]
Hypertension [Hypertension defined as use of any hypertensive medications, systolic blood pressure >140 mmHg, or diastolic blood pressure >90 mmHg]
Diabetes [Diabetes defined as having a fasting glucose level >126 mg/dl, non-fasting glusocse level >200 mg/dl, a physician diagnosis of diabetes, or using sugar-lowereing medications in two weeks prior to enrollment]
Aspirin used [Aspirin use in two weeks prior to study enrollment];
 
libname mylib3 'E:\Professional Folder\WBOB\ARIC WBC Project\2_Summer 2017 RA Projects\SAS Datasets';

data wbccancer;
set mylib3.wbccancer;
run;

proc contents data = wbccancer;
run;

*Applying missing center data exclusion criteria (N=15812 > 15792);

proc freq data = wbccancer;
table center;
run;

data wbccancer;
set wbccancer;
if center = " " then delete;
run;

proc freq data = wbccancer;
table center;
run;

*Applying race (Black or white) exclusion criteria (N=15792 > 15744);

proc freq data = wbccancer;
table RACEGRP;
run;

data wbccancer;
set wbccancer;
if RACEGRP = "A" then delete;
if RACEGRP = "I" then delete;
run;

proc freq data = wbccancer;
table RACEGRP;
run;

*Applying cancer history exclusion criteria (N=15744 > 14688);

proc freq data = wbccancer;
table prvcancr;
run;

proc print data = wbccancer;
where prvcancr = .;
var id anycancer;
run;

data wbccancer;
set wbccancer;
if prvcancr ne 0 then delete;
run;

proc freq data = wbccancer;
table prvcancr anycancer cancerdeath;
run;

proc print data = wbccancer;
where NEUTROPHILSV1 = .;
var id WBCV1 NEUTROPHILSV1 BANDSV1 LYMPHOCYTESV1 MONOCYTESV1 EOSINOPHILSV1 BASOPHILSV1;
run;

*Applying the Washington exclusion criteria (apparently the differentials were not done for that center, and when I checkec the missing observation for one of the elements of the differential (neutrophil) the vast majority of those oboservations where from washington;
*Lucas also indicated this in his code, altough interestingly the HTMA ARIC description file did not;
* N = 14688 > 10970;
 
proc freq data = wbccancer;
table center;
run;

data wbccancer;
set wbccancer;
if center = "W" then delete;
run;

proc freq data = wbccancer;
table center;
run;

*Looking at the distribution of WBC indices in our dataset;

proc univariate data = wbccancer;
var WBCV1 NEUTROPHILSV1 BANDSV1 LYMPHOCYTESV1 MONOCYTESV1 EOSINOPHILSV1 BASOPHILSV1;
histogram WBCV1 NEUTROPHILSV1 BANDSV1 LYMPHOCYTESV1 MONOCYTESV1 EOSINOPHILSV1 BASOPHILSV1 / normal;
run;

proc univariate data = wbccancer;
var WBCV2 NEUTROPHILSV2 BANDSV2 LYMPHOCYTESV2 MONOCYTESV2 EOSINOPHILSV2 BASOPHILSV2;
histogram WBCV2 NEUTROPHILSV2 BANDSV2 LYMPHOCYTESV2 MONOCYTESV2 EOSINOPHILSV2 BASOPHILSV2 / normal;
run;

*Checking bands as a potential exclusion criteria;

proc means data = wbccancer n min mean max median std;
where bandsv1 gt 0;
var bandsv1;
run;

data wbccancertest;
set wbccancer;
allneutrophilsv1 = NEUTROPHILSV1 + BANDSV1;
allneutrophilsv2 = NEUTROPHILSV2 + BANDSV2;
run;

proc univariate data = wbccancertest;
var allneutrophilsv1 allneutrophilsv2;
histogram allneutrophilsv1 allneutrophilsv2 / normal;
run;

*Recoding the exposure variables for the analysis;
*When we multiply the differential percentage by the WBC cell count, all of the observations that were coded as "A" even though the differential variables are numeric got set to missing, yay!;

data wbccancer;
set wbccancer;
NEUTROPHILSCOUNTV1 = (NEUTROPHILSV1/100) * WBCV1;
BANDSCOUNTV1 = (BANDSV1/100) * WBCV1;
LYMPHOCYTESCOUNTV1 = (LYMPHOCYTESV1/100) * WBCV1;
MONOCYTESCOUNTV1 = (MONOCYTESV1/100) * WBCV1;
EOSINOPHILSCOUNTV1 = (EOSINOPHILSV1/100) * WBCV1;
BASOPHILSCOUNTV1 = (BASOPHILSV1/100) * WBCV1;
NEUTROPHILSTOTALCOUNTV1 = NEUTROPHILSCOUNTV1 + BANDSCOUNTV1;
GRANULOCYTESCOUNTV1 = NEUTROPHILSTOTALCOUNTV1 + EOSINOPHILSCOUNTV1 + BASOPHILSCOUNTV1;
NLRV1 = NEUTROPHILSTOTALCOUNTV1 / LYMPHOCYTESCOUNTV1;
run;

data wbccancer;
set wbccancer;
NEUTROPHILSCOUNTV2 = (NEUTROPHILSV2/100) * WBCV2;
BANDSCOUNTV2 = (BANDSV2/100) * WBCV2;
LYMPHOCYTESCOUNTV2 = (LYMPHOCYTESV2/100) * WBCV2;
MONOCYTESCOUNTV2 = (MONOCYTESV2/100) * WBCV2;
EOSINOPHILSCOUNTV2 = (EOSINOPHILSV2/100) * WBCV2;
BASOPHILSCOUNTV2 = (BASOPHILSV2/100) * WBCV2;
NEUTROPHILSTOTALCOUNTV2 = NEUTROPHILSCOUNTV2 + BANDSCOUNTV2;
GRANULOCYTESCOUNTV2 = NEUTROPHILSTOTALCOUNTV2 + EOSINOPHILSCOUNTV2 + BASOPHILSCOUNTV2;
NLRV2 = NEUTROPHILSTOTALCOUNTV2 / LYMPHOCYTESCOUNTV2;
run;

proc univariate data = wbccancer;
var WBCV1 NEUTROPHILSTOTALCOUNTV1 NEUTROPHILSCOUNTV1 BANDSCOUNTV1 LYMPHOCYTESCOUNTV1 MONOCYTESCOUNTV1 EOSINOPHILSCOUNTV1 BASOPHILSCOUNTV1 GRANULOCYTESCOUNTV1 NLRV1;
histogram WBCV1 NEUTROPHILSTOTALCOUNTV1 NEUTROPHILSCOUNTV1 BANDSCOUNTV1 LYMPHOCYTESCOUNTV1 MONOCYTESCOUNTV1 EOSINOPHILSCOUNTV1 BASOPHILSCOUNTV1 GRANULOCYTESCOUNTV1 NLRV1 / normal;
run;

proc univariate data = wbccancer;
var WBCV2 NEUTROPHILSTOTALCOUNTV2 NEUTROPHILSCOUNTV2 BANDSCOUNTV2 LYMPHOCYTESCOUNTV2 MONOCYTESCOUNTV2 EOSINOPHILSCOUNTV2 BASOPHILSCOUNTV2 GRANULOCYTESCOUNTV2 NLRV2;
histogram WBCV2 NEUTROPHILSTOTALCOUNTV2 NEUTROPHILSCOUNTV2 BANDSCOUNTV2 LYMPHOCYTESCOUNTV2 MONOCYTESCOUNTV2 EOSINOPHILSCOUNTV2 BASOPHILSCOUNTV2 GRANULOCYTESCOUNTV2 NLRV2 / normal;
run;

* This is how the categories for our exposure variables of interest were made;

data wbccancerblack;
set wbccancer;
where RACEGRP='B';
run;

*tertiles for blacks;
proc rank data=wbccancerblack groups=3 out=wbccancerblack;
var WBCV1 NEUTROPHILSTOTALCOUNTV1 NEUTROPHILSCOUNTV1 BANDSCOUNTV1 LYMPHOCYTESCOUNTV1 MONOCYTESCOUNTV1 EOSINOPHILSCOUNTV1 BASOPHILSCOUNTV1 GRANULOCYTESCOUNTV1 NLRV1; 
ranks WBCV1TERT NEUTROPHILSTOTALCOUNTV1TERT NEUTROPHILSCOUNTV1TERT BANDSCOUNTV1TERT LYMPHOCYTESCOUNTV1TERT MONOCYTESCOUNTV1TERT EOSINOPHILSCOUNTV1TERT BASOPHILSCOUNTV1TERT GRANULOCYTESCOUNTV1TERT NLRV1TERT;
run;

*Quartiles for blacks;
proc rank data=wbccancerblack groups=4 out=wbccancerblack;
var WBCV1 NEUTROPHILSTOTALCOUNTV1 NEUTROPHILSCOUNTV1 BANDSCOUNTV1 LYMPHOCYTESCOUNTV1 MONOCYTESCOUNTV1 EOSINOPHILSCOUNTV1 BASOPHILSCOUNTV1 GRANULOCYTESCOUNTV1 NLRV1; 
ranks WBCV1QUART NEUTROPHILSTOTALCOUNTV1QUART NEUTROPHILSCOUNTV1QUART BANDSCOUNTV1QUART LYMPHOCYTESCOUNTV1QUART MONOCYTESCOUNTV1QUART EOSINOPHILSCOUNTV1QUART BASOPHILSCOUNTV1QUART GRANULOCYTESCOUNTV1QUART NLRV1QUART;
run;

*Quintiles for blacks;
proc rank data=wbccancerblack groups=5 out=wbccancerblack;
var WBCV1 NEUTROPHILSTOTALCOUNTV1 NEUTROPHILSCOUNTV1 BANDSCOUNTV1 LYMPHOCYTESCOUNTV1 MONOCYTESCOUNTV1 EOSINOPHILSCOUNTV1 BASOPHILSCOUNTV1 GRANULOCYTESCOUNTV1 NLRV1; 
ranks WBCV1QUINT NEUTROPHILSTOTALCOUNTV1QUINT NEUTROPHILSCOUNTV1QUINT BANDSCOUNTV1QUINT LYMPHOCYTESCOUNTV1QUINT MONOCYTESCOUNTV1QUINT EOSINOPHILSCOUNTV1QUINT BASOPHILSCOUNTV1QUINT GRANULOCYTESCOUNTV1QUINT NLRV1QUINT;
run;

data wbccancerwhite;
set wbccancer;
where RACEGRP='W';
run;

*tertiles for whites;
proc rank data=wbccancerwhite groups=3 out=wbccancerwhite;
var WBCV1 NEUTROPHILSTOTALCOUNTV1 NEUTROPHILSCOUNTV1 BANDSCOUNTV1 LYMPHOCYTESCOUNTV1 MONOCYTESCOUNTV1 EOSINOPHILSCOUNTV1 BASOPHILSCOUNTV1 GRANULOCYTESCOUNTV1 NLRV1; 
ranks WBCV1TERT NEUTROPHILSTOTALCOUNTV1TERT NEUTROPHILSCOUNTV1TERT BANDSCOUNTV1TERT LYMPHOCYTESCOUNTV1TERT MONOCYTESCOUNTV1TERT EOSINOPHILSCOUNTV1TERT BASOPHILSCOUNTV1TERT GRANULOCYTESCOUNTV1TERT NLRV1TERT;
run;

*Quartiles for whites;
proc rank data=wbccancerwhite groups=4 out=wbccancerwhite;
var WBCV1 NEUTROPHILSTOTALCOUNTV1 NEUTROPHILSCOUNTV1 BANDSCOUNTV1 LYMPHOCYTESCOUNTV1 MONOCYTESCOUNTV1 EOSINOPHILSCOUNTV1 BASOPHILSCOUNTV1 GRANULOCYTESCOUNTV1 NLRV1; 
ranks WBCV1QUART NEUTROPHILSTOTALCOUNTV1QUART NEUTROPHILSCOUNTV1QUART BANDSCOUNTV1QUART LYMPHOCYTESCOUNTV1QUART MONOCYTESCOUNTV1QUART EOSINOPHILSCOUNTV1QUART BASOPHILSCOUNTV1QUART GRANULOCYTESCOUNTV1QUART NLRV1QUART;
run;

*Quintiles for whites;
proc rank data=wbccancerwhite groups=5 out=wbccancerwhite;
var WBCV1 NEUTROPHILSTOTALCOUNTV1 NEUTROPHILSCOUNTV1 BANDSCOUNTV1 LYMPHOCYTESCOUNTV1 MONOCYTESCOUNTV1 EOSINOPHILSCOUNTV1 BASOPHILSCOUNTV1 GRANULOCYTESCOUNTV1 NLRV1; 
ranks WBCV1QUINT NEUTROPHILSTOTALCOUNTV1QUINT NEUTROPHILSCOUNTV1QUINT BANDSCOUNTV1QUINT LYMPHOCYTESCOUNTV1QUINT MONOCYTESCOUNTV1QUINT EOSINOPHILSCOUNTV1QUINT BASOPHILSCOUNTV1QUINT GRANULOCYTESCOUNTV1QUINT NLRV1QUINT;
run;

*Recombining the datasets together;
data wbccancermkI;
set wbccancerblack wbccancerwhite;
run;

*Let's remove the ends of the distribution based on the 68 (1SD) 95 (2SD) and 99.7 (3SD rule);

proc sort data = wbccancermkI; by RACEGRP; run;

proc means data = wbccancermkI n mean std;
var WBCV1;
by RACEGRP;
run;

*In the previous analysis, all values below 2SD and above 2SD were excluded;  
*For blacks we have 3879 5.6677752 1.9729980 so the exclusion criteria will be WBC lt 1.7217792 and WBC gt 9.6137712;
*For whites we have 6868 6.3154193 1.8754815 so the exclusion criteria will be WBC lt 2.5644563 and WBC gt 10.0663823;

*In the approach above we were trying to combine based on distributions of both the WBC count overall Normal Range, and the distribution of each differential indices as a percentage;
*However, I am not super comfortable with this definition because some individual may have high WBC and low lymphs or vice versa, so our categories for differential counts (not percents) may not match;
*Now we cut the WBC indices off at Q1 and Q99 based on proc univariate to remove counts due to occult disease; 
*then we establish the +/- 2SD as the normal range based on the litterature Dr. Tang shared with us;
*then we are remaking the exposure variable by categorizing values as below NR, within NR (Tertiles) and above NR for each differential indices;

*In this version of the analysis, we will exclude values below Q1 and above Q99 for lymphocytes, then we will categorise values as below NR, within NR (Tertiles) and above NR;  

data wbccancermkII;
set wbccancermkI;
NEUTROPHILSCOUNTV1 = (NEUTROPHILSV1/100) * WBCV1;
BANDSCOUNTV1 = (BANDSV1/100) * WBCV1;
LYMPHOCYTESCOUNTV1 = (LYMPHOCYTESV1/100) * WBCV1;
MONOCYTESCOUNTV1 = (MONOCYTESV1/100) * WBCV1;
EOSINOPHILSCOUNTV1 = (EOSINOPHILSV1/100) * WBCV1;
BASOPHILSCOUNTV1 = (BASOPHILSV1/100) * WBCV1;
NEUTROPHILSTOTALCOUNTV1 = NEUTROPHILSCOUNTV1 + BANDSCOUNTV1;
GRANULOCYTESCOUNTV1 = NEUTROPHILSTOTALCOUNTV1 + EOSINOPHILSCOUNTV1 + BASOPHILSCOUNTV1;
NLRV1 = NEUTROPHILSTOTALCOUNTV1 / LYMPHOCYTESCOUNTV1;
NEUTROPHILSTOTALV1 = NEUTROPHILSV1 + BANDSV1;
run;

proc sort data = wbccancermkII; by RACEGRP; run;

proc means data = wbccancermkII n mean std;
var LYMPHOCYTESCOUNTV1;
by RACEGRP;
run;

proc univariate data = wbccancermkII;
var WBCV1 LYMPHOCYTESV1 LYMPHOCYTESCOUNTV1;
histogram WBCV1 LYMPHOCYTESV1 LYMPHOCYTESCOUNTV1 / normal;
by RACEGRP;
run;

*For blacks q1 = 0.86800 q99 = 4.45200 based on LYMPHOCYTESCOUNTV1;
*For whites q1 = 0.84000 q99 = 3.69600 based on LYMPHOCYTESCOUNTV1;

data LYMPHOCYTES;
set wbccancermkII;
if RACEGRP = "B" and LYMPHOCYTESCOUNTV1 lt 0.86800 then delete;
if RACEGRP = "B" and LYMPHOCYTESCOUNTV1 gt 4.45200 then delete;
if RACEGRP = "W" and LYMPHOCYTESCOUNTV1 lt 0.84000 then delete;
if RACEGRP = "W" and LYMPHOCYTESCOUNTV1 gt 3.69600 then delete;
run;

proc univariate data = LYMPHOCYTES;
var LYMPHOCYTESCOUNTV1;
histogram LYMPHOCYTESCOUNTV1 / normal;
output out=UniWidePctls pctlpre=LYMPHOCYTESCOUNTV1P_ pctlpts=2.5,97.5;
by RACEGRP;
run; 

proc print data=UniWidePctls noobs; run;


*For blacks LYMPHOCYTESCOUNTV1 we have 3790 2.144719 0.67183249 and the pct are 1.08 3.685;
*For whites LYMPHOCYTESCOUNTV1 we have 6732 1.87014676 0.53292811 and the pct are 1.02	3.135;

*Lymphocyte indices for blacks;

data wbccancerblacklymphocytes;
set LYMPHOCYTES(keep = ID RACEGRP LYMPHOCYTESCOUNTV1);
where RACEGRP='B' and 1.02 lt LYMPHOCYTESCOUNTV1 lt 3.685;
run;

proc rank data=wbccancerblacklymphocytes groups=3 out=wbccancerblacklymphocytes;
var LYMPHOCYTESCOUNTV1; 
ranks LYMPHOCYTESCOUNTV1TERTNRB;
run;

*Lymphocyte indices for whites;

data wbccancerwhitelymphocytes;
set LYMPHOCYTES (keep = ID RACEGRP LYMPHOCYTESCOUNTV1);
where RACEGRP='W' and 1.02 lt LYMPHOCYTESCOUNTV1 lt 3.135;
run;

proc rank data=wbccancerwhitelymphocytes groups=3 out=wbccancerwhitelymphocytes;
var LYMPHOCYTESCOUNTV1; 
ranks LYMPHOCYTESCOUNTV1TERTNRW;
run;

*Combining all the differential indices together;

proc sort data = wbccancerblacklymphocytes; by ID; run;
proc sort data = LYMPHOCYTES; by ID; run;

data wbccancerblack;
merge LYMPHOCYTES wbccancerblacklymphocytes;
by ID;
run;

proc print data = wbccancerblack;
where LYMPHOCYTESCOUNTV1 = .;
run;

proc sort data = wbccancerwhitelymphocytes; by ID; run;
proc sort data = LYMPHOCYTES; by ID; run;

data wbccancerwhite;
merge LYMPHOCYTES wbccancerwhitelymphocytes;
by ID;
run;

proc print data = wbccancerwhite;
where LYMPHOCYTESCOUNTV1 = .;
run;

proc sort data = wbccancerblack; by ID; run;
proc sort data = wbccancerwhite; by ID; run;

data LYMPHOCYTES;
merge wbccancerblack wbccancerwhite;
by ID;
run;

proc print data = LYMPHOCYTES;
where LYMPHOCYTESCOUNTV1 = .;
run;

*For blacks LYMPHOCYTESCOUNTV1 we have 3790 2.144719 0.67183249 and the pct are 1.08 3.685;

data LYMPHOCYTESblack;
set LYMPHOCYTES;
where RACEGRP = "B";
if LYMPHOCYTESCOUNTV1 = . then LYMPHOCYTESV1CAT = .;
else if LYMPHOCYTESCOUNTV1 le 1.08 then LYMPHOCYTESV1CAT = 0;
else if LYMPHOCYTESCOUNTV1 ge 3.685 then LYMPHOCYTESV1CAT = 4;
else LYMPHOCYTESV1CAT = LYMPHOCYTESCOUNTV1TERTNRB + 1;
run;

proc freq data = LYMPHOCYTESblack;
tables LYMPHOCYTESV1CAT;
run;

proc sort data = LYMPHOCYTESblack; by LYMPHOCYTESV1CAT; run;

proc means data = LYMPHOCYTESblack n min mean max;
var LYMPHOCYTESCOUNTV1;
by LYMPHOCYTESV1CAT;
run;

*For whites LYMPHOCYTESCOUNTV1 we have 6732 1.87014676 0.53292811 and the pct are 1.02	3.135;

data LYMPHOCYTESwhite;
set LYMPHOCYTES;
where RACEGRP = "W";
if LYMPHOCYTESCOUNTV1 = . then LYMPHOCYTESV1CAT = .;
else if LYMPHOCYTESCOUNTV1 le 1.02 then LYMPHOCYTESV1CAT = 0;
else if LYMPHOCYTESCOUNTV1 ge 3.135 then LYMPHOCYTESV1CAT = 4;
else LYMPHOCYTESV1CAT = LYMPHOCYTESCOUNTV1TERTNRW + 1;
run;

proc freq data = LYMPHOCYTESwhite;
tables LYMPHOCYTESV1CAT;
run;

proc sort data = LYMPHOCYTESwhite; by LYMPHOCYTESV1CAT; run;

proc means data = LYMPHOCYTESwhite n min mean max;
var LYMPHOCYTESCOUNTV1;
by LYMPHOCYTESV1CAT;
run;

data LYMPHOCYTES;
set LYMPHOCYTESblack LYMPHOCYTESwhite;
run;

proc freq data = LYMPHOCYTES;
tables LYMPHOCYTESV1CAT;
run;

proc sort data = LYMPHOCYTES; by LYMPHOCYTESV1CAT; run;

proc means data = LYMPHOCYTES n min mean max;
var LYMPHOCYTESCOUNTV1;
by LYMPHOCYTESV1CAT;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Neutrophils;

*In the previous analysis, all values below 2SD and above 2SD were excluded;  
*For blacks we have 3879 5.6677752 1.9729980 so the exclusion criteria will be WBC lt 1.7217792 and WBC gt 9.6137712;
*For whites we have 6868 6.3154193 1.8754815 so the exclusion criteria will be WBC lt 2.5644563 and WBC gt 10.0663823;

*In the approach above we were trying to combine based on distributions of both the WBC count overall Normal Range, and the distribution of each differential indices as a percentage;
*However, I am not super comfortable with this definition because some individual may have high WBC and low lymphs or vice versa, so our categories for differential counts (not percents) may not match;
*Now we cut the WBC indices (aka the absolute off at Q1 and Q99 based on proc univariate to remove counts due to occult disease; 
*then we establish the +/- 2SD as the normal range based on the litterature Dr. Tang shared with us;
*then we are remaking the exposure variable by categorizing values as below NR, within NR (Tertiles) and above NR for each differential indices;

*In this version of the analysis, we will exclude values below Q1 and above Q99 for lymphocytes and neutrophils, then we will categorise values as below NR, within NR (Tertiles) and above NR;  

proc sort data = wbccancermkII; by RACEGRP; run;

proc means data = wbccancermkII n mean std;
var NEUTROPHILSCOUNTV1;
by RACEGRP;
run;

proc univariate data = wbccancermkII;
var WBCV1 NEUTROPHILSV1 NEUTROPHILSCOUNTV1;
histogram WBCV1 NEUTROPHILSV1 NEUTROPHILSCOUNTV1 / normal;
by RACEGRP;
run;

*For blacks q1 = 0.66000 q99 = 7.54000 based on NEUTROPHILSCOUNTV1;
*For whites q1 = 1.58100 q99 = 8.75000 based on NEUTROPHILSCOUNTV1;

data NEUTROPHILS;
set wbccancermkII(keep = ID RACEGRP WBCV1 NEUTROPHILSCOUNTV1);
if RACEGRP = "B" and NEUTROPHILSCOUNTV1 lt 0.66000 then delete;
if RACEGRP = "B" and NEUTROPHILSCOUNTV1 gt 7.54000 then delete;
if RACEGRP = "W" and NEUTROPHILSCOUNTV1 lt 1.58100 then delete;
if RACEGRP = "W" and NEUTROPHILSCOUNTV1 gt 8.75000 then delete;
run;

proc univariate data = NEUTROPHILS;
var NEUTROPHILSCOUNTV1;
histogram NEUTROPHILSCOUNTV1 / normal;
output out=UniWidePctls pctlpre=NEUTROPHILSCOUNTV1P_ pctlpts=2.5,97.5;
by RACEGRP;
run; 

proc print data=UniWidePctls noobs; run;

*For blacks NEUTROPHILSCOUNTV1 we have 3790 2.73299789 1.27877851 and the pct are 0.945 5.952;
*For whites NEUTROPHILSCOUNTV1 we have 6732 3.853565 1.316786 and the pct are 1.927	7.140;

*NEUTROPHILS indices for blacks;

data NEUTROPHILSblack;
set NEUTROPHILS(keep = ID RACEGRP NEUTROPHILSCOUNTV1);
where RACEGRP='B' and 0.945 lt NEUTROPHILSCOUNTV1 lt 5.952;
run;

proc rank data=NEUTROPHILSblack groups=3 out=wbccancerblackNEUTROPHILS;
var NEUTROPHILSCOUNTV1; 
ranks NEUTROPHILSCOUNTV1TERTNRB;
run;

*NEUTROPHILS indices for whites;

data NEUTROPHILSwhite;
set NEUTROPHILS(keep = ID RACEGRP NEUTROPHILSCOUNTV1);
where RACEGRP='W' and 1.927 lt NEUTROPHILSCOUNTV1 lt 7.140;
run;

proc rank data=NEUTROPHILSwhite groups=3 out=NEUTROPHILSwhite;
var NEUTROPHILSCOUNTV1; 
ranks NEUTROPHILSCOUNTV1TERTNRW;
run;

*Combining all the differential indices together;

proc sort data = NEUTROPHILSblack; by ID; run;
proc sort data = NEUTROPHILS; by ID; run;

data NEUTROPHILS;
merge NEUTROPHILS NEUTROPHILSblack;
by ID;
run;

proc sort data = NEUTROPHILSwhite; by ID; run;
proc sort data = NEUTROPHILS; by ID; run;

data NEUTROPHILS;
merge NEUTROPHILS NEUTROPHILSwhite;
by ID;
run;

*For blacks NEUTROPHILSCOUNTV1 we have 3790 2.73299789 1.27877851 and the pct are 0.945 5.952;

data NEUTROPHILSblack;
set NEUTROPHILS;
where RACEGRP = "B";
if NEUTROPHILSCOUNTV1 = . then NEUTROPHILSV1CAT = .;
else if NEUTROPHILSCOUNTV1 le 0.945 then NEUTROPHILSV1CAT = 0;
else if NEUTROPHILSCOUNTV1 ge 5.952 then NEUTROPHILSV1CAT = 4;
else NEUTROPHILSV1CAT = NEUTROPHILSCOUNTV1TERTNRB + 1;
run;

proc freq data = NEUTROPHILSblack;
tables NEUTROPHILSV1CAT;
run;

proc sort data = NEUTROPHILSblack; by NEUTROPHILSV1CAT; run;

proc means data = NEUTROPHILSblack n min mean max;
var NEUTROPHILSCOUNTV1;
by NEUTROPHILSV1CAT;
run;

*For whites NEUTROPHILSCOUNTV1 we have 6732 3.853565 1.316786 and the pct are 1.927	7.140;

data NEUTROPHILSwhite;
set NEUTROPHILS;
where RACEGRP = "W";
if NEUTROPHILSCOUNTV1 = . then NEUTROPHILSV1CAT = .;
else if NEUTROPHILSCOUNTV1 le 1.927 then NEUTROPHILSV1CAT = 0;
else if NEUTROPHILSCOUNTV1 ge 7.140 then NEUTROPHILSV1CAT = 4;
else NEUTROPHILSV1CAT = NEUTROPHILSCOUNTV1TERTNRW + 1;
run;

proc freq data = NEUTROPHILSwhite;
tables NEUTROPHILSV1CAT;
run;

proc sort data = NEUTROPHILSwhite; by NEUTROPHILSV1CAT; run;

proc means data = NEUTROPHILSwhite n min mean max;
var NEUTROPHILSCOUNTV1;
by NEUTROPHILSV1CAT;
run;

data NEUTROPHILS;
set NEUTROPHILSblack NEUTROPHILSwhite;
run;

proc freq data = NEUTROPHILS;
tables NEUTROPHILSV1CAT;
run;

proc sort data = NEUTROPHILS; by NEUTROPHILSV1CAT; run;

proc means data = NEUTROPHILS n min mean max;
var NEUTROPHILSCOUNTV1;
by NEUTROPHILSV1CAT;
run;

proc print data = NEUTROPHILS;
where NEUTROPHILSV1CAT = .;
var ID RACEGRP NEUTROPHILSCOUNTV1;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Merging to main dataset;

proc sort data = LYMPHOCYTES; by ID; run;

proc contents data = NEUTROPHILS; run;

data NEUTROPHILS;
set NEUTROPHILS (KEEP = ID NEUTROPHILSV1CAT) ;
run;

proc sort data = NEUTROPHILS; by ID; run;

data wbccancermkIII;
merge LYMPHOCYTES NEUTROPHILS;
by ID;
run;

proc contents data = wbccancermkIII;
run;

proc corr data = wbccancermkIII;
var WBCV1 LYMPHOCYTESCOUNTV1 NEUTROPHILSTOTALCOUNTV1;
run;

data mylib3.wbccancermkI;
set wbccancermkIII;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

data mylib3.wbccancermkI;
set LYMPHOCYTES;
run;

proc corr data = mylib3.wbccancermkI;
var WBCV1 LYMPHOCYTESCOUNTV1 NEUTROPHILSTOTALCOUNTV1;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

libname mylibmkI 'E:\Professional Folder\WBOB\ARIC WBC Project\2_Summer 2017 RA Projects\SAS Datasets';

data wbccancermkI;
set mylibmkI.wbccancermkI;
run;

proc contents data = wbccancermkI;
run;

*Descriptive Statistics (use the variable with the * by the name);

*Age (V1AGE01)

Study center (center) 

Sex (gender) 

Race (racegrp) 

Education (elevel01 elevel02*) 

Drinking (EVRDRK01 FORDRK01 CURDRK02 DRNKR01 ETHANL03*) and I created DRINKING*

Smoking (EVRSMK01 FORSMK01 CURSMK01 CIGTYR01* CIGT01 CIGR01 CIGRYR01) and I created SMOKING*

Hormone Replacement Therapy (HORMON01 HORMON02 RHXA16) and I created HORMONEUSE*

Menopause (RHXA07*) and I created MENOPAUSE*

CVD (HYPERT05 STROKE01 PRVCHD05 RANGNA01) and I created CVD*

BMI (BMI01)

Inflammation (CRP4, Aspirin*) I created CRPCAT*

Diabetes (DIABETES DIABTS02* DIABTS03)

Physical Activity (SPRT_I02);

proc means data = wbccancermkI;
var V1AGE01;
run;

proc freq data = wbccancermkI;
tables center gender racegrp;
run;
 
proc freq data = wbccancermkI;
tables elevel01 elevel02;
run;

proc freq data = wbccancermkI;
tables EVRDRK01 FORDRK01 CURDRK02 DRNKR01;
run;

data wbccancermkI;
set wbccancermkI;
if EVRDRK01 = 0 then DRINKING = 0;
else if FORDRK01 = 1 then DRINKING = 1;
else if CURDRK02 = 1 then DRINKING = 2;
run;

proc freq data = wbccancermkI;
tables EVRDRK01 FORDRK01 CURDRK02 DRNKR01 DRINKING;
run;

proc means data = wbccancermkI;
var ETHANL03;
run;
 
proc freq data = wbccancermkI;
tables EVRSMK01 FORSMK01 CURSMK01 CIGT01 CIGR01;
run;

data wbccancermkI;
set wbccancermkI;
if EVRSMK01 = 0 then SMOKING = 0;
else if FORSMK01 = 1 then SMOKING = 1;
else if CURSMK01 = 1 then SMOKING = 2;
run;

proc freq data = wbccancermkI;
tables EVRSMK01 FORSMK01 CURSMK01 CIGT01 CIGR01 SMOKING;
run;

proc means data = wbccancermkI;
var CIGTYR01 CIGRYR01;
run;
 
proc freq data = wbccancermkI;
tables HORMON01 HORMON02 RHXA16;
run;

data wbccancermkI;
set wbccancermkI;
if HORMON02 = 3 then HORMONEUSE = 1;
else HORMONEUSE = 0;
run;

proc freq data = wbccancermkI;
tables HORMON01 HORMON02 RHXA16 RHXA07 HORMONEUSE;
run;

proc freq data = wbccancermkI;
tables RHXA07;
run;

data wbccancermkI;
set wbccancermkI;
if RHXA07 = "Y" then MENOPAUSE = 1;
else MENOPAUSE = 0;
run;

proc freq data = wbccancermkI;
tables RHXA07 MENOPAUSE MENOPAUSE*GENDER;
run;

proc freq data = wbccancermkI;
tables HYPERT05 STROKE01 PRVCHD05 RANGNA01;
run;

data wbccancermkI;
set wbccancermkI;
if HYPERT05 = 1 then CVD = 1;
else if STROKE01 = "Y" then CVD = 2;
else if PRVCHD05 = 1 then CVD = 2;
else if RANGNA01 = 1 then CVD = 2;
else CVD = 0;
run;

proc freq data = wbccancermkI;
tables HYPERT05 STROKE01 PRVCHD05 RANGNA01 CVD;
run;

proc means data = wbccancermkI;
var BMI01;
run;

proc means data = wbccancermkI;
var CRP4; *need to code this as a categorical variable so that we do not loose observations in the model;
run;

proc rank data=wbccancermkI groups=4 out=wbccancermkI;
var CRP4; 
ranks CRP4QUART;
run;

proc freq data = wbccancermkI;
tables CRP4QUART;
run;

data wbccancermkI;
set wbccancermkI;
if CRP4QUART = . then CRPCAT = 0;
else if CRP4QUART = 0 then CRPCAT = 1;
if CRP4QUART = 1 then CRPCAT = 2;
if CRP4QUART = 2 then CRPCAT = 3;
if CRP4QUART = 3 then CRPCAT = 4;
run;

proc freq data = wbccancermkI;
tables CRP4QUART CRPCAT;
run;

proc freq data = wbccancermkI;
tables ASPIRINC;
run;

proc freq data = wbccancermkI;
tables DIABETES DIABTS02 DIABTS03;
run;

proc freq data = wbccancermkI;
tables SPRT_I02;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Exposures of interest (Tertiles > Quartiles > Quintiles / Continuous > Continuous per SD > Splines);

*WBCV1
GRANULOCYTESCOUNTV1
NEUTROPHILSTOTALCOUNTV1
LYMPHOCYTESCOUNTV1
NLRV1;

*Outcomes of interest;

*Overall cancer incidence

Lung Cancer incidence
Lung Cancer incidence In Never Smokers

Colorectal Cancer incidence 
Colorectal Cancer incidence Adjusted for Hormone Use

Male Prostate Cancer incidence
Female Breast inciddence Cancer;

*Overall cancer mortality 

Lung Cancer mortality
Lung Cancer mortality In Never Smokers

Colorectal Cancer mortality 
Colorectal Cancer mortality Adjusted for Hormone Use

Male Prostate Cancer mortality
Female Breast mortality Cancer;

*Statistical Models;

*Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status;

*Summary of the variables to use in remaining analyses;

proc contents data = wbccancermkI;
run;

proc means data = wbccancermkI;
var V1AGE01 ETHANL03 CIGTYR01 BMI01;
run;

proc freq data = wbccancermkI;
tables CENTER GENDER RACEGRP ELEVEL02 DRINKING SMOKING HORMONEUSE ASPIRINC MENOPAUSE CVD DIABTS02 SPRT_I02 CRPCAT;
run;

proc means data = wbccancermkI;
var WBCV1 NEUTROPHILSTOTALCOUNTV1 NEUTROPHILSCOUNTV1 BANDSCOUNTV1 LYMPHOCYTESCOUNTV1 MONOCYTESCOUNTV1 EOSINOPHILSCOUNTV1 BASOPHILSCOUNTV1 GRANULOCYTESCOUNTV1 NLRV1; 
run;

proc freq data = wbccancermkI;
tables WBCV1TERT NEUTROPHILSTOTALCOUNTV1TERT NEUTROPHILSCOUNTV1TERT BANDSCOUNTV1TERT LYMPHOCYTESCOUNTV1TERT MONOCYTESCOUNTV1TERT EOSINOPHILSCOUNTV1TERT BASOPHILSCOUNTV1TERT GRANULOCYTESCOUNTV1TERT NLRV1TERT;
run;

proc freq data = wbccancermkI;
tables WBCV1CAT NEUTROPHILSTOTALV1CAT LYMPHOCYTESV1CAT MONOCYTESV1CAT EOSINOPHILSV1CAT BASOPHILSV1CAT;
run;

data wbccancermkI;
set wbccancermkI;
if anycancer = 1 then anycancerfu = (dxdate_1 - V1DATE01)/365.2; else anycancerfu = 26.1;
cancerdeathfu = (deathcensdt - V1DATE01)/365.2;
run;

proc means data = wbccancermkI;
var anycancerfu cancerdeathfu;
run;

proc freq data = wbccancermkI;
tables anycancer cancerdeath breastcancer12 breastdeath12 colorectalcancer12 colorectaldeath12 lungcancer12 lungdeath12 prostatecancer12 prostatedeath12;
run;

proc means data = wbccancermkI;
var anycancerfu cancerdeathfu breastfu1 breastDeathfu1 colorectalfu1 colorectalDeathfu1 lungfu1 lungDeathfu1 prostatefu1 prostateDeathfu1;
run;

proc corr data = wbccancermkI;
var WBCV1 LYMPHOCYTESCOUNTV1 NEUTROPHILSTOTALCOUNTV1;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Cancer Incidence;

*****************************************************************************************************************************************************************************************;

*Lymphocytes Table;

*Overall cancer incidence;

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(anycancerfu);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT /rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*SMOKING/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER/rl;
run;

*Lung cancer incidence (lungfu1*lungcancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(lungfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

*proc phreg data=wbccancermkI;
*class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
*model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
*run;

*proc phreg data=wbccancermkI;
*class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESCOUNTV1QUINT(ref='0');
*model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESCOUNTV1QUINT/rl;
*run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT/rl;
run;

*Colorectal cancer (colorectalfu1*colorectalcancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(colorectalfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT/rl;
run;

*Colorectal cancer adjusted for hormone therapy use (colorectalfu1*colorectalcancer12);

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT/rl;
run;

*Male Prostate cancer (prostatefu1*prostatecancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(prostatefu1);
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT/rl;
run;

*Female breast cancer(breastfu1*breastcancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(breastfu1);
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='1') LYMPHOCYTESV1CAT(ref='1');
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Cancer mortality;

*****************************************************************************************************************************************************************************************;

*Lymphocytes Table;

*Overall cancer mortality;

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(cancerdeathfu);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*SMOKING/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0')CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='1') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER/rl;
run;

*Lung cancer mortality (lungDeathfu1*lungdeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(lungDeathfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*SMOKING/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT/rl;
run;

*Colorectal cancer (colorectalDeathfu1*colorectaldeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(colorectalDeathfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT/rl;
run;

*Colorectal cancer adjusted for hormone therapy use (colorectalDeathfu1*colorectaldeath12);

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT/rl;
run;

*Male Prostate cancer (prostateDeathfu1*prostatedeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(prostateDeathfu1);
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT/rl;
run;

*Female breast cancer(breastDeathfu1*breastdeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(breastDeathfu1);
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='1') LYMPHOCYTESV1CAT(ref='1');
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Reporting Cancer cases and total person years for the analysis tables;

*Overall cancer incidence;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var anycancer anycancerfu;
tables (LYMPHOCYTESV1CAT all),(anycancer anycancerfu)*sum;
title 'Overall cancer incidence';
run;

*Overall cancer incidence by gender;

proc sort data = wbccancermkI;
by gender;
run;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var anycancer anycancerfu;
tables (LYMPHOCYTESV1CAT all),(anycancer anycancerfu)*sum;
by gender;
title 'Overall cancer incidence by gender';
run;

proc tabulate data = wbccancermkI;
where SMOKING = 2;
class LYMPHOCYTESV1CAT; 
var anycancer anycancerfu;
tables (LYMPHOCYTESV1CAT all),(anycancer anycancerfu)*sum;
by gender;
title 'Overall cancer incidence by gender among smokers';
run;


*Overall cancer incidence by race;

proc sort data = wbccancermkI;
by RACEGRP;
run;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var anycancer anycancerfu;
tables (LYMPHOCYTESV1CAT all),(anycancer anycancerfu)*sum;
by RACEGRP;
title 'Overall cancer incidence by race';
run;

*Overall cancer incidence by smoking status;

proc sort data = wbccancermkI;
by SMOKING;
run;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var anycancer anycancerfu;
tables (LYMPHOCYTESV1CAT all),(anycancer anycancerfu)*sum;
by SMOKING;
title 'Overall cancer incidence by smoking status';
run;

*Lung cancer incidence;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var lungfu1 lungcancer12;
tables (LYMPHOCYTESV1CAT all),(lungfu1 lungcancer12)*sum;
title 'Lung cancer incidence';
run;

proc tabulate data = wbccancermkI;
where SMOKING = 2;
class LYMPHOCYTESV1CAT; 
var lungfu1 lungcancer12;
tables (LYMPHOCYTESV1CAT all),(lungfu1 lungcancer12)*sum;
title 'Lung cancer incidence';
run;

*Colorectal cancer incidence;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var colorectalfu1 colorectalcancer12;
tables (LYMPHOCYTESV1CAT all),(colorectalfu1 colorectalcancer12)*sum;
title 'Colorectal cancer incidence';
run;

*Prostate cancer incidence;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var prostatefu1 prostatecancer12;
tables (LYMPHOCYTESV1CAT all),(prostatefu1 prostatecancer12)*sum;
title 'Prostate cancer incidence';
run;

*Breast cancer incidence;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var breastfu1 breastcancer12;
tables (LYMPHOCYTESV1CAT all),(breastfu1 breastcancer12)*sum;
title 'Breast cancer incidence';
run;

*Reporting Cancer cases and total person years for the analysis tables;

*Overall cancer mortality;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var cancerdeathfu cancerdeath;
tables (LYMPHOCYTESV1CAT all),(cancerdeathfu cancerdeath)*sum;
title 'Overall cancer mortality';
run;

*Overall cancer incidence by gender;

proc sort data = wbccancermkI;
by gender;
run;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var cancerdeathfu cancerdeath;
tables (LYMPHOCYTESV1CAT all),(cancerdeathfu cancerdeath)*sum;
by gender;
title 'Overall cancer mortality by gender';
run;

proc tabulate data = wbccancermkI;
where SMOKING = 2;
class LYMPHOCYTESV1CAT; 
var cancerdeathfu cancerdeath;
tables (LYMPHOCYTESV1CAT all),(cancerdeathfu cancerdeath)*sum;
by gender;
title 'Overall cancer mortality by gender among smokers';
run;

*Overall cancer incidence by race;

proc sort data = wbccancermkI;
by RACEGRP;
run;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var cancerdeathfu cancerdeath;
tables (LYMPHOCYTESV1CAT all),(cancerdeathfu cancerdeath)*sum;
by RACEGRP;
title 'Overall cancer mortality by race';
run;

*Overall cancer incidence by smoking status;

proc sort data = wbccancermkI;
by SMOKING;
run;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var cancerdeathfu cancerdeath;
tables (LYMPHOCYTESV1CAT all),(cancerdeathfu cancerdeath)*sum;
by SMOKING;
title 'Overall cancer mortality by smoking status';
run;

*Lung cancer mortality;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var lungDeathfu1 lungdeath12;
tables (LYMPHOCYTESV1CAT all),(lungDeathfu1 lungdeath12)*sum;
title 'Lung cancer mortality';
run;

proc tabulate data = wbccancermkI;
where SMOKING ne 1;
class LYMPHOCYTESV1CAT; 
var lungDeathfu1 lungdeath12;
tables (LYMPHOCYTESV1CAT all),(lungDeathfu1 lungdeath12)*sum;
title 'Lung cancer mortality';
run;

proc tabulate data = wbccancermkI;
where SMOKING = 2;
class LYMPHOCYTESV1CAT; 
var lungDeathfu1 lungdeath12;
tables (LYMPHOCYTESV1CAT all),(lungDeathfu1 lungdeath12)*sum;
title 'Lung cancer mortality';
run;

*Colorectal cancer mortality;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var colorectalDeathfu1 colorectaldeath12;
tables (LYMPHOCYTESV1CAT all),(colorectalDeathfu1 colorectaldeath12)*sum;
title 'Colorectal cancer mortality';
run;

*Prostate cancer mortality;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var prostateDeathfu1 prostatedeath12;
tables (LYMPHOCYTESV1CAT all),(prostateDeathfu1 prostatedeath12)*sum;
title 'Prostate cancer mortality';
run;

*Breast cancer mortality;

proc tabulate data = wbccancermkI;
class LYMPHOCYTESV1CAT; 
var breastDeathfu1 breastdeath12;
tables (LYMPHOCYTESV1CAT all),(breastDeathfu1 breastdeath12)*sum;
title 'Breast cancer mortality';
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

proc sort data  = wbccancermkI; by LYMPHOCYTESV1CAT; run;

proc means data = wbccancermkI min mean max;
var LYMPHOCYTESCOUNTV1;
by LYMPHOCYTESV1CAT;
run;

data menopause;
set wbccancermkI;
if GENDER = "F" and MENOPAUSE = 1 then delete;

proc phreg data=menopause;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER/rl;
run;

proc phreg data=menopause;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER/rl;
run;

proc phreg data=menopause;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER/rl;
run;

proc phreg data=menopause;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER/rl;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Lymphocytes Table for p-trends;

data wbccancermkI;
set wbccancermkI;
if LYMPHOCYTESV1CAT = 0 then delete;
run;

*Overall cancer incidence;

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(anycancerfu);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')    ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')    ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')    ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')    ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')    ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')    ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')    ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

data wbccancermkI;
set wbccancermkI;
if LYMPHOCYTESV1CAT = 0 then delete;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

*Lung cancer incidence (lungfu1*lungcancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP;
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(lungfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP;
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

*proc phreg data=wbccancermkI;
*class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')  ;
*model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
*run;

*proc phreg data=wbccancermkI;
*class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESCOUNTV1QUINT(ref='0');
*model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESCOUNTV1QUINT/rl;
*run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT/rl;
run;

*Colorectal cancer (colorectalfu1*colorectalcancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP;
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(colorectalfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP;
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0');
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT/rl;
run;

*Colorectal cancer adjusted for hormone therapy use (colorectalfu1*colorectalcancer12);

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT/rl;
run;

*Male Prostate cancer (prostatefu1*prostatecancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP;
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(prostatefu1);
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP;
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')    ;
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT/rl;
run;

*Female breast cancer(breastfu1*breastcancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP;
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(breastfu1);
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')    ;
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0');
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Lymphocytes Table for p-trends;

data wbccancermkI;
set wbccancermkI;
if LYMPHOCYTESV1CAT = 0 then delete;
run;

*Overall cancer mortality;

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(cancerdeathfu);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 3;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

data wbccancermkI;
set wbccancermkI;
if LYMPHOCYTESV1CAT = 0 then delete;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0')CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

*Lung cancer mortality (lungDeathfu1*lungdeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP;
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(lungDeathfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ;
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')  ;
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT/rl;
run;

*Colorectal cancer (colorectalDeathfu1*colorectaldeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP;
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(colorectalDeathfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP;
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')    ;
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT/rl;
run;

*Colorectal cancer adjusted for hormone therapy use (colorectalDeathfu1*colorectaldeath12);

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT/rl;
run;

*Male Prostate cancer (prostateDeathfu1*prostatedeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP;
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(prostateDeathfu1);
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP;
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')    ;
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0')  ;
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT/rl;
run;

*Female breast cancer(breastDeathfu1*breastdeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD /rl;
PHAZARD = LYMPHOCYTESV1CAT*log(breastDeathfu1);
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP;
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0');
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0');
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Calculating Incidence Rates;

*Changes based on Dr. Folsom's comments (FROM MAGNESIUM AACR2017 SAS CODE);

*Finding Incidence Rates;
data magnesium;
set magnesium;
ln = log(tpyrsany12);
run;

*This option does not give me an lsmeans because of the param=ref in the class statement);
PROC GENMOD DATA=magnesium;
Class magdie (ref="0" param=ref);
MODEL CRC12 = magdie v1age01/ DIST=poi LINK=log offset=ln;
lsmeans magdie / ilink cl;
run;


*can use the mean and LCL/UCL from the LSMeans table to make updated figures which present the rates in each groups (need to report the age adjusted incidence rate in the final version);
PROC GENMOD DATA=magnesium;
Class magdie (ref="0");
MODEL CRC12 = magdie v1age01/ DIST=poi LINK=log offset=ln;
lsmeans magdie / ilink cl;
run;

PROC GENMOD DATA=magnesium;
Class magdie (ref="0");
MODEL COLONCA12 = magdie v1age01/ DIST=poi LINK=log offset=ln;
lsmeans magdie / ilink cl;
run;

PROC GENMOD DATA=magnesium;
Class magdie (ref="0");
MODEL RECTALCA12 = magdie v1age01/ DIST=poi LINK=log offset=ln;
lsmeans magdie / ilink cl;
run;

PROC GENMOD DATA=magnesium;
where gender = "M";
Class magdie (ref="0");
MODEL CRC12 = magdie v1age01/ DIST=poi LINK=log offset=ln;
lsmeans magdie / ilink cl;
run;

PROC GENMOD DATA=magnesium;
where gender = "F";
Class magdie (ref="0");
MODEL CRC12 = magdie v1age01/ DIST=poi LINK=log offset=ln;
lsmeans magdie / ilink cl;
run;

PROC GENMOD DATA=magnesium;
where racegrp = "W";
Class magdie (ref="0");
MODEL CRC12 = magdie v1age01/ DIST=poi LINK=log offset=ln;
lsmeans magdie / ilink cl;
run;

PROC GENMOD DATA=magnesium;
where racegrp = "B";
Class magdie (ref="0");
MODEL CRC12 = magdie v1age01/ DIST=poi LINK=log offset=ln;
lsmeans magdie / ilink cl;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Calculating Person Time (FROM LEG LENGTH 2018 WITH AA SAS CODE);

*Testing the person years calculation for Dr. Prizment;
*(Note: cellcat- categorical exposure of interest, death - outcome);

proc tabulate data = leglength2012;
class legquintb; 
var CRC12 tpyrsany12;
tables (legquintb all),(CRC12 tpyrsany12)*sum;
title 'colorectal cancer person-years';
run;

*table 2 person years;
proc tabulate data = leglength2012;
class legquintb; 
var CRC12 tpyrsany12;
tables (legquintb all),(CRC12 tpyrsany12)*sum;
title 'colorectal cancer person-years';
run;

proc tabulate data = leglength2012;
class sit_htquintb; 
var CRC12 tpyrsany12;
tables (sit_htquintb all),(CRC12 tpyrsany12)*sum;
title 'colorectal cancer person-years';
run;

proc tabulate data = leglength2012;
class heightquintb; 
var CRC12 tpyrsany12;
tables (heightquintb all),(CRC12 tpyrsany12)*sum;
title 'colorectal cancer person-years';
run;

*Table 3 person years;

proc sort data = leglength2012;
by gender;
run;

proc tabulate data = leglength2012;
class legquintb; 
var CRC12 tpyrsany12;
tables (legquintb all),(CRC12 tpyrsany12)*sum;
by gender;
title 'colorectal cancer person-years';
run;

proc sort data = leglength2012;
by racegrp;
run;

proc tabulate data = leglength2012;
class legquintb; 
var CRC12 tpyrsany12;
tables (legquintb all),(CRC12 tpyrsany12)*sum;
by racegrp;
title 'colorectal cancer person-years';
run;

proc sort data = leglength2012;
by agecategory;
run;

proc tabulate data = leglength2012;
class legquintb; 
var CRC12 tpyrsany12;
tables (legquintb all),(CRC12 tpyrsany12)*sum;
by agecategory;
title 'colorectal cancer person-years';
run;

*Table 4 PY;

proc sort data = leglength2012;
by gender;
run;

proc tabulate data = leglength2012;
class sit_htquintb; 
var CRC12 tpyrsany12;
tables (sit_htquintb all),(CRC12 tpyrsany12)*sum;
by gender;
title 'colorectal cancer person-years';
run;

proc sort data = leglength2012;
by racegrp;
run;

proc tabulate data = leglength2012;
class sit_htquintb; 
var CRC12 tpyrsany12;
tables (sit_htquintb all),(CRC12 tpyrsany12)*sum;
by racegrp;
title 'colorectal cancer person-years';
run;

proc sort data = leglength2012;
by agecategory;
run;

proc tabulate data = leglength2012;
class sit_htquintb; 
var CRC12 tpyrsany12;
tables (sit_htquintb all),(CRC12 tpyrsany12)*sum;
by agecategory;
title 'colorectal cancer person-years';
run;

*Table 5 PY;

proc sort data = leglength2012;
by gender;
run;

proc tabulate data = leglength2012;
class heightquintb; 
var CRC12 tpyrsany12;
tables (heightquintb all),(CRC12 tpyrsany12)*sum;
by gender;
title 'colorectal cancer person-years';
run;

proc sort data = leglength2012;
by racegrp;
run;

proc tabulate data = leglength2012;
class heightquintb; 
var CRC12 tpyrsany12;
tables (heightquintb all),(CRC12 tpyrsany12)*sum;
by racegrp;
title 'colorectal cancer person-years';
run;

proc sort data = leglength2012;
by agecategory;
run;

proc tabulate data = leglength2012;
class heightquintb; 
var CRC12 tpyrsany12;
tables (heightquintb all),(CRC12 tpyrsany12)*sum;
by agecategory;
title 'colorectal cancer person-years';
run;

*Table 6 PY;

proc tabulate data = leglength2012;
class legquintb; 
var colonca12 tpyrsany12;
tables (legquintb all),(colonca12 tpyrsany12)*sum;
title 'colorectal cancer person-years';
run;

proc tabulate data = leglength2012;
class sit_htquintb; 
var colonca12 tpyrsany12;
tables (sit_htquintb all),(colonca12 tpyrsany12)*sum;
title 'colorectal cancer person-years';
run;

proc tabulate data = leglength2012;
class heightquintb; 
var colonca12 tpyrsany12;
tables (heightquintb all),(colonca12 tpyrsany12)*sum;
title 'colorectal cancer person-years';
run;

*Table 7 PY;

proc sort data = leglength2012;
by gender;
run;

proc tabulate data = leglength2012;
class legquintb; 
var colonca12 tpyrsany12;
tables (legquintb all),(colonca12 tpyrsany12)*sum;
by gender;
title 'colorectal cancer person-years';
run;

proc sort data = leglength2012;
by racegrp;
run;

proc tabulate data = leglength2012;
class legquintb; 
var colonca12 tpyrsany12;
tables (legquintb all),(colonca12 tpyrsany12)*sum;
by racegrp;
title 'colorectal cancer person-years';
run;

proc sort data = leglength2012;
by agecategory;
run;

proc tabulate data = leglength2012;
class legquintb; 
var colonca12 tpyrsany12;
tables (legquintb all),(colonca12 tpyrsany12)*sum;
by agecategory;
title 'colorectal cancer person-years';
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

libname mylib 'E:\Professional Folder\WBOB\ARIC Data\Updated ARIC cases\CANCER';

Data overall2012;
set mylib.OVERALL_CANCER_2012;
Run;

proc contents data=overall2012;
run;

*We are going tp use the individual cancer datasets, because the overall cancer dataset lists primary & secondary cancers, which is a lot of extraneous data;

Data bladder2012;
set mylib.BLADDER_CANCER_2012;
Run;

proc contents data=bladder2012;
run;

data bladder2012;
set bladder2012 (Keep= id incidence death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data bladder2012;
set bladder2012;
Bladdercancer12 = incidence;
Bladderdeath12 = death;
Bladderfu1 = incidence_t1;
Bladderfu2 = incidence_t2;
Bladderfu3 = incidence_t3;
Bladderfu4 = incidence_t4;
BladderDeathfu1 = death_t1;
BladderDeathfu2 = death_t2;
BladderDeathfu3 = death_t3;
BladderDeathfu4 = death_t4;
bladdercancerprev = prvcancr;
run;

Data breast2012;
set mylib.BREAST_CANCER_2012;
Run;

proc contents data=breast2012;
run;

data breast2012;
set breast2012 (Keep= id incidence death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data breast2012;
set breast2012;
breastcancer12 = incidence;
breastdeath12 = death;
breastfu1 = incidence_t1;
breastfu2 = incidence_t2;
breastfu3 = incidence_t3;
breastfu4 = incidence_t4;
breastDeathfu1 = death_t1;
breastDeathfu2 = death_t2;
breastDeathfu3 = death_t3;
breastDeathfu4 = death_t4;
breastcancerprev = prvcancr;
run;

Data colorectal2012;
set mylib.COLORECTAL_CANCER_2012;
Run;

proc contents data=colorectal2012;
run;

data colorectal2012;
set colorectal2012 (Keep= id colorectal_incidence colorectal_death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data colorectal2012;
set colorectal2012;
colorectalcancer12 = colorectal_incidence;
colorectaldeath12 = colorectal_death;
colorectalfu1 = incidence_t1;
colorectalfu2 = incidence_t2;
colorectalfu3 = incidence_t3;
colorectalfu4 = incidence_t4;
colorectalDeathfu1 = death_t1;
colorectalDeathfu2 = death_t2;
colorectalDeathfu3 = death_t3;
colorectalDeathfu4 = death_t4;
colorectalcancerprev = prvcancr;
run;

proc means data = colorectal2012;
var colorectalfu1 colorectalDeathfu1;
run;

Data liver2012;
set mylib.LIVER_CANCER_2012;
Run;

proc contents data=liver2012;
run;

data liver2012;
set liver2012 (Keep= id incidence death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data liver2012;
set liver2012;
livercancer12 = incidence;
liverdeath12 = death;
liverfu1 = incidence_t1;
liverfu2 = incidence_t2;
liverfu3 = incidence_t3;
liverfu4 = incidence_t4;
liverDeathfu1 = death_t1;
liverDeathfu2 = death_t2;
liverDeathfu3 = death_t3;
liverDeathfu4 = death_t4;
livercancerprev = prvcancr;
run;

Data lung2012;
set mylib.LUNG_CANCER_2012;
Run;

proc contents data=lung2012;
run;

data lung2012;
set lung2012 (Keep= id incidence death incidence_t1 incidence_t2 incidence_t3 incidence_t4 death_t1 death_t2 death_t3 death_t4 prvcancr);
run;

data lung2012;
set lung2012;
lungcancer12 = incidence;
lungdeath12 = death;
lungfu1 = incidence_t1;
lungfu2 = incidence_t2;
lungfu3 = incidence_t3;
lungfu4 = incidence_t4;
lungDeathfu1 = death_t1;
lungDeathfu2 = death_t2;
lungDeathfu3 = death_t3;
lungDeathfu4 = death_t4;
lungcancerprev = prvcancr;
run;

proc means data = lung2012;
where lungcancer12 = 1;
var lungfu1 lungDeathfu1;
run;

proc means data = colorectal2012;
where colorectalcancer12 = 1;
var colorectalfu1 colorectalDeathfu1;
run;

proc freq data = lung2012;
tables lungcancer12 lungdeath12;
run;

proc freq data = colorectal2012;
tables colorectalcancer12 colorectaldeath12;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Adjusting Lymphocyte counts for WBC levels exploratory analysis;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*SMOKING WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0')  LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER WBCV1/rl;
run;

*Lung cancer incidence (lungfu1*lungcancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD  WBCV1/rl;
PHAZARD = LYMPHOCYTESV1CAT*log(lungfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

*proc phreg data=wbccancermkI;
*class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
*model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
*run;

*proc phreg data=wbccancermkI;
*class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESCOUNTV1QUINT(ref='0');
*model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESCOUNTV1QUINT WBCV1/rl;
*run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

*Colorectal cancer (colorectalfu1*colorectalcancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD  WBCV1/rl;
PHAZARD = LYMPHOCYTESV1CAT*log(colorectalfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT WBCV1/rl;
run;

*Colorectal cancer adjusted for hormone therapy use (colorectalfu1*colorectalcancer12);

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model colorectalfu1*colorectalcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

*Male Prostate cancer (prostatefu1*prostatecancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD  WBCV1/rl;
PHAZARD = LYMPHOCYTESV1CAT*log(prostatefu1);
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model prostatefu1*prostatecancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT WBCV1/rl;
run;

*Female breast cancer(breastfu1*breastcancer12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD  WBCV1/rl;
PHAZARD = LYMPHOCYTESV1CAT*log(breastfu1);
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='1') LYMPHOCYTESV1CAT(ref='1');
model breastfu1*breastcancer12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Cancer mortality;

*****************************************************************************************************************************************************************************************;

*Lymphocytes Table;

*Overall cancer mortality;

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD  WBCV1/rl;
PHAZARD = LYMPHOCYTESV1CAT*log(cancerdeathfu);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*SMOKING WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2 and GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0')CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='1') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2 and GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*GENDER WBCV1/rl;
run;

*Lung cancer mortality (lungDeathfu1*lungdeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD  WBCV1/rl;
PHAZARD = LYMPHOCYTESV1CAT*log(lungDeathfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT LYMPHOCYTESV1CAT*SMOKING WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

*Colorectal cancer (colorectalDeathfu1*colorectaldeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD  WBCV1/rl;
PHAZARD = LYMPHOCYTESV1CAT*log(colorectalDeathfu1);
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT WBCV1/rl;
run;

*Colorectal cancer adjusted for hormone therapy use (colorectalDeathfu1*colorectaldeath12);

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model colorectalDeathfu1*colorectaldeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

*Male Prostate cancer (prostateDeathfu1*prostatedeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD  WBCV1/rl;
PHAZARD = LYMPHOCYTESV1CAT*log(prostateDeathfu1);
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
Where GENDER = "M";
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model prostateDeathfu1*prostatedeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC LYMPHOCYTESV1CAT WBCV1/rl;
run;

*Female breast cancer(breastDeathfu1*breastdeath12);

* Test proportional hazard assumption
Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status
check model with bands = 0;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT PHAZARD  WBCV1/rl;
PHAZARD = LYMPHOCYTESV1CAT*log(breastDeathfu1);
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT WBCV1/rl;
run;

proc phreg data=wbccancermkI;
where GENDER = "F" and MENOPAUSE = 1;
class CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='1') LYMPHOCYTESV1CAT(ref='1');
model breastDeathfu1*breastdeath12(0) = V1AGE01 CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT WBCV1/rl;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

libname mylibmkI 'E:\Professional Folder\WBOB\ARIC WBC Project\2_Summer 2017 RA Projects\SAS Datasets';

data wbccancermkI;
set mylibmkI.wbccancermkI;
run;

proc contents data = wbccancermkI;
run;

*Descriptive Statistics (use the variable with the * by the name);

*Age (V1AGE01)

Study center (center) 

Sex (gender) 

Race (racegrp) 

Education (elevel01 elevel02*) 

Drinking (EVRDRK01 FORDRK01 CURDRK02 DRNKR01 ETHANL03*) and I created DRINKING*

Smoking (EVRSMK01 FORSMK01 CURSMK01 CIGTYR01* CIGT01 CIGR01 CIGRYR01) and I created SMOKING*

Hormone Replacement Therapy (HORMON01 HORMON02 RHXA16) and I created HORMONEUSE*

Menopause (RHXA07*) and I created MENOPAUSE*

CVD (HYPERT05 STROKE01 PRVCHD05 RANGNA01) and I created CVD*

BMI (BMI01)

Inflammation (CRP4, Aspirin*) I created CRPCAT*

Diabetes (DIABETES DIABTS02* DIABTS03)

Physical Activity (SPRT_I02);

proc means data = wbccancermkI;
var V1AGE01;
run;

proc freq data = wbccancermkI;
tables center gender racegrp;
run;
 
proc freq data = wbccancermkI;
tables elevel01 elevel02;
run;

proc freq data = wbccancermkI;
tables EVRDRK01 FORDRK01 CURDRK02 DRNKR01;
run;

data wbccancermkI;
set wbccancermkI;
if EVRDRK01 = 0 then DRINKING = 0;
else if FORDRK01 = 1 then DRINKING = 1;
else if CURDRK02 = 1 then DRINKING = 2;
run;

proc freq data = wbccancermkI;
tables EVRDRK01 FORDRK01 CURDRK02 DRNKR01 DRINKING;
run;

proc means data = wbccancermkI;
var ETHANL03;
run;
 
proc freq data = wbccancermkI;
tables EVRSMK01 FORSMK01 CURSMK01 CIGT01 CIGR01;
run;

data wbccancermkI;
set wbccancermkI;
if EVRSMK01 = 0 then SMOKING = 0;
else if FORSMK01 = 1 then SMOKING = 1;
else if CURSMK01 = 1 then SMOKING = 2;
run;

proc freq data = wbccancermkI;
tables EVRSMK01 FORSMK01 CURSMK01 CIGT01 CIGR01 SMOKING;
run;

proc means data = wbccancermkI;
var CIGTYR01 CIGRYR01;
run;
 
proc freq data = wbccancermkI;
tables HORMON01 HORMON02 RHXA16;
run;

data wbccancermkI;
set wbccancermkI;
if HORMON02 = 3 then HORMONEUSE = 1;
else HORMONEUSE = 0;
run;

proc freq data = wbccancermkI;
tables HORMON01 HORMON02 RHXA16 RHXA07 HORMONEUSE;
run;

proc freq data = wbccancermkI;
tables RHXA07;
run;

data wbccancermkI;
set wbccancermkI;
if RHXA07 = "Y" then MENOPAUSE = 1;
else MENOPAUSE = 0;
run;

proc freq data = wbccancermkI;
tables RHXA07 MENOPAUSE MENOPAUSE*GENDER;
run;

proc freq data = wbccancermkI;
tables HYPERT05 STROKE01 PRVCHD05 RANGNA01;
run;

data wbccancermkI;
set wbccancermkI;
if HYPERT05 = 1 then CVD = 1;
else if STROKE01 = "Y" then CVD = 2;
else if PRVCHD05 = 1 then CVD = 2;
else if RANGNA01 = 1 then CVD = 2;
else CVD = 0;
run;

proc freq data = wbccancermkI;
tables HYPERT05 STROKE01 PRVCHD05 RANGNA01 CVD;
run;

proc means data = wbccancermkI;
var BMI01;
run;

proc means data = wbccancermkI;
var CRP4; *need to code this as a categorical variable so that we do not loose observations in the model;
run;

proc rank data=wbccancermkI groups=4 out=wbccancermkI;
var CRP4; 
ranks CRP4QUART;
run;

proc freq data = wbccancermkI;
tables CRP4QUART;
run;

data wbccancermkI;
set wbccancermkI;
if CRP4QUART = . then CRPCAT = 0;
else if CRP4QUART = 0 then CRPCAT = 1;
if CRP4QUART = 1 then CRPCAT = 2;
if CRP4QUART = 2 then CRPCAT = 3;
if CRP4QUART = 3 then CRPCAT = 4;
run;

proc freq data = wbccancermkI;
tables CRP4QUART CRPCAT;
run;

proc freq data = wbccancermkI;
tables ASPIRINC;
run;

proc freq data = wbccancermkI;
tables DIABETES DIABTS02 DIABTS03;
run;

proc freq data = wbccancermkI;
tables SPRT_I02;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Exposures of interest (Tertiles > Quartiles > Quintiles / Continuous > Continuous per SD > Splines);

*WBCV1
GRANULOCYTESCOUNTV1
NEUTROPHILSTOTALCOUNTV1
LYMPHOCYTESCOUNTV1
NLRV1;

*Outcomes of interest;

*Overall cancer incidence

Lung Cancer incidence
Lung Cancer incidence In Never Smokers

Colorectal Cancer incidence 
Colorectal Cancer incidence Adjusted for Hormone Use

Male Prostate Cancer incidence
Female Breast inciddence Cancer;

*Overall cancer mortality 

Lung Cancer mortality
Lung Cancer mortality In Never Smokers

Colorectal Cancer mortality 
Colorectal Cancer mortality Adjusted for Hormone Use

Male Prostate Cancer mortality
Female Breast mortality Cancer;

*Statistical Models;

*Model 1: Age + Sex + Race + Center
Model 2: Model 1 + Education + Drinking + Smoking + BMI + Physical activity
Model 3: Model 2 + CVD + Diabetes + CRP + ASPIRIN + HRT + Menopause status;

*Summary of the variables to use in remaining analyses;

proc contents data = wbccancermkI;
run;

proc means data = wbccancermkI;
var V1AGE01 ETHANL03 CIGTYR01 BMI01;
run;

proc freq data = wbccancermkI;
tables CENTER GENDER RACEGRP ELEVEL02 DRINKING SMOKING HORMONEUSE ASPIRINC MENOPAUSE CVD DIABTS02 SPRT_I02 CRPCAT;
run;

proc means data = wbccancermkI;
var WBCV1 NEUTROPHILSTOTALCOUNTV1 NEUTROPHILSCOUNTV1 BANDSCOUNTV1 LYMPHOCYTESCOUNTV1 MONOCYTESCOUNTV1 EOSINOPHILSCOUNTV1 BASOPHILSCOUNTV1 GRANULOCYTESCOUNTV1 NLRV1; 
run;

proc freq data = wbccancermkI;
tables WBCV1TERT NEUTROPHILSTOTALCOUNTV1TERT NEUTROPHILSCOUNTV1TERT BANDSCOUNTV1TERT LYMPHOCYTESCOUNTV1TERT MONOCYTESCOUNTV1TERT EOSINOPHILSCOUNTV1TERT BASOPHILSCOUNTV1TERT GRANULOCYTESCOUNTV1TERT NLRV1TERT;
run;

proc freq data = wbccancermkI;
tables WBCV1CAT NEUTROPHILSTOTALV1CAT LYMPHOCYTESV1CAT MONOCYTESV1CAT EOSINOPHILSV1CAT BASOPHILSV1CAT;
run;

data wbccancermkI;
set wbccancermkI;
if anycancer = 1 then anycancerfu = (dxdate_1 - V1DATE01)/365.2; else anycancerfu = 26.1;
cancerdeathfu = (deathcensdt - V1DATE01)/365.2;
run;

proc means data = wbccancermkI;
var anycancerfu cancerdeathfu;
run;

proc freq data = wbccancermkI;
tables anycancer cancerdeath breastcancer12 breastdeath12 colorectalcancer12 colorectaldeath12 lungcancer12 lungdeath12 prostatecancer12 prostatedeath12;
run;

proc means data = wbccancermkI;
var anycancerfu cancerdeathfu breastfu1 breastDeathfu1 colorectalfu1 colorectalDeathfu1 lungfu1 lungDeathfu1 prostatefu1 prostateDeathfu1;
run;

proc corr data = wbccancermkI;
var WBCV1 LYMPHOCYTESCOUNTV1 NEUTROPHILSTOTALCOUNTV1;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*AACR Poster Table 1: Overall Incidence and mortality broken down by smoking status;

*Overall cancer incidence;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where LYMPHOCYTESV1CAT ne 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

*Overall cancer mortality;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where LYMPHOCYTESV1CAT ne 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

*AACR Poster Table 2: Lung cancer Incidence and mortality broken down by smoking status;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2') LYMPHOCYTESV1CAT(ref='1');
model lungfu1*lungcancer12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2') LYMPHOCYTESV1CAT(ref='1');
model lungDeathfu1*lungdeath12(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

*AACR Poster Table 3: non-lung cancer incidence and mortality broken down by smoking status;

proc freq data = wbccancermkI;
tables anycancer cancerdeath breastcancer12 breastdeath12 colorectalcancer12 colorectaldeath12 lungcancer12 lungdeath12 prostatecancer12 prostatedeath12;
run;

data wbccancermkI;
set wbccancermkI;
if anycancer = 0 then lungfree = 0;
else if anycancer = 1 and lungcancer12 = 1 then lungfree = 0;
else lungfree = 1;
if cancerdeath = 0 then lungdeathfree = 0;
else if cancerdeath = 1 and lungdeath12 = 1 then lungdeathfree = 0;
else lungdeathfree = 1;
run;

proc freq data = wbccancermkI;
tables anycancer lungcancer12 lungfree cancerdeath lungdeath12 lungdeathfree;
run; 

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model anycancerfu*lungfree(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*lungfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*lungfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*lungfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*lungfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*lungfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*lungfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where LYMPHOCYTESV1CAT ne 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*lungfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model cancerdeathfu*lungdeathfree(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*lungdeathfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*lungdeathfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*lungdeathfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*lungdeathfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*lungdeathfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*lungdeathfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where LYMPHOCYTESV1CAT ne 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*lungdeathfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

*AACR Poster Table 4: Overall Incidence and mortality broken down by smoking status;

proc freq data = wbccancermkI;
tables anycancer cancerdeath breastcancer12 breastdeath12 colorectalcancer12 colorectaldeath12 lungcancer12 lungdeath12 prostatecancer12 prostatedeath12;
run;

data wbccancermkI;
set wbccancermkI;
if anycancer = 0 then prostatefree = 0;
else if anycancer = 1 and prostatecancer12 = 1 then prostatefree = 0;
else prostatefree = 1;
run;

proc freq data = wbccancermkI;
tables anycancer prostatecancer12 prostatefree;
run; 

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP LYMPHOCYTESV1CAT (ref='1');
model anycancerfu*prostatefree(0) = V1AGE01 GENDER CENTER RACEGRP LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*prostatefree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*prostatefree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*prostatefree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*prostatefree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*prostatefree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

*AACR Poster Table 5: Overall Incidence and mortality broken down by smoking status;

data wbccancermkI;
set wbccancermkI;
if SMOKING = 0 then CURRENTSMOKE = 0;
else if SMOKING = 1 then CURRENTSMOKE = 0;
else CURRENTSMOKE = 1;
run;

proc freq data = wbccancermkI;
tables SMOKING CURRENTSMOKE;
run; 

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0')  LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 1 and GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 1 and GENDER = "F";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0')  LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 1 and RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0')  LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 1 and RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0')LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 1 and GENDER = "M";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 1 and GENDER = "F";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 1 and RACEGRP = "B";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where CURRENTSMOKE = 1 and RACEGRP = "W";
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT/rl;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*SER Table numbers;

proc freq data = wbccancermkI;
tables anycancer cancerdeath anycancer*SMOKING cancerdeath*smoking lungcancer12*smoking lungdeath12*smoking lungfree*smoking lungdeathfree*smoking prostatefree*smoking;
run; 

proc freq data = wbccancermkI;
tables anycancer*CURRENTSMOKE cancerdeath*CURRENTSMOKE anycancer*CURRENTSMOKE*GENDER anycancer*CURRENTSMOKE*RACEGRP cancerdeath*CURRENTSMOKE*GENDER cancerdeath*CURRENTSMOKE*RACEGRP;
run; 

proc sort data = wbccancermkI; 
by RACEGRP;
run;

proc means data = wbccancermkI mean min max;
var WBCV1 LYMPHOCYTESV1;
by RACEGRP;
run;

proc means data = wbccancermkI mean min max;
var WBCV1 LYMPHOCYTESV1;
run;

proc sort data = wbccancermkI; 
by SMOKING;
run;

proc means data = wbccancermkI n mean min max;
var WBCV1 LYMPHOCYTESV1;
by SMOKING;
run;

proc means data = wbccancermkI n mean min max;
var WBCV1 LYMPHOCYTESV1;
by SMOKING;
where RACEGRP = "B";
run;

proc means data = wbccancermkI n mean min max;
var WBCV1 LYMPHOCYTESV1;
by SMOKING;
where RACEGRP = "W";
run;

proc reg data = wbccancermkI;
model LYMPHOCYTESV1CAT = SMOKING;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where LYMPHOCYTESV1CAT ne 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*lungfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where LYMPHOCYTESV1CAT ne 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model anycancerfu*lungfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*lungdeathfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where LYMPHOCYTESV1CAT ne 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') LYMPHOCYTESV1CAT(ref='1');
model cancerdeathfu*lungdeathfree(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 LYMPHOCYTESV1CAT SMOKING*LYMPHOCYTESV1CAT/rl;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*Granulocytes analysis for the poster;

libname mylibmkI 'E:\Professional Folder\WBOB\ARIC WBC Project\2_Summer 2017 RA Projects\SAS Datasets';

data wbccancermkI;
set mylibmkI.wbccancermkI;
run;

proc contents data = wbccancermkI;
run;

*In the previous analysis, all values below 2SD and above 2SD were excluded;  
*For blacks we have 3879 5.6677752 1.9729980 so the exclusion criteria will be WBC lt 1.7217792 and WBC gt 9.6137712;
*For whites we have 6868 6.3154193 1.8754815 so the exclusion criteria will be WBC lt 2.5644563 and WBC gt 10.0663823;

*NEUTROPHILSCOUNTV1 = (NEUTROPHILSV1/100) * WBCV1;
*BANDSCOUNTV1 = (BANDSV1/100) * WBCV1;
*GRANULOCYTESCOUNTV1 = (GRANULOCYTESV1/100) * WBCV1;
*MONOCYTESCOUNTV1 = (MONOCYTESV1/100) * WBCV1;
*EOSINOPHILSCOUNTV1 = (EOSINOPHILSV1/100) * WBCV1;
*BASOPHILSCOUNTV1 = (BASOPHILSV1/100) * WBCV1;
*NEUTROPHILSTOTALCOUNTV1 = NEUTROPHILSCOUNTV1 + BANDSCOUNTV1;
*GRANULOCYTESCOUNTV1 = NEUTROPHILSTOTALCOUNTV1 + EOSINOPHILSCOUNTV1 + BASOPHILSCOUNTV1;
*NLRV1 = NEUTROPHILSTOTALCOUNTV1 / GRANULOCYTESCOUNTV1;
*NEUTROPHILSTOTALV1 = NEUTROPHILSV1 + BANDSV1;

*In the approach above we were trying to combine based on distributions of both the WBC count overall Normal Range, and the distribution of each differential indices as a percentage;
*However, I am not super comfortable with this definition because some individual may have high WBC and low lymphs or vice versa, so our categories for differential counts (not percents) may not match;
*Now we cut the WBC indices off at Q1 and Q99 based on proc univariate to remove counts due to occult disease; 
*then we establish the +/- 2SD as the normal range based on the litterature Dr. Tang shared with us;
*then we are remaking the exposure variable by categorizing values as below NR, within NR (Tertiles) and above NR for each differential indices;

*In this version of the analysis, we will exclude values below Q1 and above Q99 for GRANULOCYTES, then we will categorise values as below NR, within NR (Tertiles) and above NR;  

*Let's remove the ends of the distribution based on the 68 (1SD) 95 (2SD) and 99.7 (3SD rule);

proc sort data = wbccancermkI; by RACEGRP; run;

proc means data = wbccancermkI n mean std;
var WBCV1 GRANULOCYTESCOUNTV1;
by RACEGRP;
run;

proc univariate data = wbccancermkI;
var WBCV1 GRANULOCYTESCOUNTV1;
histogram WBCV1 GRANULOCYTESCOUNTV1 / normal;
by RACEGRP;
run;

*For blacks q1 = 0.858 q99 = 8.184 based on GRANULOCYTESCOUNTV1;
*For whites q1 = 1.680 q99 = 8.850 based on GRANULOCYTESCOUNTV1;

data wbccancermkI;
set wbccancermkI;
if RACEGRP = "B" and GRANULOCYTESCOUNTV1 lt 0.858 then delete;
if RACEGRP = "B" and GRANULOCYTESCOUNTV1 gt 8.184 then delete;
if RACEGRP = "W" and GRANULOCYTESCOUNTV1 lt 1.680 then delete;
if RACEGRP = "W" and GRANULOCYTESCOUNTV1 gt 8.850 then delete;
run;

proc univariate data = wbccancermkI;
var GRANULOCYTESCOUNTV1;
histogram GRANULOCYTESCOUNTV1 / normal;
output out=UniWidePctls pctlpre=GRANULOCYTESCOUNTV1P_ pctlpts=2.5,97.5;
by RACEGRP;
run; 

proc print data=UniWidePctls noobs; run;

*For blacks GRANULOCYTESCOUNTV1 we have 3596 3.05258565 1.34034675 and the pct are 1.131 6.363;
*For whites GRANULOCYTESCOUNTV1 we have 6589 4.008228 1.33382134 and the pct are 2.052 7.344;

*Granulocytes indices for blacks;

data wbccancerblack;
set wbccancermkI(keep = ID RACEGRP GRANULOCYTESCOUNTV1);
where RACEGRP='B' and 1.131 lt GRANULOCYTESCOUNTV1 lt 6.363;
run;

proc rank data=wbccancerblack groups=3 out=wbccancerblack;
var GRANULOCYTESCOUNTV1; 
ranks GRANULOCYTESCOUNTV1TERTNRB;
run;

*Granulocytes indices for whites;

data wbccancerwhite;
set wbccancermkI (keep = ID RACEGRP GRANULOCYTESCOUNTV1);
where RACEGRP='W' and 2.052 lt GRANULOCYTESCOUNTV1 lt 7.344;
run;

proc rank data=wbccancerwhite groups=3 out=wbccancerwhite;
var GRANULOCYTESCOUNTV1; 
ranks GRANULOCYTESCOUNTV1TERTNRW;
run;

*Combining all the differential indices together;

proc sort data = wbccancerblack; by ID; run;
proc sort data = wbccancermkI; by ID; run;

data wbccancerblackv2;
merge wbccancermkI wbccancerblack;
by ID;
run;

proc print data = wbccancerblackv2;
where GRANULOCYTESCOUNTV1 = .;
run;

proc sort data = wbccancerwhite; by ID; run;
proc sort data = wbccancermkI; by ID; run;

data wbccancerwhitev2;
merge wbccancermkI wbccancerwhite;
by ID;
run;

proc print data = wbccancerwhitev2;
where GRANULOCYTESCOUNTV1 = .;
run;

*For blacks GRANULOCYTESCOUNTV1 we have 3596 3.05258565 1.34034675 and the pct are 1.131 6.363;

data wbccancerblackv2;
set wbccancerblackv2;
where RACEGRP = "B";
if GRANULOCYTESCOUNTV1 = . then GRANULOCYTESV1CAT = .;
else if GRANULOCYTESCOUNTV1 le 1.131 then GRANULOCYTESV1CAT = 0;
else if GRANULOCYTESCOUNTV1 ge 6.363 then GRANULOCYTESV1CAT = 4;
else GRANULOCYTESV1CAT = GRANULOCYTESCOUNTV1TERTNRB + 1;
run;

proc freq data = wbccancerblackv2;
tables GRANULOCYTESV1CAT;
run;

proc sort data = wbccancerblackv2; by GRANULOCYTESV1CAT; run;

proc means data = wbccancerblackv2 n min mean max;
var GRANULOCYTESCOUNTV1;
by GRANULOCYTESV1CAT;
run;

*For whites GRANULOCYTESCOUNTV1 we have 6589 4.008228 1.33382134 and the pct are 2.052 7.344;

data wbccancerwhitev2;
set wbccancerwhitev2;
where RACEGRP = "W";
if GRANULOCYTESCOUNTV1 = . then GRANULOCYTESV1CAT = .;
else if GRANULOCYTESCOUNTV1 le 2.052 then GRANULOCYTESV1CAT = 0;
else if GRANULOCYTESCOUNTV1 ge 7.344 then GRANULOCYTESV1CAT = 4;
else GRANULOCYTESV1CAT = GRANULOCYTESCOUNTV1TERTNRW + 1;
run;

proc freq data = wbccancerwhitev2;
tables GRANULOCYTESV1CAT;
run;

proc sort data = wbccancerwhitev2; by GRANULOCYTESV1CAT; run;

proc means data = wbccancerwhitev2 n min mean max;
var GRANULOCYTESCOUNTV1;
by GRANULOCYTESV1CAT;
run;

data wbccancermkIII;
set wbccancerblackv2 wbccancerwhitev2;
run;

proc freq data = wbccancermkIII;
tables GRANULOCYTESV1CAT;
run;

proc sort data = wbccancermkIII; by GRANULOCYTESV1CAT; run;

proc means data = wbccancermkIII n min mean max;
var GRANULOCYTESCOUNTV1;
by GRANULOCYTESV1CAT;
run;

proc print data = wbccancermkIII;
where GRANULOCYTESV1CAT = .;
var ID RACEGRP WBCV1 GRANULOCYTESV1 GRANULOCYTESCOUNTV1;
run;

data wbccancermkI;
set wbccancermkIII;
run;

*SER Bonus Poster Granulocyte Table: Overall Incidence and mortality broken down by smoking status;

*Overall cancer incidence;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP GRANULOCYTESV1CAT (ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   GRANULOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') GRANULOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0')   GRANULOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1')   GRANULOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2')   GRANULOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') GRANULOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT SMOKING*GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
where GRANULOCYTESV1CAT ne 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') GRANULOCYTESV1CAT(ref='1');
model anycancerfu*anycancer(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT SMOKING*GRANULOCYTESV1CAT/rl;
run;

*Overall cancer mortality;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP GRANULOCYTESV1CAT (ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') GRANULOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') CVD(ref='0') DIABTS02(ref='0') ASPIRINC(ref='0') HORMONEUSE(ref='0') MENOPAUSE(ref='0') GRANULOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01   CVD DIABTS02 ASPIRINC HORMONEUSE MENOPAUSE GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') GRANULOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 1;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='1') GRANULOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where SMOKING = 2;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='2') GRANULOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') GRANULOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT SMOKING*GRANULOCYTESV1CAT/rl;
run;

proc phreg data=wbccancermkI;
Where GRANULOCYTESV1CAT ne 0;
class GENDER CENTER RACEGRP ELEVEL02(ref='1') DRINKING(ref='0') SMOKING(ref='0') GRANULOCYTESV1CAT(ref='1');
model cancerdeathfu*cancerdeath(0) = V1AGE01 GENDER CENTER RACEGRP ELEVEL02 DRINKING SMOKING ETHANL03 CIGTYR01 SPRT_I02 BMI01 GRANULOCYTESV1CAT SMOKING*GRANULOCYTESV1CAT/rl;
run;

*Saving the dataset;

libname mylibmkI 'E:\Professional Folder\WBOB\ARIC WBC Project\2_Summer 2017 RA Projects\SAS Datasets';

data mylibmkI.wbccancerSER;
set wbccancermkI;
run;

*Testing splines;

proc export data= wbccancermkI
   outfile= "E:\Professional Folder\WBOB\ARIC WBC Project\2_Summer 2017 RA Projects\SAS Datasets\WBC_STATA"
   dbms=XLSX
   replace;
run;
