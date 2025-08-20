*Importing the USRDS updated dataset;

*Linking a folder to stata with the dataset using the "cd" command;
*Change to the dataset directory;
cd "E:\Professional Folder\HHRI\HHRI MISSION study\11_Mission Diarrhea_Nutrition_Baseline_Data Generation & Analysis\Datasets"

cd "G:\Professional Folder\HHRI\9_HHRI MISSION study\11_Mission Diarrhea_Nutrition_Baseline_Data Generation & Analysis\MOSIO Data"

cd "D:\Professional Folder\HHRI\9_HHRI MISSION study\11_Mission Diarrhea_Nutrition_Baseline_Data Generation & Analysis\MOSIO Data"

cd "D:\ROOK_03_Professional\HHRI\14_MISSION_MOSIO\Documents\ATC Diarrhea data report APR2023"

cd "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/04_11_MISSION_MOSIO/Documents/ATC Diarrhea data report APR2023"

cd "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/04_14_MISSION_MOSIO/Documents/06_ATC Diarrhea data report APR2023"

cd "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_04_14_MISSION_MOSIO/Documents/11_MOSIO final data pull_10112023"

cd "/Volumes/RAVPOWER2/ROOK_03_Professional/HHRI/Bioinf_1_Projects_04_14_MISSION_MOSIO/B_11_Documents_MOSIO final data pull_10112023"

*Opening A data folder through stata  using the dir command
dir

*importing a csv spreadsheet/dataset into stata using the "insheet" command
*Trying to import as a csv because the import excel command did not import the variable names correctly
insheet using SurveySummary-umndiaStory-2021-03-11_13_10_37_GO_edits2.csv, clear
insheet using umndiaSurvey1-Export_User_Responses-2021-08-16_12-18-52_GOedits_V3.csv, clear
insheet using SurveySummary-umndiaStory-2021-10-22_11_35_56_GoeditsV3.csv, clear
insheet using SurveySummary-umndiaStory-2022-02-01_05_39_39_GOeditsV3.csv, clear
insheet using SurveySummary-umndiaStory-2023-04-27_07_11_57_GOV2.csv, clear
insheet using 06_SurveySummary-umndiaStory-2023-10-11_13_20_01_GOeditsv3.csv, clear

describe

*NOV2023 exclude the PIDs (PID 3520 not in the MOSIO dataset so it will be offset when merging with redcap)
drop if participant == 3501  
drop if participant == 3519  
drop if participant == 3522  
drop if participant == 3523  
drop if participant == 3525  
drop if participant == 3526  
drop if participant == 3529  
drop if participant == 3533  
drop if participant == 3540  
drop if participant == 4003  
drop if participant == 4007  
drop if participant == 4008  
drop if participant == 4009  
drop if participant == 4010  
drop if participant == 4023  
drop if participant == 4024  
drop if participant == 4028  
drop if participant == 4030

*I am trying to find the first time that each participant reported a diarrhea event;
*https://www.stata.com/support/faqs/data-management/first-and-last-occurrences/;
*We did not have day number in the latest report so I will use collection instead); 
*That means that the max value is based on collection point ~52 collections (2 collections per week) rather than days;
*create a firsttime variable based on days so that it can be combined with transplant date as well;

by participant, sort: egen firsttime = min(cond(outcome_diarrhea == 1, collection_id, .))
replace firsttime = 52 if firsttime == .

by participant, sort: egen firsttimedays = min(cond(outcome_diarrhea == 1, daynumber, .))
replace firsttimedays = 198 if firsttimedays == .

by participant, sort: egen everdiarrhea= max(cond(outcome_diarrhea == 1, outcome_diarrhea, .))
replace everdiarrhea = 0 if everdiarrhea == .

tabulate everdiarrhea, missing
tabulate participant everdiarrhea, missing
tabulate participant outcome_diarrhea, missing

by participant, sort: egen firstCTCAE2 = min(cond(diarrheactcaegrade == 2, collection_id, .))
replace firstCTCAE2 = 52 if firstCTCAE2 == .

by participant, sort: egen firstCTCAE2days = min(cond(diarrheactcaegrade == 2, daynumber, .))
replace firstCTCAE2days = 198 if firstCTCAE2days == .

by participant, sort: egen everCTCAE2= max(cond(diarrheactcaegrade == 2, diarrheactcaegrade, .))
replace everCTCAE2 = 0 if everCTCAE2 == .

tabulate everCTCAE2, missing
tabulate participant everCTCAE2, missing
tabulate participant everCTCAE2, missing
tabulate participant collection, missing

*Starting to generate some descriptive figures
*https://www.ssc.wisc.edu/sscc/pubs/sfs/sfs-bargraph.htm;
*https://www.ssc.wisc.edu/sscc/pubs/stata_bar_graphs.htm;

graph bar (count), over(diarrheactcaegrade) over(collection)

*Adding different colors

graph bar (count), over(diarrheactcaegrade) over(collection) asyvars

