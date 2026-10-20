# L07 Logistic Regression（分類問題）（2026.10.14 UPDATE）

來源：AIoT-DA L7（Notion）

## 單元重點

- 什麼是分類問題：用 Excel 說明二元分類，`x = rand()*100`，`y = 0 或 1`，畫圖
- 在特徵空間中找 **hyperplane** 當作分類的決策依據（上面的一維問題，就是在找 dim = 0 的分界點）
- 用 Logistic Regression 解分類問題，以買 iPhone 為例：
  - x = 口袋裡的錢
  - z = 買 iPhone 的慾望（x 的線性函數）
  - p = 買 iPhone 的機率（z 經過 sigmoid function）
- cost function J 是什麼
- Confusion matrix 與其他評估指標

## 課前準備

- 開 Google Meet、登入 iLearning／Google Drive 下載講義
- 講義：`Scikit_Learn_Cheat_Sheet_Python.pdf`、`ML_資料分析模板L5_Linear_Regression_all_(ans)v2.ipynb`、`mydata.csv`、`mydata_multiple.csv`

## 課程內容

### Part 1 複習線性回歸

- 講義：`Scikit_Learn_Cheat_Sheet_Python.pdf`
- 機器學習的分類（課堂手繪圖）：

| 類型 | 資料 | 任務 | y |
|---|---|---|---|
| Supervised Learning | x, y | Regression 迴歸 | 連續（continuous） |
| Supervised Learning | x, y | Classification 分類 | 離散（discrete） |
| Unsupervised Learning | 只有 x | Cluster 分群 | — |

- scikit-learn 首頁的六大類：Classification、Regression、Clustering、Dimensionality reduction、Model selection、Preprocessing

| 類別 | 用途 | 應用 | 演算法 |
|---|---|---|---|
| Classification | 判斷物件屬於哪一類 | 垃圾郵件偵測、影像辨識 | Gradient boosting、nearest neighbors、random forest、logistic regression |
| Regression | 預測連續值 | 藥物反應、股價 | Gradient boosting、nearest neighbors、random forest、ridge |
| Clustering | 自動把相似的物件分組 | 客群分析、實驗結果分組 | k-Means、HDBSCAN、hierarchical clustering |
| Dimensionality reduction | 減少隨機變數的數量 | 視覺化、提升效率 | PCA、feature selection、non-negative matrix factorization |
| Model selection | 比較、驗證、選擇參數與模型 | 透過參數調整提升準確度 | grid search、cross validation、metrics |
| Preprocessing | 特徵擷取與正規化 | 把文字等輸入轉成機器學習可用的資料 | preprocessing、feature extraction |

### Part 2 Logistic Regression

