PROC IMPORT OUT= work.asmicgutcovariates DATAFILE= "D:\Professional Folder\WBOB\ASMIC Study\15_ASMIC Fall 2019 Update\1_ASMIC Datasets\5_Aspirin_Gut_Covariates.xlsx" DBMS=xlsx REPLACE;
  SHEET="Sheet1"; 
  GETNAMES=YES;
RUN;

proc print data = asmicgutcovariates;
run;

data asmicgutcovariates;
set asmicgutcovariates (drop = BarcodeSequence LinkerPrimerSequence Description) ;
GUTID = _SampleID;
run;

proc print data = asmicgutcovariates;
run;

proc contents data = asmicgutcovariates;
run;

PROC IMPORT OUT= work.asmicoralcovariates DATAFILE= "D:\Professional Folder\WBOB\ASMIC Study\15_ASMIC Fall 2019 Update\1_ASMIC Datasets\5_Aspirin_Oral_Covariates.xlsx" DBMS=xlsx REPLACE;
  SHEET="ASMIC Oral Sample ID key"; 
  GETNAMES=YES;
RUN;

proc print data = asmicoralcovariates;
run;

data asmicoralcovariates;
set asmicoralcovariates (drop = BarcodeSequence LinkerPrimerSequence Description) ;
OralID = _SampleID;
run;

proc print data = asmicoralcovariates;
run;

proc contents data = asmicoralcovariates;
run;

proc sort data = asmicgutcovariates; by collection; run;

data asmicgutcovariates;
set asmicgutcovariates;
if Collection = 2 then delete;
if Collection = 4 then delete;
if Collection = 5 then delete;
run;

proc print data = asmicgutcovariates;
run;

proc sort data = asmicgutcovariates; by Subject_ID; run;
proc sort data = asmicoralcovariates; by Subject_ID; run;

data asmicsubjectid;
merge asmicgutcovariates asmicoralcovariates;
by Subject_ID; 
run;

proc print data = asmicsubjectid;
run;

PROC IMPORT OUT= work.asmicmic1 DATAFILE= "D:\Professional Folder\WBOB\ASMIC Study\15_ASMIC Fall 2019 Update\1_ASMIC Datasets\ASMIC_MIC1.xlsx" DBMS=xlsx REPLACE;
  SHEET="To Be Tested"; 
  GETNAMES=YES;
RUN;

proc print data = asmicmic1;
run;

proc contents data = asmicmic1;
run;

data asmicmic1;
set asmicmic1;
OralID = ID;
if Tube = 3 then delete;
run;

*We only have 99 observations at Visit 2 because we are missing BK402902 but we have BK402903. Somthing to investigate furhter;

proc sort data = asmicsubjectID; by OralID; run;
proc sort data = asmicmic1; by OralID; run;

data asmicardl;
merge asmicsubjectID asmicmic1;
by OralID;
run;

proc print data = asmicardl;
run;

*I have to use the correct treatment assignment!!;

PROC IMPORT OUT= work.asmictxt DATAFILE= "D:\Professional Folder\WBOB\ASMIC Study\15_ASMIC Fall 2019 Update\1_ASMIC Datasets\7_Aspirin_Gut_Txt.xlsx" DBMS=xlsx REPLACE;
  SHEET="7_Aspirin_Gut_Txt"; 
  GETNAMES=YES;
RUN;

proc print data = asmictxt;
run;
 
data asmictxt;
set asmictxt (keep = Subject_ID Collection SampleID Contents);
run;
 
proc sort data = asmictxt; by SampleID; run;
proc sort data = asmicardl; by SampleID; run;

data asmicardl2;
merge asmicardl asmictxt;
by sampleID;
run;

data asmicardl2;
set asmicardl2;
if OralID = . then delete;
run;

proc freq data = asmicardl2;
tables Contents;
run;

proc print data = asmicardl2;
run;

