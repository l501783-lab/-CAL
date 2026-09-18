# 複利試算終端

複利累積、本金反推與退休提領試算工具。純前端、零依賴、可離線，安裝到 iPhone 或 Android 主畫面後與原生 App 無異。

## 功能

- **累積**：期初本金＋每月定額，算出各年期末總值
- **反推**：由目標終值反推所需期初本金
- **提領**：年初提領、提領金額隨通膨調整，逐年追蹤餘額與見底年度
- **歷史回測**：內建 S&P 500、道瓊、台灣加權、MSCI World、0050、0056、台積電、黃金的年度報酬，可自選區間
- **重播實際年序**：以真實的逐年漲跌模擬，呈現報酬順序風險
- **均值回歸提醒**：提領試算會比對所選區間與該標的全期均值，並以長期均值重算一次對照
- 名目與實質（通膨調整後）雙軌顯示，金額單位為新台幣萬元

## 部署到 GitHub Pages

```bash
git init
git add .
git commit -m "init: 複利試算終端"
git branch -M main
git remote add origin https://github.com/<你的帳號>/<repo名稱>.git
git push -u origin main
```

推上去之後：

1. 進 repo 的 **Settings → Pages**
2. Source 選 **Deploy from a branch**
3. Branch 選 `main`、資料夾選 `/ (root)`，按 Save
4. 等一兩分鐘，網址是 `https://<你的帳號>.github.io/<repo名稱>/`

所有路徑都是相對路徑，放在子目錄也能正常運作。`.nojekyll` 用來關掉 Jekyll 處理，避免底線開頭的檔案被忽略。

## 安裝到手機

- **Android / Chrome**：開啟網址後會跳出安裝提示，或從選單選「安裝應用程式」
- **iPhone / Safari**：點下方「分享」→「加入主畫面」。iOS 不支援自動安裝提示，這是系統限制

裝好之後沒有網路也能用，因為 Service Worker 會把整個 App 快取起來。

## 改版

修改 `index.html` 後，記得把 `sw.js` 裡的 `VERSION` 加一（例如 `v1` → `v2`），使用者下次開啟才會拿到新版本。

## 更新歷史報酬資料

歷史年度報酬寫在 `index.html` 的 `DATA` 物件裡，格式如下：

```js
sp500: {
  name: "S&P 500",
  note: "含息總報酬（美元計價）",
  r: [-3.1, 30.5, 7.6, ...]   // 從 1990 年起，每年一個數字
}
```

若該標的不是從 1990 年開始，加上 `start` 欄位指定起始年（例如 0056 是 `start: 2008`）。要新增一年，在陣列尾端補上數字即可，程式會自動更新可選年份範圍。

## 之後若要上架 App Store 或 Google Play

這份程式碼可以直接用 [Capacitor](https://capacitorjs.com/) 包成原生專案：

```bash
npm init -y
npm i @capacitor/core @capacitor/cli
npx cap init "複利試算終端" com.yourname.compound --web-dir=.
npx cap add ios
npx cap add android
npx cap sync
```

iOS 端需要 macOS 與 Xcode，Apple Developer Program 年費 99 美元。請留意 App Store 審查指南 4.2 的「最低功能性」條款——單純把網頁包成 App 容易被退件，建議先補上本機儲存、情境比較等原生功能。

## 免責聲明

本工具僅供試算，不構成任何投資建議。內建歷史報酬為年度概略值，未扣除手續費、管理費及稅負，亦不保證未來績效。指數名稱為各自所有者之商標，本專案與其無任何關聯。
