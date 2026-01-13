# Jersey Number Detection | 球衣號碼檢測

![Python](https://img.shields.io/badge/Python-3.8+-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-orange)
![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-red)

YOLOv8-based jersey number detection for volleyball players.

使用 YOLOv8 模型訓練球衣號碼檢測，用於識別排球運動員的背號。

---

## Training Results | 訓練結果

| Training Curves | Confusion Matrix |
|-----------------|------------------|
| ![Results](runs/jersey_detection/results.png) | ![Confusion Matrix](runs/jersey_detection/confusion_matrix.png) |

## Overview | 概述

This project trains a YOLOv8m model to detect jersey numbers on volleyball players. The dataset is compiled from multiple Roboflow datasets.

本專案訓練 YOLOv8m 模型檢測排球運動員的球衣號碼。資料集來自多個 Roboflow 公開資料集。

## System Requirements | 系統需求

- **OS:** Windows 10/11, Linux, macOS
- **Python:** 3.8-3.11
- **GPU:** NVIDIA GPU with CUDA 11.8/12.1 (recommended)
- **RAM:** 16GB+
- **Disk:** 20GB+

## Installation | 安裝

```bash
# Clone repository
git clone https://github.com/DL-Volleyball-Analysis/jersey-number-detection.git
cd jersey-number-detection

# Create environment (Conda recommended)
conda create -n jersey_detection python=3.10
conda activate jersey_detection

# Install PyTorch with CUDA
conda install pytorch torchvision pytorch-cuda=11.8 -c pytorch -c nvidia

# Install dependencies
pip install -r requirements.txt
```

## Dataset | 資料集

### Download from Roboflow

1. Get API key from [Roboflow](https://roboflow.com/)
2. Set environment variable:
   ```bash
   export ROBOFLOW_API_KEY=your_api_key
   ```
3. Download datasets:
   ```bash
   python download_datasets.py
   ```
4. Organize datasets:
   ```bash
   python organize_datasets.py
   ```

### Dataset Sources

- `volleyai-actions/jersey-number-detection-s01j4`
- `workspace67/jersey-fxmll`
- `teste-5efoz/player-number-detect`
- `hgjhj/jersey-number-detection-br3ld`

## Training | 訓練

```bash
python train_model.py
```

### Configuration | 配置

| Parameter | Value |
|-----------|-------|
| Model | YOLOv8m |
| Epochs | 100 |
| Image Size | 640x640 |
| Batch Size | 16 (auto-optimized) |
| Optimizer | AdamW |
| Learning Rate | 0.001 |

### Output | 輸出

- Best model: `runs/jersey_detection/weights/best.pt`
- Training curves: `runs/jersey_detection/results.png`
- Confusion matrix: `runs/jersey_detection/confusion_matrix.png`

## Integration | 整合

```bash
python integrate_model.py
```

Or manually copy:
```bash
cp runs/jersey_detection/weights/best.pt ../volleyball-analysis/models/jersey_detection_yv8.pt
```

### Usage Example | 使用範例

```python
from integration_example import JerseyNumberDetector

detector = JerseyNumberDetector()
detections = detector.detect(image)
```

## Troubleshooting | 疑難排解

| Issue | Solution |
|-------|----------|
| CUDA not available | Reinstall PyTorch with CUDA support |
| Out of memory | Reduce batch size or use smaller model |
| Download failed | Check API key and network |

## License | 授權

MIT License

---

*Part of [DL-Volleyball-Analysis](https://github.com/DL-Volleyball-Analysis) - Senior Capstone Project*
