---
name: 06-workspace
title: 專案流程自動化（開工/收工/里程碑 關鍵字驅動）
description: 部署 startup / shutdown 系統技能，讓 AI 根據「開工/記錄/里程碑/收工」關鍵字自動執行對應流程（一次安裝，所有專案通用）
---

# 專案駕駛艙 — 系統技能部署

## 說明
此技能會將 startup / shutdown 寫入 AI 編輯器的系統技能目錄。
**只需安裝一次**，之後在任何專案說「開工」「記錄」「完成」「收工」都會自動觸發。

## 步驟 0：詢問使用者偏好

### 0.1 自動偵測 Obsidian Vault 路徑
先嘗試讀取 AI 編輯器的設定檔中的 Obsidian 路徑：
- **OpenCode**：檢查 `~\.config\opencode\opencode.json` 的 `mcp.obsidian` 區塊參數
- **Antigravity**：檢查 `~\.gemini\antigravity\mcp_config.json`

若找不到或無法解析，請直接詢問使用者：「你的 Obsidian Vault 在哪個資料夾？」
- 例如：`D:\Obsidian`、`C:\Users\名字\Documents\Obsidian Vault`

### 0.2 詢問每日筆記資料夾名稱
- 預設值：`每日筆記`
- 範例：`Daily Notes`、`日記`、`Journal`

### 0.3 詢問 AI 回覆語言
- 預設值：`繁體中文`
- 範例：`簡體中文`、`English`、`日本語`

### 0.4 確認要部署到哪些編輯器
確認使用者使用的編輯器：
- **OpenCode**（`~\.config\opencode\skills\`）
- **Antigravity**（`~\.gemini\antigravity\skills\`）
- 若兩個都用，則兩邊都寫入。

## 步驟 1：寫入 startup SKILL.md

將以下內容帶入步驟 0 蒐集到的變數（`${VAULT_PATH}`、`${DAILY_NOTES_DIR}`、`${RESPONSE_LANG}`），再寫入到對應的編輯器目錄：

（內容略 - 請參考完整版自行填入變數後寫入）

## 步驟 2：寫入 shutdown SKILL.md

（內容略 - 請參考完整版自行填入變數後寫入）

## 步驟 3：寫入對應編輯器

| 編輯器 | 目標路徑 |
|--------|---------|
| OpenCode | `~\.config\opencode\skills\startup\SKILL.md` 與 `~\.config\opencode\skills\shutdown\SKILL.md` |
| Antigravity | `~\.gemini\antigravity\skills\startup\SKILL.md` 與 `~\.gemini\antigravity\skills\shutdown\SKILL.md` |

## 驗證
確認以下檔案已建立且內容正確（依編輯器而定）：
- `<editor-config>/skills/startup/SKILL.md`
- `<editor-config>/skills/shutdown/SKILL.md`