proc univariate data = asmicardl2;
var GDF_15;
histogram;
run;

proc rank data = asmicardl2 groups = 3 out=asmicardl2;
var GDF_15;
ranks GDF_tert;
run;

proc rank data = asmicardl2 groups = 4 out=asmicardl2;
var GDF_15;
ranks GDF_quart;
run;

proc rank data = asmicardl2 groups = 5 out=asmicardl2;
var GDF_15;
ranks GDF_quint;
run;

proc univariate NORMAL PLOT data=asmicardl2; 
var GDF_15;
histogram GDF_15/normal;
run;

data asmicardl2;
set asmicardl2
GDF_std = GDF_15/549.6433;
GDF_z = (GDF_15 - 615.31190) / 549.6433;
run; 
 
***************************************************************************************************************************************************************************;

*Testing regression models;

*Use Anova to test the change in mean GDF_15 between the aspirin and placebo group at visit 3;

proc glm data = asmicardl2;
where Tube = 2 and collection = 1 ;
class contents;
model GDF_15 = contents;
run;

proc glm data = asmicardl2;
where Tube = 2 and collection = 3;
class contents;
model GDF_15 = contents;
run;

proc glm data = asmicardl2;
where Tube = 2;
class contents collection;
model GDF_15 = contents collection contents*collection;
run;

proc anova data = asmicardl2;
where Tube = 2 and collection = 3;
class contents;
model GDF_15 = contents;
run;

proc anova data = asmicardl2;
where Tube = 2;
class contents collection;
model GDF_15 = contents collection contents*collection;
run;

proc anova data = asmicardl2;
where tube = 2 and contents = "Aspirin";
class collection;
model GDF_15 = collection;
run;

proc anova data = asmicardl2;
where tube = 2 and contents = "Placebo";
class collection;
model GDF_15 = collection;
run;

*Using polytomous regression for categories of GDF as an outcome;

proc logistic data = asmicardl2;
where Tube = 2 and collection = 3;
class contents GDF_tert;
model GDF_tert = contents;
run;

proc logistic data = asmicardl2;
where Tube = 2 and collection = 3;
class contents GDF_tert;
model GDF_quart = contents;
run;

proc logistic data = asmicardl2;
where Tube = 2 and collection = 3;
class contents GDF_tert;
model GDF_quint = contents;
run;

*Log transforming the outcome;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3;
class contents;
model GDF_15 = contents / dist = poisson link = log;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3;
class contents;
model GDF_15 = contents / dist = normal link = log;
run;

data asmicardl2; 
set asmicardl2;
bmi = bmi_v1 + 0;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3;
class Contents gender;
model GDF_15 = Contents age_today gender bmi / dist = normal link = log;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3;
class Contents;
model GDF_15 = Contents / dist = normal link = log;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin";
class Collection;
model GDF_15 = Collection / dist = normal link = log;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents;
model GDF_15 = Collection Contents Contents*Collection / dist = normal link = log;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents;
model GDF_15 = Collection Contents;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents;
model GDF_15 = Collection Contents / dist = normal link = log;
run;

data asmicardl2;
set asmicardl2;
logGDF = log(GDF_15);
run;

proc univariate data = asmicardl2;
var logGDF;
histogram;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents;
model logGDF = Collection Contents;
run;

***************************************************************************************************************************************************************************;

*1_test association at visit 3;
*2_test association over time in the aspirin arm;
*3_testing the association adjusted for baseline (covariate adjustment, difference as an outcome, adjusted for baseline);

proc print data = asmicardl2;
run;

data asmicardl2;
set asmicardl2;
logGDF = log(GDF_15);
run;

