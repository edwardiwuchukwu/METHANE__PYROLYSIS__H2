Predicting Hydrogen Production from Methane Pyrolysis Powered by Induced Heating: An Application of a Hybrid Bayesian Neural Network

This repository contains a notebook that processes methane pyrolysis operating data and compares three proposed neural network approaches for predicting the interval hydrogen production rate (Mass of Hydrogen - MoH):

FNN — feed-forward neural network baseline.
BNN — dropout-based Bayesian neural network with Monte Carlo dropout inference.
H-BNN — hybrid Bayesian neural network initialized from the corresponding FNN weights and trained with a heteroscedastic Gaussian likelihood.
The workflow also performs preliminary data analysis, shared architecture and hyperparameter selection, model evaluation, hydrogen output post-processing, and model-comparison plotting.

REPOSITORY FILES
The notebook containing data preparation, preliminary analysis, architecture/hyperparameter search, FNN/BNN/H-BNN training and evaluation, post-processing, and comparison plots. It includes Palatia typography declarations and an optional Matplotlib font loader.

`Methane_Pyrolysis_CODE__BLOCK.py` — ordered Python export of the notebook's executable code cells.

`MP___DATA___SHEET.csv` — input dataset required by the notebook. It is not included in this package and must be supplied separately.
`requirements.txt` — Python package dependencies.

`README.md` — project description and run instructions.

DATASET
The notebook reads `MP___DATA___SHEET.csv`. The main data preparation and aggregation workflow uses the following columns:
`F(mL/h)`, `T(C)`, `P(W)` (renamed to `p(W)`), `TOS(h)`, `Delta_t(h)`, `n_CH4_in(mol/h)`, `E_cum(kWh)` (renamed to `Energy_cum(kWh)`), `MoH_interval(kg)` and `VoH_cum(mL)`.

The preliminary analysis cell also references `Energy_interval(kWh)` and `Y_H2_cum_measured(%)`. Keep these columns in the CSV if the preliminary analysis section is to run.

The model features are:
Continuous: `T(C)` and `TOS_mid(h)`, where `TOS_mid(h) = TOS(h) - 0.5 * Delta_t(h)`.
Categorical, one-hot encoded: `F(mL/h)`, `p(W)`, and `Delta_t(h)`.
The primary target is calculated from interval hydrogen mass and interval duration:
`rH2_interval(g/h) = 1000 * MoH_interval(kg) / Delta_t(h)`.

The target is log-transformed. Rows missing any of the explicitly required preprocessing columns are removed, and rows with non-positive `Delta_t(h)` are excluded. 

Exact duplicate operating points are grouped, and the mean log-transformed target is used for model fitting. The data are split into training, validation, and test subsets; continuous features and the target are scaled using the training subset only. Bootstrap/noise augmentation is applied only to the training subset.

RUN IN GOOGLE COLAB
Open `Methane_Pyrolysis_CODE__BLOCK.ipynb’ file in Google Colab.

Upload MP___DATA___SHEET.csv to /content/.

Run the notebook cells in the existing order.

To use the Palatia font in plots, upload an authorized Palatia.ttf file to /content/. Otherwise, Matplotlib uses its default font.

The notebook uses NumPy, Pandas, PyTorch, scikit-learn, and Matplotlib. Install any missing packages if the runtime reports an import error.

REPRODUCIBILITY
The notebook uses `random_state=42` for the data splits and training-set augmentation, and sets NumPy and PyTorch seeds to `42` in the modelling workflow. The default augmentation setting is `AUG_SCALE = 10`; the augmentation function also documents scales 2 and 5. Exact numerical reproducibility can still depend on library versions, hardware, and runtime configuration.

ABOUT THIS CODE

The code was cleaned for presentation and navigation. Markdown title and section-heading cells were added, and the original code cell source and saved outputs were retained. 

