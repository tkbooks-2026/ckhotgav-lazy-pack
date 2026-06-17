---
name: ckhotgav-lazy-pack
description: >
  AI 編輯器 (Antigravity/OpenCode) 新手全能懶人包。
  當使用者說「AI 編輯器」「懶人包」「Antigravity」「OpenCode」「環境設定」「安裝技能」等關鍵字時觸發。
  觸發後請讀取此 SKILL.md 列出所有技能（00~12），詢問使用者要安裝哪些，
  然後按照對應的 SKILL.md 指示逐步安裝。
---

# 技能清單

| 編號 | 技能 | 觸發字 |
|------|------|--------|
| `00` | 一鍵全裝 | 一次裝完所有技能 |
| `01` | 基礎環境 | Node / Python / PowerShell 設定 |
| `02` | NotebookLM | NBLM / 知識庫 / MCP |
| `03` | GitHub | gh / 版控 / git |
| `04` | Netlify | 靜態網頁 / 部署 |
| `05` | GAS | Apps Script / clasp |
| `06` | 專案流程自動化 | 開工 / 收工 / 里程碑 關鍵字驅動 |
| `07` | Obsidian第二大腦 | 筆記 / Vault / MCPVault |
| `08` | 系統健康檢查 | doctor / 檢查 / 健康 / 版本狀態 |
| `09` | 一鍵升級 | 升級 / update / 舊版 |
| `10` | 疑難排解 | 報錯 / 失敗 / 修 |
| `11` | 其他 AI 編輯器 | Claude Code / Gemini / Codex |
| `12` | 其他部署平台 | Vercel / Cloudflare / Railway |
| `13` | PHP+MySQL 環境 | PHP / MySQL / Composer / Docker |

# 安裝流程

讀取此 SKILL.md 後，請列出上方所有技能，逐一詢問使用者想安裝哪些，
然後依序讀取對應資料夾中的 SKILL.md 並執行安裝。

每安裝完一項技能，請確認使用者已完成驗證步驟後，再繼續下一項。

> ⚠️ **平台提示**：本懶人包以 **Windows** 為主要目標平台。若偵測到 macOS / Linux，請提醒使用者部分指令需自行轉換。

# 更新本懶人包

當使用者說「更新懶人包」時：
1. 引導使用者執行 `git pull`（若本機有 clone）
2. 重新讀取所有 SKILL.md 確認內容是否有變
3. 提示使用者哪些技能有更新
