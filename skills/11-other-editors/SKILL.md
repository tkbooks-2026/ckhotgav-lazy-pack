---
name: 11-other-editors
title: 其他 AI 編輯器（選用）
description: 其他 AI 編輯器的設定（Claude Code、Gemini CLI、Codex CLI）— 選用
---

# 其他 AI 編輯器（選用）

本懶人包主力支援 **OpenCode** + **Antigravity**，但以下編輯器也可串接相同的生態系。

## Claude Code
安裝：`npm.cmd install -g @anthropic-ai/claude-code`
串接 NotebookLM：`nlm setup add claude-code`
串接 Obsidian：編輯 `~\.claude\mcp.json` 加入 obsidian MCP 設定

## Gemini CLI
安裝：參考 https://github.com/google-gemini/gemini-cli
串接 NotebookLM：`nlm setup add gemini`

## Codex CLI (OpenAI)
安裝：`npm.cmd install -g @openai/codex`
串接 NotebookLM：`nlm setup add codex`

## 驗證
1. 對該編輯器的 AI 說：「列出我的 NotebookLM 筆記本」
2. 對 AI 說：「幫我在 Obsidian 建立一篇測試筆記」
