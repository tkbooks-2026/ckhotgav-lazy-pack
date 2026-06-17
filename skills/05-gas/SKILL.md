---
name: 05-gas
title: Google Apps Script (GAS) 設定
description: 安裝 clasp CLI 並完成 Google 授權，讓 AI 可以把腳本推送到 Google Apps Script
---

# Google Apps Script (GAS) 設定

## 步驟 0：事前準備（必須先做！）
請引導使用者在瀏覽器開啟此網址，並把「Google Apps Script API」切換為開啟 (ON)：
https://script.google.com/home/usersettings

> ⚠️ 若未開啟此設定，後續的 clasp push 會一直報「沒有權限」的錯誤。

## 步驟 1：安裝 clasp
執行：`npm.cmd install -g @google/clasp`

## 步驟 2：登入 Google 帳號
執行：`clasp.cmd login`
瀏覽器會打開 Google 授權頁面，登入並點選「允許」。

## 使用說明
安裝完成後，你可以對 AI 說：
> 「幫我寫一個 Google Apps Script，功能是（描述你想要的功能），
>  寫完後幫我用 clasp push 部署到我的帳號。」

> ⚠️ **首次部署必做**：專案根目錄需要先有兩個檔案：
> - `appsscript.json`：GAS 設定檔（runtime、權限等）
> - `.clasp.json`：clasp 設定檔（指向你的 GAS 專案 ID）
>
> 若使用 `clasp create` 會自動產生，否則請 AI 幫你建立範本。

AI 會建立必要的 `appsscript.json` 與 `.js` 腳本檔案，並執行 `clasp.cmd push` 上傳。

## 驗證
執行 `clasp.cmd whoami`，看到你的 Google 帳號 email 即完成。
