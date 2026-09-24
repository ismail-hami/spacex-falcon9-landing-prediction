
# 🚀 SpaceX Falcon 9 Landing Prediction

End-to-end data science project that predicts whether a **Falcon 9 first stage will land successfully** — from data collection and SQL/visual analysis to an interactive dashboard and machine learning models.

> SpaceX advertises Falcon 9 launches at about **$62 million**, while other providers charge upward of **$165 million**. Most of the savings come from reusing the first stage. If we can predict whether the first stage will land, we can estimate the cost of a launch — useful for a company that wants to bid against SpaceX.

---

## 📌 Project Overview

| | |
|---|---|
| **Goal** | Predict first-stage landing success (binary classification: landed / did not land) |
| **Data** | 90 Falcon 9 launches, June 2010 – November 2020 |
| **Overall success rate** | 66.7% (60 of 90 landings) |
| **Approach** | API + web scraping → wrangling → SQL & visual EDA → interactive map → dashboard → ML |
| **Best test accuracy** | 83.3% (all four models tied — see [Results](#-results)) |

---

## 🗂️ Repository Structure

```
spacex-falcon9-landing-prediction/
├── README.md
├── requirements.txt
├── notebooks/
│   ├── 01_data_collection_api.ipynb
│   ├── 02_web_scraping.ipynb
│   ├── 03_data_wrangling.ipynb
│   ├── 04_eda_sql.ipynb
│   ├── 05_eda_visualization.ipynb
│   ├── 06_interactive_map_folium.ipynb
│   └── 07_machine_learning_prediction.ipynb
├── dashboard/
│   ├── spacex-dash-app.py
│   └── spacex_launch_dash.csv
└── data/
    ├── dataset_part_2.csv
    ├── dataset_part_3.csv
    └── spacex_web_scraped.csv
```

---

## 🔬 Workflow

| # | Notebook | What it does |
|---|---|---|
| 1 | **Data collection (API)** | Requests launch data from the SpaceX REST API (v4), resolves rocket, launchpad, payload and core IDs with helper functions, keeps Falcon 9 launches only, and handles missing payload mass. |
| 2 | **Web scraping** | Scrapes the Falcon 9 / Falcon Heavy launch records table from Wikipedia with BeautifulSoup into a DataFrame (121 rows × 11 columns). |
| 3 | **Data wrangling** | Counts launches per site, orbit types and mission outcomes, then builds the binary `Class` label (1 = landed, 0 = did not land). |
| 4 | **EDA with SQL** | Loads the data into SQLite and answers 10 analysis questions with SQL queries (launch sites, payload mass, booster versions, outcomes). |
| 5 | **EDA with visualization** | Six visual analyses (flight number, payload mass, orbit and yearly success trend) plus feature engineering: one-hot encoding and casting to `float64`. |
| 6 | **Interactive map (Folium)** | Marks launch sites and launch outcomes on an interactive map and measures distances from sites to the coastline, railway, highway and nearest city. |
| 7 | **Machine learning** | Standardizes features, splits the data, tunes four classifiers with cross-validated grid search and compares them. |

---

## 📊 Interactive Dashboard

A Plotly Dash app for exploring launch records:

- **Site dropdown** – choose *All Sites* or a single launch site
- **Pie chart** – successful launches by site (or success vs. failure for one site)
- **Payload slider** – filter launches by payload mass (0 – 10,000 kg)
- **Scatter chart** – payload mass vs. outcome, coloured by booster version category

Run it locally:

```bash
cd dashboard
python spacex-dash-app.py
```

Then open <http://127.0.0.1:8050> in your browser.

---

## 🤖 Machine Learning

**Pipeline**

1. Load the engineered feature matrix (`X`) and the `Class` label (`Y`)
2. Standardize all features with `StandardScaler`
3. Split into train / test sets — 80 / 20, `random_state=2`
4. Tune each model with `GridSearchCV` (10-fold cross-validation)
5. Evaluate on the held-out test set and plot confusion matrices

**Models:** Logistic Regression · Support Vector Machine · Decision Tree · K-Nearest Neighbors

### 🏆 Results

<!-- TODO: replace with the numbers printed by your own run of notebook 07 -->

| Model | CV accuracy | Test accuracy |
|---|---|---|
| Logistic Regression | 0.846 | 0.833 |
| SVM | 0.848 | 0.833 |
| Decision Tree | 0.875 | 0.833 |
| KNN | 0.848 | 0.833 |

**Takeaways**

- All four models reach the same **83.3% test accuracy** (15 of 18 correct) and make the same mistakes: they never miss a real landing, but wrongly predict 3 non-landings as landings.
- The Decision Tree has the best cross-validation score, but the test set has only 18 launches — one prediction changes accuracy by about 5.6 points — so the models can't be reliably ranked.
- Standardizing the features matters: without it, SVM and KNN are slow and unreliable because `PayloadMass` is on a much larger scale than the other features.

---

## ⚙️ Installation

```bash
git clone https://github.com/<your-username>/spacex-falcon9-landing-prediction.git
cd spacex-falcon9-landing-prediction

python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

pip install -r requirements.txt
jupyter lab
```

The notebooks download their data from public URLs, so no extra setup is needed. Local copies of the CSV files are in `data/`.

---

## 🛠️ Tech Stack

**Python** · pandas · NumPy · scikit-learn · Matplotlib · Seaborn · Plotly · Dash · Folium · BeautifulSoup · Requests · SQLite · Jupyter

---

## ⚠️ Notes & Limitations

- **Small dataset** – only 90 launches, so results have high variance.
- **API availability** – notebook 01 depends on the public SpaceX API (`api.spacexdata.com`), which was returning server errors when the notebook was last run. Its outputs are kept as-is; the cleaned datasets in `data/` let you continue from notebook 03 onward.
- **Class imbalance** – 60 landings vs. 30 non-landings, so accuracy alone can hide weak performance on the minority class (see the confusion matrices in notebook 07).

---

## 🙏 Acknowledgements

This project was built as the capstone of the **IBM Data Science Professional Certificate** (Coursera). Lab templates and datasets are provided by IBM Skills Network; launch data comes from the SpaceX API and Wikipedia.

---

## 👤 Author

**Xyatoo**

- GitHub: [@your-username](https://github.com/your-username)
- LinkedIn: [your-profile](https://www.linkedin.com/in/your-profile)
