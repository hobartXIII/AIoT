# Quiz 1 監督式學習演算法

來源：AIoT-DA「AIoT L8 Quiz 1 準備」（Notion，2026.10.28 版）。頁面內容是一段和 ChatGPT 的對話：先列出監督式學習十大演算法，再請它出題，最後畫成資訊圖表。

## 考試範圍

頁面開頭寫：**Supervised learning only, topics 8 and 10 are not included**（只考監督式學習，第 8、10 項不考）。

| 位置 | 不考的項目 |
|---|---|
| 資訊圖表（「五個核心思維模式」下方的圖） | **#8 人工神經網路（MLP／DNN）**、**#10 線性判別分析（LDA）** |
| Top 10 Algorithms 表格 | **#9 Neural Networks（MLP）** |

兩邊的編號不一樣，但不考的其實都是 MLP，再加上資訊圖表裡的 LDA。

## 會考的項目

會考的演算法有 9 個：Top 10 表格和資訊圖表的項目合起來，扣掉 MLP 和 LDA。

### 依任務類型分

| 類型 | 演算法 | Python |
|---|---|---|
| **只能回歸**（預測連續值） | 線性迴歸（Linear Regression） | `LinearRegression` |
| | Ridge／Lasso 迴歸 | `Ridge`、`Lasso` |
| **只能分類**（預測類別） | 邏輯迴歸（Logistic Regression） | `LogisticRegression` |
| | 朴素貝氏（Naive Bayes） | `GaussianNB`、`MultinomialNB` |
| **回歸、分類都能做** | 決策樹（Decision Tree） | `DecisionTreeClassifier`、`DecisionTreeRegressor` |
| | 隨機森林（Random Forest） | `RandomForestClassifier`、`RandomForestRegressor` |
| | 支援向量機（SVM／SVR） | `SVC`、`SVR` |
| | K 近鄰（KNN） | `KNeighborsClassifier`、`KNeighborsRegressor` |
| | 梯度提升（XGBoost／LightGBM） | `XGBClassifier`、`XGBRegressor` |

### 依方法原理分

| 家族 | 演算法 | 核心思想 |
|---|---|---|
| **線性模型** | 線性迴歸 | 最小化平方誤差（OLS） |
| | Ridge／Lasso | 線性迴歸加上正則化（Ridge 用 L2，Lasso 用 L1） |
| | 邏輯迴歸 | 線性組合經過 Sigmoid 轉成機率 |
| **距離／實例** | KNN | 取最近的 k 個樣本，分類用投票、回歸取平均，不需要訓練 |
| **間隔最大化** | SVM | 找間隔最大的超平面，可用核技巧處理非線性 |
| **機率模型** | 朴素貝氏 | 貝氏定理，假設特徵之間條件獨立 |
| **樹模型** | 決策樹 | 依特徵切分資料，用資訊增益或 Gini 選切點 |
| **集成（Ensemble）** | 隨機森林 | 很多棵樹做 Bagging，降低變異 |
| | 梯度提升 | 用 Boosting 一棵一棵加樹，每棵擬合上一輪的殘差 |

### 資訊圖表上的共通觀念（也可能考）

- **評估指標**
  - 分類：Accuracy、Precision、Recall、F1、ROC-AUC、Confusion Matrix
  - 回歸：MSE、RMSE、MAE、R²
- **學習流程**：帶標籤資料 → 選演算法並設定超參數 → 訓練 → 評估與預測
- **實務要點**：資料前處理（缺值、編碼、標準化）、交叉驗證、超參數搜尋（Grid／Random／Bayesian）、類別不平衡

### 不考

人工神經網路（MLP／DNN）、線性判別分析（LDA）

## 單元重點

- 監督式學習（Supervised Learning）用**有標籤**的訓練資料，學出從特徵 X 到目標 y 的對應關係
- 兩大類：**回歸**（Regression，預測連續值）、**分類**（Classification，預測類別）
- 用 prompt 讓 AI 產生題庫與資訊圖表，當作考前複習

