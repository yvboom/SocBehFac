# SocBehFac
Replication materials for "Socio-Behavioral Factors Contributing to Recent Mortality Trends in the United States"

INSTRUCTIONS FOR REPLICATION

Socio-Behavioral Factors Contributing to Recent Mortality Trends in the United States

Demographic Research, Summer 2024

Authors: S. Preston, Y. Vierboom, & M. Myrskylä
Replication files by Y. Vierboom

Table of Contents :
Part 1. Overview of the sub-folders in the “Replication_analysis” folder
Part 2. How to download the data
Part 3. What each do-file does


*-------------------------------------------------------------------------------------------------*
PART 1: Overview

Welcome to the replication files! 
In very basic terms, you get the data, save it where specified, and run the master do-file. The master do-file runs all the component do-files to produce all the numbers, tables, and figures in the text.

This analysis was run using Stata 17. If you don’t have Stata, you can open the “Logfiles” folder and see a history of the codes run by the analysis.

If you should run into any questions, please send an email to yvierboom@gmail.com.


Overview of the folders:

Data/Original. This sub-folder is empty. This where you will store the dataset you download from IPUMS. 

Data/Formatted. This is where the do-files will save created datasets.

Dofiles. This sub-folder contains the master do-file, along with its components. See the end of this document for a more in-depth look at what each do-file does.

Logfiles. In case you don’t have Stata, I’ve stored all logfiles from the analyses in this folder. If you run the do-files, they will save and overwrite new versions of the logfiles.


Results. This is empty now. This is where the do-files save their results. 

***

PART 2: Get the Data
Below, I outline how to build the raw dataset needed to run the analyses. The data is publicly available through IPUMS (Blewett et al. 2023) and will be saved in the “Data” folder.
2A. Make an account to access the IPUMS Health Surveys (https://healthsurveys.ipums.org/) and select the NHIS. 

2B. Select the following variables:
year
strata
psu 
hhweight
pernum
nhispid
hhx
fmx
px
perweight
sampweight
fweight
intervmo
intervwyr
astatflg
cstatflg
age
sex
racea
hispeth
educrec2
bmicalc
usualpl
hinotcove   
diabeticev  
alc5upyr    
alcamt      
alcstat1    
alcdayswk   
cigsday1    
smokestatus2
hearing     
aeffort     
ahopeless   
anervous    
arestless   
asad        
aworthless  
mortelig    
mortstat    
mortdodq    
mortdody    
mortucodld  
mortwt      
mortwtsa    

2C. Select samples 1997-2019

2D. Submit extract

2E. Download data 

2F. Extract and store data in Replication_files -> Data -> Original

***

PART 3. What each do-file does

***********0_MASTER_DOFILE************
This is the only do-file you need to run. But if you’re looking for more information on what each component does, see the list below.


**************1_raw_format**************
*GOAL: To format the original data.

*INPUT: Raw NHIS data downloaded from IPUMS (and saved in "Data -> Original" folder).

*STEPS:
1. Get data
2. Format and clean variables
3. Tell Stata it's survey data
4. Tell Stata it's surival data
5. Expand into person year observations
6. Variable for analysis subpop
7. Save

*OUTPUT: NHIS_PYL.dta in "Data -> Formatted"


**************2_Table1: **************
*GOAL: To create Table 1, "Characteristics fo the sample, by period of interview."

*INPUT: 
-Table1data.dta created by dofile 1_raw_format.do 
and saved in Data/Formatted
-NHIS_PYL.dta created by dofile 1_raw_format.do 
and saved in Data/Formatted

*STEPS:
1. Open data
2. Create new variables
3. Count N and put that in Table 1
4. Estimate mean age and put that in Table 1
5. Arrange other variables and their means
6. Estimate mean year of follow-up (using the PYL file)
7. Put steps 5-6 in Table 1

*OUTPUT: 
1. Table1data in Data/Formatted (later used to create Table 3)
2. Table1 in Results folder


************3_Table2************
*GOAL: Produce Table 2: "Coefficients from Cox proportional
hazard models predicting deaths per person per year."

*INPUT: 
-NHIS_PYL in Data/Formatted (made by dofile 1)

*STEPS:
1. Open data
2. Display person years
3. Run models
4. Print models in Table 2
5. Estimate model halves (later needed for Table 4)

*OUTPUT: 
[Requires running four different models
(Three in table and one cited in footnote)]
-saves each model as .dta in Data/Formatted
-saves first and second half of M3 at .dta in Data/Formatted
-Table 2 in Results


************4_Table3-4_Figure1: *************
*GOAL: Make Tables 3 and 4. Make Figure 1. 

*INPUT: The idea of Tables 3 and 4 is that it's:
distributions * coefficents
(and Figure 1 is just a graphical representation
of that).

-Table1.dta -- created by 2_Table1.do
-m1.dta -- all models creaded by 3_Table2.do
-m2.dta
-m3.dta
-m3_1.dta (first half of period modeled with m3)
-m3_2.dta (second half of period modeled with m3)


*OUTPUT: 
-Results/Table3
-Results/Table4
-Results/Figure1
-Figure.dta used in sensitivity analysis in


 *************5_AppFigure1**************
*STEPS:
1. Adjust distributions dataset (from Table 1)
2. Adjust coefficients dataset
3. Merge 1 & 2
4. Do some calculations (ie, one times the other, divided by year)
5. Export for Tables 3 and 4
6. Save there for later sensitivity analysis
7. Create Figure 1


5_AppFigure1:

*GOAL: Make Appendix Figure 1 (test of 5 vs 3
* years of follow-up. 

*INPUT:
-Table1data.dta (created by 2_Table1.do) 
-Table1.dta (created by 2_Table1.do)
-Figure.dta (created by 4_Table3-4_Figure1.do

*OUTPUT: 
-AppFigure1

*STEPS:
Basically, we need to repeat the entire analysis, using
data that's been censored at 3 years, instead of 5. So
it's a lot of steps.

1. Open and format the original data
2. Tell Stata it's survival data
3. Expand into person years
4. Subpop variable
5. Calculate sum of mean observation
6. Merge this data with other variable distributions
7. Regression
8. Adjust regression data
9. Merge distribution and coefficient data
10. Estimate contributions
11. Merge with original contributions
12. Graph differences in contributions from 3 vs 5 year censoring

// End
