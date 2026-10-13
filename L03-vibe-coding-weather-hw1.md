# L03 AI vibe coding 天氣預報（2026.9.23）— HW1

來源：AIoT-DA L3 與子頁「Part 5. AI Vibe coding 天氣預測 Forecast with CWA API」（Notion）

## 課程內容

- 範例網站：https://airbox.edimaxcloud.com/
- 參考成品：https://taiwan-weather-map.vercel.app/
- 資料來源：中央氣象署（CWA）OpenData，老師建議用資料集 **O-A0003-001**（自動氣象站觀測）
- API key：自己到 CWA OpenData 申請（原頁面貼有老師的 key，這裡不抄錄）

## 圖片補充 1：Vibe Coding 個人網站流程（DIC-1）

標題：Lecture 2 Vibe Coding — Antigravity × GitHub × OpenSpec，用 AI 寫程式打造屬於你的網站。

1. **建立 GitHub Repository**（例如 `YourName_PersonalPage`），取得 Repo URL
2. **連接 Antigravity**：Connect to GitHub → Select repository → Open in workspace
3. **告訴 AI 你的需求**，例如：
   > Create a personal website for me. Include profile, skills, projects, and a real-time clock. Use HTML, CSS and JavaScript. Make it modern and responsive.
4. **AI 生成網站**：`index.html`（結構）、`style.css`（外觀）、`app.js`（功能）、`README.md`
5. **本機測試**：`python -m http.server 5173` → 開 `http://localhost:5173`
6. **Vibe Coding 持續優化**：改配色、加照片、加內容、手機版、互動效果、修錯 — Prompt → Build → Test → Improve
7. **Push 到 GitHub**：`git add .` → `git commit -m "Update"` → `git push`
8. **Deploy 上線**：GitHub Pages（Settings → Pages → Deploy from a branch → main / root → Save）

**網站必備內容（DIC-1）**：Profile（姓名、照片、簡介）、Skills（至少 3 項）、Projects（至少 1 個作品）、Live Clock（JavaScript 即時時鐘）、Personal Design（自己的風格）

**繳交內容**：① GitHub Repository URL ② Live Website URL（`https://YOUR_NAME.github.io/...`）

## 圖片補充 2：Taiwan Weather Forecast 24 步流程

技術棧：**CWA API × JSON × Python × SQLite × Streamlit**

| 步驟 | 內容 |
|---|---|
| 1–3 | 課程介紹、台灣天氣與生活、註冊 CWA Open Data 取得 API Key、選擇資料集 |
| 4 | 用 `requests` 呼叫 API 取得 JSON |
| 5–6 | 解析 JSON 結構（`locations` → `weatherElement` → `MinT` / `MaxT`），提取最高與最低氣溫 |
| 7 | 用 Pandas 整理資料（regionName、dataDate、mint、maxt） |
| 8–9 | 建立 SQLite 資料庫 `data.db`，資料表 `TemperatureForecasts`（id, regionName, dataDate, mint, maxt） |
| 10 | 用 SQL 驗證：`SELECT DISTINCT regionName FROM TemperatureForecasts;` |
| 11–12 | Streamlit 入門；用 `sqlite3.connect` + `pd.read_sql_query` 從資料庫讀資料 |
| 13–16 | 下拉選單選地區、畫一週最高/最低氣溫折線圖、顯示資料表格、整合成 Web App |
| 17–19 | 進階：用 Folium + Streamlit 畫台灣地圖（依溫度分色：<20、20–25、25–30、>30°C）、選日期顯示地圖、完整 Dashboard |
| 20 | 程式碼品質：結構清晰、錯誤處理、不重複插入、註解 |
| 21 | 上傳 GitHub（建立 Repository、連結 remote、commit & push） |
| 22–24 | 延伸應用（天氣提醒 Line Bot、旅遊建議、農業防災）、回顧、下一步 |

## 圖片補充 3：用 Antigravity × Gemini × GitHub 打造 AI Coding Agent

