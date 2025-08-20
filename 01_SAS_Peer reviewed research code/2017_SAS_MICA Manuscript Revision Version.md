options ps = 3000 ls = 120 nofmterr;
*options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '           ';
*options ps = 3000 ls = 120;
libname mylib 'C:\Users\onyea005\Desktop\WBOB\MICA Study\Preliminary Analysis';
*%inc 'C:\Documents and Settings\prizm001\Desktop\format.sas'; 

/*Creating the main dataset from the sas database*/

data one;
set mylib.analys;

proc contents;
run;

proc sort;
by id;
run;

/*Importing additional plasma results from the excel spreadsheet*/

PROC IMPORT OUT= work.Plasma DATAFILE= "C:\Users\onyea005\Desktop\WBOB\MICA Study\Preliminary Analysis\MICA.xlsx" DBMS=xlsx REPLACE;
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

*if batch=. then delete;
proc means ;
var id;
run;

proc freq data=plasma2;
table mica;
run;

proc sort data=plasma2;
by id;
run;

/*Merging the cohort data with the plasma lab results data*/

data comb;
merge  plasma2 (in=a) one (in=b);
by id;
if a=1 and b=1;

proc print data = comb;
run;

data comb;
set comb;
keep id panc AGE SEX GENDER ETHNIC1 EDUC BDATE BIRTHDT DEATHDTE CASE ELIG FCONDATE DX_DATE DXSOURCE BIOPSY CYTOLOGY STAT PACK_YRS DIABETES DIAB_AGE diab sumliq MICA; 
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
iF ID IN (1501, 5057, 5055, 385, 506, 81,  82, 994, 308) THEN delete;
run;

data analys1;
set analys1;
if MICA='NS' then do; mican=0; mican1 =0; mican2=2; exc=1; end;
else if MICA ne 'NS' then do; exc=2;  mican=round (MICA,0.01); mican1=round (MICA,0.01); mican2=round (MICA,0.01); end;
IF 0 le  MICAn LE 2 THEN exc1=1;
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
else if 2 lt mican le  32.10 then micac=1;
else if 32.14 le mican le  64.7  then micac=2;
else if  mican ge 64.8 then micac=3; 
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
  insetgroup  mean  min max; 
 run;

proc univariate NORMAL PLOT data=analys3; var mican;
where case=2;
histogram  mican/normal;
run;
 
/*Cleaning up the variables used in adjusting the models */

data results1;
set analys3;
run;

proc sort data =results1;
by case;
run;

*Making a categorical variable for age, alcohol, and ever vs. never smokers;

data results1;
set results1;
if age lt 70 then agecat = "1";
if age ge 70 then agecat = "2";
if sumliq=0 then alco=0;
else if 0<sumliq le 7 then alco=1; 
else if sumliq gt 7 then alco=2; 
if stat = "." then delete;
if stat = 1 then stat2=0; else stat2=1;
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

PROC IMPORT OUT= work.mayo2 DATAFILE= "C:\Users\onyea005\Desktop\WBOB\MICA Study\Preliminary Analysis\MAYO.xlsx" DBMS=xlsx REPLACE;
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
   outfile= "C:\Users\onyea005\Desktop\WBOB\MICA Study\Preliminary Analysis\MICA_STATA"
   dbms=XLSX
   replace;
run;

/* Analysis */
 
*Analysis 1: Using MICA as a continuous variable with all NS values coded as 0 (mican1);

*Crude Model;
proc logistic data = results2;
model case (EVENT='1')= mican1;
run;

*Adjusted Model;
proc logistic data = results2;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican1 age sex stat2;
run;

proc logistic data = results2;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican1 age sex stat2 pack_yrs;
run;

proc logistic data = results2;
class sex stat2 (ref='0') alco(ref='0'); 
model case (EVENT='1')= mican1 age sex stat2 pack_yrs alco;
run;

*Stratified analysis;
proc logistic data = results2;
where sex = "M";
class stat2 (ref='0'); 
model case (EVENT='1')= mican1 age stat2;
run;

proc logistic data = results2;
where sex = "F";
class stat2 (ref='0'); 
model case (EVENT='1')= mican1 age stat2;
run;

proc logistic data = results2;
where agecat = "1";
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican1 age sex stat2;
run;

proc logistic data = results2;
where agecat = "2";
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican1 age sex stat2;
run;

*interactions;
proc logistic data = results2;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican1 age sex stat2 mican1*age;
oddsratio mican1;
run;

proc logistic data = results2;
class sex stat2 (ref= '0'); 
model case (EVENT='1')= mican1 age sex stat2 mican1*sex;
oddsratio mican1;
run;

proc logistic data = results2;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican1 age sex stat2 mican1*stat2;
oddsratio mican1;
run;

*Analysis 2: Using MICA as a continuous variable with all NS values coded as 2 (mican2);

*Crude Model;
proc logistic data = results2;
model case (EVENT='1')= mican2;
run;

*Adjusted Model;
proc logistic data = results2;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican2 age sex stat2;
run;

proc logistic data = results2;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican2 age sex stat2 pack_yrs;
run;

proc logistic data = results2;
class sex stat2 (ref='0') alco(ref='0'); 
model case (EVENT='1')= mican2 age sex stat2 pack_yrs alco;
run;

*Stratified analysis;
proc logistic data = results2;
where sex = "M";
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican2 age stat2;
run;

proc logistic data = results2;
where sex = "F";
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican2 age stat2;
run;

proc logistic data = results2;
where agecat = "1";
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican2 age sex stat2;
run;

proc logistic data = results2;
where agecat = "2";
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican2 age sex stat2;
run;

*interactions;
proc logistic data = results2;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican2 age sex stat2 mican2*age;
oddsratio mican2;
run;

proc logistic data = results2;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican2 age sex stat2 mican2*sex;
oddsratio mican2;
run;

proc logistic data = results2;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican2 age sex stat2 mican2*stat2;
oddsratio mican2;
run;

*Analysis 3: Using MICA as a categorical variable with all NS values coded as 0 (micac);

*Crude Model;
proc logistic data = results2;
class micac (ref='0'); 
model case (EVENT='1')= micac;
run;

*Adjusted Model;
proc logistic data = results2;
class sex stat2 (ref='0') micac (ref='0'); 
model case (EVENT='1')= micac age sex stat2;
run;

proc logistic data = results2;
class sex stat2 (ref='0') micac (ref='0'); 
model case (EVENT='1')= micac age sex stat2 pack_yrs;
run;

proc logistic data = results2;
class sex stat2 (ref='0') alco(ref='0') micac (ref='0'); 
model case (EVENT='1')= micac age sex stat2 pack_yrs alco;
run;

*Stratified analysis;
proc logistic data = results2;
where sex = "M";
class sex stat2 (ref='0') micac (ref='0'); 
model case (EVENT='1')= micac age stat2;
run;

