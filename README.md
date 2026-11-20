# Tomorrowland Thailand 2026 — Our Journey (V2.2)

手機優先的靜態旅遊行程網站，搭配 Google Apps Script + GitHub Actions 自動同步。**原始 Google Sheets 不會被修改。**

## 資料架構與目前同步邊界說明
- **《每日行程總覽》**：團隊行程決策的唯一人工維護基準。
- **《網站行程資料》**：網站前端時間軸專用的結構化資料表（含精確時段、地圖連結、泰文店名與地址）。**注意：目前《每日行程總覽》與《網站行程資料》之間尚未建立全自動雙向／單向同步轉譯管線**，若《每日行程總覽》有行程異動，仍需同步更新《網站行程資料》後，再由 Apps Script 輸出 `data.json`。
- **Apps Script (`Code.gs`) ➔ GitHub Actions (`sync.yml`) ➔ `data.json`**：自動化同步管線僅負責將《網站行程資料》、《人員航班與住宿分房》、《🎧 DJ 演出攻略》進行隱私過濾與結構驗證後，發布為公開 `data.json`。

## V2.1 修正重點
- **P0 語法與安全性修復**：修正 `highlightSafe()` 正規表示式跳脫語法；全面移除餐廳卡片內嵌 `onclick` 動態字串參數，改用 `data-driver-day` / `data-driver-idx` 事件委派機制；公開 `data.json` 徹底移除所有訂單編號、護照英文全名、分房名單與金額。
- **P1 資料一致性**：前端住宿總覽將 `83 Siamfandee Suites` 合併為單一 `12/14 - 12/17` 住宿卡；`12/9 Bangkok Family trip BTS Asoke` 同步標示「正式團員 8 人包棟（當晚實際共 10 人入住，含豪億與朋友）」及「房東進房與 10 人入住待確認」狀態。

## 部署順序（第三輪驗收通過後再執行）
1. 本地預覽：`python3 -m http.server 8000` 開啟 `http://localhost:8000` 測試。
2. 更新 Google Apps Script `Code.gs` 並部署新版本 Web App。
3. 推送到 GitHub `main` 分支並手動觸發 `Sync public itinerary` workflow 驗證。

## V2.2 新增重點
- **每日行程切換自動定位**：使用者點擊 Day 1–Day 9 分頁時，渲染完成後平滑捲動至對應 `DAY X` 標題並自動扣除黏附導覽列高度（支援 `prefers-reduced-motion`；初次載入、搜尋與重置不意外跳動）。
- **12/14 午餐雙餐廳導航**：保留單筆午餐行程與既有 76 筆活動結構，新增 `🍜 船麵導航`（Ayutthaya Boat Noodle）與 `🍳 Pad Thai 導航`（PadThai Wangdoem）獨立按鈕。
- **12/14 夜市 BTS 回程與車站導航**：補充夜市至 BTS Talat Phlu（S10）步行約 450m、Silom Line 往 National Stadium 方向、Sala Daeng / Chong Nonsi 分流返宿、午夜前後末班車提醒與分兩台 Grab 備案，並新增 `🚇 BTS Talat Phlu (S10) 導航`。
- **DJ 陣容手機版精簡**：預設僅顯示時間（跨午夜場次醒目標示日期）、DJ 名稱、舞台、必聽等級與精簡衝突摘要；曲風、亮點介紹與完整衝突說明改為點擊「詳情 ▾」展開，並強化「必聽」卡片視覺辨識度。