proc univariate data = asmicardl2;
var logGDF;
histogram;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3;
class Contents gender;
model GDF_15 = Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3;
class Contents gender;
model logGDF = Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3;
class Contents gender;
model GDF_15 = Contents / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin";
class Collection gender;
model GDF_15 = Collection;
lsmeans Collection / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin";
class Collection gender;
model logGDF = Collection;
lsmeans Collection / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin";
class Collection gender;
model GDF_15 = Collection / dist=normal link=log;
lsmeans Collection / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents gender;
model GDF_15 = Collection Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents gender;
model logGDF = Collection Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents gender;
model GDF_15 = Collection Contents / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

*transposing the MIC1 Values;

/*
The VAR statement can be used to specify the variables that are to be transposed (DEPOSIT). An ID statement
specifies a variable (MONTH) whose value controls the variables to which values of DEPOSIT are assigned. The
variable _NAME_ is not needed as is dropped using a data set option.
*/

proc print data = asmicardl2;
run;

Proc sort data = asmicardl2;
by Subject_ID;
run;

proc transpose data = asmicardl2 out=asmicardl2t (drop=_name_ _label_) prefix = GDF;
var GDF_15;
by Subject_ID;
ID Collection;
run;

proc print data = asmicardl2t;
run;

*I am deleting the visit 1 since that information is already transposed in the datasets, and I do not want to duplicate entries (i.e. one data file witl 50 observations);
*Keep the OralID and Gut ID for future merging;

proc print data = asmicardl2;
run;

data asmicardl2diff;
set asmicardl2;
keep Subject_ID GUTID OralID Collection Treatment age_today gender weight_v1 height_v1 bmi_v1 Contents;
if Collection = 1 then delete;
run;

proc print data = asmicardl2diff;
run;

proc sort data = asmicardl2diff; by Subject_ID; run;
proc sort data = asmicardl2t; by Subject_ID; run;

data asmicardl2change;
merge asmicardl2diff asmicardl2t;
by Subject_ID;
run;

proc print data = asmicardl2change;
run;

data asmicardl2change;
set asmicardl2change;
GDFdelta = GDF3 - GDF1;
logGDF3 = log(GDF3);
logGDF1 = log(GDF1);
logGDFdelta = log(GDF3) - log(GDF1);
run;

proc genmod data = asmicardl2change;
class Contents gender;
model GDF3 = Contents GDF1;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
class Contents gender;
model logGDF3 = Contents logGDF1;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
class Contents gender;
model GDF3 = Contents GDF1 / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
class Contents gender;
model GDFdelta = Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
class Contents gender;
model logGDFdelta = Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
class Contents gender;
model GDFdelta = Contents / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc export data= asmicardl2
 outfile= "D:\Professional Folder\WBOB\ASMIC Study\15_ASMIC Fall 2019 Update\1_ASMIC Datasets\MIC1DATA"
 dbms=XLSX
 replace;
run;

proc export data= asmicardl2change
 outfile= "D:\Professional Folder\WBOB\ASMIC Study\15_ASMIC Fall 2019 Update\1_ASMIC Datasets\MIC1DATATRANSPOSED"
 dbms=XLSX
 replace;
run;

*Calculating the IQR to delete outlier values and retest the association;
proc means data = asmicardl2 mean p25 p75 ;
var GDF_15;
run;

*The upper IQR value is 988;

proc print data = asmicardl2;
where GDF_15 gt 988;
run;

