*SAS Code for the statistical analysis of the Magnesium and CRC manuscript;

*Part 1: Preparing the analysis datasset;

***********************************************************************************************************************************************************************************************************************

*Repeating the analysis with updated CRC data;

options ps = 3000 ls = 120 nofmterr;
options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '           ';
libname mylib 'C:\Users\onyea005\Desktop\WBOB\Magnesium\Codes_Datasets';

Data mg1;
set mylib.crp;
Run;

Data mg2;
set mylib.mg2;
Run;

proc contents data=mg1;
Run;

proc contents data=mg2;
run;

data mg2;
set mg2;
label
tpyrsCRC = "differential person-years for colorectal cancer: follow up time for CRC risk"
tpyrsany = "same person-years for all cancers"
CRC_1= "first colorectal cancer"
CRC = "Colorectal cancer Status";
run;

data mg2;
set mg2;
if magnb1= . then delete;
run;

data mag;
merge mg1 mg2 (in=a);
by id;
if a;

if educat=1 then edu=0;
if educat=2 then edu=0;
if educat=3 then edu=1;
logcrp=log (crp_s);
if bmi01<30 then bmicat=0;
if bmi01 GE 30 then bmicat=1;
if bmi01<25 then bmicat3=0;
if 25 le bmi01 < 30 then bmicat3=1;
if bmi01 ge 30 then bmicat3=2;
if include1='YES';

*Checking the inclusion criteria for the analysis;
proc freq data = mag;
 table include1;
 run;

 *running some descriptive statistics on exposure and outcomes;
proc means data = mag;
var tpyrsany magnf1 magnb1 magnf3 magnb2 v2date21 v3date31 v1date01 crc_1 date2 ;
run;

*more descriptive statistics on the outcome;
proc freq data = mag;
tables crc_1;
run;

*Making our main exposure, dietary magnesium into a categorical variable;
proc rank data=mag out=mag1 groups=4;
var magnf1;
ranks magdieq;
run;

proc means data=mag1;
var magnf1;
class magdieq;
run;

*Combining the three lowest tertiles of magnesium into a single reference level;

data mag1;
set mag1;
if magdieq in (0,1,2) then magdie=0;
else if magdieq=3 then magdie=1;
run;

proc freq data = mag1;
table magdieq magdie;
run;

*Saving your final dataset to rerun the analysis;

data mylib.mag1;
set mag1;
run;

*********************************************************************************************************************************************************************************************

*Rerunning the analysis for the poster with the new, updated 2012 aric cancer cases data;

options ps = 3000 ls = 120 nofmterr;
options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '           ';
libname mylib 'C:\Users\onyea005\Desktop\WBOB\Magnesium\Codes_Datasets\SAS Datasets';
libname mylib2 'C:\Users\onyea005\Desktop\WBOB\ARIC Data\Updated ARIC cases\CANCER';

*Loading the magnesium data and CRC data;

data mag1;
set mylib.mag1;
run;

data CRCdata;
set mylib2.crcdata;
run;

proc sort data = mag1; by ID; run;
proc sort data = crcdata; by ID; run;

data magnesium;
merge mag1 crcdata;
by id;
run;

proc contents data = magnesium;
run;

proc means data = magnesium n min mean median max std;
var magnf1;
run;
 
proc freq data = magnesium;
tables magdie CRC12*magdie;
run;

proc sort data = magnesium; by magdie; run;

proc means data = magnesium;
where CRC12 = 1;
var tpyrsany12;
by magdie;
run;

proc means data = magnesium mean std;
var bmi01 tcal1 v1age01 calcf1 dfib1 alco1  SPRT_I31 logcrp;
by magdie ;
run;

proc ttest data = magnesium;
class magdie;
var bmi01 tcal1 v1age01 calcf1 dfib1 alco1  SPRT_I31 logcrp;
run;

proc freq data = magnesium;
tables gender*magdie CIGT01*magdie ASPIRINCODE01*magdie racegrp*magdie / norow nopercent; 
run;

proc freq data = magnesium;
tables gender*magdie CIGT01*magdie ASPIRINCODE01*magdie racegrp*magdie / norow nopercent chisq; 
run;

proc freq data = magnesium;
tables CRC12*magdie COLONCA12*magdie RECTALCA12*magdie CRC12*gender CRC12*racegrp;
run;

proc freq data = magnesium;
tables gender racegrp;
run;

proc means data = magnesium;
var tpyrsany12;
run;

*************************************************************************************************************************************************************************************

*SAS Code for Statistical Models;

*Main Model;

*Base Model (age, race, gender);
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)=magdie center racegrp v1age01 gender/RL;
Run;

*Proportional hazards assumption (age, race, gender);
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)=magdie center racegrp v1age01 gender phazard /RL;
phazard=magdie*(log(tpyrsany12));
Run;

*Correcting for Proportional hazards assumption (age, race, gender);
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)=magdie center racegrp v1age01 gender ph12_24 /RL;
if 0 lt tpyrsany12 le 12 then time1 = 1; else time1=0;
if tpyrsany12 gt 12 then time2 = 1; else time2 = 0;
ph0_12=magdie*time1;
ph12_24=magdie*time2;
Run;

*Basic Model + physical activity, bmi and total energy use (matches the abstract results from the SAS code I lost);

Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)=magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31/RL;
Run;

Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)=magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 phazard /RL;
phazard=magdie*(log(tpyrsany12));
Run;

*Multivariate Model: further adjusted for alcohol intake, dietary calcium intake,  dietary fiber, CRP levels, aspirin use and cigarette smoking. Dietary magnesium was categorized into quartiles;
*I did not use ever hrt because it dropped over 100 cases from the models;

Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 phazard alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
phazard=magdie*(log(tpyrsany12));
Run;