*Will need to edit the collection_id variable to week (rounddown(collection_id/7)) so that the x-axis is not as visually polluted;
*Will need to edit the collection_id variable to week (rounddown(collection/2)) so that the x-axis is not as visually polluted. Changed from 7 to 2 since we are doing 2 collections per week;

graph bar (count), over(diarrheactcaegrade) over(week) asyvars

*The grade 3 CTCAE events were not complete surveys
*APR 2023 updating the MOSIO data for Ajay's Microbiome and TAC grant
drop if diarrheactcaegrade == 3
graph bar (count), over(diarrheactcaegrade, label(labsize(small))) over(week) asyvars ytitle("Frequency of MMF related Diarrhea") legend(rows(1) stack size(small))
graph bar (count), over(diarrheactcaegrade, label(labsize(small))) over(week) asyvars ytitle("Frequency of MMF related Diarrhea") xtitle("Weeks of follow-up post transplant") legend(rows(1) stack size(small))
graph bar (count), over(diarrheactcaegrade, label(labsize(small))) over(week) asyvars ytitle("Frequency of MMF related Diarrhea") xtitle("Biweekly Collection Timepoint") legend(rows(1) stack size(small))
graph bar (count), over(diarrheactcaegrade, label(labsize(small))) over(week) asyvars ytitle("Frequency of MMF related Diarrhea") legend(rows(1) stack size(small))

tabulate participant, missing
tabulate diarrheactcaegrade, missing
tabulate participant diarrheactcaegrade, missing

*Handy Guides for exporting STATA data to excel

*https://www.stata.com/support/faqs/data-management/copying-tables/
*https://www.stata.com/stata-news/news29-1/export-tables-to-excel/
*https://www.stata.com/stata-news/news29-1/export-tables-to-excel/
*http://www.leahbrooks.org/leahweb/teaching/pppa8022/2018/handouts/ta_material/2018-03-28_Exporting_Stata_Results_to_Excel_03.07.18.pdf 

*Making the survival curves;

*reshape wide week, i(participant)  j(collection)

/*insheet using SurveySummary-umndiaStory-2021-10-22_11_35_56_GoeditsV4.csv, clear

describe

by Participant, sort: egen firsttime = min(cond(outcome_diarrhea == 1, collection, .))
replace firsttime = 52 if firsttime == .

by Participant, sort: egen everdiarrhea= max(cond(outcome_diarrhea == 1, outcome_diarrhea, .))
replace everdiarrhea = 0 if everdiarrhea == .

by Participant, sort: egen firstCTCAE2 = min(cond(diarrheactcaegrade == 2, collection, .))
replace firstCTCAE2 = 52 if firstCTCAE2 == .

by Participant, sort: egen everCTCAE2= max(cond(diarrheactcaegrade == 2, diarrheactcaegrade, .))
replace everCTCAE2 = 0 if everCTCAE2 == . */

*Create a new variable for maxCTCAE to make the demographic table;

generate maxCTCAE = .
replace maxCTCAE = 0 if everdiarrhea == 0
replace maxCTCAE = 1 if everdiarrhea == 1 & everCTCAE2 == 0
replace maxCTCAE = 2 if everCTCAE2 == 2
tabulate maxCTCAE everCTCAE2, missing

*reshape wide firsttime responsedate daynumber week diarrheactcaegrade outcome_diarrhea outcome_diarrhea_sev, i(participant)  j(collection)

*Trying to find observations where there is multiple collection-ids per participant
*https://www.stata.com/support/faqs/data-management/first-and-last-occurrences/;
*sum(state == 1) == 1  & sum(state[_n - 1] == 1) == 0

*by participant, sort: gen byte collectioncheck = cond(collection_id == collection_id[_n-1])

*https://www.stata.com/support/faqs/data-management/duplicate-observations/

sort participant collection_id
quietly by participant collection_id:  gen dup = cond(_N==1,0,_n)
tabulate dup

tabulate participant maxCTCAE, missing

drop if collection_id == .
tabulate participant collection_id

*drop howmanybowelmovementsdoyoutypica haveyouexperiencedahigherfrequen  pleasespecifythedatethediarrheas sincethemorefrequentorloosebowel  maximumnumberofbowelmovementina2  didhavingmorefrequentorloosebowe didyoutakeanyoverthecountermedic whatisthenameofthemedication didyouseeadoctorornurseorcallthe  wereyouhospitalizedbecauseofmore wereanyurgentinterventionssuchas v19
*reshape wide firsttime responsedate daynumber diarrheactcaegrade outcome_diarrhea everdiarrhea everCTCAE2 maxCTCAE, i(participant)  j(collection_id)

