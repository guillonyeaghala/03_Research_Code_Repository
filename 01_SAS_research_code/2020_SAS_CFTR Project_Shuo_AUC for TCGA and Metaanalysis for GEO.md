**************************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************;

*Summer 2020 CFTR analysis;

*The updated guidelines for the CFTR expression analysis are listed in the following document:
(H:\Professional Folder\WBOB\CFTR Project\Documents\8_March 2020 CFTR updates/22_CFTR Updated analysis plan_03182020)

*GSE dataset analysis
*We reproduced the R2 Tables for "GSE14333(Sieber, 290 cases)", "GSE17538(Smith, 232 cases)", "GSE39582(Marissa, 566 cases)",  "GSE33113 (Than 90 cases, originally Medema 90 cases)"
and the DFS and OS on the TCGA data in "11_CFTR Grant proposal FEB2020"
*2-Clean the data for the analyses (import the data, merge the R2 copied values for GSE14333, GSE17538, GSE39582 and GSE33113;
*3-Make a descriptive table for TCGA and the datasets with DFS + TCGA_DFS and the datasets with OS and TCGA_OS;
*4-Low cut point analysis (5, 10, 15, 20 and 25%) for the DFS, OS, TCGA DFS and TCGA OS;
*5-ROC cutpoint analysis (using Youdent statistic) for the DFS, OS, TCGA DFS and TCGA OS;
*6-Sensitivity analysis using the second most abundant CFTR marker;
 
*I am reorganizing elements from the CFTR analysis so that we have the TCGA elements first, then we can tackle the GSE replication elements;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Using the latest correct file TCGA from Dr. Scott "20200104_for ap and go" to make the corresponding tables for the February 2020 CFTR grant proposal;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

PROC IMPORT OUT= work.TCGATEST2 DATAFILE= "H:\Professional Folder\WBOB\CFTR Project\Datasets\20200104_for ap and go_SAS.xlsx" DBMS=xlsx REPLACE;
     SHEET="4_OS file"; 
     GETNAMES=YES;
RUN;

proc contents data = TCGATEST2;
run;

proc print data = TCGATEST2;
run;

proc univariate data = TCGATEST2;
var CFTR;
histogram CFTR/ normal;
output out=UniWidePctls pctlpre=CFTRP_ pctlpts=2.5,5,10,15,20,25;
run; 

proc print data=UniWidePctls noobs; run;

data TCGATEST2;
set TCGATEST2;
if CFTR = . then delete;
if CFTR le 5.836 then CFTR025 = 0; else CFTR025 = 1;
if CFTR le 7.967 then CFTR050 = 0; else CFTR050 = 1;
if CFTR le 9.663 then CFTR100 = 0; else CFTR100 = 1;
if CFTR le 10.39 then CFTR150 = 0; else CFTR150 = 1;
if CFTR le 10.89 then CFTR200 = 0; else CFTR200 = 1;
if CFTR le 11.19 then CFTR250 = 0; else CFTR250 = 1;
run;

data TCGATEST2;
set TCGATEST2;
OS_time_month = OS_time / 30.5;
run;

proc means data = TCGATEST2 min mean median max;
var OS_time_month;
run;

*There were 39 observations with follow up time greater than 5 years (or 60 months);
proc print data = TCGATEST2;
where OS_time_month gt 60;
run;

*There were 10 deaths in the 39 observations with follow up time greater than 5 years (or 60 months);
proc freq data = TCGATEST2;
table OS;
where OS_time_month gt 60;
run;

*There were 247 observations with follow up time less than 5 years (or 60 months);
proc print data = TCGATEST2;
where OS_time_month le 60;
run;

*There were 62 deaths in the 247 observations with follow up time less than 5 years (or 60 months);
proc freq data = TCGATEST2;
table OS;
where OS_time_month le 60;
run;

*Updating the outcome variable to be dicothomous based on 5 year survival;
data TCGATEST2;
set TCGATEST2;
if OS_time_month gt 60 and OS = 1 then OS_5 = 0;
else OS_5 = OS;
run;

proc freq data = TCGATEST2;
table OS*OS_5;
run;

proc means data = TCGATEST2 min mean median std max;
var OS_time_month CFTR;
run;

proc freq data = TCGATEST2;
tables CFTR025 CFTR050 CFTR100 CFTR150 CFTR200 CFTR250;
run;

*I AM THE BIGGEST FOOL EVER!!! The number in parenthesis is the value of ëno-eventí, and I used 0, which in this case was the wrong valueeeeee!!!!!!;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 10 by 1);	
time OS_time_month*OS(0);
strata CFTR250;
run;

*I can see two ways to limit survival at 5 year. option 1 would be to look at a max of 5 years for follow up, 
and option 2 would be to recode those who had an event after 5 years as no event;

*Option 1;
data TCGATEST3;
set TCGATEST2;
if OS_time_month gt 60 then delete;
run;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 10 by 1);	
time OS_time_month*OS(0);
strata CFTR250;
run;

*option 2;
data TCGATEST2;
set TCGATEST2;
run;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 10 by 1);	
time OS_time_month*OS_5(0);
strata CFTR250;
run;

*Z-standardization of the CFTR variable;
proc means data = TCGATEST2 mean std;
var CFTR; 
run;

data TCGATEST2;
set TCGATEST2;
ZCFTR = (CFTR - 11.4984189)/ 1.7884178;
run;

data mylib.TCGATEST2;
set TCGATEST2;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Running the analysis for the 5 year survival;

data TCGATEST2;
set mylib.TCGATEST2;
run;

proc contents data = TCGATEST2;
run;

*Youden's Statistic using the CTABLE option;

*Going to use the full dataset (not restrict to cancerstage = 1) since ZCFTR is associated with DFS in that model;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

proc logistic data=TCGATEST2;
model OS(event="1") = CFTR/ CTABLE outroc=troc; roc; roccontrast;
run;

proc logistic data=TCGATEST2;
model OS(event="1") = ZCFTR/ CTABLE outroc=troc; roc; roccontrast;
run;

*I can see two ways to limit survival at 5 year. option 1 would be to look at a max of 5 years for follow up, 
and option 2 would be to recode those who had an event after 5 years as no event;

*Option 1;
proc logistic data=TCGATEST2;
where OS_time_month le 60;
model OS(event="1") = CFTR/ CTABLE outroc=troc; roc; roccontrast;
run;

proc logistic data=TCGATEST2;
where OS_time_month le 60;
model OS(event="1") = ZCFTR/ CTABLE outroc=troc; roc; roccontrast;
run;

*Option 2;
proc logistic data=TCGATEST2;
model OS_5(event="1") = CFTR/ CTABLE outroc=troc; roc; roccontrast;
run;

proc logistic data=TCGATEST2;
model OS_5(event="1") = ZCFTR/ CTABLE outroc=troc; roc; roccontrast;
run;

*I exported te table to excel to calculate youden's statistic;
*The spreadsheet is located at H:\Professional Folder\WBOB\CFTR Project\Documents\9_Youden Statistic Test

*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;
*Find Y value mathcing the largest youden value;

Logit(0.20) = -1.2996 - -0.2405 *(X)
X = [Logit(0.20) - (-1.2996)]/(-0.2405);

