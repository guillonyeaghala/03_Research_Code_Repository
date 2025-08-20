Options ps = 3000 ls = 120 nofmterr;
options nocenter formdlim = '^' ps = 500 ls = 100 formchar = '           ';
libname mylib 'C:\Users\onyea005\Desktop\WBOB\Leg Length\AACR 2016\Leg lengths\SAS Program _ Dataset\SAS Datasets';/*output leg datasets*/

 *This is the final dataset used for the main models;

* the variables of interest were leg, height01, sit_ht01, ratio prop;
* the quartiles were legmq heightmq sit_htmq ratiomq propmq for men;
* the quartiles were legfq heightfq sit_htfq ratiofq propfq for women;

*sex-specific quartiles for both genders combined: They were already sex specific in the quartile files, but with this code we just have to specify the gender in the where statement of the model to make them work;

data both;
set mylib.mquart mylib.fquart;
if sex='M' then do; weightqb=weightmq; heightqb=heightmq; legqb=legmq; sit_htqb=sit_htmq; ratioqb=ratiomq; propqb=propmq;
end;
else if sex='F' then  do; weightqb=weightfq; heightqb=heightfq; legqb=legfq; sit_htqb=sit_htfq; ratioqb=ratiofq; propqb=propfq; end;
if hom29 ge 7 and hom29 le v1age01 then agestsm=hom29 ; else agestsm=0;
run;

proc contents data = both;
run;

proc means;
var  ratio;
class propqb;
run;

proc means;
var  prop;
class propqb;
run;

proc freq;
table propqb*ratioqb;
run;

*Correlation of leg lentgh, trunk length, and heigth;
proc corr data=both;
var leg sit_ht01 height01;
run;

proc corr data=both;
var legsdsex torsdsex htsdsex;
run;

*
continuous variables
height01
leg
sit_ht01

standardized variables
htsdsex
legsdsex
torsdsex

categorical variables
heightq
legqb
sit_htqb
;

Proc freq data = both;
table race;
run;

/*Finding Person years*/

proc print data = both (obs=50);
run;

data both;
set both;
person_years = CRCdate - V1DATE01;
run;

/* You do not need person years in this case, but number of events accrued ofer the follow up time, either using the CensDAT6 for censoring date, or the CRCdate, which can be done checking proc contents */

/*Remember to divide by 365.25 to convert the actual measurements from days to years*/

proc means data = both min max median;
where CRC = 1;
var person_years;
run;
 
*running some descriptive analyses for the results section;

proc means data = both;
var age1;
run; 

proc freq data=both;
tables race sex;
run;

proc means data = both median std;
where sex = 'M'; 
var height01 leg;
run;

proc means data = both median std;
where sex = 'F'; 
var height01 leg;
run;

/* Hazard Ratio for height quartiles in model 1 (Adjusted for age, sex, race and study center) */
proc phreg data=both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Sex stratified analysis;

*Hazard Ratios among Men;

proc phreg data = both;
where sex='M';
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex='M';
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex='M';
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Hazard Ratios among Women;

proc phreg data = both;
where sex='F';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex='F';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex='F';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Interaction with sex;

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 heightqb*sex /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 legqb*sex /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 sit_htqb*sex /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Race Stratified Analysis;

*Hazard Ratios among Whites;

proc phreg data = both;
where race='W';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where race='W';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where race='W';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Hazard Ratios among Blacks;

proc phreg data = both;
where race='B';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where race='B';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where race='B';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Interaction with Race;

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 heightqb*race/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 legqb*race/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 sit_htqb*race/rl;
title "CRC cancer-body size multivar adjusted";
run;

*Sex & Race Stratified Analysis;

*Hazard Ratios among Whites Men;

proc phreg data = both;
where sex = 'M' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex = 'M' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex = 'M' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Hazard Ratios among Black Men;

proc phreg data = both;
where sex = 'M' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex = 'M' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex = 'M' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Hazard Ratios among White Women;

proc phreg data = both;
where sex = 'F' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex = 'F' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex = 'F' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Hazard Ratios among Black Women;

