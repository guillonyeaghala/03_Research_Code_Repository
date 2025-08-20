options ps = 3000 ls = 120 nofmterr;
libname mylib 'C:\Users\onyea005\Desktop\OLINK\Data Analysis';

/*Creating the main dataset from the sas database

data one;
set mylib.analys; */

/*Importing the datasets*/

PROC IMPORT OUT= mylib.umgc_npx DATAFILE= "C:\Users\onyea005\Desktop\OLINK\Data Analysis\UMGC OLINK Data.xlsx" DBMS=xlsx REPLACE;
     SHEET="UMGC NPX"; 
     GETNAMES=YES;
RUN;

proc print data=mylib.umgc_npx (obs=50);
run;

PROC IMPORT OUT= mylib.olink1 DATAFILE= "C:\Users\onyea005\Desktop\OLINK\Data Analysis\OLINK NPX Raw Data.xlsx" DBMS=xlsx REPLACE;
     SHEET="OLINK NPX Run 1"; 
     GETNAMES=YES;
RUN;

proc print data=mylib.olink1 (obs=50);
run;

PROC IMPORT OUT= mylib.olink2 DATAFILE= "C:\Users\onyea005\Desktop\OLINK\Data Analysis\OLINK NPX Raw Data.xlsx" DBMS=xlsx REPLACE;
     SHEET="OLINK NPX Run 2"; 
     GETNAMES=YES;
RUN;

proc print data=mylib.olink2 (obs=50);
run;

/*Merging the datasets: Combine the 2 olink runs first, then add the UMGC to the dataset*/

proc contents data = mylib.olink1;
run;

proc contents data = mylib.olink2;
run;

/*Converting the Sample ID to a Numeric variable in the olink1 dataset*/

data mylib.olink1;
set mylib.olink1;
Sample = input(sample_ID,5.); 
drop sample_ID; 
rename Sample=sample_ID;
run;

data olinkmk1;
set mylib.olink1 mylib.olink2;
run;

proc print data = olinkmk1;
run;

/*Checking for duplicate sample ids*/

proc sort data=olinkmk1 out=olinkmk2;
  by sample_ID;
run;

data dupes;
  set olinkmk2;
  by sample_ID;
  if not (first.id and last.id) then output;
run;

proc print data=dupes;
run;

PROC FREQ data=dupes;
 TABLES sample_ID / noprint out=duplicates;
RUN;

PROC PRINT data=duplicates;
 WHERE count ge 2;
RUN; 

/*Removing the duplicates*/

PROC SORT data=olinkmk2 NODUPKEY;
BY sample_ID;
RUN; 

proc print data = olinkmk2;
run;

/*Making a categorical variable that will be used tp differenciate the OLINK and UMGC runs*/

data olinkmk2;
set olinkmk2;
if sample_ID ne . then batch="OLINK";
run;

proc print data = olinkmk2;
run;

data umgc;
set mylib.umgc_npx;
Status = .;
_Flagged = "Unknown";
sample_ID = Protein_name____Sample_name;
if sample_ID ne . then batch="UMGC";
run;

proc print data = umgc;
run;

proc contents data = umgc;
run;

data umgc;
set umgc;
Case_Status = input(Status,5.); 
drop Status; 
rename Case_Status=Status;
run;

data umgc;
set umgc;
drop Protein_name____Sample_name _104_Inc_Ctrl_1 _119_Ext_Ctrl _146_Inc_Ctrl_2 _147_Det_Ctrl;
run;
 
data umgc;
set umgc;
Sample = input(sample_ID,5.); 
drop sample_ID; 
rename Sample=sample_ID;
run;

proc print data = umgc;
run;

proc contents data = umgc;
run;

/*Making the final dataset to be used for comparisons*/

data mylib.olinkfinal;
set olinkmk2 umgc;
run;

proc print data = mylib.olinkfinal;
run;


/*Starting the data analysis*/

data olinkmkI;
set mylib.olinkfinal;
run;

data MDL;
set olinkmkI;
where batch= "OLINK";
rename _101_IL_8 = _101_IL_8_U; 
rename _102_VEGF_A = _102_VEGF_A_U;
rename _103_BDNF = _103_BDNF_U;
rename _105_MCP_3 = _105_MCP_3_U;
rename _106_hGDNF = _106_hGDNF_U;
rename _107_CDCP1 = _107_CDCP1_U; 
rename _108_CD244 = _108_CD244_U; 
rename _109_IL_7 = _109_IL_7_U;
rename _110_OPG = _110_OPG_U; 
rename _111_LAP_TGF_beta_1 = _111_LAP_TGF_beta_1_U; 
rename _112_uPA = _112_uPA_U; 
rename _113_IL_6 = _113_IL_6_U; 
rename _114_IL_17C = _114_IL_17C_U; 
rename _115_MCP_1 = _115_MCP_1_U; 
rename _116_IL_17A = _116_IL_17A_U; 
rename _117_CXCL11 = _117_CXCL11_U; 
rename _118_AXIN1 = _118_AXIN1_U; 
rename _120_TRAIL = _120_TRAIL_U; 
rename _121_IL_20RA = _121_IL_20RA_U; 
rename _122_CXCL9 = _122_CXCL9_U; 
rename _123_CST5 = _123_CST5_U;
rename _124_IL_2RB = _124_IL_2RB_U; 
rename _125_IL_1_alpha = _125_IL_1_alpha_U; 
rename _126_OSM = _126_OSM_U; 
rename _127_IL_2 = _127_IL_2_U; 
rename _128_CXCL1 = _128_CXCL1_U; 
rename _129_TSLP = _129_TSLP_U; 
rename _130_CCL4 = _130_CCL4_U; 
rename _131_CD6 = _131_CD6_U; 
rename _132_SCF = _132_SCF_U; 
rename _133_IL_18 = _133_IL_18_U; 
rename _134_SLAMF1 = _134_SLAMF1_U;
rename _135_TGFA = _135_TGFA_U; 
rename _136_MCP_4 = _136_MCP_4_U; 
rename _137_CCL11 = _137_CCL11_U; 
rename _138_TNFSF14 = _138_TNFSF14_U; 
rename _139_FGF_23 = _139_FGF_23_U; 
rename _140_IL_10RA = _140_IL_10RA_U; 
rename _141_FGF_5 = _141_FGF_5_U; 
rename _142_MMP_1 = _142_MMP_1_U; 
rename _143_LIF_R = _143_LIF_R_U;
rename _144_FGF_21 = _144_FGF_21_U; 
rename _145_CCL19 = _145_CCL19_U; 
rename _148_IL_15RA = _148_IL_15RA_U; 
rename _149_IL_10RB = _149_IL_10RB_U;
rename _150_IL_22_RA1 = _150_IL_22_RA1_U; 
rename _151_IL_18R1 = _151_IL_18R1_U; 
rename _152_PD_L1 = _152_PD_L1_U; 
rename _153_Beta_NGF = _153_Beta_NGF_U; 
rename _154_CXCL5 = _154_CXCL5_U; 
rename _155_TRANCE = _155_TRANCE_U; 
rename _156_HGF = _156_HGF_U; 
rename _157_IL_12B = _157_IL_12B_U; 
rename _158_IL_24 = _158_IL_24_U; 
rename _159_IL_13 = _159_IL_13_U; 
rename _160_ARTN = _160_ARTN_U; 
rename _161_MMP_10 = _161_MMP_10_U; 
rename _162_IL_10 = _162_IL_10_U; 
rename _163_TNF = _163_TNF_U; 
rename _164_CCL23 = _164_CCL23_U; 
rename _165_CD5 = _165_CD5_U; 
rename _166_MIP_1_alpha = _166_MIP_1_alpha_U; 
rename _167_Flt3L = _167_Flt3L_U; 
rename _168_CXCL6 = _168_CXCL6_U; 
rename _169_CXCL10 = _169_CXCL10_U; 
rename _170_4E_BP1 = _170_4E_BP1_U; 
rename _171_IL_20 = _171_IL_20_U; 
rename _172_SIRT2 = _172_SIRT2_U; 
rename _173_CCL28 = _173_CCL28_U; 
rename _174_DNER = _174_DNER_U; 
rename _175_EN_RAGE = _175_EN_RAGE_U; 
rename _176_CD40 = _176_CD40_U; 
rename _177_IL_33 = _177_IL_33_U; 
rename _178_IFN_gamma = _178_IFN_gamma_U; 
rename _179_FGF_19 = _179_FGF_19_U; 
rename _180_IL_4  = _180_IL_4_U; 
rename _181_LIF = _181_LIF_U;
rename _182_NRTN = _182_NRTN_U; 
rename _183_MCP_2 = _183_MCP_2_U; 
rename _184_CASP_8 = _184_CASP_8_U; 
rename _185_CCL25 = _185_CCL25_U; 
rename _186_CX3CL1 = _186_CX3CL1_U; 
rename _187_TNFRSF9 = _187_TNFRSF9_U; 
rename _188_NT_3 = _188_NT_3_U; 
rename _189_TWEAK = _189_TWEAK_U; 
rename _190_CCL20 = _190_CCL20_U; 
rename _191_ST1A1 = _191_ST1A1_U; 
rename _192_STAMPB = _192_STAMPB_U; 
rename _193_IL_5 = _193_IL_5_U; 
rename _194_ADA = _194_ADA_U; 
rename _195_TNFB = _195_TNFB_U; 
rename _196_CSF_1 = _196_CSF_1_U; 
run;

