---
name: 03-github
title: GitHub 版本控制設定
description: 安裝 GitHub CLI，完成帳號登入，並設定 Git 使用者資訊
---

# GitHub 版本控制設定

## 步驟 1：確認 Git 已安裝
執行：`git --version`
- 若顯示版本號，跳至步驟 2。
- 若未安裝，優先嘗試執行 `winget install --id Git.Git -e --source winget` 自動安裝。若無法安裝，請引導使用者前往 https://gitforwindows.org/ 下載安裝，安裝完畢後需重新啟動終端機。

## 步驟 2：確認 GitHub CLI 已安裝
執行：`gh --version`
- 若顯示版本號，跳至步驟 3。
- 若未安裝，優先嘗試執行 `winget install --id GitHub.cli -e --source winget` 自動安裝。若無法安裝，請引導使用者前往 https://cli.github.com 下載安裝。

## 步驟 3：登入 GitHub 帳號
執行：`gh auth login --web --git-protocol https`
一路按 Enter，直到瀏覽器打開，完成網頁授權。

## 步驟 4：設定 Git 使用者身分
詢問使用者的 GitHub 帳號暱稱與 email（GitHub 註冊時用的），然後執行：
```
git config --global user.name "<your-github-nickname>"
git config --global user.email "<your-github-email@example.com>"
```

## 常見問題排除
若遇到 `GITHUB_TOKEN` 衝突報錯（SecurityException 或 Bad credentials），執行：
```
[Environment]::SetEnvironmentVariable("GITHUB_TOKEN", $null, "User")
[Environment]::SetEnvironmentVariable("GITHUB_TOKEN", $null, "Process")
```
然後重新執行步驟 2。

## 驗證
執行 `gh auth status`，確認看到「Logged in to github.com」即完成。