data asmicardl3;
set asmicardl2;
if tube = 3 then delete;
if GDF_15 gt 988 then delete;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3;
class Contents gender;
model GDF_15 = Contents;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3;
class Contents gender;
model logGDF = Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3;
class Contents gender;
model GDF_15 = Contents / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3 and gender = "F";
class Contents gender;
model GDF_15 = Contents;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3 and gender = "F";
class Contents gender;
model logGDF = Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3 and gender = "F";
class Contents gender;
model GDF_15 = Contents / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3 and gender = "M";
class Contents gender;
model GDF_15 = Contents;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3 and gender = "M";
class Contents gender;
model logGDF = Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3 and gender = "M";
class Contents gender;
model GDF_15 = Contents / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin";
class Collection gender Subject_ID;
model GDF_15 = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin";
class Collection gender Subject_ID;
model logGDF = Collection;
lsmeans Collection / corr exp diff cl;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin";
class Collection gender Subject_ID;
model GDF_15 = Collection / dist=normal link=log;
lsmeans Collection / corr exp diff cl;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin" and gender = "F";
class Collection gender Subject_ID;
model GDF_15 = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin" and gender = "F";
class Collection gender Subject_ID;
model logGDF = Collection;
lsmeans Collection / corr exp diff cl;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin" and gender = "F";
class Collection gender Subject_ID;
model GDF_15 = Collection / dist=normal link=log;
lsmeans Collection / corr exp diff cl;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin" and gender = "M";
class Collection gender Subject_ID;
model GDF_15 = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin" and gender = "M";
class Collection gender Subject_ID;
model logGDF = Collection;
lsmeans Collection / corr exp diff cl;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2 and Contents = "Aspirin" and gender = "M";
class Collection gender Subject_ID;
model GDF_15 = Collection / dist=normal link=log;
lsmeans Collection / corr exp diff cl;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents gender;
model GDF_15 = Collection Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents gender;
model logGDF = Collection Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents gender;
model GDF_15 = Collection Contents / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and gender = "F";
class Collection Contents gender;
model GDF_15 = Collection Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and gender = "F";
class Collection Contents gender;
model logGDF = Collection Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and gender = "M";
class Collection Contents gender;
model GDF_15 = Collection Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and gender = "M";
class Collection Contents gender;
model logGDF = Collection Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and gender = "F";
class Collection Contents gender;
model GDF_15 = Collection Contents age_today;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and gender = "F";
class Collection Contents gender;
model logGDF = Collection Contents age_today;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and gender = "M";
class Collection Contents gender;
model GDF_15 = Collection Contents age_today;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2;
where tube = 2 and gender = "M";
class Collection Contents gender;
model logGDF = Collection Contents age_today;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
class Contents gender;
model GDF3 = Contents GDF1;
run;

proc genmod data = asmicardl2change;
class Contents gender;
model logGDF3 = Contents logGDF1;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
class Contents gender;
model GDF3 = Contents GDF1 / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
where gender = "F";
class Contents gender;
model GDF3 = Contents GDF1;
run;

proc genmod data = asmicardl2change;
where gender = "F";
class Contents gender;
model logGDF3 = Contents logGDF1;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
where gender = "F";
class Contents gender;
model GDF3 = Contents GDF1 / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
where gender = "M";
class Contents gender;
model GDF3 = Contents GDF1;
run;

proc genmod data = asmicardl2change;
where gender = "M";
class Contents gender;
model logGDF3 = Contents logGDF1;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
where gender = "M";
class Contents gender;
model GDF3 = Contents GDF1 / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
class Contents gender;
model GDFdelta = Contents;
run;

proc genmod data = asmicardl2change;
class Contents gender;
model logGDFdelta = Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
class Contents gender;
model GDFdelta = Contents / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
where gender = "F";
class Contents gender;
model GDFdelta = Contents;
run;

proc genmod data = asmicardl2change;
where gender = "F";
class Contents gender;
model logGDFdelta = Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
where gender = "F";
class Contents gender;
model GDFdelta = Contents / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
where gender = "M";
class Contents gender;
model GDFdelta = Contents;
run;

proc genmod data = asmicardl2change;
where gender = "M";
class Contents gender;
model logGDFdelta = Contents;
lsmeans Contents / corr exp diff cl;
run;

proc genmod data = asmicardl2change;
where gender = "M";
class Contents gender;
model GDFdelta = Contents / dist=normal link=log;
lsmeans Contents / corr exp diff cl;
run;

**********************************************************************************************************************************************************;
 
*Testing other covariates;

proc print data = asmicardl2;
run;