用 Colab 與 CRISP-DM 框架解問題。推薦推導文章：[淺入淺出瞭解機器學習_03_Logistic Regression](https://hackmd.io/@shaoeChen/Sy-uqc39D)（頁面內嵌全文，以下整理重點）。

**從階梯函數到 sigmoid**（課堂手繪圖）：

- 綠線 `z = ax + b`：線性回歸的輸出，值域是整條實數線
- 藍線 `y = u(x − c)`：理想的分類器，是在 x = c 跳階的階梯函數，輸出只有 0 和 1
- 紅線 `p = 1 / (1 + exp(−z))`：sigmoid，把 z 壓到 0～1 之間，形狀接近階梯但平滑、可微分

**重點**：

- 名字有 regression，但輸出是**類別**，屬於監督式學習的**分類**。
- 線性回歸的 hypothesis 加上 sigmoid 就變成 logistic regression：

$$
h_\theta(x) = g(\theta^T x), \qquad g(z) = \frac{1}{1 + e^{-z}}
$$

- z 趨近正無限大時 g(z) 接近 1，趨近負無限大時接近 0；z = 0 時 g(z) = 0.5。
- h(x) 是「給定 x 時 y = 1 的機率」；例如 0.51 代表 51% 是 1、49% 是 0。
- sigmoid 這種非線性轉換，在深度學習裡叫 **activation function**（啟動函數、激活函數）。
- sigmoid 兩端很平，梯度很小，優化困難；中間斜坡的梯度大，收斂快。sigmoid 的導數 $g'(z) = g(z)(1 - g(z))$ 在 z = 0 最大（課堂手繪圖：紅色鐘形曲線就是梯度）。

**Loss function：cross-entropy**（由最大似然推得）：

$$
L = -\left[\, y \log h_\theta(x) + (1 - y) \log (1 - h_\theta(x)) \,\right]
$$

$$
J(\theta) = \frac{1}{m} \sum_{i=1}^{m} L^{(i)}
$$

- y = 1 時後一項消掉，y = 0 時前一項消掉。
- 參數更新一樣用梯度下降；和線性回歸的差別只在 hypothesis 加了 sigmoid、loss 從 MSE 換成 cross-entropy。
- 鏈式法則（課堂手繪圖：x₁、x₂ 乘上 θ₁、θ₂ 加 b，經過 sigmoid）：

$$
\frac{\partial L}{\partial \theta} = \frac{\partial z}{\partial \theta} \times \frac{\partial L}{\partial z}
$$

**Decision boundary**：

- θᵀx ≥ 0 時輸出 1，否則輸出 0，所以決策邊界是 θᵀx = 0 這條線（或超平面），只能做**線性分割**。
- Andrew Ng 的例子：$h_\theta(x) = g(\theta_0 + \theta_1x_1 + \theta_2x_2)$，θ = (−3, 1, 1)，邊界是 x₁ + x₂ = 3，右上方 ŷ = 1、左下方 ŷ = 0。
- 資料不是線性可分時：換模型（MLP、SVM），或加多項式特徵；多項式越複雜越容易 overfitting，要在 high bias 和 high variance 之間權衡。

> 上面的公式是依文章文字補寫的標準式；頁面上的公式是 LaTeX 圖，文字抓取時遺失。

**補充子頁面**（CSDN 文章轉存）：

- Logistic regression 和 linear regression 都是廣義線性模型（GLM）；logistic 假設 y 服從**伯努利分布**，linear 假設 y 服從**高斯分布**。
- 推導分兩步：先找預測函數 h（sigmoid），再構造代價函數 J（預測與實際類別的偏差），最後用梯度下降求 J(θ) 的最小值。
- 文章附的程式用多項式特徵映射（`mapFeature`）、正則化係數 λ = 0.1、`scipy.optimize.fmin_bfgs` 最小化代價函數；初始 θ 全 0 時 J 約為 0.693147（= ln 2）。

### Part 3 評估分類問題的好壞

Accuracy、recall、precision 等指標各有適用時機，亂用會無法真正評估模型。核心是**混淆矩陣**（[Wikipedia](https://en.wikipedia.org/wiki/Confusion_matrix)）：

| | 預測 Positive（PP） | 預測 Negative（PN） |
|---|---|---|
| **實際 Positive（P）** | True positive（TP） | False negative（FN） |
| **實際 Negative（N）** | False positive（FP） | True negative（TN） |

總數 = P + N。轉成條件機率（O = positive、X = negative，P(預測｜實際)）：

$$
\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN} = \frac{P(O|O) + P(X|X)}{P(O|O) + P(X|X) + P(O|X) + P(X|O)}
$$

$$
\text{Selectivity} = \frac{TP}{TP + FN} = \frac{P(O|O)}{P(O|O) + P(X|O)}
$$

> 頁面把 TP / (TP + FN) 標成 Selectivity；這個式子一般叫 **Recall**（Sensitivity、TPR）。Wikipedia 的 Selectivity 是 TN / (TN + FP)（Specificity）。考試前可對照上課說明。

### 練習資料

- **AIoT 班級**（分類問題）：`grade.csv`、`predict.csv`、`answer.csv`
- **Security Problem**：[Kaggle spam mails dataset](https://www.kaggle.com/datasets/venky73/spam-mails-dataset)
- **Titanic**：
  - training：https://raw.githubusercontent.com/agconti/kaggle-titanic/master/data/train.csv
  - testing：https://raw.githubusercontent.com/agconti/kaggle-titanic/master/data/test.csv

### 適合用 Logistic Regression 的資料集（ChatGPT 產生的表格截圖）

| 問題 | 樣本數 | 特徵數 | 說明 | 特徵 | 標籤 |
|---|---|---|---|---|---|
| Titanic: Machine Learning from Disaster | ~891 | 11 | 預測鐵達尼號乘客是否生還 | 艙等、年齡、性別、票價、兄弟姊妹 | Survived（0／1） |
| Heart Disease UCI | 303 | 13 | 預測是否有心臟病 | 年齡、性別、血壓、膽固醇等 | Heart disease（0／1） |
| Bank Marketing | 41,188 | 17 | 預測客戶是否會申購定存 | 年齡、職業、婚姻、教育、餘額 | Subscription（0／1） |
| Pima Indians Diabetes Database | 768 | 8 | 預測糖尿病發病 | 懷孕次數、血糖、血壓、BMI、年齡 | Diabetes（0／1） |
| Breast Cancer Wisconsin (Diagnostic) | 569 | 30 | 判斷腫瘤良性或惡性 | 半徑、紋理、周長、面積、平滑度 | Diagnosis（0／1） |
| Loan Default Prediction | ~30,000 | ~25 | 預測貸款違約 | 貸款金額、收入、信用分數、就業 | Default（0／1） |
| Customer Churn Prediction | ~10,000 | 21 | 預測客戶是否退訂 | 使用年資、服務用量、客服通話、費用 | Churn（0／1） |
| Credit Card Fraud Detection | 284,807 | 30 | 辨識信用卡詐騙交易 | 交易金額、時間、地點、特徵 | Fraud（0／1） |
| Employee Attrition | ~30,000 | 35 | 預測員工離職 | 年齡、職位、薪資、績效 | Attrition（0／1） |
| Marketing Response Prediction | ~10,000 | 10 | 預測客戶對行銷的回應 | 年齡、性別、收入、過去購買 | Response（0／1） |

## 本章重點回顧（延伸閱讀）

- James Yeh：[資料分析&機器學習] 第3.3講：線性分類-邏輯斯回歸（Logistic Regression）介紹（Medium）
- [Tour of Evaluation Metrics for Imbalanced Classification](https://machinelearningmastery.com/tour-of-evaluation-metrics-for-imbalanced-classification/)
- 詳細數學推導：http://t.csdn.cn/0fLKG（內容整理在上方「補充子頁面」）
- iT 邦：https://ithelp.ithome.com.tw/m/articles/10269006
- Scikit-Learn `sklearn.linear_model.LogisticRegression`：https://hackmd.io/@shaoeChen/H10yw7hUb
- Stanford SLP3 第 5 章 Logistic Regression：https://web.stanford.edu/~jurafsky/slp3/5.pdf

## 附錄

- **專業網站**：machine learning crash course in 10 hours、deep learning crash course、try django 2.2（YouTube）；書：《Python 資料科學自學聖經》、《Keras 大神歸位》（Chollet，旗標 2022）、《Pytorch in Action》
- **程式設計與報告製作工具**：excalidraw（可和 VS Code 套件結合存取修改）