data UMGC;
set olinkmkI;
where batch = "UMGC";
rename _101_IL_8 = _101_IL_8_M; 
rename _102_VEGF_A = _102_VEGF_A_M;
rename _103_BDNF = _103_BDNF_M;
rename _105_MCP_3 = _105_MCP_3_M;
rename _106_hGDNF = _106_hGDNF_M;
rename _107_CDCP1 = _107_CDCP1_M; 
rename _108_CD244 = _108_CD244_M; 
rename _109_IL_7 = _109_IL_7_M;
rename _110_OPG = _110_OPG_M; 
rename _111_LAP_TGF_beta_1 = _111_LAP_TGF_beta_1_M; 
rename _112_uPA = _112_uPA_M; 
rename _113_IL_6 = _113_IL_6_M; 
rename _114_IL_17C = _114_IL_17C_M; 
rename _115_MCP_1 = _115_MCP_1_M; 
rename _116_IL_17A = _116_IL_17A_M; 
rename _117_CXCL11 = _117_CXCL11_M; 
rename _118_AXIN1 = _118_AXIN1_M; 
rename _120_TRAIL = _120_TRAIL_M; 
rename _121_IL_20RA = _121_IL_20RA_M; 
rename _122_CXCL9 = _122_CXCL9_M; 
rename _123_CST5 = _123_CST5_M;
rename _124_IL_2RB = _124_IL_2RB_M; 
rename _125_IL_1_alpha = _125_IL_1_alpha_M; 
rename _126_OSM = _126_OSM_M; 
rename _127_IL_2 = _127_IL_2_M; 
rename _128_CXCL1 = _128_CXCL1_M; 
rename _129_TSLP = _129_TSLP_M; 
rename _130_CCL4 = _130_CCL4_M; 
rename _131_CD6 = _131_CD6_M; 
rename _132_SCF = _132_SCF_M; 
rename _133_IL_18 = _133_IL_18_M; 
rename _134_SLAMF1 = _134_SLAMF1_M;
rename _135_TGFA = _135_TGFA_M; 
rename _136_MCP_4 = _136_MCP_4_M; 
rename _137_CCL11 = _137_CCL11_M; 
rename _138_TNFSF14 = _138_TNFSF14_M; 
rename _139_FGF_23 = _139_FGF_23_M; 
rename _140_IL_10RA = _140_IL_10RA_M; 
rename _141_FGF_5 = _141_FGF_5_M; 
rename _142_MMP_1 = _142_MMP_1_M; 
rename _143_LIF_R = _143_LIF_R_M;
rename _144_FGF_21 = _144_FGF_21_M; 
rename _145_CCL19 = _145_CCL19_M; 
rename _148_IL_15RA = _148_IL_15RA_M; 
rename _149_IL_10RB = _149_IL_10RB_M;
rename _150_IL_22_RA1 = _150_IL_22_RA1_M; 
rename _151_IL_18R1 = _151_IL_18R1_M; 
rename _152_PD_L1 = _152_PD_L1_M; 
rename _153_Beta_NGF = _153_Beta_NGF_M; 
rename _154_CXCL5 = _154_CXCL5_M; 
rename _155_TRANCE = _155_TRANCE_M; 
rename _156_HGF = _156_HGF_M; 
rename _157_IL_12B = _157_IL_12B_M; 
rename _158_IL_24 = _158_IL_24_M; 
rename _159_IL_13 = _159_IL_13_M; 
rename _160_ARTN = _160_ARTN_M; 
rename _161_MMP_10 = _161_MMP_10_M; 
rename _162_IL_10 = _162_IL_10_M; 
rename _163_TNF = _163_TNF_M; 
rename _164_CCL23 = _164_CCL23_M; 
rename _165_CD5 = _165_CD5_M; 
rename _166_MIP_1_alpha = _166_MIP_1_alpha_M; 
rename _167_Flt3L = _167_Flt3L_M; 
rename _168_CXCL6 = _168_CXCL6_M; 
rename _169_CXCL10 = _169_CXCL10_M; 
rename _170_4E_BP1 = _170_4E_BP1_M; 
rename _171_IL_20 = _171_IL_20_M; 
rename _172_SIRT2 = _172_SIRT2_M; 
rename _173_CCL28 = _173_CCL28_M; 
rename _174_DNER = _174_DNER_M; 
rename _175_EN_RAGE = _175_EN_RAGE_M; 
rename _176_CD40 = _176_CD40_M; 
rename _177_IL_33 = _177_IL_33_M; 
rename _178_IFN_gamma = _178_IFN_gamma_M; 
rename _179_FGF_19 = _179_FGF_19_M; 
rename _180_IL_4  = _180_IL_4_M; 
rename _181_LIF = _181_LIF_M;
rename _182_NRTN = _182_NRTN_M; 
rename _183_MCP_2 = _183_MCP_2_M; 
rename _184_CASP_8 = _184_CASP_8_M; 
rename _185_CCL25 = _185_CCL25_M; 
rename _186_CX3CL1 = _186_CX3CL1_M; 
rename _187_TNFRSF9 = _187_TNFRSF9_M; 
rename _188_NT_3 = _188_NT_3_M; 
rename _189_TWEAK = _189_TWEAK_M; 
rename _190_CCL20 = _190_CCL20_M; 
rename _191_ST1A1 = _191_ST1A1_M; 
rename _192_STAMPB = _192_STAMPB_M; 
rename _193_IL_5 = _193_IL_5_M; 
rename _194_ADA = _194_ADA_M; 
rename _195_TNFB = _195_TNFB_M; 
rename _196_CSF_1 = _196_CSF_1_M; 
run;

Proc sort data = MDL;
by sample_ID;
run;

data MDL;
set MDL;
check1 = 1;
run;

Proc sort data = UMGC;
by sample_ID;
run;

data UMGC;
set UMGC;
check2 = 1;
run;

data olinkmkII;
merge MDL UMGC;
by sample_ID;
run;

proc print data = olinkmkII;
run;

data olinkmkIII;
set olinkmkII;
where check1 = 1 and check2=1;
run;

proc print data = olinkmkIII;
run;