## 監督式學習十大演算法

| 排名 | 演算法 | 類型 | Python（scikit-learn） |
|---|---|---|---|
| 1 | Linear Regression | 回歸 | `LinearRegression` |
| 2 | Logistic Regression | 分類 | `LogisticRegression` |
| 3 | Decision Tree | 兩者 | `DecisionTreeClassifier`、`DecisionTreeRegressor` |
| 4 | Random Forest | 兩者 | `RandomForestClassifier`、`RandomForestRegressor` |
| 5 | Support Vector Machine（SVM） | 兩者 | `SVC`、`SVR` |
| 6 | K-Nearest Neighbors（KNN） | 兩者 | `KNeighborsClassifier`、`KNeighborsRegressor` |
| 7 | Naive Bayes | 分類 | `GaussianNB`、`MultinomialNB` |
| 8 | Gradient Boosting（XGBoost） | 兩者 | `XGBClassifier`、`XGBRegressor` |
| 9 | ~~Neural Networks（MLP）~~（不考） | 兩者 | `MLPClassifier`、`MLPRegressor` |
| 10 | Ridge / Lasso Regression | 回歸 | `Ridge`、`Lasso` |

- 這是教學用的排名，不是效能排名。
- XGBoost 來自獨立的 `xgboost` 套件，不在 scikit-learn 裡。
- **#9 Neural Networks（MLP）不考**：老師在頁面上把它劃掉了。

## 題庫 prompt

L8 新增的 prompt：

````text
List top 10 topics for machine learning supervised learning
````

````text
List top 20 problems for <Linear Regression>, simple choice with answer emphasized and explanation
````

````text
五個核心思維模式： 這個領域所有專家都認同的五個核心思維模式是什麼？
````

````text
十個深度測驗問題： 生成 10 個問題以判斷一個人是真正深入了解，還是死記硬背。
````

原本的 prompt：

````text
LIST top 10 algorithms of supervised learning
````

````text
list 100 problems for 1,2,3 topics , list the most important and popular for reasearch and INTERVIEW , 選擇題 with answers (選項粗體)
````

````text
USE cHINESE question item if answer , 粗體,
````

````text
for supervised learning algorithm essence , draw 資訊圖表 a4, 直式 高資訊密度
````

ChatGPT 確認的題目格式：題目與選項用繁體中文、英文術語放括號、正確選項加粗，每題附答案與解析。頁面只留了前兩題當格式範例：

1. 線性迴歸（Linear Regression）的主要目標是什麼？
   - A. 最大化分類準確率（Accuracy）
   - **B. 最小化殘差平方和（Sum of Squared Errors, SSE）**
   - C. 最大化輸入特徵的變異數
   - D. 最小化模型使用的特徵數量

   解析：普通最小平方法（OLS）透過最小化實際值與預測值的平方誤差，求得最佳迴歸係數：$\beta^* = \arg\min_\beta \sum_{i=1}^{n}(y_i - \hat{y}_i)^2$

2. 下列何者是多元線性迴歸（Multiple Linear Regression）的數學模型？
   - A. $\hat{y} = \sigma(w^T x + b)$
   - **B. $\hat{y} = b + w_1x_1 + w_2x_2 + \cdots + w_px_p$**
   - C. $\hat{y} = \arg\max P(y \mid x)$
   - D. $\hat{y} = \log(x)$

   解析：多元線性迴歸用多個輸入特徵的線性組合預測連續數值。

頁面沒有放完整的 100 題。

## 資訊圖表：監督式學習演算法精華總覽

位置：頁面上「五個核心思維模式」下方的圖。**#8 人工神經網路、#10 LDA 不考**。

A4 直式圖表，由上而下：

