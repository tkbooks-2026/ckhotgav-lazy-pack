---
name: 01-environment
title: 基礎環境建設
description: 基礎環境建設，安裝 Node.js、Python，並解決 Windows PowerShell 執行權限問題
---

# 基礎環境建設

## 步驟 1：確認 Node.js 已安裝
執行：`node --version`
- 若顯示版本號 (如 v20.x.x)，代表已安裝，跳至步驟 2。
- 若顯示錯誤，優先嘗試執行 `winget install --id OpenJS.NodeJS.LTS -e --source winget` 進行自動安裝。若無法使用 winget，請引導使用者前往 https://nodejs.org 下載 LTS 版本安裝。

## 步驟 2：確認 Python 已安裝
執行：`python --version`
- 若顯示版本號，跳至步驟 3。
- 若顯示錯誤，優先嘗試執行 `winget install --id Python.Python.3.12 -e --source winget` 進行自動安裝。若無法使用 winget，請引導使用者前往 https://python.org 下載並安裝（請以**粗體強烈提醒**使用者安裝時務必勾選「Add Python to PATH」）。

## 步驟 3：解決 Windows PowerShell 執行權限（僅 Windows 用戶）
執行：`Get-ExecutionPolicy`
- 若結果為 `Restricted`，請執行：
  `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`
  並輸入 Y 確認。
- 若結果已是 `RemoteSigned` 或 `Unrestricted`，跳過此步驟。

## 驗證
執行 `node --version` 與 `python --version`，兩者都顯示版本號即完成。
