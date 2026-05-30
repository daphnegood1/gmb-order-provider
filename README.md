# GMB Order Provider

這是一個 Codex Skill，用來分析 GMB / Google 商家檔案中的訂購供應商，包含外帶、外送、Nidin、Uber Eats、foodpanda、LINE 等連結與統計資料。

## 適用情境

- 比對官方門市清單與 Google 商家檔案 / GMB 連結
- 稽核門市的外帶與外送訂購供應商
- 辨識 foodpanda、Uber Eats、Nidin、LINE 等訂購連結
- 產生 `stores.json`、`summary.json`、CSV 資料
- 建立或更新靜態 GitHub Pages 分析網站

## 安裝方式

建議使用 Codex 內建的 **Skill Installer** 安裝。

請在 Codex 開一個新對話，貼上：

```text
請幫我安裝這個 Skill：
https://github.com/daphnegood1/gmb-order-provider
```

安裝完成後，重新開啟 Codex，然後搜尋：

```text
GMB Order Provider
```

完整安裝說明請看 [INSTALL.md](INSTALL.md)。

## Skill 結構

```text
gmb-order-provider/
  SKILL.md
  README.md
  INSTALL.md
  agents/
    openai.yaml
  references/
  scripts/
```

## 注意事項

這個 repo 不應放入 API key、密碼、token、`.env`、私人資料或執行後產生的大量資料檔。`.gitignore` 已排除常見私密檔案與輸出資料夾。
