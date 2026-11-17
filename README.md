# JPCL-SAM-ML-Reanalysis

Reproducible analysis notebooks accompanying our Comment on machine-learning-based prediction and molecular design of self-assembled monolayer (SAM)-forming molecules for perovskite solar cells.

This repository provides the computational workflows used to reassess model validation, feature-family contributions, and learning-curve behavior based on the corrected dataset released by Li et al. following their published Correction.

## 1. Repository Contents

The repository contains three Jupyter notebooks covering the principal computational analyses reported in the manuscript and Supporting Information.

### 1.1 `JPCL_Fig1_Validation.ipynb`

Repeated nested cross-validation comparing point-wise and molecule-wise validation strategies.

- 20 independent repetitions.
- 5 outer folds and 4 inner folds.
- Molecular identities grouped according to exact SMILES for molecule-wise validation.
- Evaluates predictive performance under two different data-partitioning strategies.
- Generates Figure 1 and Supporting Information Figure S1.

### 1.2 `JPCL_Fig2_Ablation_Control.ipynb`

Feature-family ablation and permutation negative-control analysis under molecule-wise validation.

Three feature representations are compared:

- **RDKit + Device:** 220 predictors (196 molecular descriptors + 24 device-related variables).
- **Device only:** 24 device-related variables.
- **RDKit only:** 196 molecular descriptors.

The feature-ablation models use identical outer-fold partitions to enable paired comparisons. The permutation negative control examines whether the improvement of the combined representation can be explained merely by adding numerical features without preserving their molecular correspondence.

- Repeated nested molecule-wise validation.
- Independent hyperparameter optimization within the outer-training partitions.
- Paired feature-family comparisons.
- Generates Figure 2 and Supporting Information Figure S2.

### 1.3 `JPCL_FigS3_Learning_Curve.ipynb`

Learning-curve analysis of the corrected RDKit + Device XGBoost model under point-wise and molecule-wise validation.

- Uses all 220 predictors (196 RDKit descriptors + 24 device-related variables).
- Evaluates five training-set sizes: 20%, 40%, 60%, 80%, and 100% of the outer training pool.
- Performs five-fold cross-validation repeated five times, yielding 25 fold–repeat evaluations for each training size and validation strategy.
- Uses point-wise (record-wise in the code) and molecule-wise data partitioning.
- Groups records sharing the same exact SMILES string in molecule-wise validation.
- Uses a greedy group-allocation procedure to approximately balance device-record counts across molecule-wise folds while keeping each molecular group intact.
- Progressively increases the number of training records in point-wise analysis and the number of unique SAM molecules in molecule-wise analysis.
- Uses a fixed XGBoost hyperparameter configuration adopted from the corrected RDKit model reported in Supporting Information Table S6, without reoptimization at individual training-set sizes.
- Evaluates training and validation performance using R², RMSE, and MAE.
- Reports mean and standard deviation across the 25 fold–repeat evaluations.

The notebook generates two learning-curve plots (R² and RMSE), together with the numerical results used to prepare Figure S3 and Table S16 in the Supporting Information.

## 2. Dataset

All three notebooks use the corrected `RDKit_10` dataset released by Li et al. following their published Correction.

**Original dataset repository:**

https://github.com/Haifeng-Li-ML/SAM-ML_NEW

The corrected dataset contains:

- 99 device records.
- 57 unique SAM molecular identities.
- 220 predictors, including 196 RDKit molecular descriptors and 24 device-related variables.
- PCE as the prediction target.

The dataset file is not redistributed in this repository. Users should obtain the corrected dataset from the original repository.

For the learning-curve notebook, the expected CSV filename is:

`Rdkit_10_修改.csv`

The notebook checks the `data/` directory first and then the current working directory for this file.

## 3. Running the Notebooks

The notebooks are designed to run in **Google Colab**.

### Step 1. Open a notebook

Open the desired `.ipynb` file in Google Colab.

### Step 2. Install dependencies

Run the first code cell to install the required scientific Python libraries.

The principal dependencies include:

- Python
- NumPy
- pandas
- Matplotlib
- scikit-learn
- XGBoost

The notebooks also report the relevant package versions during execution.

### Step 3. Upload the corrected dataset

When prompted by the notebook, upload the corrected CSV file:

`Rdkit_10_修改.csv`

For the learning-curve notebook, the CSV should be uploaded directly rather than as a ZIP archive.

### Step 4. Execute the notebook

Run all cells sequentially.

The notebooks perform the corresponding data checks, validation procedures, model training, performance evaluation, and result generation.

### Step 5. Retrieve the results

Generated figures, numerical results, and configuration files are saved in the `results/` directory when applicable.

The learning-curve notebook additionally packages its outputs into:

`JPCL_R4_2_LearningCurve_results.zip`

The ZIP file is automatically offered for download after successful execution.

## 4. Learning-Curve Outputs

The third notebook generates the following files:

| File | Description |
|---|---|
| `learning_curve_raw_metrics.csv` | Fold-level training and validation metrics |
| `learning_curve_summary.csv` | Mean, standard deviation, and empirical percentile summaries |
| `learning_curve_run_config.json` | Dataset audit and computational configuration |
| `learning_curve_R2.png` | Training and validation R² learning curves |
| `learning_curve_RMSE.png` | Training and validation RMSE learning curves |

The numerical summary supports the construction of Table S16, while the two learning-curve plots provide the components used to prepare Figure S3.

## 5. Computational Reproducibility

The notebooks use fixed random seeds and explicitly defined validation procedures to facilitate reproducibility.

For the learning-curve analysis:

- Base random seed for data splitting and subset sampling: 2101.
- XGBoost model random state: 2026.
- Number of folds: 5.
- Number of repetitions: 5.
- Training fractions: 20%, 40%, 60%, 80%, and 100%.

Molecule-wise validation preserves molecular identities throughout data partitioning, preventing records belonging to the same exact-SMILES group from appearing in both training and validation sets.

The learning-curve analysis uses a fixed hyperparameter configuration and does not perform additional hyperparameter optimization at each training-set size. Its purpose is to examine how training-set size affects predictive performance under the two validation strategies, rather than to identify the optimal model configuration.

The reported standard deviations and empirical percentile ranges describe variability across the evaluated fold–repeat partitions. They should not be interpreted as formal confidence intervals. No independent experimental noise baseline is estimated in this analysis.

## 6. Data Availability

The corrected dataset and associated model files are available from the original repository released by Li et al.:

https://github.com/Haifeng-Li-ML/SAM-ML_NEW

The numerical data underlying Figures 1 and 2 are provided as Data S1 in the Supporting Information.

The analysis notebooks in this repository provide the computational procedures used for the corresponding validation, feature-ablation, permutation negative-control, and learning-curve analyses.

## 7. Usage and Citation

The notebooks are provided to facilitate reproducibility and independent examination of the analyses reported in the associated Comment.

Users should cite the original dataset and the associated research article when using the corrected dataset or reproducing the reported analyses.
