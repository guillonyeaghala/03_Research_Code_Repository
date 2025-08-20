options ps = 3000 ls = 120 nofmterr;
*options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '   ';
*options ps = 3000 ls = 120;
libname mylib 'E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets';
*%inc 'C:\Documents and Settings\prizm001\Desktop\format.sas'; 

/*Creating the main dataset from the sas database*/

data one;
set mylib.analys;

proc contents data = one;
run;

proc freq data = one;
tables sumliq;
run;

proc sort data = one;
by id;
run;

/*Importing additional plasma results from the excel spreadsheet*/

PROC IMPORT OUT= work.Plasma DATAFILE= "E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets\MICA.xlsx" DBMS=xlsx REPLACE;
  SHEET="MICA 2"; 
  GETNAMES=YES;
RUN;

proc contents data=plasma;
run;

proc means data=plasma;
var PANC_ID__;
run;

proc print data=plasma;
where batch=.;
run;

/*Removing missing values from the plasma results*/

data plasma2 ;
set plasma (rename=(PANC_ID__=id)); 
run;

proc print data = plasma2;
var id;
run;

/*Need to convert MICA from character to numeric*/
*if batch=. then delete;
proc means data = plasma2 min mean median max std;
var mica;
run;

proc freq data=plasma2;
table mica;
run;

proc sort data=plasma2;
by id;
run;

/*Merging the cohort data with the plasma lab results data*/

data comb;
merge plasma2 one;
by id;
run;

proc print data = comb;
run;

data comb;
set comb;
keep id panc AGE SEX GENDER ETHNIC1 EDUC BDATE BIRTHDT DEATHDTE CASE ELIG FCONDATE DX_DATE DXSOURCE BIOPSY CYTOLOGY STAT PACK_YRS DIABETES DTFIB CYP1A1_G CYP1A2 CARBO PROT NAT1_GEN NAT2_GEN NAT2_P CALOR DIAB_AGE diab sumliq MICA; 
run;

proc print data = comb;
run;

/* Cleaning the lab values for the analysis */

* Remove unreliable data as indicated by the lab, Make a category for MICA below detection threshold (MICA = NS), create a new MICA value called MICAn for all values greater than 2, then divide these values in tertiles (MICAnc) for the anylsis;
* Analysis 2 has the values ranked automatically by sas into tertiles (MICAnc) and Analysis 3 has the values ranked manually into tertiles;
* the exc1 and exc2 are the exclusion criteria for MICA levels;
* Running Descriptive Statistics on Covariates of interest;
* Making MICA into a continuous variable, in MICAN1 all NS are coded as 0, in MICAN2 all NS are coded as 2, and all MICA values greater than 2 are coded as MICa, but rounded down to the nearest 0.01;
* the exc1 variable tells us which observatoins had non detectable vs detectable MICA levels;
 
data analys1;
set comb;
* unlreliable data;
iF ID IN (1501, 5057, 5055, 385, 506, 81, 82, 994, 308) THEN delete;
run;

data analys1;
set analys1;
if MICA='NS' then do; mican=0; mican1 =0; mican2=2; exc=1; end;
else if MICA ne 'NS' then do; exc=2; mican=round (MICA,0.01); mican1=round (MICA,0.01); mican2=round (MICA,0.01); end;
IF 0 le MICAn LE 2 THEN exc1=1;
else exc1=2;
if ethnic=1 then race=1;
else race=0;
run;

proc rank data=analys1 out=analys2 groups=3;
where MICAn gt 2;
 var MICAn;
 ranks MICAnc;
run;

*Average concentration of MICA in each category;
proc means data = analys2;
class micanc;
var mican;
run;

*Use the min and max of micanc to create the final 4 categories of MICA as a categorical variable, and create a dicothomous variable for mica based on detectable levels;

data analys3;
set analys1;
if 0 le mican le 2 then micac=0;
else if 2 lt mican le 32.10 then micac=1;
else if 32.14 le mican le 64.7 then micac=2;
else if mican ge 64.8 then micac=3; 
if 0 le mican le 2 then micad = 0; else micad = 1;
run;

proc freq data = analys3;
tables micac;
tables micad;
run;

*Descriptive statistics;

*Average concentration of MICA in each category of MICA;

proc means mean median min max data = analys3;
class micac;
var mican;
run;

*Average concentration of MICA by case control status;

proc means mean median min max data = analys3;
class case;
var mican;
run;

proc ttest data = analys3;
class case;
var mican;
run;

*Average concentration of MICA by gender;

proc means mean median min max data = analys3;
class sex;
var mican;
run;

*Average concentration of MICA by smoking status;

proc means mean median min max data = analys3;
class stat;
var mican;
run;

*Making graphs for the outcome variable;

proc sort data=analys3;
by case;
run;

proc univariate data = analys3;
var mican mican1 mican2;
histogram;
run;

proc boxplot data = analys3;
plot mican*case;
inset min max mean stddev;
run;

proc boxplot data=analys3;
where micac ne 0;
 plot mican*case;
 insetgroup mean min max; 
 run;

proc univariate NORMAL PLOT data=analys3; var mican;
where case=2;
histogram mican/normal;
run;
 
/*Cleaning up the variables used in adjusting the models */

data results1;
set analys3;
run;

proc sort data =results1;
by case;
run;

*Making a categorical variable for age, alcohol, and ever vs. never smokers;

/* if sumliq=0 then alco=0;
else if 0<sumliq le 7 then alco=1; 
else if sumliq gt 7 then alco=2; */

*I changed the coding for alcohol consumption so we would not loose as many observations in the final models;

data results1;
set results1;
if age lt 70 then agecat = "1";
if age ge 70 then agecat = "2";
if 0<sumliq le 7 then alco=1; 
else if sumliq gt 7 then alco=2; 
else alco = 0;
if stat = "." then delete;
if stat = 1 then stat2=0; else stat2=1;
run;

proc freq data = results1;
tables alco;
run;

/*Table 1 association between exploratory variables and groups*/

Proc Freq data = results1;
tables case*agecat micac*agecat/chisq;
run;

Proc Freq data = results1;
tables case*sex micac*sex/chisq;
run;

Proc Freq data = results1;
where case = 1;
tables micac*sex/norow nopercent;
run;

Proc Freq data = results1;
where case = 2;
tables micac*sex/norow nopercent;
run;

Proc Freq data = results1;
tables case*sex micac*sex/chisq;
run;

Proc Freq data = results1;
tables case*stat micac*stat/chisq;
run;

Proc Freq data = results1;
where case = 1;
tables micac*stat/norow nopercent;
run;

Proc Freq data = results1;
where case = 2;
tables micac*stat/norow nopercent;
run;

Proc Freq data = results1;
tables case*alco micac*alco/chisq;
run;

Proc Freq data = results1;
where case = 2;
tables micac*agecat/chisq;
run;

Proc Freq data = results1;
where case = 1;
tables micac*agecat/norow nopercent;
run;

Proc Freq data = results1;
where case = 2;
tables micac*agecat/norow nopercent;
run;

Proc Freq data = results1;
where case = 2;
tables micac*sex/chisq;
run;

Proc Freq data = results1;
where case = 2;
tables micac*stat/chisq;
run;

Proc Freq data = results1;
where case = 2;
tables case*alco micac*alco/chisq;
run;

Proc Freq data = results1;
where case = 1;
tables micac*alco/norow nopercent;
run;

Proc Freq data = results1;
where case = 2;
tables micac*alco/norow nopercent;
run;

/*Table 1 Specific to case control study*/

Proc Freq data = results1;
tables case*agecat case*sex case*stat case*alco case*stat2/norow nopercent chisq;
run;

Proc Freq data = results1;
tables agecat*case sex*case stat*case alco*case/norow nopercent chisq;
run;

proc sort data = results1;
by case;
run;

proc means data = results1;
var age2;
by case;
run;

/*Importing additinal data on survival found from the Mayo Clinic*/

PROC IMPORT OUT= work.mayo2 DATAFILE= "E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets\MAYO.xlsx" DBMS=xlsx REPLACE;
  SHEET="Elig Test"; 
  GETNAMES=YES;
RUN;

proc print data = mayo2;
run;

data mayo2;
set mayo2 (rename=(Date_Last_Pt_Contact_or_Death = deathdte));
run;

proc print data = mayo2;
run;

proc contents data = results1;
run;

proc contents data = mayo2;
run;

proc print data = results1;
run;

proc sort data= mayo2;
by id;
run;

proc sort data = plasma2;
by id;
run;

proc sort data= results1;
by id;
run;

data mayo2;
set mayo2;
format deathdte mmddyy10.;
keep id deathdte;
run;

proc print data = mayo2;
run;

/*Merging Version2*/

proc freq data = results1;
tables case;
run;

proc sort data = results1;
by id;
run;

proc sort data = mayo2;
by id;
run;

data results2;
merge results1 mayo2;
by id;
run;

proc freq data = results2;
tables case;
run;

proc print data = results2;
run;

data results2;
set results2;
if id=168 then delete;
if case=1 and id le 6000 and deathdte ne . then period=deathdte-dx_date;
run;

proc contents data= results2;
run;

proc print data = results2 (obs=50);
run;

proc freq data = results2;
tables case;
run;

proc print data = results2;
where case = .;
run;

proc freq data = results2;
tables agecat;
run;

proc means data=results2 mean median min max;
var period;
run;

data results2;
set results2;
if deathdte ne . and period lt 1000 then status2 = 1; else status2 = 0;
run;

proc freq data = results2;
tables status2;
run;

*Dataset export for comparative analysis in STATA;

proc export data= results2
 outfile= "E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets\MICA_STATA"
 dbms=XLSX
 replace;
run;

/* preparing dataset for survival analysis by genetic information */

proc print data = results2;
run;

PROC IMPORT OUT= work.micadna DATAFILE= "E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets\MICA_SNP_DATASET.xlsx" DBMS=xlsx REPLACE;
  SHEET="MICA_SNP_DATASET"; 
  GETNAMES=YES;
RUN;

proc print data = micadna;
run;

data micadna;
set micadna;
keep ID Sample_Name rs1049174 rs2255336;
if ID = "." then delete;
run;

