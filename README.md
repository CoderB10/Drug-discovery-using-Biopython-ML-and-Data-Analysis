# Drug-discovery-using-Biopython-ML-and-Data-Analysis

💊 ML & Data Analysis in Drug Discovery (ChEMBL + QSAR)

**QSAR (Quantitative Structure–Activity Relationship)** pipeline end-to-end:
✅ fetch bioactivity data → ✅ clean & transform → ✅ build molecular features → ✅ train ML models → ✅ compare results

---

## 🧭 What this project does

### 1) 🔎 Collect bioactivity data (ChEMBL)
- Search a target in ChEMBL and pull **IC50** activity records for compounds.

### 2) 🧹 Clean + prepare the dataset
- Keep key columns (e.g., ChEMBL ID, SMILES, IC50)
- Convert IC50 into **pIC50** (more stable for modeling)
- Label compounds into **bioactivity classes** (active / inactive / intermediate)

### 3) 🧪 Engineer molecular features
- **Lipinski descriptors (RDKit)**: MW, LogP, HBD, HBA
- **Fingerprints (PaDEL / PubChem)**: binary feature vectors for ML

### 4) 📊 Explore chemical space (EDA)
- Basic distribution plots and class comparisons
- **Mann–Whitney U tests** to check whether descriptor distributions differ between classes

### 5) 🤖 Train & benchmark ML models
- Build a baseline QSAR regression model
- Compare many regressors automatically using **LazyPredict**

---

## ✨ Key insights from the notebook

Here are the main takeaways observed in the saved notebook run:

### 📌 Insight 1: Actives vs inactives aren’t “the same” statistically
Using **Mann–Whitney U tests**:
- ✅ **pIC50** shows a **different distribution** between actives and inactives (reject H0)
- ✅ **Molecular Weight (MW)** differs significantly (reject H0)
- ✅ **H-bond donors (HBD)** differ significantly (reject H0)
- ✅ **H-bond acceptors (HBA)** differ significantly (reject H0)
- ⚖️ **LogP** appears **similar** between actives and inactives here (fail to reject H0)

### 📌 Insight 2: Feature filtering matters a lot
- The fingerprint feature space starts large (**881 bits**)
- After variance filtering, it shrinks to **137 informative features** 🧠

### 📌 Insight 3: Baseline models achieve moderate predictability
- Best models reach about **R² ≈ 0.54** on the test set
- Error remains meaningful (RMSE ~ **1.06**), so improvements like better splitting/tuning can help 🚀

---

## 🏁 Results snapshot (from the saved notebook run)

**Modeling dataset (fingerprints):**
- **4695 compounds**
- **881 fingerprint bits** → after variance filtering: **137 features**
- Split: **80/20** (3756 / 939)

**Baseline:**
- 🌲 Random Forest test **R² ≈ 0.54**

**Top test-set models (LazyPredict):**
- 🟩 HistGradientBoostingRegressor: **R² 0.54**, RMSE 1.06
- 🟦 LGBMRegressor (LightGBM): **R² 0.54**, RMSE 1.06
- 🌲 RandomForestRegressor: **R² 0.52**, RMSE 1.08

> Interpretation: the models capture real structure–activity signal, but there’s room for better generalization and tuning.

---

## 🧰 Tech stack

### 🐍 Core tools & libraries
- Python, Jupyter Notebook
- pandas, numpy
- scikit-learn
- matplotlib / seaborn
- `chembl_webresource_client` (ChEMBL data access)
- RDKit (Lipinski descriptors)
- PaDEL-Descriptor (fingerprints; requires Java)
- LazyPredict (quick multi-model benchmarking)

### 🤖 ML models used (notebook)
**Baseline model**
- RandomForestRegressor 🌲

**Benchmarked via LazyPredict**
- HistGradientBoostingRegressor
- GradientBoostingRegressor
- LGBMRegressor (LightGBM)
- XGBRegressor (XGBoost)
- SVR / NuSVR
- KNeighborsRegressor
- MLPRegressor
- BaggingRegressor
- ExtraTreesRegressor
- (plus additional linear models like Ridge / Lasso variants)

---

## ⚙️ How to run

1) Clone the repo and open the notebook:
```bash
git clone <your-repo-url>
cd <your-repo-folder>
jupyter notebook
