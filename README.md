# Modality-Invariant Representation Learning for Fair Skin Disease Classification

This repository contains the implementation of a dual-encoder vision transformer (ViT) architecture for learning modality-invariant representations of skin lesion images. The model is trained on a combination of paired (clinical + dermoscopic) and unpaired datasets, with multi‑objective losses that enforce fairness across Fitzpatrick skin types (FST) and modality invariance.

## Overview

The pipeline:

1. **Data preparation**: Harmonizes seven disease classes from four public datasets (HIBA, Fitzpatrick17k, HAM10000, DermNet). Builds CSV files with labels, image paths, and estimated FST (via ITA index).
2. **Balanced augmentation**: Oversamples minority classes and FST groups in training splits.
3. **Model**: Dual‑encoder ViT (clinical & dermoscopic) sharing a projection head. Produces shared embeddings used for classification, skin‑type uniformity, contrastive learning, and mutual information minimization.
4. **Loss functions**:
   - Focal loss for classification (γ=2, label smoothing 0.05)
   - Confusion loss (KL divergence to uniform) for skin‑type fairness
   - Supervised contrastive loss (SupCon) for class‑wise embedding clustering
   - Mutual information loss (MSE) between clinical and dermoscopic embeddings of paired images
5. **Training**: Uses weighted random sampling (class‑balanced), AdamW optimizer with cosine decay schedule, and checkpointing.
6. **Fairness evaluation**: Computes EOM, PQD, DPM, and FATE metrics on validation and test sets, as well as per‑FST AUROC.
7. **Ablation studies**: Compares full model against variants without each loss component.
8. **Visualizations**: Training curves, per‑FST AUROC bar charts, UMAP/t‑SNE of shared embeddings coloured by class, FST, and modality.

## Datasets

The model is trained and evaluated on the following datasets (all publicly available):

| Dataset | Modality | Use |
|---------|----------|-----|
| **HIBA** (Hospital Italiano de Buenos Aires) | Clinical + Dermoscopic (paired) | Paired training |
| **Fitzpatrick17k** | Clinical (unpaired) | Unpaired clinical training |
| **HAM10000** | Dermoscopic (unpaired) | Unpaired dermoscopic training |
| **DermNet** | Various (unpaired) | External evaluation |

### Unified taxonomy (7 classes)

| ID | Class |
|----|-------|
| 0 | Melanoma |
| 1 | Nevus |
| 2 | Basal Cell Carcinoma |
| 3 | Actinic Keratosis |
| 4 | Seborrheic Keratosis |
| 5 | Squamous Cell Carcinoma |
| 6 | Other |

### Dataset links

- HIBA: [HIBASkinLesionsDataset](https://www.kaggle.com/datasets/asosenge/hibaskinlesionsdataset-main)
- Fitzpatrick17k: [Fitzpatrick17k](https://www.kaggle.com/datasets/asosenge/fitzpatrick17k)
- HAM10000: [HAM10000](https://www.kaggle.com/datasets/asosenge/ham10000)
- DermNet: [DermNet](https://www.kaggle.com/datasets/shubhamgoel27/dermnet)

### Citations

If you use this code or the datasets, please cite the original sources:

```bibtex
@article{groh2021fitzpatrick17k,
  title={Fitzpatrick17k: A diverse dataset for skin cancer detection},
  author={Groh, Matthew and Badrinarayanan, Varun and Li, Siyuan and others},
  journal={arXiv preprint arXiv:2106.09374},
  year={2021}
}

@article{tschandl2018ham10000,
  title={The HAM10000 dataset, a large collection of multi-source dermatoscopic images of common pigmented skin lesions},
  author={Tschandl, Philipp and Rosendahl, Cliff and Kittler, Harald},
  journal={Scientific data},
  volume={5},
  number={1},
  pages={1--9},
  year={2018},
  publisher={Nature Publishing Group}
}

@inproceedings{benitez2020hiba,
  title={HIBA skin lesions dataset: A large and diverse dataset for skin cancer classification},
  author={Benitez, Diego S. and others},
  booktitle={Iberian Conference on Pattern Recognition and Image Analysis},
  year={2020}
}
```

## Architecture

- **Backbone**: `vit_small_patch16_224` from `timm` (pretrained on ImageNet‑21k)
- **Projection head**: Linear(384) → BatchNorm1d → GELU → Linear(512) with L2 normalization
- **Dual encoders**: Clinical and dermoscopic branches share the projection head weights.
- **Classifier**: Linear(512) → 7 classes
- **Skin‑type classifier**: Linear(512) → 256 → ReLU → Linear(6)

## Training

- **Optimizer**: AdamW (lr=1e-4, weight decay=1e-4)
- **Scheduler**: Cosine decay with 5‑epoch warmup
- **Batch size**: 32
- **Epochs**: 15 (full) or 1 (quick ablation)
- **Data augmentation**: Random crop, flip, rotation, colour jitter, Gaussian blur, affine, autocontrast, random erasing.
- **Weighted random sampler**: Class‑balanced (oversampling minority classes).

## Fairness Metrics

- **EOM** (Equality of Opportunity): 1 − std(per‑FST AUROC)
- **PQD** (Performance Difference): max(per‑FST AUROC) − min(per‑FST AUROC)
- **DPM** (Demographic Parity): max(per‑FST prediction rate) − min(per‑FST prediction rate)
- **FATE**: α·AUROC + (1−α)·EOM (α=0.5)

## Results (summary)

- **Validation**: AUROC = 0.9605, EOM = 0.9651, FATE = 0.9628
- **Test (internal)**: AUROC = 0.9656, EOM = 0.9691, FATE = 0.9673
- **Cross‑dataset (DermNet)**: AUROC = 0.6925

Ablation shows that removing any loss component degrades fairness (EOM, FATE), with the full model achieving the best trade‑off.

## Running the code

This code is designed to run on **Kaggle** (GPU T4 or better). To reproduce:

1. Upload the notebook to a Kaggle notebook.
2. Add the required datasets as inputs (see links above).
3. Set your HuggingFace token as a Kaggle secret (`HF_TOKEN`) to avoid download rate limits.
4. Run all cells sequentially.

The notebook will:
- Download and cache the ViT backbone (first run only).
- Build CSV files in `/kaggle/working/csvs`.
- Train the model and save checkpoints in `/kaggle/working/checkpoints`.
- Generate evaluation figures and tables in `/kaggle/working/results`.

To run a quick test (1 epoch), set `QUICK = True` in the ablation cell.

## Requirements

All dependencies are installed in the first cell (`!pip install ...`). The environment includes:

- PyTorch, torchvision
- timm, einops
- scikit‑learn, umap‑learn, seaborn, matplotlib
- pandas, tqdm, Pillow
- shap, captum

## License

This code is provided for research purposes. Please cite the relevant papers if you use it.