proc print data = micadna;
run;

proc sort data = micadna;
by ID;
run;

proc freq data = micadna;
tables rs1049174 rs2255336;
run;

data micadna;
set micadna;
if rs1049174 = "Unknown" then rs1049174 = "NA";
if rs2255336 = "Unknown" then rs2255336 = "NA";
run;

proc freq data = micadna;
tables rs1049174 rs2255336;
run;

/*Preparing a new dataset to merge with genotyping table*/

proc sort data = results1;
by id;
run;

proc sort data = mayo2;
by id;
run;

data results3;
merge results1 mayo2;
by id;
run;

proc freq data = results3;
tables case;
run;

proc sort data = results3;
by id;
run;

proc sort data = micadna;
by id;
run;

data dnasurvival;
merge results3 micadna;
by id;
run;

proc print data = dnasurvival;
run;

proc freq data = dnasurvival;
tables rs1049174 rs2255336;
run;

proc freq data = dnasurvival;
tables case;
run;

/* checking for duplicate IDs*/

PROC FREQ data = dnasurvival;
 TABLES ID / noprint out=keylist;
RUN;

PROC PRINT data = keylist;
 WHERE count ge 2;
RUN; 

PROC SORT data=dnasurvival NODUP;
 BY ID;
RUN; 

proc print data = dnasurvival;
run;

proc freq data = dnasurvival;
tables rs1049174 rs2255336;
run;

/*sorting by ID I found entries that had the same ID in our database, assigned to multiple genotyping sample names in the spreadsheet, so I will use the no dup key option to only keep the first entry since the genetic results are the same, only the sample name changes*/
 
PROC SORT data=dnasurvival NODUPKEY;
 BY ID;
RUN; 

proc print data = dnasurvival;
run;

proc freq data = dnasurvival;
tables rs1049174 rs2255336;
run;

proc freq data = dnasurvival;
tables case;
run;

data dnasurvival;
set dnasurvival;
if rs1049174 = "NA" then rs1049174 = " ";
if rs2255336 = "NA" then rs2255336 = " "; 
run;

/*Repeating the analysis of NKG2D and pancreatic cancer risk*/

proc freq data = dnasurvival;
tables case;
run;

proc freq data = dnasurvival;
tables case*rs1049174 case*rs2255336;
run;

proc contents data = dnasurvival;
run;

data dnasurvival;
set dnasurvival; 
label AGE = "recorded age";
label BDATE = "recorded birthdate";
label CASE = "case status";
label DEATHDTE = "recorded death date";
label DIABETES = "recorded diabetes history";
label DIAB_AGE = "recorded age of diabetes onset"; 
label DXSOURCE = "source of diagnosis";
label DX_DATE = "recorded date of diagnosis";
label EDUC = "educational status";
label ELIG = "eligibility status";
label FCONDATE = "recorded first contact date";
label MICA = "mica level in pg/ml";
label PACK_YRS = "number of pack-years";
label SEX = "recorded sex";
label STAT = "smoking status";
label micac = "mica re-coded as a categorical variable";
label stat2 = "Smoking recoded as smoker/non-smoker";
run; 

data micadataset;
set dnasurvival;
if sex = 'M' and age2 lt 70 then strata=1;
if sex = 'M' and age2 ge 70 then strata=2;
if sex = 'F' and age2 lt 70 then strata=3;
if sex = 'F' and age2 ge 70 then strata=4;
run;

proc freq data = micadataset;
table strata;
run;

data mylib.micafinal;
set micadataset;
run;

data micafinal;
set mylib.micafinal;
run;

proc contents data = micafinal;
run;

proc export data= micafinal
 outfile= "E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets\PANC_DATA_MICA_ANALYSIS"
 dbms=XLSX
 replace;
run;

*The missing cases come from the participants who had genetic SNP data but no MICA level measured; 
proc print data = micafinal;
where case = .;
run;

proc freq data = micafinal;
tables STAT DIABETES case*DIABETES;
run;

*recoding the diabetes variable for manuscript revisions;
data micafinal;
set micafinal;
if DIABETES = 3 then DIAB = 0; else DIAB =1;
run;

*Adding the MICA STR genetic dataset from Dr. Nathan Pankratz's Lab for the genetic analysis;

PROC IMPORT OUT= work.micastr DATAFILE= "E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets\final_db.xlsx" DBMS=xlsx REPLACE;
  SHEET="db"; 
  GETNAMES=YES;
RUN;

proc print data = micastr (obs=100);
run;

proc contents data = micastr;
run;

data micastr;
set micastr;
rename BlindHap1 = Allele1;
rename BlindHap2 = Allele2;
run;

proc contents data = micastr;
run;

data micastr;
set micastr (keep = A4_A4 A4_A5 A4_A6 A4_A9 A4_A5_1 A5_1_A5_1 A5_A5 A5_A6 A5_A9 A5_A5_1 A6_A6 A6_A9 A6_A5_1 A9_A9 A9_A5_1 AGE2 Allele1 Allele2 Affected DNA Male SomeCollege CurrentSmoker PastSmoker NeverSmoked Diabetes Genotype ID Index SAMPLE countA5_1 countA6_withUpstreamVariant dominantA9 dominantA5_1 maxNumRepeat minNumRepeat numA9 numA6_upstream numRepeats recessiveA9 recessiveA5_1 repeat1 repeat2 sum);
run;

data micastr;
set micastr;
ID2 = round(ID);
run;

proc freq data = micastr;
tables ID ID2;
run;

data micastr;
set micastr (drop = ID);
rename ID2 = ID;
rename Diabetes = Diabetes2;
run;

proc contents data = micastr;
run;

PROC LOGISTIC data = micastr;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 dominantA5_1 numRepeats;
RUN;

*Making A dataset for MICA STR analysis Manuscript;

proc sort data = micafinal;
by ID;
run;

proc sort data = micastr;
by ID;
run;

data micastrfinal;
merge micafinal micastr;
by ID;
run;

proc contents data = micastrfinal;
run;

proc freq data = micastrfinal;
tables case dominantA5_1 case*dominantA5_1 affected affected*dominantA5_1 case*affected;
run;

proc freq data = micastrfinal;
tables DIABETES EDUC SomeCollege CurrentSmoker PastSmoker NeverSmoked Diabetes2;
run;

***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

*restricting the dataset to the 541 observation with a call for A5.1 from Dr. Pankratz'a analysis;

data micastrfinal;
set micastrfinal;
if dominantA5_1 = . then delete;
run;
 
proc freq data = micastrfinal;
tables case dominantA5_1 case*dominantA5_1 affected affected*dominantA5_1 case*affected;
run;

***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

*Testing the alco variable from the manuscript dataset;

*Manuscript Revisions;

options ps = 3000 ls = 120 nofmterr;
libname mylib2 'E:\Professional Folder\WBOB\MICA Study\Preliminary Analysis\SAS Datasets';

data micafinalmk1;
set  mylib2.micafinalmk1;
run;

proc contents data = micafinalmk1;
run;

*The missing cases come from the participants who had genetic SNP data but no MICA level measured; 
proc print data = micafinalmk1;
where case = .;
run;

*recoding the diabetes variable for manuscript revisions;
data micafinalmk1;
set micafinalmk1;
if DIABETES = 3 then DIABETES3 = 0; else DIABETES3 =1;
rename alco = alcohol;
run;

proc freq data = micafinalmk1;
tables STAT DIABETES3 case*DIABETES3 alcohol;
run;

data micafinalmk1;
set micafinalmk1 (KEEP = ID DIABETES3 ALCOHOL);
run;

proc print data = micafinalmk1;
run;

proc sort data = micafinalmk1;
by ID;
run;

proc sort data = micastrfinal;
by ID;
run;

*Trying to force SAS to only merge the 541 observation from the SNP data, because the other merge seems to create other observations?;

proc freq data = micastrfinal;
tables case dominantA5_1 case*dominantA5_1 affected affected*dominantA5_1 case*affected;
run;

data micastrfinal;
set micastrfinal;
if dominantA5_1 ne . then a = 1;
run;

data micafinalmk1;
set micafinalmk1;
if DIABETES3 ne . then b = 1;
run;

proc freq data = micastrfinal;
tables case dominantA5_1 affected a;
run;

/*
data micastrfinal;
merge micastrfinal micafinalmk1;
by ID;
run;
*/

proc freq data = micastrfinal;
tables case dominantA5_1 affected a b;
run;

proc freq data = micastrfinal;
tables case case*alco case*alcohol affected affected*alco affected*alcohol;
run;

proc freq data = micastrfinal;
tables case dominantA5_1 case*dominantA5_1 affected affected*dominantA5_1 case*affected;
run;

*Survival Analysis prep;

data micastrfinal;
set micastrfinal;
if id=168 then delete;
if case=1 and id le 6000 and deathdte ne . then period=deathdte-dx_date;
run;

proc contents data= micastrfinal;
run;

proc print data = micastrfinal (obs=50);
run;

proc freq data = micastrfinal;
tables case;
run;

proc print data = micastrfinal;
where case = .;
run;

proc freq data = micastrfinal;
tables agecat;
run;

proc means data=micastrfinal mean median min max;
var period;
run;

data micastrfinal;
set micastrfinal;
if deathdte ne . and period lt 1000 then status2 = 1; else status2 = 0;
run;

proc freq data = micastrfinal;
tables status2;
run;

***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

proc contents data = micastrfinal;
run;

