---
name: 10-troubleshoot
title: 疑難排解
description: 常見錯誤與修復流程
---

# 疑難排解

## 🔴 NotebookLM 認證失敗
**症狀**：`Authentication expired`
**修復**：`nlm login` 重新登入，重啟編輯器

## 🔴 PowerShell 拒絕執行腳本
**症狀**：`cannot be loaded because running scripts is disabled`
**修復**：`Set-ExecutionPolicy RemoteSigned -Scope CurrentUser`

## 🔴 npm 全域安裝權限錯誤
**症狀**：EACCES / EPERM
**修復**：改用 `npm.cmd install -g` 或使用 nvm-windows

## 🔴 clasp 報「沒有權限」
**症狀**：Permission denied
**修復**：確認 https://script.google.com/home/usersettings 已開啟 GAS API

## 🟡 Netlify deploy 卡住
**修復**：`netlify status` 確認登入，`netlify link` 連結網站

## 🟡 GitHub push 失敗
**修復**：清除 GITHUB_TOKEN 環境變數，重新 `gh auth login`

## 🟡 Obsidian MCP 無法連線
**修復**：確認設定檔有 obsidian 區塊、路徑用 `\\`、重啟編輯器

## 🟡 說「開工」沒反應
**修復**：確認 SKILL.md 存在，opencode.json 有 startup/shutdown 權限

## 仍然找不到解？
1. 執行 08-doctor 取得完整狀態報告
2. 對 AI 說：「我遇到 XXX 錯誤，請幫我找其他方法」