proc contents data = asmicardl2;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 1;
class Collection Contents gender;
model GDF_15 = age_today bmi gender;
run;

proc genmod data = asmicardl2;
where tube = 2 and collection = 3;
class Collection Contents gender;
model GDF_15 = age_today bmi gender;
run;

proc genmod data = asmicardl2;
where tube = 2 and contents = "Aspirin";
class Collection Contents gender;
model GDF_15 = age_today bmi gender;
run;

proc genmod data = asmicardl2;
where tube = 2 and contents = "Placebo";
class Collection Contents gender Subject_ID;
model GDF_15 = age_today bmi gender;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2 and contents = "Aspirin";
class Collection Contents gender Subject_ID;
model GDF_15 = age_today bmi gender;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents gender Subject_ID;
model GDF_15 = age_today bmi*Contents gender;
repeated subject=Subject_ID;
run;

proc genmod data = asmicardl2;
where tube = 2;
class Collection Contents gender Subject_ID;
model GDF_15 = age_today bmi gender*Contents;
repeated subject=Subject_ID;
run;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'D:\Professional Folder\WBOB\ASMIC Study\15_ASMIC Fall 2019 Update\1_ASMIC Datasets';

data mylib.asmicardl2;
set asmicardl2;
run;

proc print data = asmicardl2;
run;

proc contents data = asmicardl2;
run;

**********************************************************************************************************************************************************************************;

*Merging the blood data from the covariates to the mic1 data;

*Running regression models on various blood markers from the covariates SAS dataset;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'D:\Professional Folder\WBOB\ASMIC Study\15_ASMIC Fall 2019 Update\1_ASMIC Datasets';

data asmicardl2;
set mylib.asmicardl2;
run;

proc print data = asmicardl2;
run;

proc contents data = asmicardl2;
run;

data asmicardl2;
set asmicardl2 (drop = weight_v1 height_v1 bmi_v1);
Sample_ID = SampleID;
run;

data metabolitesbloodasmicdataset;
set mylib.metabolitesbloodasmicdataset;
run;

proc print data = metabolitesbloodasmicdataset;
run;

proc contents data = metabolitesbloodasmicdataset;
run;

data metabolitesbloodasmicdataset;
set metabolitesbloodasmicdataset (keep = Visit Bottle_Number Contents ID LCA DCA Sample_ID Bcell IgDpMemB IgDmMemB NaiveB Tcell CT ActCT CM_CT E_CT pE_CT pE1_CT pE2_CT EM_CT EM1_CT EM2_CT EM3_CT EM4_CT N_CT HT ActHT CM_HT E_HT EM_HT N_HT DC DCm DCp NK NKHI NKLO MONO MONOc MONOnc record_id_es age_today gender weight_v1 height_v1 bmi_v1);
run;

proc contents data = metabolitesbloodasmicdataset;
run;

proc sort data = asmicardl2; by Sample_ID; run;
proc sort data = metabolitesbloodasmicdataset; by Sample_ID; run;

data asmicblood;
merge asmicardl2 metabolitesbloodasmicdataset;
by Sample_ID; 
run;

proc print data = asmicblood;
run;

proc contents data = asmicblood;
run;

data mylib.asmicblood;
set asmicblood;
run;

***********************************************************************************************************************************************************************************;

*B cell;

proc genmod data = asmicblood;
where tube = 2 and collection = 3;
class Contents gender;
model Bcell = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "F";
class Contents gender;
model Bcell = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "M";
class Contents gender;
model Bcell = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin";
class Collection gender Subject_ID;
model Bcell = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "F";
class Collection gender Subject_ID;
model Bcell = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "M";
class Collection gender Subject_ID;
model Bcell = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2;
class Collection Contents gender;
model Bcell = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "F";
class Collection Contents gender;
model Bcell = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "M";
class Collection Contents gender;
model Bcell = Collection Contents;
run;

***********************************************************************************************************************************************************************************;

*T cell;

