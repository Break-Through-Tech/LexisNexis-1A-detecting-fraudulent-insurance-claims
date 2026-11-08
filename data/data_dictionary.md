# Data Dictionary — LexisNexis Fraud Detection Project

Updated at the end of Milestone 2.

There are now three versions of the data, and which one a notebook should load
depends on what it is doing.

| File | Rows | Columns | Made by | What it is for |
|---|---|---|---|---|
| `fraud_oracle.csv` | 15,420 | 33 | original download | The raw file. Nothing should load this except Task 1 |
| `fraud_oracle_cleaned.csv` | 15,419 | 34 | Milestone 1, Task 2 | Cleaned. One bad row dropped, four car brands corrected, `Age_missing` flag added |
| `fraud_oracle_engineered.csv` | 15,419 | 34 | Milestone 2, Task 1 | **What every modeling notebook loads.** Leaky columns removed, engineered columns added |

Two small files travel with the engineered csv and matter as much as it does:

| File | What it holds |
|---|---|
| `data/model_features.json` | The 24 column names the model is allowed to see. Every modeling notebook reads this instead of writing its own list |
| `data/m2_task6_decision.json` | The locked final model, its settings, and the chosen review capacity, written at the end of Milestone 2 |

The reason `model_features.json` exists is that in early Milestone 2 each
notebook was building its own feature list, so four notebooks were training on
four different sets of columns and their scores were not comparable. One file,
read by everyone, fixes that.

---

## Original columns

Every column in `fraud_oracle.csv`, based on `df.info()`, `df.head()`, and
`.unique()` / `.describe()` checks run during Milestone 1.

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
| Age_missing | number | Added in Milestone 1, Task 2. 1 if the original Age was 0 (meaning never recorded), 0 otherwise | 0 or 1 (319 rows are 1) |

---

## Columns added in Milestone 2

Built by `add_engineered_features()` in Task 1 and written into
`fraud_oracle_engineered.csv`.

| Column Name | Data Type | Built from | What It Means | In the model? |
|---|---|---|---|---|
| ClaimLagMonths | number | Month, MonthClaimed | How many months passed between the accident month and the claim month, wrapping around the end of the year | **Yes** |
| AgeBand | text | Age | The policyholder's age grouped into bands instead of an exact number | No |
| VehicleAgeApprox | number | AgeOfVehicle | A numeric stand-in for the car's age, turning the text buckets back into a number | No |
| NewPolicy | number | Days_Policy_Accident | 1 if the policy had been active 30 days or less when the accident happened, 0 otherwise | No |
| WeekendAccident | number | DayOfWeek | 1 if the accident happened on a Saturday or Sunday, 0 otherwise | No |
| WeekendClaim | number | DayOfWeekClaimed | 1 if the claim was filed on a Saturday or Sunday, 0 otherwise | No |

### Why only one of the six is in the model

`ClaimLagMonths` is the only one that tells the model something it has no other
way to find out. It is built from `Month` and `MonthClaimed`, and both of those
are excluded from the feature set, so without this column the gap between the
accident and the claim is invisible to the model.

The other five are each built from a column the model already has.
`WeekendAccident` comes from `DayOfWeek`, which is already a feature, and
one-hot encoding gives the model a separate Saturday column and a separate
Sunday column, so it can already find a weekend effect on its own if one is
there. The same reasoning covers `WeekendClaim` from `DayOfWeekClaimed`,
`NewPolicy` from `Days_Policy_Accident`, and `AgeBand` from `Age`, where a tree
can split age wherever it likes and does not need the splits picked for it in
advance.

`VehicleAgeApprox` is the one real exception and it is worth writing down.
One-hot encoding turns `AgeOfVehicle` into separate yes/no columns and throws
away the fact that "7 years" is older than "3 years", so a numeric version does
carry information the one-hot version does not. It was not tested before the
model was locked. This is the first thing to try if anyone picks the project
back up, not a change to make now.

The five unused columns stay in the csv rather than being deleted. They are
cheap to carry and removing them would mean rewriting Task 1 for no gain.

---

## Which columns the model actually sees

Settled across the Milestone 1 leakage audit and the Milestone 2 feature tests,
and recorded in `data/model_features.json`.

### The 24 features

AccidentArea, Age, Age_missing, AgentType, AgeOfPolicyHolder, AgeOfVehicle,
BasePolicy, ClaimLagMonths, DayOfWeek, DayOfWeekClaimed, Days_Policy_Accident,
Days_Policy_Claim, Deductible, DriverRating, Fault, Make, MaritalStatus,
NumberOfCars, PastNumberOfClaims, Sex, VehicleCategory, VehiclePrice,
WeekOfMonth, WeekOfMonthClaimed.

