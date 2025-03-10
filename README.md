# HOSPITAL-PERFORMANCE-DASHBOARD
 
🏥 Hospital Performance Dashboard – Project Documentation

Author: Deepeka Gurunathan

Project Overview: This project analyzes hospital performance using Power BI, incorporating patient trends, billing insights, bed occupancy rates, and claim rejection analysis.

**Data Transformations & Column Additions**

1. Generated "Patient ID" Randomly
Purpose: To create unique identifiers for each patient.
DAX Formula:
Patient ID ="PT" & FORMAT(RANDBETWEEN(100000, 999999), "000000")
Outcome: Ensured each patient had a unique ID (e.g., PT123456).

 2. Created "Duration of Admission" Column
Purpose: To calculate the length of stay (days) between admission and discharge.
DAX Formula: Duration of Admission = DATEDIFF('healthcare_dataset'[Date of Admission], 'healthcare_dataset'[Discharge Date], DAY)
Outcome: Allowed analysis of long vs. short hospital stays.

 3. Grouped Patients by Age
Purpose: To categorize patients into age brackets for better analysis.
DAX Formula:
Age Group = SWITCH(
     TRUE(),
    'healthcare_dataset'[Patient Age] >= 80, "80+",
    'healthcare_dataset'[Patient Age] >= 70, "70-79",
    'healthcare_dataset'[Patient Age] >= 60, "60-69",
    'healthcare_dataset'[Patient Age] >= 50, "50-59",
    'healthcare_dataset'[Patient Age] >= 40, "40-49",
    'healthcare_dataset'[Patient Age] >= 30, "30-39",
    'healthcare_dataset'[Patient Age] >= 20, "20-29",
    "10-19")
Outcome: Allowed for demographic segmentation and trend analysis.

4. Randomly Assigned Patient Admission Time
Purpose: To generate random admission times for analysis of peak vs. non-peak hours.
DAX Formula:
Patient Admission Time = TIME(
    RANDBETWEEN(0, 23),  -- Random hour (0-23)
    RANDBETWEEN(0, 59),  -- Random minutes (0-59)
    0
    )
 Outcome: Enabled analysis of patient admissions by time of day.

 5. Classified Admission Time into Peak & Non-Peak Hours
Purpose: To determine when most admissions occur.
DAX Formula: Admission Peak Hours = 
IF(
    HOUR('healthcare_dataset'[Patient Admission Time]) IN {7, 8, 9, 10, 16, 17, 18, 19}, 
    "Peak Hours", 
    "Non-Peak Hours"
)
Outcome: Identified busiest hospital hours for better resource allocation.

6. Created "Insurance Paid Amount" Column
Purpose: To calculate the amount covered by insurance.
DAX Formula:  
 Outcome: Insurance covers 50-90% of the billing amount dynamically.

7. Created "Out-of-Pocket Amount" Column
Purpose: To calculate the amount paid by the patient.
DAX Formula:
Out of Pocket Amount = 'healthcare_dataset'[Billing Amount] - 'healthcare_dataset'[Insurance Paid Amount]

8. Fixed "Bed Occupancy Rate" Calculation
Issue: Initial calculation resulted in extremely high occupancy percentages (5000%+).
DAX Formula: Bed Occupancy Rate = 
VAR TotalOccupiedBeds = DISTINCTCOUNT('healthcare_dataset'[Assigned Bed])
VAR TotalAvailableBeds = DISTINCTCOUNT('healthcare_dataset'[Room Number])
RETURN DIVIDE(TotalOccupiedBeds, TotalAvailableBeds, 0) * 100
Outcome: Corrected occupancy rate to reflect real-time bed utilization.

9. Standardized Patient Names (Proper Case)
Issue: Names were in inconsistent casing.
Power Query Fix: Text.Proper([Name])
Outcome: Converted "jeFfreY WoOd" → "Jeffrey Wood".

10. Created "Claim Status" Column (Random Assignment)
Purpose: To simulate claim approval vs. rejection.
DAX Formula:
Claim Status = 
IF(
    RANDBETWEEN(1, 5) = 1,  -- 20% rejection probability
    "Rejected",
    "Approved"
)
Outcome: Assigned "Rejected" (20%) or "Approved" (80%) dynamically.

11. Created "Claim Rejection Rate" Measure
   Purpose: To analyze claim rejections per insurance provider.
DAX Formula:
Claim Rejection Rate = 
DIVIDE(
    COUNTROWS(FILTER('healthcare_dataset', 'healthcare_dataset'[Claim Status] = "Rejected")),
    COUNTROWS('healthcare_dataset')
) * 100
Outcome: Helped in identifying high-rejection insurers.

**Charts & Visualizations**
1. KPI Cards
•	Total Patients
•	Average Length of Stay
•	Bed Occupancy Rate
•	Emergency Billing Amount
Purpose: To provide a quick summary of key hospital performance metrics.

2. Stacked Column Chart (Claim Rejection Analysis)
X-Axis: Insurance Provider
Y-Axis: Claim Rejection Rate
Legend: Admission Type (Elective, Emergency, Urgent)
Purpose: Showed which insurance providers reject claims the most.

3. Line Chart (Patient Discharge Trend)
X-Axis: Date
Y-Axis: No. of Patients Discharged
Outcome: Identified a drop in patient discharges in recent months.

4. Donut Chart (Patient Admission During Peak Hours)
Categories: Peak Hours vs Non-Peak Hours
Outcome: Showed that most admissions occur during non-peak hours.

5. Bar Chart (Patients by Medical Condition)
X-Axis: Medical Conditions
Y-Axis: Number of Patients
Outcome: Chronic conditions (Diabetes, Hypertension, Arthritis) are the leading causes of hospitalization.

6. Bullet Chart (Billing Analysis)
Primary Value: Insurance Paid Amount
Target Value: Billing Amount
Reference Line: Out-of-Pocket Amount
Outcome: Compared how much insurance vs. patients are paying.

**Enhancements & Future Improvements**

Add Readmission Rate KPI to track how many patients return within 30 days.
Create a Trend Chart for Claim Rejections Over Time.
Fix Missing Data in Recent Discharge Trends.

**Summary**

•	Fixed Bed Occupancy Rate Calculation

•	Created Claim Status & Rejection Rate Columns

•	Transformed Patient Names to Proper Case

•	Visualized Billing Amount vs. Insurance Coverage

•	Analyzed Patient Flow, Discharges, and Claim Rejections

This project showcases advanced Power BI analytics for hospital management, providing key insights for optimizing operations, billing, and patient care.

