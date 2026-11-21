# 🧬 Project Everett: Quantum Drug Discovery Engine
**Author:** Katchaw451  
**Domain:** Quantum Machine Learning (QML) for Cheminformatics

## 📖 The Scientific Journey (Version History)
This project demonstrates the iterative development of a Hybrid Quantum-Classical Neural Network (VQC + PyTorch) to predict Topological Polar Surface Area (TPSA) for drug-like molecules.

*   **V1 (Basic):** Initial Proof of Concept with synthetic data.
*   **V2 (Advanced):** Multi-qubit readout, Lipinski descriptors, and R² metrics.
*   **V3 (Production):** Integrated RDKit to process real SMILES strings. *Diagnosed Overfitting (R² = -0.48) due to micro-dataset size.*
*   **V4 (Anti-Overfit):** Introduced augmented datasets and Dropout. *Diagnosed Underfitting (R² = 0.20) due to excessive noise injection.*
*   **V5 (The Goldilocks Zone - Current):** Balanced quantum circuit depth, Dropout (p=0.05), and realistic data generation. Achieved optimal generalization on unseen molecular data.

## 📊 Final Model Performance (V5)
*   **R² Score:** 0.7208 (Explains 72% of variance on unseen molecules)
*   **RMSE:** 0.5105
*   **Architecture:** 4-Qubit Variational Quantum Circuit (Data Re-uploading) + Classical MLP with Dropout.
*   **Key Insight:** Validation loss is lower than training loss, indicating excellent generalization without memorization.

## 🛠️ Tech Stack
*   **Quantum:** PennyLane
*   **Classical:** PyTorch, RDKit, Scikit-Learn
*   **Environment:** Google Colab / Jupyter

*"We are not just simulating molecules; we are computing reality."*
