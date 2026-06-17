---
name: 04-netlify
title: Netlify 網頁部署設定
description: 安裝 Netlify CLI 並完成帳號登入，讓 AI 可以一鍵部署靜態網頁
---

# Netlify 網頁部署設定

## 步驟 1：安裝 Netlify CLI
執行：`npm.cmd install -g netlify-cli`

（Windows 用戶請注意：使用 `npm.cmd` 而非 `npm`，可避免 PowerShell 權限問題）

## 步驟 2：登入 Netlify 帳號
執行：`netlify.cmd login`
瀏覽器會打開 Netlify 授權頁面，登入你的 Netlify 帳號（可用 GitHub 登入）並授權。

## 使用說明
安裝完成後，你可以對 AI 說：
> 「幫我寫一個（主題）的網頁，完成後直接幫我部署到 Netlify。」

AI 會寫好程式碼並執行 `netlify deploy --prod` 幫你部署，並回傳網址。

## 個人開發 vs. CI/CD 補充說明
- **本機開發**（你自己的電腦）：用 `netlify login` 即可，不需要設定 Token。
- **GitHub Actions 自動部署**：需要到 Netlify 後台產生 Personal Access Token，
  並設定在 GitHub Secrets 的 `NETLIFY_AUTH_TOKEN` 中。

## 驗證
執行 `netlify.cmd status`，看到帳號資訊即完成。