proc phreg data = both;
where sex = 'F' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex = 'F' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex = 'F' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Using Residuals;

*Residual of height;
proc reg data=both;
model htsdsex =legsdsex ;
output out=resid1 r=r_htsex;
run;

proc print data = resid1 (obs=50);
run;

proc reg data=both;
model height01 =leg;
output out=resid2 r=r_height;
run;

proc print data = resid2 (obs=50);
run;

*Merging residuals to the main dataset;

proc print data = both (obs=50);
run;

proc sort data = both;
by id;
run;

proc sort data = resid2;
by id;
run;

data bothmkI;
merge  both resid2;
by id;
run;

proc print data = bothmkI (obs=50);
run;

*Merging residuals does not work, have to save them directly to the main dataset;

proc reg data=both;
model height01 =leg;
output out=both r=leg_r_height;
run;

proc reg data=both;
model height01 =sit_ht01;
output out=both r=tor_r_height;
run;

proc reg data=both;
model htsdsex =legsdsex ;
output out=both r=leg_r_htsex;
run;

proc reg data=both;
model htsdsex = torsdsex ;
output out=both r=tor_r_htsex;
run;

data both;
set both;
if race='W' then race2=1;
else if race='B' then race2=2;
if sex='M' then sex2=1;
else if sex='F' then sex2=2;
run;

proc sort data = both;
by race;
run;

proc reg data=both;
model legsdsex  = htsdsex race2;
output out=both r=r_legsdsex;
run;

proc reg data=both;
model leg = height01 sex2 race2 ;
output out=both r=r_leg;
run;

proc print data = both (obs=50);
run;

data mylib.bothmkI;
set both;
run;

proc print data = mylib.bothmkI (obs=50);
run;

*Using Residuals;

proc phreg data = mylib.bothmkI;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 r_legsdsex/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = mylib.bothmkI;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 r_leg/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = mylib.bothmkI;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1/rl;
title "CRC cancer-body size multivar adjusted";
run;

* Simultaneous Adjustment for leg length and height with residuals;

* categorical leg length ang heigth;

proc phreg data = mylib.bothmkI;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0') heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb heightqb sex WHR1 sexhrt1/rl;
title "CRC cancer-body size multivar adjusted";
run;

*categorical leg length ang continuous heigth;

proc phreg data = mylib.bothmkI;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb heightqb sex WHR1 sexhrt1 r_legsdsex height01/rl;
title "CRC cancer-body size multivar adjusted";
run;

*continuous leg length and height;

proc phreg data = mylib.bothmkI;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3");
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 height01 leg sex WHR1 sexhrt1 r_legsdsex/rl;
title "CRC cancer-body size multivar adjusted";
run;

*residuals for leg length and height;

proc phreg data = mylib.bothmkI;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 r_legsdsex/rl;
title "CRC cancer-body size multivar adjusted";
run;

*both residuals and height as continuous;

proc phreg data = mylib.bothmkI;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3");
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 height01 sex WHR1 sexhrt1 r_legsdsex/rl;
title "CRC cancer-body size multivar adjusted";
run;

*********************************************************************************************************************************************************************************************************************************************************

*August Tables Updates;

*Table 2;

proc freq data=both;
tables crc*legqb;
run;

proc freq data=both;
tables crc*heightqb;
run;

proc freq data=both;
tables crc*sit_htqb;
run; 

*Table 3;

proc freq data=both;
where sex='M';
tables crc*legqb;
run; 

proc freq data=both;
where sex='F';
tables crc*legqb;
run; 

proc freq data=both;
where race='W';
tables crc*legqb;
run; 

proc freq data=both;
where race='B';
tables crc*legqb;
run; 

proc freq data=both;
where sex='M';
tables crc*heightqb;
run; 

proc freq data=both;
where sex='F';
tables crc*heightqb;
run; 

proc freq data=both;
where race='W';
tables crc*heightqb;
run; 

proc freq data=both;
where race='B';
tables crc*heightqb;
run; 

proc freq data=both;
where sex='M';
tables crc*sit_htqb;
run; 

