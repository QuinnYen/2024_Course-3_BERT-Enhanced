# 情緒分析模型 (BERT)

本專案是使用 BERT 模型對 IMDB 電影評論資料集進行情緒分析。模型可以將文本分類為正面或負面情緒。

## 資料集

使用 IMDB 電影評論資料集,透過 Hugging Face 的 `datasets` 套件載入。資料集分為訓練集和測試集,我們將其合併後再分割為訓練集、驗證集和測試集,比例約為 8:1:1。

## 模型下載

由於模型檔案較大，我們將其存放在 Hugging Face Model Hub，您可以從以下連結下載：

[下載模型](https://huggingface.co/MatchaCat4477/best_BERT/resolve/main/best_BERT.pt)

下載後，請將模型檔案 `best_BERT.pt` 放在專案根目錄下。

## 模型架構

利用 BERT 預訓練模型,在此基礎上添加了分類器層,用於情緒二分類任務。模型的主要組成部分:

- BERT 基礎模型
- Dropout 層
- 線性分類層

## 訓練流程

1. 資料預處理:將文本轉換為模型可接受的輸入格式,並創建 PyTorch 資料集和資料載入器
2. 模型初始化:從預訓練的 BERT 模型初始化我們的分類模型
3. 訓練迴圈:對模型進行多個 epoch 的訓練,每個 epoch 包括:
   - 訓練階段:遍歷訓練資料,計算損失並更新模型權重
   - 驗證階段:在驗證集上評估模型性能
4. 模型儲存:訓練完成後,儲存最佳模型權重
5. 繪製學習曲線:將訓練過程中的損失和評估指標繪製成圖表,以觀察模型的學習情況

## 評估指標

使用以下指標評估模型性能:

- 準確率 (Accuracy)
- F1分數 (F1-score)
- 召回率 (Recall)
- 精確率 (Precision)

## 使用方法

1. 安裝所需套件:
   ```
   pip install -r requirements.txt
   ```

2. 訓練模型:
   ```
   python train_Bert.py
   ```
   訓練完成後,模型權重會儲存在 `outputs` 目錄下。
   
3. 使用訓練好的模型進行預測:
   ```
   python predict.py
   ```
   輸入要分析的文本,模型會給出情緒預測結果和信心度。

## 程式架構

- `train_Bert.py`:模型訓練主程式
- `predict.py`:使用訓練好的模型進行情緒預測
- `outputs/`:儲存訓練過程中的模型權重、預測結果、圖表等輸出內容
- `requirements.txt`:所需的 Python 套件列表

## 預測結果範例

模型輸出會包含以下資訊：
- 預測的情感傾向（正面/負面）
- 預測的信心度
- 完整的機率分布

例如：
```
預測結果：負面
信心度：96.90%
完整機率分布：
負面：96.90%
正面：3.10%
```

## 注意事項

1. 確保您有足夠的硬碟空間（模型檔案約 450MB）
2. 建議使用 GPU 進行預測，可以大幅提升處理速度
3. 第一次執行時會下載 BERT 基礎模型，需要網路連接

## 錯誤排除

如果遇到 "RuntimeError: Expected all tensors to be on the same device" 錯誤，請確保：
1. 檢查 CUDA 是否可用
2. 確認所有張量都在同一個設備上（CPU 或 GPU）

如果有任何問題，歡迎提出 Issue。
