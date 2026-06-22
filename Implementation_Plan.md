# Radiogenomic Bridge: Identifying Imaging Phenotypes of HRD-Enriched Breast Cancer

## 1. Objective
The goal of this analysis is to determine whether the imaging phenotype of breast tumors carries a detectable signal for Homologous Recombination Deficiency (HRD). Because HR-deficient breast cancers skew heavily toward triple-negative/basal tumors, we map the **Triple-Negative Molecular Subtype (Label 3)** as a proxy for the HRD-enriched phenotype (Target = `1`). All other subtypes are categorized as non-enriched (Target = `0`).

## 2. Data Preprocessing & Methodology
To construct a robust radiogenomic pipeline, the following data wrangling and cleaning steps were executed:
* **Data Integration:** Merged Duke breast-MRI imaging features with clinical annotations based on Patient ID.
* **Target Binarization:** Converted the `Molecular Subtype` clinical feature into a binary `HRD_Proxy` target.
* **Missing Data:** Dropped rows with NaNs to ensure a clean feature space for baseline modeling.
* **Collinearity Filtering:** Calculated the absolute correlation matrix of the imaging features. To prevent multicollinearity from skewing model interpretation, features with a correlation threshold of $|\rho| \geq 0.8$ were dropped.

## 3. Modeling Strategy: Cross-Validated Feature Selection
To ensure the identified features are representative of the entire dataset and not an artifact of a single train/test split, we utilize a **10-Fold Stratified Cross-Validation** approach paired with a Random Forest Classifier. 

Stratification ensures the class imbalance of the HRD proxy is maintained across all folds. Feature importances (Gini impurity decrease) are extracted from each fold and averaged to determine the most consistently predictive imaging phenotypes.

### Implementation Code
```python
import numpy as np
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import StratifiedKFold

# Define features (post-correlation drop) and target
X = corr_thresh
y = merged_df['HRD_Proxy']

# Initialize Random Forest and Stratified 10-Fold CV
rf = RandomForestClassifier(n_estimators=100, random_state=42)
skf = StratifiedKFold(n_splits=10, shuffle=True, random_state=42)

# Array to accumulate feature importances across all folds
importances = np.zeros(X.shape[1])

# Run 10-Fold Cross-Validation
for train_idx, test_idx in skf.split(X, y):
    X_train, y_train = X.iloc[train_idx], y.iloc[train_idx]
    rf.fit(X_train, y_train)
    importances += rf.feature_importances_

# Calculate average importance across all 10 folds
importances /= skf.get_n_splits()

# Consolidate into a sorted DataFrame
feature_importance_df = pd.DataFrame({
    'Feature': X.columns,
    'Average_Importance': importances
}).sort_values(by='Average_Importance', ascending=False)

# Extract Top 15 Features
top_15_features = feature_importance_df.head(15)['Feature'].tolist()

print("Top 15 Consensus Features Driving HRD Classification:")
for feat in top_15_features:
    print(f"- {feat}")