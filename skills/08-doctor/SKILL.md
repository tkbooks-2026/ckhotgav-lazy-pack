---
name: 08-doctor
title: 系統健康檢查（工具版本與狀態總覽）
description: 跑整體健康檢查，列出所有已裝項目狀態
---

# 整體驗證 Doctor

執行以下檢查並輸出結果表格，讓使用者一目了然哪些裝了、哪些還沒。

## 檢查清單

| # | 項目 | 指令 | 預期 |
|---|------|------|------|
| 1 | Node.js | `node --version` | v18+ |
| 2 | Python | `python --version` | 3.11+ |
| 3 | PowerShell 權限（Windows） | `Get-ExecutionPolicy` | RemoteSigned / Unrestricted |
| 4 | GitHub CLI | `gh --version` | 2.0+ |
| 5 | GitHub 登入 | `gh auth status` | Logged in |
| 6 | Netlify CLI | `netlify --version` | 任何版本 |
| 7 | Netlify 登入 | `netlify status` | 已授權帳號 |
| 8 | clasp (GAS) | `clasp --version` | 任何版本 |
| 9 | clasp 登入 | `clasp whoami` | 顯示 email |
| 10 | NotebookLM CLI | `nlm --version` | 0.6+ |
| 11 | NotebookLM 登入 | `nlm login --check` | Logged in |
| 12 | Obsidian MCP 設定 | 讀取編輯器設定檔 | 有 obsidian 區塊 |
| 13 | startup skill | 檢查技能目錄 | 存在 |
| 14 | shutdown skill | 檢查技能目錄 | 存在 |

## 輸出格式

執行後請以表格輸出結果，最後總結通過/失敗項數，並針對失敗項給出修復建議。

## 平台提示
- **Windows**：所有指令如上表
- **macOS/Linux**：注意 `python` 可能為 `python3`、`npm.cmd` 改為 `npm`、PowerShell 權限檢查不適用