*APR2023 here are the variable that can be dropped in preparation of the KM curve;
*variable responses not constant within participant
*variable responses_haveyouexperiencedahig not constant within participant
*variable responses_pleasespecifiythedatet not constant within participant
*variable responses_sincethemorefrequentor not constant within participant
*variable maximumnumberofbowelmovementina2 not constant within participant
*variable responses_didhavingmorefrequento not constant within participant
*variable responses_didyoutakeanyovertheco not constant within participant
*variable responses_whatisthenameofthemedi not constant within participant
*variable responses_didyouseeadoctorornurs not constant within participant
*variable response_wereyouhospitalizedbeca not constant within participant
*variable responses_wereanyurgentintervent not constant within participant
*variable v20 not constant within participant

drop week outcome_diarrhea_sev responses responses_haveyouexperiencedahig responses_pleasespecifiythedatet responses_sincethemorefrequentor maximumnumberofbowelmovementina2 responses_didhavingmorefrequento responses_didyoutakeanyovertheco responses_whatisthenameofthemedi responses_didyouseeadoctorornurs response_wereyouhospitalizedbeca responses_wereanyurgentintervent v20

reshape wide firsttime responsedate daynumber diarrheactcaegrade outcome_diarrhea everdiarrhea everCTCAE2 maxCTCAE, i(participant)  j(collection_id)

*NOV2023
drop week outcome_diarrhea_sev responses responses_haveyouexperiencedahig responses_pleasespecifiythedatet responses_sincethemorefrequentor maximumnumberofbowelmovementina2 responses_didhavingmorefrequento responses_didyoutakeanyovertheco responses_whatisthenameofthemedi responses_didyouseeadoctorornurs response_wereyouhospitalizedbeca responses_wereanyurgentintervent v20
drop week outcome_diarrhea_sev responses_haveyouexperiencedahig responses_pleasespecifiythedatet responses_sincethemorefrequentor maximumnumberofbowelmovementina2 responses_didhavingmorefrequento responses_didyoutakeanyovertheco responses_whatisthenameofthemedi responses_didyouseeadoctorornurs response_wereyouhospitalizedbeca responses_wereanyurgentintervent v20

keep firsttime firsttimedays firstCTCAE2 firstCTCAE2days responsedate daynumber diarrheactcaegrade outcome_diarrhea everdiarrhea everCTCAE2 maxCTCAE participant collection_id
reshape wide firsttime firsttimedays firstCTCAE2 firstCTCAE2days responsedate daynumber diarrheactcaegrade outcome_diarrhea everdiarrhea everCTCAE2 maxCTCAE, i(participant)  j(collection_id)

*Exporting the dataset in wide format to merge with the REdcap data
export excel using "C:\Users\Guillaume Onyeaghala\Desktop\MOSIO Data for Demographics_v2.xlsx", firstrow(variables)
export excel using "C:\Users\Guillaume Onyeaghala\Desktop\MOSIO Data for Demographics_v3.xlsx", firstrow(variables)
export excel using "C:\Users\Guillaume Onyeaghala\Desktop\MOSIO Data for Demographics_02072022.xlsx", firstrow(variables)
export excel using "C:\Users\Guillaume Onyeaghala\Desktop\MOSIO Data for Demographics_04272023.xlsx", firstrow(variables)
export excel using "/Users/guillaumeonyeaghala/Desktop/MOSIO Data for analysis_07112023.xlsx", firstrow(variables)
export excel using "/Users/guillaumeonyeaghala/Desktop/MOSIO Data for analysis_11082023.xlsx", firstrow(variables)

save Mission_MOSIO_wide_10152021, replace
save Mission_MOSIO_wide_11152021, replace
save Mission_MOSIO_wide_02072022, replace
save Mission_MOSIO_wide_04272023, replace
save Mission_MOSIO_wide_07112023, replace
save Mission_MOSIO_wide_11082023, replace

*How many participants went from a stage 2 CTCAE to a stage 1 CTCAE?
*https://www.stata.com/support/faqs/data-management/first-and-last-occurrences/

insheet using SurveySummary-umndiaStory-2021-10-22_11_35_56_GoeditsV4.csv, clear

describe

insheet using 06_SurveySummary-umndiaStory-2023-10-11_13_20_01_GOeditsv3.csv, clear

describe

by id (time), sort: gen byte first = sum(state == 1) == 1  & sum(state[_n - 1] == 1) == 0  

*close but no dice, I have to use the variable rather than the sum, since the sum would report any event where a 2 turned to a 1, not just the consecutive one
by Participant (collection_id), sort: gen byte twotoone = sum(diarrheactcaegrade == 1) == 1  & sum(diarrheactcaegrade[_n - 1] == 2) == 1
replace twotoone = 0 if twotoone == .

by participant (collection_id), sort: gen byte twotoone = diarrheactcaegrade == 1 & diarrheactcaegrade[_n - 1] == 2
replace twotoone = 0 if twotoone == .

tabulate twotoone

by participant, sort: egen twotoonecheck= max(cond(twotoone == 1, twotoone, .))
replace twotoonecheck = 0 if twotoone == .
 
tabulate twotoonecheck 
tabulate twotoonecheck participant

