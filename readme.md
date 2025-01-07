# Quantum-Enhanced Conformal Methods for Multi-Output Uncertainty

Welcome to the **QECMMOU** repository (**Quantum-Enhanced Conformal Methods for Multi-Output Uncertainty**). This project combines **quantum circuit data generation** with **distributional conformal prediction** to produce rigorous coverage sets for multi-dimensional quantum measurement outcomes. The repository features two main notebooks:

1. **Data Generation**  
2. **Modeling + Conformal**  

Below is an overview of each notebook, as well as instructions on installation, usage, and references.

---

## Overview

In contemporary quantum computing, **measurement randomness**—arising from inherent quantum mechanics, gate noise, and hardware imperfections—poses significant challenges for **uncertainty quantification**. Traditional conformal prediction (CP) provides finite-sample coverage guarantees in classical settings, often for univariate or low-dimensional data. Here, we push CP into quantum territory, exploring multi-output distributions (4D or 12D) derived from:
- 2-qubit circuits in a single measurement basis (computational **Z** basis).
- 2-qubit circuits measured in multiple bases (**Z**, **X**, **Y**), concatenating distributions into a higher dimension (12D).

Our methodology demonstrates how a classical multi-output regressor—e.g., a random forest—paired with **distributional conformal** can achieve valid coverage over these quantum-generated probability vectors. The result is a robust, distribution-free pipeline bridging quantum data generation and classical uncertainty frameworks.

---

## Repository Structure

- **`data_generation`**  
  *Generates synthetic quantum-circuit datasets.*  
  - **Random 2-Qubit Circuits**: Creates random circuits with user-specified depth range, gate variety, and number of shots.  
  - **Multi-Basis Measurements**: (Optional) Measures the circuit output in additional bases (**X**, **Y**).  
  - **Feature Extraction**: Captures gate counts, circuit depth, total operations, etc.  
  - **Probability Vectors**: Stores measured distributions (4D or 12D).  
  - **Duplicate Dropping**: Ensures unique feature rows.  
  - **Saves** the resulting arrays to `.npz` or `.pkl` files.

- **`modeling_conformal`**  
  *Implements multi-output regression and conformal coverage.*  
  - **Data Loading**: Reads the `.npz` or `.pkl` dataset from the data-generation notebook.  
  - **Train–Cal–Test Splits**: Typically splits data into 70% training, 15% calibration, 15% test sets.  
  - **Random Forest Regressor**: Fits a multi-output model that predicts probability vectors from classical features.  
  - **Distributional Conformal**: Calculates residuals on the calibration set, sorts them, and identifies thresholds \(\tau\) for coverage.  
  - **Coverage Evaluation**: Measures empirical coverage on the test set, printing coverage and set sizes for various \(\alpha\)-levels.

---

## Installation and Dependencies

1. **Clone this repository** or download the `.zip`:
   ```bash
   git clone https://github.com/detasar/QECMMOU.git
   cd QECMMOU
   ```
2. **Recommended Python version:** 3.8 or above (tested on 3.10).
3. **Recommended environment:** Conda or venv.
4. **Install required packages** (Qiskit, scikit-learn, NumPy, pandas, etc.):
```bash
  pip install qiskit qiskit-aer numpy pandas scikit-learn tqdm
```
If running in **Google Colab:**

Simply open each `.ipynb` notebook in Colab.
* Install missing packages via `!pip install ...` commands at the top of the notebook.

---
## Usage Instructions
1. **Data Generation**

* **Edit** `num_samples`, `shots`, `min_depth`, `max_depth`, or other parameters at the top.
**Run** all cells to produce a dataset.
The notebook saves a `.npz` or `.pkl` file with `(X_data, Y_data)` arrays.

2. **Modeling + Conformal**

* **Specify** the dataset filename you created (e.g., `"quantum_data_no_duplicates.npz"`).
* **Run** the cells to:
  * Load the dataset.
  * Train a multi-output regressor (random forest).
  * Compute distributional conformal thresholds.
  * Print coverage results for multiple α-values.
    
3. **Check the console outputs or final cells for coverage percentages and hypercube/hypersphere sizes.**

---
## Example Walkthrough
 1. **Generate 2000 quantum-circuit samples:**

  * In `data_generation.ipynb`, `set num_samples = 2000`, `shots = 1024`, and run.
  * Produces a `my_quantum_dataset.npz.`

2. **Train model and evaluate coverage:**

  * In `modeling_conformal.ipynb`, change `datafile = "my_quantum_dataset.npz"`.
  * Run through training, calibration, and test coverage. Observe coverage near 1 - alpha.
    
---
## Potential Extensions
* More Qubits: Expand to 3–4 qubits for higher-dimensional measurement distributions.
* Hardware Integration: Real quantum devices add drift/correlated noise. This pipeline can adapt with repeated calibration or advanced conformal mechanisms.
Custom Scores: Replace the ℓ2 or ℓ∞ norm with domain-specific distances, e.g., fidelity-based quantum distances.

---
## References
Below are primary references used in or relevant to this project:

* [1] S. Park and O. Simeone, Quantum Conformal Prediction for Reliable Uncertainty Quantification in Quantum Machine Learning, IEEE Trans. Quantum Eng. (2023).
* [2] C. Xu, H. Jiang, and Y. Xie, Conformal Prediction for Multi-Dimensional Time Series by Ellipsoidal Sets, ICML (2024).
* [3] S. Feldman, S. Bates, and Y. Romano, Calibrated Multiple-Output Quantile Regression with Representation Learning, arXiv:2110.00816.
(For a fuller list, see the references in the paper itself.)

