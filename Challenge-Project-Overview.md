---

> ## Challenge Advisor: Update & Finalize Your Project Overview
>
> > 💡 **These grey text instructions are just for you, the team's Challenge Advisor; please delete them once you have completed the steps below.**
>
> We've pre-populated this Challenge Project Overview page — which is what will be shared with your Break Through Tech student team in August — using the details from your submission form. You should have received an email inviting you to join this repo as a Collaborator, enabling you to add files and make edits.
> 
> In order for your project to be finalized and assigned to a team, please:
> 1. **Review all sections below** and update or expand any content as needed, making sure to address the SME Feedback in the section immediately below. Look for square brackets to find the places below that require additional inputs from you (e.g., "About [Company / Org Name]").
> 2. **Add your dataset** to the [data folder](data) in this repo.
> 3. **Close the Issue assigned to you in this repo** to let us know that you have made your edits and the overview page is ready for final review. You can do this by going to the _Issues_ tab in the top left section of the menu above, add a comment that says "CA review complete", and click the button to Close the Issue. 
>
> If you're unfamiliar with how to edit a page like this in GitHub, check out [this tutorial](https://ubc-lib-geo.github.io/gis-workshop-waml-template/content/handson/edit-readme.html) for a quick overview (start with step 2 and only edit this page), and [this guide](https://ubc-lib-geo.github.io/gis-workshop-waml-template/content/markdown.html) on how to use Markdown to compose text.
>
>
> ❌ Remember that this is a public repo. Do NOT include: Proprietary data, PII, API keys, credentials, or anything confidential.

---

## 📋 BTT Internal Evaluation Notes
*(This section is for BTT staff and CAs only — remove before sharing with students)*

### Technical Vetting
| Check | Status | Notes |
| :--- | :--- | :--- |
| Python Compatibility | 🟢 | The stack relies on scikit-learn, imbalanced-learn (SMOTE), and SHAP, which are fully compatible with standard Google Colab environments. |
| Data Readiness | 🟢 | The provided Kaggle dataset is pre-structured and cleaned, requiring minimal preprocessing, which allows fellows to focus on feature engineering and modeling. |
| Resource Check | 🟢 | The dataset is small (~sub-1GB) and fits comfortably in Colab memory; no GPUs or paid APIs are required. |

### Internal Scores
- **Student Fit Score:** 9/10
- **Technical Depth Score:** 7/10
- **Overall Recommendation:** APPROVE

### Advisor Feedback Draft
This project is a classic 'Goldilocks' problem that aligns perfectly with the BTT curriculum. The focus on business-aligned metrics like PR-AUC and top-decile capture rates is excellent for student professional growth. Technical adjustments: 1) Require a strict temporal or hold-out split rather than K-Fold to prevent leakage, and 2) Shift the focus from Streamlit dashboarding to a comprehensive model-card documentation that justifies the SHAP value findings. Please finalize the feature list to ensure no future-looking data is included.

---

# Detecting Fraudulent Insurance Claims: A Risk-Scoring Model for Faster, Fairer Claims Triage

**Company / Org:** LexisNexis Risk Solutions Group  
**Challenge Advisor:** Stephanie Le, ledaquynhnhi@gmail.com  
**Program:** Break Through Tech AI Studio - Fall 2026  

---

## 🏢 About LexisNexis Risk Solutions Group
LexisNexis Risk Solutions Group is a global leader in providing data, analytics, and technology solutions to help organizations manage risk and improve decision-making. The team objectives center on leveraging predictive modeling to enhance operational efficiency within the insurance sector, specifically by optimizing claims processing workflows.

---

## 🎯 The Challenge
### Project Summary
This project tasks the team with developing a robust risk-scoring model to classify auto insurance claims based on their likelihood of fraud. By utilizing structured historical claims data and supervised machine learning techniques, students will create an interpretable system that enables the company to prioritize high-risk investigations while expediting the processing of legitimate claims.

### Success Criteria
Precision, recall, F1, PR-AUC on the minority (fraud) class, plus a business-framed metric like '% of fraud cases captured if investigators review only the top 10–20% highest-scored claims'. Successful outcome by December: a working, interpretable model that clearly beats a naive baseline on recall/precision trade-off, with a short explanation of which features drive risk.

### Project Milestones
Use these milestones to guide your work. Your team will create a GitHub Projects board to track tasks within each milestone.
| Month | Milestone | Key Activities |
|-------|-----------|----------------|
| **September** | Data Exploration & Preprocessing | Conduct Exploratory Data Analysis (EDA), finalize the data dictionary, clean the raw dataset, and establish a baseline predictive model. |
| **October** | Feature Engineering & Baseline Modeling | Implement feature extraction, apply SMOTE to mitigate class imbalance, and train 2–3 different classifier algorithms for comparison. |
| **November** | Model Optimization & Evaluation | Fine-tune model hyperparameters, validate against business-aligned metrics, and integrate SHAP values for model interpretability. |
| **December** | Insights, Deliverables & Presentation | Finalize the documentation, polish the GitHub repository, and prepare a presentation summarizing business recommendations and model performance. |

> **Note for the team:** Please create a GitHub Projects board in this repository to break these milestones into weekly tasks. Go to the **Projects** tab → **New project** → Choose **Board** → Add columns for each month.

---

## 📊 Dataset
**Name and Source:** Vehicle Claim Fraud Detection (Kaggle): https://www.kaggle.com/datasets/shivamb/vehicle-claim-fraud-detection  
**Format:** CSV  
**Size:** under 1gb  
**Location:** Accessible via Kaggle or provided local project directory  

### Key Details
- Structured historical auto insurance claims data including policy details, claimant demographics, and incident characteristics. Publicly available via Kaggle: https://www.kaggle.com/datasets/shivamb/vehicle-claim-fraud-detection
- Ensure all categorical variables are properly encoded and that the team performs rigorous checks for data leakage, particularly ensuring that no variables contain "post-incident" information not available at the time of claim filing.

---

## 🛠️ Suggested Approach
**ML Problem Type:** Classification  
**Recommended Libraries:**
- logistic regression
- random forest
- gradient boosting
- SMOTE
- SHAP
- Streamlit
**Evaluation Metrics:** Precision, Recall, F1-Score, PR-AUC, and the Top-Decile Fraud Capture Rate.

---

## 📚 Resources to Get Started
The following resources will help your team understand the problem space and potential technical approaches for this project:
**Background Reading:**
- Industry standards for insurance fraud detection and the role of interpretability in risk-scoring models.
**Technical Tutorials:**
- Scikit-learn documentation on handling imbalanced datasets and the official SHAP GitHub library tutorials.
**Code Examples:**
- Reference standard implementations of Gradient Boosting and Random Forest classifiers within the provided Kaggle community notebooks.

---

## 🤝 How We'll Work Together
**Check-ins:** During our biweekly 60-min AI Studio Lab Section meeting block (2nd and 4th week of every month)  
**Communication:** Email and scheduled Slack channels  
**Response time:** 24-48 hours during the work week  
**Recommended Tools:**
- **Coding:** Google Colab Free Tier  
- **Collaboration:** GitHub, Notion  
- **Virtual Meetings:** Zoom, Google Meet  

---

## 🚀 Getting Started
1. **Review this overview document** and note any questions for our first meeting.
2. **Begin reviewing the dataset** using the link provided in the Dataset section.
3. **Read the GitHub Projects documentation** [here](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects).

I'm excited to work with you!

---

## ❓ Questions?
Please bring any questions to our first meeting during the week of August 24th (Break Through Tech's Bridge to Studio - Session B).