proc freq data=both;
where sex='F';
tables crc*sit_htqb;
run; 

proc freq data=both;
where race='W';
tables crc*sit_htqb;
run; 

proc freq data=both;
where race='B';
tables crc*sit_htqb;
run; 

************************************************************************************************************************************************************************************************************************************************************************************************************************************;

proc phreg data = mylib.bothmkI;
class center educat (ref="1") sexhrt1(ref="0") smoke1(ref="3");
model tpyrCRC*CRC (0) = age1 center educat smoke1 smokyrs1 WHR1 sexhrt1 r_legsdsex/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = mylib.bothmkI;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3");
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sex WHR1 sexhrt1 r_leg/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = mylib.bothmkI;
class center educat (ref="1") sexhrt1(ref="0") smoke1(ref="3");
model tpyrCRC*CRC (0) = age1 center educat smoke1 smokyrs1 WHR1 sexhrt1 r_leg/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = mylib.bothmkI;
class center educat (ref="1") sexhrt1(ref="0") smoke1(ref="3");
model tpyrCRC*CRC (0) = age1 center educat smoke1 smokyrs1 sexhrt1 r_leg/rl;
title "CRC cancer-body size multivar adjusted";
run;

*Residuals stratified by gender;

proc phreg data = mylib.bothmkI;
where sex='M';
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3");
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sex WHR1 sexhrt1 height01 leg_r_height tor_r_height/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = mylib.bothmkI;
where sex='F';
class sex center race educat (ref="1") sexhrt1(ref="1") smoke1(ref="3");
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sex WHR1 sexhrt1 height01 leg_r_height tor_r_height/rl;
title "CRC cancer-body size multivar adjusted";
run;

*Residuals stratified by race;

proc phreg data = mylib.bothmkI;
where race='W';
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3");
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sex WHR1 sexhrt1 height01 leg_r_height tor_r_height/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = mylib.bothmkI;
where race='B';
class sex center race educat (ref="1") sexhrt1(ref="1") smoke1(ref="3");
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sex WHR1 sexhrt1 height01 leg_r_height tor_r_height/rl;
title "CRC cancer-body size multivar adjusted";
run;

********************************************************************************************************************************************************************************************************,

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 heightqb*sex /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 heightqb*race/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex='M';
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex='F';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

/* Hazard Ratio for leg quartiles in model 1 (Adjusted for age, sex, race and study center) */
proc phreg data=both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 legqb*sex /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 legqb*race/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex='M';
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex='F';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

/* Hazard Ratio for sitting height quartiles in model 1 (Adjusted for age, sex, race and study center) */
proc phreg data=both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 sit_htqb*sex /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 sit_htqb*race/rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex='M';
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = both;
where sex='F';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*********************************************************************************************************************************************************************************************;

*Data Analysis Portion;

*Version 2: Adjusted for age, sex, race, study center, education, smoking status, pack-years, ever use for HRT, and WHR since BMI includes height;

*
continuos variables
height01
leg
sit_ht01

stadardized variables
htsdsex
legsdsex
torsdsex

categorical variables
heightq
legqb
sit_htqb
;

proc freq data = both;
tables sexhrt1*sex;
run;

proc means data = both mean;
class heightqb;
var height01;
run;

/* Hazard Ratio for height quartiles in model 1 (Adjusted for age, sex, race and study center) */
proc phreg data=both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 heightqb sex  WHR1 sexhrt1 /rl;
contrast "trend in height" heightqb 160 167 170 177;
title "CRC cancer-body size multivar adjusted";
run;

proc means data = both mean;
class legqb;
var leg;
run;

/* Hazard Ratio for leg quartiles in model 1 (Adjusted for age, sex, race and study center) */
proc phreg data=both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 legqb sex  WHR1 sexhrt1 /rl;
contrast "trend in leg length" legqb 74 78 82 86;
title "CRC cancer-body size multivar adjusted";
run;

proc means data = both mean;
class sit_htqb;
var sit_ht01;
run;

/* Hazard Ratio for sitting height quartiles in model 1 (Adjusted for age, sex, race and study center) */
proc phreg data=both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
contrast "trend in sitting height" sit_htqb 84 88 90 93;
title "CRC cancer-body size multivar adjusted";
run;

