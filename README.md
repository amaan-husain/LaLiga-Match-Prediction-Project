#  La Liga Match Outcome Prediction


This is a **Data Science and Machine Learning** project to predict football match results (Win / Lose / Draw). In this project, we use the Random Forest algorithm. We focus on **LaLiga** match data spanning across 4 seasons (21/22 - 24/25). We made use of various **team statistics** (like xG, long pass completion, etc.) and **contextual variables** (like venue, opponent, etc.). This project takes place in 3 stages : data collection, cleaning the data and making the model. The model managed to achieve an accuracy score of 47.7%. 

The data collection part was especially challenging because of the ever increasing protection against web scraping, but the soccerdata package came to the rescue. I learned many lessons, having completed this project, which I will surely apply in all my future projects. **Thank you** for taking the time to check out the project!


---

##  Table of Contents
- [Overview](#overview)
- [Dataset](#dataset)
- [Methodology](#methodology)
- [Results](#results)
- [Repository Structure](#repository-structure)
- [Installation & Usage](#installation--usage)
- [Technologies](#technologies)
- [Future Improvements](#future-improvements)
- [Author](#author)
- [License](#license)


---

## Overview

**Goal**: Predict match outcomes in La Liga using historical team performance data.

**Approach**: Three-stage machine learning pipeline:
1. **Data Collection** 
2. **Data Cleaning** 
3. **Modeling** 

**Key Achievement**: 47.7% accuracy on 3-class prediction (26.7% above baseline), with possession and passing metrics emerging as strongest predictors.

---

## Dataset

- **Source**: FBref.com via `soccerdata` Python package
- **Scope**: La Liga seasons 2021–22 through 2024–25
- **Size**: 3,040 matches
- **Features**: 90+ match-level statistics across five categories:
  - Fixtures (date, venue, opponent, result, etc.)
  - Shooting (shots, xG, conversion rates, etc.)
  - Passing (completion %, distance, progression, etc.)
  - Possession (touches, carries, dribbles, etc.)
  - Goal/Shot Creation (key passes, assists, xAG, etc.)

---

## Methodology

###  Data Collection (`01_Data_Collection.ipynb`)

Scraped match-level statistics using the `soccerdata` package:
- Extracted five datasets : fixtures, shooting, goal and shot creation, passing and possession
- Reset the indices of the datasets, so that we get the "team" column (along with some other columns)
- Exported raw CSVs for reproducibility

###  Data Cleaning & Feature Engineering (`02_Cleaning.ipynb`)

**Preprocessing**:
- Fixed the column names
- Merged five datasets into 1 dataset
- Removed columns with missing values
- Converted data types (datetime, numeric, categorical)

**Feature Engineering**:
- Created **3-match rolling averages** for various metrics
- Encoded categorical variables:
  - Opponent team
  - Home/Away venue
  - Day of week and match hour

**Rationale**: Rolling averages capture recent team form without data leakage, using `closed='left'` to exclude the current match from calculations.

###  Model Development (`03_Model.ipynb`)

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

## Results

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

 The Random Forest model **outperformed the baseline by 26.7%**, showing that team-level rolling averages contain predictive signal despite the randomness in sports outcomes.

---

### Feature Importance
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


## Repository Structure
```
LaLiga-Match-Prediction-Project/
├── 01_Data_Collection.ipynb    # Web scraping and data gathering
├── 02_Cleaning.ipynb      # Preprocessing and feature engineering
├── 03_Model.ipynb  # Model training and evaluation
├── requirements.txt            # Python dependencies
├── .gitignore                  # Git exclusions
├── LICENSE                     # MIT License
└── README.md                   # Project documentation
```

---

## Installation & Usage

### Prerequisites
- **Python 3.11**
- **Jupyter Notebook** or **Google Colab**

### Option 1: Run Locally with Jupyter Notebook (Recommended)

1. **Clone the repository**
```bash
git clone https://github.com/amaan-husain/LaLiga-Match-Prediction-Project.git
cd LaLiga-Match-Prediction-Project
```

2. **Install dependencies**
```bash
pip install --upgrade pip
pip install -r requirements.txt
```

3. **Launch Jupyter Notebook**
```bash
jupyter notebook
```

4. **Run notebooks sequentially**
   - Open and run `01_Data_Collection.ipynb` (takes ~10-15 minutes for web scraping)
   - Then run `02_Cleaning.ipynb` (creates the merged dataset)
   - Finally run `03_Model.ipynb` (trains and evaluates the model)

### Option 2: Run on Google Colab

1. **Upload notebooks to Google Drive** or open directly from GitHub

2. **For `01_Data_Collection.ipynb`**: 
   - Run the first and second cell: `!pip install lxml` & `!pip install soccerdata`
   - Then run the rest of the notebook

3. **Download the generated CSV files** and upload them for the next notebooks

4. **Run notebooks in order**: 01 → 02 → 03

###  Important Notes

- **Data collection takes time**: The first notebook scrapes data from FBref and may take 10-15 minutes
- **CSV files not included**: You must run `01_Data_Collection.ipynb` to generate the raw data files
- **Run in sequence**: Each notebook depends on outputs from the previous one
---

## Technologies

| Category | Tools |
|----------|-------|
| **Language** | Python 3.11 |
| **Data Collection** | `soccerdata` |
| **Data Processing** | `pandas`, `numpy` |
| **Machine Learning** | `scikit-learn` (Random Forest) |
| **Development** | Jupyter Notebooks, Google Colab |

---  


## Future Improvements

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

## Author

**Amaan Husain**  
📧 amaanh247@gmail.com  
🔗 [LinkedIn](https://www.linkedin.com/in/amaan-husain-ab2356118/) • [GitHub](https://github.com/amaan-husain)


---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- Data sourced from [FBref.com](https://fbref.com)
- Built with the [`soccerdata`](https://github.com/probberechts/soccerdata) Python package

### THANK YOU FOR CHECKING THE PROJECT OUT! :smile:
---

