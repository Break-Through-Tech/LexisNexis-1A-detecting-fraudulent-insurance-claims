# Data Dictionary — LexisNexis Fraud Detection Project

This covers every column in `fraud_oracle.csv` (15,420 rows, 33 columns), based on `df.info()`, `df.head()`, and `.unique()` / `.describe()` checks run during Task 1, plus the one column added in Task 2.

The cleaned file produced by Task 2, `fraud_oracle_cleaned.csv`, has 15,419 rows and 34 columns: one bad row dropped, four car brands corrected, and a new `Age_missing` flag column.

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
| Age | number | Age of the person on the policy. In the original file 320 rows have an Age of 0, which is a placeholder for "not recorded" rather than a real age. In the cleaned file those are blank and marked by `Age_missing` | 16 to 80 (0 in the original file means not recorded) |
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
| Year | number | Year the claim is from. Used only to build the train/validation/test split, never given to the model as an input | 1994, 1995, 1996 |
| BasePolicy | text | The base type of coverage for the policy | Liability, Collision, All Perils |
| Age_missing | number | **Only in `fraud_oracle_cleaned.csv`.** Added in Task 2. 1 if the original Age was 0 (meaning never recorded), 0 otherwise | 0 or 1 (319 rows are 1) |

## How Each Column Is Used in Modeling

Settled in the Task 3 leakage audit and confirmed in Task 5. Nothing is deleted from the file; these are just the lists the modeling notebook uses.

**26 features** (what the model actually learns from): Month, WeekOfMonth, DayOfWeek, Make, AccidentArea, DayOfWeekClaimed, MonthClaimed, WeekOfMonthClaimed, Sex, MaritalStatus, Age, PolicyType, VehicleCategory, VehiclePrice, Deductible, Days_Policy_Accident, Days_Policy_Claim, PastNumberOfClaims, AgeOfVehicle, AgeOfPolicyHolder, AgentType, NumberOfCars, BasePolicy, Age_missing, Fault, DriverRating.

**6 columns dropped, never given to the model:**

| Column | Why it is excluded |
|---|---|
| PolicyNumber | Runs in order by Year (1994 is 1 to 6142, 1995 is 6143 to 11337, 1996 is 11338 to 15420), correlation with Year 0.937, so it encodes time |
| RepNumber | Operational identifier for which rep handled the claim. Fraud rates are flat across all 16 reps, so nothing is lost |
| NumberOfSuppliments | Supplements accumulate after the claim is filed, so the final count was not known at filing time |
| PoliceReportFiled | Timing cannot be confirmed from the data. Dropping costs about 0.001 PR-AUC, which is cheaper than the risk of leaving a leaky column in |
| WitnessPresent | Same reasoning |
| AddressChange_Claim | Same reasoning |

**1 column used only for splitting:** Year. Needed to build the train (1994) / validation (1995) / test (1996) split, never shown to the model as an input.

**1 target column:** FraudFound_P.

26 + 6 + 1 + 1 = 34, so every column in the cleaned file is accounted for.

If the host company can confirm that PoliceReportFiled, WitnessPresent and AddressChange_Claim are recorded at intake rather than during an investigation, all three can be added back to the feature set.

## Notes on Data Quality

- Age has a minimum value of 0 in the original file, on 320 rows. This is a placeholder for "not recorded", not a real age. In the cleaned file those values are blank and marked by `Age_missing`.
- Age and AgeOfPolicyHolder disagree on roughly 45% of rows. The data cannot say which one is correct, so both are currently in the feature set and one should be chosen deliberately.
- Four car brands were misspelled in the original file (Accura, Nisson, Porche, Mecedes) and are corrected in the cleaned file. The brand count stayed at 19 before and after, confirming nothing was merged by accident.
- One row (index 1516) had a "0" for both DayOfWeekClaimed and MonthClaimed. It was not a fraud case and was dropped, which is why the cleaned file has 15,419 rows rather than 15,420.
- Several columns that represent numeric ideas (AgeOfVehicle, AgeOfPolicyHolder, Days_Policy_Accident, Days_Policy_Claim, PastNumberOfClaims, NumberOfSuppliments, NumberOfCars, VehiclePrice) are stored as grouped text ranges rather than exact numbers, and will need to be converted for modeling.
