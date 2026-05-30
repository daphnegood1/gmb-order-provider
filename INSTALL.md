# 安裝導引

建議使用 Codex 內建的 **Skill Installer** 從 GitHub 安裝。這樣不用自己下載 ZIP、解壓縮或搬資料夾。

## 最簡單的安裝方式

請在 Codex 開一個新對話，直接貼上這段文字：

```text
請幫我安裝這個 Skill：
https://github.com/daphnegood1/gmb-order-provider
```

Codex 會使用 Skill Installer，把這個 GitHub repo 安裝到你的 Codex skills 目錄。

## 安裝完成後

1. 重新開啟 Codex。
2. 到 Skill 清單搜尋：

```text
GMB Order Provider
```

3. 如果看得到 `GMB Order Provider`，代表安裝成功。

## 如何使用

安裝後，可以直接對 Codex 說：

```text
請用 GMB Order Provider 分析門市的 order 供應商
```

或：

```text
請用 GMB Order Provider 幫我比對 GMB 訂購供應商
```

## 如果 Skill Installer 無法安裝

請確認：

- Codex 的 Skill 清單中有 `Skill Installer`
- 網路可以連到 GitHub
- GitHub repo 網址正確：

```text
https://github.com/daphnegood1/gmb-order-provider
```

如果仍無法安裝，再改用手動下載或 Git clone。