1. 下載 Antigravity IDE（antigravity.google，Windows / Mac / Linux）
2. 啟動並使用內建 Gemini
3. 建立專案資料夾，請 AI 先規劃結構（`.venv`、`data`、`src`、`app.py`、`README.md`）
4. 建立 GitHub Repository（例如 `HW1-Taiwan-Weather`）
5. 取得 Git Repository URL（HTTPS）
6. 讓 AI 連到 GitHub：把 Repo URL 告訴 Antigravity，請它做 git 初始化與連結
7. 開始 Vibe Coding：例如「We are going to build a Taiwan weather forecast web app using Python, CWA API, SQLite and Streamlit. Please create a step-by-step plan first.」→ 讓 AI 先規劃再實作
8. 開發 → 測試 → 儲存：`streamlit run app.py`，沒問題就請 AI commit & push
9. 持續迭代優化（加地圖、圖表）
10. 完成專案

## HW1 設計文件重點：CWA 溫度 + Windy 地圖

### 架構

```
CWA OpenData → FastAPI（抓取、正規化、驗證、快取）→ 前端 (React/Next.js)
                                                   ├─ Windy Map Forecast API（天氣背景圖）
                                                   └─ Leaflet overlay（CWA 測站溫度標記）
```

- **Windy** 只當天氣背景層；Windy 地圖本身就是 Leaflet map instance。
- **Leaflet** 負責畫自訂的測站標記、popup、heatmap。
- 注意：Windy 的 `temp` 圖層是模型溫度，CWA overlay 是實際測站觀測，不要混淆。

### CWA 資料欄位（每小時更新）

`StationName`, `StationId`, `DateTime`, `StationLatitude`, `StationLongitude`, `CountyName`, `TownName`, `AirTemperature`, `RelativeHumidity`, `WindSpeed`, `WindDirection`, `Precipitation`, `AirPressure`…

### 後端 API

| Endpoint | 用途 |
|---|---|
| `GET /api/temperature/latest` | 所有測站最新觀測 |
| `GET /api/temperature/geojson` | GeoJSON 格式給 Leaflet |
| `GET /api/temperature/stations/{station_id}` | 單站詳細 |
| `GET /api/health` | 健康檢查、快取狀態 |

### 資料驗證

排除：缺經緯度、缺溫度、溫度無法轉數字、溫度 < -20°C 或 > 50°C、缺站號、時間無效。
無效值例如 `"", "X", "NA", "-99", "-999"`。

### 前端重點

- 先載入 Leaflet 1.4.x，再載入 Windy 的 `libBoot.js`，用 `windyInit(options, callback)` 初始化（中心約 lat 23.7, lon 121.0, zoom 7）。
- 用 `windyAPI.store.set("overlay", "wind" | "temp" | "rain" | "clouds")` 切換背景圖層。
- 測站用 `L.circleMarker` 依溫度上色，點擊顯示 popup（站名、縣市、溫度、濕度、風速、時間）。
- 溫度色階：<10 / 10–15 / 15–20 / 20–25 / 25–30 / 30–35 / >35°C
- 自動更新：後端每 10 分鐘、前端每 5 分鐘；畫面顯示最後 CWA 更新時間。

### 安全

- CWA key 只放後端（`.env`），不要放前端、不要 commit；提供 `.env.example`。
- Windy key 是前端可見的瀏覽器 key。

### 開發階段

1. **MVP**：latest API、台灣中心 Windy 地圖、測站標記、圖例、popup、手動刷新
2. **Dashboard**：自動刷新、縣市篩選、測站搜尋、圖層切換
3. **進階視覺化**：heatmap、時間軸、歷史回放、警示門檻
4. **Production**：Redis、PostGIS、rate limit、監控、部署

### MVP 驗收條件

開得到 Windy 地圖且以台灣為中心、測站標記依溫度變色、點擊有詳細資訊、顯示最新觀測時間、可刷新、後端隱藏 CWA key、缺資料時前端不當機、附安裝說明。