*How many participants went from a stage 1 CTCAE to a stage 2 CTCAE?

by Participant (collection_id), sort: gen byte onetotwo = diarrheactcaegrade == 2 & diarrheactcaegrade[_n - 1] == 1
replace onetotwo = 0 if onetotwo == .

by participant (collection_id), sort: gen byte onetotwo = diarrheactcaegrade == 2 & diarrheactcaegrade[_n - 1] == 1
replace onetotwo = 0 if onetotwo == .

tabulate onetotwo

by participant, sort: egen onetotwocheck= max(cond(onetotwo == 1, onetotwo, .))
replace onetotwocheck = 0 if onetotwo == .

tabulate onetotwocheck
tabulate onetotwocheck participant

*what was the longest duration of consecutive diarrhea events for each participant?

by Participant (collection_id), sort: gen byte diarrhealength = sum(outcome_diarrhea == 1 & outcome_diarrhea[_n - 1] == 1)
replace diarrhealength = 0 if diarrhealength == .

by participant (collection_id), sort: gen byte diarrhealength = sum(outcome_diarrhea == 1 & outcome_diarrhea[_n - 1] == 1)
replace diarrhealength = 0 if diarrhealength == .

tabulate diarrhealength
tabulate Participant diarrhealength, missing
tabulate participant diarrhealength, missing
tabstat diarrhealength

by Participant, sort: egen diarrheamaxlength= max(cond(outcome_diarrhea == 1, diarrhealength, .))
replace diarrheamaxlength = 0 if diarrheamaxlength == .

by participant, sort: egen diarrheamaxlength= max(cond(outcome_diarrhea == 1, diarrhealength, .))
replace diarrheamaxlength = 0 if diarrheamaxlength == .

tabulate diarrheamaxlength
tabulate Participant diarrheamaxlength, missing
tabulate participant diarrheamaxlength, missing

save Mission_MOSIO_wide_11082023_postexclusionLong, replace

*Now you can transpose the max length variable

keep firsttime firsttimedays firstCTCAE2 firstCTCAE2days responsedate daynumber diarrheactcaegrade outcome_diarrhea everdiarrhea everCTCAE2 maxCTCAE twotoone onetotwo twotoonecheck onetotwocheck diarrhealength diarrheamaxlength participant collection_id
reshape wide firsttime firsttimedays firstCTCAE2 firstCTCAE2days responsedate daynumber diarrheactcaegrade outcome_diarrhea everdiarrhea everCTCAE2 maxCTCAE twotoone onetotwo twotoonecheck onetotwocheck diarrhealength diarrheamaxlength, i(participant)  j(collection_id)

export excel using "/Users/guillaumeonyeaghala/Desktop/MOSIO Data for analysis_11082023_V2.xlsx", firstrow(variables)
export excel using "/Users/guillaumeonyeaghala/Desktop/MOSIO Data for analysis_11082023_postexclusion.xlsx", firstrow(variables)
export excel using "/Users/guillaumeonyeaghala/Desktop/MOSIO Data for analysis_11082023_postexclusion_days.xlsx", firstrow(variables)

save Mission_MOSIO_wide_11082023_V2, replace
save Mission_MOSIO_wide_11082023_postexclusion, replace


*set up survival data for mission MOSIO data
stset firstCTCAE2days1, f(everCTCAE21)
list

stset firsttimedays1, f(everdiarrhea1)
list
/*Generate a KM survival estimate
sts list if pheno1v2==0
sts list if pheno1v2==1
sts list if pheno1v2==2
sts list, by(pheno1v2)

*graph the KM curves
sts graph, by(pheno1v2) risktable ci
sts graph, by(pheno1v2) risktable */

*graph the KM curves
*chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://www.stata.com/manuals/ststsgraph.pdf

sts graph, risktable ci
sts graph, by(maxCTCAE2) risktable ci
sts graph, risktable

/*Test equality of KM curves
sts test pheno1v2

*Run Cox Model
stcox i.pheno1v2

*Check proportionality

* Graphical test
sts graph, by(pheno1v2)
stphplot, by(pheno1v2)

* Residual test
stcox i.pheno1v2
estat phtestv2 */

tabstat firsttime1, by(everCTCAE21) stat(n median min max)
tabstat week, by(participant) stat(n)
tabstat participant, by(week) stat(n)

*exporting the data for Levi

use Mission_MOSIO_wide, clear
export delimited using "C:\Users\Guillaume Onyeaghala\Desktop\MOSIO_Data_wide_03152021.csv", replace

*Importing the Redcap data to make a table 1;

*importing a csv spreadsheet/dataset into stata using the "insheet" command

*"MISSION_HHRI_UMN_COMBV2" contains the nutrition data, and based on the information we got from Stephanie and Yuliya, we should exclude observations "3501", "3502", "3504", "4006", "4009" and "4017" from all the analyses, and I believe in the first analysis we only used the V1D1 observations. In addition, Yuliya has not yet entered the supplement data through NDSR to my knowledge, but I figure we could do the majority of the analyses while we figure out how to get that information. She mentioned that she had entered this information in a free text field in RedCap, but I have not been able to pull that field in my reports. 