Everything else in the engineered csv is left out. That is the 9 columns in the
table below, plus `Year` and `FraudFound_P`, plus the five engineered columns
covered in the section above.

### The 9 columns excluded, and why

| Column | Removed in | Why it is excluded |
|---|---|---|
| Month | Milestone 2 | Fraud nearly disappears for a block of months in every year, and the block moves between years. October 1994 has 1 fraud case, October 1995 has 50. A real season would land in the same months every year, so this is an operational artifact rather than a pattern, and the model learns the wrong lesson from it |
| MonthClaimed | Milestone 2 | Same problem, same reasoning. The information worth keeping from both month columns is captured by `ClaimLagMonths` instead |
| PolicyType | Milestone 2 | It is `VehicleCategory` and `BasePolicy` pasted together, so it is a duplicate of two columns already in the feature set. Removing it made no measurable difference and removed nine redundant one-hot columns |
| PolicyNumber | Milestone 1 | Runs in order by Year (1994 is 1 to 6142, 1995 is 6143 to 11337, 1996 is 11338 to 15420), correlation with Year 0.937, so it encodes time |
| RepNumber | Milestone 1 | Operational identifier for which rep handled the claim. Fraud rates are flat across all 16 reps, so nothing is lost |
| NumberOfSuppliments | Milestone 1 | Supplements accumulate after the claim is filed, so the final count was not known at filing time |
| PoliceReportFiled | Milestone 1 | Timing cannot be confirmed from the data. Dropping costs about 0.001 PR-AUC, which is cheaper than the risk of leaving a leaky column in |
| WitnessPresent | Milestone 1 | Same reasoning |
| AddressChange_Claim | Milestone 1 | Same reasoning |

**Used only for splitting:** Year. Needed to build the train (1994) / validation
(1995) / test (1996) split, never shown to the model as an input.

**Target:** FraudFound_P.

### Two calendar columns we kept, and why

`DayOfWeek` and `WeekOfMonth` survive even though `Month` did not, and the
difference is worth writing down because it is not an inconsistency.

A single training year contains one January, so whatever the model learns about
January is an accident of that one month. The same training year contains
roughly fifty-two Mondays, so a Monday effect has enough repetitions behind it to
be a pattern rather than a coincidence. We tested this rather than assuming it.
Dropping all four calendar columns cost PR-AUC, 0.1556 down to 0.1423, so the two
frequent ones stayed.

---

## What is in the data but not in any model

Three columns are reasonable features that we excluded on a timing argument we
could not confirm from the data alone: `PoliceReportFiled`, `WitnessPresent`,
and `AddressChange_Claim`. If the host company can confirm that these are
recorded when the claim is filed rather than during an investigation, all three
can be added back. This is the single clearest question to put to them.

---

## Notes on data quality

- Age has a minimum value of 0 in the original file, on 320 rows. This is a
  placeholder for "not recorded", not a real age. In the cleaned file those
  values are blank and marked by `Age_missing`.
- Age and AgeOfPolicyHolder disagree on roughly 45% of rows. The data cannot say
  which one is correct, so both are currently in the feature set and one should
  be chosen deliberately.
- Four car brands were misspelled in the original file (Accura, Nisson, Porche,
  Mecedes) and are corrected in the cleaned file. The brand count stayed at 19
  before and after, confirming nothing was merged by accident.
- One row (index 1516) had a "0" for both DayOfWeekClaimed and MonthClaimed. It
  was not a fraud case and was dropped, which is why the cleaned file has 15,419
  rows rather than 15,420.
- Several columns that represent numeric ideas (AgeOfVehicle, AgeOfPolicyHolder,
  Days_Policy_Accident, Days_Policy_Claim, PastNumberOfClaims, NumberOfCars,
  VehiclePrice) are stored as grouped text ranges rather than exact numbers.
  These are handled by one-hot encoding inside the modeling pipeline rather than
  converted in the csv, so the file stays readable.
- `Make` has 19 values and several of them appear on fewer than 1% of claims.
  The modeling pipeline groups the rare ones together with
  `OneHotEncoder(min_frequency=0.01)` rather than giving each a column of its
  own, because a brand with eleven claims in the training year cannot support a
  reliable fraud estimate.

---

## Class balance by year

| Year | Claims | Fraud | Fraud rate | Used as |
|---|---|---|---|---|
| 1994 | 6,141 | 409 | 6.66% | Training |
| 1995 | 5,195 | 301 | 5.79% | Validation |
| 1996 | 4,083 | 213 | 5.22% | Test, sealed until November |

The fraud rate falls every year. That matters for November, because a score
cutoff chosen on 1995 will flag more of 1996 than intended. A cutoff set to flag
the top 20% of 1995 flags 24.3% of 1996. The fix is to set the cutoff by review
capacity, meaning take the highest-scored N claims, rather than by a fixed
probability threshold.
