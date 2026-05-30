---
name: gmb-order-provider
description: 建立或更新 GMB / Google 商家檔案訂購供應商分析資料集與靜態 GitHub Pages 網站。當使用者要分析門市、比對官方門市清單與 Google 商家檔案或 GMB 連結、稽核外帶與外送訂購供應商、辨識 foodpanda / Uber Eats / Nidin / LINE 連結、產生 stores.json / summary.json / CSV 資料、製作縣市下拉篩選、台灣縣市門市分布地圖、六大區門市統計，或發布分析網站到 GitHub Pages 時使用。預設工作流程以迷客夏台灣門市分析為範例。
---

# GMB Order Provider

## 概覽

使用這個 Skill 重現迷客夏訂購供應商稽核流程：建立台灣官方門市母體、補上 GMB 與 Google Maps 連結、區分外帶與外送供應商，並發布靜態分析網站。

可重複使用的腳本放在 `scripts/`。執行完整稽核前先讀 `references/workflow.md`；需要變更欄位、狀態或圖表時，先讀 `references/data-model.md`。

## 核心流程

1. 從乾淨的專案資料夾或既有靜態網站 repo 開始。
2. 使用 `https://www.milksha.com/store_detail.php?uID=1` 作為官方門市母體來源。
3. 產生基礎資料：

```powershell
python <skill>/scripts/fetch_official_stores.py
```

4. 從公開可搜尋的門市頁面稽核第三方外送供應商：

```powershell
python <skill>/scripts/audit_order_providers.py --workers 10
```

5. 稽核官方 Nidin 訂購供應商：

```powershell
python <skill>/scripts/audit_nidin_order.py
```

6. 在資料與介面中分開處理外帶與外送：
   - `takeoutProviders`
   - `deliveryProviders`
   - `takeoutProviderCounts`
   - `deliveryProviderCounts`

7. 發布前驗證統計數字：

```powershell
python -c "import json; s=json.load(open('data/summary.json',encoding='utf-8')); print(s)"
```

## 來源規則

- 以迷客夏官方門市頁作為門市母體。
- 以 Nidin API 作為官方線上訂購、外帶與 Nidin 外送可用性的公開權威來源。
- 以 Footinder 頁面作為 foodpanda、Uber Eats、lin.ee 的次要交叉檢查來源。
- 將 Google Maps / GMB 頁面視為佐證連結，但不要從無法取得的動態按鈕猜測供應商名稱。
- 如果公開來源無法確認供應商，保留該門市並標記為需人工複查。

## 靜態網站要求

建立公開網站時，包含：

- 指標卡：官方門市數、已找到 GMB 數、外帶數、外送數、未知數。
- 全站縣市下拉篩選；選擇縣市後，指標卡、圖表、地圖與明細表都同步更新。
- 台灣縣市門市分布地圖：每個縣市直接標出門市數，未設店縣市顯示 `0`，並保留右側或下方縣市清單。
- 六大區門市統計卡：台北基宜、桃竹苗、中彰投、雲嘉南、高屏、花東；區域統計加總應等於目前篩選範圍的門市總數。
- 外帶供應商與外送供應商分開呈現圖表。
- 可搜尋、可篩選的明細表，包含 GMB、官方、Nidin 與外部佐證連結。
- 將資料檔保留在 `data/`，方便未來更新。

## 台灣地圖與區域統計規格

- 以 `stores.json` 的門市地址或縣市欄位推算縣市；地址能拆出 `新竹市` / `新竹縣` 時要分開統計，不要只保留「新竹縣市」。
- 支援 22 個縣市標籤：台北市、新北市、基隆市、宜蘭縣、桃園市、新竹市、新竹縣、苗栗縣、台中市、彰化縣、南投縣、雲林縣、嘉義市、嘉義縣、台南市、高雄市、屏東縣、花蓮縣、台東縣、澎湖縣、金門縣、連江縣。
- 使用六大區：
  - 台北基宜：台北市、新北市、基隆市、宜蘭縣
  - 桃竹苗：桃園市、新竹市、新竹縣、苗栗縣
  - 中彰投：台中市、彰化縣、南投縣
  - 雲嘉南：雲林縣、嘉義市、嘉義縣、台南市
  - 高屏：高雄市、屏東縣
  - 花東：花蓮縣、台東縣
- 若資料含離島，縣市地圖仍顯示澎湖、金門、連江數字；六大區卡片不把離島併入上述六區，除非使用者另外指定。
- 地圖可用內嵌 SVG 或靜態資產，不依賴外部地圖服務；GitHub Pages 必須能離線載入基本視覺。
- 驗證時檢查：縣市標籤加總等於目前篩選範圍門市數；六大區統計加總等於目前篩選範圍門市數，若資料含離島則需明確呈現離島另計或調整區域定義。

## 驗證

修改腳本後，執行語法與資料檢查：

```powershell
node --check app.js
python -c "import json; stores=json.load(open('data/stores.json',encoding='utf-8'))['stores']; summary=json.load(open('data/summary.json',encoding='utf-8')); assert len(stores)==summary['officialStoreCount']; assert sum(1 for x in stores if x['takeoutAvailable'] is True)==summary['takeoutCount']; assert sum(1 for x in stores if x['deliveryAvailable'] is True)==summary['deliveryCount']; print('ok')"
```

發布到 GitHub Pages 時，除非使用者指定其他部署方式，否則推送到 `main`，並從分支根目錄啟用 Pages。