proc corr data = olinkmkIII;
var _101_IL_8_U _102_VEGF_A_U _103_BDNF_U _105_MCP_3_U _106_hGDNF_U _107_CDCP1_U _108_CD244_U _109_IL_7_U _110_OPG_U _111_LAP_TGF_beta_1_U _112_uPA_U _113_IL_6_U _114_IL_17C_U _115_MCP_1_U _116_IL_17A_U _117_CXCL11_U _118_AXIN1_U _120_TRAIL_U _121_IL_20RA_U _122_CXCL9_U _123_CST5_U _124_IL_2RB_U _125_IL_1_alpha_U _126_OSM_U _127_IL_2_U _128_CXCL1_U _129_TSLP_U _130_CCL4_U _131_CD6_U _132_SCF_U _133_IL_18_U _134_SLAMF1_U _135_TGFA_U _136_MCP_4_U _137_CCL11_U _138_TNFSF14_U _139_FGF_23_U _140_IL_10RA_U _141_FGF_5_U _142_MMP_1_U _143_LIF_R_U _144_FGF_21_U _145_CCL19_U _148_IL_15RA_U _149_IL_10RB_U _150_IL_22_RA1_U _151_IL_18R1_U _152_PD_L1_U _153_Beta_NGF_U _154_CXCL5_U _155_TRANCE_U _156_HGF_U _157_IL_12B_U _158_IL_24_U _159_IL_13_U _160_ARTN_U _161_MMP_10_U _162_IL_10_U _163_TNF_U _164_CCL23_U _165_CD5_U _166_MIP_1_alpha_U _167_Flt3L_U _168_CXCL6_U _169_CXCL10_U _170_4E_BP1_U _171_IL_20_U _172_SIRT2_U _173_CCL28_U _174_DNER_U _175_EN_RAGE_U _176_CD40_U _177_IL_33_U _178_IFN_gamma_U _179_FGF_19_U _180_IL_4_U _181_LIF_U _182_NRTN_U _183_MCP_2_U _184_CASP_8_U _185_CCL25_U _186_CX3CL1_U _187_TNFRSF9_U _188_NT_3_U _189_TWEAK_U _190_CCL20_U _191_ST1A1_U _192_STAMPB_U _193_IL_5_U _194_ADA_U _195_TNFB_U _196_CSF_1_U _101_IL_8_M _102_VEGF_A_M _103_BDNF_M _105_MCP_3_M _106_hGDNF_M _107_CDCP1_M _108_CD244_M _109_IL_7_M _110_OPG_M _111_LAP_TGF_beta_1_M _112_uPA_M _113_IL_6_M _114_IL_17C_M _115_MCP_1_M _116_IL_17A_M _117_CXCL11_M _118_AXIN1_M _120_TRAIL_M _121_IL_20RA_M _122_CXCL9_M _123_CST5_M _124_IL_2RB_M _125_IL_1_alpha_M _126_OSM_M _127_IL_2_M _128_CXCL1_M _129_TSLP_M _130_CCL4_M _131_CD6_M _132_SCF_M _133_IL_18_M _134_SLAMF1_M _135_TGFA_M _136_MCP_4_M _137_CCL11_M _138_TNFSF14_M _139_FGF_23_M _140_IL_10RA_M _141_FGF_5_M _142_MMP_1_M _143_LIF_R_M _144_FGF_21_M _145_CCL19_M _148_IL_15RA_M _149_IL_10RB_M _150_IL_22_RA1_M _151_IL_18R1_M _152_PD_L1_M _153_Beta_NGF_M _154_CXCL5_M _155_TRANCE_M _156_HGF_M _157_IL_12B_M _158_IL_24_M _159_IL_13_M _160_ARTN_M _161_MMP_10_M _162_IL_10_M _163_TNF_M _164_CCL23_M _165_CD5_M _166_MIP_1_alpha_M _167_Flt3L_M _168_CXCL6_M _169_CXCL10_M _170_4E_BP1_M _171_IL_20_M _172_SIRT2_M _173_CCL28_M _174_DNER_M _175_EN_RAGE_M _176_CD40_M _177_IL_33_M _178_IFN_gamma_M _179_FGF_19_M _180_IL_4_M _181_LIF_M _182_NRTN_M _183_MCP_2_M _184_CASP_8_M _185_CCL25_M _186_CX3CL1_M _187_TNFRSF9_M _188_NT_3_M _189_TWEAK_M _190_CCL20_M _191_ST1A1_M _192_STAMPB_M _193_IL_5_M _194_ADA_M _195_TNFB_M _196_CSF_1_M;
run; 

proc means data = olinkmkIII range;
by batch;
run;

/*Making a new variable that is the difference between lab variables*/
data olinkmkIII;
set olinkmkIII;
_101_IL_8_D = _101_IL_8_U - _101_IL_8_M; 
_102_VEGF_A_D = _102_VEGF_A_U - _102_VEGF_A_M;
_103_BDNF_D = _103_BDNF_U - _103_BDNF_M;
_105_MCP_3_D = _105_MCP_3_U - _105_MCP_3_M;
_106_hGDNF_D = _106_hGDNF_U - _106_hGDNF_M;
_107_CDCP1_D = _107_CDCP1_U - _107_CDCP1_M; 
_108_CD244_D = _108_CD244_U - _108_CD244_M; 
_109_IL_7_D = _109_IL_7_U - _109_IL_7_M;
_110_OPG_D = _110_OPG_U - _110_OPG_M; 
_111_LAP_TGF_beta_1_D =_111_LAP_TGF_beta_1_U - _111_LAP_TGF_beta_1_M; 
_112_uPA_D = _112_uPA_U - _112_uPA_M; 
_113_IL_6_D = _113_IL_6_U - _113_IL_6_M; 
_114_IL_17C_D = _114_IL_17C_U - _114_IL_17C_M; 
_115_MCP_1_D = _115_MCP_1_U - _115_MCP_1_M; 
_116_IL_17A_D = _116_IL_17A_U - _116_IL_17A_M; 
_117_CXCL11_D = _117_CXCL11_U - _117_CXCL11_M; 
_118_AXIN1_D = _118_AXIN1_U - _118_AXIN1_M; 
_120_TRAIL_D = _120_TRAIL_U - _120_TRAIL_M; 
_121_IL_20RA_D = _121_IL_20RA_U - _121_IL_20RA_M; 
_122_CXCL9_D = _122_CXCL9_U - _122_CXCL9_M; 
_123_CST5_D = _123_CST5_U - _123_CST5_M;
_124_IL_2RB_D = _124_IL_2RB_U - _124_IL_2RB_M; 
_125_IL_1_alpha_D = _125_IL_1_alpha_U - _125_IL_1_alpha_M; 
_126_OSM_D = _126_OSM_U - _126_OSM_M; 
_127_IL_2_D = _127_IL_2_U - _127_IL_2_M; 
_128_CXCL1_D = _128_CXCL1_U - _128_CXCL1_M; 
_129_TSLP_D = _129_TSLP_U - _129_TSLP_M; 
_130_CCL4_D = _130_CCL4_U - _130_CCL4_M; 
_131_CD6_D = _131_CD6_U - _131_CD6_M; 
_132_SCF_D = _132_SCF_U - _132_SCF_M; 
_133_IL_18_D = _133_IL_18_U - _133_IL_18_M; 
_134_SLAMF1_D = _134_SLAMF1_U - _134_SLAMF1_M;
_135_TGFA_D = _135_TGFA_U - _135_TGFA_M; 
_136_MCP_4_D = _136_MCP_4_U - _136_MCP_4_M; 
_137_CCL11_D = _137_CCL11_U - _137_CCL11_M; 
_138_TNFSF14_D = _138_TNFSF14_U - _138_TNFSF14_M; 
_139_FGF_23_D = _139_FGF_23_U - _139_FGF_23_M; 
_140_IL_10RA_D = _140_IL_10RA_U - _140_IL_10RA_M; 
_141_FGF_5_D = _141_FGF_5_U - _141_FGF_5_M; 
_142_MMP_1_D = _142_MMP_1_U - _142_MMP_1_M; 
_143_LIF_R_D = _143_LIF_R_U - _143_LIF_R_M;
_144_FGF_21_D = _144_FGF_21_U - _144_FGF_21_M; 
_145_CCL19_D = _145_CCL19_U - _145_CCL19_M; 
_148_IL_15RA_D = _148_IL_15RA_U - _148_IL_15RA_M; 
_149_IL_10RB_D = _149_IL_10RB_U - _149_IL_10RB_M;
_150_IL_22_RA1_D = _150_IL_22_RA1_U - _150_IL_22_RA1_M; 
_151_IL_18R1_D = _151_IL_18R1_U - _151_IL_18R1_M; 
_152_PD_L1_D = _152_PD_L1_U - _152_PD_L1_M; 
_153_Beta_NGF_D = _153_Beta_NGF_U - _153_Beta_NGF_M; 
_154_CXCL5_D = _154_CXCL5_U - _154_CXCL5_M; 
_155_TRANCE_D = _155_TRANCE_U - _155_TRANCE_M; 
_156_HGF_D = _156_HGF_U - _156_HGF_M; 
_157_IL_12B_D = _157_IL_12B_U - _157_IL_12B_M; 
_158_IL_24_D = _158_IL_24_U - _158_IL_24_M; 
_159_IL_13_D = _159_IL_13_U - _159_IL_13_M; 
_160_ARTN_D = _160_ARTN_U - _160_ARTN_M; 
_161_MMP_10_D = _161_MMP_10_U - _161_MMP_10_M; 
_162_IL_10_D = _162_IL_10_U - _162_IL_10_M; 
_163_TNF_D = _163_TNF_U - _163_TNF_M; 
_164_CCL23_D = _164_CCL23_U - _164_CCL23_M; 
_165_CD5_D = _165_CD5_U - _165_CD5_M; 
_166_MIP_1_alpha_D = _166_MIP_1_alpha_U - _166_MIP_1_alpha_M; 
_167_Flt3L_D = _167_Flt3L_U - _167_Flt3L_M; 
_168_CXCL6_D = _168_CXCL6_U - _168_CXCL6_M; 
_169_CXCL10_D = _169_CXCL10_U - _169_CXCL10_M; 
_170_4E_BP1_D = _170_4E_BP1_U - _170_4E_BP1_M; 
_171_IL_20_D = _171_IL_20_U - _171_IL_20_M; 
_172_SIRT2_D = _172_SIRT2_U - _172_SIRT2_M; 
_173_CCL28_D = _173_CCL28_U - _173_CCL28_M; 
_174_DNER_D = _174_DNER_U - _174_DNER_M; 
_175_EN_RAGE_D = _175_EN_RAGE_U - _175_EN_RAGE_M; 
_176_CD40_D = _176_CD40_U - _176_CD40_M; 
_177_IL_33_D = _177_IL_33_U - _177_IL_33_M; 
_178_IFN_gamma_D = _178_IFN_gamma_U - _178_IFN_gamma_M; 
_179_FGF_19_D = _179_FGF_19_U - _179_FGF_19_M; 
_180_IL_4_D  = _180_IL_4_U - _180_IL_4_M; 
_181_LIF_D = _181_LIF_U - _181_LIF_M;
_182_NRTN_D = _182_NRTN_U - _182_NRTN_M; 
_183_MCP_2_D = _183_MCP_2_U - _183_MCP_2_M; 
_184_CASP_8_D = _184_CASP_8_U - _184_CASP_8_M; 
_185_CCL25_D = _185_CCL25_U - _185_CCL25_M; 
_186_CX3CL1_D = _186_CX3CL1_U - _186_CX3CL1_M; 
_187_TNFRSF9_D = _187_TNFRSF9_U - _187_TNFRSF9_M; 
_188_NT_3_D = _188_NT_3_U - _188_NT_3_M; 
_189_TWEAK_D = _189_TWEAK_U - _189_TWEAK_M; 
_190_CCL20_D = _190_CCL20_U - _190_CCL20_M; 
_191_ST1A1_D = _191_ST1A1_U - _191_ST1A1_M; 
_192_STAMPB_D = _192_STAMPB_U - _192_STAMPB_M; 
_193_IL_5_D = _193_IL_5_U - _193_IL_5_M; 
_194_ADA_D = _194_ADA_U - _194_ADA_M; 
_195_TNFB_D = _195_TNFB_U - _195_TNFB_M; 
_196_CSF_1_D = _196_CSF_1_U - _196_CSF_1_M; 
run;

