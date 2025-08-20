******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

options ps = 3000 ls = 120 nofmterr;
options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '           ';
libname mylib 'E:\Professional Folder\MDH _ IAC\MDH\Current Projects\Updated 2018 Parasite Manuscript\Parasite Project\Codes and Datasets\Datasets';

data parasite;
set mylib.parasite;
run;

data parasitemkI;
set mylib.parasitemkI;
run;

proc print data = parasitemkI (obs=500);
var AlienNumber;
run;

/*Starting to generate the tables*/

proc freq data = parasitemkI;
table year rhayear treatment1 treatment2 treatment3 treatment4;
run;

*I will need to recode the No documented treatment as presumptive albendazole for the second set of analyses;

data parasitemkII;
set parasitemkI;
if rhayear = . then delete;
if arrivalstatus ne "Primary Refugee" then delete;
run;

/*Region*/

proc freq data = parasitemkII;
tables country;
run;

*Running this crosstabulation to make sure I catch the new countries from the 2016 dataset;

proc freq data = parasitemkII;
tables country*rhayear;
run;

*Need to add Syria, Congo, skrilanka, and Honduras coming from the 2014 and 2015;

data parasitemkII;
set parasitemkII;
if (country = "BURUNDI" or country = "CAMEROON"  or country = "DJIBOUTI" or country = "CONGO, DEMOCRATIC REPUBLIC" or country = "CONGO" or country = "ERITREA" or country = "ETHIOPIA" or country = "GAMBIA, THE" or country = "GUINEA" or country = "IVORY COAST" or country = "RWANDA" or country = "KENYA" or country = "LIBERIA" or country = "MALI" or country = "SIERRA LEONE" or country = "SOMALIA" or country = "SUDAN" or country = "TANZANIA, UNITED REPUBLIC OF" or country = "TOGO" or country = "UGANDA" or country = "ZIMBABWE") then Region = "Sub-Saharan Africa";
if (country = "AFGHANISTAN" or country = "IRAN" or country = "IRAQ" or country = "WEST BANK" or country = "SYRIA") then Region = "Middle-East";
if (country = "ARMENIA" or country = "KYRGYZSTAN" or country = "UZBEKISTAN" or country = "BELARUS" or country = "KAZAKHSTAN " or country = "RUSSIA" or country = "MOLDOVA" or country = "UKRAINE" or country = "LATVIA") then Region = "Eastern Europe"; 
if (country = "BHUTAN" or country = "BURMA" or country = "TIBET" or country =  "CAMBODIA" or country =  "CHINA" or country =  "LAOS/HMONG"  or country = "THAILAND" or country = "SRI LANKA" or country = "NEPAL" or country = "PHILIPPINES" or country = "TIBET" or country = "VIETNAM") then Region = "Asia";
if (country = "CUBA" or country = "HAITI" or country = "MEXICO" or country = "HONDURAS" or country = "EL SALVADOR" or country = "COLOMBIA") then Region = "Americas";
run;

*******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
*******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

*Creating the overseas treatment variable for exposure groups based on their overseas treatment documentation (for 2006 to 2009, everybody is assigned as presumptive albendazole);

data parasitemkII;  
set parasitemkII;
Length OverseasTreatment $40.;
if (Treatment1 = " ") and (region ne "Eastern Europe") and (rhayear lt 2010) then OverseasTreatment = "Presumptive Albendazole";
else if (Treatment1 = " ") and (region = "Eastern Europe") and (rhayear lt 2010) then OverseasTreatment = "No Documented Treatment";
else if (Treatment1 = " ")and (rhayear ge 2010)then OverseasTreatment = "No Documented Treatment"; 
run;

data parasitemkII;  
set parasitemkII; 
if (Treatment1 = "Albendazole" or Treatment2 = "Albendazole" or Treatment3 = "Albendazole" or Treatment4 = "Albendazole") then OverseasTreatment = "Albendazole";
if (Treatment1 = "Albendazole" or Treatment2 = "Albendazole" or Treatment3 = "Albendazole" or Treatment4 = "Albendazole") and (Treatment1 = "Ivermectin" or Treatment2 = "Ivermectin" or Treatment3 = "Ivermectin" or Treatment4 = "Ivermectin") then OverseasTreatment = "Albendazole & Ivermectin";
if (Treatment1 = "Albendazole" or Treatment2 = "Albendazole" or Treatment3 = "Albendazole" or Treatment4 = "Albendazole") and (Treatment1 = "Praziquantel" or Treatment2 = "Praziquantel" or Treatment3 = "Praziquantel" or Treatment4 = "Praziquantel") then OverseasTreatment = "Albendazole & Praziquantel";
If Treatment1 ne ' ' then TreatmentCheck=1; Else Treatmentcheck=0;
run;

proc freq data = parasitemkII;
tables Arrivalstatus overseastreatment;
run;

Proc print data = parasitemkII (obs=500);
where OverseasTreatment ne "No Treatment";
title 'making sure treatment categories worked';
run;

proc freq data = parasitemkII;
tables Arrivalstatus overseastreatment;
run;

proc sort data = parasitemkII; by rhayear; run;

proc freq data = parasitemkII;
tables overseastreatment*rhayear;
run;

proc print data = parasitemkII;
where region = " ";
run;

proc freq data = parasitemkII;
tables Region*Overseastreatment/nopercent nocol missing;
run;

data parasitemkIII;
set parasitemkII;
if rhayear = . then delete;
if overseastreatment = " " then delete;
if arrivalstatus ne "Primary Refugee" then delete;
run;

proc freq data = parasitemkIII;
tables Arrivalstatus overseastreatment;
run;

proc sort data = parasitemkIII; by rhayear; run;

proc freq data = parasitemkIII;
tables overseastreatment*rhayear;
run;

*******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
*******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

/*Primary Refugees Evaluated for eosinophilia by CBC with differential*/

proc freq data = parasitemkIII;
tables CBCWITHDIFFERENTIAL / nocol norow nopercent; /*Could we use parasite screening instead?*/
title 'Primaries evaluated for eosinophila by CBCDIFF';
run;

/*Documented Pre-departure presumptive treatment*/

proc freq data = parasitemkIII;
tables OverseasTreatment / nocol norow nopercent ; /*Could we use parasite screening instead?*/
title 'documented Treatments';
run;

/*Serology By Pre-departure presumptive treatment category*/

proc freq data = parasitemkIII;
tables SCHISTOSERO*OverseasTreatment STRONGYSERO*OverseasTreatment / nocol norow nopercent;
title 'serology screening within treatment groups';
run; 

/*O&P By  pre-departure treatment category*/

proc freq data = parasitemkIII;
tables OVAPARASCREEN*OverseasTreatment  / nocol norow nopercent;
title 'O&P screening within treatment groups';
run; 

*******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
*******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

/*Types of pathogenic intestinal parasites found using O&P Stool exam*/

proc print data = parasitemkIII (obs=100);
where Parasitescreening = " ";
run;

proc print data = parasitemkIII (obs=100);
run;

/*To get multiple vs any I set up a count variable to determine how many positive results per person we had*/
/*When I was looking at the AMBOEBIASIS data from 2006-2009, there were a bunch of missing observation that threw off my counts of positive events, so I included a loop in the recoding statements  with or = " " to make sure that missing observations did not get counted as positive*/

proc freq data = parasitemkIII;
tables OVAPARASCREEN DIENTA HYMENO AMBOEBIASIS PARAGONIMUS ASCARIS SCHISTOSOMIASIS CLONORCHIS STRONGYLOIDES GIARDIA TRICHURIS HOOKWORM;
run;

data parasitemkIII;
set parasitemkIII;
if PARASITESCREENING = " " then delete;
if (DIENTA = "Negative" or DIENTA = "NEGATIVE" or DIENTA = "NOT_DONE" or DIENTA = "Not Done" or DIENTA = "Pending" or DIENTA = " ") then count1 = 0; else count1 = 1;
if (HYMENO = "Negative" or HYMENO = "NEGATIVE" or HYMENO = "Not Done" or HYMENO = "NOT_DONE" or HYMENO = "Pending" or HYMENO = " ") then count2 = 0; else count2 = 1;
if (AMBOEBIASIS = "Negative" or AMBOEBIASIS = "NEGATIVE" or AMBOEBIASIS = "Not Done" or AMBOEBIASIS = "NOT_DONE" or AMBOEBIASIS = "Pending" or AMBOEBIASIS = " ") then count3 = 0; else count3 = 1;
if (PARAGONIMUS = "Negative" or PARAGONIMUS = "NEGATIVE" or PARAGONIMUS = "Not Done" or PARAGONIMUS = "NOT_DONE" or PARAGONIMUS = "Pending" or PARAGONIMUS = " ") then count4 = 0; else count4 = 1;
if (ASCARIS = "Negative" or ASCARIS = "NEGATIVE" or ASCARIS = "Not Done" or ASCARIS = "NOT_DONE" or ASCARIS = "Pending" or ASCARIS = " ") then count5 = 0; else count5 = 1;
if (SCHISTOSOMIASIS = "Negative" or SCHISTOSOMIASIS = "NEGATIVE" or SCHISTOSOMIASIS = "Not Done" or SCHISTOSOMIASIS = "NOT_DONE" or SCHISTOSOMIASIS = "Pending" or SCHISTOSOMIASIS = " ") then count6 = 0; else count6 = 1;
if (CLONORCHIS = "Negative" or CLONORCHIS = "NEGATIVE" or CLONORCHIS = "Not Done" or CLONORCHIS = "NOT_DONE" or CLONORCHIS = "Pending" or CLONORCHIS = " ") then count7 = 0; else count7 = 1;
if (STRONGYLOIDES = "Negative" or STRONGYLOIDES = "NEGATIVE" or STRONGYLOIDES = "Not Done" or STRONGYLOIDES = "NOT_DONE" or STRONGYLOIDES = "Pending" or STRONGYLOIDES = " ") then count8 = 0; else count8 = 1;
if (GIARDIA = "Negative" or GIARDIA = "NEGATIVE" or GIARDIA = "Not Done" or GIARDIA = "NOT_DONE" or GIARDIA = "Pending" or GIARDIA = " ") then count9 = 0; else count9 = 1;
if (TRICHURIS = "Negative" or TRICHURIS = "NEGATIVE" or TRICHURIS = "Not Done" or TRICHURIS = "NOT_DONE" or TRICHURIS = "Pending" or TRICHURIS = " ") then count10 = 0; else count10 = 1;
if (HOOKWORM = "Negative" or HOOKWORM = "NEGATIVE" or HOOKWORM = "Not Done" or HOOKWORM = "NOT_DONE" or HOOKWORM = "Pending" or HOOKWORM = " ") then count11 = 0; else count11 = 1;
total = sum (of count1 - count11);
if total gt 1 then PathogenicParasite = "Multiple";
if total = 1 then PathogenicParasite = "Any"; else PathogenicParasite = "None";
if total ge 1 then PathogenicInfection = 1; else PathogenicInfection = 0; 
run;

