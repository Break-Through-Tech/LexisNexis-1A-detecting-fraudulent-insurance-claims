# Model Selection

## Review capacity

**Investigators will review the top 20% of claims.** Results are reported at this cutoff, not as recall at a 0.5 threshold.

- Source: Stephanie's guidance (top 20%)
- Note: the Task 6 decision file (`data/m2_task6_decision.json`) recommended 10%; we are using 20% per Stephanie

## Model

Random forest (`RandomForestClassifier`), fully grown, with class weights:

- `n_estimators`: 500
- `class_weight`: "balanced"
- `max_depth`: None (no depth limit)
- `min_samples_leaf`: 1 (no leaf limit)

## Features (24 columns)

Source: `data/model_features.json`

1. WeekOfMonth
2. DayOfWeek
3. WeekOfMonthClaimed
4. DayOfWeekClaimed
5. Make
6. VehicleCategory
7. VehiclePrice
8. AgeOfVehicle
9. NumberOfCars
10. AccidentArea
11. Fault
12. Sex
13. MaritalStatus
14. Age
15. Age_missing
16. AgeOfPolicyHolder
17. BasePolicy
18. Deductible
19. DriverRating
20. AgentType
21. Days_Policy_Accident
22. Days_Policy_Claim
23. PastNumberOfClaims
24. ClaimLagMonths