proc print data = olinkmkIII;
run;

proc means data=olinkmkIII mean std;
var _101_IL_8_D _102_VEGF_A_D _103_BDNF_D _105_MCP_3_D _106_hGDNF_D _107_CDCP1_D _108_CD244_D _109_IL_7_D _110_OPG_D _111_LAP_TGF_beta_1_D _112_uPA_D _113_IL_6_D _114_IL_17C_D _115_MCP_1_D _116_IL_17A_D _117_CXCL11_D _118_AXIN1_D _120_TRAIL_D _121_IL_20RA_D _122_CXCL9_D _123_CST5_D _124_IL_2RB_D _125_IL_1_alpha_D _126_OSM_D _127_IL_2_D _128_CXCL1_D _129_TSLP_D _130_CCL4_D _131_CD6_D _132_SCF_D _133_IL_18_D _134_SLAMF1_D _135_TGFA_D _136_MCP_4_D _137_CCL11_D _138_TNFSF14_D _139_FGF_23_D _140_IL_10RA_D _141_FGF_5_D _142_MMP_1_D _143_LIF_R_D _144_FGF_21_D _145_CCL19_D _148_IL_15RA_D _149_IL_10RB_D _150_IL_22_RA1_D _151_IL_18R1_D _152_PD_L1_D _153_Beta_NGF_D _154_CXCL5_D _155_TRANCE_D _156_HGF_D _157_IL_12B_D _158_IL_24_D _159_IL_13_D _160_ARTN_D _161_MMP_10_D _162_IL_10_D _163_TNF_D _164_CCL23_D _165_CD5_D _166_MIP_1_alpha_D _167_Flt3L_D _168_CXCL6_D _169_CXCL10_D _170_4E_BP1_D _171_IL_20_D _172_SIRT2_D _173_CCL28_D _174_DNER_D _175_EN_RAGE_D _176_CD40_D _177_IL_33_D _178_IFN_gamma_D _179_FGF_19_D _180_IL_4_D _181_LIF_D _182_NRTN_D _183_MCP_2_D _184_CASP_8_D _185_CCL25_D _186_CX3CL1_D _187_TNFRSF9_D _188_NT_3_D _189_TWEAK_D _190_CCL20_D _191_ST1A1_D _192_STAMPB_D _193_IL_5_D _194_ADA_D _195_TNFB_D _196_CSF_1_D;
output out = score;
run;

proc print data = score;
run; 

PROC IMPORT OUT= mylib.scoremkI DATAFILE= "C:\Users\onyea005\Desktop\OLINK\Data Analysis\Score Table.xlsx" DBMS=xlsx REPLACE;
     SHEET="Sheet3"; 
     GETNAMES=YES;
RUN;

proc print data = mylib.scoremkI;
run;

PROC IMPORT OUT= mylib.scoremkII DATAFILE= "C:\Users\onyea005\Desktop\OLINK\Data Analysis\Score Table V2.xlsx" DBMS=xlsx REPLACE;
SHEET="Sheet2"
GETNAMES=YES;
RUN;

proc print data = mylib.scoremkII;
run;

data scoremkII;
set mylib.scoremkI;
score = (STD/MEAN)*100;
run;

proc print data = scoremkII;
run;

data olinkmkIV;
set olinkmkI;
where sample_ID in (1879, 1887, 2325, 2427, 2463, 2684, 2896, 3033, 3379, 3796, 3958, 4141, 4651, 5197, 5227, 5743, 6141, 6807, 11482, 14136, 15266, 16191, 17047, 18298, 18429, 19218, 19645, 20011, 22827, 24120); 
run;

proc print data = olinkmkIV;
run;

/*OlinkmkIV is the ideal dataset because it has all the 96 tests classified by batch for comparison*/

data mylib.olinkmkIV;
set olinkmkIV;
run;

/*Getting values for the ICC*/
data olinkmkIV;
set mylib.olinkmkIV;
run;

proc print data = olinkmkIV;
run;

proc anova data=olinkmkIV;
class batch;
model _101_IL_8 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _102_VEGF_A = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _103_BDNF = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _105_MCP_3 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _106_hGDNF = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _107_CDCP1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _108_CD244 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _109_IL_7 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _110_OPG = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _111_LAP_TGF_beta_1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _112_uPA = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _113_IL_6 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _114_IL_17C = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _115_MCP_1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _116_IL_17A = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _117_CXCL11 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _118_AXIN1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _120_TRAIL = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _121_IL_20RA = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _122_CXCL9 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _123_CST5 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _124_IL_2RB = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _125_IL_1_alpha = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _126_OSM = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _127_IL_2 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _128_CXCL1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _129_TSLP = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _130_CCL4 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _131_CD6 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _132_SCF = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _133_IL_18 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _134_SLAMF1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _135_TGFA = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _136_MCP_4 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _137_CCL11 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _138_TNFSF14 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _139_FGF_23 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _140_IL_10RA = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _141_FGF_5 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _142_MMP_1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _143_LIF_R = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _144_FGF_21 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _145_CCL19 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _148_IL_15RA = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _149_IL_10RB = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _150_IL_22_RA1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _151_IL_18R1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _152_PD_L1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _153_Beta_NGF = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _154_CXCL5 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _155_TRANCE = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _156_HGF = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _157_IL_12B = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _158_IL_24 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _159_IL_13 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _160_ARTN = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _161_MMP_10 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _162_IL_10 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _163_TNF = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _164_CCL23 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _165_CD5 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _166_MIP_1_alpha = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _167_Flt3L = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _168_CXCL6 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _169_CXCL10 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _170_4E_BP1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _171_IL_20 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _172_SIRT2 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _173_CCL28 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _174_DNER = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _175_EN_RAGE = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _176_CD40 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _177_IL_33 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _178_IFN_gamma = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _179_FGF_19 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _180_IL_4 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _181_LIF = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _182_NRTN = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _183_MCP_2 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _184_CASP_8 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _185_CCL25 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _186_CX3CL1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _187_TNFRSF9 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _188_NT_3 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _189_TWEAK = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _190_CCL20 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _191_ST1A1 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _192_STAMPB = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _193_IL_5 = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _194_ADA = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _195_TNFB = batch;
run;

