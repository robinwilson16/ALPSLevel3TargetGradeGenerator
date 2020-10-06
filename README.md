# ALPS Level 3 Target Grade Generator
**Summary**

These scripts calculate target grade for learners by first working out an Average GCSE Score by taking a mean average of all GCSE results from the PLR using the size of the qualification (short course is 0.5, standard GCSE is 1 and double award is 2) and then using this to look at the learning aim type of the level 3 qualification being studied (i.e. A Level, BTEC Extended Diploma, etc.) in order to assign a target grade to the learners.

> The calculation for average GCSE score is to add up all GCSE grades and then take the mean average (e.g. for 4 standard size GCSEs ( 9 + 7 + 8 + 8 ) / 4 = 8)

Targets are assigned by looking at the academic and vocational ALPS Benchmarks available from the ALPS Education website at https://alps.education/

These targets are updated each year so the mapping table should be reviewed yearly to ensure it is still up to date.

An example of these ALPS Benchmark tables is here:

Academic:

![A Level ALPS Table](https://github.com/robinwilson16/ALPSLevel3TargetGradeGenerator/blob/master/Target%20Grade%20Mapping%20Tables/A%20Level.png)

BTEC:

![BTEC ALPS Table 1](https://github.com/robinwilson16/ALPSLevel3TargetGradeGenerator/blob/master/Target%20Grade%20Mapping%20Tables/BTEC1.png)
![BTEC ALPS Table 2](https://github.com/robinwilson16/ALPSLevel3TargetGradeGenerator/blob/master/Target%20Grade%20Mapping%20Tables/BTEC2.png)

**Setting Up**

1. Use scripts in `Tables with Data` folder to create and populate ALPS data used in the calculations.
2. :
2a. Use scripts in `Stored Procedures to Update ProSolution` folder to create scripts to update ProSolution user fields (if required).
2b. Use script in `View to Import into ProMonitor` folder to create view to be used by ProMonitor Target import (if required)
3. Use Exec script contained in `Run ProSolution Import` folder to execute ProSolution import
> Ensure you review the user fields being used in ProSolution to ensure these are not already being used for something else and amend as appropriate.
> Ensure ProMonitor script will insert data into correct target field to confirm that personal targets entered by staff will not be overwritten.
> Recommend that this is tested on the TRAINING systems before running on live.

## Updating the Calculations Each Year

**Downloading Yearly ALPS Grade Mappings and Boundaries**

1. Go to https://alps.education/
2. Click the latest tab at the top
3. Click Briefing Papers
3a. For A Levels use `Benchmark Update - England`
3b. For BTEC use `Benchmark Update - BTEC England`

**Updating ALPS Grade Boundaries in Table**

Using the information above, open the table called ALPS_TargetSetting which contains the average GCSE grade boundaries and the L3 target grades these would generate depending on the type of subject being studied.
Ensure the grade and points are correct as these are the most likely thing to have changed. Also check the grade boundaries are still valid too.


> There have been extensive changes in 2019/20 due to the move to the 9-1 GCSE grading system (before 9-1 grades were mapped to A*-G)

Next open the table called `GradePoints` used to calculate the average GCSE score (will not be needed once everything is reformed and 1-9)
Ensure each grade has the correct grade points mapping. This information is in `Benchmark Update - England`

![GCSE Mapping Letters to Numerical Grades](https://github.com/robinwilson16/ALPSLevel3TargetGradeGenerator/blob/master/Target%20Grade%20Mapping%20Tables/GCSEs.png)

## Level 2 Targets

Level 2 targets are also now calculated and the methodology for this is as follows:

**For GCSE Maths and English:**

* To establish highest GCSE Maths and English grades look at PLR, QOE, CoF and prior college achieved quals and rank and pick highest ones
* If grade is < [VALUE IN CONFIG TABLE] make target [VALUE IN CONFIG TABLE]
* Otherwise add + [VALUE IN CONFIG TABLE] grades to grade (e.g. so if grade was 5 then target would be 6 or 7 depending on values set)


**For Other Level 2s that are Pass/Fail**
* To establish that course is pass/fail exclude any aims where we have previously graded as M and D in any year or A - G in any year
* Aim is level 2
* Aim type is not GCSE

**For Other Level 2s that are P/M/D**
* Set target to [VALUE IN CONFIG TABLE]
* If value in config table is NULL then this part is disabled