proc sort data = parasitemkIII; by OverseasTreatment;
run; 

proc sort data = parasitemkIII; by rhayear;
run; 

proc freq data = parasitemkIII;
tables rhayear*PathogenicInfection / nocol norow nopercent;
title 'counting the number of infections';
run;

proc print data = parasitemkIII (obs=500);
var AlienNumber;
run;

*******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
*******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************
******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************;

/*To get the invidual type of parasite identified, I'll put the data in long format to avoid overwriting the observation for multiple counts, transcribe it to the parasite name, then put it back to wide format*/

data parasitemkV;
set parasitemkIII;
where PathogenicInfection = 1;
keep AlienNumber count1 count2 count3 count4 count5 count6 count7 count8 count9 count10 count11;

proc freq data = parasitemkV;
tables count1 count2 count3 count4 count5 count6 count7 count8 count9 count10 count11;
run;

proc sort data = parasitemkV; by AlienNumber; run;

proc transpose data = parasitemkV out=parasitemkVlong;
by AlienNumber;
run;

data parasitemkVlong;
set parasitemkVlong;
rename _NAME_ = count;
if col1 = . then delete;
if col1 = 0 then delete;
run;

proc print data = parasitemkVlong (obs=50);
run;

data parasitemkVlong;
set parasitemkVlong;
length ParasiteType $20.;
if count = "count1" then ParasiteType = "Dientamoeba";
if count = "count2" then ParasiteType = "Hymenolepis";
if count = "count3" then ParasiteType = "Entamoeba Histolitica";
if count = "count4" then ParasiteType = "Paragonimus";
if count = "count5" then ParasiteType = "Ascaris";
if count = "count6" then ParasiteType = "Schistosoma";
if count = "count7" then ParasiteType = "Clonorchis";
if count = "count8" then ParasiteType = "Strongyloides";
if count = "count9" then ParasiteType = "Giardia";
if count = "count10" then ParasiteType = "Trichuris";
if count = "count11" then ParasiteType = "Hookworm";
run;

proc print data = parasitemkVlong (obs=100);
run;

proc freq data = parasitemkVlong;
tables ParasiteType/ nocol norow nopercent;
title 'Positive cases';
run;

/*# of Cases treated or Refered (% among all cases)*/
data parasitemkVI;
set parasitemkIII;
where PathogenicInfection = 1;
keep AlienNumber DIENTA HYMENO AMBOEBIASIS PARAGONIMUS ASCARIS SCHISTOSOMIASIS CLONORCHIS STRONGYLOIDES GIARDIA TRICHURIS HOOKWORM;
run;
 
proc sort data = parasitemkVI; by AlienNumber; run;

proc transpose data = parasitemkVI out=parasitemkVIlong;
by AlienNumber;
var DIENTA HYMENO AMBOEBIASIS PARAGONIMUS ASCARIS SCHISTOSOMIASIS CLONORCHIS STRONGYLOIDES GIARDIA TRICHURIS HOOKWORM; /* Need to use a Var statement because the variables are not numeric*/
run;

data parasitemkVIlong;
set parasitemkVIlong;
drop _LABEL_;
rename _NAME_ = Parasite;
rename COL1 = Result;
if COL1 = "Negative" then delete;
if COL1 = "Not Done" then delete;
if COL1 = "Pending" then delete;
if COL1 = " " then delete;
if COL1 = "NEGATIVE" then delete;
if COL1 = "NOT_DONE" then delete;
run;

data parasitemkVlong;
set parasitemkVlong (drop = COL2);
run;

data parasitemkVIlong;
set parasitemkVIlong (drop = COL2);
run;

proc sort data = parasitemkVlong; by AlienNumber; run;
proc sort data = parasitemkVIlong; by AlienNumber; run;

data pathogenic;
merge parasitemkVlong parasitemkVIlong;
By AlienNumber;
run;

proc sort data = pathogenic; by ParasiteType; run;

proc freq data = pathogenic;
tables ParasiteType*Result/ nocol norow nopercent;
title 'calculating % treated & referrals'; /*some of the results had missing values therefore we had mismatches?*/
run;

proc print data = pathogenic (obs=500);
run;

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

data mylib.pathogenic;
set pathogenic;
run;

proc sort data = parasitemkIII; by AlienNumber; run;
proc sort data = pathogenic; by AlienNumber; run;

*The parasitemkIII dataset will have each individual infection as a dicothomous variable, as well as  the number of cases treated among those referred;

data parasitemkIV;
merge parasitemkIII pathogenic;
By AlienNumber;
run;

/*# of Cases treated or Refered (% among all cases)*/
proc freq data = parasitemkIV;
tables ParasiteType*Result/ nocol norow nopercent;
title 'calculating % treated & referrals'; /*some of the results had missing values therefore we had mismatches?*/
run;

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

options ps = 3000 ls = 120 nofmterr;
options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '           ';
libname mylib 'E:\Professional Folder\MDH _ IAC\MDH\Current Projects\Updated 2018 Parasite Manuscript\Parasite Project\Codes and Datasets\Datasets';

data mylib.parasitemkIV;
set parasitemkIV;
run;

data parasitemkIV;
set mylib.parasitemkIV;
run;

/*Modeling our risk of pathogenic infections by treatment group*/

/*making our dependent variable*/

data parasitemkIV;
set parasitemkIV;
run;

proc freq data = parasitemkIV;
tables rhayear*pathogenicInfection /norow nocol nopercent;
title 'Prevalence of pathogenic infections among refugees with domestic O&P Screening';
run;

/*Comparing Baseline Characteristics Between Infected and Non Infected*/

data parasitemkIV;
set parasitemkIV;
age = (arrivaldate - dob)/365.25;
if gender = "M" then male = 1; 
if gender = "F" then male = 0;
if age lt 18 then agecategory = 1; else agecategory = 2;
run;

proc ttest data = parasitemkIV;
class PathogenicInfection;
var age;
title 'comparing age between infected and non infected';
run;

proc freq data = parasitemkIV;
tables agecategory*overseastreatment /norow nocol nopercent;
title 'Prevalence of pathogenic infections among refugees with domestic O&P Screening';
run;

proc sort data = parasitemkIV; by descending male descending PathogenicInfection; run;

proc freq data = parasitemkIV order=data;
tables male*PathogenicInfection/nocol norow nopercent chisq;
title 'comparing gender proportions between infected and non infected';
run;

/*Amended analysis: Combine stool and serology results to form a single outcome, simplify overseas txt to two categories, and adjust by country of origin*/

proc freq data = parasitemkIV;
tables schistosero strongysero;
run;

data parasitemkIV;
set parasitemkIV;
if (OverseasTreatment = "No Documented Treatment")then txtdoc = 0; 
else if (OverseasTreatment = "Presumptive Albendazole")then txtdoc = 1; else txtdoc = 2;
if (schistosero = "Positive-not treated" or schistosero = "Positive-referred" or schistosero = "Positive-treated" or schistosero = "POSITIVE_NOT_TREATED" or schistosero = "POSITIVE_REFERRED" or schistosero = "POSITIVE_TREATED") then schistosomaserology = 1 ; else schistosomaserology = 0; 
if (STRONGYSERO = "Positive-not treated" or STRONGYSERO = "Positive-referred" or STRONGYSERO = "Positive-treated" or STRONGYSERO = "POSITIVE_NOT_TREATED" or STRONGYSERO = "POSITIVE_REFERRED" or STRONGYSERO = "POSITIVE_TREATED") then strongyloidesserology = 1 ; else strongyloidesserology = 0;
if (PathogenicInfection = 1 or schistosomaserology = 1 or strongyloidesserology = 1) then positivecase = 1; else positivecase = 0; 
if (count6 = 1 or schistosomaserology = 1) then schistocase = 1; else schistocase = 0; 
if (count8 = 1 or strongyloidesserology = 1) then strongycase = 1; else strongycase = 0; 
run;

proc print data = parasitemkIV (obs=500);
run;

proc sort data = parasitemkIV; by txtdoc; run;

proc freq data = parasitemkIV;
tables txtdoc /nocol norow nopercent;
run;

proc sort data = parasitemkIV; by positivecase; run;

proc freq data = parasitemkIV;
tables positivecase/nocol norow nopercent;
run;

proc sort data = parasitemkIV; by pathogenicinfection; run;

proc freq data = parasitemkIV;
tables pathogenicinfection/nocol norow nopercent;
run;

proc sort data = parasitemkIV; by schistosomaserology; run;

proc freq data = parasitemkIV;
tables schistosomaserology/nocol norow nopercent;
run;

proc sort data = parasitemkIV; by strongyloidesserology; run;

proc freq data = parasitemkIV;
tables strongyloidesserology/nocol norow nopercent;
run;

proc sort data = parasitemkIV; by OverseasTreatment;
run; 

proc freq data = parasitemkIV;
tables schistocase*overseastreatment strongycase*overseastreatment / nocol norow nopercent;
title 'counting the number of infections';
run;

*Making outcome variables for individual parasites (maybe we do not need to delete the observation with missing parasite information, but rather code them differently?); 
*Need to update the result categories since they were entered differently in the 2006-2009 & 2016 datatset;