proc anova data=olinkmkIV;
class batch;
model _196_CSF_1 = batch;
run;

/*non parametric test analysis*/

   ods graphics on;
   proc npar1way data=olinkmkIV wilcoxon median 
         plots=(wilcoxonboxplot medianplot);
      class batch;
      var _101_IL_8 _102_VEGF_A _103_BDNF _105_MCP_3 _106_hGDNF _107_CDCP1 _108_CD244 _109_IL_7 _110_OPG _111_LAP_TGF_beta_1 _112_uPA _113_IL_6 _114_IL_17C _115_MCP_1 _116_IL_17A _117_CXCL11 _118_AXIN1 _120_TRAIL _121_IL_20RA _122_CXCL9 _123_CST5 _124_IL_2RB _125_IL_1_alpha _126_OSM _127_IL_2 _128_CXCL1 _129_TSLP _130_CCL4 _131_CD6 _132_SCF _133_IL_18 _134_SLAMF1 _135_TGFA _136_MCP_4 _137_CCL11 _138_TNFSF14 _139_FGF_23 _140_IL_10RA _141_FGF_5 _142_MMP_1 _143_LIF_R _144_FGF_21 _145_CCL19 _148_IL_15RA _149_IL_10RB _150_IL_22_RA1 _151_IL_18R1 _152_PD_L1 _153_Beta_NGF _154_CXCL5 _155_TRANCE _156_HGF _157_IL_12B _158_IL_24 _159_IL_13 _160_ARTN _161_MMP_10 _162_IL_10 _163_TNF _164_CCL23 _165_CD5 _166_MIP_1_alpha _167_Flt3L _168_CXCL6 _169_CXCL10 _170_4E_BP1 _171_IL_20 _172_SIRT2 _173_CCL28 _174_DNER _175_EN_RAGE _176_CD40 _177_IL_33 _178_IFN_gamma _179_FGF_19 _180_IL_4 _181_LIF _182_NRTN _183_MCP_2 _184_CASP_8 _185_CCL25 _186_CX3CL1 _187_TNFRSF9 _188_NT_3 _189_TWEAK _190_CCL20 _191_ST1A1 _192_STAMPB _193_IL_5 _194_ADA _195_TNFB _196_CSF_1;
      run;
   ods graphics off;

/*Alternatively, we can use proc univariate since the two samples are not independent and perform the non parametric test on the difference between observations*/

proc print data = olinkmkIII;
run;

proc univariate data = olinkmkIII;
var  _101_IL_8_D _102_VEGF_A_D _103_BDNF_D _105_MCP_3_D _106_hGDNF_D _107_CDCP1_D _108_CD244_D _109_IL_7_D _110_OPG_D _111_LAP_TGF_beta_1_D _112_uPA_D _113_IL_6_D _114_IL_17C_D _115_MCP_1_D _116_IL_17A_D _117_CXCL11_D _118_AXIN1_D _120_TRAIL_D _121_IL_20RA_D _122_CXCL9_D _123_CST5_D _124_IL_2RB_D _125_IL_1_alpha_D _126_OSM_D _127_IL_2_D _128_CXCL1_D _129_TSLP_D _130_CCL4_D _131_CD6_D _132_SCF_D _133_IL_18_D _134_SLAMF1_D _135_TGFA_D _136_MCP_4_D _137_CCL11_D _138_TNFSF14_D _139_FGF_23_D _140_IL_10RA_D _141_FGF_5_D _142_MMP_1_D _143_LIF_R_D _144_FGF_21_D _145_CCL19_D _148_IL_15RA_D _149_IL_10RB_D _150_IL_22_RA1_D _151_IL_18R1_D _152_PD_L1_D _153_Beta_NGF_D _154_CXCL5_D _155_TRANCE_D _156_HGF_D _157_IL_12B_D _158_IL_24_D _159_IL_13_D _160_ARTN_D _161_MMP_10_D _162_IL_10_D _163_TNF_D _164_CCL23_D _165_CD5_D _166_MIP_1_alpha_D _167_Flt3L_D _168_CXCL6_D _169_CXCL10_D _170_4E_BP1_D _171_IL_20_D _172_SIRT2_D _173_CCL28_D _174_DNER_D _175_EN_RAGE_D _176_CD40_D _177_IL_33_D _178_IFN_gamma_D _179_FGF_19_D _180_IL_4_D _181_LIF_D _182_NRTN_D _183_MCP_2_D _184_CASP_8_D _185_CCL25_D _186_CX3CL1_D _187_TNFRSF9_D _188_NT_3_D _189_TWEAK_D _190_CCL20_D _191_ST1A1_D _192_STAMPB_D _193_IL_5_D _194_ADA_D _195_TNFB_D _196_CSF_1_D;
output out = nonparametric 
MSIGN=_101_IL_8_D _102_VEGF_A_D _103_BDNF_D _105_MCP_3_D _106_hGDNF_D _107_CDCP1_D _108_CD244_D _109_IL_7_D _110_OPG_D _111_LAP_TGF_beta_1_D _112_uPA_D _113_IL_6_D _114_IL_17C_D _115_MCP_1_D _116_IL_17A_D _117_CXCL11_D _118_AXIN1_D _120_TRAIL_D _121_IL_20RA_D _122_CXCL9_D _123_CST5_D _124_IL_2RB_D _125_IL_1_alpha_D _126_OSM_D _127_IL_2_D _128_CXCL1_D _129_TSLP_D _130_CCL4_D _131_CD6_D _132_SCF_D _133_IL_18_D _134_SLAMF1_D _135_TGFA_D _136_MCP_4_D _137_CCL11_D _138_TNFSF14_D _139_FGF_23_D _140_IL_10RA_D _141_FGF_5_D _142_MMP_1_D _143_LIF_R_D _144_FGF_21_D _145_CCL19_D _148_IL_15RA_D _149_IL_10RB_D _150_IL_22_RA1_D _151_IL_18R1_D _152_PD_L1_D _153_Beta_NGF_D _154_CXCL5_D _155_TRANCE_D _156_HGF_D _157_IL_12B_D _158_IL_24_D _159_IL_13_D _160_ARTN_D _161_MMP_10_D _162_IL_10_D _163_TNF_D _164_CCL23_D _165_CD5_D _166_MIP_1_alpha_D _167_Flt3L_D _168_CXCL6_D _169_CXCL10_D _170_4E_BP1_D _171_IL_20_D _172_SIRT2_D _173_CCL28_D _174_DNER_D _175_EN_RAGE_D _176_CD40_D _177_IL_33_D _178_IFN_gamma_D _179_FGF_19_D _180_IL_4_D _181_LIF_D _182_NRTN_D _183_MCP_2_D _184_CASP_8_D _185_CCL25_D _186_CX3CL1_D _187_TNFRSF9_D _188_NT_3_D _189_TWEAK_D _190_CCL20_D _191_ST1A1_D _192_STAMPB_D _193_IL_5_D _194_ADA_D _195_TNFB_D _196_CSF_1_D;
run;

proc print data=nonparametric;
run;