proc genmod data = asmicblood;
where tube = 2 and collection = 3;
class Contents gender;
model Tcell = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "F";
class Contents gender;
model Tcell = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "M";
class Contents gender;
model Tcell = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin";
class Collection gender Subject_ID;
model Tcell = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "F";
class Collection gender Subject_ID;
model Tcell = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "M";
class Collection gender Subject_ID;
model Tcell = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2;
class Collection Contents gender;
model Tcell = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "F";
class Collection Contents gender;
model Tcell = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "M";
class Collection Contents gender;
model Tcell = Collection Contents;
run;

***********************************************************************************************************************************************************************************;

*IgDmMemB;

proc genmod data = asmicblood;
where tube = 2 and collection = 3;
class Contents gender;
model IgDmMemB = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "F";
class Contents gender;
model IgDmMemB = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "M";
class Contents gender;
model IgDmMemB = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin";
class Collection gender Subject_ID;
model IgDmMemB = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "F";
class Collection gender Subject_ID;
model IgDmMemB = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "M";
class Collection gender Subject_ID;
model IgDmMemB = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2;
class Collection Contents gender;
model IgDmMemB = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "F";
class Collection Contents gender;
model IgDmMemB = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "M";
class Collection Contents gender;
model IgDmMemB = Collection Contents;
run;

***********************************************************************************************************************************************************************************;

*IgDpMemB;

proc genmod data = asmicblood;
where tube = 2 and collection = 3;
class Contents gender;
model IgDpMemB = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "F";
class Contents gender;
model IgDpMemB = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "M";
class Contents gender;
model IgDpMemB = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin";
class Collection gender Subject_ID;
model IgDpMemB = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "F";
class Collection gender Subject_ID;
model IgDpMemB = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "M";
class Collection gender Subject_ID;
model IgDpMemB = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2;
class Collection Contents gender;
model IgDpMemB = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "F";
class Collection Contents gender;
model IgDpMemB = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "M";
class Collection Contents gender;
model IgDpMemB = Collection Contents;
run;

***********************************************************************************************************************************************************************************;

*NK;

proc genmod data = asmicblood;
where tube = 2 and collection = 3;
class Contents gender;
model NK = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "F";
class Contents gender;
model NK = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "M";
class Contents gender;
model NK = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin";
class Collection gender Subject_ID;
model NK = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "F";
class Collection gender Subject_ID;
model NK = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "M";
class Collection gender Subject_ID;
model NK = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2;
class Collection Contents gender;
model NK = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "F";
class Collection Contents gender;
model NK = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "M";
class Collection Contents gender;
model NK = Collection Contents;
run;

***********************************************************************************************************************************************************************************;

*Mono;

proc genmod data = asmicblood;
where tube = 2 and collection = 3;
class Contents gender;
model Mono = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "F";
class Contents gender;
model Mono = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and collection = 3 and gender = "M";
class Contents gender;
model Mono = Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin";
class Collection gender Subject_ID;
model Mono = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "F";
class Collection gender Subject_ID;
model Mono = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2 and Contents = "Aspirin" and gender = "M";
class Collection gender Subject_ID;
model Mono = Collection;
repeated subject=Subject_ID;
run;

proc genmod data = asmicblood;
where tube = 2;
class Collection Contents gender;
model Mono = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "F";
class Collection Contents gender;
model Mono = Collection Contents;
run;

proc genmod data = asmicblood;
where tube = 2 and gender = "M";
class Collection Contents gender;
model Mono = Collection Contents;
run;

***********************************************************************************************************************************************************************************;

proc sort data = asmicblood; by Contents;
run;

proc corr data = asmicblood;
var GDF_15 Bcell Tcell NK Mono;
by Contents;
run;

***********************************************************************************************************************************************************************************;
***********************************************************************************************************************************************************************************;
***********************************************************************************************************************************************************************************;

*Adding the PGE_M data to the rest of the biomarkers;

