## Research question: Can the imaging phenotype see HR deficiency?

This repository contains Python notebooks that showcase preliminary results to answer the aforementioned research question. The notebooks should be run as follows:
- merge_dfs.ipynb: data cleaning (input: *Clinical_and_Other_Features.xlsx* and *Imaging_Features.xlsx*)
- quick_eda.ipynb: exploration of data using correlation analysis (input: *merged_image_clinical_features.csv*)
- random_forest_classification.ipynb: feature engineering (with visualizations) and preliminary classification using random forest (input: *thresholded_image_clinical_features.csv*)

**Note:** The agent would ensure it's on track by referencing the implementation plan with milestones for each step. The implementation plan without milestones is also shown for reference.   
  
------------------
File tree:
.
└── huang_lab_assessment/
    ├── Huang_Lab_Report_Final.pdf
    ├── Implementation_Plan_Milestones.md
    ├── Implementation_Plan.md
    ├── merge_dfs.ipynb
    ├── quick_eda.ipynb
    ├── random_forest_classification.ipynb
    ├── README.md
    ├── data/
    │   ├── Clinical_and_Other_Features.xlsx
    │   ├── Imaging_Features.xlsx
    │   ├── merged_image_clinical_features.csv
    │   └── thresholded_image_clinical_features.csv
    └── results/
        ├── 3D_PCA_UMAP_top_15_features.png
        ├── clinical_feature_clustermap.png
        ├── clinical_feature_correlation_matrix.png
        ├── clustermap_top_15_features.png
        ├── roc_curves_top_15_features.png 
        └── top_15_feature_importance.png