/* Dataset description:

- Results1 = PANC + MICA plasma
- Results2 = Results1 + Mayo Death Cases
- DNAsurvival = Results1 + Mayo Death Cases + NKG2D DNA
- Micafinal = DNAsurvival
- Micastrfinal = micafinal + MICA STR dataset
- micastrfinal = micastrfinal + Diabetes + Alcohol comparision from MICA manuscript

Outcome
- case (control = 1, case = 2)
- panc (control = 0, case = 1) 
- affected (control = 0, case = 1)
- status2 (death)
- period (survival)

Exposure
- micac (mica re-coded as a categorical variable: if 0 le mican le 2 then micac=0; else if 2 lt mican le 32.10 then micac=1; else if 32.14 le mican le 64.7 then micac=2; else if mican ge 64.8 then micac=3;)
- micad (mica re-coded as a dicothomous variable: if 0 le mican le 2 then micad = 0; else micad = 1;)
- mican (mica re-coded as a numeric variable)

- Allele1 (Allele number 1: can be A4, A5, A5.1, A6, A9)
- Allele2 (Allele number 2: can be A4, A5, A5.1, A6, A9)
- numRepeats (Number of repeats in sequenced gene)
- dominantA9  (A9 allele coded as dominant = 1)
- dominantA5_1 (A5.1 allele coded as dominant = 1)

Covariates
- age2 (Continuous Age)
- agecat (Categorical Age cutoff at 70)
- Male (Indicator variable for gender)
- SEX (Dicothomous variable for gender)
- alco (alcohol variable recoded from sumliq in the initial dataset)
- alcohol (alcohol variable from original manuscript for comparision)
- NeverSmoked (Indicator variable for smoking)
- PastSmoker (Indicator variable for smoking)
- CurrentSmoker (Indicator variable for smoking)
- STAT (Categorical variable for smoking)
- STAT2 (dicothomous variable for smoking)
- EDUC (Categorical variable for education)
- SomeCollege (indicator variable for education)
- Diabetes (Original categorical variable for diabetes)
- Diabetes2 (Dicothomous diabetes variable from Dr. Pankratz)
- Diabetes3 (Dicothomous diabetes variable from original manuscript for comparison)

*/

/* Data cleaning*/

*Making dominant and additive variables for every allele (A4 A5 A5.1 A6 A9);

proc freq data = micastrfinal;
tables allele1 allele2 dominantA5_1;
run;

data micastrfinal;
set micastrfinal;
if dominantA5_1 = . then delete;
run;

proc freq data = micastrfinal;
tables allele1 allele2 dominantA5_1;
run;

data micastrfinal;
set micastrfinal;
if allele1 = "A4" then count1A4 = 1; else count1A4 = 0;
if allele1 = "A5" then count1A5 = 1; else count1A5 = 0;
if allele1 = "A5.1" then count1A5_1 = 1; else count1A5_1 = 0;
if (allele1 = "A6" or allele1 = "A6_withUpstreamVariant") then count1A6 = 1; else count1A6 = 0;
if allele1 = "A9" then count1A9 = 1; else count1A9 = 0;
run;

proc freq data = micastrfinal;
tables count1A4 count1A5 count1A5_1 count1A6 count1A9;
run;

data micastrfinal;
set micastrfinal;
if allele2 = "A4" then count2A4 = 1; else count2A4 = 0;
if allele2 = "A5" then count2A5 = 1; else count2A5 = 0;
if allele2 = "A5.1" then count2A5_1 = 1; else count2A5_1 = 0;
if (allele2 = "A6" or allele2 = "A6_withUpstreamVariant") then count2A6 = 1; else count2A6 = 0;
if allele2 = "A9" then count2A9 = 1; else count2A9 = 0;
run;

proc freq data = micastrfinal;
tables count2A4 coun21A5 count2A5_1 count2A6 count2A9;
run;

data micastrfinal;
set micastrfinal;
additiveA4 = count1A4 + count2A4;
additiveA5 = count1A5 + count2A5;
additiveA5_1 = count1A5_1 + count2A5_1;
additiveA6 = count1A6 + count2A6;
additiveA9 = count1A9 + count2A9;
run;

proc freq data = micastrfinal;
tables additiveA4 additiveA5 additiveA5_1 additiveA6 additiveA9;
run;

data micastrfinal;
set micastrfinal;
if additiveA4 ge 1 then dominantA4 = 1; else dominantA4 = 0;
if additiveA5 ge 1 then dominantA5 = 1; else dominantA5 = 0;
if additiveA6 ge 1 then dominantA6 = 1; else dominantA6 = 0;
if additiveA9 ge 1 then dominantA9 = 1; else dominantA9 = 0;
run;

proc freq data = micastrfinal;
tables dominantA4 dominantA5 dominantA5_1 dominantA6 dominantA9;
run;

data mylib.micastrfinal;
set micastrfinal;
run;

/***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************/

options ps = 3000 ls = 120 nofmterr;
libname mylib 'E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets';

data micastrfinal;
set mylib.micastrfinal;
run;

proc contents data = micastrfinal;
run;

*Descriptive statistics;

proc freq data = micastrfinal;
tables additiveA4 additiveA5 additiveA5_1 additiveA6 additiveA9;
run;

proc freq data = micastrfinal;
tables dominantA4 dominantA5 dominantA5_1 dominantA6 dominantA9;
run;

proc freq data = micastrfinal;
tables micac micad;
run;

proc freq data = micastrfinal;
tables affected*dominantA4 affected*dominantA5 affected*dominantA5_1 affected*dominantA6 affected*dominantA9 / chisq;
run;

proc freq data = micastrfinal;
tables affected*additiveA4 affected*additiveA5 affected*additiveA5_1 affected*additiveA6 affected*additiveA9 / chisq;
run;

proc means data = micastrfinal min mean median max std;
var mican;
run;

proc univariate NORMAL PLOT data=micastrfinal; var mican;
histogram mican/normal;
run;

/***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************/

*MICA Genetic Analysis Dataset;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets';

data micastrfinal;
set mylib.micastrfinal;
run;

proc contents data = micastrfinal;
run;

proc freq data = micastrfinal;
tables affected;
run;

proc freq data = micastrfinal;
tables affected*dominantA4 affected*dominantA5 affected*dominantA5_1 affected*dominantA6 affected*dominantA9 / chisq;
run;

/*
data micastrfinal;
set micastrfinal;
if affected = . then delete;
if age2 = . then delete;
if Male = . then delete;
if Diabetes2 = . then delete;
if alco = . then delete;
run;

proc freq data = micastrfinal;
tables affected;
run;

*/

/********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************/

*Descriptive statistics;

*Figure 1: Distribution of MICA STR polymorphisms in the PANC study;

proc freq data = micastrfinal;
tables additiveA4 additiveA5 additiveA5_1 additiveA6 additiveA9;
run;

proc freq data = micastrfinal;
tables dominantA4 dominantA5 dominantA5_1 dominantA6 dominantA9;
run;

proc freq data = micastrfinal;
tables micac micad;
run;

proc means data = micastrfinal min mean median max std;
var mican;
run;

proc univariate NORMAL PLOT data=micastrfinal; var mican;
histogram mican/normal;
run;

proc univariate NORMAL PLOT data=micastrfinal; 
var dominantA5_1;
histogram dominantA5_1;
run;

*Figure 2: Distribution of s-MICA levels by MICA STR polymorphism in the PANC study;

