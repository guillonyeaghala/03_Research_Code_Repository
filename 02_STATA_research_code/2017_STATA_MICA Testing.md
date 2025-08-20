* MICA Testing 

* Linking the Folder 

cd "C:\Users\guillaumeo\Desktop\MICA Study\Preliminary Analysis"

* Opening the data folder through stata  using the dir command

dir

*bringing in the excel spread
import excel MICA_STATA.xlsx, firstrow clear


*examine the data. 'd' is short for describe
describe

*descriptive statistics on MICA
summarize mican, detail
histogram mican, normal
keep if mican >= 2
histogram mican, normal

*recoding the case control status
recode CASE 2=0 1=1
tabulate CASE

*running a logistic regression relating bmi to the risk of death using the linear splines
logistic CASE mican 
predict p_linear
twoway (line p_linear mican,sort col(green)) 

*Making some splines for mica
*Getting the percentiles for mica
sort mican
centile mican, centile(33 66 99)
return list

*making a spline using the mkspline command
mkspline spline0 32.1376 spline1 64.6652 spline2 224.906 spline3  =mican, di marginal

*running a logistic regression relating bmi to the risk of death using the linear splines
logistic CASE mican spline1-spline3
predict p_linspline
twoway (line p_linspline mican,sort col(red)) (line p_linear mican,sort col(green)) 

*running a logistic regression relating mican to the risk of cancer using quadratic splines
foreach var of varlist mican spline0-spline3 {
g `var'_sq=`var'*`var'
}
logistic CASE mican mican_sq spline0_sq spline1_sq spline2_sq spline3_sq 
predict p_qspline
twoway (line p_qspline mican,sort col(blue)) (line p_linspline mican,sort col(red)) (line p_linear mican,sort col(green))

*running a logistic regression relating mica to the risk of cancer using restricted quadratic splines
foreach var of varlist spline0-spline3 {
g rq`var'=`var'*`var'-spline3*spline3
}
logistic CASE mican rqspline1 rqspline2 
predict p_rqspline
twoway (line p_rqspline mican,sort col(black)) (line p_qspline mican,sort col(blue)) (line p_linspline mican,sort col(red)) (line p_linear mican,sort col(green))

* If I want to look at mica as a continuous variable, I may want to remove any outliers 
graph box mican
graph box mican, over (CASE)

* quartiles of  price
egen Q1_mican= pctile(mican), p(25)
egen Q3_mican= pctile(mican), p(75)
egen IC_mican= iqr(mican)

*Identification of extreme value
gen xtreme=1 if (mican< Q1_mican-1.25*IC_mican| mican> Q3_mican+1.25*IC_mican) & missing(mican)==0
recode xtreme . =0
tab xtreme
list if xtreme==1
tab xtreme CASE

*Run the analysis with mica as a continuous variable
describe
tabulate SEX
generate male=.

*now recode sex, and I need " " for character strings
replace male=0 if SEX=="F"
replace male=1 if SEX=="M"
tabulate male

*Adjusted logistic regression model
logistic CASE mican AGE i.male i.stat2
logistic CASE i.micac c.AGE i.male i.stat2 //mica as a categorical variable
logistic CASE i.micac##c.AGE i.male i.stat2 // interaction with age
logistic CASE i.micac##i.male c.AGE i.stat2 // interaction with sex
logistic CASE i.micac##i.stat2 c.AGE i.male // interaction with smoking status

*Adjusted logistic regression model with interactions
*must make age and mica into integers for interactions
generate micamk1= round(mican,1)
generate agemk1 = round(AGE,1)

logistic CASE c.micamk1##c.agemk1 i.male i.stat2
logistic CASE c.micamk1##c.agemk1##i.male##i.stat2

* mica*sex interaction
logistic CASE i.micac##i.male##c.agemk1##i.stat2
margins micac, over (male) 
marginsplot, xdimension(micac) plotdimension(male) 

* mica*smoking interaction
logistic CASE c.micamk1##c.agemk1##i.male##i.stat2
margins micamk1, over (stat2)
marginsplot, xdimension(stat2) plotdimension(micamk1) 

* mica*age interaction
logistic CASE c.micamk1##c.agemk1##i.male##i.stat2
margins micamk1, over (agemk1)
marginsplot, xdimension(agemk1) plotdimension(micamk1) 
