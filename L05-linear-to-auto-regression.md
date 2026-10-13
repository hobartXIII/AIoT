# L05 From Simple Linear Regression to Auto Regression 自回歸股票預測（2026.9.30）

來源：AIoT-DA L5（Notion）

## 單元重點

- 學習 Colab 工具
- 用 CRISP-DM 模板解 Simple Linear Regression（先了解分析方法）
- 參考：scikit-learn supervised learning — https://scikit-learn.org/stable/supervised_learning.html

## CRISP-DM 五步驟

1. Business Understanding / Data Loading
2. Data Preprocessing
3. Model Selection
4. Model Evaluation
5. Model Deployment

## 講義

- `ML_資料分析模板L5_Linear_Regression_all_(ans)v2.ipynb`
- `AIoT L9 Linear regression.pdf`（Simple Linear Regression 數學推導）
- `Scikit_Learn_Cheat_Sheet_Python.pdf`（圖：sklearn 六大類 — Classification、Regression、Clustering、Dimensionality reduction、Model selection、Preprocessing）
- `mydata.csv`、`mydata_multiple.csv`
- 參考書：《Python 資料科學自學聖經》（Colab、Numpy/Pandas、爬蟲、Matplotlib/Seaborn、資料預處理、sklearn 監督/非監督、DNN/CNN/RNN、台股預測、遷移學習、調參）

## Linear Regression 的各種形式（含 sklearn 範例）

```python
import numpy as np
from sklearn.linear_model import LinearRegression, Ridge, Lasso, ElasticNet
from sklearn.preprocessing import PolynomialFeatures
```

| # | 方法 | 說明 | 關鍵程式 |
|---|---|---|---|
| 1 | 簡單線性回歸 | 一個自變數，線性關係明顯時用 | `LinearRegression().fit(X, y)` → `intercept_`, `coef_` |
| 2 | 多元線性回歸 | 多個特徵 | 同上，`X` 有多欄 |
| 3 | 多項式回歸 | 把 X 做多項式轉換，捕捉非線性 | `PolynomialFeatures(degree=2).fit_transform(X)` 再丟進 `LinearRegression` |
| 4 | 嶺回歸 Ridge | **L2 正則化**，減少過擬合，適合多重共線性 | `Ridge(alpha=1.0)` |
| 5 | 套索回歸 Lasso | **L1 正則化**，部分係數變 0 → 可做特徵篩選 | `Lasso(alpha=0.1)` |
| 6 | 彈性網路 Elastic Net | L1 + L2 結合 | `ElasticNet(alpha=0.1, l1_ratio=0.5)` |
| 7 | 自迴歸 AR | 用時間序列過去的值預測未來（股價、氣象） | `statsmodels` 的 `AutoReg(y, lags=2)` |

自迴歸範例：

```python
from statsmodels.tsa.ar_model import AutoReg

y = [112, 118, 132, 129, 121, 135, 148, 148, 136, 119]
model_fit = AutoReg(y, lags=2).fit()
print(model_fit.summary())
pred = model_fit.predict(start=len(y), end=len(y) + 2)  # 預測未來 3 期
```

## 數學概念：最小平方法 + 梯度下降

（圖：每個資料點與回歸線之間的誤差平方 — 讓這些正方形面積總和最小。）

成本函數（MSE）：

$$J(a,b) = \frac{1}{n}\sum_{i=1}^{n}\big(y_i - (a x_i + b)\big)^2$$

梯度：

$$\frac{\partial J}{\partial a} = -\frac{2}{n}\sum x_i\big(y_i - (a x_i + b)\big), \quad \frac{\partial J}{\partial b} = -\frac{2}{n}\sum \big(y_i - (a x_i + b)\big)$$

更新：$a \leftarrow a - \alpha \frac{\partial J}{\partial a}$，$b \leftarrow b - \alpha \frac{\partial J}{\partial b}$（$\alpha$ 是 learning rate）

```python
x = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
y = np.array([2, 3, 5, 7, 11, 13, 17, 19, 23, 29])
a, b, alpha, n = 0, 0, 0.01, len(x)
for _ in range(1000):
    y_pred = a * x + b
    da = (-2 / n) * np.sum(x * (y - y_pred))
    db = (-2 / n) * np.sum(y - y_pred)
    a -= alpha * da
    b -= alpha * db
# 結果約 a ≈ 2.93, b ≈ -3.21 → y = 2.93x - 3.21
```

推薦閱讀：「简单线性回归模型」、「线性回归及梯度下降」。

## 練習 Prompt

**Prompt 1：讀 CSV 做線性回歸**
> Write a complete Python script that reads a CSV file named "mydata.csv", uses the first column as input and the second column as output, performs linear regression, prints the model parameters, evaluates the model with MSE and R², and plots the results.

**Prompt 2：合成資料（用 Colab + CRISP-DM）**
> write python to solve linear regression problem.
> generate 200 random points (x,y) x:-100 to 100, y = 10x + 50 + 50·N(0,1), draw scatter plot with points in blue
> use linear regression in sklearn to get a, b and draw regression line in red
> apply CRISP-DM steps
> model evaluation, to compute MSE, RMSE, R2

## 線性回歸 + Outlier（學生優秀作業範例）

- 檔案：`hw3.ipynb`、`hw3.pdf`
- 圖：200 個藍點 + 紅色回歸線，偏離最遠的約 20 個點用紅圈標出並編號（outlier 偵測）。

## 自回歸練習：台積電（2330）股票預測

- 參考：https://weikaiwei.com/finance/stocker/
- 資料：`2330.csv`；成果範例：`2330_prediction.ipynb`

Prompt：
> write python to solve multiple linear regression problem use SVR algorithm in sklearn,
> 1. read 2330.csv
> 2. y is current price, x1 to x5 is historical price with time lags
> 3. write python to predict y from x1 to x5
> 4. apply CRISP-DM 6 steps
> 5. add in section 5, draw original data and prediction data
> 6. COMPARE THE RESULT WITH multiple linear regression

接著用「參考這張圖片, modify my code to generate similar figure」讓 AI 模仿目標圖。

**圖片重點**：
- 目標圖：藍線是實際股價，紅色虛線是訓練/預測分界，右側黃線是預測值並帶信賴區間。
- **「喇叭圖」**：預測區間越往未來越寬，形狀像喇叭（信賴區間隨時間擴大）。可用 **Prophet** 套件畫：
  - 官方快速入門：https://facebook.github.io/prophet/docs/quick_start.html
  - 範例 Colab：https://colab.research.google.com/drive/15Eq_067KKDDwlb4awMp6XPcjOZzVaHVr?usp=sharing
  - 喇叭怎麼畫出來的：https://github.com/facebook/prophet/issues/637

## 其他

- LR pseudocode（LaTeX）：`test.pdf`
