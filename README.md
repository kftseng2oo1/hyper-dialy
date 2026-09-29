# 日課 · 體重與飲食記錄

單頁離線 PWA。記錄每天的體重、吃了什麼（可附照片）、BMI 與趨勢圖。
所有資料只留在裝置上，沒有後端、沒有帳號、不會上傳。

## 檔案

| 檔案 | 用途 |
|---|---|
| `index.html` | 全部的 App（HTML + CSS + JS，無外部相依） |
| `install.html` | 給使用者看的安裝手冊，可直接用 LINE 傳連結 |
| `manifest.json` | PWA 安裝資訊 |
| `sw.js` | Service Worker，離線快取 |
| `icon-*.png` | 圖示，含 maskable |
| `og.png` | LINE／社群分享時的預覽圖（1200×630） |
| `.nojekyll` | 讓 GitHub Pages 不要跑 Jekyll |

## 部署到 GitHub Pages

```bash
cd daily-log
git init -b main
git add .
git commit -m "日課：體重與飲食記錄 PWA"
git remote add origin https://github.com/kftseng2oo1/dialy.git
git push -u origin main
```

接著到 repo 的 **Settings → Pages**，Source 選 `Deploy from a branch`，
分支 `main`、資料夾 `/ (root)`，存檔後約一分鐘會出現：

    https://kftseng2oo1.github.io/dialy/

安裝手冊在 `install.html`，網址是：

    https://kftseng2oo1.github.io/dialy/install.html

用 GitHub CLI 的話一行搞定：

```bash
gh repo create dialy --public --source=. --push
gh api -X POST repos/kftseng2oo1/dialy/pages \
  -f 'source[branch]=main' -f 'source[path]=/'
```

## 安裝到手機

- **iOS**：用 Safari 開啟網址 → 分享 → 加入主畫面
- **Android**：Chrome 開啟 → 選單 → 安裝應用程式

Service Worker 需要 HTTPS，GitHub Pages 預設就有。

## 改版

改完 `index.html` 之後，把 `sw.js` 最上面的 `VERSION` 加一：

```js
var VERSION = 'v2';
```

舊快取會在下次啟動時自動清掉。使用者開啟時若偵測到新版，
畫面會提示「已下載新版本，關掉再開就會套用」。

## 資料存放

- 體重、飲食、身高、目標：`localStorage`
- 照片：`IndexedDB`（`daily-log-photos`，分 `photos` / `thumbs`）

換手機或清瀏覽器資料前，記得到「設定」匯出 JSON 備份；
要連照片一起帶走就勾「備份包含照片」。