proc univariate data = olinkmkIII;
var  _101_IL_8_D _102_VEGF_A_D _103_BDNF_D _105_MCP_3_D _106_hGDNF_D _107_CDCP1_D _108_CD244_D _109_IL_7_D _110_OPG_D _111_LAP_TGF_beta_1_D _112_uPA_D _113_IL_6_D _114_IL_17C_D _115_MCP_1_D _116_IL_17A_D _117_CXCL11_D _118_AXIN1_D _120_TRAIL_D _121_IL_20RA_D _122_CXCL9_D _123_CST5_D _124_IL_2RB_D _125_IL_1_alpha_D _126_OSM_D _127_IL_2_D _128_CXCL1_D _129_TSLP_D _130_CCL4_D _131_CD6_D _132_SCF_D _133_IL_18_D _134_SLAMF1_D _135_TGFA_D _136_MCP_4_D _137_CCL11_D _138_TNFSF14_D _139_FGF_23_D _140_IL_10RA_D _141_FGF_5_D _142_MMP_1_D _143_LIF_R_D _144_FGF_21_D _145_CCL19_D _148_IL_15RA_D _149_IL_10RB_D _150_IL_22_RA1_D _151_IL_18R1_D _152_PD_L1_D _153_Beta_NGF_D _154_CXCL5_D _155_TRANCE_D _156_HGF_D _157_IL_12B_D _158_IL_24_D _159_IL_13_D _160_ARTN_D _161_MMP_10_D _162_IL_10_D _163_TNF_D _164_CCL23_D _165_CD5_D _166_MIP_1_alpha_D _167_Flt3L_D _168_CXCL6_D _169_CXCL10_D _170_4E_BP1_D _171_IL_20_D _172_SIRT2_D _173_CCL28_D _174_DNER_D _175_EN_RAGE_D _176_CD40_D _177_IL_33_D _178_IFN_gamma_D _179_FGF_19_D _180_IL_4_D _181_LIF_D _182_NRTN_D _183_MCP_2_D _184_CASP_8_D _185_CCL25_D _186_CX3CL1_D _187_TNFRSF9_D _188_NT_3_D _189_TWEAK_D _190_CCL20_D _191_ST1A1_D _192_STAMPB_D _193_IL_5_D _194_ADA_D _195_TNFB_D _196_CSF_1_D;
output out = nonparametric2 
SIGNRANK=_101_IL_8_D _102_VEGF_A_D _103_BDNF_D _105_MCP_3_D _106_hGDNF_D _107_CDCP1_D _108_CD244_D _109_IL_7_D _110_OPG_D _111_LAP_TGF_beta_1_D _112_uPA_D _113_IL_6_D _114_IL_17C_D _115_MCP_1_D _116_IL_17A_D _117_CXCL11_D _118_AXIN1_D _120_TRAIL_D _121_IL_20RA_D _122_CXCL9_D _123_CST5_D _124_IL_2RB_D _125_IL_1_alpha_D _126_OSM_D _127_IL_2_D _128_CXCL1_D _129_TSLP_D _130_CCL4_D _131_CD6_D _132_SCF_D _133_IL_18_D _134_SLAMF1_D _135_TGFA_D _136_MCP_4_D _137_CCL11_D _138_TNFSF14_D _139_FGF_23_D _140_IL_10RA_D _141_FGF_5_D _142_MMP_1_D _143_LIF_R_D _144_FGF_21_D _145_CCL19_D _148_IL_15RA_D _149_IL_10RB_D _150_IL_22_RA1_D _151_IL_18R1_D _152_PD_L1_D _153_Beta_NGF_D _154_CXCL5_D _155_TRANCE_D _156_HGF_D _157_IL_12B_D _158_IL_24_D _159_IL_13_D _160_ARTN_D _161_MMP_10_D _162_IL_10_D _163_TNF_D _164_CCL23_D _165_CD5_D _166_MIP_1_alpha_D _167_Flt3L_D _168_CXCL6_D _169_CXCL10_D _170_4E_BP1_D _171_IL_20_D _172_SIRT2_D _173_CCL28_D _174_DNER_D _175_EN_RAGE_D _176_CD40_D _177_IL_33_D _178_IFN_gamma_D _179_FGF_19_D _180_IL_4_D _181_LIF_D _182_NRTN_D _183_MCP_2_D _184_CASP_8_D _185_CCL25_D _186_CX3CL1_D _187_TNFRSF9_D _188_NT_3_D _189_TWEAK_D _190_CCL20_D _191_ST1A1_D _192_STAMPB_D _193_IL_5_D _194_ADA_D _195_TNFB_D _196_CSF_1_D;;
run;

proc print data=nonparametric2;
run;

proc univariate data = olinkmkIII;
var  _101_IL_8_D _102_VEGF_A_D _103_BDNF_D _105_MCP_3_D _106_hGDNF_D _107_CDCP1_D _108_CD244_D _109_IL_7_D _110_OPG_D _111_LAP_TGF_beta_1_D _112_uPA_D _113_IL_6_D _114_IL_17C_D _115_MCP_1_D _116_IL_17A_D _117_CXCL11_D _118_AXIN1_D _120_TRAIL_D _121_IL_20RA_D _122_CXCL9_D _123_CST5_D _124_IL_2RB_D _125_IL_1_alpha_D _126_OSM_D _127_IL_2_D _128_CXCL1_D _129_TSLP_D _130_CCL4_D _131_CD6_D _132_SCF_D _133_IL_18_D _134_SLAMF1_D _135_TGFA_D _136_MCP_4_D _137_CCL11_D _138_TNFSF14_D _139_FGF_23_D _140_IL_10RA_D _141_FGF_5_D _142_MMP_1_D _143_LIF_R_D _144_FGF_21_D _145_CCL19_D _148_IL_15RA_D _149_IL_10RB_D _150_IL_22_RA1_D _151_IL_18R1_D _152_PD_L1_D _153_Beta_NGF_D _154_CXCL5_D _155_TRANCE_D _156_HGF_D _157_IL_12B_D _158_IL_24_D _159_IL_13_D _160_ARTN_D _161_MMP_10_D _162_IL_10_D _163_TNF_D _164_CCL23_D _165_CD5_D _166_MIP_1_alpha_D _167_Flt3L_D _168_CXCL6_D _169_CXCL10_D _170_4E_BP1_D _171_IL_20_D _172_SIRT2_D _173_CCL28_D _174_DNER_D _175_EN_RAGE_D _176_CD40_D _177_IL_33_D _178_IFN_gamma_D _179_FGF_19_D _180_IL_4_D _181_LIF_D _182_NRTN_D _183_MCP_2_D _184_CASP_8_D _185_CCL25_D _186_CX3CL1_D _187_TNFRSF9_D _188_NT_3_D _189_TWEAK_D _190_CCL20_D _191_ST1A1_D _192_STAMPB_D _193_IL_5_D _194_ADA_D _195_TNFB_D _196_CSF_1_D;
output out = nonparametric3 
NORMAL=_101_IL_8_D _102_VEGF_A_D _103_BDNF_D _105_MCP_3_D _106_hGDNF_D _107_CDCP1_D _108_CD244_D _109_IL_7_D _110_OPG_D _111_LAP_TGF_beta_1_D _112_uPA_D _113_IL_6_D _114_IL_17C_D _115_MCP_1_D _116_IL_17A_D _117_CXCL11_D _118_AXIN1_D _120_TRAIL_D _121_IL_20RA_D _122_CXCL9_D _123_CST5_D _124_IL_2RB_D _125_IL_1_alpha_D _126_OSM_D _127_IL_2_D _128_CXCL1_D _129_TSLP_D _130_CCL4_D _131_CD6_D _132_SCF_D _133_IL_18_D _134_SLAMF1_D _135_TGFA_D _136_MCP_4_D _137_CCL11_D _138_TNFSF14_D _139_FGF_23_D _140_IL_10RA_D _141_FGF_5_D _142_MMP_1_D _143_LIF_R_D _144_FGF_21_D _145_CCL19_D _148_IL_15RA_D _149_IL_10RB_D _150_IL_22_RA1_D _151_IL_18R1_D _152_PD_L1_D _153_Beta_NGF_D _154_CXCL5_D _155_TRANCE_D _156_HGF_D _157_IL_12B_D _158_IL_24_D _159_IL_13_D _160_ARTN_D _161_MMP_10_D _162_IL_10_D _163_TNF_D _164_CCL23_D _165_CD5_D _166_MIP_1_alpha_D _167_Flt3L_D _168_CXCL6_D _169_CXCL10_D _170_4E_BP1_D _171_IL_20_D _172_SIRT2_D _173_CCL28_D _174_DNER_D _175_EN_RAGE_D _176_CD40_D _177_IL_33_D _178_IFN_gamma_D _179_FGF_19_D _180_IL_4_D _181_LIF_D _182_NRTN_D _183_MCP_2_D _184_CASP_8_D _185_CCL25_D _186_CX3CL1_D _187_TNFRSF9_D _188_NT_3_D _189_TWEAK_D _190_CCL20_D _191_ST1A1_D _192_STAMPB_D _193_IL_5_D _194_ADA_D _195_TNFB_D _196_CSF_1_D;;
run;

