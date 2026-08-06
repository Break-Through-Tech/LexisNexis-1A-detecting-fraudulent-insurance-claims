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
In this project, you will use structured historical auto insurance claims data (policy details, claimant demographics, and incident characteristics) and supervised classification techniques (like logistic regression, random forest, gradient boosting) to build a model that scores claims by likelihood of fraud. This will help our company address the challenge of prioritizing limited fraud-investigation resources toward the claims most likely to be fraudulent, reducing losses from fraudulent payouts while minimizing review delays for legitimate claimants

### Success Criteria
Given fraud is a rare-class problem, accuracy alone is misleading. Use precision, recall, F1, and PR-AUC on the minority (fraud) class, plus a business-framed metric like "% of fraud cases captured if investigators review only the top 10–20% highest-scored claims." A successful outcome by December: a working, interpretable model that clearly beats a naive baseline on recall/precision trade-off, with a short explanation of which features drive risk.

### Stretch Goals
cost-sensitive learning (weighting false negatives more heavily, since missed fraud is costlier than false alarms), a lightweight Streamlit demo for exploring flagged claims, stacked/ensemble models, unsupervised anomaly detection as a complementary check, enriching with a second public dataset.

### Project Milestones
Use these milestones to guide your work. Your team will create a GitHub Projects board to track tasks within each milestone.

| Month | Milestone | Key Activities |
|---|---|---|
| September | [Title] | Data exploration & cleaning; EDA on fraud vs. non-fraud patterns, review the provided data dictionary, establish a baseline model (logistic regression) and baseline metrics. |
| October | [Title] | Feature engineering; address class imbalance (class weighting, SMOTE), train/tune 2–3 model types (random forest, gradient boosting) and compare. |
| November | [Title] | Finalize best model, evaluate with business-relevant metrics, add interpretability (SHAP or feature importance); polish GitHub repo, write-up, and final presentation. |

> **Note for the team:** Please create a GitHub Projects board in this repository to break these milestones into weekly tasks. Go to the **Projects** tab → **New project** → Choose **Board** → Add columns for each month.

---

## 📊 Dataset
**Name and Source:** Vehicle Insurance Claim Fraud Detection (Kaggle): 
**Format:** CSV  
**Size:** under 1gb  
**Location:** https://www.kaggle.com/datasets/shivamb/vehicle-claim-fraud-detection

### Key Details
- [Brief description of what's in the data]
- [Any known limitations or preprocessing needed]
- [Link to data dictionary or documentation, if available]

---

## 🛠️ Suggested Approach

**ML Problem Type:** Classification  

**Recommended Libraries:**
- [e.g., pandas, scikit-learn, TensorFlow, Hugging Face]

**Evaluation Metrics:**
- [e.g., Accuracy, Precision/Recall, RMSE, BLEU score]
  
---

## 📚 Resources to Get Started

The following resources will help your team understand the problem space and potential technical approaches for this project:

**Background Reading:**
- [e.g., Link to an article or blog post about the problem domain]
- [e.g., Link to an industry report or case study]

**Technical Tutorials:**
- [e.g., Link to a free tutorial on the ML technique(s) involved]
- [e.g., Link to documentation for a key library or tool]

**Code Examples:**
- [e.g., Link to a relevant GitHub repo]
- [e.g., Link to a sample implementation or starter code]

**Other:**
- [Links to any additional resources — e.g., papers, videos, podcasts, etc.]

*Feel free to explore beyond these, and share anything interesting you find with me!*

---

## 🤝 How We'll Work Together

**Official check-ins:** During our biweekly 45-minute AI Studio Lab Section meeting block (2nd and 4th week of every month)

 **Other ways to reach out to me with questions:** 
* [e.g., Your team's channel within Break Through Tech’s Discord space]
* [e.g., Email; please copy your teammates and AI Studio Coach]
* [e.g., Request a team check-in on Zoom]
* [Note: I will aim to respond within 48 hours. Please reach out to your AI Studio Coach with urgent questions.]

> 💡 **Challenge Advisor: Please update the above based on your availability and preference. If you are not able to answer questions or meet with fellows outside of the biweekly Lab Section check-ins, simply write in "N/A (only available during the official check-in times)"**

**Recommended free coding / collaboration tools**
* […]
* […]

---

## 🚀 Getting Started

1. **Review this overview document** and note any questions for our first meeting
2. **Begin reviewing the dataset** using the link above
3. **Read the GitHub Projects documentation** [here](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects)

I’m excited to work with you!

---

## ❓ Questions?

Please bring any questions to our first meeting during the week of August 24th (Break Through Tech’s Bridge to Studio - Session C). 
