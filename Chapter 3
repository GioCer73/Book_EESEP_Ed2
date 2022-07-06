********************************************************************************
* Book: Econometric Evaluation of Socio-Economic Programs Theory and Applications
* Author: Giovanni Cerulli
* Publisher: Springer
* Edition: Second
********************************************************************************
* CHAPTER 3 - Methods Based on Selection on Unobservables
********************************************************************************
* Stata codes
********************************************************************************
* DATASETS DOWNLOAD:
https://www.dropbox.com/sh/bwlcgt0ejdzidaq/AABKz5VBnrdDu2Dkd9ZcRkI-a?dl=0
********************************************************************************
clear all
* 1.5.3 An Application to Determine the Effect of Education on Fertility
use fertil2.dta , clear
xi: reg children educ7
estimates store DIM

xi: ivtreatreg children educ7 age agesq evermarr urban electric tv , ///
hetero(age agesq evermarr urban) model(cf-ols) graphic
estimates store cfr

xi: ivtreatreg children educ7 age agesq evermarr urban electric tv , ///
hetero(age agesq evermarr urban) iv(frsthalf) model(direct-2sls) graphic
estimates store direct_2sls

xi: ivtreatreg children educ7 age agesq evermarr urban electric tv , ///
hetero(age agesq evermarr urban) iv(frsthalf) model(probit-ols) graphic
estimates store probit_ols

xi: ivtreatreg children educ7 age agesq evermarr urban electric tv , ///
hetero(age agesq evermarr urban) iv(frsthalf) model(heckit) graphic
estimates store heckit

xi: ivtreatreg children educ7 age agesq evermarr urban electric tv , ///
hetero(age agesq evermarr urban) iv(frsthalf) model(probit-2sls) graphic
estimates store probit_2sls

xi: bootstrap atet=e(atet) atent=e(atent), rep(100): ///
ivtreatreg children educ7 age agesq evermarr urban electric tv , ///
hetero(age agesq evermarr urban) iv(frsthalf) model(probit-2sls) 

estimates table  DIM cfr probit_ols direct_2sls probit_2sls heckit , ///
b(%9.5f) star keep(educ7 G_fv)   

estimates table  DIM cfr probit_ols direct_2sls probit_2sls heckit , ///
b(%9.5f) se t keep(educ7 G_fv)   


* 1.5.4 Applying the Selection-Model Using etregress
xi: etregress children age agesq evermarr urban electric tv , ///
treat(educ7 = age agesq evermarr urban electric tv)

reg children educ7 age agesq evermarr urban electric tv

* 1.6 Implementation and Application of DID

* 1.6.1 DID with Repeated Cross Sections
use DID1 , clear 
regress Y S T ST
regress Y T if S==1
regress Y S if T==1
sum Y if S==1 & T==0
sum Y if S==1 & T==1
sum Y if S==0 & T==0
sum Y if S==0 & T==1
diff Y, treated(S) period(T)

* 1.6.2 DID Application with Panel Data 
use DID2 , clear 
sort id year
by id: gen Y_1 = Y[_n-1]
by id: gen d_it_1 = d_it[_n-1]
by id: gen x1_1 = x1[_n-1]
by id: gen x2_1 = x2[_n-1]
gen delta_Y = Y-Y_1
gen delta_x1 = x1-x1_1
gen delta_x2 = x2-x2_1
gen delta_d = d_it-d_it_1
reg delta_Y d_it if d_it_1==0
reg delta_Y d_it if delta_d==d_it
reg delta_Y delta_d if d_it_1==0
reg Y d_it
reg delta_Y delta_d , noconst
tsset id year
xtreg Y d_it , fe
sum delta_Y if d_it == 1 & d_it_1==0
scalar mean_t = r(mean)
di mean_t
sum delta_Y if d_it == 0 & d_it_1==0
return list
scalar mean_c = r(mean)
di mean_c
scalar did = mean_t - mean_c
di "DID = " did
reg delta_Y d_it delta_x1 delta_x2
 
* END
 
