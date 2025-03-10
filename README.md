# HOSPITAL-PERFORMANCE-DASHBOARD
 
🏥 Hospital Performance Dashboard – Project Documentation

Author: Deepeka Gurunathan

Project Overview: This project focuses on analyzing hospital performance using Power BI, incorporating patient trends, billing insights, bed occupancy rates, and claim rejection analysis.

**Data Transformations & Column Additions**

1. Created "Insurance Paid Amount" Column
Purpose: To calculate the amount covered by insurance.
DAX Formula:  
 Outcome: Insurance covers 50-90% of the billing amount dynamically.

2. Created "Out-of-Pocket Amount" Column
Purpose: To calculate the amount paid by the patient.
DAX Formula:
Out of Pocket Amount = 'healthcare_dataset'[Billing Amount] - 'healthcare_dataset'[Insurance Paid Amount]

3. Fixed "Bed Occupancy Rate" Calculation
Issue: Initial calculation resulted in extremely high occupancy percentages (5000%+).
DAX Formula: Bed Occupancy Rate = 
VAR TotalOccupiedBeds = DISTINCTCOUNT('healthcare_dataset'[Assigned Bed])
VAR TotalAvailableBeds = DISTINCTCOUNT('healthcare_dataset'[Room Number])
RETURN DIVIDE(TotalOccupiedBeds, TotalAvailableBeds, 0) * 100
Outcome: Corrected occupancy rate to reflect real-time bed utilization.

4. Standardized Patient Names (Proper Case)
Issue: Names were in inconsistent casing.
Power Query Fix: Text.Proper([Name])
Outcome: Converted "jeFfreY WoOd" → "Jeffrey Wood".

5. Created "Claim Status" Column (Random Assignment)
Purpose: To simulate claim approval vs. rejection.
DAX Formula:
Claim Status = 
IF(
    RANDBETWEEN(1, 5) = 1,  -- 20% rejection probability
    "Rejected",
    "Approved"
)
Outcome: Assigned "Rejected" (20%) or "Approved" (80%) dynamically.

6. Created "Claim Rejection Rate" Measure
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
Outcome: Chronic conditions (Diabetes, Hypertension, Arthritis) are leading causes of hospitalization.

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

