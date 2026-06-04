# Disaster-risk-app

📌 Overview
This project is a Streamlit-based web application that uses a Logistic Regression model trained on EM-DAT panel data (2001–2024) to predict whether a given country in Asia will experience a High or Low natural disaster occurrence in a given year — and forecasts the risk for the following year.
The system is designed with interpretability as a first-order requirement, making it useful not only for prediction but also for understanding what drives disaster risk at a country level.

🖥️ Application Features
The app is organized into five tabs:
🔮 Predict Risk

Select any Asian country and year from the dataset
See the current-year risk probability and next-year forecast side-by-side
View a risk label (Low / Moderate / High / Very High) with colour-coded badges
Understand the top 6 risk drivers — features pushing the model toward High or Low Risk
Explore the country's historical event count profile with the selected year highlighted

🏆 Country Rankings

Cross-country risk comparison for any selected year
Ranked table of all countries by predicted risk probability with gradient highlighting
Horizontal bar chart of the Top 15 riskiest countries
Interactive choropleth map of Asia showing risk levels by country

📈 Trend Analysis

Historical trend visualizations across the dataset
Multi-country and multi-year comparisons powered by Plotly

🧠 Model Insights

Top 20 feature importance chart (logistic regression coefficients)
Expandable methodological notes covering:

Target variable design (country-specific median thresholds)
Why Logistic Regression was chosen over more complex models
Temporal leakage prevention and lag feature construction
Class imbalance handling via class_weight='balanced'


🧠 Modelling Approach

DetailValueAlgorithmLogistic RegressionRegularisationL2 (C=1.0)Class balancingclass_weight='balanced'Validation5-Fold Cross-Validation + Temporal Train/Test SplitTarget variableBinary — 1 = High Occurrence (≥ country median), 0 = Low OccurrenceKey featuresprev_year_count, log_prev_total_deaths, log_prev_total_affected, log_prev_total_damage, disaster type counts (flood, storm, etc.)DatasetEM-DAT (CRED, UCLouvain) · Asia · 2001–2024

Why Logistic Regression?
Interpretability is a core requirement for this project. Logistic regression produces signed, magnitude-ranked coefficients that directly answer what drives risk — a result that policymakers and domain experts can act on. More complex models (Random Forest, XGBoost) may offer marginally higher AUC-ROC but at the cost of transparency.

Temporal Leakage Prevention
All lag features (prev_year_count, log_prev_total_deaths, etc.) use data from year t-1 to predict year t. The scaler was fitted exclusively on the training set. event_count — the basis for the target variable — is explicitly excluded from the feature set.

Next-Year Forecasting
When forecasting the following year, the current year's actuals are rolled forward into the lag columns before prediction, so the model always receives the correct temporal input rather than repeated values.

⚙️ Installation & Running Locally
1. Clone the repository
bashgit clone https://github.com/<your-org>/disaster-risk-intelligence.git
cd disaster-risk-intelligence
2. Install dependencies
bashpip install -r requirements.txt
3. Ensure the folder structure matches
Place the model artifacts in models/ and the data files in data/ as shown in the project structure above.
4. Run the app
bashstreamlit run app.py

📦 Dependencies
streamlit
pandas
numpy
plotly
scikit-learn
joblib
shap
matplotlib

📊 Dataset

Source: EM-DAT — The International Disaster Database, maintained by CRED (Centre for Research on the Epidemiology of Disasters), UCLouvain, Belgium.
Scope: Asian countries, 2001–2024
Features engineered from raw data: Annual event counts, log-transformed impact metrics (deaths, affected, damage), disaster-type breakdowns (floods, storms, earthquakes, etc.)