proc print data=nonparametric3;
run;
 
proc univariate data = olinkmkIII;
var  _101_IL_8_D _102_VEGF_A_D _103_BDNF_D _105_MCP_3_D _106_hGDNF_D _107_CDCP1_D _108_CD244_D _109_IL_7_D _110_OPG_D _111_LAP_TGF_beta_1_D _112_uPA_D _113_IL_6_D _114_IL_17C_D _115_MCP_1_D _116_IL_17A_D _117_CXCL11_D _118_AXIN1_D _120_TRAIL_D _121_IL_20RA_D _122_CXCL9_D _123_CST5_D _124_IL_2RB_D _125_IL_1_alpha_D _126_OSM_D _127_IL_2_D _128_CXCL1_D _129_TSLP_D _130_CCL4_D _131_CD6_D _132_SCF_D _133_IL_18_D _134_SLAMF1_D _135_TGFA_D _136_MCP_4_D _137_CCL11_D _138_TNFSF14_D _139_FGF_23_D _140_IL_10RA_D _141_FGF_5_D _142_MMP_1_D _143_LIF_R_D _144_FGF_21_D _145_CCL19_D _148_IL_15RA_D _149_IL_10RB_D _150_IL_22_RA1_D _151_IL_18R1_D _152_PD_L1_D _153_Beta_NGF_D _154_CXCL5_D _155_TRANCE_D _156_HGF_D _157_IL_12B_D _158_IL_24_D _159_IL_13_D _160_ARTN_D _161_MMP_10_D _162_IL_10_D _163_TNF_D _164_CCL23_D _165_CD5_D _166_MIP_1_alpha_D _167_Flt3L_D _168_CXCL6_D _169_CXCL10_D _170_4E_BP1_D _171_IL_20_D _172_SIRT2_D _173_CCL28_D _174_DNER_D _175_EN_RAGE_D _176_CD40_D _177_IL_33_D _178_IFN_gamma_D _179_FGF_19_D _180_IL_4_D _181_LIF_D _182_NRTN_D _183_MCP_2_D _184_CASP_8_D _185_CCL25_D _186_CX3CL1_D _187_TNFRSF9_D _188_NT_3_D _189_TWEAK_D _190_CCL20_D _191_ST1A1_D _192_STAMPB_D _193_IL_5_D _194_ADA_D _195_TNFB_D _196_CSF_1_D;
output out = nonparametric4 
CV=_101_IL_8_D _102_VEGF_A_D _103_BDNF_D _105_MCP_3_D _106_hGDNF_D _107_CDCP1_D _108_CD244_D _109_IL_7_D _110_OPG_D _111_LAP_TGF_beta_1_D _112_uPA_D _113_IL_6_D _114_IL_17C_D _115_MCP_1_D _116_IL_17A_D _117_CXCL11_D _118_AXIN1_D _120_TRAIL_D _121_IL_20RA_D _122_CXCL9_D _123_CST5_D _124_IL_2RB_D _125_IL_1_alpha_D _126_OSM_D _127_IL_2_D _128_CXCL1_D _129_TSLP_D _130_CCL4_D _131_CD6_D _132_SCF_D _133_IL_18_D _134_SLAMF1_D _135_TGFA_D _136_MCP_4_D _137_CCL11_D _138_TNFSF14_D _139_FGF_23_D _140_IL_10RA_D _141_FGF_5_D _142_MMP_1_D _143_LIF_R_D _144_FGF_21_D _145_CCL19_D _148_IL_15RA_D _149_IL_10RB_D _150_IL_22_RA1_D _151_IL_18R1_D _152_PD_L1_D _153_Beta_NGF_D _154_CXCL5_D _155_TRANCE_D _156_HGF_D _157_IL_12B_D _158_IL_24_D _159_IL_13_D _160_ARTN_D _161_MMP_10_D _162_IL_10_D _163_TNF_D _164_CCL23_D _165_CD5_D _166_MIP_1_alpha_D _167_Flt3L_D _168_CXCL6_D _169_CXCL10_D _170_4E_BP1_D _171_IL_20_D _172_SIRT2_D _173_CCL28_D _174_DNER_D _175_EN_RAGE_D _176_CD40_D _177_IL_33_D _178_IFN_gamma_D _179_FGF_19_D _180_IL_4_D _181_LIF_D _182_NRTN_D _183_MCP_2_D _184_CASP_8_D _185_CCL25_D _186_CX3CL1_D _187_TNFRSF9_D _188_NT_3_D _189_TWEAK_D _190_CCL20_D _191_ST1A1_D _192_STAMPB_D _193_IL_5_D _194_ADA_D _195_TNFB_D _196_CSF_1_D;;
run;

proc print data=nonparametric4;
run;

data nonparametricsummary;
set nonparametric nonparametric2 nonparametric3 nonparametric4;
run;

proc print data = nonparametricsummary;
run;

ods graphics on;
proc univariate data = olinkmkIII normal plots;
histogram;
var _101_IL_8_U _102_VEGF_A_U _103_BDNF_U _105_MCP_3_U _106_hGDNF_U _107_CDCP1_U _108_CD244_U _109_IL_7_U _110_OPG_U _111_LAP_TGF_beta_1_U _112_uPA_U _113_IL_6_U _114_IL_17C_U _115_MCP_1_U _116_IL_17A_U _117_CXCL11_U _118_AXIN1_U _120_TRAIL_U _121_IL_20RA_U _122_CXCL9_U _123_CST5_U _124_IL_2RB_U _125_IL_1_alpha_U _126_OSM_U _127_IL_2_U _128_CXCL1_U _129_TSLP_U _130_CCL4_U _131_CD6_U _132_SCF_U _133_IL_18_U _134_SLAMF1_U _135_TGFA_U _136_MCP_4_U _137_CCL11_U _138_TNFSF14_U _139_FGF_23_U _140_IL_10RA_U _141_FGF_5_U _142_MMP_1_U _143_LIF_R_U _144_FGF_21_U _145_CCL19_U _148_IL_15RA_U _149_IL_10RB_U _150_IL_22_RA1_U _151_IL_18R1_U _152_PD_L1_U _153_Beta_NGF_U _154_CXCL5_U _155_TRANCE_U _156_HGF_U _157_IL_12B_U _158_IL_24_U _159_IL_13_U _160_ARTN_U _161_MMP_10_U _162_IL_10_U _163_TNF_U _164_CCL23_U _165_CD5_U _166_MIP_1_alpha_U _167_Flt3L_U _168_CXCL6_U _169_CXCL10_U _170_4E_BP1_U _171_IL_20_U _172_SIRT2_U _173_CCL28_U _174_DNER_U _175_EN_RAGE_U _176_CD40_U _177_IL_33_U _178_IFN_gamma_U _179_FGF_19_U _180_IL_4_U _181_LIF_U _182_NRTN_U _183_MCP_2_U _184_CASP_8_U _185_CCL25_U _186_CX3CL1_U _187_TNFRSF9_U _188_NT_3_U _189_TWEAK_U _190_CCL20_U _191_ST1A1_U _192_STAMPB_U _193_IL_5_U _194_ADA_U _195_TNFB_U _196_CSF_1_U _101_IL_8_M _102_VEGF_A_M _103_BDNF_M _105_MCP_3_M _106_hGDNF_M _107_CDCP1_M _108_CD244_M _109_IL_7_M _110_OPG_M _111_LAP_TGF_beta_1_M _112_uPA_M _113_IL_6_M _114_IL_17C_M _115_MCP_1_M _116_IL_17A_M _117_CXCL11_M _118_AXIN1_M _120_TRAIL_M _121_IL_20RA_M _122_CXCL9_M _123_CST5_M _124_IL_2RB_M _125_IL_1_alpha_M _126_OSM_M _127_IL_2_M _128_CXCL1_M _129_TSLP_M _130_CCL4_M _131_CD6_M _132_SCF_M _133_IL_18_M _134_SLAMF1_M _135_TGFA_M _136_MCP_4_M _137_CCL11_M _138_TNFSF14_M _139_FGF_23_M _140_IL_10RA_M _141_FGF_5_M _142_MMP_1_M _143_LIF_R_M _144_FGF_21_M _145_CCL19_M _148_IL_15RA_M _149_IL_10RB_M _150_IL_22_RA1_M _151_IL_18R1_M _152_PD_L1_M _153_Beta_NGF_M _154_CXCL5_M _155_TRANCE_M _156_HGF_M _157_IL_12B_M _158_IL_24_M _159_IL_13_M _160_ARTN_M _161_MMP_10_M _162_IL_10_M _163_TNF_M _164_CCL23_M _165_CD5_M _166_MIP_1_alpha_M _167_Flt3L_M _168_CXCL6_M _169_CXCL10_M _170_4E_BP1_M _171_IL_20_M _172_SIRT2_M _173_CCL28_M _174_DNER_M _175_EN_RAGE_M _176_CD40_M _177_IL_33_M _178_IFN_gamma_M _179_FGF_19_M _180_IL_4_M _181_LIF_M _182_NRTN_M _183_MCP_2_M _184_CASP_8_M _185_CCL25_M _186_CX3CL1_M _187_TNFRSF9_M _188_NT_3_M _189_TWEAK_M _190_CCL20_M _191_ST1A1_M _192_STAMPB_M _193_IL_5_M _194_ADA_M _195_TNFB_M _196_CSF_1_M;
run;
ods graphics off;


