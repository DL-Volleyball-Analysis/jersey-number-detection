> **Archived.** Part of the NTOU senior capstone *Volleyball Match Analysis System Based on Deep Learning*.
> The code now lives in [volleyball-analysis/training/jersey-numbers](https://github.com/DL-Volleyball-Analysis/volleyball-analysis/tree/main/training/jersey-numbers);
> this repository is kept read-only as the record of the capstone version.

# Capstone: Jersey Number Detection | 球衣號碼偵測

A YOLOv8m detector for single digits (0-9) on volleyball shirts, trained on four merged Roboflow Universe
datasets, meant to turn player tracks into shirt numbers.

<p align="center"><img src="runs/jersey_detection/val_batch0_pred.jpg" width="640" alt="Validation predictions"></p>

## Result and correction
| | |
|---|---|
| Reported in the capstone | validation mAP@0.5 0.966, mAP@0.5:0.95 0.736 (epoch 95 of 100, 640 px) |
| Found in October 2026 | the merge script (`organize_datasets.py`) mapped class *indices* across datasets and ignored class *names*, so balls, whole players and whole-number boxes from two of the four datasets became the digits 0, 1 and 2. On the 87 test crops of one dataset the model predicts "0" on 86 whole-number boxes. The validation score therefore does not measure digit reading, and the model should not be used. |

Details: [docs/results/actions.md](https://github.com/DL-Volleyball-Analysis/volleyball-analysis/blob/main/docs/results/actions.md).
A corrected merge and retraining are planned in the main repository (change `add-player-actions`).

| Training curves | Confusion matrix (normalised) |
|---|---|
| ![Training curves](runs/jersey_detection/results.png) | ![Confusion matrix](runs/jersey_detection/confusion_matrix_normalized.png) |

## Contents
| Path | What it is |
|---|---|
| `download_datasets.py` | downloads the four Roboflow datasets (needs `ROBOFLOW_API_KEY`) |
| `organize_datasets.py` | merges them into one YOLO dataset (contains the class-mapping bug above) |
| `train_model.py` | YOLOv8m training |
| `integrate_model.py` | copies the weights into the capstone web app |
| `runs/jersey_detection/` | training arguments, per-epoch metrics, curves and sample predictions |

## Data
Roboflow Universe: volleyai-actions/jersey-number-detection-s01j4, workspace67/jersey-fxmll,
teste-5efoz/player-number-detect, hgjhj/jersey-number-detection-br3ld (CC BY 4.0).

## Team
Liang Yu-Jia 梁祐嘉 (lead), Tsai Pei-Ying 蔡佩穎, Chung Chia-Hsin 鍾佳芯; advisor Professor Ting Pei-Yi 丁培毅.
Department of Computer Science and Engineering, National Taiwan Ocean University. MIT License.
