---
name: 12-other-deploy
title: 其他部署平台（選用）
description: 其他部署平台（Vercel、Cloudflare Pages、Railway）— 選用
---

# 其他部署平台（選用）

## Vercel
安裝：`npm.cmd install -g vercel`
登入：`vercel login`
部署：`vercel --prod`
適合：Next.js、React、Vite 等前端框架

## Cloudflare Pages
安裝：`npm.cmd install -g wrangler`
登入：`wrangler login`
部署：`wrangler pages deploy ./dist`
適合：靜態網頁、Workers

## Railway
安裝：`npm.cmd install -g @railway/cli`
登入：`railway login`
部署：`railway init` → `railway up`
適合：後端服務、資料庫

## 比較表

| 平台 | 靜態網頁 | 後端 | 免費額度 |
|------|:---:|:---:|---------|
| Netlify | ✅ | ⚠️ Functions | 100GB/月 |
| Vercel | ✅ | ✅ Serverless | 100GB/月 |
| Cloudflare Pages | ✅ | ✅ Workers | 無限 |
| Railway | ⚠️ | ✅ 完整 | $5/月起 |
