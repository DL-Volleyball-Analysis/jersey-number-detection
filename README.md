# Jersey Number Detection | 球衣號碼檢測模型訓練專案

![Python](https://img.shields.io/badge/Python-3.8+-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-orange)
![YOLOv8](https://img.shields.io/badge/YOLOv8-Ultralytics-red)
![CUDA](https://img.shields.io/badge/CUDA-11.8%2F12.1-green)
![License](https://img.shields.io/badge/license-MIT-blue)

This project trains a YOLOv8 model to detect jersey numbers on volleyball players. The dataset comes from multiple Roboflow Universe public datasets.

這個專案用於訓練一個專門檢測排球運動員球衣號碼的 YOLOv8 模型。資料集來自 Roboflow Universe 的多個公開資料集。

---

## Training Results Preview | 訓練結果預覽

Results using YOLOv8m model | 使用 YOLOv8m 模型訓練後的結果：

| Training Curves | Confusion Matrix |
|-----------------|------------------|
| ![Training Results](runs/jersey_detection/results.png) | ![Confusion Matrix](runs/jersey_detection/confusion_matrix.png) |

| Validation Results | Training Batch |
|--------------------|----------------|
| ![Validation Results](runs/jersey_detection/val_batch0_pred.jpg) | ![Training Batch](runs/jersey_detection/train_batch0.jpg) |

## Topics

`yolov8` `jersey-detection` `computer-vision` `deep-learning` `pytorch` `object-detection` `volleyball` `roboflow` `yolo` `machine-learning`

## Table of Contents | 目錄

- [System Requirements | 系統需求](#system-requirements--系統需求)
- [Installation | 安裝步驟](#installation--安裝步驟)
- [Dataset Download | 資料集下載](#dataset-download--資料集下載)
- [Dataset Organization | 資料集組織](#dataset-organization--資料集組織)
- [Model Training | 模型訓練](#model-training--模型訓練)
- [Integration | 整合到專案](#integration--整合到專案)
- [Troubleshooting | 疑難排解](#troubleshooting--疑難排解)

---

## System Requirements | 系統需求

### Windows (Recommended for GPU Training)

| Requirement | Value |
|-------------|-------|
| OS | Windows 10/11 |
| Python | 3.8-3.11 (3.10 recommended) |
| GPU | NVIDIA GPU with CUDA (6GB+ VRAM recommended) |
| CUDA | 11.8 or 12.1 |
| RAM | 16GB minimum |
| Disk | 20GB+ available |

### Check CUDA Version | 檢查 CUDA 版本

```cmd
nvidia-smi
```

---

## Installation | 安裝步驟

### 1. Clone Repository | 克隆專案

```cmd
git clone https://github.com/DL-Volleyball-Analysis/jersey-number-detection.git
cd jersey-number-detection
```

### 2. Create Environment | 創建虛擬環境

#### Option A: Conda (Recommended) | 使用 Conda（推薦）

```cmd
conda create -n jersey_detection python=3.10
conda activate jersey_detection
```

**Install PyTorch with CUDA | 安裝 PyTorch（CUDA 版本）:**

```cmd
# CUDA 11.8
conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia

# CUDA 12.1
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia

# CPU only (not recommended | 不推薦)
conda install pytorch torchvision torchaudio cpuonly -c pytorch
```

#### Option B: venv | 使用 Python venv

```cmd
python -m venv venv
venv\Scripts\activate  # Windows
source venv/bin/activate  # Linux/macOS
```

**Install PyTorch:**

```cmd
# CUDA 11.8
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118

# CUDA 12.1
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```

### 3. Install Dependencies | 安裝依賴

```cmd
pip install -r requirements.txt
```

### 4. Verify Installation | 驗證安裝

```cmd
python -c "import torch; print(f'PyTorch: {torch.__version__}'); print(f'CUDA available: {torch.cuda.is_available()}')"
```

If you see `CUDA available: True`, installation is successful!

如果看到 `CUDA available: True`，表示安裝成功！

---

## Dataset Download | 資料集下載

### Step 1: Get Roboflow API Key | 獲取 Roboflow API Key

1. Go to [Roboflow](https://roboflow.com/) | 前往 Roboflow
2. Register/Login | 註冊/登入帳號
3. Go to Account Settings → API | 前往帳戶設定 → API
4. Copy your API Key | 複製 API Key

### Step 2: Set Environment Variable | 設置環境變數

```cmd
# Windows CMD
set ROBOFLOW_API_KEY=your_api_key_here

# Windows PowerShell
$env:ROBOFLOW_API_KEY="your_api_key_here"

# Linux/macOS
export ROBOFLOW_API_KEY=your_api_key_here
```

### Step 3: Download Datasets | 下載資料集

```cmd
python download_datasets.py
```

This downloads 4 datasets | 下載以下 4 個資料集：

1. `volleyai-actions/jersey-number-detection-s01j4`
2. `workspace67/jersey-fxmll`
3. `teste-5efoz/player-number-detect`
4. `hgjhj/jersey-number-detection-br3ld`

Datasets saved to `datasets/raw/` | 資料集保存於 `datasets/raw/`

**Expected time | 預期時間**: 10-30 minutes depending on network speed

---

## Dataset Organization | 資料集組織

After download, merge and organize datasets | 下載完成後，合併和組織資料集：

```cmd
python organize_datasets.py
```

This script will | 此腳本會：
1. Analyze all dataset classes | 分析所有資料集的類別
2. Unify class mappings | 統一類別映射
3. Merge train/valid/test sets | 合併訓練/驗證/測試集
4. Generate `data.yaml` config | 生成配置文件

**Output structure | 輸出結構:**

```
datasets/processed/merged/
├── train/
│   ├── images/
│   └── labels/
├── val/
│   ├── images/
│   └── labels/
├── test/
│   ├── images/
│   └── labels/
└── data.yaml
```

---

## Model Training | 模型訓練

### Basic Training | 基本訓練

```cmd
python train_model.py
```

### Training Configuration | 訓練配置

Default configuration (can be modified in `train_model.py`) | 預設配置：

| Parameter | Value | Description |
|-----------|-------|-------------|
| Model | YOLOv8m | Medium - balanced accuracy and speed |
| Epochs | 100 | Training iterations |
| Image Size | 640x640 | Input resolution |
| Batch Size | 16 | Auto-optimized for GPU |
| Early Stop | 20 epochs | Patience value |
| Optimizer | AdamW | Training optimizer |
| Learning Rate | 0.001 | Initial LR |
| AMP | Enabled | Mixed precision training |

### Model Options | 模型選擇

Modify `MODEL_NAME` in `train_model.py` | 在 `train_model.py` 中修改 `MODEL_NAME`：

| Model | Speed | Accuracy | Use Case |
|-------|-------|----------|----------|
| yolov8n.pt | Fastest | Lower | Quick testing |
| yolov8s.pt | Fast | Moderate | Balanced |
| yolov8m.pt | Moderate | Good | **Default - Recommended** |
| yolov8l.pt | Slow | High | High accuracy |
| yolov8x.pt | Slowest | Highest | Maximum accuracy |

### Expected Training Time | 預期訓練時間

| GPU | Time (100 epochs, YOLOv8m) |
|-----|---------------------------|
| RTX 5060 | ~2-3 hours |
| RTX 3060 | ~3-5 hours |
| RTX 4090 | ~1.5-2.5 hours |
| CPU | 15+ hours (not recommended) |

### Training Output | 訓練輸出

After training, you will find | 訓練完成後：

- **Best model | 最佳模型**: `runs/jersey_detection/weights/best.pt`
- **Last model | 最新模型**: `runs/jersey_detection/weights/last.pt`
- **Training curves | 訓練曲線**: `runs/jersey_detection/results.png`
- **Confusion matrix | 混淆矩陣**: `runs/jersey_detection/confusion_matrix.png`

---

## Integration | 整合到專案

### Method 1: Integration Script | 使用整合腳本

```cmd
python integrate_model.py
```

This script will | 此腳本會：
1. Find the best model | 尋找最佳模型
2. Copy to project directory | 複製到專案目錄
3. Create usage example code | 創建使用範例

### Method 2: Manual Copy | 手動複製

```cmd
# Windows
copy runs\jersey_detection\weights\best.pt ..\volleyball-analysis\models\jersey_detection_yv8.pt

# Linux/macOS
cp runs/jersey_detection/weights/best.pt ../volleyball-analysis/models/jersey_detection_yv8.pt
```

### Usage Example | 使用範例

```python
from integration_example import JerseyNumberDetector

# Initialize detector | 初始化檢測器
detector = JerseyNumberDetector()

# Detect jersey numbers | 檢測球衣號碼
detections = detector.detect(image)
```

---

## Troubleshooting | 疑難排解

### Issue 1: CUDA Not Available | CUDA 不可用

**Symptom | 症狀**: "CUDA available: False"

**Solution | 解決方案**:
1. Verify CUDA PyTorch is installed | 確認已安裝 CUDA 版本的 PyTorch
2. Check NVIDIA drivers (`nvidia-smi`) | 確認 NVIDIA 驅動程式已安裝
3. Verify CUDA version match | 確認 CUDA 版本匹配
4. Reinstall PyTorch with correct CUDA version | 重新安裝對應版本的 PyTorch

### Issue 2: Out of Memory | 記憶體不足

**Symptom | 症狀**: "CUDA out of memory"

**Solution | 解決方案**:
1. Reduce batch size | 減少 batch size
2. Use smaller model (yolov8n.pt) | 使用較小的模型
3. Reduce image size (`imgsz`) | 減少圖片大小
4. Close other GPU applications | 關閉其他使用 GPU 的應用程式

### Issue 3: Dataset Download Failed | 下載資料集失敗

**Solution | 解決方案**:
1. Verify `ROBOFLOW_API_KEY` is set | 確認環境變數已設置
2. Check network connection | 檢查網路連接
3. Check Roboflow API quota | 確認 API 配額未用完
4. Retry download script | 重新運行下載腳本

### Issue 4: Empty Dataset | 資料集為空

**Solution | 解決方案**:
1. Run `download_datasets.py` first | 先運行下載腳本
2. Run `organize_datasets.py` | 運行組織腳本
3. Check `datasets/processed/merged/` directory | 檢查目錄下是否有文件

### Issue 5: Slow Training | 訓練速度很慢

**Solution | 解決方案**:
1. Verify GPU is being used | 確認使用 GPU
2. Increase batch size if memory allows | 增加 batch size
3. Reduce workers if data loading is bottleneck | 減少 workers 數量
4. Use smaller model for quick testing | 使用較小的模型測試

### Issue 6: Poor Model Accuracy | 模型準確度不佳

**Solution | 解決方案**:
1. Train more epochs | 訓練更多輪數
2. Use larger model (yolov8m/l) | 使用更大的模型
3. Check dataset quality and annotations | 檢查資料集品質和標註
4. Adjust learning rate and hyperparameters | 調整學習率和超參數
5. Enable data augmentation | 使用資料增強

---

## Training Monitoring | 訓練監控

### TensorBoard (Optional)

```cmd
pip install tensorboard
tensorboard --logdir runs/jersey_detection
```

Then open `http://localhost:6006` in your browser

然後在瀏覽器中打開 `http://localhost:6006`

### Training Logs | 訓練日誌

Logs saved in `runs/jersey_detection/` | 訓練日誌保存於 `runs/jersey_detection/`：

- `results.csv` - Metrics per epoch | 每個 epoch 的指標
- `train_batch*.jpg` - Training batch visualization | 訓練批次視覺化
- `val_batch*.jpg` - Validation batch visualization | 驗證批次視覺化

---

## Important Notes | 注意事項

1. **Environment**: Conda recommended for GPU training (better CUDA dependency management)
2. **Backup**: Ensure 20GB+ disk space before training
3. **Resume**: Training can resume from checkpoint with `resume=True`
4. **Model Selection**: Larger models are more accurate but slower
5. **GPU Memory**: Training script auto-optimizes parameters
6. **Dataset Quality**: Correct annotations directly impact model performance

**環境選擇**: 推薦使用 Conda（可更好管理 CUDA 依賴）  
**備份**: 訓練前確保有 20GB+ 磁碟空間  
**中斷恢復**: 可從檢查點恢復訓練  
**模型選擇**: 較大模型更準確但訓練更慢  
**資料集品質**: 正確的標註直接影響模型性能

---

## References | 參考資料

- [YOLOv8 Documentation](https://docs.ultralytics.com/)
- [Roboflow Universe](https://universe.roboflow.com/)
- [PyTorch Installation Guide](https://pytorch.org/get-started/locally/)

## License | 授權

MIT License

## Contributing | 貢獻

Issues and Pull Requests are welcome! | 歡迎提交 Issue 或 Pull Request！

---

*Part of [DL-Volleyball-Analysis](https://github.com/DL-Volleyball-Analysis) - Senior Capstone Project*

*National Taiwan Ocean University - Department of Computer Science*