*use logit calculator (https://www.medcalc.org/manual/logit_function.php) and apply the formula to calculate X in Excel;

X = 0.360475514;

data TCGATEST2;
set TCGATEST2;
if ZCFTR LT 0.360475514 then OPTIMAL = 0;
else if ZCFTR GE 0.360475514 then OPTIMAL = 1;
run;

proc logistic data=TCGATEST2;
class OPTIMAL(ref="1");
model OS_5(event="1") = OPTIMAL;
run;

proc phreg data=TCGATEST2;
class OPTIMAL(ref="1");
model OS_time_month*OS_5(0) = OPTIMAL/rl;
run;

proc univariate data = TCGATEST2;
var ZCFTR;
histogram;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OS_time_month*OS_5(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = TCGATEST2;
tables OS_5*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data TCGATEST2;
set TCGATEST2;
if pathologic_stage = 'Stage IA' then stage = 1;
if pathologic_stage = 'Stage II' then stage = 2;
if pathologic_stage = 'Stage IIA' then stage = 2;
if pathologic_stage = 'Stage IIB' then stage = 2;
if pathologic_stage = 'Stage IIC' then stage = 2;
if pathologic_stage = 'Stage III' then stage = 3;
if pathologic_stage = 'Stage IIIA' then stage = 3;
if pathologic_stage = 'Stage IIIB' then stage = 3;
if pathologic_stage = 'Stage IIIC' then stage = 3;
if pathologic_stage = 'Stage IV' then stage = 4;
if pathologic_stage = 'Stage IVA' then stage = 4;
if pathologic_stage = 'Stage IVB' then stage = 4;
run;

data TCGATEST2;
set TCGATEST2;
if stage = 1 then cancer_stage = 0;
if stage = 2 then cancer_stage = 1;
if stage = 3 then cancer_stage = 1;
if stage = 4 then cancer_stage = 1;
run;

data TCGATEST2;
set TCGATEST2;
if ZCFTR LT 0.360475514 then OPTIMAL = 0;
else if ZCFTR GE 0.360475514 then OPTIMAL = 1;
run;

proc logistic data=TCGATEST2;
where cancer_stage = 1;
class stage OPTIMAL(ref="1");
model OS_5(event="1") = stage OPTIMAL;
run;

proc phreg data=TCGATEST2;
where cancer_stage = 1;
class stage OPTIMAL(ref="1");
model OS_time_month*OS_5(0) = stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OS_time_month*OS_5(0);
strata OPTIMAL;
run;
ODS Graphics off;

***************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Running the analysis for the full TCGA analysis follow up period, based on the Zscore value for CFTR;

data TCGATEST2;
set mylib.TCGATEST2;
run;

proc contents data = TCGATEST2;
run;

*Youden's Statistic using the CTABLE option;

*Going to use the full dataset (not restrict to cancerstage = 1) since ZCFTR is associated with DFS in that model;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

proc logistic data=TCGATEST2;
model OS(event="1") = ZCFTR/ CTABLE outroc=troc; roc; roccontrast;
run;

*I exported te table to excel to calculate youden's statistic;
*The spreadsheet is located at H:\Professional Folder\WBOB\CFTR Project\Documents\9_Youden Statistic Test

*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;
*Find Y value mathcing the largest youden value;

Logit(0.26) = -1.0994 - -0.2151 *(X)
X = [Logit(0.26) - (-1.0994)]/(-0.2151);

*use logit calculator (https://www.medcalc.org/manual/logit_function.php) and apply the formula to calculate X in Excel;

X = -0.248402812;

data TCGATEST2;
set TCGATEST2;
if ZCFTR LT -0.248402812 then OPTIMAL_Full = 0;
else if ZCFTR GE -0.248402812 then OPTIMAL_Full = 1;
run;

proc freq data = TCGATEST2;
table OPTIMAL_Full;
run;

proc logistic data=TCGATEST2;
class OPTIMAL_Full(ref="1");
model OS(event="1") = OPTIMAL_Full;
run;

proc phreg data=TCGATEST2;
class OPTIMAL_Full(ref="1");
model OS_time_month*OS(0) = OPTIMAL_Full/rl;
run;

proc univariate data = TCGATEST2;
var ZCFTR;
histogram;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OS_time_month*OS(0);
strata OPTIMAL_Full;
run;
ODS Graphics off;

proc freq data = TCGATEST2;
tables OS*OPTIMAL_Full;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data TCGATEST2;
set TCGATEST2;
if pathologic_stage = 'Stage IA' then stage = 1;
if pathologic_stage = 'Stage II' then stage = 2;
if pathologic_stage = 'Stage IIA' then stage = 2;
if pathologic_stage = 'Stage IIB' then stage = 2;
if pathologic_stage = 'Stage IIC' then stage = 2;
if pathologic_stage = 'Stage III' then stage = 3;
if pathologic_stage = 'Stage IIIA' then stage = 3;
if pathologic_stage = 'Stage IIIB' then stage = 3;
if pathologic_stage = 'Stage IIIC' then stage = 3;
if pathologic_stage = 'Stage IV' then stage = 4;
if pathologic_stage = 'Stage IVA' then stage = 4;
if pathologic_stage = 'Stage IVB' then stage = 4;
run;

data TCGATEST2;
set TCGATEST2;
if stage = 1 then cancer_stage = 0;
if stage = 2 then cancer_stage = 1;
if stage = 3 then cancer_stage = 1;
if stage = 4 then cancer_stage = 1;
run;

data TCGATEST2;
set TCGATEST2;
if ZCFTR LT -0.248402812 then OPTIMAL_Full = 0;
else if ZCFTR GE -0.248402812 then OPTIMAL_Full = 1;
run;

proc logistic data=TCGATEST2;
where cancer_stage = 1;
class stage OPTIMAL_Full(ref="1");
model OS(event="1") = stage OPTIMAL_Full;
run;

proc phreg data=TCGATEST2;
where cancer_stage = 1;
class stage OPTIMAL_Full(ref="1");
model OS_time_month*OS(0) = stage OPTIMAL_Full/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OS_time_month*OS(0);
strata OPTIMAL_Full;
run;
ODS Graphics off;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************;

/*
options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

/*Importing the excel spreadsheet*/

PROC IMPORT OUT= work.GSE14333T DATAFILE= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE14333T.xlsx" DBMS=xlsx REPLACE;
     SHEET="Sheet3"; 
     GETNAMES=YES;
RUN;

proc contents data = GSE14333T;
run;

*My first goal is to reproduce the survival charts from the R2 kaplan meier genomic software (https://hgserver1.amc.nl/cgi-bin/r2/main.cgi);
*However, the expression values in the datasets are much higher than those in the spreadsheet, which leads me to think that the later were already normalized;
*I will be using the distribution of the data to find a separation point similar to the grapth high = 200, low = 20;

proc univariate data = GSE14333T;
var _205043_at;
histogram;
run;

data GSE14333T2;
set GSE14333T;
if _205043_at = 0 then delete;
if _DFS_Cens = . then delete;
if _DFS_Time = . then delete;
run;

proc univariate data = GSE14333T2;
var _205043_at;
histogram;
run;

*Survival Analysis;

/* ods graphics on;
proc lifetest data=Exposed plots=(survival(atrisk) logsurv);
   time Days*Status(0);
   strata Treatment;
run;
ods graphics off;
In the TIME statement, the survival time variable, Days, is crossed with the censoring variable, Status, with the value 0 indicating censoring. That is, the values of Days are considered censored if the corresponding values of Status are 0; otherwise, they are considered as event times. In the STRATA statement, the variable Treatment is specified, which indicates that the data are to be divided into strata based on the values of Treatment. ODS Graphics must be enabled before producing graphs. Two plots are requested through the PLOTS= optionóa plot of the survival curves with at risk numbers and a plot of the negative log of the survival curves.

*/

data GSE14333T2;
set GSE14333T2;
if _205043_at lt 7.5 then CFTR205043 = "low";
else CFTR205043 = "high";
run;

ODS Graphics on;
proc lifetest  data=GSE14333T2 method=life plots=(s) intervals = 0 to 180 by 20;
time _DFS_Time*_DFS_Cens(0);
strata CFTR205043;
run;
ODS Graphics off;

ODS Graphics on;
proc lifetest  data=GSE14333T2 method=life plots=(s) intervals = 0 to 180 by 20;
time _DFS_Time*_DFS_Cens(0);
strata CFTR205043;
run;
ODS Graphics off;

**I've found that you can copy the un-normalized expression data from R2 going to view in 2geneview > datatable since the spreadsheets only have the normalized data;
*Merging those digits to my main dataset;

PROC IMPORT OUT= work.GSE14333TRAW DATAFILE= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE14333TRAW.xlsx" DBMS=xlsx REPLACE;
     SHEET="Sheet1"; 
     GETNAMES=YES;
RUN;

proc contents data = GSE14333TRAW;
run;

proc sort data = GSE14333T2; by ID_REF; run;
proc sort data = GSE14333TRAW; by ID_REF; run;

data GSE14333T3;
merge GSE14333T2 GSE14333TRAW;
by ID_REF;
run;

data GSE14333T3;
set GSE14333T3;
if CFTR_205043_at__ le 98.7 then CFTRCAT = "L";
else CFTRCAT = "H";
run;

ODS Graphics on;
proc lifetest  data=GSE14333T3 method=life plots=(s) intervals = 0 to 180 by 20;
time _DFS_Time*_DFS_Cens(0);
strata CFTRCAT;
run;
ODS Graphics off;

ODS Graphics on;
proc lifetest  data=GSE14333T3 method=life plots=(survival(atrisk) logsurv);
time _DFS_Time*_DFS_Cens(0);
strata CFTRCAT;
run;
ODS Graphics off;

ODS Graphics on;
proc lifetest  data=GSE14333T3 method=life plots=(s) intervals = 0 to 120;
time _DFS_Time*_DFS_Cens(0);
strata CFTRCAT;
run;
ODS Graphics off;

*Running only the log rank test as R2 indicated that is the feature used by its scan algorythm;
proc lifetest  data=GSE14333T3;
time _DFS_Time*_DFS_Cens(0);
STRATA CFTRCAT / test =(logrank);
run;

Proc lifetest  data=GSE14333T3 method = km;
time _DFS_Time*_DFS_Cens(0);
STRATA CFTRCAT;
run;

*Running a chi-square test since the results seemed to be derived from one;
proc freq data = GSE14333T3;
tables CFTRCAT*_DFS_Cens / chisq;
run;

**Looks like the statistical tests were not from the survival analysis but from a chisquare test;
*trying to see if we get more interesting results for mortality rather than survival;

proc phreg data=GSE14333T3;
class CFTRCAT;
model _DFS_Time*_DFS_Cens(0) = CFTRCAT/rl;
run;

proc phreg data=GSE14333T3;
model _DFS_Time*_DFS_Cens(0) = CFTR_205043_at__/rl;
run;

proc phreg data=GSE14333T3;
model _DFS_Time*_DFS_Cens(0) = _205043_at _Age_Diag _Gender/rl;
run;

*generating a file to import into R2: Did not find similar results when importing the dataset back;

proc export data= GSE14333T3
   outfile= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE14333TKM"
   dbms=XLSX
   replace;
run;

*On the bright side, one thing I've found is that the transformation used was the 2log transformation!!;

*Running the exact same code used in epi3 since on the framingham training dataset the survival curves were non-homogeneous;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time _DFS_Time*_DFS_Cens(0);
strata CFTRCAT;
run;

data GSE14333T3;
set GSE14333T3;
DFSdays = _DFS_Time*30.58;
DFSyears = _DFS_Time/12;
run;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 4000 by 500);	
time DFSdays*_DFS_Cens(0);
strata CFTRCAT;
run;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 10 by 1);	
time DFSyears*_DFS_Cens(0);
strata CFTRCAT;
run;

**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************;

*I AM THE BIGGEST FOOL EVER!!! The number in parenthesis is the value of ëno-eventí, and I used 0, which in this case was the wrong valueeeeee!!!!!!;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 10 by 1);	
time DFSyears*_DFS_Cens(1);
strata CFTRCAT;
run;

**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************;

*Restarting the code for the data report;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

**********************************************************************************************************************************************************************************;

*GSE14333T;

/*Importing the excel spreadsheet*/

PROC IMPORT OUT= work.GSE14333T DATAFILE= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE14333T.xlsx" DBMS=xlsx REPLACE;
     SHEET="Sheet3"; 
     GETNAMES=YES;
RUN;

proc contents data = GSE14333T;
run;

*My first goal is to reproduce the survival charts from the R2 kaplan meier genomic software (https://hgserver1.amc.nl/cgi-bin/r2/main.cgi);
*However, the expression values in the datasets are much higher than those in the spreadsheet, which leads me to think that the later were already normalized;

proc univariate data = GSE14333T;
var _205043_at;
histogram;
run;

data GSE14333T2;
set GSE14333T;
if _205043_at = 0 then delete;
if _DFS_Cens = . then delete;
if _DFS_Time = . then delete;
run;

proc univariate data = GSE14333T2;
var _205043_at;
histogram;
run;

**I've found that you can copy the un-normalized expression data from R2 going to view in 2geneview > datatable since the spreadsheets only have the normalized data;
*Merging those digits to my main dataset;

PROC IMPORT OUT= work.GSE14333TRAW DATAFILE= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE14333TRAW.xlsx" DBMS=xlsx REPLACE;
     SHEET="Sheet1"; 
     GETNAMES=YES;
RUN;

proc contents data = GSE14333TRAW;
run;

proc sort data = GSE14333T2; by ID_REF; run;
proc sort data = GSE14333TRAW; by ID_REF; run;

data GSE14333T3;
merge GSE14333T2 GSE14333TRAW;
by ID_REF;
run;

data GSE14333T3;
set GSE14333T3;
if CFTR_205043_at__ le 98.7 then CFTRCAT = "L";
else CFTRCAT = "H";
run;

*On the bright side, one thing I've found is that the transformation used was the 2log transformation!!;

/* ods graphics on;
proc lifetest data=Exposed plots=(survival(atrisk) logsurv);
   time Days*Status(0);
   strata Treatment;
run;
ods graphics off;
In the TIME statement, the survival time variable, Days, is crossed with the censoring variable, Status, with the value 0 indicating censoring. That is, the values of Days are considered censored if the corresponding values of Status are 0; otherwise, they are considered as event times. In the STRATA statement, the variable Treatment is specified, which indicates that the data are to be divided into strata based on the values of Treatment. ODS Graphics must be enabled before producing graphs. Two plots are requested through the PLOTS= optionóa plot of the survival curves with at risk numbers and a plot of the negative log of the survival curves.

*/

*Running the exact same code used in epi3 since on the framingham training dataset the survival curves were non-homogeneous;

*Survival Analysis;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time _DFS_Time*_DFS_Cens(0);
strata CFTRCAT;
run;

*I AM THE BIGGEST FOOL EVER!!! The number in parenthesis is the value of ëno-eventí, and I used 0, which in this case was the wrong valueeeeee!!!!!!;

PROC LIFETEST data = GSE14333T3 GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 10 by 1);	
time _DFS_Time*_DFS_Cens(1);
strata CFTRCAT;
run;

*CFTR categories;

proc rank data=GSE14333T3 groups=3 out=GSE14333T3;
var CFTR_205043_at__; 
ranks CFTR_tert;
run;

proc rank data=GSE14333T3 groups=4 out=GSE14333T3;
var CFTR_205043_at__; 
ranks CFTR_quart;
run;

proc rank data=GSE14333T3 groups=5 out=GSE14333T3;
var CFTR_205043_at__; 
ranks CFTR_quint;
run;

*CFTR per SD;

proc means data = GSE14333T3 min mean median max std;
var CFTR_205043_at__;
run;

data GSE14333T3;
set GSE14333T3;
CFTR_std = CFTR_205043_at__/467.5589204;
run;

*generating a file to import into R2: Did not find similar results when importing the dataset back;

proc export data= GSE14333T3
   outfile= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE14333TKM"
   dbms=XLSX
   replace;
run;

*Saving the dataset;

data mylib.GSE14333T3;
set GSE14333T3;
run;

**********************************************************************************************************************************************************************************;

*GSE17538T;

/*Importing the excel spreadsheet*/

PROC IMPORT OUT= work.GSE17538T DATAFILE= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE17538T.xlsx" DBMS=xlsx REPLACE;
     SHEET="Sheet3"; 
     GETNAMES=YES;
RUN;

proc contents data = GSE17538T;
run;

*My first goal is to reproduce the survival charts from the R2 kaplan meier genomic software (https://hgserver1.amc.nl/cgi-bin/r2/main.cgi);
*However, the expression values in the datasets are much higher than those in the spreadsheet, which leads me to think that the later were already normalized;
*for this particular dataset, the missing observation caused the data to shift prior to the text to column conversion in excel, so I had to manually correct that in sheet 4 and copy it back to sheet 3;

proc univariate data = GSE17538T;
var _205043_at;
histogram;
run;

data GSE17538T2;
set GSE17538T;
if dfs_time = . then delete;
if dfs_time = 0 then delete;
run;

proc univariate data = GSE17538T2;
var _205043_at;
histogram;
run;

**I've found that you can copy the un-normalized expression data from R2 going to view in 2geneview > datatable since the spreadsheets only have the normalized data;
*Merging those digits to my main dataset;

PROC IMPORT OUT= work.GSE17538TRAW DATAFILE= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE17538-GPL570TRAW.xlsx" DBMS=xlsx REPLACE;
     SHEET="Sheet1"; 
     GETNAMES=YES;
RUN;

proc contents data = GSE17538TRAW;
run;

proc sort data = GSE17538T2; by ID_REF; run;
proc sort data = GSE17538TRAW; by ID_REF; run;

data GSE17538T3;
merge GSE17538T2 GSE17538TRAW;
by ID_REF;
run;

data GSE17538T3;
set GSE17538T3;
if CFTR_205043_at__ = . then delete;
if CFTR_205043_at__ le 413.6 then CFTRCAT = "L";
else CFTRCAT = "H";
if dfs_event__disease_free_survival = " no recurrence" then dfs_event = 0;
else if  dfs_event__disease_free_survival = " recurrence" then dfs_event = 1;
if overall_event__death_from_any_ca = " no death" then os_event = 0;
else if  overall_event__death_from_any_ca = " death" then os_event = 1;
run;

proc freq data = GSE17538T3;
table dfs_event os_event;
run;

*On the bright side, one thing I've found is that the transformation used was the 2log transformation!!;

/* ods graphics on;
proc lifetest data=Exposed plots=(survival(atrisk) logsurv);
   time Days*Status(0);
   strata Treatment;
run;
ods graphics off;
In the TIME statement, the survival time variable, Days, is crossed with the censoring variable, Status, with the value 0 indicating censoring. That is, the values of Days are considered censored if the corresponding values of Status are 0; otherwise, they are considered as event times. In the STRATA statement, the variable Treatment is specified, which indicates that the data are to be divided into strata based on the values of Treatment. ODS Graphics must be enabled before producing graphs. Two plots are requested through the PLOTS= optionóa plot of the survival curves with at risk numbers and a plot of the negative log of the survival curves.

*/

*Running the exact same code used in epi3 since on the framingham training dataset the survival curves were non-homogeneous;

*Survival Analysis;

*The number in parenthesis is the value of ëno-eventí;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 10 by 1);	
time dfs_time*dfs_event(0);
strata CFTRCAT;
run;

*CFTR categories;

proc rank data=GSE17538T3 groups=3 out=GSE17538T3;
var CFTR_205043_at__; 
ranks CFTR_tert;
run;

proc rank data=GSE17538T3 groups=4 out=GSE17538T3;
var CFTR_205043_at__; 
ranks CFTR_quart;
run;

proc rank data=GSE17538T3 groups=5 out=GSE17538T3;
var CFTR_205043_at__; 
ranks CFTR_quint;
run;

*CFTR per SD;

proc means data = GSE17538T3 min mean median max std;
var CFTR_205043_at__;
run;

data GSE17538T3;
set GSE17538T3;
CFTR_std = CFTR_205043_at__/405.7689543;
run;

*Creating a variable for cancer stage;
proc freq data = GSE17538T3;
tables ajcc_stage;
run;

data GSE17538T3;
set GSE17538T3;
if ajcc_stage le 1 then cancer_stage = 0;
else if ajcc_stage gt 1 then cancer_stage = 1;
run;

proc freq data = GSE17538T3;
tables ajcc_stage cancer_stage;
run;

*generating a file to import into R2: Did not find similar results when importing the dataset back;

proc export data= GSE17538T3
   outfile= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE17538TKM"
   dbms=XLSX
   replace;
run;

*Saving the dataset;

data mylib.GSE17538T3;
set GSE17538T3;
run;

**********************************************************************************************************************************************************************************;

*GSE39582T;

/*Importing the excel spreadsheet*/

PROC IMPORT OUT= work.GSE39582T DATAFILE= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE39582T.xlsx" DBMS=xlsx REPLACE;
     SHEET="Sheet3"; 
     GETNAMES=YES;
RUN;

proc contents data = GSE39582T;
run;

*My first goal is to reproduce the survival charts from the R2 kaplan meier genomic software (https://hgserver1.amc.nl/cgi-bin/r2/main.cgi);
*However, the expression values in the datasets are much higher than those in the spreadsheet, which leads me to think that the later were already normalized;

proc univariate data = GSE39582T;
var _205043_at;
histogram;
run;

data GSE39582T2;
set GSE39582T;
if _205043_at = 0 then delete;
run;

proc univariate data = GSE39582T2;
var _205043_at;
histogram;
run;

**I've found that you can copy the un-normalized expression data from R2 going to view in 2geneview > datatable since the spreadsheets only have the normalized data;
*Merging those digits to my main dataset;

PROC IMPORT OUT= work.GSE39582TRAW DATAFILE= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE39582TRAW.xlsx" DBMS=xlsx REPLACE;
     SHEET="Sheet1"; 
     GETNAMES=YES;
RUN;

proc contents data = GSE39582TRAW;
run;

proc sort data = GSE39582T2; by ID_REF; run;
proc sort data = GSE39582TRAW; by ID_REF; run;

data GSE39582T3;
merge GSE39582T2 GSE39582TRAW;
by ID_REF;
run;

data GSE39582T3;
set GSE39582T3;
if CFTR_205043_at__ = 0 then delete;
if CFTR_205043_at__ le 1830.8 then CFTRCAT = "L";
else CFTRCAT = "H";
run;


*On the bright side, one thing I've found is that the transformation used was the 2log transformation!!;

/* ods graphics on;
proc lifetest data=Exposed plots=(survival(atrisk) logsurv);
   time Days*Status(0);
   strata Treatment;
run;
ods graphics off;
In the TIME statement, the survival time variable, Days, is crossed with the censoring variable, Status, with the value 0 indicating censoring. That is, the values of Days are considered censored if the corresponding values of Status are 0; otherwise, they are considered as event times. In the STRATA statement, the variable Treatment is specified, which indicates that the data are to be divided into strata based on the values of Treatment. ODS Graphics must be enabled before producing graphs. Two plots are requested through the PLOTS= optionóa plot of the survival curves with at risk numbers and a plot of the negative log of the survival curves.

*/

*Running the exact same code used in epi3 since on the framingham training dataset the survival curves were non-homogeneous;

*Survival Analysis;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time rfs_delay*rfs_event(0);
strata CFTRCAT;
run;

*I AM THE BIGGEST FOOL EVER!!! The number in parenthesis is the value of ëno-eventí, and I used 0, which in this case was the wrong valueeeeee!!!!!!;

PROC LIFETEST data = GSE39582T3 GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 10 by 1);	
time rfs_delay*rfs_event(1);
strata CFTRCAT;
run;

*Survival Analysis;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time os_delay*os_event(0);
strata CFTRCAT;
run;

*I AM THE BIGGEST FOOL EVER!!! The number in parenthesis is the value of ëno-eventí, and I used 0, which in this case was the wrong valueeeeee!!!!!!;

PROC LIFETEST data = GSE39582T3 GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 10 by 1);	
time os_delay*os_event(1);
strata CFTRCAT;
run;

*CFTR categories;

data GSE39582T3;
set GSE39582T3;
Age2 = input(Age, informat.);
run;

proc rank data=GSE39582T3 groups=3 out=GSE39582T3;
var CFTR_205043_at__; 
ranks CFTR_tert;
run;

proc rank data=GSE39582T3 groups=4 out=GSE39582T3;
var CFTR_205043_at__; 
ranks CFTR_quart;
run;

proc rank data=GSE39582T3 groups=5 out=GSE39582T3;
var CFTR_205043_at__; 
ranks CFTR_quint;
run;

*CFTR per SD;

proc means data = GSE39582T3 min mean median max std;
var CFTR_205043_at__;
run;

data GSE39582T3;
set GSE39582T3;
CFTR_std = CFTR_205043_at__/584.9578002;
run;

*Cox regression models for tnm stage, p53 status, kras and braf mutations, mmr status cin status and cimp status;
*I wasn't able to recode the string variables in SAS so I went back to the spreadsheers and used filter to handcode each mutations as 0, 1, and 2 by status;

proc contents data = GSE39582T3;
run;

proc freq data = GSE39582T3;
tables tnm_stage TP53 BRAF KRAS MMR CIMP CIN;
run;

proc freq data = GSE39582T3;
tables tnm_stage;
run;

data GSE39582T3;
set GSE39582T3;
if tnm_stage = "2" then cancer_stage = 1;
else if tnm_stage = "3" then cancer_stage = 1;
else if tnm_stage = "3" then cancer_stage = 1;
else cancer_stage = 0;
run;

*generating a file to import into R2: Did not find similar results when importing the dataset back;

proc export data= GSE39582T3
   outfile= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE39582TKM"
   dbms=XLSX
   replace;
run;

*Saving the dataset;

data mylib.GSE39582T3;
set GSE39582T3;
run;

**********************************************************************************************************************************************************************************;

*GSE33113T;

/*Importing the excel spreadsheet*/

PROC IMPORT OUT= work.GSE33113T DATAFILE= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE33113T.xlsx" DBMS=xlsx REPLACE;
     SHEET="Sheet3"; 
     GETNAMES=YES;
RUN;

proc contents data = GSE33113T;
run;