PROC IMPORT OUT= work.aspirinpge DATAFILE= "D:\Professional Folder\WBOB\ASMIC Study\15_ASMIC Fall 2019 Update\1_ASMIC Datasets\ASMIC PGEM.xlsx" DBMS=xlsx REPLACE;
  SHEET="Sheet2"; 
  GETNAMES=YES;
RUN;

proc print data = aspirinpge;
run;

proc contents data = aspirinpge;
run;

data aspirinpge;
set aspirinpge;
rename Study_subject_ID = OralID;
rename VAR3 = PGEM;
run;

data aspirinpge;
set aspirinpge (keep = OralID PGEM);
run;

proc contents data = aspirinpge;
run;

proc print data = aspirinpge;
run;

proc print data = asmicblood;
run;

proc sort data = asmicblood; by OralID; run;
proc sort data = aspirinpge; by OralID; run;

data asmicbloodfinal;
merge asmicblood aspirinpge;
by OralID;
run;

proc print data = asmicbloodfinal;
run;

data asmicblood;
set asmicbloodfinal;
run;

proc print data = asmicblood;
run;

options ps = 3000 ls = 120 nofmterr;
libname mylib 'D:\Professional Folder\WBOB\ASMIC Study\15_ASMIC Fall 2019 Update\1_ASMIC Datasets';

data mylib.asmicblood;
set asmicblood;
run;

***********************************************************************************************************************************************************************************;

*Transposing the data to explore immune cell dynamics in the aspirin and placebo arm;

*transposing the MIC1, Bcell, Tcell, NK and Mono Values;

/*
The VAR statement can be used to specify the variables that are to be transposed (DEPOSIT). An ID statement
specifies a variable (MONTH) whose value controls the variables to which values of DEPOSIT are assigned. The
variable _NAME_ is not needed as is dropped using a data set option.
*/

proc print data = asmicblood;
run;

Proc sort data = asmicblood;
by Subject_ID;
run;

proc transpose data = asmicblood out=asmicgdft (drop=_name_ _label_) prefix = GDF;
var GDF_15;
by Subject_ID;
ID Collection;
run;

proc print data = asmicgdft;
run;

proc transpose data = asmicblood out=asmicbcellt (drop=_name_ _label_) prefix = Bcell;
var Bcell;
by Subject_ID;
ID Collection;
run;

proc print data = asmicbcellt;
run;

proc transpose data = asmicblood out=asmictcellt (drop=_name_ _label_) prefix = Tcell;
var Tcell;
by Subject_ID;
ID Collection;
run;

proc print data = asmictcellt;
run;

proc transpose data = asmicblood out=asmicnkt (drop=_name_ _label_) prefix = NK;
var NK;
by Subject_ID;
ID Collection;
run;

proc print data = asmicnkt;
run;

proc transpose data = asmicblood out=asmicmonot (drop=_name_ _label_) prefix = MONO;
var MONO;
by Subject_ID;
ID Collection;
run;

proc print data = asmicmonot;
run;

proc transpose data = asmicblood out=asmicpget (drop=_name_ _label_) prefix = PGEM;
var PGEM;
by Subject_ID;
ID Collection;
run;

proc print data = asmicpget;
run;

*I am deleting the visit 1 since that information is already transposed in the datasets, and I do not want to duplicate entries (i.e. one data file witl 50 observations);
*Keep the OralID and GUTID so that you can merge the dataset to the longitudinal dataset for downstream analyses;

proc print data = asmicblood;
run;

data asmicblooddiff;
set asmicblood;
keep Subject_ID GUTID OralID Collection Treatment age_today gender weight_v1 height_v1 bmi_v1 Contents tube;
if Collection = 1 then delete;
if tube = 3 then delete;
run;

proc print data = asmicblooddiff;
run;

