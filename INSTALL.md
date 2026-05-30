# 安裝導引

## 方法一：從 GitHub 下載 ZIP

1. 打開這個 GitHub repo。
2. 點選 `Code`。
3. 選擇 `Download ZIP`。
4. 解壓縮 ZIP。
5. 在 Codex 的 Skill 匯入功能中，選擇解壓縮後「第一層就看得到 `SKILL.md`」的資料夾。
6. 重新開啟 Codex。
7. 在 Skill 清單搜尋 `GMB` 或 `GMB Order Provider`。

如果解壓縮後的資料夾名稱是 `gmb-order-provider-main`，通常不需要改名；重點是該資料夾第一層要直接有 `SKILL.md`。

## 方法二：手動放進 Codex Skills 資料夾

Windows 預設位置通常是：

```text
C:\Users\你的使用者名稱\.codex\skills\
```

請把整個 Skill 資料夾放進去，最後結構應該像這樣：

```text
C:\Users\你的使用者名稱\.codex\skills\gmb-order-provider\SKILL.md
```

放好後重新開啟 Codex，再搜尋 `GMB` 或 `GMB Order Provider`。

## 方法三：用 Git Clone

如果你熟悉 Git，可以直接 clone 到 Codex skills 目錄：

```powershell
cd "$env:USERPROFILE\.codex\skills"
git clone https://github.com/daphnegood1/gmb-order-provider.git
```

完成後重新開啟 Codex。

## 驗證是否安裝成功

安裝成功後，Codex 的 Skill 清單應該可以看到：

```text
GMB Order Provider
```

也可以對 Codex 說：

```text
請用 GMB Order Provider 分析門市的 order 供應商
```