*My first goal is to reproduce the survival charts from the R2 kaplan meier genomic software (https://hgserver1.amc.nl/cgi-bin/r2/main.cgi);
*However, the expression values in the datasets are much higher than those in the spreadsheet, which leads me to think that the later were already normalized;

proc univariate data = GSE33113T;
var _205043_at;
histogram;
run;

data GSE33113T2;
set GSE33113T;
run;

proc univariate data = GSE33113T2;
var _205043_at;
histogram;
run;

proc means data = GSE33113T2 p25;
var _205043_at;
run;

data GSE33113T3;
set GSE33113T2;
if _205043_at le 305.65 then CFTRCAT = "L";
else CFTRCAT = "H";
run;

*On the bright side, one thing I've found is that the transformation used was the 2log transformation!!;

/* ods graphics on;
proc lifetest data=Exposed plots=(survival(atrisk) logsurv);
   time Days*Status(0);
   strata Treatment;
run;
ods graphics off;
In the TIME statement, the survival time variable, Days, is crossed with the censoring variable, Status, with the value 0 indicating censoring. That is, the values of Days are considered censored if the corresponding values of Status are 0; otherwise, they are considered as event times. In the STRATA statement, the variable Treatment is specified, which indicates that the data are to be divided into strata based on the values of Treatment. ODS Graphics must be enabled before producing graphs. Two plots are requested through the PLOTS= optionóa plot of the survival curves with at risk numbers and a plot of the negative log of the survival curves.

*/

*Running the exact same code used in epi3 since on the framingham training dataset the survival curves were non-homogeneous;

*Survival Analysis;

PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time time*relapse(0);
strata CFTRCAT;
run;

*CFTR categories;

proc rank data=GSE33113T3 groups=3 out=GSE33113T3;
var _205043_at; 
ranks CFTR_tert;
run;

proc rank data=GSE33113T3 groups=4 out=GSE33113T3;
var _205043_at; 
ranks CFTR_quart;
run;

proc rank data=GSE33113T3 groups=5 out=GSE33113T3;
var _205043_at; 
ranks CFTR_quint;
run;

*CFTR per SD;

proc means data = GSE33113T3 min mean median max std;
var _205043_at;
run;

data GSE33113T3;
set GSE33113T3;
CFTR_std = _205043_at/538.60;
run;

*generating a file to import into R2: Did not find similar results when importing the dataset back;

proc export data= GSE92921T3
   outfile= "H:\Professional Folder\WBOB\CFTR Project\Datasets\GSE92921TKM"
   dbms=XLSX
   replace;
run;

*Saving the dataset;

data mylib.GSE33113T3;
set GSE33113T3;
run;

**********************************************************************************************************************************************************************************;

*descriptive stats in preparation of the metaanalysis;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

data GSE14333T3;
set mylib.GSE14333T3;
run;

proc contents data = GSE14333T3;
run;

proc means data = GSE14333T3 min mean median max std;
var CFTR_205043_at__ _Age_Diag;
run;

proc freq data = GSE14333T3;
tables _DukesStage _Gender;
run;

proc univariate NORMAL PLOT data=GSE14333T3; 
var CFTR_205043_at__;
histogram CFTR_205043_at__/normal;
run;

data GSE17538T3;
set mylib.GSE17538T3;
run;

proc contents data = GSE17538T3;
run;

proc means data = GSE17538T3 min mean median max std;
var CFTR_205043_at__ age;
run;

proc freq data = GSE17538T3;
tables grade gender ethnicity ajcc_stage;
run;

proc univariate NORMAL PLOT data=GSE17538T3; 
var CFTR_205043_at__;
histogram CFTR_205043_at__/normal;
run;

data GSE39582T3;
set mylib.GSE39582T3;
run;

proc contents data = GSE39582T3;
run;

proc means data = GSE39582T3 min mean median max std;
var CFTR_205043_at__ Age2;
run;

proc freq data = GSE39582T3;
tables BRAF CIMP CIN KRAS MMR Sex TP53 tnm_stage;
run;

proc univariate NORMAL PLOT data=GSE39582T3; 
var CFTR_205043_at__;
histogram CFTR_205043_at__/normal;
run;

proc contents data = GSE33113T3;
run;

proc means data = GSE33113T3 min mean median max std;
var _205043_at age_at_diagnosis;
run;

proc freq data = GSE33113T3;
tables Sex disease_status;
run;

proc univariate NORMAL PLOT data=GSE33113T3; 
var _205043_at;
histogram _205043_at/normal;
run;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;

*We are calculating different cutpoints accross all of the datasets to see if we can have a common trend;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

**********************************************************************************************************************************************************************************;

data GSE14333T3;
set mylib.GSE14333T3;
run;

proc contents data = GSE14333T3;
run;

proc means data = GSE14333T3 min mean median max std;
var CFTR_205043_at__;
run;

proc univariate NORMAL PLOT data=GSE14333T3; 
var CFTR_205043_at__;
histogram CFTR_205043_at__/normal;
run;

proc univariate data = GSE14333T3;
var CFTR_205043_at__;
histogram CFTR_205043_at__/ normal;
output out=UniWidePctls pctlpre=CFTR_205043_at__P_ pctlpts=2.5,5,10,15,20,25;
run; 

proc print data=UniWidePctls noobs; run;

data GSE14333T3;
set GSE14333T3;
if CFTR_205043_at__ = . then delete;
if CFTR_205043_at__ le 2.5 then CFTR025 = 0; else CFTR025 = 1;
if CFTR_205043_at__ le 44.6 then CFTR050 = 0; else CFTR050 = 1;
if CFTR_205043_at__ le 109.6 then CFTR100 = 0; else CFTR100 = 1;
if CFTR_205043_at__ le 177.7 then CFTR150 = 0; else CFTR150 = 1;
if CFTR_205043_at__ le 257.3 then CFTR200 = 0; else CFTR200 = 1;
if CFTR_205043_at__ le 291.5 then CFTR250 = 0; else CFTR250 = 1;
run;

proc freq data = GSE14333T3;
tables CFTR025 CFTR050 CFTR100 CFTR150 CFTR200 CFTR250;
run;

/*
proc phreg data=GSE14333T3;
class _Gender Location;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR_205043_at__/rl;
run;
*/

proc phreg data=GSE14333T3;
class _Gender Location CFTR025;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR025/rl;
run;

proc phreg data=GSE14333T3;
class _Gender Location CFTR050;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR050/rl;
run;

proc phreg data=GSE14333T3;
class _Gender Location CFTR100;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR100/rl;
run;

proc phreg data=GSE14333T3;
class _Gender Location CFTR150;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR150/rl;
run;

proc phreg data=GSE14333T3;
class _Gender Location CFTR200;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR200/rl;
run;

proc phreg data=GSE14333T3;
class _Gender Location CFTR250;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR250/rl;
run;

proc contents data = GSE14333T3;
run;

proc freq data = GSE14333T3;
tables _DukesStage;
run;

proc sort data = GSE14333T3;
by _DukesStage;
run;

*the dukestage variable had a space before the character variable in the original spreadsheet so I had to add that to the code;

data GSE14333T3;
set GSE14333T3;
Stage = _DukesStage;
if _DukesStage = ' A' then cancer_stage = 0;
if _DukesStage = ' B' then cancer_stage = 1;
if _DukesStage = ' C' then cancer_stage = 1;
if Stage = ' A' then cancer_stage = 0;
if Stage = ' B' then cancer_stage = 1;
if Stage = ' C' then cancer_stage = 1;
run;

proc freq data = GSE14333T3;
tables _DukesStage Stage cancer_stage;
run;

proc phreg data=GSE14333T3;
where cancer_stage = 1;
class _Gender Location CFTR025;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR025/rl;
run;

proc phreg data=GSE14333T3;
where cancer_stage = 1;
class _Gender Location CFTR050;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR050/rl;
run;

proc phreg data=GSE14333T3;
where cancer_stage = 1;
class _Gender Location CFTR100;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR100/rl;
run;

proc phreg data=GSE14333T3;
where cancer_stage = 1;
class _Gender Location CFTR150;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR150/rl;
run;

proc phreg data=GSE14333T3;
where cancer_stage = 1;
class _Gender Location CFTR200;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR200/rl;
run;

proc phreg data=GSE14333T3;
where cancer_stage = 1;
class _Gender Location CFTR250;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

proc phreg data=GSE14333T3;
where cancer_stage = 1;
class _Gender Location Stage CFTR050;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location Stage CFTR050/rl;
run;

proc phreg data=GSE14333T3;
where cancer_stage = 1;
class _Gender Location Stage CFTR100;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location Stage CFTR100/rl;
run;

proc phreg data=GSE14333T3;
where cancer_stage = 1;
class _Gender Location Stage CFTR150;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location Stage CFTR150/rl;
run;

proc phreg data=GSE14333T3;
where cancer_stage = 1;
class _Gender Location Stage CFTR200;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location Stage CFTR200/rl;
run;

proc phreg data=GSE14333T3;
where cancer_stage = 1;
class _Gender Location Stage CFTR250;
model _DFS_Time*_DFS_Cens(1) = _Age_Diag _Gender Location Stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

data GSE17538T3;
set mylib.GSE17538T3;
run;

proc contents data = GSE17538T3;
run;

proc univariate data = GSE17538T3;
var CFTR_205043_at__;
histogram CFTR_205043_at__/ normal;
output out=UniWidePctls pctlpre=CFTR_205043_at__P_ pctlpts=2.5,5,10,15,20,25;
run; 

proc print data=UniWidePctls noobs; run;

data GSE17538T3;
set GSE17538T3;
if CFTR_205043_at__ = . then delete;
if CFTR_205043_at__ le 3.2 then CFTR025 = 0; else CFTR025 = 1;
if CFTR_205043_at__ le 35.15 then CFTR050 = 0; else CFTR050 = 1;
if CFTR_205043_at__ le 96.45 then CFTR100 = 0; else CFTR100 = 1;
if CFTR_205043_at__ le 127.25 then CFTR150 = 0; else CFTR150 = 1;
if CFTR_205043_at__ le 210.9 then CFTR200 = 0; else CFTR200 = 1;
if CFTR_205043_at__ le 273.45 then CFTR250 = 0; else CFTR250 = 1;
run;

proc freq data = GSE17538T3;
tables CFTR025 CFTR050 CFTR100 CFTR150 CFTR200 CFTR250;
run;

/*
data GSE17538T3;
set GSE17538T3;
if CFTR_205043_at__ = . then delete;
if CFTR_205043_at__ le 413.6 then CFTRCAT = "L";
else CFTRCAT = "H";
if dfs_event__disease_free_survival = " no recurrence" then dfs_event = 0;
else if  dfs_event__disease_free_survival = " recurrence" then dfs_event = 1;
if overall_event__death_from_any_ca = " no death" then os_event = 0;
else if  overall_event__death_from_any_ca = " death" then os_event = 1;
run;

proc phreg data=GSE17538T3;
class gender ethnicity grade;
model dfs_time*dfs_event(0) = age gender ethnicity grade CFTR_205043_at__/rl;
run;

proc phreg data=GSE17538T3;
class gender ethnicity ;
model os_time*os_event(0) = age gender CFTR_205043_at__/rl;
run;
*/

proc phreg data=GSE17538T3;
class gender CFTR025;
model dfs_time*dfs_event(0) = age gender CFTR025/rl;
run;

proc phreg data=GSE17538T3;
class gender CFTR050;
model dfs_time*dfs_event(0) = age gender CFTR050/rl;
run;

proc phreg data=GSE17538T3;
class gender CFTR100;
model dfs_time*dfs_event(0) = age gender CFTR100/rl;
run;

proc phreg data=GSE17538T3;
class gender CFTR150;
model dfs_time*dfs_event(0) = age gender CFTR150/rl;
run;

proc phreg data=GSE17538T3;
class gender CFTR200;
model dfs_time*dfs_event(0) = age gender CFTR200/rl;
run;

proc phreg data=GSE17538T3;
class gender CFTR250;
model dfs_time*dfs_event(0) = age gender CFTR250/rl;
run;

proc contents data = GSE17538T3;
run;

proc freq data = GSE17538T3;
tables ajcc_stage;
run;

proc sort data = GSE17538T3;
by ajcc_stage;
run;

data GSE17538T3;
set GSE17538T3;
if ajcc_stage = 1 then cancer_stage = 0;
if ajcc_stage = 2 then cancer_stage = 1;
if ajcc_stage = 3 then cancer_stage = 1;
if ajcc_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE17538T3;
tables ajcc_stage cancer_stage;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR025;
model dfs_time*dfs_event(0) = age gender CFTR025/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR050;
model dfs_time*dfs_event(0) = age gender CFTR050/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR100;
model dfs_time*dfs_event(0) = age gender CFTR100/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR150;
model dfs_time*dfs_event(0) = age gender CFTR150/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR200;
model dfs_time*dfs_event(0) = age gender CFTR200/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR250;
model dfs_time*dfs_event(0) = age gender CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender ajcc_stage CFTR050;
model dfs_time*dfs_event(0) = age gender ajcc_stage CFTR050/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender ajcc_stage CFTR100;
model dfs_time*dfs_event(0) = age gender ajcc_stage CFTR100/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender ajcc_stage CFTR150;
model dfs_time*dfs_event(0) = age gender ajcc_stage CFTR150/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender ajcc_stage CFTR200;
model dfs_time*dfs_event(0) = age gender ajcc_stage CFTR200/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender ajcc_stage CFTR250;
model dfs_time*dfs_event(0) = age gender ajcc_stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Overall Survival;

proc phreg data=GSE17538T3;
class gender CFTR025;
model os_time*os_event(0) = age gender CFTR025/rl;
run;

proc phreg data=GSE17538T3;
class gender CFTR050;
model os_time*os_event(0) = age gender CFTR050/rl;
run;

proc phreg data=GSE17538T3;
class gender CFTR100;
model os_time*os_event(0) = age gender CFTR100/rl;
run;

proc phreg data=GSE17538T3;
class gender CFTR150;
model os_time*os_event(0) = age gender CFTR150/rl;
run;

proc phreg data=GSE17538T3;
class gender CFTR200;
model os_time*os_event(0) = age gender CFTR200/rl;
run;

proc phreg data=GSE17538T3;
class gender CFTR250;
model os_time*os_event(0) = age gender CFTR250/rl;
run;

proc contents data = GSE17538T3;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR025;
model os_time*os_event(0) = age gender CFTR025/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR050;
model os_time*os_event(0) = age gender CFTR050/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR100;
model os_time*os_event(0) = age gender CFTR100/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR150;
model os_time*os_event(0) = age gender CFTR150/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR200;
model os_time*os_event(0) = age gender CFTR200/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender CFTR250;
model os_time*os_event(0) = age gender CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender ajcc_stage CFTR050;
model os_time*os_event(0) = age gender ajcc_stage CFTR050/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender ajcc_stage CFTR100;
model os_time*os_event(0) = age gender ajcc_stage CFTR100/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender ajcc_stage CFTR150;
model os_time*os_event(0) = age gender ajcc_stage CFTR150/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender ajcc_stage CFTR200;
model os_time*os_event(0) = age gender ajcc_stage CFTR200/rl;
run;

proc phreg data=GSE17538T3;
where cancer_stage = 1;
class gender ajcc_stage CFTR250;
model os_time*os_event(0) = age gender ajcc_stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

data GSE39582T3;
set mylib.GSE39582T3;
run;

proc contents data = GSE39582T3;
run;

proc print data = GSE39582T3;
run;

proc univariate data = GSE39582T3;
var CFTR_205043_at__;
histogram CFTR_205043_at__/ normal;
output out=UniWidePctls pctlpre=CFTR_205043_at__P_ pctlpts=2.5,5,10,15,20,25;
run; 

proc print data=UniWidePctls noobs; run;

data GSE39582T3;
set GSE39582T3;
if CFTR_205043_at__ = . then delete;
if CFTR_205043_at__ le 32.7 then CFTR025 = 0; else CFTR025 = 1;
if CFTR_205043_at__ le 52.1 then CFTR050 = 0; else CFTR050 = 1;
if CFTR_205043_at__ le 149.8 then CFTR100 = 0; else CFTR100 = 1;
if CFTR_205043_at__ le 227.8 then CFTR150 = 0; else CFTR150 = 1;
if CFTR_205043_at__ le 317.2 then CFTR200 = 0; else CFTR200 = 1;
if CFTR_205043_at__ le 394.8 then CFTR250 = 0; else CFTR250 = 1;
run;

proc freq data = GSE39582T3;
tables CFTR025 CFTR050 CFTR100 CFTR150 CFTR200 CFTR250;
run;

/*
proc phreg data=GSE39582T3;
class Sex;
model rfs_delay*rfs_event(0) = Age2 Sex CFTR_205043_at__/rl;
run;

proc phreg data=GSE39582T3;
class Sex;
model os_delay*os_event(0) = Age2 Sex CFTR_205043_at__/rl;
run;
*/

proc phreg data=GSE39582T3;
class Sex CFTR025;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3;
class Sex CFTR050;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3;
class Sex CFTR100;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3;
class Sex CFTR150;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3;
class Sex CFTR200;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3;
class Sex CFTR250;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR250/rl;
run;

proc contents data = GSE39582T3;
run;

proc freq data = GSE39582T3;
tables tnm_stage;
run;

proc sort data = GSE39582T3;
by tnm_stage;
run;

*the dukestage variable had a space before the character variable in the original spreadsheet so I had to add that to the code;

data GSE39582T3;
set GSE39582T3;
if tnm_stage = 0 then cancer_stage = 0;
if tnm_stage = 1 then cancer_stage = 0;
if tnm_stage = 2 then cancer_stage = 1;
if tnm_stage = 3 then cancer_stage = 1;
if tnm_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE39582T3;
tables tnm_stage cancer_stage;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR025;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR050;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR100;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR150;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR200;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR250;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR250/rl;
run;

proc freq data = GSE39582T3;
tables tnm_stage cancer_stage MMR;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR025;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR050;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR100;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR150;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR200;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR250;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR050;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR050/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR100;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR100/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR150;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR150/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR200;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR200/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR250;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage, without restricting to MSS samples;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex tnm_stage CFTR050;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR050/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex tnm_stage CFTR100;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR100/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex tnm_stage CFTR150;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR150/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex tnm_stage CFTR200;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR200/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex tnm_stage CFTR250;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Overall Survival;

proc phreg data=GSE39582T3;
class Sex CFTR025;
model os_delay*os_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3;
class Sex CFTR050;
model os_delay*os_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3;
class Sex CFTR100;
model os_delay*os_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3;
class Sex CFTR150;
model os_delay*os_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3;
class Sex CFTR200;
model os_delay*os_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3;
class Sex CFTR250;
model os_delay*os_event(0) = Age2 Sex  CFTR250/rl;
run;

proc contents data = GSE39582T3;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR025;
model os_delay*os_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR050;
model os_delay*os_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR100;
model os_delay*os_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR150;
model os_delay*os_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR200;
model os_delay*os_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex CFTR250;
model os_delay*os_event(0) = Age2 Sex  CFTR250/rl;
run;

proc freq data = GSE39582T3;
tables tnm_stage cancer_stage MMR;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR025;
model os_delay*os_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR050;
model os_delay*os_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR100;
model os_delay*os_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR150;
model os_delay*os_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR200;
model os_delay*os_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR250;
model os_delay*os_event(0) = Age2 Sex  CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR050;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR050/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR100;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR100/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR150;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR150/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR200;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR200/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR250;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage, without reestricting to MSS samples;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex tnm_stage CFTR050;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR050/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex tnm_stage CFTR100;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR100/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex tnm_stage CFTR150;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR150/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex tnm_stage CFTR200;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR200/rl;
run;

proc phreg data=GSE39582T3;
where cancer_stage = 1;
class Sex tnm_stage CFTR250;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Testing the raw data for GSE39582 instead of the R2 data;
*The results for DFS were significant at the 15th percentile, and those for overall survial were significant at the 15th, 20th and 25th percentiles;

data GSE39582T3;
set mylib.GSE39582T3;
run;

proc contents data = GSE39582T3;
run;

proc print data = GSE39582T3;
run;

proc univariate data = GSE39582T3;
var _205043_at;
histogram _205043_at/ normal;
output out=UniWidePctls pctlpre=_205043_atP_ pctlpts=2.5,5,10,15,20,25;
run; 

proc print data=UniWidePctls noobs; run;

data GSE39582T3V2;
set GSE39582T3;
if _205043_at = . then delete;
if _205043_at le 5.15023 then CFTR025 = 0; else CFTR025 = 1;
if _205043_at le 5.73784 then CFTR050 = 0; else CFTR050 = 1;
if _205043_at le 7.22611 then CFTR100 = 0; else CFTR100 = 1;
if _205043_at le 7.83181 then CFTR150 = 0; else CFTR150 = 1;
if _205043_at le 8.31061 then CFTR200 = 0; else CFTR200 = 1;
if _205043_at le 8.64075 then CFTR250 = 0; else CFTR250 = 1;
run;

proc freq data = GSE39582T3V2;
tables CFTR025 CFTR050 CFTR100 CFTR150 CFTR200 CFTR250;
run;

/*
proc phreg data=GSE39582T3;
class Sex;
model rfs_delay*rfs_event(0) = Age2 Sex CFTR_205043_at__/rl;
run;

proc phreg data=GSE39582T3;
class Sex;
model os_delay*os_event(0) = Age2 Sex CFTR_205043_at__/rl;
run;
*/

proc phreg data=GSE39582T3V2;
class Sex CFTR025;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3V2;
class Sex CFTR050;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3V2;
class Sex CFTR100;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3V2;
class Sex CFTR150;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3V2;
class Sex CFTR200;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3V2;
class Sex CFTR250;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR250/rl;
run;

proc contents data = GSE39582T3V2;
run;

proc freq data = GSE39582T3V2;
tables tnm_stage;
run;

proc sort data = GSE39582T3V2;
by tnm_stage;
run;

*the dukestage variable had a space before the character variable in the original spreadsheet so I had to add that to the code;

data GSE39582T3V2;
set GSE39582T3V2;
if tnm_stage = 0 then cancer_stage = 0;
if tnm_stage = 1 then cancer_stage = 0;
if tnm_stage = 2 then cancer_stage = 1;
if tnm_stage = 3 then cancer_stage = 1;
if tnm_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE39582T3V2;
tables tnm_stage cancer_stage;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR025;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR050;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR100;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR150;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR200;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR250;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR250/rl;
run;

proc freq data = GSE39582T3V2;
tables tnm_stage cancer_stage MMR;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR025;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR050;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR100;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR150;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR200;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR250;
model rfs_delay*rfs_event(0) = Age2 Sex  CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR050;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR050/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR100;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR100/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR150;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR150/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR200;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR200/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR250;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage, without reestricting to MSS samples;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex tnm_stage CFTR050;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR050/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex tnm_stage CFTR100;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR100/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex tnm_stage CFTR150;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR150/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex tnm_stage CFTR200;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR200/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex tnm_stage CFTR250;
model rfs_delay*rfs_event(0) = Age2 Sex tnm_stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Overall Survival;

proc phreg data=GSE39582T3V2;
class Sex CFTR025;
model os_delay*os_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3V2;
class Sex CFTR050;
model os_delay*os_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3V2;
class Sex CFTR100;
model os_delay*os_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3V2;
class Sex CFTR150;
model os_delay*os_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3V2;
class Sex CFTR200;
model os_delay*os_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3V2;
class Sex CFTR250;
model os_delay*os_event(0) = Age2 Sex  CFTR250/rl;
run;

proc contents data = GSE39582T3V2;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR025;
model os_delay*os_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR050;
model os_delay*os_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR100;
model os_delay*os_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR150;
model os_delay*os_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR200;
model os_delay*os_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex CFTR250;
model os_delay*os_event(0) = Age2 Sex  CFTR250/rl;
run;

proc freq data = GSE39582T3V2;
tables tnm_stage cancer_stage MMR;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR025;
model os_delay*os_event(0) = Age2 Sex  CFTR025/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR050;
model os_delay*os_event(0) = Age2 Sex  CFTR050/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR100;
model os_delay*os_event(0) = Age2 Sex  CFTR100/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR150;
model os_delay*os_event(0) = Age2 Sex  CFTR150/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR200;
model os_delay*os_event(0) = Age2 Sex  CFTR200/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex CFTR250;
model os_delay*os_event(0) = Age2 Sex  CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR050;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR050/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR100;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR100/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR150;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR150/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR200;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR200/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1 and MMR = 2;
class Sex tnm_stage CFTR250;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage, without reestricting to MSS samples;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex tnm_stage CFTR050;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR050/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex tnm_stage CFTR100;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR100/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex tnm_stage CFTR150;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR150/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex tnm_stage CFTR200;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR200/rl;
run;

proc phreg data=GSE39582T3V2;
where cancer_stage = 1;
class Sex tnm_stage CFTR250;
model os_delay*os_event(0) = Age2 Sex tnm_stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

data GSE33113T3;
set mylib.GSE33113T3;
run;

proc contents data = GSE33113T3;
run;

proc univariate data = GSE33113T3;
var _205043_at;
histogram _205043_at/ normal;
output out=UniWidePctls pctlpre=_205043_atP_ pctlpts=2.5,5,10,15,20,25;
run; 

proc print data=UniWidePctls noobs; run;

data GSE33113T3;
set GSE33113T3;
if _205043_at = . then delete;
if _205043_at le 3.1 then CFTR025 = 0; else CFTR025 = 1;
if _205043_at le 26.4 then CFTR050 = 0; else CFTR050 = 1;
if _205043_at le 101.9 then CFTR100 = 0; else CFTR100 = 1;
if _205043_at le 167.9 then CFTR150 = 0; else CFTR150 = 1;
if _205043_at le 258.7 then CFTR200 = 0; else CFTR200 = 1;
if _205043_at le 305.65 then CFTR250 = 0; else CFTR250 = 1;
run;

proc freq data = GSE33113T3;
tables CFTR025 CFTR050 CFTR100 CFTR150 CFTR200 CFTR250;
run;

/*
proc phreg data=GSE33113T3;
class Sex;
model time*relapse(0) = age_at_diagnosis Sex _205043_at/rl;
run;
*/

proc phreg data=GSE33113T3;
class Sex CFTR025;
model time*relapse(0) = age_at_diagnosis Sex  CFTR025/rl;
run;

proc phreg data=GSE33113T3;
class Sex CFTR050;
model time*relapse(0) = age_at_diagnosis Sex  CFTR050/rl;
run;

proc phreg data=GSE33113T3;
class Sex CFTR100;
model time*relapse(0) = age_at_diagnosis Sex  CFTR100/rl;
run;

proc phreg data=GSE33113T3;
class Sex CFTR150;
model time*relapse(0) = age_at_diagnosis Sex  CFTR150/rl;
run;

proc phreg data=GSE33113T3;
class Sex CFTR200;
model time*relapse(0) = age_at_diagnosis Sex  CFTR200/rl;
run;

proc phreg data=GSE33113T3;
class Sex CFTR250;
model time*relapse(0) = age_at_diagnosis Sex CFTR250/rl;
run;


**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Creating Zscore values for all datasets to combine them;
*In this version, I will also create a zscore based on the log2 scale values: In the initial analysis during the summer 2019, the 
TCGA data was on the log2 scale, while the R2 GSE data was non transformed and the GSE direct download was on the log2 scale.
This makes sence since most GSE data are normalized in the affy package (in R) which normalize data on the log2 scale.
Luckily I saved both the raw value (which was the untransformed R2 values) and the log2 scale values for each of the datasets);