*"MOSIO_Data_wide_03152021" contains the diarrhea data. The variables "firsttime" and "ever_diarrhea" report when a participant got their first diarrhea events, and whether the participant ever got a diarrhea event, respectively. Likewise, the variables "everCTCAE2" and "firstCTCAE2 '' report whether a participant ever experienced a CTCAE2 grade diarrhea, and when that event occured, respectively.

*Lastly, "MicrobiomeAndImmunos_DATA_2021-04-23_1439_baselineonly_DD" contains the baseline demographic information for the participants, along with the data dictionary for the variables included. 

*Trying to import as a csv because the import excel command did not import the variable names correctly
insheet using MicrobiomeAndImmunos_DATA_2021-04-23_1439_baselineonly_mosio.csv, clear

describe

generate participant = .
replace participant = record_id

sort participant
save Mission_REDCAP, replace

*Merging the mosio dataset

use Mission_MOSIO_wide
sort participant
drop _merge
merge participant using  Mission_REDCAP
save Mission_MOSIO_REDCAP, replace

*Generating descriptive statistics table for the poster
use Mission_MOSIO_REDCAP, clear

drop if participant == 3502
drop if participant == 3504
drop if participant == 4006
drop if participant == 4009
drop if participant == 4003
drop if participant == 4007
drop if participant == 4010

*Recoding the outcome variable since Levi looked at the association in the first month
generate CTCAE21M = . 
replace CTCAE21M = everCTCAE2 if firstCTCAE2 <=30
replace CTCAE21M = 0 if firstCTCAE2 > 30

tabulate CTCAE21M everCTCAE2, missing

*sex
table sex CTCAE21M, missing
tabulate sex CTCAE21M, row missing
tabulate sex CTCAE21M, column missing
*predom_race
tabulate predom_race CTCAE21M, column missing
*donor_type
tabulate donor_type CTCAE21M, column missing
*smoking
tabulate smoking CTCAE21M, column missing
*diabetes_pretx
tabulate diabetes_pretx CTCAE21M, column missing
*bmi_baseline
tabstat bmi_baseline, by(CTCAE21M) stat(n mean sd)

*In this part of the analysis, I am using the dataset where I manually merged the B-glucuronidase data to the mosio demographic data

*Change to the dataset directory;
cd "E:\Professional Folder\HHRI\HHRI MISSION study\11_Mission Diarrhea_Nutrition_Baseline_Data Generation & Analysis\Datasets"

cd "G:\Professional Folder\HHRI\9_HHRI MISSION study\11_Mission Diarrhea_Nutrition_Baseline_Data Generation & Analysis\MOSIO Data"

cd "D:\Professional Folder\HHRI\9_HHRI MISSION study\11_Mission Diarrhea_Nutrition_Baseline_Data Generation & Analysis\MOSIO Data"

*Opening A data folder through stata  using the dir command
dir

*importing a csv spreadsheet/dataset into stata using the "insheet" command
*Trying to import as a csv because the import excel command did not import the variable names correctly
insheet using SurveySummary-umndiaStory-2021-03-11_13_10_37_GO_edits2.csv, clear
insheet using umndiaSurvey1-Export_User_Responses-2021-08-16_12-18-52_GOedits_V3.csv, clear
insheet using SurveySummary-umndiaStory-2021-10-22_11_35_56_GoeditsV3.csv, clear
insheet using Beta_glucoronidase_gene_families_renamed_test_mosio_M1_nomissing.csv, clear
insheet using Beta_glucoronidase_gene_families_renamed_test_mosio_M1_nomissing_v2.csv, clear

describe

*Create a new variable for maxCTCAE to make the demographic table;

generate maxCTCAE = .
replace maxCTCAE = 0 if everdiarrhea == 0
replace maxCTCAE = 1 if everdiarrhea == 1 & everctcae2 == 0
replace maxCTCAE = 2 if everctcae2 == 2
tabulate maxCTCAE everctcae2, missing

