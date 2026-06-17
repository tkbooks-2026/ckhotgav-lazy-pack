---
name: 07-obsidian
title: Obsidian第二大腦 本地筆記設定
description: 安裝 Obsidian 並設定 MCPVault 外掛，讓 AI 可以直接讀寫你的 Obsidian 筆記庫
---

# Obsidian 本地筆記設定

## 步驟 1：安裝 Obsidian
前往官網下載並安裝：https://obsidian.md/download
安裝完成後，建立或選擇一個筆記庫（Vault）資料夾，例如 `D:\Obsidian`。

## 步驟 2：安裝 MCPVault 連結工具
MCPVault 是讓 AI 能讀寫你的 Obsidian 筆記的橋接工具。
在終端機執行：
```
npm.cmd install -g @bitbonsai/mcpvault
```

## 步驟 3：設定 MCP 設定檔
詢問使用者的 Obsidian Vault 路徑（例如 `D:\Obsidian`），
然後確認 AI 編輯器的 MCP 設定檔包含以下區塊：

> ⚠️ 不同編輯器的設定檔位置不同：
> - **OpenCode**：`~\.config\opencode\opencode.json`
> - **Antigravity**：`~\.gemini\antigravity\mcp_config.json`
> - **其他編輯器**：請參考各編輯器說明

```json
{
  "mcp": {
    "obsidian": {
      "type": "local",
      "command": ["npx", "-y", "@bitbonsai/mcpvault"],
      "args": ["D:\\Obsidian"],
      "enabled": true
    }
  }
}
```

> ⚠️ Windows 用戶注意：路徑中的反斜線需要寫成雙反斜線 `\\`。

## 步驟 4：重新啟動 AI 編輯器
設定完成後，重新啟動編輯器讓設定生效。

## 使用說明
設定完成後，你可以對 AI 說：
> 「幫我在 Obsidian 裡建立一篇新筆記，標題是（XXX），內容是（YYY）。」
> 「幫我把剛才的設定紀錄存到 Obsidian 的某個資料夾裡。」

## 驗證
請 AI 在你的 Obsidian Vault 資料夾建立一個測試筆記，確認在 Obsidian 應用程式中可以看到該筆記即完成。
