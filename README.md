# AUGR-VQA

Official implementation repository for **AUGR-VQA: An Uncertainty-Aware Guided Reasoning Framework for Brain Tumor Visual Question Answering**.

AUGR-VQA combines a question-adaptive regional reasoning backbone, **QAdp-DG-PRUGTM**, with **Q-CUR**, a post-prediction reliability module for calibrated confidence, selective-risk analysis, and review-flag support.

This repository contains the code and notebooks used for dataset preparation, model training, ablation studies, reliability evaluation, statistical robustness analysis, and baseline comparison for the BTUMQA-225K brain tumor MRI VQA study.

---

## 📋 Directory Layout

The workspace is organized into sequential phase-based directories reflecting the pipeline of the study:

```text
AUGR-VQA/
├── notebooks/                                 # All 21 execution notebooks (designed for Google Colab/Local)
├── phase_1/p1a_segmentation_monai_brats/      # MONAI-based 3D BraTS segmentation & slice caching
│   └── dataset_cache/                         # Raw image dataset (rsnabrats20212d-001.zip)
├── phase_2/
│   ├── p2b_mc_dropout_uncertainty/            # Region MC-Dropout uncertainty estimation
│   └── p2c_ugtm_modulated_tokens/             # Uncertainty-guided token modulation (UGTM)
├── phase_3/
│   ├── p3a_brats_vqa_dataset/                 # BTUMQA-225K VQA QA pairs & train split CSVs
│   ├── p3b_text_preprocessing/                # Tokenization & pre-extracted text embeddings
│   └── p3d_qa_shortcut_bias_diagnostic/       # Diagnostic shortcut-bias model audits
├── phase_4/p4a_qgca_training/                 # AUGR-VQA core architecture training checkpoints
├── phase_5/
│   ├── p5a_proposed_model_qcur_reliability/   # Q-CUR calibration and reliability curves
│   ├── p5b_final_evaluation_ablation_calibration/ # Ablation comparison & multi-seed evaluations
│   ├── p5c_statistical_confidence_slice_robustness/ # Bootstrapping and slice robustness evaluation
│   ├── p5d_modern_baseline_comparison_models/ # Supervised baseline models (A3, A4, B1, C1)
│   └── p5e_modern_vlm_baseline_comparison/    # Zero-shot VLM comparison (LLaVA-Med, MedGemma)
├── phase_6/
│   ├── p6a_models_unified_results_comparison/ # Final unified evaluation showcase
│   └── p6b_model_complexity/                  # Parameter and operational complexity audit
└── phase_7/
    ├── p7a_paper_ready_augr_vqa_release/      # Harmonized paper-ready dataset release metadata
    └── p7b_augr_vqa_qualitative_case_figure/  # Qualitative ribbon figure & case study report assets
```

---

## 🛠️ Environment Setup

The notebooks in this repository are designed to be **dual-compatible** out-of-the-box. They automatically detect whether they are running in **Google Colab** (and mount Google Drive) or on your **local machine/GPU laptop** (resolving paths relative to your local workspace, using a git-ignored `temp/` folder for local caching instead of `/content/`).

### Option A: Local GPU Environment (Recommended for local runs)
If you have a local NVIDIA GPU (e.g. ASUS RTX 5060/4090), you can set up a Conda environment for accelerated execution:

1. Create and activate the Conda environment using the provided config file:
   ```bash
   conda env create -f environment.yml
   conda activate augr-vqa
   ```
2. Start your local Jupyter Lab or VS Code and select the `augr-vqa` kernel.

### Option B: Local Pip Installation
Alternatively, you can install the dependencies via pip:
```bash
pip install -r requirements.txt
```

### Option C: Google Colab
All notebooks are fully compatible with Google Colab. Simply upload the `notebooks/` folder to your Google Drive and open them directly in Colab. Google Drive mounting at `/content/drive/MyDrive/AUGR-VQA` is handled programmatically in the first cell of each notebook.

