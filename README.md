# 🧬 Project Everett: Quantum-Classical Cheminformatics & Solubility Engine
**Author:** Katchaw451  
**Domain:** Quantum Machine Learning (QML) for Molecular Property Prediction  
**Dataset:** DeepChem Delaney (ESOL) — 1,128 Real Molecules (`logS` Aqueous Solubility)

---

## 📖 Overview
**Project Everett** investigates the practical capabilities, optimization bottlenecks, and architectural trade-offs of Hybrid Quantum-Classical Neural Networks (Variational Quantum Circuits + PyTorch) on real-world molecular data. 

Rather than relying on synthetic toy problems or unbenchmarked metrics, **V6.1 and V6.2** evaluate a 4-qubit Variational Quantum Circuit (VQC) on the **Delaney (ESOL)** dataset using strict leakage-free preprocessing (`train_test_split` prior to scaling and PCA) and direct comparisons against classical machine learning baselines.

---

## 🧪 Scientific Progression (Version History)

* **V1 – V2 (Foundations):** Initial Proof of Concept, multi-qubit `PauliZ` expectation readouts, and Lipinski molecular descriptors.
* **V3 – V4 (Diagnostics):** Integrated RDKit SMILES parsing; diagnosed small-sample overfitting ($R^2 = -0.48$) and excessive noise underfitting ($R^2 = 0.20$).
* **V5 (Goldilocks Baseline):** Balanced circuit depth and light regularization ($p = 0.05$) on simulated/augmented descriptors ($R^2 = 0.7208$).
* **V6.1 (Real-World ESOL Benchmark):** Transitioned to the 1,128-molecule Delaney (ESOL) dataset with 7 RDKit physicochemical descriptors (`MolWt`, `MolLogP`, `NumHDonors`, `NumHAcceptors`, `TPSA`, `NumRotatableBonds`, `FractionCSP3`) compressed via PCA (93.41% variance retained) into 4 qubits. Benchmarked full-batch QML ($R^2 = 0.5366$) against Classical MLP ($R^2 = 0.8301$) and Random Forest ($R^2 = 0.8609$).
* **V6.2 (Optimization & Classical Skip Ablation — Current):** Introduced bounded $\frac{\pi}{2}\tanh(x)$ angle encoding to prevent periodic phase wrapping, mini-batch training (`batch_size=64`), differential quantum/classical learning rates with `CosineAnnealingLR`, and an explicit ablation study isolating the quantum circuit from a classical residual skip branch.

---

## 📊 V6.2 Benchmark & Ablation Results (Unseen Test Set)

To separate genuine quantum circuit learning from classical feature pass-through, **V6.2** evaluates the hybrid model both **with** and **without** (`Quantum-Only`) a direct classical skip projection, alongside two classical baselines trained on the exact same split:

| Model / Variant | $R^2$ (Test) | RMSE `[log(mol/L)]` | Training Time (s) | Architectural Notes |
| :--- | :---: | :---: | :---: | :--- |
| **Hybrid QML (With Skip)** | **0.8449** | **0.8562** | 59.17 | 4-qubit VQC (`PauliZ` readouts) + 4D classical residual skip path |
| **Hybrid QML (Quantum-Only)** | **0.6891** | **1.2123** | 57.73 | Pure VQC output feeding classical head (skip branch disabled) |
| **Classical MLP** | **0.8515** | **0.8377** | 2.05 | Compact 2-hidden-layer PyTorch MLP (`SiLU` + `Dropout`) |
| **Random Forest (300 trees)** | **0.8609** | **0.8110** | 1.63 | Classical ensemble baseline on 7 standardized RDKit descriptors |

### Key Experimental Findings
1. **Encoding & Optimization Unlock VQC Learning (+0.1525 $R^2$):** Even with the classical skip path completely disabled (`Quantum-Only`), switching from unbounded full-batch training in V6.1 ($R^2 = 0.5366$) to $\frac{\pi}{2}\tanh(x)$ angle scaling, mini-batches, and differential learning rates raised pure VQC performance to **$R^2 = 0.6891$**.
2. **Value of Ablation Studies (+0.1558 $R^2$ from Skip):** Re-enabling the classical skip connection brings the hybrid model to **$R^2 = 0.8449$** ($\text{RMSE} = 0.8562\text{ log(mol/L)}$), matching the Classical MLP ($0.8515$). Isolating both variants prevents over-attributing linear/tabular accuracy gains solely to the quantum circuit.
3. **Simulation Overhead vs. Tabular Baselines:** On a classical statevector simulator (`default.qubit`), training the 4-qubit hybrid model takes ~28x longer than a comparable PyTorch MLP (`59.17s` vs. `2.05s`), while Random Forest remains the strongest and fastest baseline (`0.8609` in `1.63s`) for low-dimensional tabular molecular descriptors.

---

## 🛠️ Tech Stack & Installation

* **Quantum Computing:** PennyLane (`default.qubit`, `AngleEmbedding`, `StronglyEntanglingLayers`)
* **Deep Learning & Classical ML:** PyTorch, Scikit-Learn
* **Cheminformatics:** RDKit
* **Data & Visualization:** Pandas, NumPy, Matplotlib, Seaborn

```bash
git clone [https://github.com/Katchaw451/project-everett-quantum-bio.git](https://github.com/Katchaw451/project-everett-quantum-bio.git)
cd project-everett-quantum-bio
pip install -r requirements.txt

"We are not just simulating molecules; we are computing reality—and benchmarking every step honestly."