*Running a model to test for an association between the BetaGlucuronidase levels at baseline an occurence of Diarrhea at a cross_sectional level
*Since the outcome is categorical, we should use an anova model (https://www.stata.com/manuals13/ranova.pdf) but that means ctcae would be the exposure

encode(gender), generate(sex_glm)
encode(race_report), generate(race_glm)
encode(donor_type_report), generate(donor_glm)

anova Total_per_SID maxCTCAE 

*Running a model to test for an association between the BetaGlucuronidase levels at baseline an occurence of Diarrhea at a cross_sectional level
*trying a binary family model for the CTCAE outcome, so multinomial logistic regression (https://stats.idre.ucla.edu/stata/dae/multinomiallogistic-regression/)

encode(gender), generate(sex_glm)
encode(race_report), generate(race_glm)
encode(donor_type_report), generate(donor_glm)

mlogit maxCTCAE total_per_sid_ec4, base(0)
mlogit maxCTCAE total_per_sid_ec4 sex_glm race_glm donor_glm, base(0)

*Taking a step back and correlating the observations (we are underpowered either ways, see https://www.stata.com/manuals13/rcorrelate.pdf for the code)
correlate maxCTCAE total_per_sid_ec4
pwcorr maxCTCAE total_per_sid_ec4, sig

*Switching Gears to a longitudinal model
insheet using SurveySummary-umndiaStory-2021-10-22_11_35_56_GoeditsV4.csv, clear

describe

by Participant, sort: egen firsttime = min(cond(outcome_diarrhea == 1, collection, .))
replace firsttime = 52 if firsttime == .

by Participant, sort: egen everdiarrhea= max(cond(outcome_diarrhea == 1, outcome_diarrhea, .))
replace everdiarrhea = 0 if everdiarrhea == .

by Participant, sort: egen firstCTCAE2 = min(cond(diarrheactcaegrade == 2, collection, .))
replace firstCTCAE2 = 52 if firstCTCAE2 == .

by Participant, sort: egen everCTCAE2= max(cond(diarrheactcaegrade == 2, diarrheactcaegrade, .))
replace everCTCAE2 = 0 if everCTCAE2 == .

tabulate firsttime everdiarrhea

*Create a new variable for maxCTCAE to make the demographic table (doesn't make sense for the longitudinal dataset as I am not using a loop to group it by PID)

generate maxCTCAE = .
replace maxCTCAE = 0 if everdiarrhea == 0
replace maxCTCAE = 1 if everdiarrhea == 1 & everCTCAE2 == 0
replace maxCTCAE = 2 if everCTCAE2 == 2
tabulate maxCTCAE everCTCAE2, missing
tabulate maxCTCAE Participant, missing

*setting up the merge
sort Participant
save MOSIO_LONG, replace

insheet using Beta_glucoronidase_gene_families_renamed_test_mosio_M1_nomissing_v3.csv, clear

describe
generate Participant = .
replace Participant = participant

*Merging to the demo dataset;
sort Participant
save MOSIO_DEMO, replace


use MOSIO_LONG
sort Participant
merge Participant using MOSIO_DEMO
save MOSIO_LONG_DEMO_BETAGLUC, replace

use MOSIO_LONG_DEMO_BETAGLUC, clear

*From 8342 lecture 9: Covariance Pattern Modeling

/*
Clustering variable (Participant)
timevariable (collection_id, from MOSIO reports)
Longitudinal time outcomes = tacrolimuslevel
Longitudinal time outcomes = percentfromgoal2 (numeric equivalent to the percentfromgoal generated by Kevin)
Longitudinal time outcomes = percentchange

*/

encode(gender), generate(sex_glm)
encode(race_report), generate(race_glm)
encode(donor_type_report), generate(donor_glm)

mixed diarrheactcaegrade total_per_sid_ec4 collection_id || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade total_per_sid_ec4 collection_id || Participant:, noconstant residuals(ar1, t(collection_id))

mixed diarrheactcaegrade total_per_sid_ec4 collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade total_per_sid_ec4 collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(ar1, t(collection_id))

mixed diarrheactcaegrade total_glucosidase collection_id || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade total_glucosidase collection_id || Participant:, noconstant residuals(ar1, t(collection_id))

mixed diarrheactcaegrade total_glucosidase collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade total_glucosidase collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(ar1, t(collection_id))

mixed diarrheactcaegrade total_galactosidase collection_id || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade total_galactosidase collection_id || Participant:, noconstant residuals(ar1, t(collection_id))

mixed diarrheactcaegrade total_galactosidase collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade total_galactosidase collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(ar1, t(collection_id))

mixed diarrheactcaegrade total_glucuronidase collection_id || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade total_glucuronidase collection_id || Participant:, noconstant residuals(ar1, t(collection_id))

mixed diarrheactcaegrade total_glucuronidase collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade total_glucuronidase collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(ar1, t(collection_id))

generate gluc_comb=.
replace gluc_comb = total_glucosidase + total_glucuronidase

mixed diarrheactcaegrade gluc_comb collection_id || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade gluc_comb collection_id || Participant:, noconstant residuals(ar1, t(collection_id))

mixed diarrheactcaegrade total_glucuronidase collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade total_glucuronidase collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(ar1, t(collection_id))

generate isomers=.
replace isomers = total_galactosidase + total_glucuronidase

mixed diarrheactcaegrade isomers collection_id || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade isomers collection_id || Participant:, noconstant residuals(ar1, t(collection_id))

mixed diarrheactcaegrade isomers collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade isomers collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(ar1, t(collection_id))

*I went back to my metacyc regrouped files which had multiple types of 3.2.1.31 groups for beta glucuronidase

mixed diarrheactcaegrade total_metacyc collection_id || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade total_metacyc collection_id || Participant:, noconstant residuals(ar1, t(collection_id))

mixed diarrheactcaegrade total_glucuronidase collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegrade total_glucuronidase collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(ar1, t(collection_id))

*Tacrolimus Level – median and range for level between phenotypes (should just be for eyeballing, actual interpretation of levels would need more context)
*https://www.fsb.miamioh.edu/lij14/311_stata_bar.pdf

graph bar (mean)isomers, over(diarrheactcaegrade, label(labsize(small))) over(Participant) asyvars ytitle("test test") legend(rows(1) stack size(small)) 
graph bar (mean)isomers, over(diarrheactcaegrade, label(labsize(small))) asyvars ytitle("test test") legend(rows(1) stack size(small)) 

generate diarrheactcaegradev2=.
replace diarrheactcaegradev2=0 if diarrheactcaegrade==0
replace diarrheactcaegradev2=1 if diarrheactcaegrade==1
replace diarrheactcaegradev2=2 if diarrheactcaegrade==2
replace diarrheactcaegradev2=2 if diarrheactcaegrade==3

graph bar (mean)isomers, over(diarrheactcaegradev2, label(labsize(small))) over(Participant) asyvars ytitle("test test") legend(rows(1) stack size(small)) 
graph bar (mean)isomers, over(diarrheactcaegradev2, label(labsize(small))) asyvars b1title("CTCAE grade") ytitle("Beta-Glucuronidase Copies Per Million") legend(rows(1) stack size(small)) 

*Checking the data at 3 months;

tabulate collection_id
drop if collection_id >= 26

mixed diarrheactcaegradev2 isomers collection_id || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegradev2 isomers collection_id || Participant:, noconstant residuals(ar1, t(collection_id))

mixed diarrheactcaegradev2 isomers collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegradev2 isomers age_manual collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(independent, t(collection_id))
mixed diarrheactcaegradev2 isomers collection_id sex_glm race_glm donor_glm || Participant:, noconstant residuals(ar1, t(collection_id))

graph bar (mean)isomers, over(diarrheactcaegradev2, label(labsize(small))) over(Participant) asyvars ytitle("test test") legend(rows(1) stack size(small)) 
graph bar (mean)isomers, over(diarrheactcaegradev2, label(labsize(small))) asyvars b1title("CTCAE grade") ytitle("Beta-Glucuronidase Copies Per Million") legend(rows(1) stack size(small)) 

*Starting to generate some descriptive figures
*https://www.ssc.wisc.edu/sscc/pubs/sfs/sfs-bargraph.htm;
*https://www.ssc.wisc.edu/sscc/pubs/stata_bar_graphs.htm;

graph bar (count), over(diarrheactcaegradev2) over(collection_id)

*Adding different colors

graph bar (count), over(diarrheactcaegradev2) over(collection_id) asyvars

*Reduced the graph to the first 3 months so it is not visually polluted, so i do not need to use a week variable
graph bar (count), over(diarrheactcaegradev2) over(week) asyvars

graph bar (count), over(diarrheactcaegradev2, label(labsize(small))) over(collection_id) asyvars ytitle("Frequency of MMF related Diarrhea") legend(rows(1) stack size(small))
graph bar (count), over(diarrheactcaegradev2, label(labsize(small))) over(collection_id) asyvars ytitle("Frequency of MMF related Diarrhea") b1title("Bi-weekly collection timepoint") legend(rows(1) stack size(small))

tabulate diarrheactcaegrade, missing
tabulate participant diarrheactcaegrade, missing

*Handy Guides for exporting STATA data to excel

*https://www.stata.com/support/faqs/data-management/copying-tables/
*https://www.stata.com/stata-news/news29-1/export-tables-to-excel/
*https://www.stata.com/stata-news/news29-1/export-tables-to-excel/
*http://www.leahbrooks.org/leahweb/teaching/pppa8022/2018/handouts/ta_material/2018-03-28_Exporting_Stata_Results_to_Excel_03.07.18.pdf 

*Making the survival curves for the dataset truncated at 3 months;

*reshape wide week, i(participant)  j(collection)

describe

by Participant, sort: egen firsttime3m = min(cond(outcome_diarrhea == 1, collection_id, .))
replace firsttime3m = 26 if firsttime3m == .

by Participant, sort: egen everdiarrhea3m= max(cond(outcome_diarrhea == 1, outcome_diarrhea, .))
replace everdiarrhea3m = 0 if everdiarrhea3m == .

by Participant, sort: egen firstCTCAE2_3m = min(cond(diarrheactcaegradev2 == 2, collection_id, .))
replace firstCTCAE2_3m = 26 if firstCTCAE2_3m == .

by Participant, sort: egen everCTCAE2_3mo= max(cond(diarrheactcaegradev2 == 2, diarrheactcaegradev2, .))
replace everCTCAE2_3mo = 0 if everCTCAE2_3mo == .

*Create a new variable for maxCTCAE to make the demographic table;

generate maxCTCAE_3m = .
replace maxCTCAE_3m = 0 if everdiarrhea3m == 0
replace maxCTCAE_3m = 1 if everdiarrhea3m == 1 & everCTCAE2_3mo == 0
replace maxCTCAE_3m = 2 if everCTCAE2_3mo == 2
tabulate maxCTCAE_3m everCTCAE2_3mo, missing

*Making the dataset into wide format for the survival curves
*drop the variable columns manually since there are so many in this dataset (aka use the keep command)
*only need to reshape the diarrheactcaegradev2 variable as it is the only one that changes within the participant clusters

keep Participant collection_id total_per_sid_ec4 total_glucosidase total_galactosidase total_glucuronidase total_metacyc age_manual sex gender race_report donor_type donor_type_report b1_demographics_complete height_baseline weight_baseline_kg bmi_baseline smoking diabetes_pretx scr_day5_avail scr_day7_avail date_creatinine_day5 creatinine_day5 date_creatinine_day7 creatinine_day7 sex_glm race_glm donor_glm gluc_comb isomers diarrheactcaegradev2 firsttime3m everdiarrhea3m firstCTCAE2_3m maxCTCAE_3m everCTCAE2_3mo

reshape wide diarrheactcaegradev2, i(Participant)  j(collection_id)

tabulate Participant maxCTCAE_3m, missing
drop if isomers==.

save Mission_MOSIO_wide_12012021, replace

*Now you can transpose the max length variable

*trying it for the maximum diarrhea severity experienced
stset firsttime3m, f(maxCTCAE_3m)

*graph the KM curves
sts graph, risktable ci
sts graph, risktable

*Now that I have tabulated the maxi
*set up survival data for mission MOSIO data
stset firstCTCAE2_3m, f(everCTCAE2_3mo)
list

/*Generate a KM survival estimate
sts list if pheno1v2==0
sts list if pheno1v2==1
sts list if pheno1v2==2
sts list, by(pheno1v2)

*graph the KM curves
sts graph, by(pheno1v2) risktable ci
sts graph, by(pheno1v2) risktable */

*graph the KM curves
sts graph, risktable ci
sts graph, risktable

*Run Cox Model
stcox isomers
stcox isomers sex_glm race_glm donor_glm


/*Test equality of KM curves
sts test pheno1v2

*Run Cox Model
stcox i.pheno1v2

*Check proportionality

* Graphical test
sts graph, by(pheno1v2)
stphplot, by(pheno1v2)

* Residual test
stcox i.pheno1v2
estat phtestv2 */

*
*Creating the demographic info abstract

*% Male – already collected in the initial analysis

tabulate sex_glm everCTCAE2_3mo, missing

*% White – already collected in the initial analysis

tabulate race_report everCTCAE2_3mo, missing

*Age at Tx – already collected in the initial

tabstat age_manual, by(everCTCAE2_3mo) stat(n mean sd)

*Weight at Tx – have BMI, good enough for now I think

tabstat bmi_baseline, by(everCTCAE2_3mo) stat(n mean sd)

*diabetes pre transplant 

tabulate diabetes_pretx everCTCAE2_3mo, missing

*average beta_glucuronidase isomers per category

use MOSIO_LONG_DEMO_BETAGLUC, clear

encode(gender), generate(sex_glm)
encode(race_report), generate(race_glm)
encode(donor_type_report), generate(donor_glm)

generate gluc_comb=.
replace gluc_comb = total_glucosidase + total_glucuronidase

generate isomers=.
replace isomers = total_galactosidase + total_glucuronidase

generate diarrheactcaegradev2=.
replace diarrheactcaegradev2=0 if diarrheactcaegrade==0
replace diarrheactcaegradev2=1 if diarrheactcaegrade==1
replace diarrheactcaegradev2=2 if diarrheactcaegrade==2
replace diarrheactcaegradev2=2 if diarrheactcaegrade==3

tabulate collection_id
drop if collection_id >= 26

tabstat isomers, by(everctcae2) stat(n mean sd)

*https://www.stata.com/new-in-stata/panel-data-multinomial-logit/

xtset participant

xtmlogit diarrheactcaegradev2 isomers collection_id, rrr

*Don't have stata 17, soo (https://www.statalist.org/forums/forum/general-stata-discussion/general/1485985-how-to-run-mlogit-for-panel-data)

xtlogit diarrheactcaegradev2 isomers collection_id, rrr

xtlogit diarrheactcaegradev2 isomers collection_id, or

mlogit diarrheactcaegradev2 isomers collection_id, cluster(participant) base(0) rrr

drop if isomers==.

tabulate diarrheactcaegradev2

tabstat firstCTCAE2, by(everCTCAE2) stat(n mean sd)
tabstat firstCTCAE2, by(everCTCAE2) stat(n median min max)
