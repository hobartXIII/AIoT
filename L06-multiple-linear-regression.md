# L06 Multiple Linear Regression（Startup 50 on Kaggle）with ChatGPT 開發（2026.9.30）— DIC-2 股票預測

來源：AIoT-DA L6（Notion）

## 單元重點

- 學習用 ChatGPT 協作開發
- 用 CRISP-DM 模板解多元回歸問題
  - **2330.TW 股價預測**（auto regression）→ DIC-2
  - **Startup 50 on Kaggle**（multiple linear regression）

## CRISP-DM 步驟細節

1. **Load Data**：載入資料，並產生隨機資料畫散佈圖
2. **Preprocessing**：nominal 特徵做 one-hot encoding；理解 train/test split 的必要性；型別轉換與 reshape
3. **Build Model**
4. **Evaluation**：MSE、MAE、R² 的意義；用 training / test 曲線判斷 overfit / underfit；用 **Optuna** 優化模型
5. **Deployment**：用 gzip、pickle 儲存模型

### 套用到 50 Startups

| 階段 | 內容 |
|---|---|
| Business Understanding | 用 R&D、Administration、Marketing 支出與州別預測新創公司利潤；成功標準：R² 高、MAE/MSE/RMSE 低 |
| Data Understanding | 讀 `50_Startups.csv`，用 `head()`、`columns` 檢查結構，檢查缺值、離群值、型別 |
| Data Preparation | `State` 用 OneHotEncoder + ColumnTransformer 編碼（並避免共線性），`train_test_split` 切訓練/測試集 |
| Modeling | `LinearRegression` |
| Evaluation | R²、MAE、MSE、RMSE |
| Deployment | 部署、監控、文件化 |

## 資料與資源

- Kaggle 範例：https://www.kaggle.com/code/akash17/50-startups-linearregression
- Raw CSV：https://gist.githubusercontent.com/GaneshSparkz/b5662effbdae8746f7f7d8ed70c42b2d/raw/faf8b1a0d58e251f48a647d3881e7a960c3f0925/50_Startups.csv
- iT 邦範例說明：https://ithelp.ithome.com.tw/m/articles/10299081
- 講義：`Lecture 3 class note.pdf`、`機器學習-線性回歸使用chatGPT.pdf`
- 練習 / 解答：`Linear_regression_using_chatGPT(practice).ipynb`、`Linear_regression_using_chatGPT(ans).ipynb`
- Notebook：
  - `50_Startup_Problem(_initial_template_and_question).ipynb`
  - `50_Startup_Problem_(Partial Answer) to step4.ipynb`
  - `L7_50_startup(Lasso_ok).ipynb`
  - `L7_50_startup＿多元回歸_(complete_prompt_and_code修正).ipynb`
  - `(O)L11=50Start_up.ipynb`（Part 1、Part 2 的最終程式）

## 課程活動

### Part 1 複習：回歸演算法類型

- 簡單線性回歸（感測器校正、分數調整 → calibration problem）
- Polynomial regression
- Auto regression（台積電股價預測）
- 多元線性回歸（Kaggle 50 Startups、Boston housing 房價預測）

### Part 2 與 ChatGPT 互動

下載 initial template 後繼續請 ChatGPT 修改。常用指令：

- `give me complete prompt that generate the above code`
- `give me complete code`
- `give me complete prompt again`
- `modify the code in step 3 by adding plot`
- `fix the error`
- `rewrite the code by using train_test_split function`

五步驟 prompt 範本：
> Generate Python code to solve the Kaggle … Regression problem using the following 5 steps:
> Step 1: Import libraries and download the dataset using a web crawler (URL …). Convert CSV to a pandas DataFrame and print a summary.
> Step 2: Load the dataset and explore its contents.
> Step 3: Separate features (X) and target (y). Split train/test with test size 20%.
> Step 4: Initialize a Linear Regression model and fit it.
> Step 5: Predict on test data, calculate and print MSE.

### Part 3 Step 2–4 說明

- Data Preparation：填補空值、`train_test_split`
- 多元回歸模型
- 評估指標：MSE、MAE、R²

**R² 公式（圖）**：

$$R^2 = 1 - \frac{\sum_i (y_i - \hat{y}_i)^2}{\sum_i (y_i - \bar{y})^2}$$

- 壞模型：誤差跟「直接猜平均值」差不多 → R² 接近 0
- 好模型：誤差遠小於猜平均值 → R² 接近 1

### Part 4 多元回歸的前提假設