data parasitemkIV;  
set parasitemkIV;
if PARASITESCREENING = " " then delete;
if (DIENTA = "Negative" or DIENTA = "NEGATIVE" or DIENTA = "NOT_DONE" or DIENTA = "Not Done" or DIENTA = "Pending" or DIENTA = " ") then DientamoebaInfection = 0; else DientamoebaInfection = 1;
if (HYMENO = "Negative" or HYMENO = "NEGATIVE" or HYMENO = "Not Done" or HYMENO = "NOT_DONE" or HYMENO = "Pending" or HYMENO = " ") then HymenolepsisInfection = 0; else HymenolepsisInfection = 1;
if (AMBOEBIASIS = "Negative" or AMBOEBIASIS = "NEGATIVE" or AMBOEBIASIS = "Not Done" or AMBOEBIASIS = "NOT_DONE" or AMBOEBIASIS = "Pending" or AMBOEBIASIS = " ")  then AmboebiasisInfection = 0; else AmboebiasisInfection = 1;
if (PARAGONIMUS = "Negative" or PARAGONIMUS = "NEGATIVE" or PARAGONIMUS = "Not Done" or PARAGONIMUS = "NOT_DONE" or PARAGONIMUS = "Pending" or PARAGONIMUS = " ") then ParagonimusInfection = 0; else ParagonimusInfection = 1;
if (ASCARIS = "Negative" or ASCARIS = "NEGATIVE" or ASCARIS = "Not Done" or ASCARIS = "NOT_DONE" or ASCARIS = "Pending" or ASCARIS = " ") then AscarisInfection = 0; else AscarisInfection = 1;
if (SCHISTOSOMIASIS = "Negative" or SCHISTOSOMIASIS = "NEGATIVE" or SCHISTOSOMIASIS = "Not Done" or SCHISTOSOMIASIS = "NOT_DONE" or SCHISTOSOMIASIS = "Pending" or SCHISTOSOMIASIS = " ")  then SchistosomiasisInfection = 0; else SchistosomiasisInfection = 1;
if (CLONORCHIS = "Negative" or CLONORCHIS = "NEGATIVE" or CLONORCHIS = "Not Done" or CLONORCHIS = "NOT_DONE" or CLONORCHIS = "Pending" or CLONORCHIS = " ")  then ClonorchisInfection = 0; else ClonorchisInfection = 1;
if (STRONGYLOIDES = "Negative" or STRONGYLOIDES = "NEGATIVE" or STRONGYLOIDES = "Not Done" or STRONGYLOIDES = "NOT_DONE" or STRONGYLOIDES = "Pending" or STRONGYLOIDES = " ")  then StrongyloidesInfection = 0; else StrongyloidesInfection = 1;
if (GIARDIA = "Negative" or GIARDIA = "NEGATIVE" or GIARDIA = "Not Done" or GIARDIA = "NOT_DONE" or GIARDIA = "Pending" or GIARDIA = " ")then GiardiaInfection = 0; else GiardiaInfection = 1;
if (TRICHURIS = "Negative" or TRICHURIS = "NEGATIVE" or TRICHURIS = "Not Done" or TRICHURIS = "NOT_DONE" or TRICHURIS = "Pending" or TRICHURIS = " ") then TrichurisInfection = 0; else TrichurisInfection = 1;
if (HOOKWORM = "Negative" or HOOKWORM = "NEGATIVE" or HOOKWORM = "Not Done" or HOOKWORM = "NOT_DONE" or HOOKWORM = "Pending" or HOOKWORM = " ") then HookwormInfection = 0; else HookwormInfection = 1;
if (SchistosomiasisInfection = 1 or schistosomaserology = 1) then SchistoInfection = 1; else SchistoInfection = 0; 
if (StrongyloidesInfection = 1 or strongyloidesserology = 1) then StrongyInfection = 1; else StrongyInfection = 0; 
if (HymenolepsisInfection = 1 or ParagonimusInfection = 1 or AscarisInfection = 1 or ClonorchisInfection = 1 or TrichurisInfection =1 or HookwormInfection = 1) then HelminthInfection = 1; else HelminthInfection = 0;
if (AmboebiasisInfection = 1 or GiardiaInfection = 1 or DientamoebaInfection = 1) then Protozoaninfection = 1; else Protozoaninfection = 0;
run;

proc print data = parasitemkIV (obs=50);
run;

/*secondary data analysis for individual parasites: make separate categories for albendazole, ivermectin, prazinquantel*/

data parasitemkIV;  
set parasitemkIV;
if (Treatment1 = "Albendazole" or Treatment2 = "Albendazole" or Treatment3 = "Albendazole" or Treatment4 = "Albendazole") then Albendazole = 1; else Albendazole = 0;
if (Treatment1 = "Ivermectin" or Treatment2 = "Ivermectin" or Treatment3 = "Ivermectin" or Treatment4 = "Ivermectin") then Ivermectin = 1; else Ivermectin = 0;
if (Treatment1 = "Praziquantel" or Treatment2 = "Praziquantel" or Treatment3 = "Praziquantel" or Treatment4 = "Praziquantel") then Prazinquantel = 1; else Prazinquantel = 0;
run;

/*Making a period category to distinguish 2006-2009 when we are using presumptive treatment information, from 2010-2016 where we are determining treatment information based on EDN data*/

data parasitemkIV;
set parasitemkIV;
if rhayear lt 2010 then EDNtreatment = 0; else EDNtreatment = 1;
run;

proc freq data = parasitemkIV;
tables rhayear EDNtreatment EDNtreatment*rhayear / nocol norow nopercent;
run;

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

/*Demographics Table*/

/*Age Average*/

proc means data = parasitemkIV mean;
class Overseastreatment;
var age;
run;

proc sort data = parasitemkIV; by agecategory; run;

proc means data = parasitemkIV mean;
class Overseastreatment;
var age;
by agecategory;
run;

/*For totals in demographic tables: the presumptive albendazole category captures the 2006 - 2009 refugees (except the one from easter europe who did not receive any txt*/

proc sort data = parasitemkIV; by region; run;

proc freq data = parasitemkIV;
tables Region*Overseastreatment/nopercent norow missing;
run;

proc sort data = parasitemkIV; by gender; run;

proc freq data = parasitemkIV; 
tables gender*Overseastreatment/nopercent norow missing;
run;

proc sort data = parasiteIV; by agecategory; run;

proc freq data = parasitemkIV; 
tables agecategory*Overseastreatment/nopercent norow missing;
run;

*Figure of unadjusted prevalence parasite rates;

proc freq data = parasitemkIV;
table 
DientamoebaInfection*overseastreatment
AmboebiasisInfection*overseastreatment
GiardiaInfection*overseastreatment
SchistoInfection*overseastreatment
StrongyInfection*overseastreatment
Helminthinfection*overseastreatment/nopercent nocol missing;
run;

*Distribution of parasitic infections;

data parasitemkIV;
set parasitemkIV;
length regioncategory $40.;
if (region = "Middle-East" or region = "Americas" or region = "Eastern Europe") then regioncategory = "Other"; else regioncategory = region;
run;

proc freq data = parasitemkIV;
table 
DientamoebaInfection*regioncategory
HymenolepsisInfection*regioncategory
AmboebiasisInfection*regioncategory
ParagonimusInfection*regioncategory
AscarisInfection*regioncategory
ClonorchisInfection*regioncategory
GiardiaInfection*regioncategory
TrichurisInfection*regioncategory
HookwormInfection*regioncategory
SchistoInfection*regioncategory
StrongyInfection*regioncategory
Pathogenicparasite*regioncategory/nopercent nocol missing;
run;

/*Seeing if infection rates have decreased over time using year as a categorical variable*/

data parasitemkIV;
set parasitemkIV;
if rhayear= 2006 then time0 = 1; else time0 = 0;
if rhayear= 2007 then time1 = 1; else time1 = 0;
if rhayear= 2008 then time2 = 1; else time2 = 0;
if rhayear= 2009 then time3 = 1; else time3 = 0;
if rhayear= 2010 then time4 = 1; else time4 = 0;
if rhayear= 2011 then time5 = 1; else time5 = 0;
if rhayear= 2012 then time6 = 1; else time6 = 0;
if rhayear= 2013 then time7 = 1; else time7 = 0;
if rhayear= 2014 then time8 = 1; else time8 = 0;
if rhayear= 2015 then time9 = 1; else time9 = 0;
if rhayear= 2016 then time10 = 1; else time10 = 0;
run;

proc logistic data = parasitemkIV descending;
model PathogenicInfection = age;
title 'Modeling age as a predictor';
run;

proc logistic data = parasitemkIV descending;
model PathogenicInfection = age age*age;
title 'Modeling Quadratic Age';
run;

proc logistic data = parasitemkIV descending;
model PathogenicInfection = time1 time2 time3 time4 time5 time6 time7 time8 time9 time10;
title 'Modeling Rates over years';
run;

proc logistic data = parasitemkIV descending;
class OverseasTreatment;
model PathogenicInfection = time1 time2 time3 time4 time5 time6 time7 time8 time9 time10 age;
title 'Model adjusted for age';
run;

proc logistic data = parasitemkIV descending;
class OverseasTreatment;
model PathogenicInfection = time1 time2 time3 time4 time5 time6 time7 time8 time9 time10 age OverseasTreatment;
title 'Adding Age, OverseasTreatment and RHA Year to the model';
run;

proc logistic data = parasitemkIV descending;
class OverseasTreatment;
model PathogenicInfection = time1 time2 time3 time4 time5 age OverseasTreatment OverseasTreatment*Time1 OverseasTreatment*Time2 OverseasTreatment*Time3 OverseasTreatment*time4 OverseasTreatment*time5 OverseasTreatment*Time6 OverseasTreatment*Time7 OverseasTreatment*Time8 OverseasTreatment*Time9 OverseasTreatment*Time10;
title 'Interaction between OverseasTreatment and Age';
run;

/*taking clusters into consideration: you would have to put the outcomes in the long format to use*/

/* Proc genmod dist=binomial link=logit gives you odds ratio */

data parasitemkXII;
set parasitemkIV;
if chartnum = " " then delete;
run;

proc genmod data = parasitemkXII descending;
class chartnum;
model PathogenicInfection = age/ dist = binomial link = logit;
repeated subject = chartnum / Type = exch;
title 'using family clusters: Age only';
run;

proc genmod data = parasitemkXII descending;
class chartnum;
model PathogenicInfection = age  age*age/ dist = binomial link = logit;
repeated subject = chartnum / Type = exch;
title 'using family clusters: Quadratic Age';
run;

proc genmod data = parasitemkXII descending;
class chartnum;
model PathogenicInfection = time1 time2 time3 time4 time5 time6 time7 time8 time9 time10 / dist = binomial link = logit;
repeated subject = chartnum / Type = exch;
title 'using family clusters: Age + RHAyear';
run;

proc genmod data = parasitemkXII descending;
class OverseasTreatment chartnum;
model PathogenicInfection = time1 time2 time3 time4 time5 time6 time7 time8 time9 time10 age / dist = binomial link = logit;
repeated subject = chartnum / Type = exch;
title 'using family clusters: Age and Overseastreatment';
run;

