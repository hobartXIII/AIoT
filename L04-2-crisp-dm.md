# L04-2 機器學習與資料分析方法 CRISP-DM（2026.9.30）

來源：AIoT-DA L4-2（Notion）

## 單元重點

- 學習 Colab 工具（文字與程式模式）
- 機器學習：回歸與分類問題
- CRISP-DM 資料分析方法與模板使用

## 課前準備

- Google Meet：https://meet.google.com/qjv-fvrx-rka
- 從 iLearning / Google Drive 下載講義：
  - `ML-資料分析模板(initial).ipynb`
  - `ML-資料分析模板(ans).ipynb`
  - `mydata.csv`

## 課程活動

- 線性回歸
- 用 CRISP-DM 框架進行資料分析的各個步驟
- 使用 Colab 的文字與程式模式

## 圖片補充

**CRISP-DM 循環**：業務理解 → 數據理解 → 數據準備 → 建模 → 評估 → 部署（循環迭代）

**範例：便利商店選址問題**

| 步驟 | 內容 |
|---|---|
| Business Understanding | 選出會賺錢的店址 |
| Data Understanding | 蒐集歷史營收、人口統計、交通流量、競爭店家、租金 |
| Data Preparation | 清理、特徵工程、整合資料 |
| Modeling | 訓練模型預測營收或成功與否 |
| Evaluation | 評估模型表現 |
| Deployment | 推薦開店地點 |

**機器學習分類**

```
Machine Learning
├─ Supervised Learning（有 x, y）
│   ├─ Regression 回歸：y 是連續值
│   └─ Classification 分類：y 是離散值
└─ Unsupervised Learning（只有 x）
    └─ Cluster 分群
```

## 資料型態（X, Y）

| 型態 | 說明 | 例子 |
|---|---|---|
| Nominal（categorical） | 類別，無順序 | 縣市、顏色 |
| Ordinal（order type） | 有順序 | 等級 |
| Interval | 可加減 | 攝氏溫度 |
| Ratio | 可加減乘除 | 重量、金額 |

延伸閱讀：
- Label encoding：https://www.mygreatlearning.com/blog/label-encoding-in-python/
- ML 常見 5 個錯誤：https://www.mygreatlearning.com/blog/5-common-mistakes-while-working-with-machine-learning-algorithms/

（圖：Data Type → Nominal 名稱型 / Numerical 數值型；數值型再分 Order only、Add Op.、Ratio Type Add/Multiply）

## 附錄

- **參考資源**：machine learning crash course in 10 hours（YouTube）、deep learning crash course（YouTube）、try django 2.2（YouTube）、《Python 資料科學自學聖經》、《Keras 大神歸位》（Chollet 著，旗標）、Pytorch in Action、Spam Detector Using Naive Bayes
- **工具**：excalidraw（畫圖）、screen task、AI for Notion、Copilot for GitHub

## CRISP-DM 步驟（本課程使用的版本）

1. Business Understanding / Data Loading
2. Data Preprocessing
3. Model Selection / Build Model
4. Model Evaluation
5. Model Deployment