1. **Linearity**：每個 xᵢ 與 y 之間有線性關係
2. **Homoscedasticity**：誤差變異數一致
3. **Multivariate normality**：常態分布
4. **Independence of errors**：誤差彼此獨立
5. **Lack of multicollinearity**：特徵之間不可有共線性

### Part 5 如何挑出有效的特徵

實務上無法逐一檢驗上面五個假設，所以用工程方法挑特徵組：

- **All-in**：你是領域專家、確定滿足假設時才用；特徵上百個時很難保證
- **Stepwise Selection（貪婪法）**
  - **Backward selection（最常用）**：用 p-value 篩選（< 0.05 顯著，< 0.01 非常顯著）
  - Forward selection
  - Bidirectional selection
- **Score selection（特徵組合最佳化）**：10 個特徵有 2¹⁰ = 1024 種組合；暴力法（sklearn Grid Search）可找到最佳但太慢，可改用 meta-heuristic 搜尋（Hill climbing、GA、PSO、SA、TS），參考聯合大學陳士杰 http://debussy.im.nuu.edu.tw/sjchen/ML_final.html
- **Lasso** / `sklearn.feature_selection.SelectFromModel`

**類別資料編碼**：
- **One-Hot Encoding**（1 → n）：產生 n 個 dummy variables
- **Dummy variable trap**：n 個 dummy 全放會線性相依（xₙ = 1 − x₁ − … − xₙ₋₁）→ 只能放 **n−1 個**（`drop_first=True`）
- **Label Encoding**（1 → 1）
- 看 p-value 的範例：https://stackoverflow.com/questions/27928275/find-p-value-significance-in-scikit-learn-linearregression

## 50 Startups 資料集

欄位：`R&D Spend`, `Administration`, `Marketing Spend`, `State`（New York / California / Florida）, `Profit`（目標）

## 結果

### Part 1：CRISP-DM 完整程式（重點）

```python
ct = ColumnTransformer([('encoder', OneHotEncoder(), [3])], remainder='passthrough')
X = ct.fit_transform(data.drop('Profit', axis=1))
X = X[:, 1:]                      # 避免 dummy variable trap
y = data['Profit']
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=0)
regressor = LinearRegression().fit(X_train, y_train)
y_pred = regressor.predict(X_test)
# r2_score, mean_absolute_error, mean_squared_error, RMSE = sqrt(MSE)
# 畫 Actual vs Predicted Profit 散佈圖
```

### 多種回歸模型比較（圖）

以 R&D Spend 為 x 軸，比較 LR、Lasso、Ridge、ElasticNet、Decision Tree、Random Forest、Gradient Boosting、SVR 等模型的擬合曲線；樹模型呈階梯狀，線性模型接近直線。

### Part 2：Forward selection 特徵選擇

做法：每一輪從剩下的特徵中挑「加進去後 RMSE 最小」的那一個，跑 1–5 個特徵；`State` 用 `pd.get_dummies(..., drop_first=True)`；最後用 `tabulate` 輸出 Markdown 表。

發現：**只有 R&D Spend 最有效**。

| 特徵數 | 選到的特徵 | RMSE | R² |
|---|---|---|---|
| 1 | R&D Spend | 8274.87 | 0.9465 |
| 2 | R&D Spend, Marketing Spend | 8198.80 | **0.9474** |
| 3 | + State_New York | 8309.06 | 0.9460 |
| 4 | + State_…（再加一個州別欄） | 8409.92 | 0.9447 |
| 5 | + …（原頁面截斷） | 9137.99 | 0.9347 |

→ 特徵不是越多越好；2 個特徵時表現最好，加更多反而變差。

## 本章重點回顧

- ChatGPT 是生成式 AI：ChatGPT 生成文字、Stable Diffusion 生成圖像
- Linear regression 數學推導見講義

## 附錄：理論補充

- Lasso 和 Ridge 的超參數調整技巧（知乎）
- Startup Profit Prediction ML App: Step By Step Guide
- **K-Fold Cross-Validation**：資料切成 k 份輪流當測試集
  - **Repeated K-Fold**：重複做 n 次 k-fold，每次重新洗牌（k=2、n=2 → 訓練 4 次）
  - **Stratified K-Fold**：每個 fold 依類別比例抽樣；測試集更能代表整體、對不平衡資料有用，主要用在分類問題
  - sklearn：`RepeatedKFold`、`StratifiedKFold`、`StratifiedShuffleSplit`

## 延伸頁面（原頁面子頁，未展開）

- AI'25-L6B Resume
- 50 STARTUP Kaggle Report