proc genmod data = parasitemkXII descending;
class OverseasTreatment chartnum;
model PathogenicInfection = time1 time2 time3 time4 time5 time6 time7 time8 time9 time10 age OverseasTreatment / dist = binomial link = logit;
repeated subject = chartnum / Type = exch;
title 'using family clusters: Age + OverseasTreatment + RHAyear';
run;

proc genmod data = parasitemkXII descending;
class chartnum OverseasTreatment;
model PathogenicInfection = time1 time2 time3 time4 time5 age OverseasTreatment OverseasTreatment*Time1 OverseasTreatment*Time2 OverseasTreatment*Time3 OverseasTreatment*time4 OverseasTreatment*time5 OverseasTreatment*Time6 OverseasTreatment*Time7 OverseasTreatment*Time8 OverseasTreatment*Time9 OverseasTreatment*Time10 / dist = binomial link = logit;
repeated subject = chartnum / Type = exch;
title 'using family clusters: Interaction between OverseasTreatment and RHA Year';
run;

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

/*Combined positive cases as an outcome: Final analysis for 2010 to 2016*/

proc sort data = parasitemkIV; by COUNTRY;
run;

proc freq data = parasitemkIV;
tables positivecase txtdoc EDNtreatment agecategory gender region overseastreatment;
run;

proc logistic data = parasitemkIV descending;
where EDNtreatment = 1;
model positivecase = txtdoc age/CL;
title 'Documentation Effect';
run;

proc logistic data = parasitemkIV descending;
where EDNtreatment = 1;
class COUNTRY txtdoc gender;
model positivecase = txtdoc age gender COUNTRY/CL;
title 'Documentation Effect';
run;

proc logistic data = parasitemkIV descending;
where EDNtreatment = 1;
class COUNTRY txtdoc agecategory gender;
model positivecase = txtdoc agecategory gender COUNTRY/CL;
title 'Documentation Effect';
run;

proc logistic data = parasitemkIV descending;
where EDNtreatment = 1;
class region txtdoc(ref= "0") agecategory gender;
model positivecase = txtdoc agecategory gender region/CL;
title 'Documentation Effect';
run;

proc logistic data = parasitemkIV descending;
where EDNtreatment = 1;
class region txtdoc(ref= "0") agecategory;
model positivecase = txtdoc agecategory region/CL;
title 'Documentation Effect';
run;

proc logistic data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory;
model positivecase = overseastreatment agecategory region/CL;
title 'Documentation Effect';
run;

proc logistic data = parasitemkIV descending;
where EDNtreatment = 1 and overseastreatment ne "No Documented Treatment";
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory;
model positivecase = overseastreatment agecategory region/CL;
title 'Documentation Effect';
run;

proc logistic data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory gender;
model positivecase = overseastreatment agecategory region gender/CL;
title 'Documentation Effect';
run;

proc logistic data = parasitemkIV descending;
where EDNtreatment = 1 and overseastreatment ne "No Documented Treatment";
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory gender;
model positivecase = overseastreatment agecategory region gender/CL;
title 'Documentation Effect';
run;

/*Making adjustment to data analysis */

/*Using genmod to get relative risk instead of odds ratios: use log link instead of logit link*/
/* Proc Genmod does not back-transform automatically*/

proc sort data = parasitemkIV; by COUNTRY;
run;

proc freq data = parasitemkIV;
tables positivecase*txtdoc;
run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") txtdoc(ref= "0") agecategory gender;
model positivecase = txtdoc agecategory region gender/type3 dist=bin link=log;
title 'Documentation Effect';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory gender;
model positivecase = overseastreatment agecategory region gender/type3 dist=bin link=log;
title 'Specific Txt V1';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1 and overseastreatment ne "No Documented Treatment";
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory gender;
model positivecase = overseastreatment agecategory region gender/type3 dist=bin link=log;
title 'Specific Txt V2';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

*Full dataset: This model is to determine if parasitososis is associated with age category, sex and region of arrival in the dataset from 2006-2016;

proc freq data = parasitemkIV; tables positivecase; run;
proc freq data = parasitemkIV; tables region; run;

*Getting the numbers for the parasitosis association table;

proc freq data = parasitemkIV; tables txtdoc*positivecase agecategory*positivecase gender*positivecase regioncategory*positivecase / nocol nopercent missing; run; 

proc genmod data = parasitemkIV descending;
class regioncategory (ref="Sub-Saharan Africa") txtdoc(ref= "0") agecategory gender;
model positivecase = txtdoc regioncategory gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Full dataset: This model is to determine if parasitososis is associated with age category, sex and region of arrival in the dataset from 2010-2016;

proc freq data = parasitemkIV; tables positivecase; run;
proc freq data = parasitemkIV; tables region; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model positivecase = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

/*Main models for updated analysis with no document treatment as the reference group*/

proc freq data = parasitemkIV; tables strongyinfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model StrongyInfection = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc freq data = parasitemkIV; tables schistoinfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model SchistoInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc freq data = parasitemkIV; tables Helminthinfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model HelminthInfection = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc freq data = parasitemkIV; tables Protozoaninfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model Protozoaninfection = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Shistosoma in africa;

data schistosomaafrica;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = schistosomaafrica;
tables EDNtreatment overseastreatment region;
run;

data schistosomaafrica;
set schistosomaafrica;
if overseastreatment = "Albendazole & Ivermectin" then delete;
run;

proc freq data = schistosomaafrica;
tables EDNtreatment overseastreatment region;
run;

data schistosomaafrica;
set schistosomaafrica;
if region ne "Sub-Saharan Africa" then delete;
run;

proc freq data = schistosomaafrica;
tables EDNtreatment overseastreatment region;
run;

proc freq data = schistosomaafrica; tables schistoinfection; run;

proc genmod data = schistosomaafrica descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model SchistoInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Strongyloides infection in Africa;

data strongyafrica;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = strongyafrica;
tables EDNtreatment overseastreatment region;
run;

