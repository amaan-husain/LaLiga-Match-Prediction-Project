# ⚽ La Liga Match Outcome Prediction

### Predicting football match results using machine learning and team performance statistics

This project applies **data science and machine learning** to predict match outcomes (Win/Draw/Loss) in Spain’s *La Liga* using multi-season team statistics from FBref.  
It demonstrates end-to-end proficiency in **data collection, cleaning, feature engineering, and predictive modeling** — skills relevant to both **computer science** and **data analytics research**.

---
---

## 📋 Table of Contents
- [Overview](#-overview)
- [Dataset](#-dataset)
- [Methodology](#-methodology)
- [Results](#-results)
- [Key Findings](#-key-findings)
- [Repository Structure](#-repository-structure)
- [Installation & Usage](#-installation--usage)
- [Technologies](#-technologies)
- [Future Improvements](#-future-improvements)
- [Author](#-author)
- [License](#-license)


---

## 📘 Overview

**Goal**: Predict match outcomes in La Liga using historical team performance data.

**Approach**: Three-stage machine learning pipeline:
1. **Data Collection** – Multi-season match statistics via `soccerdata` package
2. **Data Cleaning** – Feature engineering with rolling averages and encoding
3. **Modeling** – Random Forest classification with temporal validation

**Key Achievement**: 47.7% accuracy on 3-class prediction (27% above baseline), with possession and passing metrics emerging as strongest predictors.

---

## 📊 Dataset

- **Source**: FBref.com via `soccerdata` Python package
- **Scope**: La Liga seasons 2021–22 through 2024–25
- **Size**: 3,040 matches
- **Features**: 90+ match-level statistics across five categories:
  - Fixtures (date, venue, opponent, result)
  - Shooting (shots, xG, conversion rates)
  - Passing (completion %, distance, progression)
  - Possession (touches, carries, dribbles)
  - Goal/Shot Creation (key passes, assists, xAG)

---

## 🧠 Methodology

### 1️⃣ Data Collection (`01_data_collection.ipynb`)

Scraped match-level statistics using the `soccerdata` package:
- Extracted five separate datasets per match
- Flattened multi-index structures for downstream processing
- Exported raw CSVs for reproducibility

### 2️⃣ Data Cleaning & Feature Engineering (`02_data_cleaning.ipynb`)

**Preprocessing**:
- Standardized multi-level column headers
- Merged five datasets into unified match records
- Removed columns with >90% missing values
- Converted data types (datetime, numeric, categorical)

**Feature Engineering**:
- Created **3-match rolling averages** for various metrics
- Encoded categorical variables:
  - Opponent team
  - Home/Away venue
  - Day of week and match hour

**Rationale**: Rolling averages capture recent team form without data leakage, using `closed='left'` to exclude the current match from calculations.

### 3️⃣ Model Development (`03_model_development.ipynb`)

**Algorithm**: Random Forest Classifier
- 100 trees, max depth 15
- Minimum samples per split: 10
- All CPU cores utilized (`n_jobs=-1`)

**Train/Test Split**: Temporal validation
- Training: All matches before 2025
- Testing: 2025 matches only (398 matches)
- Simulates real-world forecasting scenario

**Target Variable**: 
- Win (2), Draw (1), Loss (0)

**Evaluation Metrics**:
- Accuracy, Precision, Recall, F1-score
- Confusion Matrix
- Feature Importance Analysis


---

## 📊 Results

| Metric | Value |
|:-------|:------|
| **Accuracy** | **47.7%** |
| **Baseline (Most Frequent Class)** | **37.7%** |
| **Improvement over Baseline** | **+26.7%** |

**Class-wise performance:**  
| Class | Precision | Recall | F1-score | Support |
|:------|:-----------|:--------|:-----------|:---------|
| Loss | 0.46 | 0.62 | 0.53 | 150 |
| Draw | 0.35 | 0.07 | 0.12 | 98 |
| Win | 0.51 | 0.60 | 0.55 | 150 |

**Macro Avg:** Precision 0.44 • Recall 0.43 • F1 0.40  
**Weighted Avg:** Precision 0.45 • Recall 0.48 • F1 0.44  

✅ The Random Forest model **outperformed the baseline by 26.7%**, showing that team-level rolling averages contain predictive signal despite the randomness in sports outcomes.

---

### 🔍 Feature Importance
**Top 10 Most Influential Features:**
1. Venue code  
2. Opponent code  
3. Long pass completion % (rolling average)  
4. Total pass completion % (rolling average)  
5. Short pass completion (rolling average)  
6. Carries total distance (rolling average)  
7. Short passes attempted (rolling average)  
8. Touches in attacking third (rolling average)  
9. Short pass completion % (rolling average)  
10. Take-ons tackled % (rolling average)  

---
### Insights

 **Recent form matters**: Rolling averages of team statistics are highly predictive  
 **Draws are unpredictable**: Low recall (7%) reflects inherent randomness in tight matches  
 **xG outperforms actual goals**: Expected goals capture quality of chances better than raw goal counts  

---


## 📁 Repository Structure
```
LaLiga-Match-Prediction-Project/
├── 01_data_collection.ipynb    # Web scraping and data gathering
├── 02_data_cleaning.ipynb      # Preprocessing and feature engineering
├── 03_model_development.ipynb  # Model training and evaluation
├── requirements.txt            # Python dependencies
├── .gitignore                  # Git exclusions
├── LICENSE                     # MIT License
└── README.md                   # Project documentation
```

---

## 🚀 Installation & Usage

### Prerequisites
- Python 3.11
- Jupyter Notebook or Google Colab

### Setup

1. **Clone the repository**
```bash
git clone https://github.com/amaan-husain/LaLiga-Match-Prediction-Project.git
cd LaLiga-Match-Prediction-Project
```

2. **Install dependencies**
```bash
pip install -r requirements.txt
```

3. **Run notebooks in order**
```bash
jupyter notebook 01_data_collection.ipynb
# Follow through 02 and 03 sequentially
```

**Note**: Data collection may take 10-15 minutes due to web scraping rate limits.

---

## 🛠️ Technologies

| Category | Tools |
|----------|-------|
| **Language** | Python 3.8+ |
| **Data Collection** | `soccerdata` |
| **Data Processing** | `pandas`, `numpy` |
| **Machine Learning** | `scikit-learn` (Random Forest) |
| **Development** | Jupyter Notebooks, Google Colab |

---  
---

## 🔮 Future Improvements

### Short-term
- [ ] Address class imbalance using SMOTE or class weights
- [ ] Hyperparameter tuning via GridSearchCV
- [ ] Add cross-validation with time-series splits

### Long-term
- [ ] Incorporate player-level statistics (lineups, injuries)
- [ ] Test ensemble methods (XGBoost, LightGBM, stacking)
- [ ] Add external factors (weather, referee, rest days)
- [ ] Build interactive dashboard with Plotly/Streamlit
- [ ] Expand to other leagues (Premier League, Serie A, Bundesliga)

---

## 👤 Author

**Amaan Husain**  
📧 amaanh247@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/amaan-husain-ab2356118/) • [GitHub](https://github.com/amaan-husain)


---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- Data sourced from [FBref.com](https://fbref.com)
- Built with the [`soccerdata`](https://github.com/probberechts/soccerdata) Python package
- Inspired by sports analytics research in predictive modeling

---