proc sort data = asmicblooddiff; by Subject_ID; run;
proc sort data = asmicgdft; by Subject_ID; run;
proc sort data = asmicbcellt; by Subject_ID; run;
proc sort data = asmictcellt; by Subject_ID; run;
proc sort data = asmicnkt; by Subject_ID; run;
proc sort data = asmicmonot; by Subject_ID; run;
proc sort data = asmicpget; by Subject_ID; run;


data asmicbloodchange;
merge asmicblooddiff asmicgdft asmicbcellt asmictcellt asmicnkt asmicmonot asmicpget;
by Subject_ID;
run;

proc print data = asmicbloodchange;
run;

data asmicbloodchange;
set asmicbloodchange;
GDFdelta = GDF3 - GDF1;
Bcelldelta = Bcell3 - Bcell1;
Tcelldelta = Tcell3 - Tcell1;
NKdelta = NK3 - NK1;
Monodelta = Mono3 - Mono1;
PGEMdelta = PGEM3 - PGEM1;
run;

data mylib.asmicbloodchange;
set asmicbloodchange;
run;

**********************************************************************************************************************************************************************************;

*Creating the mapping file for the asmic gut / mic_1 analysis which includes MIC1, WBC proportions, and PGE levels. We also need to transpose the biomarker data and merge it to 
the longitudinal data so that you can run a change over time analysis restricted to visit 3;
 
options ps = 3000 ls = 120 nofmterr;
libname mylib 'D:\Professional Folder\WBOB\ASMIC Study\15_ASMIC Fall 2019 Update\1_ASMIC Datasets';

data asmicbloodchange;
set mylib.asmicbloodchange;
run;

proc print data = asmicbloodchange;
run;

proc contents data = asmicbloodchange;
run;

data asmicblood;
set mylib.asmicblood;
run;

proc print data = asmicblood;
run;

proc contents data = asmicblood;
run;

*Importing the mapping file from the dissertation file to merge it to the asmic blood data;
PROC IMPORT OUT= work.aspirinoralmap DATAFILE= "D:\Dissertation\3_Dissertation Analysis\1_Dissertation Processed Datasets\1_asmic_oral_mapping\Aspirin_Oral_Map.xlsx" DBMS=xlsx REPLACE;
  SHEET="Aspirin_oral_Txt"; 
  GETNAMES=YES;
RUN;

proc print data = aspirinoralmap;
run;

proc contents data = aspirinoralmap;
run;

data aspirinoralmap;
set aspirinoralmap (drop = bmi_v1 height_v1 weight_v1);
run;

proc sort data = aspirinoralmap; by _SampleID; run;
proc sort data = asmicblood; by _SampleID; run;

data aspirinoralmap2;
merge aspirinoralmap asmicblood;
by _SampleID; 
run;

proc print data = aspirinoralmap2;
run;

proc contents data = aspirinoralmap2;
run;

*Exporting the new mapping file to convert to a text file;
proc export data= aspirinoralmap2
 outfile= "D:\Dissertation\3_Dissertation Analysis\1_Dissertation Processed Datasets\1_asmic_oral_mapping\Aspirin_Oral_Map_2"
 dbms=XLSX
 replace;
run;

*Adding the transposed data to the longitudinal data for downstream analysis;

data asmicbloodchange;
set mylib.asmicbloodchange;
run;

proc print data = asmicbloodchange;
run;

proc sort data = aspirinoralmap2; by OralID; run;
proc sort data = asmicbloodchange; by OralID; run;

data aspirinoralmap3;
merge aspirinoralmap2 asmicbloodchange;
by OralID;
run;

proc print data = aspirinoralmap3;
run;

*Exporting the new mapping file to convert to a text file;
proc export data= aspirinoralmap3
 outfile= "D:\Dissertation\3_Dissertation Analysis\1_Dissertation Processed Datasets\1_asmic_oral_mapping\Aspirin_Oral_Map_3"
 dbms=XLSX
 replace;
run;