options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

data GSE14333T3;
set mylib.GSE14333T3;
run;

proc contents data = GSE14333T3;
run;

data GSE14333T3;
set GSE14333T3;
Stage = _DukesStage;
if _DukesStage = ' A' then cancer_stage = 0;
if _DukesStage = ' B' then cancer_stage = 1;
if _DukesStage = ' C' then cancer_stage = 1;
if Stage = ' A' then cancer_stage = 0;
if Stage = ' B' then cancer_stage = 1;
if Stage = ' C' then cancer_stage = 1;
run;

proc freq data = GSE14333T3;
tables _DukesStage Stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE14333T3 mean std;
var CFTR_205043_at__ _205043_at; 
run;

data GSE14333T3;
set GSE14333T3;
ZCFTR = (CFTR_205043_at__- 651.3318584)/ 467.5589204;
ZCFTR2 = (_205043_at- 9.1535852)/ 1.8360383;
run;

proc means data = GSE14333T3 mean std min max;
var CFTR_205043_at__ ZCFTR ZCFTR2; 
run;

proc contents data = GSE14333T3;
run;

data GSE14333T3;
set GSE14333T3;
ID = "ID_REF";
DATASET = "GSE14333T3";
if _DFS_Cens = 0 then DFS = 1;
else if _DFS_Cens = 1 then DFS = 0;
DFSTIME = _DFS_Time;
DFSTIMEMONTH = _DFS_Time;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = CFTR_205043_at__;
CFTRLEVELLOG = _205043_at;
run;

data GSE14333T3MKI;
set GSE14333T3 (keep = ID DATASET DFS DFSTIME DFSTIMEMONTH CANCERSTAGE CFTRLEVEL ZCFTR CFTRLEVELLOG ZCFTR2);
run;

proc contents data = GSE14333T3MKI;
run;

data GSE17538T3;
set mylib.GSE17538T3;
run;

proc contents data = GSE17538T3;
run;

data GSE17538T3;
set GSE17538T3;
if ajcc_stage = 1 then cancer_stage = 0;
if ajcc_stage = 2 then cancer_stage = 1;
if ajcc_stage = 3 then cancer_stage = 1;
if ajcc_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE17538T3;
tables ajcc_stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE17538T3 mean std;
var CFTR_205043_at__ _205043_at; 
run;

data GSE17538T3;
set GSE17538T3;
ZCFTR = (CFTR_205043_at__- 595.1975000)/ 405.7689543;
ZCFTR2 = (_205043_at- 9.9299050)/ 1.1211490;
run;

proc means data = GSE17538T3 mean std min max;
var CFTR_205043_at__ _205043_at ZCFTR ZCFTR2; 
run;

proc contents data = GSE17538T3;
run;

data GSE17538T3;
set GSE17538T3;
ID = "ID_REF";
DATASET = "GSE17538T3";
if dfs_event = 0 then DFS = 0;
else if dfs_event = 1 then DFS = 1;
DFSTIME = dfs_time;
DFSTIMEMONTH = dfs_time;
if os_event = 0 then OS = 0;
else if os_event = 1 then OS = 1;
OSTIME = os_time;
OSTIMEMONTH = os_time;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = CFTR_205043_at__; 
CFTRLEVELLOG = _205043_at; 
run;

data GSE17538T3MKI;
set GSE17538T3 (keep = ID DATASET DFS DFSTIME DFSTIMEMONTH CANCERSTAGE CFTRLEVEL CFTRLEVELLOG ZCFTR ZCFTR2);
run;

proc contents data = GSE17538T3MKI;
run;

data GSE17538T3MKII;
set GSE17538T3 (keep = ID DATASET OS OSTIME OSTIMEMONTH CANCERSTAGE CFTRLEVEL CFTRLEVELLOG ZCFTR ZCFTR2);
run;

proc contents data = GSE17538T3MKII;
run;

data GSE39582T3;
set mylib.GSE39582T3;
run;

proc contents data = GSE39582T3;
run;

data GSE39582T3;
set GSE39582T3;
if tnm_stage = 0 then cancer_stage = 0;
if tnm_stage = 1 then cancer_stage = 0;
if tnm_stage = 2 then cancer_stage = 1;
if tnm_stage = 3 then cancer_stage = 1;
if tnm_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE39582T3;
tables tnm_stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE39582T3 mean std;
var _205043_at; 
run;

data GSE39582T3;
set GSE39582T3;
ZCFTR = (_205043_at- 9.2281037)/ 1.4876138;
ZCFTR2 = (_205043_at- 9.2281037)/ 1.4876138;
run;

proc means data = GSE39582T3 mean std min max;
var _205043_at ZCFTR; 
run;

proc contents data = GSE39582T3;
run;

data GSE39582T3;
set GSE39582T3;
ID = "ID_REF";
DATASET = "GSE39582T3";
if rfs_event = 0 then DFS = 0;
else if rfs_event = 1 then DFS = 1;
DFSTIME = rfs_delay;
DFSTIMEMONTH = rfs_delay;
if os_event = 0 then OS = 0;
else if os_event = 1 then OS = 1;
OSTIME = os_delay;
OSTIMEMONTH = os_delay;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = _205043_at;
CFTRLEVELLOG = _205043_at;
run;

data GSE39582T3MKI;
set GSE39582T3 (keep = ID DATASET DFS DFSTIME DFSTIMEMONTH CANCERSTAGE CFTRLEVEL CFTRLEVELLOG ZCFTR ZCFTR2);
run;

proc contents data = GSE39582T3MKI;
run;

data GSE39582T3MKII;
set GSE39582T3 (keep = ID DATASET OS OSTIME OSTIMEMONTH CANCERSTAGE CFTRLEVEL CFTRLEVELLOG ZCFTR ZCFTR2);
run;

proc contents data = GSE39582T3MKII;
run;

data GSE33113T3;
set mylib.GSE33113T3;
run;

proc contents data = GSE33113T3;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE33113T3 mean std;
var _205043_at; 
run;

*Since the original GSE33113 did not have values normalized, I will need to log transform them manually;
data GSE33113T3;
set GSE33113T3;
CFTRLOG = LOG2(_205043_at);
run;

data GSE33113T3;
set GSE33113T3;
ZCFTR = (_205043_at- 735.8114583)/538.6032849;
ZCFTR2 = (CFTRLOG - 8.8227871)/ 1.9920439;
run;

proc means data = GSE33113T3 mean std min max;
var _205043_at CFTRLOG ZCFTR ZCFTR2 time; 
run;

proc contents data = GSE33113T3;
run;

data GSE33113T3;
set GSE33113T3;
ID = "ID_REF";
DATASET = "GSE33113T3";
if relapse = 0 then DFS = 0;
else if relapse = 1 then DFS = 1;
DFSTIME = time;
DFSTIMEMONTH = time/30.5;
CANCERSTAGE = 1;
CFTRLEVEL = _205043_at;
CFTRLEVELLOG = CFTRLOG;
run;

data GSE33113T3MKI;
set GSE33113T3 (keep = ID DATASET DFS DFSTIME DFSTIMEMONTH CANCERSTAGE CFTRLEVEL CFTRLEVELLOG ZCFTR ZCFTR2);
run;

proc contents data = GSE33113T3MKI;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Now doing the combined cutpoint analysis using the CFTR values on the log2 scale, and only the datasets with CRC data;

**********************************************************************************************************************************************************************************;

*Combining the datasets with Disease free survival;

data DFSCFTR;
set GSE14333T3MKI GSE17538T3MKI GSE39582T3MKI GSE33113T3MKI;
run;

proc freq data = DFSCFTR;
table DATASET;
run;

*Testing for homogeneity of survival curves over strata;

data DFSCFTR;
set GSE14333T3MKI GSE17538T3MKI GSE33113T3MKI;
run;

proc lifetest method = life plot = s;
time DFSTIMEMONTH*DFS(0);
strata DATASET;
run;

proc lifetest method = life plot = s;
time DFSTIMEMONTH*DFS(0);
strata DATASET;
run;

proc logistic data = DFSCFTR;
where CANCERSTAGE = 1;
model DFS (ref='0') = ZCFTR;
run;

proc logistic data = DFSCFTR;
where CANCERSTAGE = 1;
model DFS (event='1') = ZCFTR;
run;

proc logistic data = DFSCFTR;
model DFS (ref='0') = ZCFTR;
run;

proc logistic data = DFSCFTR;
model DFS (event='1') = ZCFTR;
run;

proc logistic data = DFSCFTR;
where CANCERSTAGE = 1;
model DFS (ref='0') = ZCFTR2;
run;

proc logistic data = DFSCFTR;
where CANCERSTAGE = 1;
model DFS (event='1') = ZCFTR2;
run;

proc logistic data = DFSCFTR;
model DFS (ref='0') = ZCFTR2;
run;

proc logistic data = DFSCFTR;
model DFS (event='1') = ZCFTR2;
run;

*Going to use the full dataset (not restrict to cancerstage = 1) since ZCFTR is associated with DFS in that model;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

ods graphics on;
proc logistic data=DFSCFTR;
where CANCERSTAGE = 1;
model DFS (event='1') = ZCFTR2 / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

*I exported te table to excel to calculate youden's statistic;
*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;

Logit(0.26) = -1.1452 --0.2844 *(X)
X = [Logit(0.28) + (-1.1452)]/(-0.2844);

*use logit calculator https://www.medcalc.org/manual/logit_function.php;

X = -0.348915066;

*Testing the cutpoint in the overall dataset;

data DFSCFTR;
set DFSCFTR;
if ZCFTR2 LT -0.348915066 then OPTIMAL = 0;
else if ZCFTR2 GE -0.348915066 then OPTIMAL = 1;
run;

*Testing the proportional hazard assumption accross datasets;

proc freq data = DFSCFTR;
tables DATASET;
run;

data DFSCFTR;
set DFSCFTR;
if DATASET = "GSE14333T3" then DATASET2 = 1;
else if DATASET = "GSE17538T3" then DATASET2 = 2;
else if DATASET = "GSE33113T3" then DATASET2 = 3;
run;

proc freq data = DFSCFTR;
tables DATASET DATASET2;
run;

proc phreg data=DFSCFTR;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1") DATASET2;
model DFSTIMEMONTH*DFS(0) = OPTIMAL phazard/rl;
phazard = DATASET2*(log(DFSTIMEMONTH));
run;

proc phreg data=DFSCFTR;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = OPTIMAL phazard/rl;
phazard = optimal*(log(DFSTIMEMONTH));
run;

proc phreg data=DFSCFTR;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

