# ALPS Level 3 Target Grade Generator
**Background**
Scripts to calculate the average GCSE points scores of learners using their PLR data and then use this information to calculate a target grade for the Level 3 academic (A Level) or vocational (BTEC) course they are studying using the published ALPS benchmarks.
The scripts can be run on a schedule so as new PLR data is made available, the average points score and targets will adjust.

**Summary**
The average GCSE points score script calculates the average GCSE score by mapping each grade to a numerical value and then taking the mean average.
The target grade stored procedure then obtains this average GCSE points score and the type of level 3 qualification the student has been enrolled to and uses this data along with the target setting table data to assign a target grade where the average points score falls between the min and max points.
> In the case of a learner studying an A Level with a an avg points score of 8, the target would be A*/A as current range for this grade is 7.75 to 9.

Each year the ALPS Mapping Data will need to be updated to ensure correct target grades are assigned. This is generally published over the summer for the following year.

> Only PLR data is used as a source to determine points score but the Quals on Entry data could also be used as well as any prior L2 GCSE college enrolment data (in case student has opted not to share their data). Using only a single souce ensures the same qualifications are not counted more than once.

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
Using the information above, open the table called ALPS_TargetSetting which is used to map the average points score to the subject type being studied in order to obtain the target grade.
Ensure min and max point values match the Benchmark Update documents to ensure correct target grades will be assigned.

> There have been extensive changes in 2019/20 due to the move to the 9-1 GCSE grading system (before 9-1 grades were mapped to A*-G)

Next open the `Calculate Grade Percent.xlsx` document and paste in the data from the `ALPS_TargetSetting` table to generate the correct percentage and use the formula to obtain the correct percentages.
> The percentage is (1/9) * MaxPoints (so a GCSE grade 9 would be 100% and 1 would be 11%)

Lastly open the table called ALPS_GCSEPoints used to calculate the average GCSE score (will not be needed once everything is reformed and 1-9)
Ensure each grade has the correct grade points mapping. This information is in `Benchmark Update - England`

The points is the UCAS points score the grade is worth (use ProMonitor marking schemes to work this out - could also look at Exam Board websites). This is only used in the ProMonitor import so not required if not using ProMonitor.