proc logistic data = results2;
where sex = "F";
class sex stat2 (ref='0') micac (ref='0'); 
model case (EVENT='1')= micac age stat2;
run;

proc logistic data = results2;
where agecat = "1";
class sex stat2 (ref='0') micac (ref='0'); 
model case (EVENT='1')= micac age sex stat2;
run;

proc logistic data = results2;
where agecat = "2";
class sex stat2 (ref='0') micac (ref='0'); 
model case (EVENT='1')= micac age sex stat2;
run;

*interactions;
proc logistic data = results2;
class sex stat2 (ref='0') micac (ref='0'); 
model case (EVENT='1')= micac age sex stat2 micac*age;
oddsratio micac;
run;

proc logistic data = results2;
class sex stat2 (ref='0') micac (ref='0'); 
model case (EVENT='1')= micac age sex stat2 micac*sex;
oddsratio micac;
run;

proc logistic data = results2;
class sex stat2 (ref='0') micac (ref='0'); 
model case (EVENT='1')= micac age sex stat2 micac*stat2;
oddsratio micac;
run;

*Analysis 4: Using MICA as a dichotomous variable with all NS values coded as 0 (micad);

*Crude Model;
proc logistic data = results2;
class micad (ref='0'); 
model case (EVENT='1')= micad;
contrast "trend in MICA" micad 0.08 59.61;
run;

proc means data = results2;
class micad;
var mican;
run;

*Adjusted Model;
proc logistic data = results2;
class sex stat2 (ref='0') micad (ref='0'); 
model case (EVENT='1')= micad age sex stat2;
contrast "trend in MICA" micad 0.08 59.61;
run;

proc logistic data = results2;
class sex stat2 (ref='0') micad (ref='0'); 
model case (EVENT='1')= micad age sex stat2 pack_yrs;
run;

proc logistic data = results2;
class sex stat2 (ref='0') micad (ref='0'); 
model case (EVENT='1')= micad age sex stat2 pack_yrs;
run;

proc logistic data = results2;
class sex stat2 (ref='0') alco(ref='0') micad (ref='0'); 
model case (EVENT='1')= micad age sex stat2 pack_yrs alco;
run;

*Stratified analysis;
proc logistic data = results2;
where sex = "M";
class sex stat2 (ref='0') micad (ref='0'); 
model case (EVENT='1')= micad age stat2;
run;

proc logistic data = results2;
where sex = "F";
class sex stat2 (ref='0') micad (ref='0'); 
model case (EVENT='1')= micad age stat2;
run;

proc logistic data = results2;
where agecat = "1";
class sex stat2 (ref='0') micad (ref='0'); 
model case (EVENT='1')= micad age sex stat2;
run;

proc logistic data = results2;
where agecat = "2";
class sex stat2 (ref='0') micad (ref='0'); 
model case (EVENT='1')= micad age sex stat2;
run;

*interactions;
proc logistic data = results2;
class sex stat2 (ref='0') micad (ref='0'); 
model case (EVENT='1')= micad age sex stat2 micad*age;
oddsratio micad;
run;

proc logistic data = results2;
class sex stat2 (ref='0') micad (ref='0'); 
model case (EVENT='1')= micad age sex stat2 micad*sex;
oddsratio micad;
run;

proc logistic data = results2;
class sex stat2 (ref='0') micad (ref='0'); 
model case (EVENT='1')= micad age sex stat2 micad*stat2;
oddsratio micad;
run;

*Analysis 5: Using MICA as a categorical variable only taking the values above 2 pg/ul into account, since the association with the dichotomous variable was non significant;

*Crude Model;
proc logistic data = results2;
where micac ne 0;
class micac (ref='1'); 
model case (EVENT='1')= micac;
run;

*Adjusted Model;
proc logistic data = results2;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2;
run;

proc logistic data = results2;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2 pack_yrs;
run;