- **目標**：從帶標籤資料學習映射 X → y，用於分類或回歸
- **流程**：帶標籤資料 (D, (X, y)) → 選擇演算法並設定超參數 → 訓練模型 f(X; θ) → 評估與預測 ŷ = f(X_new)
- **常用評估指標**
  - 分類：Accuracy、Precision、Recall、F1-score、ROC-AUC、Confusion Matrix
  - 回歸：MSE、RMSE、MAE、R²

| # | 演算法 | 類型 | 核心思想 | 優點 | 缺點 | 適用 |
|---|---|---|---|---|---|---|
| 1 | 線性迴歸 | 回歸 | 假設輸入與輸出呈線性關係，最小化平方誤差 | 簡單、可解釋 | 只能建模線性關係，容易受異常值影響 | 連續數值預測、特徵與目標近似線性 |
| 2 | 邏輯迴歸 | 分類 | 用 Sigmoid 把線性組合轉成機率 | 簡單高效、可解釋、輸出為機率 | 線性邊界、對特徵尺度敏感 | 二元／多類分類基線模型 |
| 3 | KNN | 分類／回歸 | 依最近 k 個樣本投票（分類）或平均（回歸） | 簡單直觀、無訓練過程 | 預測慢、需儲存全部資料、對特徵尺度敏感 | 小型資料集、非線性關係 |
| 4 | SVM | 分類／回歸（SVR） | 找最大間隔的超平面；SVR 用 ε-不敏感損失 | 高維有效、泛化能力強、可用核技巧處理非線性 | 訓練時間長、需調參數（C、γ） | 中小型資料集、高維與複雜邊界 |
| 5 | 決策樹 | 分類／回歸 | 用特徵切分資料，形成樹狀規則（資訊增益、Gini） | 可解釋、可處理非線性、不需特徵縮放 | 容易過擬合、對資料小變動敏感 | 分類與迴歸、特徵型態多樣 |
| 6 | 隨機森林 | 分類／回歸 | 多棵決策樹的集成，用 Bagging 降低變異 | 準確度高、抗過擬合、可評估特徵重要性 | 模型較大、較難解釋 | 大多數實務問題 |
| 7 | 梯度提升樹（XGBoost／LightGBM） | 分類／回歸 | 逐步加入新樹擬合上一輪的殘差 | 高準確度、處理複雜資料、支援正則化與缺值 | 需調整較多超參數、訓練時間較長 | 結構化資料的競賽與實務 |
| 8 | ~~人工神經網路（MLP／DNN）~~（不考） | 分類／回歸 | 多層非線性變換學習複雜特徵表示 | 表示能力強、可建模高度非線性 | 需大量資料、難以解釋、訓練成本高 | 大規模資料、影像語音文字 |
| 9 | 朴素貝氏 | 分類 | 基於貝氏定理，假設特徵條件獨立（Gaussian／Multinomial／Bernoulli NB） | 訓練快、適合高維、小資料也有效 | 獨立假設不一定成立 | 文字分類、垃圾郵件過濾 |
| 10 | ~~線性判別分析（LDA）~~（不考） | 分類 | 找使類間差異最大、類內差異最小的投影 | 降維＋分類、可解釋 | 假設高斯分布、相同共變異數 | 類別可分性較好的資料 |

**實務選擇建議**（圖表右下）：

1. 先建立基線模型：線性迴歸／邏輯迴歸
2. 資料量小：KNN、樸素貝氏、決策樹
3. 資料量中等：SVM、隨機森林、LDA
4. 追求高效能：梯度提升樹（XGBoost／LightGBM）
5. 大規模或非結構化資料：神經網路（不考）
6. 需可解釋性：線性模型、決策樹、LDA、隨機森林

**關鍵實務要點**：資料前處理（缺值、特徵編碼、標準化）、交叉驗證（Cross-Validation）評估泛化能力、超參數調整（Grid／Random／Bayesian Search）、處理類別不平衡與異常值、根據業務目標選擇評估指標與演算法。

圖表底部還有一張「演算法特性比較」星等表（資料規模、特徵維度、非線性能力、可解釋性、訓練速度、常用場景），星等太小看不清楚，這裡不抄。