data strongyafrica;
set strongyafrica;
if (overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = strongyafrica;
tables EDNtreatment overseastreatment region;
run;

data strongyafrica;
set strongyafrica;
if region ne "Sub-Saharan Africa" then delete;
run;

proc freq data = strongyafrica;
tables EDNtreatment overseastreatment region;
run;

proc freq data = strongyafrica; tables strongyinfection; run;

proc genmod data = strongyafrica descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model strongyInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Strongyloides infection in Asia;

data strongyasia;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = strongyasia;
tables EDNtreatment overseastreatment region;
run;

data strongyasia;
set strongyasia;
if (overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = strongyasia;
tables EDNtreatment overseastreatment region;
run;

data strongyasia;
set strongyasia;
if region ne "Asia" then delete;
run;

proc freq data = strongyasia;
tables EDNtreatment overseastreatment region;
run;

proc freq data = strongyasia; tables strongyinfection; run;

proc genmod data = strongyasia descending;
where EDNtreatment = 1;
class region (ref="Asia") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model strongyInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Helminths in Africa;

data helminthafrica;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = helminthafrica;
tables EDNtreatment overseastreatment region;
run;

data helminthafrica;
set helminthafrica;
if (overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = helminthafrica;
tables EDNtreatment overseastreatment region;
run;

data helminthafrica;
set helminthafrica;
if region ne "Sub-Saharan Africa" then delete;
run;

proc freq data = helminthafrica;
tables EDNtreatment overseastreatment region;
run;

proc freq data = helminthafrica; tables helminthinfection; run;

proc genmod data = helminthafrica descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model helminthInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Helminths in Asia;

data helminthasia;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = helminthasia;
tables EDNtreatment overseastreatment region;
run;

data helminthasia;
set helminthasia;
if (overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = helminthasia;
tables EDNtreatment overseastreatment region;
run;

data helminthasia;
set helminthasia;
if region ne "Asia" then delete;
run;

proc freq data = helminthasia;
tables EDNtreatment overseastreatment region;
run;

proc freq data = helminthasia; tables helminthinfection; run;

proc genmod data = helminthasia descending;
where EDNtreatment = 1;
class region (ref="Asia") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model helminthInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

/*Main models for updated analysis with albendazole as the reference level*/
/*Some of the findings for albendazole vs albendazole & ivermectin was in the opposite direction than what I expected (ie the combined effect being greater than albendazole alone)*/

proc freq data = parasitemkIV; tables strongyinfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory gender;
model StrongyInfection = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc freq data = parasitemkIV; tables schistoinfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory gender;
model SchistoInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc freq data = parasitemkIV; tables Helminthinfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory gender;
model HelminthInfection = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc freq data = parasitemkIV; tables Protozoaninfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory gender;
model Protozoaninfection = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Shistosoma in africa;

data schistosomaafrica;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = schistosomaafrica;
tables EDNtreatment overseastreatment region;
run;

data schistosomaafrica;
set schistosomaafrica;
if (overseastreatment = "No Documented Treatment" or overseastreatment = "Albendazole & Ivermectin") then delete;
run;

proc freq data = schistosomaafrica;
tables EDNtreatment overseastreatment region;
run;

data schistosomaafrica;
set schistosomaafrica;
if region ne "Sub-Saharan Africa" then delete;
run;

proc freq data = schistosomaafrica;
tables EDNtreatment overseastreatment region;
run;

proc freq data = schistosomaafrica; tables schistoinfection; run;

proc genmod data = schistosomaafrica descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory gender;
model SchistoInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Strongyloides infection in Africa;

data strongyafrica;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = strongyafrica;
tables EDNtreatment overseastreatment region;
run;

data strongyafrica;
set strongyafrica;
if (overseastreatment = "No Documented Treatment" or overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = strongyafrica;
tables EDNtreatment overseastreatment region;
run;

data strongyafrica;
set strongyafrica;
if region ne "Sub-Saharan Africa" then delete;
run;

proc freq data = strongyafrica;
tables EDNtreatment overseastreatment region;
run;

proc freq data = strongyafrica; tables strongyinfection; run;

proc genmod data = strongyafrica descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory gender;
model strongyInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Strongyloides infection in Asia;

data strongyasia;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = strongyasia;
tables EDNtreatment overseastreatment region;
run;

data strongyasia;
set strongyasia;
if (overseastreatment = "No Documented Treatment" or overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = strongyasia;
tables EDNtreatment overseastreatment region;
run;

data strongyasia;
set strongyasia;
if region ne "Asia" then delete;
run;

proc freq data = strongyasia;
tables EDNtreatment overseastreatment region;
run;

proc freq data = strongyasia; tables strongyinfection; run;

proc genmod data = strongyasia descending;
where EDNtreatment = 1;
class region (ref="Asia") overseastreatment(ref= "Albendazole") agecategory gender;
model strongyInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Helminths in Africa;

data helminthafrica;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = helminthafrica;
tables EDNtreatment overseastreatment region;
run;

data helminthafrica;
set helminthafrica;
if (overseastreatment = "No Documented Treatment" or overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = helminthafrica;
tables EDNtreatment overseastreatment region;
run;

data helminthafrica;
set helminthafrica;
if region ne "Sub-Saharan Africa" then delete;
run;

proc freq data = helminthafrica;
tables EDNtreatment overseastreatment region;
run;

proc freq data = helminthafrica; tables helminthinfection; run;

proc genmod data = helminthafrica descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "Albendazole") agecategory gender;
model helminthInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Helminths in Asia;

data helminthasia;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = helminthasia;
tables EDNtreatment overseastreatment region;
run;

data helminthasia;
set helminthasia;
if (overseastreatment = "No Documented Treatment" or overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = helminthasia;
tables EDNtreatment overseastreatment region;
run;

data helminthasia;
set helminthasia;
if region ne "Asia" then delete;
run;

proc freq data = helminthasia;
tables EDNtreatment overseastreatment region;
run;

proc freq data = helminthasia; tables helminthinfection; run;

proc genmod data = helminthasia descending;
where EDNtreatment = 1;
class region (ref="Asia") overseastreatment(ref= "Albendazole") agecategory gender;
model helminthInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

*Recoding the presumptive treatment variable exclusively based on CDC recommendation guidelines from 2006 to 2016 then reruning the models for schisto, strongy and soil transmitted helminths;
*1999 Albendazole in Sub saharan africa and Asian refugees;
*2005 Albendazole + Ivermectin in Asian refugees, Albendazole + Ivermectin + Prazinquantel for Sub-Saharan Africa Refugees except (Angola, Cameroon, Chad, Democratic republic of congo, Guinea, Sudan, Nigerian and Gabon;
*2008 Albendazole to Middle Eastern Refugees;
*2013 Albendazole + Ivermectin for Asian, Sub Saharan African, Middle eastern, Latin American & Carribean Refugees;

*For the analysis, to sort of account for the lack of EDN data prior to 2010 and a lag time between CDC recommendations and implementations, We will code refugees who arrived between 2006-2010 as presumptively treated with albendazole
and anyone who arrived between 2010 and 2016 as presumptively treated with Albendazole & Ivermectin;
 
proc freq data = parasitemkIV;
tables region;
run;

data parasitemkIV;  
set parasitemkIV;
Length OverseasTreatment2 $40.;
if (region ne "Eastern Europe") and (rhayear lt 2010) then OverseasTreatment2 = "Presumptive Albendazole";
else if (region ne "Eastern Europe") and (rhayear ge 2010) then OverseasTreatment2 = "Presumptive Albendazole & Ivermectin";
else OverseasTreatment2 = "No Presumptive Treatment"; 
run;

proc freq data = parasitemkIV;
tables region Overseastreatment Overseastreatment2;
run;

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

*This model is to determine if the combined presumptive treatment (Albendazole & Ivermectin) is more effective than albendazole only for reducing infections;

proc freq data = parasitemkIV; tables positivecase region gender agecategory overseastreatment2; run;

*Getting the numbers for the parasitosis association table;

proc freq data = parasitemkIV; where overseastreatment2 ne "No Presumptive Treatment";
tables overseastreatment2*positivecase agecategory*positivecase gender*positivecase regioncategory*positivecase / nocol nopercent missing; run; 

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment";
class regioncategory (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model positivecase = Overseastreatment2 regioncategory gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on age category;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and agecategory = 1;
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model positivecase = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and agecategory = 2;
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model positivecase = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on gender;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and gender = "F";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model positivecase = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and gender = "M";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model positivecase = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on Region;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Asia";
class overseastreatment2(ref= "Presumptive Albendazole") agecategory gender region;
model positivecase = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Sub-Saharan Africa";
class overseastreatment2(ref= "Presumptive Albendazole") agecategory gender region;
model positivecase = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Middle-East";
class overseastreatment2(ref= "Presumptive Albendazole") agecategory gender region;
model positivecase = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Repeating the analysis for strongyloides and other soil transmitted helminths;

*Strongyloides;

proc freq data = parasitemkIV; tables strongyinfection region gender agecategory overseastreatment2; run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on age category;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and agecategory = 1;
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and agecategory = 2;
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on gender;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and gender = "F";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and gender = "M";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on Region;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Asia";
class region overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Sub-Saharan Africa";
class region overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Middle-East";
class region overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Helminth Infection;

proc freq data = parasitemkIV; tables helminthinfection region gender agecategory overseastreatment2; run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on age category;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and agecategory = 1;
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and agecategory = 2;
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on gender;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and gender = "F";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and gender = "M";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on Region;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Asia";
class region overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Sub-Saharan Africa";
class region  overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Regioncategory = "Other";
class regioncategory overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 regioncategory gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

/*Main models for updated analysis with no document treatment as the reference group*/

proc freq data = parasitemkIV; tables strongyinfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model StrongyInfection = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc freq data = parasitemkIV; tables schistoinfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model SchistoInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc freq data = parasitemkIV; tables Helminthinfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model HelminthInfection = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc freq data = parasitemkIV; tables Protozoaninfection; run;

proc genmod data = parasitemkIV descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model Protozoaninfection = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Shistosoma in africa;

data schistosomaafrica;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = schistosomaafrica;
tables EDNtreatment overseastreatment region;
run;

data schistosomaafrica;
set schistosomaafrica;
if overseastreatment = "Albendazole & Ivermectin" then delete;
run;

proc freq data = schistosomaafrica;
tables EDNtreatment overseastreatment region;
run;

data schistosomaafrica;
set schistosomaafrica;
if region ne "Sub-Saharan Africa" then delete;
run;

proc freq data = schistosomaafrica;
tables EDNtreatment overseastreatment region;
run;

proc freq data = schistosomaafrica; tables schistoinfection; run;

proc genmod data = schistosomaafrica descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model SchistoInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Strongyloides infection in Africa;

data strongyafrica;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = strongyafrica;
tables EDNtreatment overseastreatment region;
run;

data strongyafrica;
set strongyafrica;
if (overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = strongyafrica;
tables EDNtreatment overseastreatment region;
run;

data strongyafrica;
set strongyafrica;
if region ne "Sub-Saharan Africa" then delete;
run;

proc freq data = strongyafrica;
tables EDNtreatment overseastreatment region;
run;

proc freq data = strongyafrica; tables strongyinfection; run;

proc genmod data = strongyafrica descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model strongyInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Strongyloides infection in Asia;

data strongyasia;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = strongyasia;
tables EDNtreatment overseastreatment region;
run;

data strongyasia;
set strongyasia;
if (overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = strongyasia;
tables EDNtreatment overseastreatment region;
run;

data strongyasia;
set strongyasia;
if region ne "Asia" then delete;
run;

proc freq data = strongyasia;
tables EDNtreatment overseastreatment region;
run;

proc freq data = strongyasia; tables strongyinfection; run;

proc genmod data = strongyasia descending;
where EDNtreatment = 1;
class region (ref="Asia") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model strongyInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Helminths in Africa;

data helminthafrica;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = helminthafrica;
tables EDNtreatment overseastreatment region;
run;

data helminthafrica;
set helminthafrica;
if (overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = helminthafrica;
tables EDNtreatment overseastreatment region;
run;

data helminthafrica;
set helminthafrica;
if region ne "Sub-Saharan Africa" then delete;
run;

proc freq data = helminthafrica;
tables EDNtreatment overseastreatment region;
run;

proc freq data = helminthafrica; tables helminthinfection; run;

proc genmod data = helminthafrica descending;
where EDNtreatment = 1;
class region (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model helminthInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Helminths in Asia;

data helminthasia;
set parasitemkIV;
if EDNtreatment ne 1 then delete;
run;

proc freq data = helminthasia;
tables EDNtreatment overseastreatment region;
run;

data helminthasia;
set helminthasia;
if (overseastreatment = "Albendazole & Praziquantel") then delete;
run;

proc freq data = helminthasia;
tables EDNtreatment overseastreatment region;
run;

data helminthasia;
set helminthasia;
if region ne "Asia" then delete;
run;

proc freq data = helminthasia;
tables EDNtreatment overseastreatment region;
run;

proc freq data = helminthasia; tables helminthinfection; run;

proc genmod data = helminthasia descending;
where EDNtreatment = 1;
class region (ref="Asia") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model helminthInfection  = Overseastreatment region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Table 2: breakdown of intestinal parasite infections by region of origin;

proc freq data = parasitemkIV;
table PathogenicParasite PathogenicParasite*Regioncategory HelminthInfection*Regioncategory ProtozoanInfection*Regioncategory/nopercent nocol missing;
run;

proc freq data = parasitemkIV;
table Positivecase*Regioncategory HelminthInfection*Regioncategory ProtozoanInfection*Regioncategory/nopercent nocol missing;
run;

proc freq data = parasitemkIV;
table 
DientamoebaInfection*Regioncategory
HymenolepsisInfection*Regioncategory
AmboebiasisInfection*Regioncategory
ParagonimusInfection*Regioncategory
AscarisInfection*Regioncategory
ClonorchisInfection*Regioncategory
GiardiaInfection*Regioncategory
TrichurisInfection*Regioncategory
HookwormInfection*Regioncategory
SchistoInfection*Regioncategory
StrongyInfection*Regioncategory/nopercent nocol missing;
run;

*Figure 1: Breakdown of intestinal parasite by overseas treatment category;

proc freq data = parasitemkIV;
table PARASITESCREENING;
run; 

proc freq data = parasitemkIV;
where PARASITESCREENING eq "Screened";
table 
DientamoebaInfection*Overseastreatment
HymenolepsisInfection*Overseastreatment
AmboebiasisInfection*Overseastreatment
ParagonimusInfection*Overseastreatment
AscarisInfection*Overseastreatment
ClonorchisInfection*Overseastreatment
GiardiaInfection*Overseastreatment
TrichurisInfection*Overseastreatment
HookwormInfection*Overseastreatment
SchistoInfection*Overseastreatment
StrongyInfection*Overseastreatment/nopercent nocol missing;
run;

/*Making adjustment to data analysis */

*testing for clusters (the statistical results with clusters in the model confirm that there are statistical differences as confirmed in the models);
data parasitemkIV;
set parasitemkIV;
if chartnum = " " then delete;
run;

proc genmod data = parasitemkIV descending;
class chartnum gender agecategory regioncategory;
model positivecase = txtdoc gender agecategory regioncategory/type3 dist=bin link=log;
repeated subject = chartnum / Type = exch;
run;

proc genmod data = parasitemkIV descending;
class chartnum gender agecategory regioncategory Overseastreatment (ref="No Documented Treatment");
model positivecase = Overseastreatment gender agecategory regioncategory/type3 dist=bin link=log;
repeated subject = chartnum / Type = exch;
run;

*Subanalyses stratified on region of origin for specific parasite subtypes;

proc freq data = parasitemkIV;
table regioncategory*overseastreatment;
run;

proc freq data = parasitemkIV;
table 
DientamoebaInfection*Regioncategory
HymenolepsisInfection*Regioncategory
AmboebiasisInfection*Regioncategory
ParagonimusInfection*Regioncategory
AscarisInfection*Regioncategory
ClonorchisInfection*Regioncategory
GiardiaInfection*Regioncategory
TrichurisInfection*Regioncategory
HookwormInfection*Regioncategory
SchistoInfection*Regioncategory
StrongyInfection*Regioncategory;
run;

proc freq data = parasitemkIV;
table 
DientamoebaInfection*Overseastreatment
HymenolepsisInfection*Overseastreatment
AmboebiasisInfection*Overseastreatment
ParagonimusInfection*Overseastreatment
AscarisInfection*Overseastreatment
ClonorchisInfection*Overseastreatment
GiardiaInfection*Overseastreatment
TrichurisInfection*Overseastreatment
HookwormInfection*Overseastreatment
SchistoInfection*Overseastreatment
StrongyInfection*Overseastreatment;
run;

*Schistosoma Category;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa";
class gender agecategory Albendazole(ref="0");
model SchistoInfection = Albendazole gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa" and Ivermectin = 0;
class gender agecategory Albendazole(ref="0");
model SchistoInfection = Albendazole gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa";
class gender agecategory Prazinquantel(ref="0");
model SchistoInfection = Prazinquantel gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Prazinquantel in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa" and Ivermectin = 0;
class gender agecategory Prazinquantel(ref="0");
model SchistoInfection = Prazinquantel gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Prazinquantel in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Strongyloides Infection category;

proc genmod data = parasitemkIV descending;
where regioncategory = "Asia";
class gender agecategory Albendazole(ref="0");
model StrongyInfection = Albendazole gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Asia';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Asia";
class gender agecategory Ivermectin(ref="0");
model StrongyInfection = Ivermectin gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Ivermectin in Asia';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa";
class gender agecategory Albendazole(ref="0");
model StrongyInfection = Albendazole gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa";
class gender agecategory Ivermectin(ref="0");
model StrongyInfection = Ivermectin gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Ivermectin in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Other Helminths Category;

proc freq data = parasitemkIV;
table PathogenicParasite PathogenicParasite*Regioncategory HelminthInfection*Regioncategory ProtozoanInfection*Regioncategory;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Asia";
class gender agecategory Albendazole(ref="0");
model HelminthInfection = Albendazole gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Asia';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Asia";
class gender agecategory Ivermectin(ref="0");
model HelminthInfection = Ivermectin gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Ivermectin in Asia';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa";
class gender agecategory Albendazole(ref="0");
model HelminthInfection = Albendazole gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa";
class gender agecategory Ivermectin(ref="0");
model HelminthInfection = Ivermectin gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Ivermectin in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Protozoans Category;

proc freq data = parasitemkIV;
table PathogenicParasite PathogenicParasite*Regioncategory HelminthInfection*Regioncategory ProtozoanInfection*Regioncategory;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Asia";
class gender agecategory txtdoc(ref="0");
model ProtozoanInfection = txtdoc gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Asia';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa";
class gender agecategory txtdoc(ref="0");
model ProtozoanInfection = txtdoc gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc freq data = parasitemkIV;
table ProtozoanInfection*Txtdoc/chisq;
where regioncategory = "Sub-Saharan Africa";
run;

proc freq data = parasitemkIV;
table ProtozoanInfection*Txtdoc/chisq;
where regioncategory = "Asia";
run;

*Updating the abstract;

proc sort data = parasitemkIV; by rhayear; run;

proc freq data = parasitemkIV;
tables txtdoc*rhayear/norow nopercent missing;
run;

*Looking at excluding any participant under 2 years old: the main model results did not;

data parasitetest;
set parasitemkIV;
where age ge 2;
run;

proc genmod data = parasitetest descending;
class gender agecategory regioncategory;
model positivecase = txtdoc gender agecategory regioncategory/type3 dist=bin link=log;
title 'Documentation Effect';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitetest descending;
class gender agecategory regioncategory Overseastreatment (ref="No Documented Treatment");
model positivecase = Overseastreatment gender agecategory regioncategory/type3 dist=bin link=log;
title 'Specific Treatment';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Proportion of Refugees who got an RHA;

proc contents data = parasitemkI;
run;

proc freq data = parasitemkI;
table year rhayear treatment1 treatment2 treatment3 treatment4 arrivalstatus rhayear*arrivalstatus;
run;

proc contents data = parasitemkIV;
run;

proc freq data = parasitemkIV;
table rhayear*arrivalstatus rhayear*EOSINOPHILIAPRESENT EOSINOPHILIAPRESENT OVAPARASCREEN SCHISTOSERO STRONGYSERO;
run;

proc freq data = parasitemkIV;
table PARASITESCREENING EOSINOPHILIAPRESENT OVAPARASCREEN SCHISTOSERO STRONGYSERO;
run;

*Eosinophilia assesment;

proc freq data = parasitemkIV;
table EOSINOPHILIAPRESENT EOSINOPHILIAPRESENT*positivecase EOSINOPHILIAPRESENT*helminthInfection EOSINOPHILIAPRESENT*protozoanInfection / nopercent nocol;
run;

*Supplemental Table:breakdown of intestinal parasite infections by age, gender region of origin;

proc freq data = parasitemkIV;
table PathogenicParasite Positivecase HelminthInfection ProtozoanInfection;
run;

proc freq datat = parasitemkIV;
table agecategory*PathogenicParasite agecategory*Positivecase agecategory*HelminthInfection agecategory*ProtozoanInfection/nopercent nocol missing chisq;
run;

proc freq datat = parasitemkIV;
table gender*PathogenicParasite gender*Positivecase gender*HelminthInfection gender*ProtozoanInfection/nopercent nocol missing chisq;
run;

proc freq datat = parasitemkIV;
table Regioncategory*PathogenicParasite Regioncategory*Positivecase Regioncategory*HelminthInfection Regioncategory*ProtozoanInfection/nopercent nocol missing chisq;
run;

proc freq data = parasitemkIV;
table 
DientamoebaInfection*Regioncategory
HymenolepsisInfection*Regioncategory
AmboebiasisInfection*Regioncategory
ParagonimusInfection*Regioncategory
AscarisInfection*Regioncategory
ClonorchisInfection*Regioncategory
GiardiaInfection*Regioncategory
TrichurisInfection*Regioncategory
HookwormInfection*Regioncategory
SchistoInfection*Regioncategory
StrongyInfection*Regioncategory/nopercent nocol missing;
run;

*Average number of parasitic infections per refugee;

proc means data = parasitemkIV n min mean max;
where positivecase = 1;
var total;
run;

proc freq data = parasitemkIV;
table positivecase*total;
run;

proc freq data = parasitemkIV;
table positivecase;
run;

*How many participants did not recieve either o&P or serology screening;

data parasitemkIV;
set parasitemkIV;
if OVAPARASCREEN ="Not Done" and SCHISTOSERO = "Not Done" and STRONGYSERO = "Not Done" then screeningcheck = 0; else screeningcheck = 1;
run;

proc freq data = parasitemkIV;
table positivecase screeningcheck screeningcheck*positivecase;
run;

proc freq data = parasitemkIV;
table txtdoc*positivecase;
run;

proc freq data = parasitemkIV;
tables pathogenicParasite*regioncategory;
run;

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

/*CDC Guidelines

Albendazole > Soil Transmited helminth infection
Ivermectin > Strongyloides infection
Prazinquantel > Schistosoma infection */

/*Parasitic infections in the dataset

if (DIENTA = "Negative" or DIENTA = "NEGATIVE" or DIENTA = "NOT_DONE" or DIENTA = "Not Done" or DIENTA = "Pending" or DIENTA = " ") then DientamoebaInfection = 0; else DientamoebaInfection = 1;
if (HYMENO = "Negative" or HYMENO = "NEGATIVE" or HYMENO = "Not Done" or HYMENO = "NOT_DONE" or HYMENO = "Pending" or HYMENO = " ") then HymenolepsisInfection = 0; else HymenolepsisInfection = 1;
if (AMBOEBIASIS = "Negative" or AMBOEBIASIS = "NEGATIVE" or AMBOEBIASIS = "Not Done" or AMBOEBIASIS = "NOT_DONE" or AMBOEBIASIS = "Pending" or AMBOEBIASIS = " ")  then AmboebiasisInfection = 0; else AmboebiasisInfection = 1;
if (PARAGONIMUS = "Negative" or PARAGONIMUS = "NEGATIVE" or PARAGONIMUS = "Not Done" or PARAGONIMUS = "NOT_DONE" or PARAGONIMUS = "Pending" or PARAGONIMUS = " ") then ParagonimusInfection = 0; else ParagonimusInfection = 1;
if (ASCARIS = "Negative" or ASCARIS = "NEGATIVE" or ASCARIS = "Not Done" or ASCARIS = "NOT_DONE" or ASCARIS = "Pending" or ASCARIS = " ") then AscarisInfection = 0; else AscarisInfection = 1;
if (SCHISTOSOMIASIS = "Negative" or SCHISTOSOMIASIS = "NEGATIVE" or SCHISTOSOMIASIS = "Not Done" or SCHISTOSOMIASIS = "NOT_DONE" or SCHISTOSOMIASIS = "Pending" or SCHISTOSOMIASIS = " ")  then SchistosomiasisInfection = 0; else SchistosomiasisInfection = 1;
if (CLONORCHIS = "Negative" or CLONORCHIS = "NEGATIVE" or CLONORCHIS = "Not Done" or CLONORCHIS = "NOT_DONE" or CLONORCHIS = "Pending" or CLONORCHIS = " ")  then ClonorchisInfection = 0; else ClonorchisInfection = 1;
if (STRONGYLOIDES = "Negative" or STRONGYLOIDES = "NEGATIVE" or STRONGYLOIDES = "Not Done" or STRONGYLOIDES = "NOT_DONE" or STRONGYLOIDES = "Pending" or STRONGYLOIDES = " ")  then StrongyloidesInfection = 0; else StrongyloidesInfection = 1;
if (GIARDIA = "Negative" or GIARDIA = "NEGATIVE" or GIARDIA = "Not Done" or GIARDIA = "NOT_DONE" or GIARDIA = "Pending" or GIARDIA = " ")then GiardiaInfection = 0; else GiardiaInfection = 1;
if (TRICHURIS = "Negative" or TRICHURIS = "NEGATIVE" or TRICHURIS = "Not Done" or TRICHURIS = "NOT_DONE" or TRICHURIS = "Pending" or TRICHURIS = " ") then TrichurisInfection = 0; else TrichurisInfection = 1;
if (HOOKWORM = "Negative" or HOOKWORM = "NEGATIVE" or HOOKWORM = "Not Done" or HOOKWORM = "NOT_DONE" or HOOKWORM = "Pending" or HOOKWORM = " ") then HookwormInfection = 0; else HookwormInfection = 1;
if (SchistosomiasisInfection = 1 or schistosomaserology = 1) then SchistoInfection = 1; else SchistoInfection = 0; 
if (StrongyloidesInfection = 1 or strongyloidesserology = 1) then StrongyInfection = 1; else StrongyInfection = 0; 
if (HymenolepsisInfection = 1 or ParagonimusInfection = 1 or AscarisInfection = 1 or ClonorchisInfection = 1 or TrichurisInfection =1 or HookwormInfection = 1) then HelminthInfection = 1; else HelminthInfection = 0;
if (AmboebiasisInfection = 1 or GiardiaInfection = 1 or DientamoebaInfection = 1) then Protozoaninfection = 1; else Protozoaninfection = 0; */


/*Exposure of interest

if (Treatment1 = "Albendazole" or Treatment2 = "Albendazole" or Treatment3 = "Albendazole" or Treatment4 = "Albendazole") then Albendazole = 1; else Albendazole = 0;
if (Treatment1 = "Ivermectin" or Treatment2 = "Ivermectin" or Treatment3 = "Ivermectin" or Treatment4 = "Ivermectin") then Ivermectin = 1; else Ivermectin = 0;
if (Treatment1 = "Praziquantel" or Treatment2 = "Praziquantel" or Treatment3 = "Praziquantel" or Treatment4 = "Praziquantel") then Prazinquantel = 1; else Prazinquantel = 0;

if (Treatment1 = "Albendazole" or Treatment2 = "Albendazole" or Treatment3 = "Albendazole" or Treatment4 = "Albendazole") then OverseasTreatment = "Albendazole";
if (Treatment1 = "Albendazole" or Treatment2 = "Albendazole" or Treatment3 = "Albendazole" or Treatment4 = "Albendazole") and (Treatment1 = "Ivermectin" or Treatment2 = "Ivermectin" or Treatment3 = "Ivermectin" or Treatment4 = "Ivermectin") then OverseasTreatment = "Albendazole & Ivermectin";
if (Treatment1 = "Albendazole" or Treatment2 = "Albendazole" or Treatment3 = "Albendazole" or Treatment4 = "Albendazole") and (Treatment1 = "Praziquantel" or Treatment2 = "Praziquantel" or Treatment3 = "Praziquantel" or Treatment4 = "Praziquantel") then OverseasTreatment = "Albendazole & Praziquantel";
If Treatment1 ne ' ' then TreatmentCheck=1; Else Treatmentcheck=0;

/*Recoding the presumptive treatment variable exclusively based on CDC recommendation guidelines from 2006 to 2016 then reruning the models for schisto, strongy and soil transmitted helminths;
*1999 Albendazole in Sub saharan africa and Asian refugees;
*2005 Albendazole + Ivermectin in Asian refugees, Albendazole + Ivermectin + Prazinquantel for Sub-Saharan Africa Refugees except (Angola, Cameroon, Chad, Democratic republic of congo, Guinea, Sudan, Nigerian and Gabon;
*2008 Albendazole to Middle Eastern Refugees;
*2013 Albendazole + Ivermectin for Asian, Sub Saharan African, Middle eastern, Latin American & Carribean Refugees;

*For the analysis, to sort of account for the lack of EDN data prior to 2010 and a lag time between CDC recommendations and implementations, We will code refugees who arrived between 2006-2010 as presumptively treated with albendazole
and anyone who arrived between 2010 and 2016 as presumptively treated with Albendazole & Ivermectin;*/


/*if rhayear lt 2010 then EDNtreatment = 0; else EDNtreatment = 1;

if (Treatment1 = " ") and (region ne "Eastern Europe") and (rhayear lt 2010) then OverseasTreatment = "Presumptive Albendazole";
else if (Treatment1 = " ") and (region = "Eastern Europe") and (rhayear lt 2010) then OverseasTreatment = "No Documented Treatment";
else if (Treatment1 = " ")and (rhayear ge 2010)then OverseasTreatment = "No Documented Treatment"; 

if(region ne "Eastern Europe") and (rhayear lt 2010) then OverseasTreatment2 = "Presumptive Albendazole";
else if (region ne "Eastern Europe") and (rhayear ge 2010) then OverseasTreatment2 = "Presumptive Albendazole & Ivermectin"; 
else OverseasTreatment2 = "No Presumptive Treatment"; */

/*Region of origin

if (country = "BURUNDI" or country = "CAMEROON"  or country = "DJIBOUTI" or country = "CONGO, DEMOCRATIC REPUBLIC" or country = "CONGO" or country = "ERITREA" or country = "ETHIOPIA" or country = "GAMBIA, THE" or country = "GUINEA" or country = "IVORY COAST" or country = "RWANDA" or country = "KENYA" or country = "LIBERIA" or country = "MALI" or country = "SIERRA LEONE" or country = "SOMALIA" or country = "SUDAN" or country = "TANZANIA, UNITED REPUBLIC OF" or country = "TOGO" or country = "UGANDA" or country = "ZIMBABWE") then Region = "Sub-Saharan Africa";
if (country = "AFGHANISTAN" or country = "IRAN" or country = "IRAQ" or country = "WEST BANK" or country = "SYRIA") then Region = "Middle-East";
if (country = "ARMENIA" or country = "KYRGYZSTAN" or country = "UZBEKISTAN" or country = "BELARUS" or country = "KAZAKHSTAN " or country = "RUSSIA" or country = "MOLDOVA" or country = "UKRAINE" or country = "LATVIA") then Region = "Eastern Europe"; 
if (country = "BHUTAN" or country = "BURMA" or country = "TIBET" or country =  "CAMBODIA" or country =  "CHINA" or country =  "LAOS/HMONG"  or country = "THAILAND" or country = "SRI LANKA" or country = "NEPAL" or country = "PHILIPPINES" or country = "TIBET" or country = "VIETNAM") then Region = "Asia";
if (country = "CUBA" or country = "HAITI" or country = "MEXICO" or country = "HONDURAS" or country = "EL SALVADOR" or country = "COLOMBIA") then Region = "Americas";
if (region = "Middle-East" or region = "Americas" or region = "Eastern Europe") then regioncategory = "Other"; else regioncategory = region; */

/*Other Covariates

age = (arrivaldate - dob)/365.25;
if gender = "M" then male = 1; 
if gender = "F" then male = 0;
if age lt 18 then agecategory = 1; else agecategory = 2; */

******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;
******************************************************************************************************************************************************************************************************************************************************************;

*Making the updated tables for the manuscript (excluding participants under the age of 1);
*Table 1: Characteristics of Primary refugees screened in minnesota by presumptive treatment status (2006-2016);
*Table 2: Prevalence Ratio of parasitic infections by CDC presumptive treatment schedule among primary MN refugees (2006-2016);
*Table 3: Prevalence Ratio of Strongyloides and Soil transmitted helminth infections by CDC Presumptive treatment schedule, stratified by age, sex and region of origin (2006-2016);
*Supplemental figure 1: Unadjusted prevalence of inestinal parasite by overseas treatment status among MN primary refugess (2006-2016);
*Supplemental Table 1: Characteristics of Primary refugees screened in minnesota by overseas treatment status (2006-2016);
*Supplemental Table 2: Prevalence ratio of strongyloides and soil transmitted helminths by documented overseas treatment among MN primary refugees (2006 - 2016);

options ps = 3000 ls = 120 nofmterr;
options nocenter formdlim = '^' ps = 500 ls = 132 formchar = '           ';
libname mylib 'E:\Professional Folder\MDH _ IAC\MDH\Current Projects\Updated 2018 Parasite Manuscript\Parasite Project\Codes and Datasets\Datasets';

data mylib.parasitemkIV;
set parasitemkIV;
run;

data parasitemkIV;
set mylib.parasitemkIV;
run;

proc contents data = parasitemkIV;
run;

*Excluding participants under the age of 1 (loose 27788 - 23168 observations);

data parasitemkIV;
set parasitemkIV;
if age lt 1 then delete;
run;

proc means data = parasitemkIV min mean median max;
var age;
run;

proc univariate data = parasitemkIV;
var age;
histogram age / normal;
run;

*Recoding the presumptive treatment variable exclusively based on CDC recommendation guidelines from 2006 to 2016 then reruning the models for schisto, strongy and soil transmitted helminths;
*1999 Albendazole in Sub saharan africa and Asian refugees;
*2005 Albendazole + Ivermectin in Asian refugees, Albendazole + Ivermectin + Prazinquantel for Sub-Saharan Africa Refugees except (Angola, Cameroon, Chad, Democratic republic of congo, Guinea, Sudan, Nigerian and Gabon;
*2008 Albendazole to Middle Eastern Refugees;
*2013 Albendazole + Ivermectin for Asian, Sub Saharan African, Middle eastern, Latin American & Carribean Refugees;

*For the analysis, to sort of account for the lack of EDN data prior to 2010 and a lag time between CDC recommendations and implementations, We will code refugees who arrived between 2006-2010 as presumptively treated with albendazole
and anyone who arrived between 2010 and 2016 as presumptively treated with Albendazole & Ivermectin;
 
proc freq data = parasitemkIV;
tables region;
run;

data parasitemkIV;  
set parasitemkIV;
Length OverseasTreatment2 $40.;
if (region ne "Eastern Europe") and (rhayear lt 2010) then OverseasTreatment2 = "Presumptive Albendazole";
else if (region ne "Eastern Europe") and (rhayear ge 2010) then OverseasTreatment2 = "Presumptive Albendazole & Ivermectin";
else OverseasTreatment2 = "No Presumptive Treatment"; 
run;

proc freq data = parasitemkIV;
tables region Overseastreatment Overseastreatment2;
run;

*Making consistent region categories for the manuscript;

data parasitemkIV;
set parasitemkIV;
length regioncategory $40.;
if (region = "Middle-East" or region = "Americas" or region = "Eastern Europe") then regioncategory = "Other"; else regioncategory = region;
run;

proc sort data = parasitemkIV; by regioncategory; run;

proc freq data = parasitemkIV;
tables regioncategory*Overseastreatment/nopercent nocol missing;
run;

proc freq data = parasitemkIV;
tables regioncategory*Overseastreatment2/nopercent nocol missing;
run;

proc sort data = parasitemkIV; by rhayear; run;

proc freq data = parasitemkIV;
tables overseastreatment*rhayear;
run;

*compare the single treatment exposure to the overeas treatment categories for agreement;
* There is 866 refugees who received ivermectin on top of albendazole and prazinquantel (likely in Sub-Saharan Africa. We could do an if then statement to differentiate the Albendazole & Prazinquantel vs A & I & P (n=866) group;

proc freq data = parasitemkIV;
tables treatment1 treatment2 treatment3 treatment4;
run;

proc freq data = parasitemkIV;
table albendazole*Overseastreatment Ivermectin*Overseastreatment Prazinquantel*Overseastreatment albendazole*prazinquantel;
run;

*Table 1: Characteristics of Primary refugees screened in minnesota by presumptive treatment status (2006-2016);

proc freq data = parasitemkIV;
tables agecategory*Overseastreatment2/nopercent norow missing;
run;

proc freq data = parasitemkIV;
tables gender*Overseastreatment2/nopercent norow missing;
run;

proc freq data = parasitemkIV;
tables regioncategory*Overseastreatment2/nopercent norow missing;
run;

*Table 2: Prevalence Ratio of parasitic infections by CDC presumptive treatment schedule among primary MN refugees (2006-2016);

*This model is to determine if the combined presumptive treatment (Albendazole & Ivermectin) is more effective than albendazole only for reducing infections;

proc freq data = parasitemkIV; tables positivecase region gender agecategory overseastreatment2; run;

proc freq data = parasitemkIV; where overseastreatment2 ne "No Presumptive Treatment";
tables overseastreatment2*positivecase agecategory*positivecase gender*positivecase regioncategory*positivecase / nocol nopercent missing; 
run; 

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment";
class regioncategory (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model positivecase = Overseastreatment2 regioncategory gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Table 3: Prevalence Ratio of Strongyloides and Soil transmitted helminth infections by CDC Presumptive treatment schedule, stratified by age, sex and region of origin (2006-2016);

*Repeating the analysis for strongyloides and other soil transmitted helminths;

*Strongyloides;

proc freq data = parasitemkIV; tables strongyinfection region gender agecategory overseastreatment2; run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on age category;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and agecategory = 1;
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and agecategory = 2;
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on gender;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and gender = "F";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and gender = "M";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on Region;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Asia";
class region overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Sub-Saharan Africa";
class region overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Middle-East";
class region overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model strongyinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Helminth Infection;

proc freq data = parasitemkIV; tables helminthinfection region gender agecategory overseastreatment2; run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on age category;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and agecategory = 1;
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and agecategory = 2;
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on gender;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and gender = "F";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and gender = "M";
class region (ref="Sub-Saharan Africa") overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Stratified on Region;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Asia";
class region overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Region = "Sub-Saharan Africa";
class region  overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 region gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "No Presumptive Treatment" and Regioncategory = "Other";
class regioncategory overseastreatment2(ref= "Presumptive Albendazole") agecategory gender;
model helminthinfection = Overseastreatment2 regioncategory gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Supplemental figure 1: Unadjusted prevalence of inestinal parasite by overseas treatment status among MN primary refugess (2006-2016);

*You should restrict the figure to only those in presumptive albendazole + presumptive albendazole & Ivermectin groups;

proc freq data = parasitemkIV;
table PARASITESCREENING overseastreatment2;
run; 

proc freq data = parasitemkIV;
where PARASITESCREENING eq "Screened";
table 
DientamoebaInfection*Overseastreatment2
HymenolepsisInfection*Overseastreatment2
AmboebiasisInfection*Overseastreatment2
ParagonimusInfection*Overseastreatment2
AscarisInfection*Overseastreatment2
ClonorchisInfection*Overseastreatment2
GiardiaInfection*Overseastreatment2
TrichurisInfection*Overseastreatment2
HookwormInfection*Overseastreatment2
SchistoInfection*Overseastreatment2
StrongyInfection*Overseastreatment2 / nopercent nocol missing;
run;

proc freq data = parasitemkIV;
where overseastreatment2 ne "No Presumptive Treatment";
table 
DientamoebaInfection*Overseastreatment2
HymenolepsisInfection*Overseastreatment2
AmboebiasisInfection*Overseastreatment2
ParagonimusInfection*Overseastreatment2
AscarisInfection*Overseastreatment2
ClonorchisInfection*Overseastreatment2
GiardiaInfection*Overseastreatment2
TrichurisInfection*Overseastreatment2
HookwormInfection*Overseastreatment2
SchistoInfection*Overseastreatment2
StrongyInfection*Overseastreatment2 / nopercent nocol missing;
run;

*Supplemental figure 2: Unadjusted prevalence of inestinal parasite by region of origin (2006-2016);

*You should restrict the figure to only those in presumptive albendazole + presumptive albendazole & Ivermectin groups;

proc freq data = parasitemkIV;
where overseastreatment2 ne "No Presumptive Treatment";
table 
DientamoebaInfection*regioncategory
HymenolepsisInfection*regioncategory
AmboebiasisInfection*regioncategory
ParagonimusInfection*regioncategory
AscarisInfection*regioncategory
ClonorchisInfection*regioncategory
GiardiaInfection*regioncategory
TrichurisInfection*regioncategory
HookwormInfection*regioncategory
SchistoInfection*regioncategory
StrongyInfection*regioncategory / nopercent nocol missing;
run;

*Supplemental Table 1: Characteristics of Primary refugees screened in minnesota by overseas treatment status (2010-2016);

proc freq data = parasitemkIV;
tables Overseastreatment;
run;

proc freq data = parasitemkIV;
where overseastreatment ne "Presumptive Albendazole";
tables agecategory*Overseastreatment/nopercent norow missing;
run;

proc freq data = parasitemkIV;
where overseastreatment ne "Presumptive Albendazole";
tables gender*Overseastreatment/nopercent norow missing;
run;

proc freq data = parasitemkIV;
where overseastreatment ne "Presumptive Albendazole";
tables regioncategory*Overseastreatment/nopercent norow missing;
run;

*Supplemental Table 2: Prevalence ratio of strongyloides and soil transmitted helminths by documented overseas treatment among MN primary refugees (2010 - 2016);

*This model is to determine if the combined presumptive treatment (Albendazole & Ivermectin) is more effective than albendazole only for reducing infections;

proc freq data = parasitemkIV; tables positivecase region gender agecategory overseastreatment; run;

proc freq data = parasitemkIV; where overseastreatment ne "Presumptive Albendazole";
tables overseastreatment*positivecase agecategory*positivecase gender*positivecase regioncategory*positivecase / nocol nopercent missing; 
run; 

proc genmod data = parasitemkIV descending;
where overseastreatment2 ne "Presumptive Albendazole";
class regioncategory (ref="Sub-Saharan Africa") overseastreatment(ref= "No Documented Treatment") agecategory gender;
model positivecase = Overseastreatment regioncategory gender agecategory/type3 dist=bin link=log;
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run; 

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Supplemental Table 3;

*Strongyloides Infection category;

proc freq data = parasitemkIV;
tables EDNtreatment;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Asia" and EDNtreatment = 1;
class gender agecategory Albendazole(ref="0");
model StrongyInfection = Albendazole gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Asia';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Asia" and EDNtreatment = 1;
class gender agecategory Ivermectin(ref="0");
model StrongyInfection = Ivermectin gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Ivermectin in Asia';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa" and EDNtreatment = 1;
class gender agecategory Albendazole(ref="0");
model StrongyInfection = Albendazole gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa" and EDNtreatment = 1;
class gender agecategory Ivermectin(ref="0");
model StrongyInfection = Ivermectin gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Ivermectin in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

*Other Helminths Category;

proc freq data = parasitemkIV;
table PathogenicParasite PathogenicParasite*Regioncategory HelminthInfection*Regioncategory ProtozoanInfection*Regioncategory;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Asia" and EDNtreatment = 1;
class gender agecategory Albendazole(ref="0");
model HelminthInfection = Albendazole gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Asia';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Asia" and EDNtreatment = 1;
class gender agecategory Ivermectin(ref="0");
model HelminthInfection = Ivermectin gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Ivermectin in Asia';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa" and EDNtreatment = 1;
class gender agecategory Albendazole(ref="0");
model HelminthInfection = Albendazole gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Albendazole in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;

proc genmod data = parasitemkIV descending;
where regioncategory = "Sub-Saharan Africa" and EDNtreatment = 1;
class gender agecategory Ivermectin(ref="0");
model HelminthInfection = Ivermectin gender agecategory /type3 dist=bin link=log;
title 'Specific Treatment effect of Ivermectin in Sub-Saharan Africa';
ODS output ParameterEstimates = reg_coef;
run;

proc print data=reg_coef;
run;

Data back_transf;
set reg_coef;
RR = exp(estimate);
left_CI=exp(LowerWaldCL);
right_CI = exp(UpperWaldCL);
run;

proc print;
var parameter RR left_CI right_CI;
run;
