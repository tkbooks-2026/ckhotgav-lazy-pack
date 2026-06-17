---
name: 02-notebooklm
title: NotebookLM 知識庫串接
description: 安裝 NotebookLM CLI 工具並完成 MCP 外掛綁定，讓 AI 能操作使用者的 NotebookLM 知識庫
---

# NotebookLM 知識庫串接

## 步驟 1：安裝 CLI 工具
執行：`pip install notebooklm-mcp-cli`

若 pip 指令失敗，改試：`pip3 install notebooklm-mcp-cli`

## 步驟 2：登入 Google 帳號
> ⚠️ 重要：此步驟必須由使用者親自在終端機執行，AI 無法代勞（系統安全限制）。

請使用者在終端機輸入：`nlm login`
瀏覽器會彈出 Google 授權畫面，登入並點選「允許」。

## 步驟 3：綁定 AI 編輯器
**如果你兩個編輯器都有用，請兩個指令都跑**（不會衝突）：
- **Antigravity 用戶**：`nlm setup add antigravity`
- **OpenCode 用戶**：`nlm setup add opencode`

執行後會顯示該編輯器的 MCP 設定路徑，請記下。

## 步驟 4：重新啟動編輯器
綁定完成後，必須重新啟動 AI 編輯器，設定才會生效。

## 步驟 5：Token 過期機制
NotebookLM 的登入 cookies **每 2-4 週會過期**。過期時 AI 操作 NBLM 會出現 `Authentication expired`。

遇到此情況請使用者：
- 終端機執行 `nlm login` 重新登入
- 或對 AI 說「幫我刷新 NotebookLM 的登入狀態」觸發 `refresh_auth`

## 特別說明（Antigravity 用戶）
因系統安全隔離，AI 可能仍無法全自動操作 NotebookLM。
遇到這種情況時，使用者可以：
1. 執行 `nlm notebook list` 取得筆記本 ID
2. 把 ID 告訴 AI，AI 會生成對應的上傳指令供使用者複製貼上執行

## 驗證
執行 `nlm notebook list` 能看到筆記本清單，即完成。