proc logistic data = results2;
where micac ne 0;
class sex stat2 (ref='0') alco(ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2 pack_yrs alco;
run;

*Stratified analysis;
proc logistic data = results2;
where sex = "M";
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age stat2;
run;

proc logistic data = results2;
where sex = "F";
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age stat2;
run;

proc logistic data = results2;
where agecat = '1';
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2;
run;

proc logistic data = results2;
where agecat = '2';
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2;
run;

*interactions;
proc logistic data = results2;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2 micac*age;
oddsratio micac;
run;

proc logistic data = results2;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2 micac*sex;
oddsratio micac;
run;

proc logistic data = results2;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2 micac*stat2;
oddsratio micac;
run;

*Analysis 6: Using MICA as a continuous variable without NS (mican1), and setting the unit to 1 standard deviation;

data results3;
set results2;
where mican1 ne 0;
run;
 
*Crude Model;
proc logistic data = results3;
model case (EVENT='1')= mican1;
units mican1 = sd;
run;

*Adjusted Model;
proc logistic data = results3;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican1 age sex stat2;
units mican1 = sd;
run;

*Stratified analysis;
proc logistic data = results3;
where sex = "M";
class stat2 (ref='0'); 
model case (EVENT='1')= mican1 age stat2;
units mican1 = sd;
run;

proc logistic data = results3;
where sex = "F";
class stat2 (ref='0'); 
model case (EVENT='1')= mican1 age stat2;
units mican1 = sd;
run;

proc logistic data = results3;
where agecat = "1";
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican1 age sex stat2;
units mican1 = sd;
run;

proc logistic data = results3;
where agecat = "2";
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican1 age sex stat2;
units mican1 = sd;
run;

*interactions;
proc logistic data = results3;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican1 age sex stat2 mican1*age;
oddsratio mican1;
units mican1 = sd;
run;

proc logistic data = results3;
class sex stat2 (ref= '0'); 
model case (EVENT='1')= mican1 age sex stat2 mican1*sex;
oddsratio mican1;
units mican1 = sd;
run;

proc logistic data = results3;
class sex stat2 (ref='0'); 
model case (EVENT='1')= mican1 age sex stat2 mican1*stat2;
oddsratio mican1;
units mican1 = sd;
run;

*Analysis 7: Categorical Mica, with contrast test for linearity using the average MICA concentration in each category (19.4 46.7 111.1) to test for a trend

*Crude Model;
proc logistic data = results2;
where micac ne 0;
class micac (ref='1'); 
model case (EVENT='1')= micac;
contrast "trend in MICA" micac 19.4 46.7 111.1;
run;

*Adjusted Model;
proc logistic data = results2;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2;
contrast "trend in MICA" micac 19.4 46.7 111.1;
run;

*Stratified analysis;
proc logistic data = results2;
where sex = "M";
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age stat2;
contrast "trend in MICA" micac 19.4 46.7 111.1;
run;

proc logistic data = results2;
where sex = "F";
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age stat2;
contrast "trend in MICA" micac 19.4 46.7 111.1;
run;

proc logistic data = results2;
where agecat = '1';
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2;
contrast "trend in MICA" micac 19.4 46.7 111.1;
run;

proc logistic data = results2;
where agecat = '2';
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2;
contrast "trend in MICA" micac 19.4 46.7 111.1;
run;

*interactions;
proc logistic data = results2;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2 micac*age;
oddsratio micac;
run;

proc logistic data = results2;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2 micac*sex;
oddsratio micac;
run;

proc logistic data = results2;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model case (EVENT='1')= micac age sex stat2 micac*stat2;
oddsratio micac;
run;

*Testing out dummy coded variables;
data results3;
set results3;
if micac=1 then micac1=1; else micac1=0;
if micac=2 then micac2=1; else micac2=0;
if micac=3 then micac3=1; else micac3=0;
if case = 1 then panc=1; else panc=0;
run;

*Crude Model;
proc logistic data = results3;
where micac ne 0;
class micac1 (ref='0') micac2 (ref='0') micac3 (ref='0'); 
model case (EVENT='1')= micac2 micac3;
run;

*Making a mica serum level in predictor categories by case control status ;

proc means mean median min max std data = results3;
where sex = "F";
class panc;
var mican;
run;

proc means mean median min max std data = results3;
where sex = "M";
class panc;
var mican;
run;

proc means mean median min max std data = results3;
where agecat = '1';
class panc;
var mican;
run;

proc means mean median min max std data = results3;
where agecat = '2';
class panc;
var mican;
run;

proc freq data = results3;
tables stat2;
run;


proc means mean median min max std data = results3;
where stat = 1;
class panc;
var mican;
run;

proc means mean median min max std data = results3;
where stat = 2;
class panc;
var mican;
run;

proc means mean median min max std data = results3;
where stat = 3;
class panc;
var mican;
run;


proc contents data = results3;
run;

proc freq data = results3;
tables alco;
run;

proc means mean median min max std data = results3;
where alco = 0;
class panc;
var mican;
run;

proc means mean median min max std data = results3;
where alco = 1;
class panc;
var mican;
run;

proc means mean median min max std data = results3;
where alco = 2;
class panc;
var mican;
run;

*Testing out continuous by categorical interaction plots;

*use genmod to get the parameter estimates;
proc genmod data = results3 descending;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model panc = micac age sex stat2 micac*age/dist=binomial link=logit type3;
store catcont;
run;

*using plm to get the stratum specific slopes of age in mica categories;
proc plm restore = catcont;
estimate 'age slope, micac = 1' age 1 age*micac  1 0 0,
         'age slope, micac = 2' age 1 age*micac  0 1 0,
         'age slope, micac = 3' age 1 age*micac  0 0 1 / e;
run;

*using plm to compare the slopes of age in mica categories;
proc plm restore = catcont;
estimate 'age slope, micac = 1 vs 2' age*micac  -1 1 0,
         'age slope, micac = 1 vs 3' age*micac  -1 0 1,
         'age slope, micac = 2 vs 3' age*micac  0 -1 1 / e;
run;

*using plm to create an interaction plot for mica*age;
proc plm restore=catcont;
effectplot slicefit (x=age sliceby=micac) / clm;
run;

*Testing out categorical by categorical interaction plots;

*use genmod to get the parameter estimates;
proc genmod data = results3 descending;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model panc = micac age sex stat2 micac*sex/dist=binomial link=logit type3;
store catcat;
run;

*using plm to get the stratum specific slopes of mica in sex categories;
proc plm restore = catcat;
slice sex*micac / sliceby=sex diff adj=bon plots=none nof e means;
run;

proc plm restore=catcat;
effectplot interaction (x=sex sliceby=micac) / clm connect;
run;

*Making sure I get the same answers using genmod and logistic regression;

proc genmod data = results3 descending;
where micac ne 0;
class sex stat2 (ref='0') micac (ref='1'); 
model panc = micac age sex stat2/dist=binomial link=logit type3;
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

ODS Graphics on;
proc lifetest  data=results2 method=life plots=(s) intervals = 0 to 180 by 20, 180 to 270 by 30, 270 to 5000 by 100;
where micac ne 0;
time period*status2(0);
strata micac;
run;
ODS Graphics off;

/*Modified censoring criteria*/

ODS Graphics on;
proc lifetest  data=results2 method=life plots=(s) intervals = 0 to 180 by 20, 180 to 270 by 30, 270 to 1000 by 100;
where micac ne 0;
time period*status2(0);
strata micac;
run;
ODS Graphics off;

/*Testing AUC Graphs*/
data results1;
set results1;
if micac = 0 then micaroc = 0; else micaroc=1;
if case = 2 then caseroc = 0; else caseroc = 1;
run;

proc freq data = results1;
tables caseroc*micaroc;
run;

/*use the mica as a continuous variables to get multiple cutpoints in the ROC Graph*/

proc logistic data=results1 noprint;
where mican ge 2;
model caseroc (EVENT='1')=mican / outroc=ROCData;
run;

symbol1 v=dot i=join;
proc gplot data=ROCData;
 plot _sensit_*_1mspec_/vaxis=0 to 1 by .1 cframe=ligr;
run;quit; 

proc print data = ROCData;
run;

/* preparing dataset for survival analysis by genetic information */

proc print data = results2;
run;

PROC IMPORT OUT= work.micadna DATAFILE= "C:\Users\onyea005\Desktop\WBOB\MICA Study\Preliminary Analysis\NKG2D genotype data for pancreatic caco study - with covariates.xlsx" DBMS=xlsx REPLACE;
     SHEET="MICA DNA"; 
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
if  rs1049174 = "Unknown" then rs1049174 = "NA";
if  rs2255336 = "Unknown" then rs2255336 = "NA";
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
merge  results3 micadna;
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

/*Crude OR*/

PROC LOGISTIC data=dnasurvival;
where rs1049174 ne " ";
class rs1049174 (Ref= "CC");
MODEL CASE (EVENT='1')=  rs1049174;
contrast "trend in genotype" rs1049174 -1 0 1;
RUN;

/*Adjusted for Age and Sex*/

PROC LOGISTIC data=dnasurvival;
where rs1049174 ne " ";
class SEX rs1049174 (Ref= "CC");
MODEL CASE (EVENT='1')=  rs1049174 age SEX;
RUN;

/*Adjusted for age, sex and smoking status*/

proc freq data = dnasurvival;
tables stat;
run;

PROC LOGISTIC data=dnasurvival;
where rs1049174 ne " ";
class SEX rs1049174 (Ref= "CC") stat2 (ref="0");
MODEL CASE (EVENT='1')=  rs1049174 age SEX stat2;
RUN;

/*Crude OR*/

PROC LOGISTIC data=dnasurvival;
where rs2255336 ne " ";
class rs2255336(Ref= "CC");
MODEL CASE (EVENT='1')= rs2255336;
RUN;

/*Adjusted for Age and Sex*/

PROC LOGISTIC data=dnasurvival;
where rs2255336 ne " ";
class SEX rs2255336(Ref= "CC");
MODEL CASE (EVENT='1')= rs2255336 age SEX;
RUN;

/*Adjusted for age, sex and smoking status*/

proc freq data = dnasurvival;
tables case;
run;

PROC LOGISTIC data=dnasurvival;
where rs2255336 ne " ";
class SEX rs2255336(Ref= "CC") stat2 (ref="0");
MODEL CASE (EVENT='1')= rs2255336 age SEX stat2;
RUN;

/* Analysis stratified on smoking status*/

proc sort data = dnasurvival;
by stat;
run;

proc freq data = dnasurvival;
tables stat;
run;

data dnasurvivalmkII;
set dnasurvival;
run;

proc freq data=dnasurvivalmkII;
tables stat2;
run;

proc freq data = dnasurvivalmkII;
where case = 1;
tables stat2*rs1049174 stat2*rs2255336;
run;

proc freq data = dnasurvivalmkII;
where case = 2;
tables stat2*rs1049174 stat2*rs2255336;
run;

/*Adjusted for Age and Sex*/

PROC LOGISTIC data=dnasurvivalmkII;
where rs1049174 ne " " and stat2 = 0;
class SEX rs1049174 (Ref= "CC");
MODEL CASE (EVENT='1')=  rs1049174 age SEX;
RUN;

/*Adjusted for age, sex and pack years*/

PROC LOGISTIC data=dnasurvivalmkII;
where rs1049174 ne " " and stat2 = 1;
class SEX rs1049174 (Ref= "CC");
MODEL CASE (EVENT='1')=  rs1049174 age SEX PACK_YRS;
RUN;

/*Adjusted for Age and Sex*/

PROC LOGISTIC data=dnasurvivalmkII;
where rs2255336 ne " " and stat2 = 0;
class SEX rs2255336 (Ref= "CC");
MODEL CASE (EVENT='1')= rs2255336 age SEX;
RUN;

/*Adjusted for age, sex and pack years*/

PROC LOGISTIC data=dnasurvivalmkII;
where rs2255336 ne " " and stat2 = 1;
class SEX rs2255336 (Ref= "CC");
MODEL CASE (EVENT='1')= rs2255336 age SEX PACK_YRS;
RUN;


**********************************************************************************************************************************************************************************************************************************************************************************************************************************************************************

* Interaction between genotype and mica;

proc freq data = dnasurvival;
tables case;
run;

proc freq data = dnasurvival;
where rs1049174 = "CC";
tables micac*case;
run;

proc freq data = dnasurvival;
where rs1049174 = "CG";
tables micac*case;
run;

proc freq data = dnasurvival;
where rs1049174 = "GG";
tables micac*case;
run;

proc freq data = dnasurvival;
where rs2255336 = "CC";
tables micac*case;
run;

proc freq data = dnasurvival;
where rs2255336 = "CT";
tables micac*case;
run;

proc freq data = dnasurvival;
where rs2255336 = "TT";
tables micac*case;
run;

/*Crude OR*/

PROC LOGISTIC data=dnasurvival;
where rs1049174 ne " " and micac ne 0;
class rs1049174 (Ref= "CC") micac (Ref="1");
MODEL CASE (EVENT='1')=  rs1049174 micac;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs1049174 ne " " and micac ne 0;
class rs1049174 (Ref= "CC") micac (Ref="1");
MODEL CASE (EVENT='1')=  rs1049174 micac rs1049174*micac;
oddsratio rs1049174;
RUN;

/*Adjusted for Age and Sex*/

PROC LOGISTIC data=dnasurvival;
where rs1049174 ne " " and micac ne 0;
class sex rs1049174 (Ref= "CC") micac (Ref="1");
MODEL CASE (EVENT='1')=  rs1049174 micac rs1049174*micac age SEX;
oddsratio rs1049174;
RUN;

/*Adjusted for age, sex and smoking status*/

PROC LOGISTIC data=dnasurvival;
where rs1049174 ne " " and micac ne 0;
class sex rs1049174 (Ref= "CC") micac (Ref="1") stat (ref="1");
MODEL CASE (EVENT='1')=  rs1049174 micac rs1049174*micac age SEX stat;
oddsratio rs1049174;
RUN;

*Stratified analyses;

proc freq data = dnasurvival;
tables stat2;
run;

PROC LOGISTIC data=dnasurvival;
where rs1049174 = "CC";
class sex micac (Ref="1");
MODEL CASE (EVENT='1')=  micac age SEX;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs1049174 = "CG";
class sex micac (Ref="1");
MODEL CASE (EVENT='1')=  micac age SEX;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs1049174 = "GG";
class sex micac (Ref="1");
MODEL CASE (EVENT='1')=  micac age SEX;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs1049174 = "CC";
class sex micac (Ref="1") stat2 (ref="0");
MODEL CASE (EVENT='1')= micac age SEX stat2;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs1049174 = "CG";
class sex micac (Ref="1") stat2 (ref="0");
MODEL CASE (EVENT='1')= micac age SEX stat2;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs1049174 = "GG";
class sex micac (Ref="1") stat2 (ref="0");
MODEL CASE (EVENT='1')= micac age SEX stat2;
RUN;

/*Crude OR*/

PROC LOGISTIC data=dnasurvival;
where rs2255336 ne " " and micac ne 0;
class rs2255336 (Ref= "CC") micac (Ref="1");
MODEL CASE (EVENT='1')=  rs2255336 micac;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs2255336 ne " " and micac ne 0;
class rs2255336 (Ref= "CC") micac (Ref="1");
MODEL CASE (EVENT='1')=  rs2255336 micac rs2255336*micac;
oddsratio rs2255336;
RUN;

/*Adjusted for Age and Sex*/

PROC LOGISTIC data=dnasurvival;
where rs2255336 ne " " and micac ne 0;
class sex rs2255336 (Ref= "CC") micac (Ref="1");
MODEL CASE (EVENT='1')=  rs2255336 micac rs2255336*micac age SEX;
oddsratio rs2255336;
RUN;

/*Adjusted for age, sex and smoking status*/

PROC LOGISTIC data=dnasurvival;
where rs2255336 ne " " and micac ne 0;
class sex rs2255336 (Ref= "CC") micac (Ref="1") stat (ref="1");
MODEL CASE (EVENT='1')=  rs2255336 micac rs2255336*micac age SEX stat;
oddsratio rs2255336;
RUN;

*Stratified analyses;

proc freq data = dnasurvival;
tables rs2255336;
run;

PROC LOGISTIC data=dnasurvival;
where rs2255336 = "CC";
class sex micac (Ref="1");
MODEL CASE (EVENT='1')=  micac age SEX;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs2255336 = "CT";
class sex micac (Ref="1");
MODEL CASE (EVENT='1')=  micac age SEX;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs2255336 = "TT";
class sex micac (Ref="1");
MODEL CASE (EVENT='1')=  micac age SEX;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs2255336 = "CC";
class sex micac (Ref="1") stat2 (ref="0");
MODEL CASE (EVENT='1')= micac age SEX stat2;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs2255336 = "CT";
class sex micac (Ref="1") stat2 (ref="0");
MODEL CASE (EVENT='1')= micac age SEX stat2;
RUN;

PROC LOGISTIC data=dnasurvival;
where rs2255336 = "TT";
class sex micac (Ref="1") stat2 (ref="0");
MODEL CASE (EVENT='1')= micac age SEX stat2;
RUN;

proc print data = dnasurvival;
run;

proc contents data = dnasurvival;
run;

data dnasurvivalmk1;
set dnasurvival (keep= ID BDATE AGE SEX ETHNIC1 DEATHDTE CASE ELIG FCONDATE DX_DATE DXSOURCE BIOPSY CYTOLOGY DIABETES DIAB_AGE STAT PACK_YRS BIRTHDT EDUC GENDER panc mican micac stat2 rs1049174 rs2255336); 
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

proc export data= dnasurvivalmk1
   outfile= "C:\Users\onyea005\Desktop\WBOB\MICA Study\Preliminary Analysis\MICA_SNP_DATASET"
   dbms=XLSX
   replace;
run;


/* Survival analysis by genotyping data */

data dnasurvival;
set dnasurvival;
if id=168 then delete;
if case=1 and id le 6000 and deathdte ne .;
period=deathdte-dx_date;
if  period  lt 90 then do; per=0; per3=1; end;
else if period ge 90 then do ; per3=0; end;
run;

data dnasurvival;
set dnasurvival;
if  period  lt 120 then do; per5=1; end;
else if period ge 120 then do ; per5=0; end;
if  period  lt 277.5 then do; per0=1; end;
else if period ge 277.5 then do ; per0=0; end;
run;

data dnasurvival;
set dnasurvival;
if deathdte ne . and period lt 1000 then status2 = 1; else status2 = 0;
run;

/* ods graphics on;
proc lifetest data=Exposed plots=(survival(atrisk) logsurv);
   time Days*Status(0);
   strata Treatment;
run;
ods graphics off;
In the TIME statement, the survival time variable, Days, is crossed with the censoring variable, Status, with the value 0 indicating censoring. That is, the values of Days are considered censored if the corresponding values of Status are 0; otherwise, they are considered as event times. In the STRATA statement, the variable Treatment is specified, which indicates that the data are to be divided into strata based on the values of Treatment. ODS Graphics must be enabled before producing graphs. Two plots are requested through the PLOTS= optionóa plot of the survival curves with at risk numbers and a plot of the negative log of the survival curves*/

/*Modified censoring criteria*/

ODS Graphics on;
proc lifetest  data=dnasurvival method=life plots=(s) intervals = 0 to 180 by 20, 180 to 270 by 30, 270 to 1000 by 100;
where rs1049174 ne " ";
time period*status2(0);
strata rs1049174;
run;
ODS Graphics off;

ODS Graphics on;
proc lifetest  data=dnasurvival method=life plots=(s) intervals = 0 to 180 by 20, 180 to 270 by 30, 270 to 1000 by 100;
where rs2255336 ne " ";
time period*status2(0);
strata rs2255336;
run;
ODS Graphics off;


**********************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
**********************************************************************************************************************************************************************************************************************************************************************************************************************************************************************

/*Testing 2.0: Remaking the micac categories to match the poster values exactly*/

*Adjusted for age and sex;

PROC LOGISTIC data=results1;
class SEX  micac (ref='1');
MODEL CASE (EVENT='1')=   micac age2  SEX;
RUN;

/*Adjusted for Age, sex, smoking status and pack years*/

PROC LOGISTIC data =results1;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')=   age2  SEX  stat micac PACK_YRS;
RUN;

/* Interaction with sex*/

proc freq data = results1;
where SEX='M';
tables Case*micac;
run;

proc freq data = results1;
where SEX='F';
tables Case*micac;
run;

proc freq data = results1;
tables MICAC*SEX;
run;

PROC LOGISTIC data =results1;
class SEX   stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')=   age2  SEX  stat micac  PACK_YRS  SEX*MICAC;
oddsratio micac;
RUN;

PROC LOGISTIC data =results1;
WHERE SEX='M';
class SEX  stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')=   age2  SEX  stat micac  PACK_YRS;
RUN;

PROC LOGISTIC data =results1;
WHERE SEX='F';
class SEX  micac (ref='1') stat (ref='1') stat micac (ref='1');
MODEL CASE (EVENT='1')=   age2  SEX  stat micac  PACK_YRS;
RUN;

/*Interaction with Age*/

proc freq data = results1;
where age lt 70;
tables Case*micac;
run;

proc freq data = results1;
where age ge 70;
tables Case*micac;
run;

proc freq data = results1;
tables Case*agecat;
run;

proc freq data = results1;
tables MICAC*agecat;
run;

PROC LOGISTIC data = results1;
class SEX  stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')=   age2 SEX  stat micac PACK_YRS age2*micac;
oddsratio micac;
RUN;

PROC LOGISTIC data = results1;
WHERE age2 lt 70;
class SEX  stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')=    SEX  stat micac  PACK_YRS;
RUN;

PROC LOGISTIC data = results1;
WHERE age2 ge 70;
class SEX  stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')=    SEX  stat micac  PACK_YRS mica*gender;
RUN;

/*4 categories for interaction graph for gender*/
proc freq data = results1;
tables micac sex;
run;

data results1;
set results1;
if sex = "F" and micac = 1 then intergender1 = 1; else intergender1 = 0;
if sex = "F" and micac = 3 then intergender2 = 1; else intergender2 = 0;
if sex = "M" and micac = 1 then intergender3 = 1; else intergender3 = 0;
if sex = "M" and micac = 3 then intergender4 = 1; else intergender4 = 0;
run;

PROC LOGISTIC data =results1;
class intergender1 (ref='0') intergender2 (ref='0') intergender3 (ref='0') intergender4 (ref='0') stat (ref='1') stat;
MODEL CASE (EVENT='1')= intergender1 intergender2 intergender3 intergender4 age2 stat PACK_YRS;
RUN;

/*4 categories for interaction graph for age*/
data results1;
set results1;
if age2 lt 70 and micac = 1 then interage1 = 1; else interage1 = 0;
if age2 lt 70 and micac = 3 then interage2 = 1; else interage2 = 0;
if age2 ge 70 and micac = 1 then interage3 = 1; else interage3 = 0;
if age2 ge 70 and micac = 3 then interage4 = 1; else interage4 = 0;
run;

PROC LOGISTIC data =results1;
class SEX interage1 (ref='0') interage2 (ref='0') interage3 (ref='0') interage4 (ref='0') stat (ref='1') stat;
MODEL CASE (EVENT='1')= interage1 interage2 interage3 interage4 SEX stat PACK_YRS;
RUN;

/*Logistic Regression V3 (do ND vs D MICA levels, then Detectable leves in tertiles*/

data results1;
set results1;
if micac = 0 then micad = 0; else micad = 1;
run;

proc freq data = results1;
tables micad;
run;

PROC LOGISTIC data=results1;
class SEX  micad (ref = '0');
MODEL CASE (EVENT='1')= micad age2 SEX;
RUN;

PROC LOGISTIC data =results1;
class SEX   stat (ref='1') micad (ref = '0');
MODEL CASE (EVENT='1')=   age2  SEX  stat micad  PACK_YRS ;
RUN;

/*Testing Things*/

PROC LOGISTIC data =results1;
class SEX   stat (ref='1') micad alco (ref="0");
MODEL CASE (EVENT='1')=   age2  SEX  stat micad  PACK_YRS alco diab;
RUN;

/*Resuming normal analysis*/

PROC LOGISTIC data=results1;
where micac ne 0;
class SEX  micac (ref='1');
MODEL CASE (EVENT='1')=   micac age2 SEX;
RUN;

PROC LOGISTIC data =results1;
where micac ne 0;
class SEX   stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')=   age2  SEX  stat micac  PACK_YRS ;
RUN;

/*Testing Things*/

PROC LOGISTIC data =results1;
where micac ne 0;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0') diab;
MODEL CASE (EVENT='1')=   age2  SEX  stat micac  PACK_YRS alco diab;
RUN;

/*Resuming Analysis*/

/*Interaction Analysis Adjusted for smoking status and pack years*/

PROC LOGISTIC data =results1;
where sex = 'M' and age2 lt 70;
class stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')=   age2  stat micac PACK_YRS;
RUN;

PROC LOGISTIC data =results1;
where sex = 'M' and age2 ge 70;
class stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')=   age2  stat micac PACK_YRS;
RUN;

PROC LOGISTIC data =results1;
where sex = 'F' and age2 lt 70;
class stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')=   age2  stat micac PACK_YRS;
RUN;

PROC LOGISTIC data =results1;
where sex = 'F' and age2 ge 70;
class stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')=   age2  stat micac PACK_YRS;
RUN;

data results1;
set results1;
if sex = 'M' and age2 lt 70 then strata=1;
if sex = 'M' and age2 ge 70 then strata=2;
if sex = 'F' and age2 lt 70 then strata=3;
if sex = 'F' and age2 ge 70 then strata=4;
run;

proc freq data = results1;
table strata;
run;

PROC LOGISTIC data =results1;
class micac (ref='1') strata (ref='4');
MODEL CASE (EVENT='1')= micac strata;
RUN;

PROC LOGISTIC data =results1;
class micac (ref='1') strata (ref='4');
MODEL CASE (EVENT='1')= micac strata micac*strata;
oddsratio micac;
RUN;

PROC LOGISTIC data =results1;
where micac ne 0;
class micac (ref='1') strata (ref='4');
MODEL CASE (EVENT='1')= micac strata;
RUN;

PROC LOGISTIC data =results1;
where micac ne 0;
class micac (ref='1') strata (ref='4');
MODEL CASE (EVENT='1')= micac strata micac*strata;
oddsratio micac;
RUN;

/*Testin interaction with smoking*/

PROC LOGISTIC data =results1;
where micac ne 0;
class micac (ref='1') stat (ref='1');
MODEL CASE (EVENT='1')= micac stat micac*stat;
oddsratio micac;
RUN;

/* Auditing the case control status between my dataset and the genotyping dataset */

PROC IMPORT OUT= micadnamkII DATAFILE= "C:\Users\onyea005\Desktop\WBOB\MICA Study\Preliminary Analysis\NKG2D_genotype" DBMS=xlsx REPLACE;
     SHEET="MICA DNA"; 
     GETNAMES=YES;
RUN;

proc print data = micadnamkII (obs=50);
run;

data micadnamkII;
set micadnamkII;
keep ID Sample_Name rs1049174 rs2255336 case;
if ID = "." then delete;
rename case = casemkI;
run;

proc print data = micadnamkII (obs=50);
run;

/* Printing out the dupliccates samples from the genotyping data */

proc print data = micadnamkII (obs=50);
run;

PROC FREQ;
 TABLES id / noprint out=keylist;
RUN;

PROC PRINT data=keylist;
 WHERE count ge 2;
RUN; 

proc print data = micadnamkII;
where id = 5067 or 5097 or 5098 or 5101 or 5112 or 5144 or 5150; 
run;

/*Case Control Status Audit*/

proc sort data = results3;
by id;
run;

proc sort data = micadnamkII;
by id;
run;

data dnasurvivalmkI;
merge  results3 micadnamkII;
by id;
run;

proc freq data = dnasurvivalmkI;
tables case*casemkI;
run;

proc contents data = dnasurvivalmkI;
run;

/*Data audit for UMGC samples */

PROC IMPORT OUT= work.micaumgc DATAFILE= "C:\Users\onyea005\Desktop\MICA Study\Preliminary Analysis\Complete Manifest_June2015_Panc.xlsx" DBMS=xlsx REPLACE;
     SHEET="Box Order"; 
     GETNAMES=YES;
RUN;

proc print data = micaumgc (obs=50);
run;

proc print data = results3 (obs=50);
run;

proc sort data = results3;
by id;
run;

proc sort data = micaumgc;
by id;
run;

data results3;
set results3;
drop CA;
run;

data micaumgcmkI;
merge  results3 micaumgc;
by id;
run;

proc print data = micaumgcmkI (obs=50);
run;

data micaumgcmkII;
set micaumgcmkI;
keep id MICA AGE SEX ETHNIC1 ETHNIC2 DEATHDTE ICD0 ICD9 CASE ELIG QC STATUS REFUSE CTLRDATE INTRVID FCDATE SURVEY SURVEYDT BLOOD URINE FFQ COMMSTAT FCONDATE DX_DATE CONT_AFF MD HOSP_CLN FU2WKDTE MD_CONT LETTER CASE_COM DATE1 COMMENT1 DATE2 COMMENT2 DATE3 COMMENT3 DXSOURCE BIOPSY CYTOLOGY AUTOPSY Age2 Blood_Urine Blood_Survey Blood_FFQ FFQ_Survey FFQ_Urine Urine_Survey Blood_Urine_FFQ Blood_Urine_Survey Blood_FFQ_Survey Urine_FFQ_Survey Blood_Urine_FFQ_Survey first last DIABETES DIAB_AGE INSULIN GALLSTN PANTITIS ANEMIA TONSILE TONSL_YR GALLBLAD GALLB_YR ULCERSUR ULCER_YR ABDOM_SU ABDOM_YR Y1 Y2 STAT SM2A SM2B SM2C SM2D SM2E SM2F YR_STOP1 YEAR1 PACK1 P_YEAR1 YR_STOP2 YEAR2 PACK2 P_YEAR2 YR_STOP3 YEAR3 PACK3 P_YEAR3 YR_STOP4 YEAR4 PACK4 P_YEAR4 YR_STOP5 YEAR5 PACK5 P_YEAR5 YR_STOP6 YEAR6 PACK6 P_YEAR6 PACK_YRS PACKYR_C LASTCIG LASTYR LASTYTOT PA_YES ASLEEPHR AWAKEHR AWAKEACT LIGHTACT MODERACT HEAVYACT SWEATY INCOME35 INCOMEHI INCOMELO NUMHOUSE SSN_YES MNLINC MEDICARE GSTM1 GSTT1 NAT2_GEN NAT1_GEN CYP1A1_G NAT2_P CYP1A2 BIRTHDT EDUC ETHNIC MARRIED GENDER FAT01 FAT02 SUMRE SUMME CAFFEINE SUMLIQ SUMCOF SUMTEA PROMME SUMFR SUMVE SUMFRVE SUMCRUVE SUMFISH WHTMEAT CALOR PROT AFAT VFAT CARBO CRUDE DTFIB panc mican exc exc1 race micac micac2 ever_sm alco AGEc diab agecat intergender1 intergender2 intergender3 intergender4 interage1 interage2 interage3 interage4 lag micad strata micaroc caseroc Order Sample_Type Lab_ID Current_Vol Original_Vol Unit Location Box Concentration K L M N O P Q R S T Vol__ul_ Total_ng ul_for_500_ng ul_left_after_500ng;
run;
 
data micaumgcmkII;
set micaumgcmkII;
if id = . then delete;
if Concentration = . then delete;
run;

proc print data = micaumgcmkII (obs=50);
run;

data micaumgcmkIII;
set micaumgcmkII;
if id in (471 885 1556 579 654 814 1138 1105 759 455 1026 928 1073 998 727 931 883 1058 1115 859 835 1027 793 795 793 1118 928 1026 889 902 1207 989 889 1441 839);
run;

proc print data = micaumgcmkIII;
run;

data micaumgcmkIII;
set micaumgcmkIII;
keep id MICA AGE SEX ETHNIC1 CASE ELIG Age2 Blood_Urine Blood_Survey Blood_FFQ FFQ_Survey FFQ_Urine Urine_Survey Blood_Urine_FFQ Blood_Urine_Survey Blood_FFQ_Survey Urine_FFQ_Survey Blood_Urine_FFQ_Survey BIRTHDT EDUC ETHNIC MARRIED GENDER FAT01 FAT02 SUMRE SUMME CAFFEINE SUMLIQ SUMCOF SUMTEA PROMME SUMFR SUMVE SUMFRVE SUMCRUVE SUMFISH WHTMEAT CALOR PROT AFAT VFAT CARBO CRUDE DTFIB panc mican exc exc1 race micac micac2 ever_sm alco AGEc diab agecat Order Sample_Type Lab_ID Current_Vol Original_Vol Unit Location Box Concentration K L M N O P Q R S T Vol__ul_ Total_ng ul_for_500_ng ul_left_after_500ng; 
run;

proc freq data = micaumgcmkIII;
tables case;
run;

proc export data= micaumgcmkIII
   outfile= "C:\Users\onyea005\Desktop\MICA Study\Preliminary Analysis\MICA_UMGC_CASE_AUDIT"
   dbms=XLSX
   replace;
run;

* Making a new dataset to rerun analyses for manuscript revisions;

proc print data = dnasurvival (obs=50);
run;

data mylib.micafinal;
set dnasurvival;
keep id panc AGE SEX GENDER ETHNIC1 EDUC BDATE BIRTHDT DEATHDTE CASE ELIG FCONDATE DX_DATE DXSOURCE BIOPSY CYTOLOGY STAT PACK_YRS ever_sm DIABETES DIAB_AGE diab MICA mican Sample_Name rs1049174 rs2255336; 
run;

proc print data = micafinal;
run;


*****************************************************************************************************************************************************************************************************************************;

proc contents data = results1;
run;

proc contents data = micadnamkII;
run;

proc sort data = results1;
by id;
run;

proc sort data = micadnamkII;
by id;
run;

data mylib.micafinalmk1;
merge  results1 micadnamkII;
by id;
run;

*Manuscript Revisions;

options ps = 3000 ls = 120 nofmterr;
*options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '           ';
*options ps = 3000 ls = 120;
libname mylib 'C:\Users\onyea005\Desktop\WBOB\MICA Study\Preliminary Analysis';
*%inc 'C:\Documents and Settings\prizm001\Desktop\format.sas'; 

data micafinalmk1;
set  mylib.micafinalmk1;
run;

proc contents data = micafinalmk1;
run;

*The missing cases come from the participants who had genetic SNP data but no MICA level measured; 
proc print data = micafinalmk1;
where case = .;
run;

proc freq data = micafinalmk1;
tables STAT DIABETES case*DIABETES;
run;

*recoding the diabetes variable for manuscript revisions;
data micafinalmk1;
set micafinalmk1;
if DIABETES = 3 then DIAB = 0; else DIAB =1;
run;

/*Testing 2.0: Remaking the micac categories to match the poster values exactly*/

*Adjusted for age and sex;

PROC LOGISTIC data= micafinalmk1;
class SEX  micac (ref='1');
MODEL CASE (EVENT='1')=   micac age  SEX;
RUN;

/*Adjusted for Age, sex, smoking status and pack years*/

PROC LOGISTIC data = micafinalmk1;
where STAT ne 4;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')=   age  SEX  stat micac PACK_YRS alco ;
RUN;

/*Full model with diabetes*/

PROC LOGISTIC data = micafinalmk1;
where STAT ne 4;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac DIABETES alco;
RUN;

/* Interaction with sex*/

proc freq data = micafinalmk1;
where SEX='M';
tables Case*micac;
run;

proc freq data = micafinalmk1;
where SEX='F';
tables Case*micac;
run;

proc freq data = micafinalmk1;
tables Case*micac;
run;

proc freq data = micafinalmk1;
tables MICAC*SEX;
run;

PROC LOGISTIC data = micafinalmk1;
where STAT ne 4;
class SEX   stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')=   age  SEX  stat micac  PACK_YRS SEX*MICAC;
oddsratio micac;
RUN;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='M' and STAT ne 4;
class SEX  stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')=   age  SEX  stat micac PACK_YRS;
RUN;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='F' and STAT ne 4;
class SEX  micac (ref='1') stat (ref='1') stat micac (ref='1');
MODEL CASE (EVENT='1')=   age  SEX  stat micac  PACK_YRS;
RUN;

/*Interaction with Age*/

proc freq data = micafinalmk1;
where age lt 70;
tables Case*micac;
run;

proc freq data = micafinalmk1;
where age ge 70;
tables Case*micac;
run;

proc freq data = micafinalmk1;
tables Case*agecat;
run;

proc freq data = micafinalmk1;
tables MICAC*agecat;
run;

PROC LOGISTIC data = micafinalmk1;
where STAT ne 4;
class SEX  stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')=   age SEX  stat micac PACK_YRS age*micac;
oddsratio micac;
RUN;

PROC LOGISTIC data = micafinalmk1;
WHERE age lt 70 and STAT ne 4;
class SEX  stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')= SEX  stat micac  PACK_YRS;
RUN;

PROC LOGISTIC data = micafinalmk1;
WHERE age ge 70 and STAT ne 4;
class SEX  stat (ref='1') micac (ref='1') ;
MODEL CASE (EVENT='1')=  SEX  stat micac  PACK_YRS;
RUN;

*full model with diabetes;
PROC LOGISTIC data = micafinalmk1;
where STAT ne 4;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac DIABETES alco;
RUN;

*Interaction with Diabetes;
PROC LOGISTIC data = micafinalmk1;
where STAT ne 4 and DIABETES ne 8;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0') DIABETES (ref='3');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS alco micac DIABETES micac*DIABETES;
oddsratio micac;
RUN;

*Stratified analysis among diabetic participants;

*Diabetics (include long term & recent diabetics);
PROC LOGISTIC data = micafinalmk1;
where STAT ne 4 and DIABETES ne 3;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac alco;
RUN;

*Non Diabetics;
PROC LOGISTIC data = micafinalmk1;
where STAT ne 4 and DIABETES = 3;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac alco;
RUN;

*Final non-diabetic model;
PROC LOGISTIC data = micafinalmk1;
where STAT ne 4 and DIABETES = 3;
class SEX   stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac;
RUN;

*repeating sex significant analyses after adjusting for diabetes;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='M' and STAT ne 4;
class  stat (ref='1') micac (ref='1') DIABETES (ref='3');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS DIABETES;
RUN;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='F' and STAT ne 4;
class SEX  micac (ref='1') stat (ref='1') stat DIABETES (ref='3');
MODEL CASE (EVENT='1')=   age  SEX  stat micac  PACK_YRS DIABETES;
RUN;

*recoding the DIABETES variable to help with model convergence issues;

PROC LOGISTIC data = micafinalmk1;
WHERE STAT ne 4;
class SEX stat (ref='1') micac (ref='1') DIAB (ref='0');
MODEL CASE (EVENT='1')= age sex stat micac PACK_YRS DIAB micac*sex;
RUN;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='M' and STAT ne 4;
class  stat (ref='1') micac (ref='1') DIAB (ref='0');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS DIAB;
RUN;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='F' and STAT ne 4;
class micac (ref='1') stat (ref='1') stat DIAB (ref='0');
MODEL CASE (EVENT='1')= age stat micac  PACK_YRS DIAB;
RUN;

**repeating sex stratified analyses among diabetics;
*We do not have a large enough sample to report results from the gender stratified analyis among diabetic participants;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='M' and STAT ne 4 and DIAB = 1;
class SEX  stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='F' and STAT ne 4 and DIAB = 1;
class SEX  stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

*repeating sex stratified analyses among non diabetics;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='M' and STAT ne 4 and DIAB = 0;
class SEX  stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='F' and STAT ne 4 and DIAB = 0;
class SEX  stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

*Updating manuscript tables with analysis results;

proc freq data=micafinalmk1;
tables diab diab*case/chisq;
run;

proc freq data=micafinalmk1;
tables micac micac*case;
run;

proc  print data = micafinalmk1;
var mican micac case;
run;

*The mica final dataset matches the manuscript exclusion criteria so use these numbers to update N in your tables;

data micafinal;
set  mylib.micafinal;
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
else if 2 lt mican le  32.10 then micac=1;
else if 32.14 le mican le  64.7  then micac=2;
else if  mican ge 64.8 then micac=3; 
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
PROC LOGISTIC data = micafinalmk1;
where STAT ne 4;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac DIABETES alco;
RUN;

*Interaction with Diabetes;
PROC LOGISTIC data = micafinalmk1;
where STAT ne 4 and DIABETES ne 8;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0') DIABETES (ref='3');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS alco micac DIABETES micac*DIABETES;
oddsratio micac;
RUN;

*Stratified analysis among diabetic participants;

*Diabetics (include long term & recent diabetics);
PROC LOGISTIC data = micafinalmk1;
where STAT ne 4 and DIABETES ne 3;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac alco;
RUN;

*Non Diabetics;
PROC LOGISTIC data = micafinalmk1;
where STAT ne 4 and DIABETES = 3;
class SEX   stat (ref='1') micac (ref='1') alco (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac alco;
RUN;

*Final non-diabetic model;
PROC LOGISTIC data = micafinalmk1;
where STAT ne 4 and DIABETES = 3;
class SEX   stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS micac;
RUN;

*Interaction with sex;
PROC LOGISTIC data = micafinalmk1;
where STAT ne 4;
class SEX (ref='M') stat (ref='1') micac (ref='1') alco (ref='0') DIAB (ref='0');
MODEL CASE (EVENT='1')= age SEX stat PACK_YRS alco micac DIAB micac*sex;
oddsratio micac;
RUN;

PROC LOGISTIC data = micafinalmk1;
where STAT ne 4 and SEX = 'M';
class stat (ref='1') micac (ref='1') alco (ref='0') DIAB (ref='0');
MODEL CASE (EVENT='1')= age stat PACK_YRS alco micac DIAB ;
oddsratio micac;
RUN;

PROC LOGISTIC data = micafinalmk1;
where STAT ne 4 and SEX = 'F';
class stat (ref='1') micac (ref='1') alco (ref='0') DIAB (ref='0');
MODEL CASE (EVENT='1')= age stat PACK_YRS alco micac DIAB ;
oddsratio micac;
RUN;

*Stratified Models among non-diabetics;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='M' and STAT ne 4 and DIAB = 0;
class SEX  stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

PROC LOGISTIC data = micafinalmk1;
WHERE SEX='F' and STAT ne 4 and DIAB = 0;
class SEX  stat (ref='1') micac (ref='1');
MODEL CASE (EVENT='1')= age stat micac PACK_YRS;
RUN;

PROC LOGISTIC data = micafinalmk1;
WHERE STAT ne 4 and DIAB = 0;
class SEX  stat (ref='1') micac (ref='1') alco(ref='0');
MODEL CASE (EVENT='1')= age SEX stat micac PACK_YRS alco micac*sex;
RUN;

*average MICA levels;

proc means data = micafinalmk1 n mean range;
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