*Mutual adjustment: Kinda pointless in this case;

proc phreg data=both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0') legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 heightqb legqb sex  WHR1 sexhrt1 /rl;
contrast "trend in height" heightqb 160 167 170 177;
contrast "trend in leg length" legqb 74 78 82 86;
title "CRC cancer-body size multivar adjusted";
run;

*Stratified analysis by sex;

proc phreg data=both;
where sex='M';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 legqb WHR1 sexhrt1 /rl;
contrast "trend in leg length" legqb 79 82 85 90;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
where sex='M';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 heightqb WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
where sex='M';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 sit_htqb WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
where sex='F';
class sex center race educat (ref="1") sexhrt1(ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 legqb  WHR1 sexhrt1 /rl;
contrast "trend in leg length" legqb 72 75 78 82;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
where sex='F';
class sex center race educat (ref="1") sexhrt1(ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 heightqb  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
where sex='F';
class sex center race educat (ref="1") sexhrt1(ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 sit_htqb  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Stratified analysis by race;

proc phreg data=both;
where race='W';
class sex center educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center educat smoke1 legqb sex  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
where race='W';
class sex center  educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center sex educat smoke1 heightqb WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
where race='W';
class sex center educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center sex educat smoke1 sit_htqb WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
where race='B';
class sex center educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center educat smoke1 legqb sex  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
where race='B';
class sex center educat (ref="1") sexhrt1(ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrCRC*CRC (0) = age1 center sex educat smoke1 heightqb  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=both;
where race='B';
class sex center educat (ref="1") sexhrt1(ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrCRC*CRC (0) = age1 center sex educat smoke1 sit_htqb  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc means data = both mean std;
class race;
var leg height01 sit_ht01;
run;

proc means data = both mean std;
class sex;
var leg height01 sit_ht01;
run;

*Interaction with sex;
proc phreg data=both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 legqb sex  WHR1 sexhrt1 sex*legqb /rl;
title "CRC cancer-body size multivar adjusted";
run;

*interaction with race;
proc phreg data=both;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrCRC*CRC (0) = age1 center  race  educat smoke1 legqb sex  WHR1 sexhrt1 race*legqb /rl;
title "CRC cancer-body size multivar adjusted";
run;

*************************************************************************************************************************************************************************

*Repeated Leg length analyses using the 2012 ARIC cancer case files; 

*EAP & Summer 2017 Revisions;
Options ps = 3000 ls = 120 nofmterr;
options nocenter formdlim = '^' ps = 500 ls = 100 formchar = '           ';
libname mylib 'C:\Users\onyea005\Desktop\WBOB\Leg Length\AACR 2016\Leg lengths\SAS Program _ Dataset';/*output leg datasets*/
libname mylib2 'C:\Users\onyea005\Desktop\WBOB\ARIC Data\Updated ARIC cases\CANCER';

*Loading the leg length data and CRC data;

data both;
set mylib.bothmki;
run;

data CRCdata;
set mylib2.crcdata;
run;

proc sort data = both; by ID; run;
proc sort data = crcdata; by ID; run;

data leglength2012;
merge both crcdata;
by id;
run;

proc contents data = leglength2012;
run;

 *This is the final dataset used for the main models;

* the variables of interest were leg, height01, sit_ht01, ratio prop;
* the quartiles were legmq heightmq sit_htmq ratiomq propmq for men;
* the quartiles were legfq heightfq sit_htfq ratiofq propfq for women;
* CRC12, COLONCA12 and RECTALCA12 are the updated CRC outcomes;
* tpyrsany12 is the updated follow up time;

*Descriptive statistics;

*Table1;
 proc freq data = leglength2012;
 tables legqb CRC12*legqb;
 run;

 proc sort data = leglength2012; by magdie; run;

 proc means data = leglength2012;
 where CRC12 = 1;
 var tpyrsany12;
 by legqb;
 run;

proc means data = leglength2012 mean std;
var bmi01 tcal1 v1age01 calcf1 dfib1 alco1  SPRT_I31 logcrp;
by legqb;
run;

proc ttest data = leglength2012;
class legqb;
var bmi01 tcal1 v1age01 calcf1 dfib1 alco1  SPRT_I31 logcrp;
run;

proc freq data = leglength2012;
tables gender*legqb CIGT01*legqb ASPIRINCODE01*legqb racegrp*legqb / norow nopercent chisq; 
run;

proc freq data = leglength2012;
tables CRC12*legqb COLONCA12*legqb RECTALCA12*legqb CRC12*gender CRC12*racegrp;
run;

proc freq data = leglength2012;
tables gender racegrp;
run;

proc means data = leglength2012;
var tpyrsany12;
run;

*Correlation of leg lentgh, trunk length, and heigth;
proc corr data=leglength2012;
var leg sit_ht01 height01;
run;

proc corr data=leglength2012;
var legsdsex torsdsex htsdsex;
run;

proc sort data=leglength2012;by sex; run;

proc corr data=leglength2012;
var leg sit_ht01 height01;
by sex;
run;

proc corr data=leglength2012;
var legsdsex torsdsex htsdsex;
by sex;
run;

proc freq data = leglength2012;
tables CRC;
by sex;
run;

******************************************************************************************************************************************************************************************************************

*Statistical Models;

*running some descriptive analyses for the results section;

proc means data = leglength2012;
var age1;
run; 

proc freq data=leglength2012;
tables race sex;
run;

proc means data = leglength2012 median std;
where sex = 'M'; 
var height01 leg;
run;

proc means data = leglength2012 median std;
where sex = 'F'; 
var height01 leg;
run;

*********************************************************************************************************************************************************************************************;

*Data Analysis Portion;

*Version 2: Adjusted for age, sex, race, study center, education, smoking status, pack-years, ever use for HRT, and WHR since BMI includes height;

*
continuos variables
height01
leg
sit_ht01

stadardized variables
htsdsex
legsdsex
torsdsex

categorical variables
heightq
legqb
sit_htqb
;

proc freq data = leglength2012;
tables sexhrt1*sex;
run;

proc means data = leglength2012 mean;
class heightqb;
var height01;
run;

/* Hazard Ratio for height quartiles in model 1 (Adjusted for age, sex, race and study center) */
proc phreg data=leglength2012;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 heightqb sex  WHR1 sexhrt1 /rl;
contrast "trend in height" heightqb 160 167 170 177;
title "CRC cancer-body size multivar adjusted";
run;

proc means data = leglength2012 mean;
class legqb;
var leg;
run;

/* Hazard Ratio for leg quartiles in model 1 (Adjusted for age, sex, race and study center) */
proc phreg data=leglength2012;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 legqb sex  WHR1 sexhrt1 /rl;
contrast "trend in leg length" legqb 74 78 82 86;
title "CRC cancer-body size multivar adjusted";
run;

proc means data = leglength2012 mean;
class sit_htqb;
var sit_ht01;
run;

/* Hazard Ratio for sitting height quartiles in model 1 (Adjusted for age, sex, race and study center) */
proc phreg data=leglength2012;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") sit_htqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
contrast "trend in sitting height" sit_htqb 84 88 90 93;
title "CRC cancer-body size multivar adjusted";
run;

*Mutual adjustment: Kinda pointless in this case;

proc phreg data=leglength2012;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") heightqb (ref='0') legqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 heightqb legqb sex  WHR1 sexhrt1 /rl;
contrast "trend in height" heightqb 160 167 170 177;
contrast "trend in leg length" legqb 74 78 82 86;
title "CRC cancer-body size multivar adjusted";
run;

*CRC Subtypes;

*Colon Cancer;
proc phreg data=leglength2012;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*COLONCA12 (0) = age1 center  race  educat smoke1 legqb sex  WHR1 sexhrt1 /rl;
contrast "trend in leg length" legqb 74 78 82 86;
title "CRC cancer-body size multivar adjusted";
run;

*Rectal Cancer;
proc phreg data=leglength2012;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*RECTALCA12 (0) = age1 center  race  educat smoke1 legqb sex  WHR1 sexhrt1 /rl;
contrast "trend in leg length" legqb 74 78 82 86;
title "CRC cancer-body size multivar adjusted";
run;

*Stratified analysis by sex;

proc phreg data=leglength2012;
where sex='M';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 legqb WHR1 sexhrt1 /rl;
contrast "trend in leg length" legqb 79 82 85 90;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=leglength2012;
where sex='M';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 heightqb WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=leglength2012;
where sex='M';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 sit_htqb WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=leglength2012;
where sex='F';
class sex center race educat (ref="1") sexhrt1(ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*CRC12(0) = age1 center  race  educat smoke1 legqb  WHR1 sexhrt1 /rl;
contrast "trend in leg length" legqb 72 75 78 82;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=leglength2012;
where sex='F';
class sex center race educat (ref="1") sexhrt1(ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 heightqb  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=leglength2012;
where sex='F';
class sex center race educat (ref="1") sexhrt1(ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 sit_htqb  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Stratified analysis by race;

proc phreg data=leglength2012;
where race='W';
class sex center educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center educat smoke1 legqb sex  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=leglength2012;
where race='W';
class sex center  educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center sex educat smoke1 heightqb WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=leglength2012;
where race='W';
class sex center educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center sex educat smoke1 sit_htqb WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=leglength2012;
where race='B';
class sex center educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center educat smoke1 legqb sex  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=leglength2012;
where race='B';
class sex center educat (ref="1") sexhrt1(ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center sex educat smoke1 heightqb  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=leglength2012;
where race='B';
class sex center educat (ref="1") sexhrt1(ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrsany12*CRC12(0) = age1 center sex educat smoke1 sit_htqb  WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc means data = leglength2012 mean std;
class race;
var leg height01 sit_ht01;
run;

proc means data = leglength2012 mean std;
class sex;
var leg height01 sit_ht01;
run;

*Interaction with sex;
proc phreg data=leglength2012;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 legqb sex  WHR1 sexhrt1 sex*legqb /rl;
title "CRC cancer-body size multivar adjusted";
run;

*interaction with race;
proc phreg data=leglength2012;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 legqb sex  WHR1 sexhrt1 race*legqb /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Sex & Race Stratified Analysis;

*Hazard Ratios among Whites Men;

proc phreg data = leglength2012;
where sex = 'M' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = leglength2012;
where sex = 'M' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = leglength2012;
where sex = 'M' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Hazard Ratios among Black Men;

proc phreg data = leglength2012;
where sex = 'M' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrsany12*CRC12(0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = leglength2012;
where sex = 'M' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*CRC12(0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = leglength2012;
where sex = 'M' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Hazard Ratios among White Women;

proc phreg data = leglength2012;
where sex = 'F' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = leglength2012;
where sex = 'F' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = leglength2012;
where sex = 'F' and race='W';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

*Hazard Ratios among Black Women;

proc phreg data = leglength2012;
where sex = 'F' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") heightqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 smokyrs1 heightqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = leglength2012;
where sex = 'F' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") legqb (ref='0');
model tpyrsany12*CRC12(0) = age1 center  race  educat smoke1 smokyrs1 legqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data = leglength2012;
where sex = 'F' and race='B';
class sex center race educat (ref="1") smoke1(ref="3") sit_htqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 smokyrs1 sit_htqb sex WHR1 sexhrt1 /rl;
title "CRC cancer-body size multivar adjusted";
run;

proc phreg data=leglength2012;
class sex center race legqb (ref='0');
model tpyrsany12*CRC12 (0) = age1 center  race legqb sex/rl;
contrast "trend in leg length" legqb 74 78 82 86;
title "Qualitiative CRC risk";
run;

proc phreg data = leglength2012;
class sex center race educat (ref="1") sexhrt1(ref="0") smoke1(ref="3");
model tpyrsany12*CRC12 (0) = age1 center  race  educat smoke1 htsdsex torsdsex sex WHR1 sexhrt1/rl;
title "CRC cancer-body size multivar adjusted";
run;


*******************************************************************************************************************************************************************************************************************************************************

