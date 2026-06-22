# Radiogenomic Bridge: Identifying Imaging Phenotypes of HRD-Enriched Breast Cancer
**Implementation Plan & Milestone Tracking**

## Objective
To determine whether the MRI imaging phenotype of breast tumors carries a detectable, biologically interpretable signal for Homologous Recombination Deficiency (HRD), using the Triple-Negative Molecular Subtype as a proxy for the HRD-enriched phenotype.

---

### Milestone 1: Data Integration & Preprocessing
**Objective:** Create a clean, unified dataset linking imaging phenotypes to biological ground truth.
* **Tasks:** * Merge Duke breast-MRI radiomic features with clinical annotations via Patient ID.
    * Map the `Molecular Subtype` column to a binary `HRD_Proxy` target (Label 3 "Triple Negative" = 1; All others = 0).
    * Perform initial data cleaning by dropping records with missing values (NaNs).
* **Success Criteria:** A single, unified Pandas dataframe is generated in memory, containing 0 missing values and a correctly encoded binary target variable representing the HRD-enriched proxy.

### Milestone 2: Feature Space Dimensionality Reduction
**Objective:** Mitigate multicollinearity to ensure model stability and interpretable feature importances.
* **Tasks:** * Calculate the absolute correlation matrix of the imaging feature space.
    * Identify and drop highly correlated feature pairs using a strict threshold.
* **Success Criteria:** The feature matrix dimensionality is successfully reduced, and a visualized correlation matrix confirms that no remaining off-diagonal elements exceed the threshold of $|\rho| \geq 0.8$.

### Milestone 3: Cross-Validated Feature Selection
**Objective:** Identify the core subset of imaging features that robustly drive the HRD signal, ensuring generalizability across the dataset.
* **Tasks:** * Initialize a Random Forest Classifier.
    * Implement 10-Fold Stratified Cross-Validation to maintain class balance across splits.
    * Extract and average the Gini feature importances across all 10 folds.
* **Success Criteria:** The pipeline successfully outputs a ranked, stable list of the Top 15 most important imaging features, demonstrating robustness against individual train/test split biases.

### Milestone 4: Radiogenomic Visualization
**Objective:** Visually validate the discriminative power of the selected features.
* **Tasks:** * Filter the dataset to include only the Top 15 consensus features.
    * Generate an unsupervised clustered heatmap (Clustermap) annotated with the `HRD_Proxy` labels.
* **Success Criteria:** The generation of a clearly legible "hero" clustermap that visibly demonstrates distinct clustering patterns (expression profiles) differentiating the HRD-enriched cohort from the non-enriched cohort.

### Milestone 5: Documentation & Future Directions
**Objective:** Finalize the deliverable, ensuring rigorous documentation of biological and analytic choices.
* **Tasks:** * Compile the PDF/Markdown report.
    * Document the explicitly encouraged use of AI/Agents (e.g., LLM-assisted boilerplate generation due to local GCP Service Account constraints).
    * Detail future improvements.
* **Success Criteria:** Submission of a reproducible repository and a final report that clearly interprets the biological relevance of the driving features and provides an honest, rigorous "What I'd do next" analysis (e.g., hyperparameter tuning, spatial feature extraction from raw DICOMs).