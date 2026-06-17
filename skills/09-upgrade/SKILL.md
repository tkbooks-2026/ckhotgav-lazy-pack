---
name: 09-upgrade
title: 一鍵升級（所有工具升級最新版）
description: 升級所有已裝的 CLI 工具到最新版
---

# 一鍵升級

依序執行以下升級指令，升級所有已安裝的 CLI 工具：

## 1. NotebookLM CLI
```powershell
uv tool upgrade notebooklm-mcp-cli
```

## 2. Netlify CLI
```powershell
npm.cmd update -g netlify-cli
```

## 3. clasp (GAS)
```powershell
npm.cmd update -g @google/clasp
```

## 4. GitHub CLI
Windows 用 winget：
```powershell
winget upgrade GitHub.CLI
```
或從 https://cli.github.com 下載最新版

## 5. MCPVault (Obsidian 橋接)
```powershell
npm.cmd update -g @bitbonsai/mcpvault
```

## 6. Node.js
從 https://nodejs.org 下載 LTS 安裝（會自動覆蓋舊版）

## 7. Python
從 https://python.org 下載新版安裝

## 升級後必做
1. 重新啟動所有 AI 編輯器（OpenCode、Antigravity）
2. 執行 08-doctor 確認一切正常
3. 確認 `nlm login` 仍有效（升級通常不會登出，但保險起見驗證一下）