/*Categorizing the correlations*/

PROC IMPORT OUT= mylib.icc DATAFILE= "C:\Users\onyea005\Desktop\OLINK\Data Analysis\ICC Table.xlsx" DBMS=xlsx REPLACE;
     SHEET="Sheet2"; 
     GETNAMES=YES;
RUN;

proc print data = mylib.icc;
run;

data icc;
set mylib.icc;
if correlation le 0.3 then linearcat = 1;
if 0.3 lt correlation le 0.5 then linearcat = 2;
if 0.5 lt correlation le 0.7 then linearcat = 3;
if correlation gt 0.7 then linearcat = 4;
if ICC le 0.3 then intracat = 1;
if 0.3 lt ICC le 0.5 then intracat = 2;
if 0.5 lt ICC le 0.7 then intracat = 3;
if ICC gt 0.7 then intracat = 4; 
run;

proc print data = ICC;
run;

proc freq data = ICC;
tables linearcat intracat;
run;

/*Sample codes for ICC*/

/* Trying to find the ICC */
/* Sample Code 
PROC NLMIXED data=dental method=firo ;
parms mu=10 s_subj = 50 s_err = 6 ;
pred = mu + beta ;
model score ~ normal(pred, s_err) ;
random beta ~ normal(0, s_subj)
subject = patient ;
estimate 'icc'
s_subj/(s_subj+s_err);
run ; */

PROC NLMIXED data=olinkmkIII method=firo ;
parms mu=10 s_subj = 50 s_err = 6 ;
pred = mu + beta ;
model score ~ normal(pred, s_err) ;
random beta ~ normal(0, s_subj)
subject = sample_ID ;
estimate 'icc'
s_subj/(s_subj+s_err);
run ; 

/*Sample Code 2

ods output CovParms = cov1 ;
proc mixed data=dental method=ml;
class patient ;
model score = ;
random patient ;
run ;

for this one, use the dataset where check 1 and check 2 are both equal to one, but stacked as row instead of separating them into columns
*/

data olinkmkIV;
set MDL UMGC;
run;

/* data olinkmkIV;
set olinkmkIV;
if sample_ID = 1879 then keep; 
if sample_ID =1887 then keep;
if sample_ID =2325 then keep;
if sample_ID =2427 then keep;
if sample_ID =2463 then keep;
if sample_ID =2684 then keep;
if sample_ID =2896 then keep;
if sample_ID =3033 then keep;
if sample_ID =3379 then keep;
if sample_ID =3796 then keep;
if sample_ID =3958 then keep;
if sample_ID =4141 then keep;
if sample_ID =4651 then keep;
if sample_ID =5197 then keep;
if sample_ID =5227 then keep;
if sample_ID =5743 then keep;
if sample_ID =6141 then keep; 
if sample_ID =6807 then keep;
if sample_ID =11482 then keep;
if sample_ID =14136 then keep;
if sample_ID =15266 then keep;
if sample_ID =16191 then keep;
if sample_ID =17047 then keep;
if sample_ID =18298 then keep;
if sample_ID =18429 then keep; 
if sample_ID =19218 then keep;
if sample_ID =19645 then keep;
if sample_ID =20011 then keep;
if sample_ID =22827 then keep;
if sample_ID =24120 then keep;
run;
*/

ods output CovParms = cov1 ;
proc mixed data=dental method=ml;
class batch ;
model _101_IL_8 = sample_ID 
_102_VEGF_A = sample_ID 
_103_BDNF = sample_ID 
_105_MCP_3 = sample_ID 
_106_hGDNF = sample_ID 
_107_CDCP1 = sample_ID 
_108_CD244 = sample_ID 
_109_IL_7 = sample_ID 
_110_OPG = sample_ID 
_111_LAP_TGF_beta_1 = sample_ID 
_112_uPA = sample_ID 
_113_IL_6 = sample_ID 
_114_IL_17C = sample_ID 
_115_MCP_1 = sample_ID 
_116_IL_17A = sample_ID 
_117_CXCL11 = sample_ID 
_118_AXIN1 = sample_ID 
_120_TRAIL = sample_ID 
_121_IL_20RA = sample_ID 
_122_CXCL9 = sample_ID 
_123_CST5 = sample_ID 
_124_IL_2RB = sample_ID 
_125_IL_1_alpha  = sample_ID
_126_OSM = sample_ID 
_127_IL_2 = sample_ID 
_128_CXCL1 = sample_ID 
_129_TSLP = sample_ID 
_130_CCL4 = sample_ID 
_131_CD6 = sample_ID 
_132_SCF = sample_ID 
_133_IL_18 = sample_ID 
_134_SLAMF1 = sample_ID 
_135_TGFA = sample_ID 
_136_MCP_4 = sample_ID 
_137_CCL11 = sample_ID 
_138_TNFSF14 = sample_ID
_139_FGF_23 = sample_ID
_140_IL_10RA  = sample_ID
_141_FGF_5 = sample_ID
_142_MMP_1 = sample_ID
_143_LIF_R = sample_ID
_144_FGF_21 = sample_ID
_145_CCL19 = sample_ID 
_148_IL_15RA = sample_ID
_149_IL_10RB = sample_ID
_150_IL_22_RA1 = sample_ID 
_151_IL_18R1 = sample_ID 
_152_PD_L1 = sample_ID 
_153_Beta_NGF = sample_ID 
_154_CXCL5 = sample_ID 
_155_TRANCE = sample_ID 
_156_HGF  = sample_ID 
_157_IL_12B = sample_ID 
_158_IL_24 = sample_ID 
_159_IL_13 = sample_ID 
_160_ARTN = sample_ID 
_161_MMP_10 = sample_ID 
_162_IL_10 = sample_ID 
_163_TNF = sample_ID 
_164_CCL23 = sample_ID 
_165_CD5 = sample_ID 
_166_MIP_1_alpha = sample_ID
_167_Flt3L = sample_ID 
_168_CXCL6 = sample_ID 
_169_CXCL10 = sample_ID 
_170_4E_BP1 = sample_ID 
_171_IL_20 = sample_ID 
_172_SIRT2 = sample_ID 
_173_CCL28 = sample_ID 
_174_DNER = sample_ID 
_175_EN_RAGE = sample_ID 
_176_CD40 = sample_ID 
_177_IL_33 = sample_ID 
_178_IFN_gamma = sample_ID 
_179_FGF_19 = sample_ID 
_180_IL_4 = sample_ID 
_181_LIF = sample_ID 
_182_NRTN = sample_ID 
_183_MCP_2 = sample_ID 
_184_CASP_8 = sample_ID 
_185_CCL25 = sample_ID 
_186_CX3CL1 = sample_ID 
_187_TNFRSF9 = sample_ID 
_188 = sample_ID_NT_3 = sample_ID 
_189_TWEAK = sample_ID 
_190_CCL20 = sample_ID 
_191_ST1A1 = sample_ID 
_192_STAMPB = sample_ID 
_193_IL_5 = sample_ID 
_194_ADA = sample_ID 
_195_TNFB = sample_ID 
_196_CSF_1 = sample_ID ;
random batch ;
run ;