*Or check (https://communities.sas.com/t5/Statistical-Procedures/95-confidence-intervals-of-Youden-index/td-p/542313) to get youden 95% CI;

*Testing the cutpoints;

data GSE14333T3MKI;
set GSE14333T3MKI;
if ZCFTR2 LT  -0.348915066 then OPTIMAL = 0;
else if ZCFTR2 GE  -0.348915066 then OPTIMAL = 1;
run;

proc logistic data=GSE14333T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFS (event='1') = OPTIMAL;
run;

proc phreg data=GSE14333T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = OPTIMAL/rl;
run;

proc phreg data=GSE14333T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = CFTRLEVEL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE14333T3MKI;
tables DFS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE14333T3;
set GSE14333T3;
if ZCFTR2 LT  -0.348915066 then OPTIMAL = 0;
else if ZCFTR2 GE  -0.348915066 then OPTIMAL = 1;
run;

proc logistic data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage OPTIMAL(ref="1");
model DFS (event='1') = Stage OPTIMAL;
run;

proc phreg data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = Stage OPTIMAL/rl;
run;

proc phreg data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = Stage CFTRLEVEL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc contents data=GSE14333T3;
run;

************************************************************************************************************************************************************;

proc contents data = GSE14333T3;
run;

proc univariate data = GSE14333T3;
var CFTRLEVELLOG;
histogram CFTRLEVELLOG/ normal;
output out=UniWidePctls pctlpre=CFTRLEVELLOGP_ pctlpts=2.5,5,10,15,20,25;
run; 

proc print data=UniWidePctls noobs; run;

data GSE14333T3;
set GSE14333T3;
if CFTRLEVELLOG le 5.92121 then CFTR050 = 0; else CFTR050 = 1;
if CFTRLEVELLOG le 7.24462 then CFTR100 = 0; else CFTR100 = 1;
if CFTRLEVELLOG le 7.94110 then CFTR150 = 0; else CFTR150 = 1;
if CFTRLEVELLOG le 8.42752 then CFTR200 = 0; else CFTR200 = 1;
if CFTRLEVELLOG le 8.63087 then CFTR250 = 0; else CFTR250 = 1;
run;

proc freq data = GSE14333T3;
tables CFTR050 CFTR100 CFTR150 CFTR200 CFTR250;
run;

proc phreg data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage CFTR050;
model DFSTIMEMONTH*DFS(0) = Stage CFTR050/rl;
run;

proc phreg data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage CFTR100;
model DFSTIMEMONTH*DFS(0) = Stage CFTR100/rl;
run;

proc phreg data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage CFTR150;
model DFSTIMEMONTH*DFS(0) = Stage CFTR150/rl;
run;

proc phreg data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage CFTR200;
model DFSTIMEMONTH*DFS(0) = Stage CFTR200/rl;
run;

proc phreg data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage CFTR250;
model DFSTIMEMONTH*DFS(0) = Stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

data GSE17538T3MKI;
set GSE17538T3MKI;
if ZCFTR2 LT  -0.348915066 then OPTIMAL = 0;
else if ZCFTR2 GE  -0.348915066 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFS (event='1') = OPTIMAL;
run;

proc phreg data=GSE17538T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = OPTIMAL/rl;
run;

proc phreg data=GSE17538T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = CFTRLEVEL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE17538T3MKI;
tables DFS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE17538T3;
set GSE17538T3;
if ZCFTR2 LT  -0.348915066 then OPTIMAL = 0;
else if ZCFTR2 GE  -0.348915066 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model DFS (event='1') = ajcc_stage OPTIMAL;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = ajcc_stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = ajcc_stage CFTRLEVEL/rl;
run;

************************************************************************************************************************************************************;

proc contents data = GSE17538T3;
run;

proc univariate data = GSE17538T3;
var CFTRLEVELLOG;
histogram CFTRLEVELLOG/ normal;
output out=UniWidePctls pctlpre=CFTRLEVELLOGP_ pctlpts=2.5,5,10,15,20,25;
run; 

proc print data=UniWidePctls noobs; run;

data GSE17538T3;
set GSE17538T3;
if CFTRLEVELLOG le 7.48909 then CFTR050 = 0; else CFTR050 = 1;
if CFTRLEVELLOG le 8.35295 then CFTR100 = 0; else CFTR100 = 1;
if CFTRLEVELLOG le 8.65901 then CFTR150 = 0; else CFTR150 = 1;
if CFTRLEVELLOG le 9.06660 then CFTR200 = 0; else CFTR200 = 1;
if CFTRLEVELLOG le 9.40925 then CFTR250 = 0; else CFTR250 = 1;
run;

proc freq data = GSE17538T3;
tables CFTR050 CFTR100 CFTR150 CFTR200 CFTR250;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage CFTR050(ref="1");
model DFSTIMEMONTH*DFS(0) = ajcc_stage CFTR050/rl;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage CFTR100(ref="1");
model DFSTIMEMONTH*DFS(0) = ajcc_stage CFTR100/rl;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage CFTR150(ref="1");
model DFSTIMEMONTH*DFS(0) = ajcc_stage CFTR150/rl;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage CFTR200(ref="1");
model DFSTIMEMONTH*DFS(0) = ajcc_stage CFTR200/rl;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage CFTR250(ref="1");
model DFSTIMEMONTH*DFS(0) = ajcc_stage CFTR250/rl;
run;

************************************************************************************************************************************************************;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage CFTR050(ref="1");
model OSTIMEMONTH*OS(0) = ajcc_stage CFTR050/rl;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage CFTR100(ref="1");
model OSTIMEMONTH*OS(0) = ajcc_stage CFTR100/rl;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage CFTR150(ref="1");
model OSTIMEMONTH*OS(0) = ajcc_stage CFTR150/rl;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage CFTR200(ref="1");
model OSTIMEMONTH*OS(0) = ajcc_stage CFTR200/rl;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage CFTR250(ref="1");
model OSTIMEMONTH*OS(0) = ajcc_stage CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

/*

data GSE39582T3MKI;
set GSE39582T3MKI;
if ZCFTR2 LT  -0.348915066 then OPTIMAL = 0;
else if ZCFTR2 GE  -0.348915066 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFS (event='1') = OPTIMAL;
run;

proc phreg data=GSE39582T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = OPTIMAL/rl;
run;

proc phreg data=GSE39582T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = CFTRLEVEL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE39582T3MKI;
tables DFS*OPTIMAL;
run;

*/

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;
*Running the association in GSE39582 at Dr. Prizment's request for the april 15th presentation;

proc contents data = GSE39582T3;
run;

proc freq data = GSE39582T3;
table MMR;
run;

data GSE39582T3;
set GSE39582T3;
if ZCFTR2 LT  -0.348915066 then OPTIMAL = 0;
else if ZCFTR2 GE  -0.348915066 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model DFS (event='1') = tnm_stage OPTIMAL;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1 and MMR = 2;
class tnm_stage OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = tnm_stage OPTIMAL/rl;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1 and MMR = 2;
class tnm_stage OPTIMAL(ref="1");
model OSTIMEMONTH*OS(0) = tnm_stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = tnm_stage CFTRLEVEL/rl;
run;

**********************************************************************************************************************************************************************************;

data GSE33113T3MKI;
set GSE33113T3MKI;
if ZCFTR2 LT  -0.348915066 then OPTIMAL = 0;
else if ZCFTR2 GE  -0.348915066 then OPTIMAL = 1;
run;

proc logistic data=GSE33113T3MKI;
class OPTIMAL(ref="1");
model DFS (event='1') = OPTIMAL;
run;

proc phreg data=GSE33113T3MKI;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = OPTIMAL/rl;
run;

proc phreg data=GSE33113T3MKI;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS(0) = CFTRLEVEL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE33113T3MKI;
tables DFS*OPTIMAL;
run;

************************************************************************************************************************************************************;

proc contents data = GSE33113T3;
run;

proc univariate data = GSE33113T3;
var CFTRLEVELLOG;
histogram CFTRLEVELLOG/ normal;
output out=UniWidePctls pctlpre=CFTRLEVELLOGP_ pctlpts=2.5,5,10,15,20,25;
run; 

proc print data=UniWidePctls noobs; run;

data GSE33113T3;
set GSE33113T3;
if CFTRLEVELLOG le 4.72247 then CFTR050 = 0; else CFTR050 = 1;
if CFTRLEVELLOG le 6.67101 then CFTR100 = 0; else CFTR100 = 1;
if CFTRLEVELLOG le 7.39146 then CFTR150 = 0; else CFTR150 = 1;
if CFTRLEVELLOG le 8.01514 then CFTR200 = 0; else CFTR200 = 1;
if CFTRLEVELLOG le 8.25534 then CFTR250 = 0; else CFTR250 = 1;
run;

proc freq data = GSE33113T3;
tables CFTR050 CFTR100 CFTR150 CFTR200 CFTR250;
run;

proc phreg data=GSE33113T3;
class CFTR050(ref="1");
model DFSTIMEMONTH*DFS(0) = CFTR050/rl;
run;

proc phreg data=GSE33113T3;
class CFTR100(ref="1");
model DFSTIMEMONTH*DFS(0) = CFTR100/rl;
run;

proc phreg data=GSE33113T3;
class CFTR150(ref="1");
model DFSTIMEMONTH*DFS(0) = CFTR150/rl;
run;

proc phreg data=GSE33113T3;
class CFTR200(ref="1");
model DFSTIMEMONTH*DFS(0) = CFTR200/rl;
run;

proc phreg data=GSE33113T3;
class CFTR250(ref="1");
model DFSTIMEMONTH*DFS(0) = CFTR250/rl;
run;

**********************************************************************************************************************************************************************************;

*Combining the datasets with overall survival (without GSE39582 since it is colon cancer only);

data OSCFTR;
set GSE17538T3MKII;
run;

proc freq data = OSCFTR;
table DATASET;
run;

proc logistic data = OSCFTR;
where CANCERSTAGE = 1;
model OS (ref='0') = ZCFTR;
run;

proc logistic data = OSCFTR;
where CANCERSTAGE = 1;
model OS (event='1') = ZCFTR;
run;

proc logistic data = OSCFTR;
model OS (ref='0') = ZCFTR;
run;

proc logistic data = OSCFTR;
model OS (event='1') = ZCFTR;
run;

proc logistic data = OSCFTR;
where CANCERSTAGE = 1;
model OS (ref='0') = ZCFTR2;
run;

proc logistic data = OSCFTR;
where CANCERSTAGE = 1;
model OS (event='1') = ZCFTR2;
run;

proc logistic data = OSCFTR;
model OS (ref='0') = ZCFTR2;
run;

proc logistic data = OSCFTR;
model OS (event='1') = ZCFTR2;
run;

*Going to use the full dataset (not restrict to cancerstage = 1) since ZCFTR sis associated with OS in that model;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

ods graphics on;
proc logistic data=OSCFTR;
model OS (event='1') = ZCFTR / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

ods graphics on;
proc logistic data=OSCFTR;
where CANCERSTAGE = 1;
model OS (event='1') = ZCFTR2 / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

*I exported te table to excel to calculate youden's statistic;
*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;

Logit(0.40) = -0.7981 -0.4663 *(X)
X = [Logit(0.40) + (-0.7981)]/(-0.4663);

*use logit calculator https://www.medcalc.org/manual/logit_function.php;

X = -0.842022071;

*Or check (https://communities.sas.com/t5/Statistical-Procedures/95-confidence-intervals-of-Youden-index/td-p/542313) to get youden 95% CI;

*Testing the cutpoints;

data GSE17538T3MKII;
set GSE17538T3MKII;
if ZCFTR2 LT -0.842022071 then OPTIMAL = 0;
else if ZCFTR2 GE -0.842022071 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OS (event='1') = OPTIMAL;
run;

proc phreg data=GSE17538T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OSTIME*OS(0) = OPTIMAL/rl;
run;

proc phreg data=GSE17538T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OSTIME*OS(0) = CFTRLEVEL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIMEMONTH*OS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE17538T3MKII;
tables OS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE17538T3;
set GSE17538T3;
if ZCFTR2 LT -0.842022071 then OPTIMAL = 0;
else if ZCFTR2 GE -0.842022071 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model OS (event='1') = ajcc_stage OPTIMAL;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model OSTIMEMONTH*OS(0) = ajcc_stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIMEMONTH*OS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model OSTIMEMONTH*OS(0) = ajcc_stage CFTRLEVEL/rl;
run;

**********************************************************************************************************************************************************************************;

/*
data GSE39582T3MKII;
set GSE39582T3MKII;
if ZCFTR LT -0.345885273 then OPTIMAL = 0;
else if ZCFTR GE -0.345885273 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OS (event='1') = OPTIMAL;
run;

proc phreg data=GSE39582T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OSTIME*OS(0) = OPTIMAL/rl;
run;

proc phreg data=GSE39582T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OSTIME*OS(0) = CFTRLEVEL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIME*OS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE39582T3MKII;
tables OS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE39582T3;
set GSE39582T3;
if ZCFTR LT -0.345885273 then OPTIMAL = 0;
else if ZCFTR GE -0.345885273 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model OS (event='1') = tnm_stage OPTIMAL;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model OSTIME*OS(0) = tnm_stage OPTIMAL/rl;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model OSTIME*OS(0) = tnm_stage CFTRLEVEL/rl;
run;
*/
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Getting the values for the descriptive tables;

data GSE14333T3;
set GSE14333T3;
ZCFTR = (CFTR_205043_at__- 651.3318584)/ 467.5589204;
run;

data GSE17538T3;
set GSE17538T3;
ZCFTR = (CFTR_205043_at__- 595.1975000)/ 405.7689543;
run;

data GSE39582T3;
set GSE39582T3;
ZCFTR = (_205043_at- 9.2281037)/ 1.4876138;
run;

*Since the GSE39582T3 values skew the averagem I wull use the _205043_at in my models and report the CFTR_205043_at__ until I can double check them;

data GSE39582T3;
set GSE39582T3;
ZCFTR = (CFTR_205043_at__- 854.0263914)/ 584.9578002;
run;

data GSE33113T3;
set GSE33113T3;
ZCFTR = (_205043_at- 735.8114583)/538.6032849;
run;


*descriptive stats in preparation of the report;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

data GSE14333T3;
set mylib.GSE14333T3;
run;

proc contents data = GSE14333T3;
run;

data GSE14333T3;
set GSE14333T3;
Stage = _DukesStage;
if _DukesStage = ' A' then cancer_stage = 0;
if _DukesStage = ' B' then cancer_stage = 1;
if _DukesStage = ' C' then cancer_stage = 1;
if Stage = ' A' then cancer_stage = 0;
if Stage = ' B' then cancer_stage = 1;
if Stage = ' C' then cancer_stage = 1;
run;

proc freq data = GSE14333T3;
tables _DukesStage Stage cancer_stage;
run;

proc means data = GSE14333T3 min mean median max std;
var CFTR_205043_at__ _Age_Diag;
run;

proc freq data = GSE14333T3;
tables cancer_stage _Gender _DFS_Cens;
run;

proc univariate NORMAL PLOT data=GSE14333T3; 
var CFTR_205043_at__;
histogram CFTR_205043_at__/normal;
run;

data GSE17538T3;
set mylib.GSE17538T3;
run;

proc contents data = GSE17538T3;
run;

data GSE17538T3;
set GSE17538T3;
if ajcc_stage = 1 then cancer_stage = 0;
if ajcc_stage = 2 then cancer_stage = 1;
if ajcc_stage = 3 then cancer_stage = 1;
if ajcc_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE17538T3;
tables ajcc_stage cancer_stage;
run;

proc means data = GSE17538T3 min mean median max std;
var CFTR_205043_at__ age;
run;

proc freq data = GSE17538T3;
tables gender cancer_stage dfs_event os_event;
run;

proc univariate NORMAL PLOT data=GSE17538T3; 
var CFTR_205043_at__;
histogram CFTR_205043_at__/normal;
run;

data GSE39582T3;
set mylib.GSE39582T3;
run;

proc contents data = GSE39582T3;
run;

data GSE39582T3;
set GSE39582T3;
if tnm_stage = 0 then cancer_stage = 0;
if tnm_stage = 1 then cancer_stage = 0;
if tnm_stage = 2 then cancer_stage = 1;
if tnm_stage = 3 then cancer_stage = 1;
if tnm_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE39582T3;
tables tnm_stage cancer_stage;
run;

proc means data = GSE39582T3 min mean median max std;
var CFTR_205043_at__ Age2;
run;

proc freq data = GSE39582T3;
tables  MMR Sex cancer_stage rfs_event os_event;
run;

proc univariate NORMAL PLOT data=GSE39582T3; 
var CFTR_205043_at__;
histogram CFTR_205043_at__/normal;
run;

data GSE33113T3;
set mylib.GSE33113T3;
run;

proc contents data = GSE33113T3;
run;

proc means data = GSE33113T3 min mean median max std;
var _205043_at age_at_diagnosis;
run;

proc freq data = GSE33113T3;
tables Sex disease_status relapse;
run;

proc univariate NORMAL PLOT data=GSE33113T3; 
var _205043_at;
histogram _205043_at/normal;
run;

data TCGARAW;
set mylib.TCGARAW;
run;

proc contents data = TCGARAW;
run;

proc means data = TCGARAW min mean median max std;
var CFTR;
run;

proc freq data = TCGARAW;
tables OS;
run;

data TCGARFS;
set mylib.TCGARFS;
run;

proc contents data = TCGARFS;
run;

proc means data = TCGARFS min mean median max std;
var CFTR;
run;

proc freq data = TCGARFS;
tables RFS;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Creating Zscore values for all datasets to evaluate their optimal cutpoint individually;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

data GSE14333T3;
set mylib.GSE14333T3;
run;

proc contents data = GSE14333T3;
run;

data GSE14333T3;
set GSE14333T3;
Stage = _DukesStage;
if _DukesStage = ' A' then cancer_stage = 0;
if _DukesStage = ' B' then cancer_stage = 1;
if _DukesStage = ' C' then cancer_stage = 1;
if Stage = ' A' then cancer_stage = 0;
if Stage = ' B' then cancer_stage = 1;
if Stage = ' C' then cancer_stage = 1;
run;

proc freq data = GSE14333T3;
tables _DukesStage Stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE14333T3 mean std;
var CFTR_205043_at__; 
run;

data GSE14333T3;
set GSE14333T3;
ZCFTR = (CFTR_205043_at__- 651.3318584)/ 467.5589204;
run;

proc means data = GSE14333T3 mean std min max;
var CFTR_205043_at__ ZCFTR; 
run;

proc contents data = GSE14333T3;
run;

data GSE14333T3;
set GSE14333T3;
ID = "ID_REF";
DATASET = "GSE14333T3";
if _DFS_Cens = 0 then DFS = 1;
else if _DFS_Cens = 1 then DFS = 0;
DFSTIME = _DFS_Time;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = CFTR_205043_at__;
run;

data GSE14333T3MKI;
set GSE14333T3 (keep = ID DATASET DFS DFSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE14333T3MKI;
run;

proc logistic data = GSE14333T3MKI;
model DFS (event='1') = ZCFTR;
run;

proc logistic data = GSE14333T3MKI;
where CANCERSTAGE = 1;
model DFS (event='1') = ZCFTR;
run;

*Going to use the full dataset (not restrict to cancerstage = 1) since ZCFTR is associated with DFS in that model;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

ods graphics on;
proc logistic data=GSE14333T3MKI;
model DFS (event='1') = ZCFTR / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

*I exported te table to excel to calculate youden's statistic;
*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;

Logit(0.22) = -1.3077 -0.4309 *(X)
X = [Logit(0.22) + (-1.3077)]/(-0.4309);

*use logit calculator https://www.medcalc.org/manual/logit_function.php;

X = -0.097548449;

*Or check (https://communities.sas.com/t5/Statistical-Procedures/95-confidence-intervals-of-Youden-index/td-p/542313) to get youden 95% CI;

*Testing the cutpoints;

data GSE14333T3MKI;
set GSE14333T3MKI;
if ZCFTR LT -0.097548449 then OPTIMAL = 0;
else if ZCFTR GE -0.097548449 then OPTIMAL = 1;
run;

proc logistic data=GSE14333T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFS (event='1') = OPTIMAL;
run;

proc phreg data=GSE14333T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIME*DFS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIME*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE14333T3MKI;
tables DFS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE14333T3;
set GSE14333T3;
if ZCFTR LT -0.097548449 then OPTIMAL = 0;
else if ZCFTR GE -0.097548449 then OPTIMAL = 1;
run;

proc logistic data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage OPTIMAL(ref="1");
model DFS (event='1') = Stage OPTIMAL;
run;

proc phreg data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage OPTIMAL(ref="1");
model DFSTIME*DFS(0) = Stage OPTIMAL/rl;
run;

**********************************************************************************************************************************************************************************;

data GSE17538T3;
set mylib.GSE17538T3;
run;

proc contents data = GSE17538T3;
run;

data GSE17538T3;
set GSE17538T3;
if ajcc_stage = 1 then cancer_stage = 0;
if ajcc_stage = 2 then cancer_stage = 1;
if ajcc_stage = 3 then cancer_stage = 1;
if ajcc_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE17538T3;
tables ajcc_stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE17538T3 mean std;
var CFTR_205043_at__; 
run;

data GSE17538T3;
set GSE17538T3;
ZCFTR = (CFTR_205043_at__- 595.1975000)/ 405.7689543;
run;

proc means data = GSE17538T3 mean std min max;
var CFTR_205043_at__ ZCFTR; 
run;

proc contents data = GSE17538T3;
run;

data GSE17538T3;
set GSE17538T3;
ID = "ID_REF";
DATASET = "GSE17538T3";
if dfs_event = 0 then DFS = 0;
else if dfs_event = 1 then DFS = 1;
DFSTIME = dfs_time;
if os_event = 0 then OS = 0;
else if os_event = 1 then OS = 1;
OSTIME = os_time;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = CFTR_205043_at__; 
run;

data GSE17538T3MKI;
set GSE17538T3 (keep = ID DATASET DFS DFSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE17538T3MKI;
run;

data GSE17538T3MKII;
set GSE17538T3 (keep = ID DATASET OS OSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE17538T3MKII;
run;

proc logistic data = GSE17538T3;
model DFS (event='1') = ZCFTR;
run;

*Going to use the full dataset (not restrict to cancerstage = 1) since ZCFTR is associated with DFS in that model;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

ods graphics on;
proc logistic data=GSE17538T3MKI;
model DFS (event='1') = ZCFTR / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

*I exported te table to excel to calculate youden's statistic;
*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;

Logit(0.24) = -1.2512 -0.2403 *(X)
X = [Logit(0.24) + (-1.2512)]/(-0.2403);

*use logit calculator https://www.medcalc.org/manual/logit_function.php;

X = -0.409989555;

*Or check (https://communities.sas.com/t5/Statistical-Procedures/95-confidence-intervals-of-Youden-index/td-p/542313) to get youden 95% CI;

*Testing the cutpoints;

data GSE17538T3MKI;
set GSE17538T3MKI;
if ZCFTR LT -0.409989555 then OPTIMAL = 0;
else if ZCFTR GE -0.409989555 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFS (event='1') = OPTIMAL;
run;

proc phreg data=GSE17538T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIME*DFS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIME*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE17538T3MKI;
tables DFS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE17538T3;
set GSE17538T3;
if ZCFTR LT -0.409989555 then OPTIMAL = 0;
else if ZCFTR GE -0.409989555 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model DFS (event='1') = ajcc_stage OPTIMAL;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model DFSTIME*DFS(0) = ajcc_stage OPTIMAL/rl;
run;

**********************************************************************************************************************************************************************************;

*Overall Survival;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

ods graphics on;
proc logistic data=GSE17538T3MKII;
model OS (event='1') = ZCFTR / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

*I exported te table to excel to calculate youden's statistic;
*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;

Logit(0.38) = -0.9407 -0.5075 *(X)
X = [Logit(0.38) + (-0.9407)]/(-0.5075);

*use logit calculator https://www.medcalc.org/manual/logit_function.php;

X = -0.888969014;

*Or check (https://communities.sas.com/t5/Statistical-Procedures/95-confidence-intervals-of-Youden-index/td-p/542313) to get youden 95% CI;

*Testing the cutpoints;

data GSE17538T3MKII;
set GSE17538T3MKII;
if ZCFTR LT -0.888969014 then OPTIMAL = 0;
else if ZCFTR GE -0.888969014 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OS (event='1') = OPTIMAL;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIME*OS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE17538T3MKII;
tables DFS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE17538T3;
set GSE17538T3;
if ZCFTR LT -0.888969014 then OPTIMAL = 0;
else if ZCFTR GE -0.888969014 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model OS (event='1') = ajcc_stage OPTIMAL;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model OSTIME*OS(0) = ajcc_stage OPTIMAL/rl;
run;

**********************************************************************************************************************************************************************************;

data GSE39582T3;
set mylib.GSE39582T3;
run;

proc contents data = GSE39582T3;
run;

data GSE39582T3;
set GSE39582T3;
if tnm_stage = 0 then cancer_stage = 0;
if tnm_stage = 1 then cancer_stage = 0;
if tnm_stage = 2 then cancer_stage = 1;
if tnm_stage = 3 then cancer_stage = 1;
if tnm_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE39582T3;
tables tnm_stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE39582T3 mean std;
var _205043_at; 
run;

data GSE39582T3;
set GSE39582T3;
ZCFTR = (_205043_at- 9.2281037)/ 1.4876138;
run;

proc means data = GSE39582T3 mean std min max;
var _205043_at ZCFTR; 
run;

proc contents data = GSE39582T3;
run;

data GSE39582T3;
set GSE39582T3;
ID = "ID_REF";
DATASET = "GSE39582T3";
if rfs_event = 0 then DFS = 0;
else if rfs_event = 1 then DFS = 1;
DFSTIME = rfs_delay;
if os_event = 0 then OS = 0;
else if os_event = 1 then OS = 1;
OSTIME = os_delay;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = _205043_at;
run;

data GSE39582T3MKI;
set GSE39582T3 (keep = ID DATASET DFS DFSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE39582T3MKI;
run;

data GSE39582T3MKII;
set GSE39582T3 (keep = ID DATASET OS OSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE39582T3MKII;
run;

*Disease free survival;

proc logistic data = GSE39582T3MKI;
model DFS (event='1') = ZCFTR;
run;

*Going to use the full dataset (not restrict to cancerstage = 1) since ZCFTR is associated with DFS in that model;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

ods graphics on;
proc logistic data=GSE39582T3MKI;
model DFS (event='1') = ZCFTR / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

*I exported te table to excel to calculate youden's statistic;
*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;

Logit(0.32) = -0.7914 -0.1523 *(X)
X = [Logit(0.32) + (-0.7914)]/(-0.1523);

***Theree were no positive Youden valuyes so I went with the largest absolute value;

*use logit calculator https://www.medcalc.org/manual/logit_function.php;

X = -1.404037225;

*Or check (https://communities.sas.com/t5/Statistical-Procedures/95-confidence-intervals-of-Youden-index/td-p/542313) to get youden 95% CI;

*Testing the cutpoints;

data GSE39582T3MKI;
set GSE39582T3MKI;
if ZCFTR LT -1.404037225 then OPTIMAL = 0;
else if ZCFTR GE -1.404037225 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFS (event='1') = OPTIMAL;
run;

proc phreg data=GSE39582T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIME*DFS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIME*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE39582T3MKI;
tables DFS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE39582T3;
set GSE39582T3;
if ZCFTR LT -1.404037225 then OPTIMAL = 0;
else if ZCFTR GE -1.404037225 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model DFS (event='1') = tnm_stage OPTIMAL;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model DFSTIME*DFS(0) = tnm_stage OPTIMAL/rl;
run;

********************************************************************************************************************************************************************************;

*overall survival;

*Going to use the full dataset (not restrict to cancerstage = 1) since ZCFTR is associated with DFS in that model;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

ods graphics on;
proc logistic data=GSE39582T3MKII;
model OS (event='1') = ZCFTR / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

*I exported te table to excel to calculate youden's statistic;
*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;

Logit(0.34) = -0.6871 -0.1424 *(X)
X = [Logit(0.34) + (-0.6871)]/(-0.1424);

*use logit calculator https://www.medcalc.org/manual/logit_function.php;

X = -0.16717544;

*Or check (https://communities.sas.com/t5/Statistical-Procedures/95-confidence-intervals-of-Youden-index/td-p/542313) to get youden 95% CI;

*Testing the cutpoints;

data GSE39582T3MKII;
set GSE39582T3MKII;
if ZCFTR LT -0.16717544 then OPTIMAL = 0;
else if ZCFTR GE -0.16717544 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OS (event='1') = OPTIMAL;
run;

proc phreg data=GSE39582T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OSTIME*OS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIME*OS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE39582T3MKII;
tables OS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE39582T3;
set GSE39582T3;
if ZCFTR LT -0.16717544 then OPTIMAL = 0;
else if ZCFTR GE -0.16717544 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model OS (event='1') = tnm_stage OPTIMAL;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model OSTIME*OS(0) = tnm_stage OPTIMAL/rl;
run;

**********************************************************************************************************************************************************************************;

data GSE33113T3;
set mylib.GSE33113T3;
run;

proc contents data = GSE33113T3;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE33113T3 mean std;
var _205043_at; 
run;

data GSE33113T3;
set GSE33113T3;
ZCFTR = (_205043_at- 735.8114583)/538.6032849;
run;

proc means data = GSE33113T3 mean std min max;
var _205043_at ZCFTR; 
run;

proc contents data = GSE33113T3;
run;

data GSE33113T3;
set GSE33113T3;
ID = "ID_REF";
DATASET = "GSE33113T3";
if relapse = 0 then DFS = 0;
else if relapse = 1 then DFS = 1;
DFSTIME = time;
CANCERSTAGE = 1;
CFTRLEVEL = _205043_at;
run;

data GSE33113T3MKI;
set GSE33113T3 (keep = ID DATASET DFS DFSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE33113T3MKI;
run;

proc logistic data = GSE33113T3MKI;
model DFS (event='1') = ZCFTR;
run;

*Going to use the full dataset (not restrict to cancerstage = 1) since ZCFTR is associated with DFS in that model;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

ods graphics on;
proc logistic data=GSE33113T3MKI;
model DFS (event='1') = ZCFTR / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

*I exported te table to excel to calculate youden's statistic;
*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;

Logit(0.24) = -1.4686 -0.5817 *(X)
X = [Logit(0.24) + (-1.4686)]/(-0.5817);

*use logit calculator https://www.medcalc.org/manual/logit_function.php;

X = -0.543098659;

*Or check (https://communities.sas.com/t5/Statistical-Procedures/95-confidence-intervals-of-Youden-index/td-p/542313) to get youden 95% CI;

*Testing the cutpoints;

data GSE33113T3MKI;
set GSE33113T3MKI;
if ZCFTR LT -0.543098659 then OPTIMAL = 0;
else if ZCFTR GE -0.543098659 then OPTIMAL = 1;
run;

proc logistic data=GSE33113T3MKI;
class OPTIMAL(ref="1");
model DFS (event='1') = OPTIMAL;
run;

proc phreg data=GSE33113T3MKI;
class OPTIMAL(ref="1");
model DFSTIME*DFS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIME*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE33113T3MKI;
tables DFS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Getting the values for the descriptive tables;

data GSE14333T3;
set GSE14333T3;
ZCFTR = (CFTR_205043_at__- 651.3318584)/ 467.5589204;
run;

data GSE17538T3;
set GSE17538T3;
ZCFTR = (CFTR_205043_at__- 595.1975000)/ 405.7689543;
run;

data GSE39582T3;
set GSE39582T3;
ZCFTR = (_205043_at- 9.2281037)/ 1.4876138;
run;

*Since the GSE39582T3 values skew the averagem I wull use the _205043_at in my models and report the CFTR_205043_at__ until I can double check them;

data GSE39582T3;
set GSE39582T3;
ZCFTR = (CFTR_205043_at__- 854.0263914)/ 584.9578002;
run;

data GSE33113T3;
set GSE33113T3;
ZCFTR = (_205043_at- 735.8114583)/538.6032849;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*descriptive stats in preparation of the report;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

data GSE14333T3;
set mylib.GSE14333T3;
run;

proc contents data = GSE14333T3;
run;

data GSE14333T3;
set GSE14333T3;
Stage = _DukesStage;
if _DukesStage = ' A' then cancer_stage = 0;
if _DukesStage = ' B' then cancer_stage = 1;
if _DukesStage = ' C' then cancer_stage = 1;
if Stage = ' A' then cancer_stage = 0;
if Stage = ' B' then cancer_stage = 1;
if Stage = ' C' then cancer_stage = 1;
run;

proc freq data = GSE14333T3;
tables _DukesStage Stage cancer_stage;
run;

proc means data = GSE14333T3 min mean median max std;
var CFTR_205043_at__ _Age_Diag;
run;

proc freq data = GSE14333T3;
tables cancer_stage _Gender _DFS_Cens;
run;

proc univariate NORMAL PLOT data=GSE14333T3; 
var CFTR_205043_at__;
histogram CFTR_205043_at__/normal;
run;

data GSE17538T3;
set mylib.GSE17538T3;
run;

proc contents data = GSE17538T3;
run;

data GSE17538T3;
set GSE17538T3;
if ajcc_stage = 1 then cancer_stage = 0;
if ajcc_stage = 2 then cancer_stage = 1;
if ajcc_stage = 3 then cancer_stage = 1;
if ajcc_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE17538T3;
tables ajcc_stage cancer_stage;
run;

proc means data = GSE17538T3 min mean median max std;
var CFTR_205043_at__ age;
run;

proc freq data = GSE17538T3;
tables gender cancer_stage dfs_event os_event;
run;

proc univariate NORMAL PLOT data=GSE17538T3; 
var CFTR_205043_at__;
histogram CFTR_205043_at__/normal;
run;

data GSE39582T3;
set mylib.GSE39582T3;
run;

proc contents data = GSE39582T3;
run;

proc means data = GSE39582T3 mean std;
var Age2;
run;

data GSE39582T3;
set GSE39582T3;
if tnm_stage = 0 then cancer_stage = 0;
if tnm_stage = 1 then cancer_stage = 0;
if tnm_stage = 2 then cancer_stage = 1;
if tnm_stage = 3 then cancer_stage = 1;
if tnm_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE39582T3;
tables tnm_stage cancer_stage;
run;

proc means data = GSE39582T3 min mean median max std;
var CFTR_205043_at__ Age2;
run;

proc freq data = GSE39582T3;
tables  MMR Sex cancer_stage rfs_event os_event;
run;

proc univariate NORMAL PLOT data=GSE39582T3; 
var CFTR_205043_at__;
histogram CFTR_205043_at__/normal;
run;

data GSE33113T3;
set mylib.GSE33113T3;
run;

proc contents data = GSE33113T3;
run;

proc means data = GSE33113T3 min mean median max std;
var _205043_at age_at_diagnosis;
run;

proc freq data = GSE33113T3;
tables Sex disease_status relapse;
run;

proc univariate NORMAL PLOT data=GSE33113T3; 
var _205043_at;
histogram _205043_at/normal;
run;

data TCGARAW;
set mylib.TCGARAW;
run;

proc contents data = TCGARAW;
run;

proc means data = TCGARAW min mean median max std;
var CFTR;
run;

proc freq data = TCGARAW;
tables OS;
run;

data TCGARFS;
set mylib.TCGARFS;
run;

proc contents data = TCGARFS;
run;

proc means data = TCGARFS min mean median max std;
var CFTR;
run;

proc freq data = TCGARFS;
tables RFS;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;


***************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

***************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;



***************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

***************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Creating Zscore values for all datasets with CRC (so exclude GSE39582) to combine them for the grant ;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

data GSE14333T3;
set mylib.GSE14333T3;
run;

proc contents data = GSE14333T3;
run;

data GSE14333T3;
set GSE14333T3;
Stage = _DukesStage;
if _DukesStage = ' A' then cancer_stage = 0;
if _DukesStage = ' B' then cancer_stage = 1;
if _DukesStage = ' C' then cancer_stage = 1;
if Stage = ' A' then cancer_stage = 0;
if Stage = ' B' then cancer_stage = 1;
if Stage = ' C' then cancer_stage = 1;
run;

proc freq data = GSE14333T3;
tables _DukesStage Stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE14333T3 mean std;
var CFTR_205043_at__; 
run;

data GSE14333T3;
set GSE14333T3;
ZCFTR = (CFTR_205043_at__- 651.3318584)/ 467.5589204;
run;

proc means data = GSE14333T3 mean std min max;
var CFTR_205043_at__ ZCFTR; 
run;

proc contents data = GSE14333T3;
run;

data GSE14333T3;
set GSE14333T3;
ID = "ID_REF";
DATASET = "GSE14333T3";
if _DFS_Cens = 0 then DFS = 1;
else if _DFS_Cens = 1 then DFS = 0;
DFSTIME = _DFS_Time;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = CFTR_205043_at__;
run;

data GSE14333T3MKI;
set GSE14333T3 (keep = ID DATASET DFS DFSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE14333T3MKI;
run;

data GSE17538T3;
set mylib.GSE17538T3;
run;

proc contents data = GSE17538T3;
run;

data GSE17538T3;
set GSE17538T3;
if ajcc_stage = 1 then cancer_stage = 0;
if ajcc_stage = 2 then cancer_stage = 1;
if ajcc_stage = 3 then cancer_stage = 1;
if ajcc_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE17538T3;
tables ajcc_stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE17538T3 mean std;
var CFTR_205043_at__; 
run;

data GSE17538T3;
set GSE17538T3;
ZCFTR = (CFTR_205043_at__- 595.1975000)/ 405.7689543;
run;

proc means data = GSE17538T3 mean std min max;
var CFTR_205043_at__ ZCFTR; 
run;

proc contents data = GSE17538T3;
run;

data GSE17538T3;
set GSE17538T3;
ID = "ID_REF";
DATASET = "GSE17538T3";
if dfs_event = 0 then DFS = 0;
else if dfs_event = 1 then DFS = 1;
DFSTIME = dfs_time;
if os_event = 0 then OS = 0;
else if os_event = 1 then OS = 1;
OSTIME = os_time;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = CFTR_205043_at__; 
run;

data GSE17538T3MKI;
set GSE17538T3 (keep = ID DATASET DFS DFSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE17538T3MKI;
run;

data GSE17538T3MKII;
set GSE17538T3 (keep = ID DATASET OS OSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE17538T3MKII;
run;

/*
data GSE39582T3;
set mylib.GSE39582T3;
run;

proc contents data = GSE39582T3;
run;

data GSE39582T3;
set GSE39582T3;
if tnm_stage = 0 then cancer_stage = 0;
if tnm_stage = 1 then cancer_stage = 0;
if tnm_stage = 2 then cancer_stage = 1;
if tnm_stage = 3 then cancer_stage = 1;
if tnm_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE39582T3;
tables tnm_stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE39582T3 mean std;
var _205043_at; 
run;

data GSE39582T3;
set GSE39582T3;
ZCFTR = (_205043_at- 9.2281037)/ 1.4876138;
run;

proc means data = GSE39582T3 mean std min max;
var _205043_at ZCFTR; 
run;

proc contents data = GSE39582T3;
run;

data GSE39582T3;
set GSE39582T3;
ID = "ID_REF";
DATASET = "GSE39582T3";
if rfs_event = 0 then DFS = 0;
else if rfs_event = 1 then DFS = 1;
DFSTIME = rfs_delay;
if os_event = 0 then OS = 0;
else if os_event = 1 then OS = 1;
OSTIME = os_delay;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = _205043_at;
run;

data GSE39582T3MKI;
set GSE39582T3 (keep = ID DATASET DFS DFSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE39582T3MKI;
run;

data GSE39582T3MKII;
set GSE39582T3 (keep = ID DATASET OS OSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE39582T3MKII;
run;

*/

data GSE33113T3;
set mylib.GSE33113T3;
run;

proc contents data = GSE33113T3;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE33113T3 mean std;
var _205043_at; 
run;

data GSE33113T3;
set GSE33113T3;
ZCFTR = (_205043_at- 735.8114583)/538.6032849;
run;

proc means data = GSE33113T3 mean std min max;
var _205043_at ZCFTR; 
run;

proc contents data = GSE33113T3;
run;

data GSE33113T3;
set GSE33113T3;
ID = "ID_REF";
DATASET = "GSE33113T3";
if relapse = 0 then DFS = 0;
else if relapse = 1 then DFS = 1;
DFSTIME = time;
CANCERSTAGE = 1;
CFTRLEVEL = _205043_at;
run;

data GSE33113T3MKI;
set GSE33113T3 (keep = ID DATASET DFS DFSTIME CANCERSTAGE CFTRLEVEL ZCFTR);
run;

proc contents data = GSE33113T3MKI;
run;

**********************************************************************************************************************************************************************************;

*Combining the datasets with Disease free survival;

data DFSCFTR;
set GSE14333T3MKI GSE17538T3MKI GSE33113T3MKI;
run;

proc freq data = DFSCFTR;
table DATASET;
run;

proc logistic data = DFSCFTR;
where CANCERSTAGE = 1;
model DFS (ref='0') = ZCFTR;
run;

proc logistic data = DFSCFTR;
where CANCERSTAGE = 1;
model DFS (event='1') = ZCFTR;
run;

proc logistic data = DFSCFTR;
model DFS (ref='0') = ZCFTR;
run;

proc logistic data = DFSCFTR;
model DFS (event='1') = ZCFTR;
run;

*Going to use the full dataset (not restrict to cancerstage = 1) since ZCFTR is associated with DFS in that model;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

ods graphics on;
proc logistic data=DFSCFTR;
model DFS (event='1') = ZCFTR / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

*I exported te table to excel to calculate youden's statistic;
*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;

Logit(0.24) = -1.3070 -0.3752 *(X)
X = [Logit(0.24) + (-1.3070)]/(-0.3752);

*use logit calculator https://www.medcalc.org/manual/logit_function.php;

X = -0.411301946;

*Or check (https://communities.sas.com/t5/Statistical-Procedures/95-confidence-intervals-of-Youden-index/td-p/542313) to get youden 95% CI;

*Testing the cutpoints;

data GSE14333T3MKI;
set GSE14333T3MKI;
if ZCFTR LT -0.411301946 then OPTIMAL = 0;
else if ZCFTR GE -0.411301946 then OPTIMAL = 1;
run;

proc phreg data=GSE14333T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIME*DFS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIME*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE14333T3MKI;
tables DFS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE14333T3;
set GSE14333T3;
if ZCFTR LT -0.402747151 then OPTIMAL = 0;
else if ZCFTR GE -0.402747151 then OPTIMAL = 1;
run;

proc phreg data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage OPTIMAL(ref="1");
model DFSTIME*DFS(0) = Stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIME*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

**********************************************************************************************************************************************************************************;

data GSE17538T3MKI;
set GSE17538T3MKI;
if ZCFTR LT -0.411301946 then OPTIMAL = 0;
else if ZCFTR GE -0.411301946 then OPTIMAL = 1;
run;

proc phreg data=GSE17538T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIME*DFS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIME*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE17538T3MKI;
tables DFS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE17538T3;
set GSE17538T3;
if ZCFTR LT -0.411301946 then OPTIMAL = 0;
else if ZCFTR GE -0.411301946 then OPTIMAL = 1;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model DFSTIME*DFS(0) = ajcc_stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIME*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

**********************************************************************************************************************************************************************************;

/*data GSE39582T3MKI;
set GSE39582T3MKI;
if ZCFTR LT -0.402747151 then OPTIMAL = 0;
else if ZCFTR GE -0.402747151 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFS (event='1') = OPTIMAL;
run;

proc phreg data=GSE39582T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIME*DFS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIME*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE39582T3MKI;
tables DFS*OPTIMAL;
run;

*/

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE39582T3;
set GSE39582T3;
if ZCFTR LT -0.411301946 then OPTIMAL = 0;
else if ZCFTR GE -0.411301946 then OPTIMAL = 1;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model DFSTIME*DFS(0) = tnm_stage OPTIMAL/rl;
run;

**********************************************************************************************************************************************************************************;

data GSE33113T3MKI;
set GSE33113T3MKI;
if ZCFTR LT -0.411301946 then OPTIMAL = 0;
else if ZCFTR GE -0.411301946 then OPTIMAL = 1;
run;

proc phreg data=GSE33113T3MKI;
class OPTIMAL(ref="1");
model DFSTIME*DFS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIME*DFS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE33113T3MKI;
tables DFS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

/* Don't need to do that for overall survival since there is only one dataset with CRC and one with colon cancer

*Combining the datasets with overall survival;

data OSCFTR;
set GSE17538T3MKII GSE39582T3MKII;
run;

proc freq data = OSCFTR;
table DATASET;
run;

proc logistic data = OSCFTR;
where CANCERSTAGE = 1;
model OS (ref='0') = ZCFTR;
run;

proc logistic data = OSCFTR;
where CANCERSTAGE = 1;
model OS (event='1') = ZCFTR;
run;

proc logistic data = OSCFTR;
model OS (ref='0') = ZCFTR;
run;

proc logistic data = OSCFTR;
model OS (event='1') = ZCFTR;
run;

*Going to use the full dataset (not restrict to cancerstage = 1) since ZCFTR sis associated with OS in that model;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

ods graphics on;
proc logistic data=OSCFTR;
model OS (event='1') = ZCFTR / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

*I exported te table to excel to calculate youden's statistic;
*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;

Logit(0.28) = -0.7351 -0.2076 *(X)
X = [Logit(0.28) + (-0.7351)]/(-0.2076);

*use logit calculator https://www.medcalc.org/manual/logit_function.php;

X = -0.345885273;

*Or check (https://communities.sas.com/t5/Statistical-Procedures/95-confidence-intervals-of-Youden-index/td-p/542313) to get youden 95% CI;

*Testing the cutpoints;

data GSE17538T3MKII;
set GSE17538T3MKII;
if ZCFTR LT -0.345885273 then OPTIMAL = 0;
else if ZCFTR GE -0.345885273 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OS (event='1') = OPTIMAL;
run;

proc phreg data=GSE17538T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OSTIME*OS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIME*OS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE17538T3MKII;
tables OS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE17538T3;
set GSE17538T3;
if ZCFTR LT -0.345885273 then OPTIMAL = 0;
else if ZCFTR GE -0.345885273 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model OS (event='1') = ajcc_stage OPTIMAL;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model OSTIME*OS(0) = ajcc_stage OPTIMAL/rl;
run;

**********************************************************************************************************************************************************************************;

data GSE39582T3MKII;
set GSE39582T3MKII;
if ZCFTR LT -0.345885273 then OPTIMAL = 0;
else if ZCFTR GE -0.345885273 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OS (event='1') = OPTIMAL;
run;

proc phreg data=GSE39582T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OSTIME*OS(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIME*OS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE39582T3MKII;
tables OS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE39582T3;
set GSE39582T3;
if ZCFTR LT -0.345885273 then OPTIMAL = 0;
else if ZCFTR GE -0.345885273 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model OS (event='1') = tnm_stage OPTIMAL;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1;
class tnm_stage OPTIMAL(ref="1");
model OSTIME*OS(0) = tnm_stage OPTIMAL/rl;
run;

*/


**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Overall Survival;

*Check (http://support.sas.com/kb/25/018.html) and (https://www.listendata.com/2015/03/sas-calculating-optimal-predicted.html) to calculate youden's statistic;

ods graphics on;
proc logistic data=GSE17538T3MKII;
model OS (event='1') = ZCFTR / CTABLE  outroc=troc; roc; roccontrast; 
run;
ods graphics off;

*I exported te table to excel to calculate youden's statistic;
*Based on (https://communities.sas.com/t5/Statistical-Procedures/ROC-in-SAS-obtaining-a-cut-off-value/td-p/161354) next i do;
*logit (Y) = intercept + beta * X;

Logit(0.38) = -0.9407 -0.5075 *(X)
X = [Logit(0.38) + (-0.9407)]/(-0.5075);

*use logit calculator https://www.medcalc.org/manual/logit_function.php;

X = -0.888969014;

*Or check (https://communities.sas.com/t5/Statistical-Procedures/95-confidence-intervals-of-Youden-index/td-p/542313) to get youden 95% CI;

*Testing the cutpoints;

data GSE17538T3MKII;
set GSE17538T3MKII;
if ZCFTR LT -0.888969014 then OPTIMAL = 0;
else if ZCFTR GE -0.888969014 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3MKII;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model OS (event='1') = OPTIMAL;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIME*OS(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE17538T3MKII;
tables DFS*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE17538T3;
set GSE17538T3;
if ZCFTR LT -0.888969014 then OPTIMAL = 0;
else if ZCFTR GE -0.888969014 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model OS (event='1') = ajcc_stage OPTIMAL;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model OSTIME*OS(0) = ajcc_stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIME*OS(0);
strata OPTIMAL;
run;
ODS Graphics off;

*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
*****************************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

*Creating Zscore values for all datasets to combine them;
*In this version, I will also create a zscore based on the log2 scale values: In the initial analysis during the summer 2019, the 
TCGA data was on the log2 scale, while the R2 GSE data was non transformed and the GSE direct download was on the log2 scale.
This makes sence since most GSE data are normalized in the affy package (in R) which normalize data on the log2 scale.
Luckily I saved both the raw value (which was the untransformed R2 values) and the log2 scale values for each of the datasets);

options ps = 3000 ls = 120 nofmterr;
libname mylib 'H:\Professional Folder\WBOB\CFTR Project\Datasets';

data GSE14333T3;
set mylib.GSE14333T3;
run;

proc contents data = GSE14333T3;
run;

data GSE14333T3;
set GSE14333T3;
Stage = _DukesStage;
if _DukesStage = ' A' then cancer_stage = 0;
if _DukesStage = ' B' then cancer_stage = 1;
if _DukesStage = ' C' then cancer_stage = 1;
if Stage = ' A' then cancer_stage = 0;
if Stage = ' B' then cancer_stage = 1;
if Stage = ' C' then cancer_stage = 1;
run;

proc freq data = GSE14333T3;
tables _DukesStage Stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE14333T3 mean std;
var CFTR_205043_at__ _205043_at; 
run;

data GSE14333T3;
set GSE14333T3;
ZCFTR = (CFTR_205043_at__- 651.3318584)/ 467.5589204;
ZCFTR2 = (_205043_at- 9.1535852)/ 1.8360383;
run;

proc means data = GSE14333T3 mean std min max;
var CFTR_205043_at__ ZCFTR ZCFTR2; 
run;

proc contents data = GSE14333T3;
run;

data GSE14333T3;
set GSE14333T3;
ID = "ID_REF";
DATASET = "GSE14333T3";
if _DFS_Cens = 0 then DFS = 1;
else if _DFS_Cens = 1 then DFS = 0;
DFSTIME = _DFS_Time;
DFSTIMEMONTH = _DFS_Time;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = CFTR_205043_at__;
CFTRLEVELLOG = _205043_at;
run;

data GSE14333T3MKI;
set GSE14333T3 (keep = ID DATASET DFS DFSTIME DFSTIMEMONTH CANCERSTAGE CFTRLEVEL ZCFTR CFTRLEVELLOG ZCFTR2);
run;

proc contents data = GSE14333T3MKI;
run;

**********************************************************************************************************************************************************************************;

*Now running the analyses using the cutpoints from the TCGA CFTR analyses;

*GSE14333 5 year;

data GSE14333T3MKI;
set GSE14333T3MKI;
if DFSTIMEMONTH gt 60 and DFS = 1 then DFS_5 = 0;
else DFS_5 = DFS;
run;

proc freq data = GSE14333T3MKI;
table DFS_5*DFS;
run;

data GSE14333T3MKI;
set GSE14333T3MKI;
if ZCFTR2 LT  0.360475514 then OPTIMAL = 0;
else if ZCFTR2 GE  0.360475514 then OPTIMAL = 1;
run;

proc logistic data=GSE14333T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFS_5 (event='1') = OPTIMAL;
run;

proc phreg data=GSE14333T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS_5(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS_5(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE14333T3MKI;
tables DFS_5*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE14333T3;
set GSE14333T3;
if DFSTIMEMONTH gt 60 and DFS = 1 then DFS_5 = 0;
else DFS_5 = DFS;
run;

data GSE14333T3;
set GSE14333T3;
if ZCFTR2 LT  0.360475514 then OPTIMAL = 0;
else if ZCFTR2 GE  0.360475514 then OPTIMAL = 1;
run;

proc logistic data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage OPTIMAL(ref="1");
model DFS_5 (event='1') = Stage OPTIMAL;
run;

proc phreg data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS_5(0) = Stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS_5(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc contents data=GSE14333T3;
run;

************************************************************************************************************************************************************;

*GSE14333 Full survival;

data GSE14333T3MKI;
set GSE14333T3MKI;
if ZCFTR2 LT  -0.248402812 then OPTIMAL_Full = 0;
else if ZCFTR2 GE  -0.248402812 then OPTIMAL_Full = 1;
run;

proc logistic data=GSE14333T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL_Full(ref="1");
model DFS(event='1') = OPTIMAL_Full;
run;

proc phreg data=GSE14333T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL_Full(ref="1");
model DFSTIMEMONTH*DFS(0) = OPTIMAL_Full/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL_Full;
run;
ODS Graphics off;

proc freq data = GSE14333T3MKI;
tables DFS*OPTIMAL_Full;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE14333T3;
set GSE14333T3;
if ZCFTR2 LT  -0.248402812 then OPTIMAL_Full = 0;
else if ZCFTR2 GE  -0.248402812 then OPTIMAL_Full = 1;
run;

proc logistic data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage OPTIMAL_Full(ref="1");
model DFS (event='1') = Stage OPTIMAL_Full;
run;

proc phreg data=GSE14333T3;
where CANCERSTAGE = 1;
class Stage OPTIMAL_Full(ref="1");
model DFSTIMEMONTH*DFS(0) = Stage OPTIMAL_Full/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL_Full;
run;
ODS Graphics off;

proc contents data=GSE14333T3;
run;

************************************************************************************************************************************************************;
************************************************************************************************************************************************************;
************************************************************************************************************************************************************;

*GSE17538T3;

data GSE17538T3;
set mylib.GSE17538T3;
run;

proc contents data = GSE17538T3;
run;

data GSE17538T3;
set GSE17538T3;
if ajcc_stage = 1 then cancer_stage = 0;
if ajcc_stage = 2 then cancer_stage = 1;
if ajcc_stage = 3 then cancer_stage = 1;
if ajcc_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE17538T3;
tables ajcc_stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE17538T3 mean std;
var CFTR_205043_at__ _205043_at; 
run;

data GSE17538T3;
set GSE17538T3;
ZCFTR = (CFTR_205043_at__- 595.1975000)/ 405.7689543;
ZCFTR2 = (_205043_at- 9.9299050)/ 1.1211490;
run;

proc means data = GSE17538T3 mean std min max;
var CFTR_205043_at__ _205043_at ZCFTR ZCFTR2; 
run;

proc contents data = GSE17538T3;
run;

data GSE17538T3;
set GSE17538T3;
ID = "ID_REF";
DATASET = "GSE17538T3";
if dfs_event = 0 then DFS = 0;
else if dfs_event = 1 then DFS = 1;
DFSTIME = dfs_time;
DFSTIMEMONTH = dfs_time;
if os_event = 0 then OS = 0;
else if os_event = 1 then OS = 1;
OSTIME = os_time;
OSTIMEMONTH = os_time;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = CFTR_205043_at__; 
CFTRLEVELLOG = _205043_at; 
run;

data GSE17538T3MKI;
set GSE17538T3 (keep = ID DATASET DFS DFSTIME DFSTIMEMONTH CANCERSTAGE CFTRLEVEL CFTRLEVELLOG ZCFTR ZCFTR2);
run;

proc contents data = GSE17538T3MKI;
run;

data GSE17538T3MKII;
set GSE17538T3 (keep = ID DATASET OS OSTIME OSTIMEMONTH CANCERSTAGE CFTRLEVEL CFTRLEVELLOG ZCFTR ZCFTR2);
run;

proc contents data = GSE17538T3MKII;
run;

**********************************************************************************************************************************************************************************;

*GSE17538 DFS 5 year survival;

data GSE17538T3MKI;
set GSE17538T3MKI;
if DFSTIMEMONTH gt 60 and DFS = 1 then DFS_5 = 0;
else DFS_5 = DFS;
run;

proc freq data = GSE17538T3MKI;
tables DFS*DFS_5;
run;

data GSE17538T3MKI;
set GSE17538T3MKI;
if ZCFTR2 LT  0.360475514 then OPTIMAL = 0;
else if ZCFTR2 GE  0.360475514 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFS_5 (event='1') = OPTIMAL;
run;

proc phreg data=GSE17538T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS_5(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS_5(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE17538T3MKI;
tables DFS_5*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE17538T3;
set GSE17538T3;
if DFSTIMEMONTH gt 60 and DFS = 1 then DFS_5 = 0;
else DFS_5 = DFS;
run;

proc freq data = GSE17538T3;
tables DFS*DFS_5;
run;

data GSE17538T3;
set GSE17538T3;
if ZCFTR2 LT  0.360475514 then OPTIMAL = 0;
else if ZCFTR2 GE  0.360475514 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model DFS_5 (event='1') = ajcc_stage OPTIMAL;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS_5(0) = ajcc_stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS_5(0);
strata OPTIMAL;
run;
ODS Graphics off;

************************************************************************************************************************************************************;

*GSE17538 DFS full survival;

data GSE17538T3MKI;
set GSE17538T3MKI;
if ZCFTR2 LT  -0.248402812 then OPTIMAL_Full = 0;
else if ZCFTR2 GE  -0.248402812 then OPTIMAL_Full = 1;
run;

proc logistic data=GSE17538T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL_Full(ref="1");
model DFS(event='1') = OPTIMAL_Full;
run;

proc phreg data=GSE17538T3MKI;
where CANCERSTAGE = 1;
class OPTIMAL_Full(ref="1");
model DFSTIMEMONTH*DFS(0) = OPTIMAL_Full/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL_Full;
run;
ODS Graphics off;

proc freq data = GSE17538T3MKI;
tables DFS*OPTIMAL_Full;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE17538T3;
set GSE17538T3;
if ZCFTR2 LT  -0.248402812 then OPTIMAL_Full = 0;
else if ZCFTR2 GE  -0.248402812 then OPTIMAL_Full = 1;
run;

proc logistic data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL_Full(ref="1");
model DFS (event='1') = ajcc_stage OPTIMAL_Full;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL_Full(ref="1");
model DFSTIMEMONTH*DFS(0) = ajcc_stage OPTIMAL_Full/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL_Full;
run;
ODS Graphics off;

**********************************************************************************************************************************************************************************;
*GSE17538 OS 5 year survival;

data GSE17538T3MKII;
set GSE17538T3MKII;
if OSTIMEMONTH gt 60 and OS = 1 then OS_5 = 0;
else OS_5 = OS;
run;

proc freq data = GSE17538T3MKII;
tables OS*OS_5;
run;

data GSE17538T3MKII;
set GSE17538T3MKII;
if ZCFTR2 LT  0.360475514 then OPTIMAL = 0;
else if ZCFTR2 GE  0.360475514 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3MKII;
class OPTIMAL(ref="1");
model OS_5 (event='1') = OPTIMAL;
run;

proc phreg data=GSE17538T3MKII;
class OPTIMAL(ref="1");
model OSTIMEMONTH*OS_5(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIMEMONTH*OS_5(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE17538T3MKII;
tables OS_5*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE17538T3;
set GSE17538T3;
if OSTIMEMONTH gt 60 and OS = 1 then OS_5 = 0;
else OS_5 = OS;
run;

proc freq data = GSE17538T3;
tables OS*OS_5;
run;

data GSE17538T3;
set GSE17538T3;
if ZCFTR2 LT  0.360475514 then OPTIMAL = 0;
else if ZCFTR2 GE  0.360475514 then OPTIMAL = 1;
run;

proc logistic data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model OS (event='1') = ajcc_stage OPTIMAL;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL(ref="1");
model OSTIMEMONTH*OS(0) = ajcc_stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIMEMONTH*OS(0);
strata OPTIMAL;
run;
ODS Graphics off;

************************************************************************************************************************************************************;

*GSE17538 OS full survival;

data GSE17538T3MKII;
set GSE17538T3MKII;
if ZCFTR2 LT  -0.248402812 then OPTIMAL_Full = 0;
else if ZCFTR2 GE  -0.248402812 then OPTIMAL_Full = 1;
run;

proc logistic data=GSE17538T3MKII;
class OPTIMAL_Full(ref="1");
model OS(event='1') = OPTIMAL_Full;
run;

proc phreg data=GSE17538T3MKII;
class OPTIMAL_Full(ref="1");
model OSTIMEMONTH*OS(0) = OPTIMAL_Full/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIMEMONTH*OS(0);
strata OPTIMAL_Full;
run;
ODS Graphics off;

proc freq data = GSE17538T3MKII;
tables OS*OPTIMAL_Full;
run;

**********************************************************************************************************************************************************************************;

*Further adjusting the model for stage after restricting to advanced cancer stage;

data GSE17538T3;
set GSE17538T3;
if ZCFTR2 LT  -0.248402812 then OPTIMAL_Full = 0;
else if ZCFTR2 GE  -0.248402812 then OPTIMAL_Full = 1;
run;

proc logistic data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL_Full(ref="1");
model OS (event='1') = ajcc_stage OPTIMAL_Full;
run;

proc phreg data=GSE17538T3;
where CANCERSTAGE = 1;
class ajcc_stage OPTIMAL_Full(ref="1");
model OSTIMEMONTH*OS(0) = ajcc_stage OPTIMAL_Full/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIMEMONTH*OS(0);
strata OPTIMAL_Full;
run;
ODS Graphics off;

**************************************************************************************************************************************;

*GSE39582;

data GSE39582T3;
set mylib.GSE39582T3;
run;

proc contents data = GSE39582T3;
run;

data GSE39582T3;
set GSE39582T3;
if tnm_stage = 0 then cancer_stage = 0;
if tnm_stage = 1 then cancer_stage = 0;
if tnm_stage = 2 then cancer_stage = 1;
if tnm_stage = 3 then cancer_stage = 1;
if tnm_stage = 4 then cancer_stage = 1;
run;

proc freq data = GSE39582T3;
tables tnm_stage cancer_stage;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE39582T3 mean std;
var _205043_at; 
run;

data GSE39582T3;
set GSE39582T3;
ZCFTR = (_205043_at- 9.2281037)/ 1.4876138;
ZCFTR2 = (_205043_at- 9.2281037)/ 1.4876138;
run;

proc means data = GSE39582T3 mean std min max;
var _205043_at ZCFTR; 
run;

proc contents data = GSE39582T3;
run;

data GSE39582T3;
set GSE39582T3;
ID = "ID_REF";
DATASET = "GSE39582T3";
if rfs_event = 0 then DFS = 0;
else if rfs_event = 1 then DFS = 1;
DFSTIME = rfs_delay;
DFSTIMEMONTH = rfs_delay;
if os_event = 0 then OS = 0;
else if os_event = 1 then OS = 1;
OSTIME = os_delay;
OSTIMEMONTH = os_delay;
CANCERSTAGE = cancer_stage;
CFTRLEVEL = _205043_at;
CFTRLEVELLOG = _205043_at;
run;

data GSE39582T3MKI;
set GSE39582T3 (keep = ID DATASET DFS DFSTIME DFSTIMEMONTH CANCERSTAGE CFTRLEVEL CFTRLEVELLOG ZCFTR ZCFTR2);
run;

proc contents data = GSE39582T3MKI;
run;

data GSE39582T3MKII;
set GSE39582T3 (keep = ID DATASET OS OSTIME OSTIMEMONTH CANCERSTAGE CFTRLEVEL CFTRLEVELLOG ZCFTR ZCFTR2);
run;

proc contents data = GSE39582T3MKII;
run;

***************************************************************************************************************************************************************;

*GSE39582 DFS 5 year survival, also adjusted for cancer stage;

data GSE39582T3;
set GSE39582T3;
if DFSTIMEMONTH gt 60 and DFS = 1 then DFS_5 = 0;
else DFS_5 = DFS;
run;

proc freq data = GSE39582T3;
table DFS*DFS_5;
run;

data GSE39582T3;
set GSE39582T3;
if ZCFTR2 LT 0.360475514 then OPTIMAL = 0;
else if ZCFTR2 GE 0.360475514 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3;
where CANCERSTAGE = 1 and MMR = 2;
class tnm_stage OPTIMAL(ref="1");
model DFS_5 (event='1') = tnm_stage OPTIMAL;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1 and MMR = 2;
class tnm_stage OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS_5(0) = tnm_stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS_5(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE39582T3;
tables DFS_5*OPTIMAL;
run;

***************************************************************************************************************************************************************;

*GSE39582 DFS Full survival, also adjusted for cancer stage;

data GSE39582T3;
set GSE39582T3;
if ZCFTR2 LT -0.248402812 then OPTIMAL_Full = 0;
else if ZCFTR2 GE -0.248402812 then OPTIMAL_Full = 1;
run;

proc logistic data=GSE39582T3;
where CANCERSTAGE = 1 and MMR = 2;
class tnm_stage OPTIMAL_Full(ref="1");
model DFS(event='1') = tnm_stage OPTIMAL_Full;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1 and MMR = 2;
class tnm_stage OPTIMAL_Full(ref="1");
model DFSTIMEMONTH*DFS(0) = tnm_stage OPTIMAL_Full/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL_Full;
run;
ODS Graphics off;

proc freq data = GSE39582T3;
tables DFS*OPTIMAL_Full;
run;

***************************************************************************************************************************************************************;

*GSE39582 OS 5 year survival, also adjusted for cancer stage;

data GSE39582T3;
set GSE39582T3;
if OSTIMEMONTH gt 60 and OS = 1 then OS_5 = 0;
else OS_5 = OS;
run;

proc freq data = GSE39582T3;
table OS*OS_5;
run;

data GSE39582T3;
set GSE39582T3;
if ZCFTR2 LT 0.360475514 then OPTIMAL = 0;
else if ZCFTR2 GE 0.360475514 then OPTIMAL = 1;
run;

proc logistic data=GSE39582T3;
where CANCERSTAGE = 1 and MMR = 2;
class tnm_stage OPTIMAL(ref="1");
model OS_5 (event='1') = tnm_stage OPTIMAL;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1 and MMR = 2;
class tnm_stage OPTIMAL(ref="1");
model OSTIMEMONTH*OS_5(0) = tnm_stage OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIMEMONTH*OS_5(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE39582T3;
tables OS_5*OPTIMAL;
run;

***************************************************************************************************************************************************************;

*GSE39582 OS Full survival, also adjusted for cancer stage;

data GSE39582T3;
set GSE39582T3;
if ZCFTR2 LT -0.248402812 then OPTIMAL_Full = 0;
else if ZCFTR2 GE -0.248402812 then OPTIMAL_Full = 1;
run;

proc logistic data=GSE39582T3;
where CANCERSTAGE = 1 and MMR = 2;
class tnm_stage OPTIMAL_Full(ref="1");
model OS(event='1') = tnm_stage OPTIMAL_Full;
run;

proc phreg data=GSE39582T3;
where CANCERSTAGE = 1 and MMR = 2;
class tnm_stage OPTIMAL_Full(ref="1");
model OSTIMEMONTH*OS(0) = tnm_stage OPTIMAL_Full/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time OSTIMEMONTH*OS(0);
strata OPTIMAL_Full;
run;
ODS Graphics off;

proc freq data = GSE39582T3;
tables OS*OPTIMAL_Full;
run;

***************************************************************************************************************************************************************;

data GSE33113T3;
set mylib.GSE33113T3;
run;

proc contents data = GSE33113T3;
run;

*Z-standardization of the CFTR variable;
proc means data = GSE33113T3 mean std;
var _205043_at; 
run;

*Since the original GSE33113 did not have values normalized, I will need to log transform them manually;
data GSE33113T3;
set GSE33113T3;
CFTRLOG = LOG2(_205043_at);
run;

data GSE33113T3;
set GSE33113T3;
ZCFTR = (_205043_at- 735.8114583)/538.6032849;
ZCFTR2 = (CFTRLOG - 8.8227871)/ 1.9920439;
run;

proc means data = GSE33113T3 mean std min max;
var _205043_at CFTRLOG ZCFTR ZCFTR2 time; 
run;

proc contents data = GSE33113T3;
run;

data GSE33113T3;
set GSE33113T3;
ID = "ID_REF";
DATASET = "GSE33113T3";
if relapse = 0 then DFS = 0;
else if relapse = 1 then DFS = 1;
DFSTIME = time;
DFSTIMEMONTH = time/30.5;
CANCERSTAGE = 1;
CFTRLEVEL = _205043_at;
CFTRLEVELLOG = CFTRLOG;
run;

data GSE33113T3MKI;
set GSE33113T3 (keep = ID DATASET DFS DFSTIME DFSTIMEMONTH CANCERSTAGE CFTRLEVEL CFTRLEVELLOG ZCFTR ZCFTR2);
run;

proc contents data = GSE33113T3MKI;
run;

**********************************************************************************************************************************************************************************;

*GSE33113 DFS 5 year survival;

data GSE33113T3MKI;
set GSE33113T3MKI;
if DFSTIMEMONTH gt 60 and DFS = 1 then DFS_5 = 0;
else DFS_5 = DFS;
run;

proc freq data = GSE33113T3MKI;
table DFS*DFS_5;
run;

data GSE33113T3MKI;
set GSE33113T3MKI;
if ZCFTR2 LT 0.360475514 then OPTIMAL = 0;
else if ZCFTR2 GE 0.360475514 then OPTIMAL = 1;
run;

proc logistic data=GSE33113T3MKI;
class OPTIMAL(ref="1");
model DFS_5 (event='1') = OPTIMAL;
run;

proc phreg data=GSE33113T3MKI;
class OPTIMAL(ref="1");
model DFSTIMEMONTH*DFS_5(0) = OPTIMAL/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS_5(0);
strata OPTIMAL;
run;
ODS Graphics off;

proc freq data = GSE33113T3MKI;
tables DFS_5*OPTIMAL;
run;

**********************************************************************************************************************************************************************************;

*GSE33113 DFS Full survival;

data GSE33113T3MKI;
set GSE33113T3MKI;
if ZCFTR2 LT -0.248402812 then OPTIMAL_Full = 0;
else if ZCFTR2 GE -0.248402812 then OPTIMAL_Full = 1;
run;

proc logistic data=GSE33113T3MKI;
class OPTIMAL_Full(ref="1");
model DFS (event='1') = OPTIMAL_Full;
run;

proc phreg data=GSE33113T3MKI;
class OPTIMAL_Full(ref="1");
model DFSTIMEMONTH*DFS(0) = OPTIMAL_Full/rl;
run;

ODS Graphics on;
PROC LIFETEST GRAPHICS NOTABLE PLOTS=S (NOCENSOR ATRISK=0 to 120 by 10);	
time DFSTIMEMONTH*DFS(0);
strata OPTIMAL_Full;
run;
ODS Graphics off;

proc freq data = GSE33113T3MKI;
tables DFS*OPTIMAL_Full;
run;

**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;
**********************************************************************************************************************************************************************************;