---

## 🚀 Reproducibility Guide

To reproduce the study's results from scratch, follow the data acquisition steps and execute the notebooks in the chronological order listed below.

### 1. Data Acquisition & Folder Setup
1. **Raw Images**: Download the RSNA BraTS 2D dataset from [Kaggle](https://www.kaggle.com/datasets/snish9/rsnabrats20212d) and place `rsnabrats20212d-001.zip` in:
   `phase_1/p1a_segmentation_monai_brats/dataset_cache/`
2. **QA Dataset**: The custom BTUMQA-225K dataset splits (qa_pairs, train, val, and test splits) are hosted on our official [Hugging Face Dataset Hub](https://huggingface.co/datasets/shuvomonowar00/BTUMQA).
   * **Manual Setup**: Download all 4 CSV files inside the `btumqa_225k/` directory on Hugging Face and place them inside the folder:
     `phase_3/p3a_brats_vqa_dataset/dataset_btumqa_225k/`
   * **Automated Setup (Recommended)**: Run the following Python snippet locally or in a Colab cell to automatically create the folder structure and download all splits directly from Hugging Face:

```python
import os
import urllib.request

# Create target directory
target_dir = "phase_3/p3a_brats_vqa_dataset/dataset_btumqa_225k"
os.makedirs(target_dir, exist_ok=True)

# Hugging Face Base URL
hf_base_url = "https://huggingface.co/datasets/shuvomonowar00/BTUMQA/resolve/main/btumqa_225k/"
csv_files = [
    "btumqa_225k_qa_pairs.csv",
    "btumqa_225k_train.csv",
    "btumqa_225k_val.csv",
    "btumqa_225k_test.csv"
]

# Download files
for file_name in csv_files:
    url = hf_base_url + file_name
    dest_path = os.path.join(target_dir, file_name)
    print(f"Downloading {file_name} from Hugging Face...")
    urllib.request.urlretrieve(url, dest_path)
print("QA Dataset successfully set up!")
```

*Note: Heavy binary assets (e.g. pre-computed Swin-T/ViT features, trained checkpoints, and text embeddings) are ignored by git via `.gitignore` to keep the repo lightweight. You can generate them by running the full pipeline.*

---

### 2. Notebook Execution Pipeline

Run the notebooks located in the `notebooks/` directory in the following order:

#### Phase 1 and 2: Segmentation, Tokenization, and Uncertainty
1. **`Phase_1A_(RSNA_BraTS_MONAI_Segmentation_RSNA_2D_Mask_Generation).ipynb`**: Performs 3D MONAI segmentation, extracts 2D slices, and generates mask overlays.
2. **`Phase_2ABC_(RSNA_BraTS_Visual_Token_Extraction_MC_Dropout_Uncertainty_UGTM).ipynb`**: Generates visual region tokens, computes MC-dropout region uncertainty, and performs UGTM token modulation.

#### Phase 3: Dataset Preparation and Bias Auditing
3. **`Phase_3A_(BTUMQA_225K_EDA).ipynb`**: Exploratory Data Analysis of the VQA dataset.
4. **`Phase_3B_(BTUMQA_225K_Text_Preprocessing).ipynb`**: Tokenizes question-answer pairs and extracts BioMedCLIP/Transformer text embeddings.
5. **`Phase_3C_(BTUMQA_225K_Clean_Metadata_Benchmark_Readiness_Bias_Audit).ipynb`**: Performs QA dataset alignment and diagnostic audits.
6. **`Phase_3D_(BTUMQA_225K_Clean_Metadata_QAdp_PRUGTM_Shortcut_Bias_Model_Audit).ipynb`**: Audits language-bias shortcuts.
7. **`Phase_3G_(BTUMQA_225K_Clean_QAdp_Input_Ablation_Shortcut_Diagnostic).ipynb`**: Performs input ablation to diagnose vision-language modality shortcuts.

#### Phase 4: Core Training
8. **`Phase_4A_(BTUMQA_225K_QAdp_PRUGTM_Question_Only_Clean_Metadata_Four_Seeds).ipynb`**: Trains the question-only baseline across 4 seeds.
9. **`Phase_4A_(BTUMQA_225K_QGCA_No_UGTM_Clean_Metadata_Four_Seeds).ipynb`**: Trains the standard QGCA fusion model without token modulation.
10. **`Phase_4A_(BTUMQA_225K_QGCA_PRUGTM_Hybrid_Clean_Metadata_Four_Seeds).ipynb`**: Trains the PRUGTM-modulated token hybrid baseline.
11. **`Phase_4A_(BTUMQA_225K_QGCA_QAdp_PRUGTM_Hybrid_Clean_Metadata_Four_Seeds).ipynb`**: Trains the fully integrated AUGR-VQA model (QAdp + PRUGTM).
12. **`Phase_4A_(BTUMQA_225K_UGTM_QGCA_Clean_Metadata_Four_Seeds).ipynb`**: Trains the UGTM-modulated baseline model.

#### Phase 5: Calibration, Baselines, and Robustness
13. **`Phase_5A_(BTUMQA_225K_PRUGTM_Hybrid_QCUR_Main_Reference_Reliability).ipynb`**: Runs Q-CUR post-prediction reliability calibration.
14. **`Phase_5B_(BTUMQA_225K_Clean_Metadata_Four_Seeds_Final_Evaluation_Ablation_Calibration_Comparison).ipynb`**: Validates prediction performance across all seeds.
15. **`Phase_5C_(BTUMQA_225K_Clean_Metadata_Four_Seeds_Statistical_Confidence_Slice_Robustness).ipynb`**: Runs bootstrapping for statistical confidence intervals and slice robustness tests.
16. **`Phase_5D_A3_(BTUMQA_225K_BiomedCLIP_Dual_View_Naturalized_Global23_Ensemble).ipynb`**: Evaluates BiomedCLIP prompt ensemble.
17. **`Phase_5D_A4_(BTUMQA_225K_BiomedCLIP_Frozen_Feature_Supervised_Baseline).ipynb`**: Trains a MLP classification head on frozen BioMedCLIP features.
18. **`Phase_5D_B1_(BTUMQA_225K_CNN_Frozen_Feature_Supervised_Baselines).ipynb`**: Evaluates ResNet50, DenseNet121, and EfficientNet standard baselines.
19. **`Phase_5D_C1_(BTUMQA_225K_Attention_Frozen_Feature_Supervised_Baselines).ipynb`**: Evaluates Swin-T and ViT attention baselines.
20. **`Phase_5E_(BTUMQA_225K_Modern_Medical_VLM_Baseline_Comparison).ipynb`**: Compares output predictions with modern medical VLMs.

#### Phase 6: Unified Analysis & Complexity
21. **`Phase_6A_(BTUMQA_225K_Models_Unified_Results_Comparison).ipynb`**: Compiles model evaluations into unified benchmark tables and curves.
22. **`Phase_6B_(BTUMQA_225K_Task_Trained_Parameter_Audit).ipynb`**: Analyzes trainable parameter counts and operational complexity (FLOPs).

#### Phase 7: Naming Harmonization & Qualitative Case Study
23. **`Phase_7A_(AUGR_VQA_Paper_Naming_Harmonization_Final_Release).ipynb`**: Harmonizes output files and creates the paper-ready release report.
24. **`Phase_7B_(BTUMQA_225K_Four_Seeds_Prediction_QCUR_Qualitative_Case_Study_Figure_Revision_1).ipynb`**: Generates qualitative case study ribbon figures.

---

## 🛡️ License

License information will be added after supervisor approval.

## 📝 Citation

Citation information will be added after manuscript finalization.

## 👥 Repository Status

This repository is released for research transparency and reproducibility. BTUMQA-225K is a controlled research evaluation resource and is **not** intended for clinical diagnosis or deployment.