*Colon cancers only;
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*COLONCA12(0)=magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Rectal cancers only;
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*RECTALCA12(0)=magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Testing the quartile association (PH assumption still severely violated when looking at quartiles);
Proc phreg data=magnesium;
Class magdieq (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdieq tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
Class magdieq (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdieq tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp phazard/RL;
phazard=magdieq*(log(tpyrsany12));
Run;

Proc phreg data=magnesium;
Class magdieq (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdieq tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp ph12_24/RL;
if 0 lt tpyrsany12 le 12 then time1 = 1; else time1=0;
if tpyrsany12 gt 12 then time2 = 1; else time2 = 0;
ph0_12=magdie*time1;
ph12_24=magdie*time2;
Run;

*************************************************************************************************************************************************************************************

*Stratified analyses;
proc freq data = magnesium;
tables gender racegrp;
run;

*Among Men;
Proc phreg data=magnesium;
where gender = 'M';
Class magdie (ref="0" param=ref) center racegrp ;
Model tpyrsany12*CRC12(0)=magdie tcal1 center racegrp v1age01 bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Among Women;
Proc phreg data=magnesium;
where gender = 'F';
Class magdie (ref="0" param=ref) center racegrp ;
Model tpyrsany12*CRC12(0)=magdie tcal1 center racegrp v1age01 bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp ever_hrt1/RL;
Run;

*Among caucasians;
Proc phreg data=magnesium;
where racegrp = 'W';
Class magdie (ref="0" param=ref) center gender;
Model tpyrsany12*CRC12(0)=magdie tcal1 center v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Among African Americans;
Proc phreg data=magnesium;
where racegrp = 'B';
Class magdie (ref="0" param=ref) center gender;
Model tpyrsany12*CRC12(0)=magdie tcal1 center v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*************************************************************************************************************************************************************************************

*Interactions;

*Race;
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp magdie*racegrp/RL;
Run;

*Gender;
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp magdie*gender/RL;
Run;

*CRP;
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp magdie*logcrp/RL;
Run;

*Calcium;
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp magdie*calcf1/RL;
Run;

*Dietary Fiber;
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp magdie*dfib1/RL;
Run;

************************************************************************************************************************************************************************************************************************************************

*Changes based on Dr. Folsom's comments: Present the models with incidence rates instead of using strata in figures 1, 2 and 3 of the poster (aka for the main models and the models stratified for race and gender)

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

*Analysis updates based on Dr. Platz's comments: Test for trend;

proc sort data = magnesium; by magdieq; run;

proc means data = magnesium n mean std;
var magnf1;
by magdieq;
run;

*Testing the quartile association + Trend (PH assumption still severely violated when looking at quartiles);
Proc phreg data=magnesium;
Class magdieq (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdieq tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
contrast "trend in magnesium" magdieq 147 212 272 383;
Run;

Proc phreg data=magnesium;
Class magdieq (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdieq tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp ph12_24/RL;
contrast "trend in magnesium" magdieq 147 212 272 383;
if 0 lt tpyrsany12 le 12 then time1 = 1; else time1=0;
if tpyrsany12 gt 12 then time2 = 1; else time2 = 0;
ph0_12=magdie*time1;
ph12_24=magdie*time2;
Run;

*Dietary Magnesium in Quartiles, Test for Trend accounting for U shape (need to model the quartiles as continuous for that model);
Proc phreg data=magnesium;
Class center racegrp gender;
Model tpyrsany12*CRC12(0)= magdieq  magdieq*magdieq tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
contrast "trend in magnesium" magdieq 147 212 272 383;
Run;

Proc phreg data=magnesium;
Class center racegrp gender;
Model tpyrsany12*CRC12(0)= magnf1 tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
Class center racegrp gender;
Model tpyrsany12*CRC12(0)= magnf1 magnf1*magnf1 tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*****************************************************************************************************************************************************************************************************************************

*minimally adjusted vs. fully adjusted differences;

*Correcting for Proportional hazards assumption (age, race, gender);
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)=magdie center racegrp v1age01 gender ph12_24 /RL;
if 0 lt tpyrsany12 le 12 then time1 = 1; else time1=0;
if tpyrsany12 gt 12 then time2 = 1; else time2 = 0;
ph0_12=magdie*time1;
ph12_24=magdie*time2;
Run;

*Multivariate Model: further adjusted for alcohol intake, dietary calcium intake,  dietary fiber, CRP levels, aspirin use and cigarette smoking. Dietary magnesium was categorized into quartiles;
*I did not use ever hrt here, because it dropped over 100 cases from the models, but it is included for the women only model;

Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp ph12_24/RL;
if 0 lt tpyrsany12 le 12 then time1 = 1; else time1=0;
if tpyrsany12 gt 12 then time2 = 1; else time2 = 0;
ph0_12=magdie*time1;
ph12_24=magdie*time2;
Run;

*Using Type 1 LR ratio test estimate for model fit: 
TYPE1: requests that a Type 1 (sequential) analysis of likelihood ratio test be performed. This consists of sequentially fitting models, beginning with the null model and continuing up to the model specified in the MODEL statement. The likelihood ratio statistic for each successive pair of models is computed and displayed in a table;
Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender / param=effect;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL Type1;
Run;

Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL Type1;
Run;

*log transforming dietary magnesium to test for trend;
data magnesium;
set magnesium;
magln = log (magnf1);
run;

proc univariate data = magnesium;
var magln;
run;

Proc phreg data=magnesium;
Class center racegrp gender;
Model tpyrsany12*CRC12(0)= magln tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
Class center racegrp gender;
Model tpyrsany12*CRC12(0)= magln magln*magln tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
Class center racegrp gender;
Model tpyrsany12*CRC12(0)= magln tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp phazard /RL;
phazard=magln*(log(tpyrsany12));
Run;

Proc phreg data=magnesium;
Class center racegrp gender;
Model tpyrsany12*CRC12(0)= magln tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp ph12_24 /RL;
if 0 lt tpyrsany12 le 12 then time1 = 1; else time1=0;
if tpyrsany12 gt 12 then time2 = 1; else time2 = 0;
ph0_12=magln*time1;
ph12_24=magln*time2;
Run;

*********************************************************************************************************************************************************************************************
*********************************************************************************************************************************************************************************************
*********************************************************************************************************************************************************************************************;

*Rerunning the analysis for the poster with the new, updated 2012 aric cancer cases data;
*This version is to test the various trends using serum magnesium quintiles;

options ps = 3000 ls = 120 nofmterr;
options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '           ';
libname mylib 'C:\Users\onyea005\Desktop\WBOB\Magnesium\Codes_Datasets\SAS Datasets';
libname mylib2 'C:\Users\onyea005\Desktop\WBOB\ARIC Data\Updated ARIC cases\CANCER';

*Loading the magnesium data and CRC data;

data mag1;
set mylib.mag1;
run;

data CRCdata;
set mylib2.crcdata;
run;

proc sort data = mag1; by ID; run;
proc sort data = crcdata; by ID; run;

data magnesium;
merge mag1 crcdata;
by id;
run;

proc contents data = magnesium;
run;

proc means data = magnesium n mean median min p20 p40 p60 p80 max std;
var magnf1 magnb1;
run;

*Trying to rerun the analyses using dietary and serum magnesium quintiles;

data magnesium;
set magnesium;
if magnb1 le 1.40 then smagquint = 0;
else if 1.40 lt magnb1 le 1.50 then smagquint = 1;
else if 1.50 lt magnb1 le 1.60 then smagquint = 2;
else if 1.60 lt magnb1 le 1.70 then smagquint = 3;
else if magnb1 gt 1.70 then smagquint = 4;
if magnf1 lt 172.50 then dmagquint = 0;
else if 172.50 le magnf1 lt 217.85 then dmagquint = 1;
else if 217.85 le magnf1 lt 264.11 then dmagquint = 2;
else if 264.11 le magnf1 lt 325.84 then dmagquint = 3;
else if magnf1 gt 325.84 then dmagquint = 4;
run;

*There are no observations between 1.6 and 1.7, so quintile # 3 for serum magnesium would not be created; 
*I will use the cutoff ponts as suggested in Dr. Lutsey's paper; 

proc univariate data = magnesium;
var magnb1;
histogram;
run;

proc rank data=magnesium groups=5 out=magnesium;
var magnb1 magnf1; 
ranks smagquintv2 dmagquintv2;
run;

proc freq data = magnesium;
tables smagquint dmagquint smagquintv2 dmagquintv2;
run;

proc print data = magnesium (obs = 500);
where 1.50 le magnb1 lt 1.60;
var magnb1 magnf1;
run;

proc print data = magnesium (obs = 500);
where 1.60 le magnb1 lt 1.70;
var magnb1 magnf1;
run;

proc print data = magnesium (obs = 500);
where 1.70 le magnb1 lt 1.80;
var magnb1 magnf1;
run;

*Highest serum quintile;
Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1/RL;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp DIABTS03/RL;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= smagquint center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp DIABTS03/RL;
Run;

Proc phreg data=magnesium;
where gender = "M";
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp DIABTS03/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "W";
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp DIABTS03/RL;
Run;

*Middle serum quintile;
Proc phreg data=magnesium;
Class smagquint (ref= "2") center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1/RL;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="2" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp DIABTS03/RL;
Run;

*Highest dietary quintile;
Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1/RL;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp DIABTS03/RL;
Run;

*Middle dietary quintile;
Proc phreg data=magnesium;
Class dmagquint (ref="2" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1/RL;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="2" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp DIABTS03/RL;
Run;

******************************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************************;

*Rerunning the analysis for the poster with the new, updated 2012 aric cancer cases data;
*This version has the analysis reports to show the differences betwenn the resultsfrom the 2006 and 2012 datasets;

options ps = 3000 ls = 120 nofmterr;
options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '           ';
libname mylib 'C:\Users\onyea005\Desktop\WBOB\Magnesium\Codes_Datasets\SAS Datasets';
libname mylib2 'C:\Users\onyea005\Desktop\WBOB\ARIC Data\Updated ARIC cases\CANCER';

*Loading the magnesium data and CRC data;

data mag1;
set mylib.mag1;
run;

data CRCdata;
set mylib2.crcdata;
run;

proc sort data = mag1; by ID; run;
proc sort data = crcdata; by ID; run;

data magnesium;
merge mag1 crcdata;
by id;
run;

proc contents data = magnesium;
run;

proc means data = magnesium n min mean median max std;
var magnf1 magnb1;
run;

*recoding the dietary magnesium variable account for different guidelines for men and women (from webmd);
*magf1 is the dietary magnesium, and magnb1 is the serum magnesium;

data magnesium;
set magnesium;
if magnf1 lt 320 and gender = "F" then dmag = 0;
else if magnf1 ge 320 and gender = "F" then dmag = 1;
else if magnf1 lt 420 and gender = "M" then dmag = 0;
else if magnf1 ge 420 and gender = "M" then dmag = 1;
run;

proc freq data = magnesium;
table gender*dmag magdie*dmag;
run;

proc print data = magnesium;
where dmag = .;
var gender magnf1 magdie;
run;

*Making quick models to present to Elizabeth and Dr. Prizment;

*New Dietary Magnesium Guidelines;

*Base model new dietary;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender/RL;
Run;

*Base model PH;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender phazard/RL;
phazard=dmag*(log(tpyrsany12));
Run;

*Adjusted model new dietary;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Adjusted model PH;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp phazard/RL;
phazard=dmag*(log(tpyrsany12));
Run;

*Colon cancer;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*rectal cancer;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*rectalca12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*rectalca12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Stratified analysis;

*Sex Stratified;

Proc phreg data=magnesium;
where gender = "M";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where gender = "M";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Race Stratified;

Proc phreg data=magnesium;
where racegrp = "W";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "W";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Time stratified analysis;
proc means data = magnesium n min mean median max std;
var tpyrsany12;
run;
 
*Setting up the variables;

/*censortime = follow-up time from baseline for study to 12/31/12
mediantime = median follow-up time for the analytic cohort
censortime_new = new follow-up time variable for the =<median time strata
For the =<median follow-up time strata, we only change the censor date for ppts with follow-up greater than the median time.
If censortime > mediantime, then censortime_new=mediantime;
Else censortime_new=censortime.*/

/* cancer = case status, where 1 is a case
For the > median follow-up strata, we only change cases that occur before median to non-cases.
If censortime =< mediantime and cancer=1, then cancer=0. */

/* 

In the < median time strata;
-Change any cancer cases that occured after the median time to 0;
-Follow up time = normal follow up time;

In the >= median time strata;
-Change any cancer cases that occured before the median time to 0;
-Follow up time = normal follow up time - median time;

*/

* The date format to manually create a variable for 12/312012 in SAS is DDMMYYw.;

data magnesium;
set magnesium;
censortime = 26;
mediantime = 13;
if tpyrsany12 gt mediantime then timestrata = 1; else timestrata = 0;
if tpyrsany12 gt mediantime then tpyrsnew = tpyrsany12 - mediantime; else tpyrsnew = tpyrsany12;
if CRC12 = 1 and  timestrata = 1 then CRCtime = 0; else CRCtime = CRC12;
if CRC12 = 1 and  timestrata = 0 then CRCtime2 = 0; else CRCtime2 = CRC12;
run;

* < median time strata;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

* > median time strata;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Trying to rerun the analyses using dietary and serum magnesium quintiles;
*There are no observations between 1.6 and 1.7, so quintile # 3 for serum magnesium would not be created; 
*I will use the cutoff ponts as suggested in Dr. Lutsey's paper; 

proc contents data = magnesium;
run;

proc means data = magnesium n mean median min p20 p40 p60 p80 max std;
var magnf1 magnb1;
run;

proc univariate data = magnesium;
var magnb1 magnf1;
histogram;
run;

data magnesium;
set magnesium;
if magnb1 le 1.40 then smagquint = 0;
else if 1.40 lt magnb1 le 1.50 then smagquint = 1;
else if 1.50 lt magnb1 le 1.60 then smagquint = 2;
else if 1.60 lt magnb1 le 1.70 then smagquint = 3;
else if magnb1 gt 1.70 then smagquint = 4;
if magnf1 lt 172.50 then dmagquint = 0;
else if 172.50 le magnf1 lt 217.85 then dmagquint = 1;
else if 217.85 le magnf1 lt 264.11 then dmagquint = 2;
else if 264.11 le magnf1 lt 325.84 then dmagquint = 3;
else if magnf1 gt 325.84 then dmagquint = 4;
run;

*Serum Magnesium Quintiles;

*Base model new dietary;
Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender/RL;
Run;

*Base model PH;
Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender phazard/RL;
phazard=smagquint*(log(tpyrsany12));
Run;

*Adjusted model new dietary;
Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Adjusted model PH;
Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp phazard/RL;
phazard=smagquint*(log(tpyrsany12));
Run;

*Colon cancer;
Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*rectal cancer;
Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*rectalca12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*rectalca12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Stratified analysis;

*Sex Stratified;

Proc phreg data=magnesium;
where gender = "M";
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where gender = "M";
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Race Stratified;

Proc phreg data=magnesium;
where racegrp = "W";
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "W";
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

* < median time strata;
Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

* > median time strata;
Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Dietary Magnesium Quintiles;

*Base model new dietary;
Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender/RL;
Run;

*Base model PH;
Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender phazard/RL;
phazard=smagquint*(log(tpyrsany12));
Run;

*Adjusted model new dietary;
Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Adjusted model PH;
Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp phazard/RL;
phazard=smagquint*(log(tpyrsany12));
Run;

*Colon cancer;
Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*rectal cancer;
Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*rectalca12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*rectalca12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Stratified analysis;

*Sex Stratified;

Proc phreg data=magnesium;
where gender = "M";
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where gender = "M";
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Race Stratified;

Proc phreg data=magnesium;
where racegrp = "W";
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "W";
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

* < median time strata;
Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

* > median time strata;
Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="4" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

******************************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************************;

*Rerunning the analysis for the poster with the new, updated 2012 aric cancer cases data;
*This version has the code formated to make the output needed for Elizabeth's table shells;

options ps = 3000 ls = 120 nofmterr;
options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '           ';
libname mylib 'C:\Users\onyea005\Desktop\WBOB\Magnesium\Codes_Datasets\SAS Datasets';
libname mylib2 'C:\Users\onyea005\Desktop\WBOB\ARIC Data\Updated ARIC cases\CANCER';

*Loading the magnesium data and CRC data;

data mag1;
set mylib.mag1;
run;

data CRCdata;
set mylib2.crcdata;
run;

proc sort data = mag1; by ID; run;
proc sort data = crcdata; by ID; run;

data magnesium;
merge mag1 crcdata;
by id;
run;

proc contents data = magnesium;
run;

proc means data = magnesium n min mean median max std;
var magnf1 magnb1;
run;

*recoding the dietary magnesium variable account for different guidelines for men and women (from webmd);
*magf1 is the dietary magnesium, and magnb1 is the serum magnesium;

data magnesium;
set magnesium;
if magnf1 lt 320 and gender = "F" then dmag = 0;
else if magnf1 ge 320 and gender = "F" then dmag = 1;
else if magnf1 lt 420 and gender = "M" then dmag = 0;
else if magnf1 ge 420 and gender = "M" then dmag = 1;
run;

proc freq data = magnesium;
table gender*dmag magdie*dmag;
run;

proc print data = magnesium;
where dmag = .;
var gender magnf1 magdie;
run;

*SAS Code for Table 1: Descriptive statistics;

*Demographics Table;
proc sort data = magnesium; by dmag; run;

proc freq data = magnesium;
tables dmag dmag*crc12;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 v1age01 bmi01 tcal1 dfib1 alco1 SPRT_I31 logcrp;
by dmag;
run; 

proc freq data = magnesium;
tables CIGT01 GENDER ASPIRINCODE01;
run;

*Hazard Ratio for colorectal, colon, and rectal cancers by magnesium intake below and above the US dietary recommendations in the ARIC study;

*Model that was reported in poster;

Proc phreg data=magnesium;
Class magdie (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdie tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Model reported in poster with updated magnesium variable;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Significance is out of the window, comparing the two variables. There were over 1000 observations amongs males that did not meet the 420 dietary magnesium recommendation;

proc freq data = magnesium;
table magdie*dmag dmag*gender magdie*gender;
run;

Proc phreg data=magnesium;
Class magdieq (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= magdieq tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

Proc phreg data=magnesium;
Class magdieq (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= magdieq tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp/RL;
Run;

*Looking at gender specific associations(results should be similar in women, different in men);

Proc phreg data=magnesium;
where gender = "M";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp /RL;
Run;

Proc phreg data=magnesium;
where gender = "M";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp phazard/RL;
phazard=magdie*(log(tpyrsany12));
Run;

Proc phreg data=magnesium;
where gender = "F";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp /RL;
phazard=magdie*(log(tpyrsany12));
Run;

Proc phreg data=magnesium;
where gender = "F";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag tcal1 center racegrp v1age01 gender bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp phazard/RL;
phazard=magdie*(log(tpyrsany12));
Run;

*adjusted for age, race, center, sex, caloric intake;

proc freq data = magnesium;
tables dmag dmag*crc12;
run;

proc means data = magnesium n min mean median max std sum;
var tpyrsany12;
by dmag;
where CRC12 = 1;
run; 

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1/RL;
Run;

**adjusted for age, race, center, sex, caloric intake, BMI, serum calcium, log (CRP);

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp/RL;
Run;

***adjusted for age, race, center, sex, caloric intake, BMI, serum calcium, log (CRP), diabetes;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp DIABTS03/RL;
Run;

*Table 2: Sex-stratified Hazard Ratio and 95% Confidence Interval for Colorectal Cancer by Category of Dietary Magnesium Intake (ARIC 1987-2006);

proc freq data = magnesium;
tables dmag dmag*crc12*gender;
run;

* adjusted for age, race, center, sex, caloric intake among men;

proc means data = magnesium n min mean median max std sum;
var tpyrsany12;
by dmag;
where CRC12 = 1 and Gender = "M";
run; 

Proc phreg data=magnesium;
where gender = "M";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag tcal1 center racegrp v1age01 gender/RL;
Run;

* adjusted for age, race, center, sex, caloric intake among women;

proc means data = magnesium n min mean median max std sum;
var tpyrsany12;
by dmag;
where CRC12 = 1 and Gender = "F";
run; 

Proc phreg data=magnesium;
where gender = "F";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag tcal1 center racegrp v1age01 gender/RL;
Run;

*Table 3: Race-stratified Hazard Ratio and 95% Confidence Interval for Colorectal Cancer by Category of Dietary Magnesium Intake (ARIC 1987-2006);

proc freq data = magnesium;
tables dmag dmag*crc12*racegrp;
run;

*adjusted for age, race, center, sex, caloric intake among whites;

proc means data = magnesium n min mean median max std sum;
var tpyrsany12;
by dmag;
where CRC12 = 1 and racegrp = "W";
run; 

Proc phreg data=magnesium;
where racegrp = "W";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag tcal1 center racegrp v1age01 gender/RL;
Run;

*adjusted for age, race, center, sex, caloric intake among blacks;

proc means data = magnesium n min mean median max std sum;
var tpyrsany12;
by dmag;
where CRC12 = 1 and racegrp = "B";
run; 

Proc phreg data=magnesium;
where racegrp = "B";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag tcal1 center racegrp v1age01 gender/RL;
Run;

*Table 5: Hazard Ratio and 95% Confidence Interval for Colon and Rectal Cancer Individually by Category of Dietary Magnesium Intake (ARIC 1987-2006);

proc freq data = magnesium;
tables dmag dmag*colonca12 dmag*rectalca12;
run;

*adjusted for age, race, center, sex, caloric intake among whites;

proc means data = magnesium n min mean median max std sum;
var tpyrsany12;
by dmag;
where colonca12 = 1;
run; 

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= dmag tcal1 center racegrp v1age01 gender/RL;
Run;

*adjusted for age, race, center, sex, caloric intake among blacks;

proc means data = magnesium n min mean median max std sum;
var tpyrsany12;
by dmag;
where rectalca12 = 1;
run; 

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*rectalca12(0)= dmag tcal1 center racegrp v1age01 gender/RL;
Run;

*****************************************************************************************************************************************************************************;

*Time stratified analysis;
proc means data = magnesium n min mean median max std;
var tpyrsany12;
run;
 
*Setting up the variables;

/*censortime = follow-up time from baseline for study to 12/31/12
mediantime = median follow-up time for the analytic cohort
censortime_new = new follow-up time variable for the =<median time strata
For the =<median follow-up time strata, we only change the censor date for ppts with follow-up greater than the median time.
If censortime > mediantime, then censortime_new=mediantime;
Else censortime_new=censortime.*/

/* cancer = case status, where 1 is a case
For the > median follow-up strata, we only change cases that occur before median to non-cases.
If censortime =< mediantime and cancer=1, then cancer=0. */

/* 

In the < median time strata;
-Change any cancer cases that occured after the median time to 0;
-Follow up time = normal follow up time;

In the >= median time strata;
-Change any cancer cases that occured before the median time to 0;
-Follow up time = normal follow up time - median time;

*/

* The date format to manually create a variable for 12/312012 in SAS is DDMMYYw.;

data magnesium;
set magnesium;
censortime = 26;
mediantime = 13;
if tpyrsany12 gt mediantime then timestrata = 1; else timestrata = 0;
if tpyrsany12 gt mediantime then tpyrsnew = tpyrsany12 - mediantime; else tpyrsnew = tpyrsany12;
if CRC12 = 1 and  timestrata = 1 then CRCtime = 0; else CRCtime = CRC12;
if CRC12 = 1 and  timestrata = 0 then CRCtime2 = 0; else CRCtime2 = CRC12;
run;

* < median time strata;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= dmag tcal1 center racegrp v1age01 gender/RL;
Run;

* > median time strata;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= dmag tcal1 center racegrp v1age01 gender/RL;
Run;

*Serum magnesium;

proc means data = magnesium n mean median min p25 p50 p75 max std;
var magnb1 magnb2;
run;

data magnesium;
set magnesium;
if magnb1 le 1.5 then smag = 0;
else if 1.5 lt magnb1 le 1.6 then smag = 1;
else if 1.6 lt magnb1 le 1.7 then smag = 2;
else if magnb1 gt 1.7 then smag = 3;
run;

proc freq data = magnesium;
tables smag;
run;

*adjusted for age, race, center, sex, caloric intake;

proc sort data = magnesium; by smag; run;

proc freq data = magnesium;
tables smag smag*crc12;
run;

proc means data = magnesium n min mean median max std sum;
var tpyrsany12;
by smag;
where CRC12 = 1;
run; 

Proc phreg data=magnesium;
Class smag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smag center racegrp v1age01 gender tcal1/RL;
Run;

**adjusted for age, race, center, sex, caloric intake, BMI, serum calcium, log (CRP);

Proc phreg data=magnesium;
Class smag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smag center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp/RL;
Run;

***adjusted for age, race, center, sex, caloric intake, BMI, serum calcium, log (CRP), diabetes;

Proc phreg data=magnesium;
Class smag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smag center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp DIABTS03/RL;
Run;

Proc phreg data=magnesium;
Class smag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= smag center racegrp v1age01 gender tcal1 bmi01 calcf1 logcrp DIABTS03/RL;
Run;

*Table 2: Hazard Ratio and 95% Confidence Interval for Colorectal Cancer by Category of Serum Magnesium among Male Participants;

Proc phreg data=magnesium;
where gender = "M";
Class smag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smag center racegrp v1age01 gender /RL;
Run;

*Table 3: Hazard Ratio and 95% Confidence Interval for Colorectal Cancer by Category of Serum Magnesium among Female Participants;

Proc phreg data=magnesium;
where gender = "F";
Class smag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smag center racegrp v1age01 gender /RL;
Run;

*Table 4: Hazard Ratio and 95% Confidence Interval for Colorectal Cancer by Category of Serum Magnesium among Black Participants;

Proc phreg data=magnesium;
where racegrp = "B";
Class smag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smag center racegrp v1age01 gender /RL;
Run;

*Table 5: Hazard Ratio and 95% Confidence Interval for Colorectal Cancer by Category of Serum Magnesium among White Participants;

Proc phreg data=magnesium;
where racegrp = "W";
Class smag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smag center racegrp v1age01 gender /RL;
Run;

*Table 6: Hazard Ratio and 95% Confidence Interval for Colon Cancer by Category of Serum Magnesium (ARIC 1987-2006);

Proc phreg data=magnesium;
Class smag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= smag center racegrp v1age01 gender /RL;
Run;

*Table 7: Hazard Ratio and 95% Confidence Interval for Rectal Cancer by Category of Serum Magnesium (ARIC 1987-2006);

Proc phreg data=magnesium;
Class smag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*rectalca12(0) = smag center racegrp v1age01 gender /RL;
Run;

*Dietary Magnesium Quintiles;

proc sort data = magnesium; by dmagquint; run;

proc means data = magnesium n min mean median max std;
var diemag;
by dmagquint;
run;

*Base model new dietary;
Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

*Base model PH;
Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender phazard/RL;
phazard=smagquint*(log(tpyrsany12));
Run;

*Adjusted model new dietary;
Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

*Adjusted model PH;
Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT phazard/RL;
phazard=smagquint*(log(tpyrsany12));
Run;

*Colon cancer;
Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*colonca12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

*rectal cancer;
Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*rectalca12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*rectalca12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

*Stratified analysis;

*Sex Stratified;

Proc phreg data=magnesium;
where gender = "M";
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
where gender = "M";
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
where gender = "M";
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1 HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

*Race Stratified;

Proc phreg data=magnesium;
where racegrp = "W";
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
where racegrp = "W";
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
where racegrp = "W";
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

*Setting up the variables for the time stratified analysis;

/*censortime = follow-up time from baseline for study to 12/31/12
mediantime = median follow-up time for the analytic cohort
censortime_new = new follow-up time variable for the =<median time strata
For the =<median follow-up time strata, we only change the censor date for ppts with follow-up greater than the median time.
If censortime > mediantime, then censortime_new=mediantime;
Else censortime_new=censortime.*/

/* cancer = case status, where 1 is a case
For the > median follow-up strata, we only change cases that occur before median to non-cases.
If censortime =< mediantime and cancer=1, then cancer=0. */

/* In the < median time strata;
-Change any cancer cases that occured after the median time to 0;
-Follow up time = normal follow up time;

In the >= median time strata;
-Change any cancer cases that occured before the median time to 0;
-Follow up time = normal follow up time - median time; */

*Time stratified analysis using the midpoint of follow up time as the median;
proc means data = magnesium n min mean median max std;
var tpyrsany12;
run;

data magnesium;
set magnesium;
censortime = 26;
mediantime = 13;
if tpyrsany12 gt mediantime then timestrata = 1; else timestrata = 0;
if tpyrsany12 gt mediantime then tpyrsnew = tpyrsany12 - mediantime; else tpyrsnew = tpyrsany12;
if CRC12 = 1 and  timestrata = 1 then CRCtime = 0; else CRCtime = CRC12;
if CRC12 = 1 and  timestrata = 0 then CRCtime2 = 0; else CRCtime2 = CRC12;
run;

* < median time strata;
Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsnew*CRCtime(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

* > median time strata;
Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsnew*CRCtime2(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

*Time stratified analysis using the true median follow up time as the median;
proc means data = magnesium n min mean median max std;
var tpyrsany12;
run;

data magnesium;
set magnesium;
censortime = 26;
mediantime = 23;
if tpyrsany12 gt mediantime then timestrata = 1; else timestrata = 0;
if tpyrsany12 gt mediantime then tpyrsnew = tpyrsany12 - mediantime; else tpyrsnew = tpyrsany12;
if CRC12 = 1 and  timestrata = 1 then CRCtime = 0; else CRCtime = CRC12;
if CRC12 = 1 and  timestrata = 0 then CRCtime2 = 0; else CRCtime2 = CRC12;
run;

* < median time strata;
Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsnew*CRCtime(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

* > median time strata;
Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= dmagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

Proc phreg data=magnesium;
Class dmagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsnew*CRCtime2(0)= dmagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" dmagquint 153 202 242 288 366;
Run;

******************************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************************;

*Rerunning the analysis for the poster with the new, updated 2012 aric cancer cases data;
*This version combines V1 + V2 for serum magnesium, and V1 + V3 for dietary magnesium;

options ps = 3000 ls = 120 nofmterr;
options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '           ';
libname mylib 'C:\Users\onyea005\Desktop\WBOB\Magnesium\Codes_Datasets\SAS Datasets';
libname mylib2 'C:\Users\onyea005\Desktop\WBOB\ARIC Data\Updated ARIC cases\CANCER';

*Loading the magnesium data and CRC data;

data mag1;
set mylib.mag1;
run;

data CRCdata;
set mylib2.crcdata;
run;

proc sort data = mag1; by ID; run;
proc sort data = crcdata; by ID; run;

data magnesium;
merge mag1 crcdata;
by id;
run;

proc contents data = magnesium;
run;

proc means data = magnesium n min mean median max std;
var magnf1 magnf3 magnb1 magnb2;
run;

proc contents data = magnesium;
run;

proc means data = magnesium n mean median min p20 p40 p60 p80 max std;
var magnf1 magnf3 magnb1 magnb2;
run;

proc freq data = magnesium;
table ever_hrt1 ever_hrt2 ever_hrt3;
run;

data magnesium;
set magnesium;
if ever_hrt1 = . and gender = "M" then HRT = 1;
else if ever_hrt1 = 0 and gender = "F" then HRT = 2;
else if ever_hrt1 = 1 and gender = "F" then HRT = 3;
run;

proc freq data = magnesium;
table ever_hrt1*gender HRT*gender;
run;

*Combining V1 + V2 for serum magnesium, and V1 + V3 for dietary magnesium;

data magnesium;
set magnesium;
if magnb1 and magnb2 ne . then do;
sermag = (magnb1 + magnb2) / 2; 
if magnb2 = . then sermag = magnb1;
end;
if magnf1 and magnf3 ne . then do;
diemag = (magnf1 + magnf3) / 2;
if magnf3 = . then diemag = magnf1;
end;
run;

data magnesium;
set magnesium;
if magnb1 and magnb2 ne . then sermag = (magnb1 + magnb2) / 2; 
else if magnb2 = . then sermag = magnb1;
if magnf1 and magnf3 ne . then diemag = (magnf1 + magnf3) / 2;
else if magnf3 = . then diemag = magnf1;
run;

proc means data = magnesium n mean median min p20 p40 p60 p80 max std;
var magnf1 magnf3 diemag magnb1 magnb2 sermag;
run;

*Trying to rerun the analyses using dietary and serum magnesium quintiles;
*magf1 and magnf3 is the dietary magnesium, magnb1 and magn is the serum magnesium;

data magnesium;
set magnesium;
if sermag le 1.50 then smagquintv3 = 0;
else if 1.50 lt sermag le 1.60 then smagquintv3 = 1;
else if 1.60 lt sermag le 1.70 then smagquintv3 = 2;
else if 1.70 lt sermag le 1.80 then smagquintv3 = 3;
else if sermag gt 1.75 then smagquint = 4;
if diemag lt 184.56 then dmagquint = 0;
else if 184.56 le diemag lt 225.44 then dmagquint = 1;
else if 225.44 le diemag lt 265.38 then dmagquint = 2;
else if 265.38 le diemag lt 318.60 then dmagquint = 3;
else if diemag gt 318.60 then dmagquint = 4;
run;

*recoding the dietary magnesium variable account for different guidelines for men and women (from webmd);

data magnesium;
set magnesium;
if diemag lt 320 and gender = "F" then dmag = 0;
else if diemag ge 320 and gender = "F" then dmag = 1;
else if diemag lt 420 and gender = "M" then dmag = 0;
else if diemag ge 420 and gender = "M" then dmag = 1;
run;

*Checking the new exposure variables;

proc freq data = magnesium;
table gender*dmag magdie*dmag dmag smagquint dmagquint;
run;

*Testing out automated ranks for the exposure variables;

proc rank data=magnesium groups=5 out=magnesium;
var sermag diemag magnb1; 
ranks smagquint dmagquint smagquintv2;
run;

proc freq data = magnesium;
tables smagquint dmagquint smagquintv2;
run;

*Serum Magnesium Quintiles;

proc sort data = magnesium; by smagquint; run;

proc means data = magnesium n min mean median max std;
var sermag;
by smagquint;
run;

*SAS Code for Table 1: Descriptive statistics;

*Demographics Table;

data magnesium;
set magnesium;
if smagquint = . then delete;
if CRC12 = . then delete;
run;

proc contents data = magnesium;
run;

proc sort data = magnesium; by smagquint; run;

proc freq data = magnesium;
tables smagquint smagquint*crc12;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 v1age01 bmi01 tcal1 dfib1 alco1 SPRT_I31 logcrp calcf1 diemag;
by smagquint;
run; 

proc freq data = magnesium;
tables GENDER*smagquint racegrp*smagquint educat*smagquint CIGT01*smagquint ASPIRINCODE01*smagquint DIABTS03*smagquint / nopercent nocol;
run;

*Table 2;

proc sort data = magnesium; by smagquint; run;

proc freq data = magnesium;
tables smagquint smagquint*crc12;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12;
by smagquint;
run; 

*Base model new dietary;
Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

*Base model PH;
Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender phazard/RL;
phazard=smagquint*(log(tpyrsany12));
Run;

*Adjusted model new dietary;
Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 educat bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

*Adjusted model PH;
Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT phazard/RL;
phazard=smagquint*(log(tpyrsany12));
Run;

*Stratified analysis;

*Sex Stratified;

proc sort data = magnesium; by smagquint; run;

proc freq data = magnesium;
tables smagquint smagquint*crc12;
where gender = "M";
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 sermag;
by smagquint;
where gender = "M";
run;

proc sort data = magnesium; by smagquint; run;

proc freq data = magnesium;
tables smagquint smagquint*crc12;
where gender = "F";
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 sermag;
by smagquint;
where gender = "F";
run;

Proc phreg data=magnesium;
where gender = "M";
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" smagquint 1.41 1.55 1.62 1.68 1.80;
Run;

Proc phreg data=magnesium;
where gender = "M";
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" smagquint 1.41 1.55 1.62 1.68 1.80;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" smagquint 1.41 1.55 1.62 1.68 1.80;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" smagquint 1.41 1.55 1.62 1.68 1.80;
Run;

*Race Stratified;

proc sort data = magnesium; by smagquint; run;

proc freq data = magnesium;
tables smagquint smagquint*crc12;
where racegrp = "W";
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 sermag;
by smagquint;
where racegrp = "W";
run;

proc sort data = magnesium; by smagquint; run;

proc freq data = magnesium;
tables smagquint smagquint*crc12;
where racegrp = "B";
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 sermag;
by smagquint;
where racegrp = "B";
run;

Proc phreg data=magnesium;
where racegrp = "W";
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" smagquint 1.41 1.55 1.62 1.68 1.80;
Run;

Proc phreg data=magnesium;
where racegrp = "W";
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" smagquint 1.41 1.55 1.62 1.68 1.80;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" smagquint 1.40 1.55 1.60 1.70 1.80;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" smagquint 1.40 1.55 1.60 1.70 1.80;
Run;

*Setting up the variables for the time stratified analysis;

/*censortime = follow-up time from baseline for study to 12/31/12
mediantime = median follow-up time for the analytic cohort
censortime_new = new follow-up time variable for the =<median time strata
For the =<median follow-up time strata, we only change the censor date for ppts with follow-up greater than the median time.
If censortime > mediantime, then censortime_new=mediantime;
Else censortime_new=censortime.*/

/* cancer = case status, where 1 is a case
For the > median follow-up strata, we only change cases that occur before median to non-cases.
If censortime =< mediantime and cancer=1, then cancer=0. */

/* In the < median time strata;
-Change any cancer cases that occured after the median time to 0;
-Follow up time = normal follow up time;

In the >= median time strata;
-Change any cancer cases that occured before the median time to 0;
-Follow up time = normal follow up time - median time; */

*Time stratified analysis using the midpoint of follow up time as the median;
proc means data = magnesium n min mean median max std;
var tpyrsany12;
run;

data magnesium;
set magnesium;
censortime = 26;
mediantime = 13;
if tpyrsany12 gt mediantime then timestrata = 1; else timestrata = 0;
if tpyrsany12 gt mediantime then tpyrsnew = tpyrsany12 - mediantime; else tpyrsnew = tpyrsany12;
if CRC12 = 1 and  timestrata = 1 then CRCtime = 0; else CRCtime = CRC12;
if CRC12 = 1 and  timestrata = 0 then CRCtime2 = 0; else CRCtime2 = CRC12;
run;

proc sort data = magnesium; by smagquint; run;

proc freq data = magnesium;
tables smagquint smagquint*crc12;
where timestrata = 0;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 sermag;
by smagquint;
where timestrata = 0;
run;

proc sort data = magnesium; by smagquint; run;

proc freq data = magnesium;
tables smagquint smagquint*crc12;
where timestrata = 1;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 sermag;
by smagquint;
where timestrata = 1;
run;

* < median time strata;
Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsnew*CRCtime(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

* > median time strata;
Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsnew*CRCtime2(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

*Time stratified analysis using the true median follow up time as the median;
proc means data = magnesium n min mean median max std;
var tpyrsany12;
run;

data magnesium;
set magnesium;
censortime = 26;
mediantime = 23;
if tpyrsany12 gt mediantime then timestrata = 1; else timestrata = 0;
if tpyrsany12 gt mediantime then tpyrsnew = tpyrsany12 - mediantime; else tpyrsnew = tpyrsany12;
if CRC12 = 1 and  timestrata = 1 then CRCtime = 0; else CRCtime = CRC12;
if CRC12 = 1 and  timestrata = 0 then CRCtime2 = 0; else CRCtime2 = CRC12;
run;

* < median time strata;
Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsnew*CRCtime(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

* > median time strata;
Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsnew*CRCtime2(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

*Colon cancer;

proc sort data = magnesium; by smagquint; run;

proc freq data = magnesium;
tables smagquint smagquint*colonca12;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 sermag;
by smagquint;
run;

Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*colonca12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

*rectal cancer;

proc sort data = magnesium; by smagquint; run;

proc freq data = magnesium;
tables smagquint smagquint*rectalca12;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 sermag;
by smagquint;
run;

Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*rectalca12(0)= smagquint center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

Proc phreg data=magnesium;
Class smagquint (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*rectalca12(0)= smagquint center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
contrast "trend in magnesium" smagquint 1.45 1.55 1.60 1.70 1.80;
Run;

*Dietary Magnesium guidelines;

proc sort data = magnesium; by dmag; run;

proc freq data = magnesium;
table dmag dmag*crc12;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 diemag;
by dmag;
run;

*Base model new dietary;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender/RL;
Run;

*Base model PH;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender phazard/RL;
phazard=smagquint*(log(tpyrsany12));
Run;

*Adjusted model new dietary;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

*Adjusted model PH;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT phazard/RL;
phazard=smagquint*(log(tpyrsany12));
Run;

*Stratified analysis;

*Sex Stratified;

proc sort data = magnesium; by dmag; run;

proc freq data = magnesium;
table dmag dmag*crc12;
where gender = "M";
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 diemag;
by dmag;
where gender = "M";
run;

proc sort data = magnesium; by dmag; run;

proc freq data = magnesium;
table dmag dmag*crc12;
where gender = "F";
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 diemag;
by dmag;
where gender = "F";
run;

Proc phreg data=magnesium;
where gender = "M";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where gender = "M";
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1 HRT/RL;
Run;

Proc phreg data=magnesium;
where gender = "F";
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

*Race Stratified;

proc sort data = magnesium; by dmag; run;

proc freq data = magnesium;
table dmag dmag*crc12;
where racegrp = "W";
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 diemag;
by dmag;
where racegrp = "W";
run;

proc sort data = magnesium; by dmag; run;

proc freq data = magnesium;
table dmag dmag*crc12;
where racegrp = "B";
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 diemag;
by dmag;
where racegrp = "B";
run;

Proc phreg data=magnesium;
where racegrp = "W";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "W";
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
where racegrp = "B";
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*CRC12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

*Setting up the variables for the time stratified analysis;

/*censortime = follow-up time from baseline for study to 12/31/12
mediantime = median follow-up time for the analytic cohort
censortime_new = new follow-up time variable for the =<median time strata
For the =<median follow-up time strata, we only change the censor date for ppts with follow-up greater than the median time.
If censortime > mediantime, then censortime_new=mediantime;
Else censortime_new=censortime.*/

/* cancer = case status, where 1 is a case
For the > median follow-up strata, we only change cases that occur before median to non-cases.
If censortime =< mediantime and cancer=1, then cancer=0. */

/* In the < median time strata;
-Change any cancer cases that occured after the median time to 0;
-Follow up time = normal follow up time;

In the >= median time strata;
-Change any cancer cases that occured before the median time to 0;
-Follow up time = normal follow up time - median time; */

*Time stratified analysis using the midpoint of follow up time as the median;

proc means data = magnesium n min mean median max std;
var tpyrsany12;
run;

data magnesium;
set magnesium;
censortime = 26;
mediantime = 13;
if tpyrsany12 gt mediantime then timestrata = 1; else timestrata = 0;
if tpyrsany12 gt mediantime then tpyrsnew = tpyrsany12 - mediantime; else tpyrsnew = tpyrsany12;
if CRC12 = 1 and  timestrata = 1 then CRCtime = 0; else CRCtime = CRC12;
if CRC12 = 1 and  timestrata = 0 then CRCtime2 = 0; else CRCtime2 = CRC12;
run;

proc sort data = magnesium; by dmag; run;

proc freq data = magnesium;
table dmag dmag*crc12;
where timestata  = 0;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 diemag;
by dmag;
where timestata  = 0;
run;

proc sort data = magnesium; by dmag; run;

proc freq data = magnesium;
table dmag dmag*crc12;
where timestata  = 0;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 diemag;
by dmag;
where timestata  = 0;
run;

* < median time strata;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsnew*CRCtime(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

* > median time strata;
Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsnew*CRCtime2(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsnew*CRCtime2(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

*Colon cancer;

proc sort data = magnesium; by dmag; run;

proc freq data = magnesium;
table dmag dmag*colonca12;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 diemag;
by dmag;
run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*colonca12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*colonca12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

*rectal cancer;

proc sort data = magnesium; by dmag; run;

proc freq data = magnesium;
table dmag dmag*rectalca12;
run;

proc means data = magnesium n min mean median max std;
var tpyrsany12 diemag;
by dmag;
run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender;
Model tpyrsany12*rectalca12(0)= dmag center racegrp v1age01 gender tcal1 alco1 calcf1 dfib1/RL;
Run;

Proc phreg data=magnesium;
Class dmag (ref="0" param=ref) center racegrp gender HRT;
Model tpyrsany12*rectalca12(0)= dmag center racegrp v1age01 gender tcal1  bmi01 SPRT_I31 alco1 calcf1 dfib1 CIGT01 CIGTYR01 ASPIRINCODE01 logcrp HRT/RL;
Run;

