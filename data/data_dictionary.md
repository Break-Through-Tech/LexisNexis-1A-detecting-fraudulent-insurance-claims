# Data Dictionary — LexisNexis Fraud Detection Project

This covers every column in `fraud_oracle.csv` (15,420 rows, 33 columns), based on
 `df.info()`, `df.head()`, and `.unique()` / `.describe()` checks run during Task 1.

| Column Name | Data Type | What It Means | Example / Possible Values |
|---|---|---|---|
| Month | text | Month the accident happened | Dec, Jan, Oct, Jun |
| WeekOfMonth | number | Which week of that month the accident happened in (1 to 5) | 5, 3, 2 |
| DayOfWeek | text | Day of the week the accident happened | Wednesday, Friday, Saturday |
| Make | text | Brand of the car | Honda, Toyota, Pontiac, and others |
| AccidentArea | text | Whether the accident happened in a city or countryside area | Urban, Rural |
| DayOfWeekClaimed | text | Day of the week the claim was actually filed | Tuesday, Monday, Thursday |
| MonthClaimed | text | Month the claim was filed (can be after the accident month) | Jan, Nov, Jul |
| WeekOfMonthClaimed | number | Which week of that month the claim was filed in | 1, 4, 2 |
| Sex | text | Sex of the person on the policy | Female, Male |
| MaritalStatus | text | Marital status of the person on the policy | Single, Married |
| Age | number | Age of the person on the policy. Ranges from 0 to 80 (mean about 40); a value of 0 looks like a placeholder for missing/unknown rather than a real age, worth double checking during cleanup | 0 to 80 |
| Fault | text | Who was found at fault for the accident | Policy Holder, Third Party |
| PolicyType | text | Combo of car category and what the policy covers | Sport - Liability, Sedan - Collision, Utility - All Perils, and other combinations of Sport/Sedan/Utility with Liability/Collision/All Perils |
| VehicleCategory | text | General category of the car | Sport, Utility, Sedan |
| VehiclePrice | text | Price range bucket of the car, not an exact number | less than 20000, 20000 to 29000, 30000 to 39000, 40000 to 59000, 60000 to 69000, more than 69000 |
| FraudFound_P | number | Target column. Whether the claim was actually fraud or not | 0 = not fraud, 1 = fraud |
| PolicyNumber | number | ID number for the policy, just an identifier | unique number per row |
| RepNumber | number | ID number for the claims rep or agent handling the case | 1 to 16 |
| Deductible | number | Dollar amount the policyholder pays before insurance kicks in | 300 to 700 |
| DriverRating | number | A numeric risk rating assigned to the driver | 1 to 4 |
| Days_Policy_Accident | text | How long the policy had been active before the accident happened, grouped into buckets | none, 1 to 7, 8 to 15, 15 to 30, more than 30 |
| Days_Policy_Claim | text | How long between the policy starting and the claim being filed, grouped into buckets | none, 8 to 15, 15 to 30, more than 30 |
| PastNumberOfClaims | text | How many claims this person filed before this one, grouped into buckets | none, 1, 2 to 4, more than 4 |
| AgeOfVehicle | text | Age of the car, grouped into buckets instead of an exact number | 3 years, 5 years, 6 years, 7 years, more than 7 |
| AgeOfPolicyHolder | text | Age of the policyholder, grouped into ranges instead of an exact number | 26 to 30, 31 to 35, 41 to 50, 51 to 65 |
| PoliceReportFiled | text | Whether a police report was filed for the accident | No, Yes |
| WitnessPresent | text | Whether a witness was present at the accident | No, Yes |
| AgentType | text | Whether the insurance agent is external or internal to the company | External |
| NumberOfSuppliments | text | How many extra supporting documents were filed with the claim, grouped into buckets | none, more than 5 |
| AddressChange_Claim | text | How recently the policyholder changed their address before filing the claim | 1 year, no change |
| NumberOfCars | text | How many cars are on the policy, grouped into buckets | 1 vehicle, 3 to 4 |
| Year | number | Year the record is from | 1994 (dataset overall covers 1994 to 1996) |
| BasePolicy | text | The base type of coverage for the policy | Liability, Collision |

# Notes on Potential Leakage
These columns might only have real values filled in after someone already started investigating the claim for fraud, not from when the claim was first submitted. So we need to double check these before using them to build the model, since they could basically be giving the model the answer early:

- PoliceReportFiled
- WitnessPresent
- NumberOfSuppliments
- AddressChange_Claim

##Notes on Data Quality

Age has some values showing up as 0, which doesn't make sense for a real person's age, so this is probably just a placeholder for when the age wasn't recorded.
A few columns are really just numbers, but they're stored as text ranges instead (like "3 years" or "more than 5" instead of an actual number). These will need to get converted into real numbers before we can use them in the model: AgeOfVehicle, AgeOfPolicyHolder, Days_Policy_Accident, Days_Policy_Claim, PastNumberOfClaims, NumberOfSuppliments, NumberOfCars, VehiclePrice.