*Testing out a method to directly calculate the geometic means for s-MICA using proc ttest and the lognormal option;
*GLM + Genmod: Unweighted Means that only account for X and Y;
*GLM + Genmod: Least Square Means (LS) adjusted for covariates in the models (can be used for both the gaussian and log distribution;
*TTest: Geometric means which adjust for abnormally large value by using a lognormal distribution;

proc ttest data=micastrfinal dist=lognormal;
class dominantA5_1;
var mican;
run;

proc ttest data=micastrfinal dist=lognormal;
where micac ne 0 and affected = 0;
class dominantA5_1;
var mican;
run;

proc ttest data=micastrfinal dist=lognormal;
where micac ne 0 and affected = 1;
class dominantA5_1;
var mican;
run;

*For additive A5.1 models, you will need to use proc survey mean and manually compute the geometric means since it has more than 2 levels (0, 1 and 2) for additiveA5_1;
*Using the procsurvey means so I can use the allgeo fuction to directly request the geometric means and 95% CIs without having to manually calculate them as follows;

data example;
set example;
logClostridium = log(clostridium);
run;

proc surveymeans data=example;
weight wgt;
strata plant;
cluster shift;
var logClostridium;
ods output statistics = estimates;
run;

data estimates;
set estimates;
Mean = exp(Mean);
StdErr = sqrt((Mean**2)*(StdErr**2));
LowerCLMean = exp(LowerCLMean);
UpperCLMean = exp(UpperCLMean);
VarName = 'Clostridium';
label Mean='Geometric Mean'
StdErr='Standard Error'
LowerCLMean='Lower 95% Confidence Limit'
UpperCLMean='Upper 95% Confidence Limit';
run;

*Using the procsurveymeans allgeo option instead;

data overall;
set micastrfinal;
where mican gt 0;
run;

proc sort data = overall;
by additiveA5_1;
run;

proc surveymeans data=overall allgeo;
by additiveA5_1;
var mican;
ods output statistics = overall1;
run; 

data cases;
set micastrfinal;
where mican gt 0 and affected = 1;
run;

proc sort data = cases;
by additiveA5_1;
run;

proc surveymeans data=cases allgeo;
by additiveA5_1;
var mican;
ods output statistics = cases1;
run; 

data controls;
set micastrfinal;
where mican gt 0 and affected = 0;
run;

proc sort data = controls;
by additiveA5_1;
run;

proc surveymeans data=controls allgeo;
by additiveA5_1;
var mican;
ods output statistics = controls1;
run; 

*Table 1;

proc sort data = micastrfinal;
by affected;
run;

proc freq data = micastrfinal;
tables Male agecat SomeCollege NeverSmoked PastSmoker Currentsmoker Diabetes2 alco / chisq norow;
by affected;
run;

proc freq data = micastrfinal;
tables Male*affected agecat*affected SomeCollege*affected STAT2*affected Diabetes2*affected alco*affected / chisq;
run;

/********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************/

* Multivariate analyses testing by repeating the analysis from Dr. Pankratz;

/*Testing 2.0: Remaking the micac categories to match the poster values exactly*/

*Adjusted for age and sex;

PROC LOGISTIC data= micastrfinal;
class SEX dominantA5_1 (ref='0');
MODEL CASE (EVENT='1')= dominantA5_1 age2 SEX;
RUN;

PROC LOGISTIC data= micastrfinal;
class SEX dominantA5_1 (ref='0');
MODEL affected (EVENT='1')= dominantA5_1 age2 SEX;
RUN;

/*Adjusted for Age, sex, smoking status and pack years*/

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') dominantA5_1 (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS alco dominantA5_1;
RUN;

/*Full model with diabetes*/

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3') dominantA5_1 (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS DIABETES alco dominantA5_1 sum;
RUN;

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3') dominantA5_1 (ref='0');
MODEL CASE (EVENT='1')= age SEX stat DIABETES alco dominantA5_1 sum;
RUN;

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age SEX stat DIABETES alco dominantA5_1 sum;
RUN;

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3');
MODEL Affected (EVENT='1')= age SEX stat DIABETES alco dominantA5_1 sum;
RUN;

*Repeating the analysis from Dr. Pankratz;

proc freq data = micastrfinal;
tables affected*dominantA5_1;
run;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0') micac (ref = '1');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats micac;
RUN;

proc freq data = micastrfinal;
where diabetes ne 1;
tables affected;
run;

proc freq data = micastrfinal;
where diabetes ne 1;
tables dominantA5_1*affected;
run;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker alco dominantA5_1 numRepeats;
RUN;

/***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************/

/* Stratified Analyses */

*Age;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where agecat = "1";
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where agecat = "2";
run;

PROC LOGISTIC data = micastrfinal;
where agecat = "1";
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where agecat = "2";
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

*Sex;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Male = 0;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Male = 1;
run;

PROC LOGISTIC data = micastrfinal;
where Male = 0;
class SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where Male = 1;
class SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

*Education;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where SomeCollege = 0;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where SomeCollege = 1;
run;

PROC LOGISTIC data = micastrfinal;
where SomeCollege = 0;
class Male(ref="0") NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where SomeCollege = 1;
class Male(ref="0") NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

*Smoking;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Neversmoked = 1;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Pastsmoker = 1;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where CurrentSmoker = 1;
run;

PROC LOGISTIC data = micastrfinal;
where NeverSmoked = 1;
class Male(ref="0") SomeCollege (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where PastSmoker = 1;
class Male(ref="0") SomeCollege (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where CurrentSmoker = 1;
class Male(ref="0") SomeCollege (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege Diabetes2 alco dominantA5_1 numRepeats;
RUN;

*Alcohol;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where alco = 0;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where alco = 1;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where alco = 2;
run;

PROC LOGISTIC data = micastrfinal;
where alco = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where alco = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0')  dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where alco = 2;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 dominantA5_1 numRepeats;
RUN;

*Alcohol without diabetes to improve model stability;

PROC LOGISTIC data = micastrfinal;
where alco = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where alco = 2;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker dominantA5_1 numRepeats;
RUN;

*Diabetes;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Diabetes2 = 0;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Diabetes2 = 1;
run;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker alco dominantA5_1 numRepeats;
RUN;

*Repeating the diabetes stratified analysis without alcohol for model stability;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker dominantA5_1 numRepeats;
RUN;

/********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************/

*Multivariate models testing the best distribution to use for the analysis of MICA and MICA genotypes;

*Correlation between mica levels and mica genotype;

proc means data = micastrfinal min mean median max std;
var mican;
run;

proc univariate NORMAL PLOT data=micastrfinal; var mican;
histogram mican/normal;
run;

*Mican has a non normal distribution: Making a log transformed mica and categories over the whole distribution;
*The manual log transformation does not allow for 0 values for mican since log of 0 is undefined, but in the model we will use poisson regression to address this (negative binomial distribution);

data micastrfinal;
set micastrfinal;
if mican = . then delete;
run;

proc means data = micastrfinal min mean median max std;
var mican;
run;

data micastrfinal;
set micastrfinal;
micalog = log(mican);
run;

proc print data = micastrfinal;
where mican = 0;
var id mican affected;
run;

proc print data = micastrfinal;
where micalog = .;
var id mican affected micalog;
run;

proc univariate NORMAL PLOT data=micastrfinal; var mican micalog;
histogram mican micalog/normal;
run;

proc freq data = micastrfinal;
tables micac;
run;

proc means data = micastrfinal min mean median max std;
var mican micalog;
by micac;
run; 

*Trying a correlation analysis between mica circulating levels and mica genotype;

proc corr data = micastrfinal sscp cov plots=matrix;
var micac dominantA5_1;
run;

proc corr data = micastrfinal sscp cov plots=matrix;
where micac ne 0;
var micac dominantA5_1;
run;

*Trying a genmod with normal distribution to emulate a glm with identity link;
 
PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
RUN;

*Trying a multinomial regression model with mica in tertiles over the cutoff of 2 pg/mL;

proc freq data = micastrfinal;
tables micac*dominantA5_1;
where micac ne 0;
run;

proc freq data = micastrfinal;
tables micac*dominantA5_1;
where micac ne 0 and affected = 0;
run;

proc logistic data = micastrfinal descending;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL micac = age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

proc logistic data = micastrfinal descending;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL micac = age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

/***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************/

*Table 2:  Association between circulating MICA levels and the MICA A5.1 polymorphism (dominant model); 

*Testing out a method to directly calculate the geometic means for s-MICA using proc ttest and the lognormal option;
*GLM + Genmod: Unweighted Means that only account for X and Y;
*GLM + Genmod: Least Square Means (LS) adjusted for covariates in the models (can be used for both the gaussian and log distribution;
*TTest: Geometric means which adjust for abnormally large value by using a lognormal distribution;

proc ttest data=micastrfinal dist=lognormal;
class dominantA5_1;
var mican;
run;

proc ttest data=micastrfinal dist=lognormal;
where micac ne 0 and affected = 0;
class dominantA5_1;
var mican;
run;

proc ttest data=micastrfinal dist=lognormal;
where micac ne 0 and affected = 1;
class dominantA5_1;
var mican;
run;

*Trying a genmod with normal distribution to emulate a glm with identity link (overall, cases, controls);
*The LSmeans options give you the estimate (linear scale) and the exponentiated values (multiplicative scale);

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

*Trying a genmod with poisson distribution to emulate a logtransformation (overall, cases, controls);

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = poisson;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = poisson;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = poisson;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

proc freq data = micastrfinal;
tables dominantA5_1*micac;
run;

proc freq data = micastrfinal;
tables dominantA5_1*micac;
where affected = 0;
run;

proc freq data = micastrfinal;
tables dominantA5_1*micac;
where affected = 1;
run;

PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = poisson;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = poisson;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = poisson;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

*Trying the analysis with mica as a categorical variable;

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
RUN;

PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
RUN;

PROC genmod data = micastrfinal;
where affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
RUN;

/***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************/

*Table 3. Odds ratio and 95% confidence interval (95%CI) for pancreatic cancer associated with the MICA A5.1 polymorphism (dominant model);

*Adjusted for age and sex;

PROC LOGISTIC data= micastrfinal;
class SEX dominantA5_1 (ref='0');
MODEL CASE (EVENT='1')= dominantA5_1 age2 SEX;
RUN;

PROC LOGISTIC data= micastrfinal;
class SEX dominantA5_1 (ref='0');
MODEL affected (EVENT='1')= dominantA5_1 age2 SEX;
RUN;

/*Adjusted for Age, sex, smoking status and pack years*/

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') dominantA5_1 (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS alco dominantA5_1;
RUN;

/*Full model with diabetes*/

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3') dominantA5_1 (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS DIABETES alco dominantA5_1 sum;
RUN;

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3') dominantA5_1 (ref='0');
MODEL CASE (EVENT='1')= age SEX stat DIABETES alco dominantA5_1 sum;
RUN;

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age SEX stat DIABETES alco dominantA5_1 sum;
RUN;

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3');
MODEL Affected (EVENT='1')= age SEX stat DIABETES alco dominantA5_1 sum;
RUN;

*Repeating the analysis from Dr. Pankratz;

proc freq data = micastrfinal;
tables affected*dominantA5_1;
run;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0') micac (ref = '1');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats micac;
RUN;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
run;

proc freq data = micastrfinal;
where diabetes ne 1;
tables affected;
run;

proc freq data = micastrfinal;
where diabetes ne 1;
tables dominantA5_1*affected;
run;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker alco dominantA5_1 numRepeats;
RUN;

/***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************/

*Supplemental Table 1. Association between circulating MICA levels and the MICA A5.1 polymorphism (additive model);
 
*For additive A5.1 models, you will need to use proc survey mean and manually compute the geometric means since it has more than 2 levels (0, 1 and 2) for additiveA5_1;
*Using the procsurvey means so I can use the allgeo fuction to directly request the geometric means and 95% CIs without having to manually calculate them as follows;

data example;
set example;
logClostridium = log(clostridium);
run;

proc surveymeans data=example;
weight wgt;
strata plant;
cluster shift;
var logClostridium;
ods output statistics = estimates;
run;

data estimates;
set estimates;
Mean = exp(Mean);
StdErr = sqrt((Mean**2)*(StdErr**2));
LowerCLMean = exp(LowerCLMean);
UpperCLMean = exp(UpperCLMean);
VarName = 'Clostridium';
label Mean='Geometric Mean'
StdErr='Standard Error'
LowerCLMean='Lower 95% Confidence Limit'
UpperCLMean='Upper 95% Confidence Limit';
run;

*Using the procsurveymeans allgeo option instead;

data overall;
set micastrfinal;
where mican gt 0;
run;

proc sort data = overall;
by additiveA5_1;
run;

proc surveymeans data=overall allgeo;
by additiveA5_1;
var mican;
ods output statistics = overall1;
run; 

data controls;
set micastrfinal;
where mican gt 0 and affected = 0;
run;

proc sort data = controls;
by additiveA5_1;
run;

proc surveymeans data=controls allgeo;
by additiveA5_1;
var mican;
ods output statistics = controls1;
run; 

data cases;
set micastrfinal;
where mican gt 0 and affected = 1;
run;

proc sort data = cases;
by additiveA5_1;
run;

proc surveymeans data=cases allgeo;
by additiveA5_1;
var mican;
ods output statistics = cases1;
run; 

* Trying the analyses with additive genetic models for MICA;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') additiveA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 additiveA5_1 numRepeats;
RUN;

proc freq data = micastrfinal;
tables additiveA5_1*Affected;
run;
 
PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats;
RUN;

proc freq data = micastrfinal;
tables additiveA5_1*Affected;
where diabetes2 = 0;
run;

PROC LOGISTIC data = micastrfinal;
where diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats;
RUN;

*Trying a multinomial regression model with mica in tertiles over the cutoff of 2 pg/mL for additive genetic MICA;

proc freq data = micastrfinal;
tables additiveA5_1*micac;
where micac ne 0;
run;

proc logistic data = micastrfinal descending;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL micac = age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats;
RUN;

proc freq data = micastrfinal;
tables additiveA5_1*micac;
where micac ne 0 and affected = 0;
run;

proc logistic data = micastrfinal descending;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL micac = age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats;
RUN;

*Repeating the s-MICA and MICA polymorphisms analyses using an additive model;

*Trying a genmod with normal distribution to emulate a glm with identity link (overall, cases, controls);

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

*Trying a genmod with poisson distribution to emulate a logtransformation (overall, cases, controls);

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = poisson;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = poisson;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = poisson;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

**I had to condition on A5_1 because when I created the additive variable, it did not take into account the missing observations. However, it did not affect the models becase the mica and the affected variable would condition who got included in the models;

proc freq data = micastrfinal;
tables affected;
run;

proc freq data = micastrfinal;
tables affected*dominantA5_1;
run;

proc freq data = micastrfinal;
tables additiveA5_1*micac;
where dominantA5_1 ne .;
run;

proc freq data = micastrfinal;
tables additiveA5_1*micac;
where dominantA5_1 ne . and affected = 0;
run;

proc freq data = micastrfinal;
tables additiveA5_1*micac;
where dominantA5_1 ne . and affected = 1;
run;

PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = poisson;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = poisson;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = poisson;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

*Trying the analysis with mica as a categorical variable;

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
RUN;

PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
RUN;

PROC genmod data = micastrfinal;
where affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL micac= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal;
RUN;

/********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************/

* Supplemental Table 2. Odds ratio and 95% confidence interval (95%CI) for pancreatic cancer associated with the MICA A5.1 polymorphism (additive model);

*Adjusted for age and sex;

PROC LOGISTIC data= micastrfinal;
class SEX additiveA5_1(ref='0');
MODEL CASE (EVENT='1')= additiveA5_1age2 SEX;
RUN;

PROC LOGISTIC data= micastrfinal;
class SEX additiveA5_1(ref='0');
MODEL affected (EVENT='1')= additiveA5_1age2 SEX;
RUN;

/*Adjusted for Age, sex, smoking status and pack years*/

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') additiveA5_1(ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS alco dominantA5_1;
RUN;

/*Full model with diabetes*/

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3') additiveA5_1(ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS DIABETES alco additiveA5_1sum;
RUN;

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3') additiveA5_1(ref='0');
MODEL CASE (EVENT='1')= age SEX stat DIABETES alco additiveA5_1sum;
RUN;

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3') additiveA5_1(ref='0');
MODEL Affected (EVENT='1')= age SEX stat DIABETES alco additiveA5_1sum;
RUN;

PROC LOGISTIC data = micastrfinal;
where STAT ne 4;
class SEX stat (ref='1') alco (ref='0') DIABETES (ref = '3');
MODEL Affected (EVENT='1')= age SEX stat DIABETES alco additiveA5_1 sum;
RUN;

*Repeating the analysis from Dr. Pankratz;

proc freq data = micastrfinal;
tables affected*dominantA5_1;
run;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") additiveA5_1(ref='0');
MODEL Affected (EVENT='1')= age2 Male additiveA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') additiveA5_1(ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 additiveA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1(ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') alco (ref='0') additiveA5_1(ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker alco additiveA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1(ref='0') micac (ref = '1');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats micac;
RUN;

proc freq data = micastrfinal;
tables additiveA5_1*affected;
run;

proc freq data = micastrfinal;
where diabetes ne 1;
tables affected;
run;

proc freq data = micastrfinal;
where diabetes ne 1;
tables additiveA5_1*affected;
run;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') alco (ref='0') additiveA5_1(ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker alco additiveA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') alco (ref='0') additiveA5_1(ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker additiveA5_1 numRepeats;
RUN;

/********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************/

*Supplemental Table 3. Association between the MICA A5.1 polymorphism (dominant model) and pancreatic cancer risk, stratified by age, sex, education, diabetes status, smoking status, and alcohol consumption;

/* Stratified Analyses */

*Age;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where agecat = "1";
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where agecat = "2";
run;

PROC LOGISTIC data = micastrfinal;
where agecat = "1";
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where agecat = "2";
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

*Sex;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Male = 0;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Male = 1;
run;

PROC LOGISTIC data = micastrfinal;
where Male = 0;
class SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where Male = 1;
class SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

*Education;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where SomeCollege = 0;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where SomeCollege = 1;
run;

PROC LOGISTIC data = micastrfinal;
where SomeCollege = 0;
class Male(ref="0") NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where SomeCollege = 1;
class Male(ref="0") NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

*Smoking;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Neversmoked = 1;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Pastsmoker = 1;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where CurrentSmoker = 1;
run;

PROC LOGISTIC data = micastrfinal;
where NeverSmoked = 1;
class Male(ref="0") SomeCollege (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where PastSmoker = 1;
class Male(ref="0") SomeCollege (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where CurrentSmoker = 1;
class Male(ref="0") SomeCollege (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege Diabetes2 alco dominantA5_1 numRepeats;
RUN;

*Alcohol;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where alco = 0;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where alco = 1;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where alco = 2;
run;

PROC LOGISTIC data = micastrfinal;
where alco = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where alco = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0')  dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where alco = 2;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 dominantA5_1 numRepeats;
RUN;

*Alcohol without diabetes to improve model stability;

PROC LOGISTIC data = micastrfinal;
where alco = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where alco = 2;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker dominantA5_1 numRepeats;
RUN;

*Diabetes;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Diabetes2 = 0;
run;

proc freq data = micastrfinal;
tables dominantA5_1*affected;
where Diabetes2 = 1;
run;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker alco dominantA5_1 numRepeats;
RUN;

*Repeating the diabetes stratified analysis without alcohol for model stability;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
where Diabetes2 = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker dominantA5_1 numRepeats;
RUN;

*Models for interaction p-values;

*Age;

PROC LOGISTIC data = micastrfinal;
class agecat Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= agecat Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats dominantA5_1*agecat;
RUN;

*Sex;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats dominantA5_1*Male;
RUN;

*Education;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male NeverSmoked PastSmoker Diabetes2 alco SomeCollege dominantA5_1 numRepeats dominantA5_1*SomeCollege;
RUN;

*Smoking;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege Diabetes2 alco dominantA5_1 numRepeats dominantA5_1*NeverSmoked;
RUN;

*Alcohol without diabetes to improve model stability;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker alco dominantA5_1 numRepeats dominantA5_1*alco;
RUN;

*Diabetes without alcohol for model stability;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 dominantA5_1 numRepeats dominantA5_1*Diabetes2;
RUN;

/********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************
********************************************************************************************************************************************************************************************************************************************************************************************************************/

*Making supplemental tables for the other MICA STR polymorphisms at Dr. Prizment's request;

*Supplemental Table 4:  Association between circulating MICA levels and other MICA STR polymorphisms (dominant model); 

proc freq data = micastrfinal;
tables dominantA4 dominantA5 dominantA5_1 dominantA6 dominantA9;
run;

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA4 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA4 numRepeats / dist = poisson;
lsmeans dominantA4 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5 numRepeats / dist = poisson;
lsmeans dominantA5 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA6 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA6 numRepeats / dist = poisson;
lsmeans dominantA6 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA9 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA9 numRepeats / dist = poisson;
lsmeans dominantA9 / corr exp diff cl;
RUN;

*Supplemental Table 5:  Association between circulating MICA levels and other MICA STR polymorphisms (dominant model); 

proc freq data = micastrfinal;
tables dominantA4*affected dominantA5*affected dominantA5_1*affected dominantA6*affected dominantA9*affected;
run;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA4 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA4 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA6 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA6 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA9 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA9 numRepeats;
RUN;

/***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************/

/* Survival Analysis */

/* ods graphics on;
proc lifetest data=Exposed plots=(survival(atrisk) logsurv);
   time Days*Status(0);
   strata Treatment;
run;
ods graphics off;
In the TIME statement, the survival time variable, Days, is crossed with the censoring variable, Status, with the value 0 indicating censoring. That is, the values of Days are considered censored if the corresponding values of Status are 0; otherwise, they are considered as event times. In the STRATA statement, the variable Treatment is specified, which indicates that the data are to be divided into strata based on the values of Treatment. ODS Graphics must be enabled before producing graphs. Two plots are requested through the PLOTS= optionóa plot of the survival curves with at risk numbers and a plot of the negative log of the survival curves*/

/*Modified censoring criteria*/

ODS Graphics on;
proc lifetest  data=micastrfinal method=life plots=(s) intervals = 0 to 180 by 20, 180 to 270 by 30, 270 to 1000 by 100;
time period*status2(0);
strata dominantA5_1;
run;
ODS Graphics off;

/***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************/

proc freq data = micafinal;
where SEX='M';
tables Case*micac;
run;

proc freq data = micafinal;
where SEX='F';
tables Case*micac;
run;

proc freq data = micafinal;
tables Case*micac;
run;

proc freq data = micafinal;
tables MICAC*SEX;
run;

PROC LOGISTIC data = micafinal;
where STAT ne 4;
class SEX stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age SEX stat micac PACK_YRS SEX*MICAC;
oddsratio micac;
RUN;

PROC LOGISTIC data = micafinal;
WHERE SEX='M' and STAT ne 4;
class SEX stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')= age SEX stat micac PACK_YRS;
RUN;

PROC LOGISTIC data = micafinal;
WHERE SEX='F' and STAT ne 4;
class SEX micac (ref='1') stat (ref='1') stat micac (ref='1');
MODEL CASE (EVENT='1')= age SEX stat micac PACK_YRS;
RUN;

/*Interaction with Age*/

proc freq data = micafinal;
where age lt 70;
tables Case*micac;
run;

proc freq data = micafinal;
where age ge 70;
tables Case*micac;
run;

proc freq data = micafinal;
tables Case*agecat;
run;

proc freq data = micafinal;
tables MICAC*agecat;
run;

PROC LOGISTIC data = micafinal;
where STAT ne 4;
class SEX stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')= age SEX stat micac PACK_YRS age*micac;
oddsratio micac;
RUN;

PROC LOGISTIC data = micafinal;
WHERE age lt 70 and STAT ne 4;
class SEX stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')= SEX stat micac PACK_YRS;
RUN;

PROC LOGISTIC data = micafinal;
WHERE age ge 70 and STAT ne 4;
class SEX stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')= SEX stat micac PACK_YRS;
RUN;

*full model with diabetes;
PROC LOGISTIC data = micafinal;
where STAT ne 4;
class SEX stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac DIABETES alco;
RUN;

*Interaction with Diabetes;
PROC LOGISTIC data = micafinal;
where STAT ne 4 and DIABETES ne 8;
class SEX stat (ref='1') micac (ref='1') alco (ref='0') DIABETES (ref='3');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS alco micac DIABETES micac*DIABETES;
oddsratio micac;
RUN;

*Stratified analysis among diabetic participants;

*Diabetics (include long term & recent diabetics);
PROC LOGISTIC data = micafinal;
where STAT ne 4 and DIABETES ne 3;
class SEX stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac alco;
RUN;

*Non Diabetics;
PROC LOGISTIC data = micafinal;
where STAT ne 4 and DIABETES = 3;
class SEX stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac alco;
RUN;

*Final non-diabetic model;
PROC LOGISTIC data = micafinal;
where STAT ne 4 and DIABETES = 3;
class SEX stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac;
RUN;

*repeating sex significant analyses after adjusting for diabetes;

PROC LOGISTIC data = micafinal;
WHERE SEX='M' and STAT ne 4;
class stat (ref='1') micac (ref='1') DIABETES (ref='3');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS DIABETES;
RUN;

PROC LOGISTIC data = micafinal;
WHERE SEX='F' and STAT ne 4;
class SEX micac (ref='1') stat (ref='1') stat DIABETES (ref='3');
MODEL CASE (EVENT='1')= age SEX stat micac PACK_YRS DIABETES;
RUN;

*recoding the DIABETES variable to help with model convergence issues;

PROC LOGISTIC data = micafinal;
WHERE STAT ne 4;
class SEX stat (ref='1') micac (ref='1') DIAB (ref='0');
MODEL CASE (EVENT='1')= age sex stat micac PACK_YRS DIAB micac*sex;
RUN;

PROC LOGISTIC data = micafinal;
WHERE SEX='M' and STAT ne 4;
class stat (ref='1') micac (ref='1') DIAB (ref='0');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS DIAB;
RUN;

PROC LOGISTIC data = micafinal;
WHERE SEX='F' and STAT ne 4;
class micac (ref='1') stat (ref='1') stat DIAB (ref='0');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS DIAB;
RUN;

**repeating sex stratified analyses among diabetics;
*We do not have a large enough sample to report results from the gender stratified analyis among diabetic participants;

PROC LOGISTIC data = micafinal;
WHERE SEX='M' and STAT ne 4 and DIAB = 1;
class SEX stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

PROC LOGISTIC data = micafinal;
WHERE SEX='F' and STAT ne 4 and DIAB = 1;
class SEX stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

*repeating sex stratified analyses among non diabetics;

PROC LOGISTIC data = micafinal;
WHERE SEX='M' and STAT ne 4 and DIAB = 0;
class SEX stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

PROC LOGISTIC data = micafinal;
WHERE SEX='F' and STAT ne 4 and DIAB = 0;
class SEX stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

*Updating manuscript tables with analysis results;

proc freq data=micafinal;
tables diab diab*case/chisq;
run;

proc freq data=micafinal;
tables micac micac*case;
run;

proc print data = micafinal;
var mican micac case;
run;

*The mica final dataset matches the manuscript exclusion criteria so use these numbers to update N in your tables;

data micafinal;
set mylib.micafinal;
run;

proc contents data = micafinal;
run;

data micafinal;
set micafinal;
if DIABETES = 3 then DIAB = 0; else DIAB =1;
run;

data micafinal;
set micafinal;
if 0 le mican le 2 then micac=0;
else if 2 lt mican le 32.10 then micac=1;
else if 32.14 le mican le 64.7 then micac=2;
else if mican ge 64.8 then micac=3; 
run;

*Table 1;

proc freq data=micafinal;
tables diab*case/chisq;
run;

*Table 2;

proc freq data = micafinal;
where case = 1;
tables micac*diab;
run;

proc freq data = micafinal;
where case = 2;
tables micac*diab;
run;

proc freq data = micafinal;
where case = 1 and diab=0;
tables micac*sex;
run;

proc freq data = micafinal;
where case = 2 and diab=0;
tables micac*sex;
run;

*full model with diabetes;
PROC LOGISTIC data = micafinal;
where STAT ne 4;
class SEX stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac DIABETES alco;
RUN;

*Interaction with Diabetes;
PROC LOGISTIC data = micafinal;
where STAT ne 4 and DIABETES ne 8;
class SEX stat (ref='1') micac (ref='1') alco (ref='0') DIABETES (ref='3');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS alco micac DIABETES micac*DIABETES;
oddsratio micac;
RUN;

*Stratified analysis among diabetic participants;

*Diabetics (include long term & recent diabetics);
PROC LOGISTIC data = micafinal;
where STAT ne 4 and DIABETES ne 3;
class SEX stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac alco;
RUN;

*Non Diabetics;
PROC LOGISTIC data = micafinal;
where STAT ne 4 and DIABETES = 3;
class SEX stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac alco;
RUN;

*Final non-diabetic model;
PROC LOGISTIC data = micafinal;
where STAT ne 4 and DIABETES = 3;
class SEX stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac;
RUN;

*Interaction with sex;
PROC LOGISTIC data = micafinal;
where STAT ne 4;
class SEX (ref='M') stat (ref='1') micac (ref='1') alco (ref='0') DIAB (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS alco micac DIAB micac*sex;
oddsratio micac;
RUN;

PROC LOGISTIC data = micafinal;
where STAT ne 4 and SEX = 'M';
class stat (ref='1') micac (ref='1') alco (ref='0') DIAB (ref='0');
MODEL CASE (EVENT='1')= age stat PACK_YRS alco micac DIAB ;
oddsratio micac;
RUN;

PROC LOGISTIC data = micafinal;
where STAT ne 4 and SEX = 'F';
class stat (ref='1') micac (ref='1') alco (ref='0') DIAB (ref='0');
MODEL CASE (EVENT='1')= age stat PACK_YRS alco micac DIAB ;
oddsratio micac;
RUN;

*Stratified Models among non-diabetics;

PROC LOGISTIC data = micafinal;
WHERE SEX='M' and STAT ne 4 and DIAB = 0;
class SEX stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

PROC LOGISTIC data = micafinal;
WHERE SEX='F' and STAT ne 4 and DIAB = 0;
class SEX stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

PROC LOGISTIC data = micafinal;
WHERE STAT ne 4 and DIAB = 0;
class SEX stat (ref='1') micac (ref='1') alco(ref='0');
MODEL CASE (EVENT='1')= age SEX stat micac PACK_YRS alco micac*sex;
RUN;

*average MICA levels;

proc means data = micafinal n mean range;
var mican;

proc freq data = micafinal;
table panc;
run;

proc means mean median min max std data = micafinal;
where diab = 0 and micac ne 0;
class panc;
var mican;
run;

proc means mean median min max std data = micafinal;
where diab = 1 and micac ne 0;
class panc;
var mican;
run;

proc ttest data = micafinal;
where case=1;
class diab;
var mican;
run;

proc ttest data = micafinal;
where case=2;
class diab;
var mican;
run;

/***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************/

*MICA Genetic Analysis Dataset;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets';

data micastrfinal;
set mylib.micastrfinal;
run;

proc contents data = micastrfinal;
run;

proc freq data = micastrfinal;
tables affected;
run;

proc freq data = micastrfinal;
tables additiveA4 additiveA5 additiveA5_1 additiveA6 additiveA9;
run;

proc freq data = micastrfinal;
tables dominantA4 dominantA5 dominantA5_1 dominantA6 dominantA9;
run;

proc freq data = micastrfinal;
tables micac micad;
run;

/*
data micastrfinal;
set micastrfinal;
if affected = . then delete;
if age2 = . then delete;
if Male = . then delete;
if Diabetes2 = . then delete;
if alco = . then delete;
run;

proc freq data = micastrfinal;
tables affected;
run;
 */

*Below I will be comparing the normal link, the poisson link and the normal distribution with log link. The results from the normal distribution with the log link were fairly similar to those of the poisson distribution which we initially reported;

proc freq data = micastrfinal;
tables affected affected*dominantA5_1 affected*additiveA5_1;
run;

PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal link=log;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal link=log;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal link=log;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = poisson;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = poisson;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = poisson;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

data micastrfinal;
set micastrfinal;
micalog = log(mican);
run;

PROC glm data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL micalog = age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

*We will use the normal distribution with the log link to log transform the values without making any assumption about the variance function (technically the data isn't count data so we should not force the poisson variance formula on the data);
*The association became weaker among cases when we switched from the poisson distribution to the normal distribution;

PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal link=log;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal link=log;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal link=log;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = poisson link=log;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal link=log;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal link=log;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal link=log;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = poisson link=log;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

/***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************/

*Table 2:  Association between circulating MICA levels and the MICA A5.1 polymorphism (dominant model); 

*Testing out a method to directly calculate the geometic means for s-MICA using proc ttest and the lognormal option;
*GLM + Genmod: Unweighted Means that only account for X and Y;
*GLM + Genmod: Least Square Means (LS) adjusted for covariates in the models (can be used for both the gaussian and log distribution;
*TTest: Geometric means which adjust for abnormally large value by using a lognormal distribution;

*Table 2 v2;
*We also need to a) use the normal distribution with the log link to account for the non-normal distribution, and b) only model participants who had a s-MICA level greater than 2 pg/ML in the study;
*We will use the normal distribution with the log link to log transform the values without making any assumption about the variance function (technically the data isn't count data so we should not force the poisson variance formula on the data);
*The association became weaker among cases when we switched from the poisson distribution to the normal distribution;

proc freq data = micastrfinal;
tables affected affected*dominantA5_1 affected*additiveA5_1;
run;

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal link=log;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal link=log;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats / dist = normal link=log;
lsmeans dominantA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal link=log;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal link=log;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1 (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal link=log;
lsmeans additiveA5_1 / corr exp diff cl;
RUN;

*Reporting the p-trend by modeling additiveA5.1 as a continuous variable instead of a categorical variable;

PROC genmod data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal link=log;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal link=log;
RUN;

PROC genmod data = micastrfinal;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats / dist = normal link=log;
RUN;

*Checking whether the association of MICA A5.1 and pancreatic cancer is independent of s-MICA levels;

proc freq data = micastrfinal;
tables affected*dominantA5_1;
run;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats mican;
RUN;

PROC LOGISTIC data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats mican;
RUN;

proc freq data = micastrfinal;
tables additiveA5_1*affected;
run;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1(ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1(ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats mican;
RUN;

PROC LOGISTIC data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') additiveA5_1(ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats mican;
RUN;

*P-trends for additive models;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinal;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats mican;
RUN;

PROC LOGISTIC data = micastrfinal;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats mican;
RUN;

***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

*This is the cleaned dataset restricted to the 541 observations with a call for A5.1 from Dr. Pankratz'a analysis;
 
options ps = 3000 ls = 120 nofmterr;
libname mylib 'E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets';

data micastrfinal;
set mylib.micastrfinal;
run;

proc contents data = micastrfinal;
run;

*Descriptive statistics;

proc freq data = micastrfinal;
tables additiveA4 additiveA5 additiveA5_1 additiveA6 additiveA9;
run;

proc freq data = micastrfinal;
tables dominantA4 dominantA5 dominantA5_1 dominantA6 dominantA9;
run;

proc freq data = micastrfinal;
tables micac micad;
run;

proc freq data = micastrfinal;
tables affected*dominantA4 affected*dominantA5 affected*dominantA5_1 affected*dominantA6 affected*dominantA9 / chisq;
run;

proc freq data = micastrfinal;
tables affected*additiveA4 affected*additiveA5 affected*additiveA5_1 affected*additiveA6 affected*additiveA9 / chisq;
run;

proc means data = micastrfinal min mean median max std;
var mican;
run;

proc univariate NORMAL PLOT data=micastrfinal; var mican;
histogram mican/normal;
run;

***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

/* Dataset description:

- Results1 = PANC + MICA plasma
- Results2 = Results1 + Mayo Death Cases
- DNAsurvival = Results1 + Mayo Death Cases + NKG2D DNA
- Micafinal = DNAsurvival
- Micastrfinal = micafinal + MICA STR dataset
- micastrfinal = micastrfinal + Diabetes + Alcohol comparision from MICA manuscript

Outcome
- case (control = 1, case = 2)
- panc (control = 0, case = 1) 
- affected (control = 0, case = 1)
- status2 (death)
- period (survival)

Exposure
- micac (mica re-coded as a categorical variable: if 0 le mican le 2 then micac=0; else if 2 lt mican le 32.10 then micac=1; else if 32.14 le mican le 64.7 then micac=2; else if mican ge 64.8 then micac=3;)
- micad (mica re-coded as a dicothomous variable: if 0 le mican le 2 then micad = 0; else micad = 1;)
- mican (mica re-coded as a numeric variable)

- Allele1 (Allele number 1: can be A4, A5, A5.1, A6, A9)
- Allele2 (Allele number 2: can be A4, A5, A5.1, A6, A9)
- numRepeats (Number of repeats in sequenced gene)
- dominantA9  (A9 allele coded as dominant = 1)
- dominantA5_1 (A5.1 allele coded as dominant = 1)

Covariates
- age2 (Continuous Age)
- agecat (Categorical Age cutoff at 70)
- Male (Indicator variable for gender)
- SEX (Dicothomous variable for gender)
- alco (alcohol variable recoded from sumliq in the initial dataset)
- alcohol (alcohol variable from original manuscript for comparision)
- NeverSmoked (Indicator variable for smoking)
- PastSmoker (Indicator variable for smoking)
- CurrentSmoker (Indicator variable for smoking)
- STAT (Categorical variable for smoking)
- STAT2 (dicothomous variable for smoking)
- EDUC (Categorical variable for education)
- SomeCollege (indicator variable for education)
- Diabetes (Original categorical variable for diabetes)
- Diabetes2 (Dicothomous diabetes variable from Dr. Pankratz)
- Diabetes3 (Dicothomous diabetes variable from original manuscript for comparison)

*/

*Adding the updated genetic SNP dataset from Dr. Nathan Pankratz's Lab for the Reviewer comments analysis;

PROC IMPORT OUT= work.micasnp DATAFILE= "E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets\16_final_dbSnP_update_SAS.xlsx" DBMS=xlsx REPLACE;
  SHEET="7_final_db.GT_RAW_INTEREST"; 
  GETNAMES=YES;
RUN;

proc print data = micasnp (obs=100);
run;

proc contents data = micasnp;
run;

data micasnp;
set micasnp;
ID2 = round(ID);
run;

proc freq data = micasnp;
tables ID ID2;
run;

data micasnp;
set micasnp (drop = ID);
rename ID2 = ID;
run;

proc contents data = micasnp;
run;

*Making a dataset with the MICA SNPs supplemental data for the reviewer comments; 

proc contents data = micastrfinal;
run;

proc sort data = micastrfinal;
by ID;
run;

proc sort data = micasnp;
by ID;
run;

data micastrfinalsnp;
merge micastrfinal micasnp;
by ID;
run;

proc contents data = micastrfinalsnp;
run;

proc freq data = micastrfinalsnp;
tables case dominantA5_1 case*dominantA5_1 affected affected*dominantA5_1 case*affected;
run;

*restricting the dataset to the 541 observation with a call for A5.1 from Dr. Pankratz'a analysis;

data micastrfinalsnp;
set micastrfinalsnp;
if dominantA5_1 = . then delete;
run;

proc freq data = micastrfinalsnp;
tables DIABETES EDUC SomeCollege CurrentSmoker PastSmoker NeverSmoked Diabetes2;
run;

data mylib.micastrfinalsnp;
set micastrfinalsnp;
run;

***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
***************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

*This is the cleaned dataset restricted to the 541 observations with a call for A5.1 from Dr. Pankratz'a analysis;
 
options ps = 3000 ls = 120 nofmterr;
libname mylib 'E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets';

data micastrfinalsnp;
set mylib.micastrfinalsnp;
run;

proc contents data = micastrfinalsnp;
run;

*Here are supplemental SNPs we will working with in our supplemental table
rs1051792_G_A
rs1051794_G_A
rs1051798_C_T
rs1051799_C_G
rs1063635_G_A
rs1131896_G_A
rs1131898_A_G
rs1140700_T_C
rs67841474_TG_T*** Not doing this last one based on Dr. Pankratz's notes, it represents MICA A5.1 and we used an alternate method for analyzing it;

*Supplemental Table:  Association between circulating MICA levels and other MICA SNPs (additive); 

proc freq data = micastrfinalsnp;
tables affected;
run;

proc freq data = micastrfinalsnp;
tables 
affected*rs1051792_G_A 
affected*rs1051794_G_A 
affected*rs1051798_C_T 
affected*rs1051799_C_G 
affected*rs1063635_G_A 
affected*rs1131896_G_A 
affected*rs1131898_A_G 
affected*rs1140700_T_C / chisq;
run;

*Testing out a method to directly calculate the geometic means for s-MICA using proc ttest and the lognormal option;
*GLM + Genmod: Unweighted Means that only account for X and Y;
*GLM + Genmod: Least Square Means (LS) adjusted for covariates in the models (can be used for both the gaussian and log distribution;
*TTest: Geometric means which adjust for abnormally large value by using a lognormal distribution;

*We also need to a) use the normal distribution with the log link to account for the non-normal distribution, and b) only model participants who had a s-MICA level greater than 2 pg/ML in the study;
*We will use the normal distribution with the log link to log transform the values without making any assumption about the variance function (technically the data isn't count data so we should not force the poisson variance formula on the data);
*The association became weaker among cases when we switched from the poisson distribution to the normal distribution;

proc freq data = micastrfinalsnp;
tables affected*rs1051792_G_A;
run;

PROC genmod data = micastrfinalsnp;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051792_G_A (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051792_G_A numRepeats / dist = normal link=log;
lsmeans rs1051792_G_A / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051792_G_A (ref='2');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051792_G_A numRepeats / dist = normal link=log;
lsmeans rs1051792_G_A / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051792_G_A (ref='2');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051792_G_A numRepeats / dist = normal link=log;
lsmeans rs1051792_G_A / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0 and affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051792_G_A (ref='1');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051792_G_A numRepeats / dist = normal link=log;
lsmeans rs1051792_G_A / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051794_G_A (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051794_G_A numRepeats / dist = normal link=log;
lsmeans rs1051794_G_A / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051798_C_T (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051798_C_T numRepeats / dist = normal link=log;
lsmeans rs1051798_C_T / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051799_C_G  (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051799_C_G  numRepeats / dist = normal link=log;
lsmeans rs1051799_C_G / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1063635_G_A (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1063635_G_A numRepeats / dist = normal link=log;
lsmeans rs1063635_G_A / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1131896_G_A (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1131896_G_A numRepeats / dist = normal link=log;
lsmeans rs1131896_G_A / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1131898_A_G (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1131898_A_G numRepeats / dist = normal link=log;
lsmeans rs1131898_A_G / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1140700_T_C (ref='0');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1140700_T_C numRepeats / dist = normal link=log;
lsmeans rs1140700_T_C / corr exp diff cl;
RUN;

*Supplemental Table:  Association between circulating pancreatic cancer risk and other MICA SNPs (additive); 

proc freq data = micastrfinalsnp;
tables affected;
run;

proc freq data = micastrfinalsnp;
tables 
affected*rs1051792_G_A 
affected*rs1051794_G_A 
affected*rs1051798_C_T 
affected*rs1051799_C_G 
affected*rs1063635_G_A 
affected*rs1131896_G_A 
affected*rs1131898_A_G 
affected*rs1140700_T_C / chisq;
run;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051792_G_A (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051792_G_A  numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051794_G_A (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051794_G_A numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051798_C_T (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051798_C_T numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051799_C_G (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051799_C_G numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1063635_G_A (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1063635_G_A numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1131896_G_A (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1131896_G_A numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1131898_A_G (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1131898_A_G numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1140700_T_C (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1140700_T_C numRepeats;
RUN;

*Reversing the reference levels for the updated supplemental tables;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051792_G_A (ref='2');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051792_G_A  numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051794_G_A (ref='2');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051794_G_A numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051798_C_T (ref='2');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051798_C_T numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051799_C_G (ref='2');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051799_C_G numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1063635_G_A (ref='2');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1063635_G_A numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1131896_G_A (ref='2');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1131896_G_A numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1131898_A_G (ref='2');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1131898_A_G numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1140700_T_C (ref='2');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1140700_T_C numRepeats;
RUN;

**Creating an excel spreadsheet to calculate LDs;

*This is the cleaned dataset restricted to the 541 observations with a call for A5.1 from Dr. Pankratz'a analysis;
 
options ps = 3000 ls = 120 nofmterr;
libname mylib 'E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets';

data micastrfinalsnp;
set mylib.micastrfinalsnp;
run;

proc contents data = micastrfinalsnp;
run;

proc means data = micastrfinalsnp n min max mean std;
where micac ne 0 and Affected = 0;
var mican;
run;

proc univariate NORMAL PLOT data=micastrfinalsnp; var mican;
where micac ne 0 and Affected = 0;
histogram  mican/normal;
run;

data micastrfinalsnp;
set micastrfinalsnp;
micaln = log(mican);
run;

proc means data = micastrfinalsnp n min max mean std;
where micac ne 0 and Affected = 0;
var micaln;
run;

proc univariate NORMAL PLOT data=micastrfinalsnp; var micaln;
where micac ne 0 and Affected = 0;
histogram  micaln/normal;
run;

PROC LOGISTIC data = micastrfinalsnp;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco additiveA5_1 numRepeats;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') micac (ref='1');
MODEL Affected (EVENT='1')= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco micac;
RUN;

data micastrfinalsnp;
set micastrfinalsnp;
if additiveA5_1 = 0 then A51geno = 'XX';
else if additiveA5_1 = 1 then A51geno = 'XA';
else if additiveA5_1 = 2 then A51geno = 'AA';
else A51geno = ' ';
if rs1051792_G_A = 0 then rs1051792geno = 'GG';
else if rs1051792_G_A = 1 then rs1051792geno = 'GA';
else if rs1051792_G_A = 2 then rs1051792geno = 'AA';
else rs1051792geno = ' ';
if rs1051794_G_A = 0 then rs1051794geno = 'GG';
else if rs1051794_G_A = 1 then rs1051794geno = 'GA';
else if rs1051794_G_A = 2 then rs1051794geno = 'AA';
else rs1051794geno = ' ';
if rs1131896_G_A = 0 then rs1131896geno = 'GG';
else if rs1131896_G_A = 1 then rs1131896geno = 'GA';
else if rs1131896_G_A = 2 then rs1131896geno = 'AA';
else rs1131896geno = ' ';
if rs1131898_A_G = 0 then rs1131898geno = 'AA';
else if rs1131898_A_G = 1 then rs1131898geno = 'AG';
else if rs1131898_A_G = 2 then rs1131898geno = 'GG';
else rs1131898geno = ' ';
if rs1051798_C_T = 0 then rs1051798geno = 'CC';
else if rs1051798_C_T = 1 then rs1051798geno = 'CT';
else if rs1051798_C_T = 2 then rs1051798geno = 'TT';
else rs1051798geno = ' ';
if rs1140700_T_C = 0 then rs1140700geno = 'TT';
else if rs1140700_T_C = 1 then rs1140700geno = 'TC';
else if rs1140700_T_C = 2 then rs1140700geno = 'CC';
else rs1140700geno = ' ';
if rs1051799_C_G = 0 then rs1051799geno = 'CC';
else if rs1051799_C_G = 1 then rs1051799geno = 'CG';
else if rs1051799_C_G = 2 then rs1051799geno = 'GG';
else rs1051799geno = ' ';
if rs1063635_G_A = 0 then rs1063635geno = 'GG';
else if rs1063635_G_A = 1 then rs1063635geno = 'GA';
else if rs1063635_G_A = 2 then rs1063635geno = 'AA';
else rs1063635geno = ' ';
run;

proc export data= micastrfinalsnp
   outfile= "C:\Users\onyea005\Desktop\MICA_SNP_LD"
   dbms=XLSX
   replace;
run;

data micastrfinalsnp;
set micastrfinalsnp;
rs1051792_G_A_2 = round(rs1051792_G_A); 
rs1051794_G_A_2 = round(rs1051794_G_A); 
rs1051798_C_T_2 = round(rs1051798_C_T); 
rs1051799_C_G_2 = round(rs1051799_C_G); 
rs1063635_G_A_2 = round(rs1063635_G_A); 
rs1131896_G_A_2 = round(rs1131896_G_A); 
rs1131898_A_G_2 = round(rs1131898_A_G); 
rs1140700_T_C_2 = round(rs1140700_T_C);
run;

proc corr data = micastrfinalsnp;
var rs1051792_G_A_2 rs1051794_G_A_2 rs1051798_C_T_2 rs1051799_C_G_2 rs1063635_G_A_2 rs1131896_G_A_2 rs1131898_A_G_2 rs1140700_T_C_2 additiveA5_1;
run;

*Making a supplemental table for RS1051792 and s-MICA since it also modulates s-MICA release.

*Testing out a method to directly calculate the geometic means for s-MICA using proc ttest and the lognormal option;
*GLM + Genmod: Unweighted Means that only account for X and Y;
*GLM + Genmod: Least Square Means (LS) adjusted for covariates in the models (can be used for both the gaussian and log distribution;
*TTest: Geometric means which adjust for abnormally large value by using a lognormal distribution;

*We also need to a) use the normal distribution with the log link to account for the non-normal distribution, and b) only model participants who had a s-MICA level greater than 2 pg/ML in the study;
*We will use the normal distribution with the log link to log transform the values without making any assumption about the variance function (technically the data isn't count data so we should not force the poisson variance formula on the data);
*The association became weaker among cases when we switched from the poisson distribution to the normal distribution;

PROC genmod data = micastrfinalsnp;
where micac ne 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051792_G_A (ref='2');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051792_G_A numRepeats / dist = normal link=log;
lsmeans rs1051792_G_A / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0 and affected = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051792_G_A (ref='1');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051792_G_A numRepeats / dist = normal link=log;
lsmeans rs1051792_G_A / corr exp diff cl;
RUN;

PROC genmod data = micastrfinalsnp;
where micac ne 0 and affected = 0;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') rs1051792_G_A (ref='2');
MODEL mican= age2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco rs1051792_G_A numRepeats / dist = normal link=log;
lsmeans rs1051792_G_A / corr exp diff cl;
RUN;

*** Rerunning the analysis for age by median age rather than the cutpoint at 70;

/*Interaction with Age*/

proc freq data = micastrfinalsnp;
where age lt 70;
tables Case*micac;
run;

proc freq data = micastrfinalsnp;
where age ge 70;
tables Case*micac;
run;

proc means data = micastrfinalsnp min mean median max;
var age;
run;

proc sort data = micastrfinalsnp;
by case;
run;

proc means data = micastrfinalsnp min mean median max;
by case;
var age;
run;

proc freq data = micastrfinalsnp;
tables Case*agecat;
run;

proc freq data = micastrfinalsnp;
tables MICAC*agecat;
run;

data micastrfinalsnp;
set micastrfinalsnp;
if age le 68 then agecat2 = 1;
else if age gt 68 then agecat2 = 2;
run;

proc freq data = micastrfinalsnp;
tables agecat2 agecat2*affected / chisq;
run;

PROC LOGISTIC data = micastrfinalsnp;
where agecat2 = 1;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats dominantA5_1;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
where agecat2 = 2;
class Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats dominantA5_1;
RUN;

PROC LOGISTIC data = micastrfinalsnp;
class agecat2 Male(ref="0") SomeCollege (ref='0') NeverSmoked (ref='0') PastSmoker (ref='0') Diabetes2 (ref='0') alco (ref='0') dominantA5_1 (ref='0');
MODEL Affected (EVENT='1')= agecat2 Male SomeCollege NeverSmoked PastSmoker Diabetes2 alco dominantA5_1 numRepeats dominantA5_1*agecat2;
RUN;

proc contents data = micastrfinalsnp;
run;

data mylib.micastrfinaldoi;
set micastrfinalsnp (KEEP = AGE
Affected
CALOR
CARBO
CASE
CurrentSmoker
DIABETES
DTFIB
Diabetes2
EDUC
MICA
Male
NeverSmoked
PACK_YRS
PROT
PastSmoker
SAMPLE
SEX
STAT
SUMLIQ
SomeCollege
additiveA4
additiveA5
additiveA6
additiveA9
additiveA5_1
age2
dominantA4
dominantA5
dominantA6
dominantA9
dominantA5_1
micac
micad
mican
rs1049174
rs2255336
rs1051792_G_A
rs1051794_G_A
rs1051798_C_T
rs1051799_C_G
rs1063635_G_A
rs1131896_G_A
rs1131898_A_G
rs1140700_T_C
rs67841474_TG_T
stat2);
Run;

proc export data= mylib.micastrfinaldoi
   outfile= "E:\Professional Folder\WBOB\MICA Genetic\Analysis\SAS Datasets\micastrfinaldoi"
   dbms=XLSX
   replace;
run;
