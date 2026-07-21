---
name: cc-supabase
description: Claude Code 連接 Supabase MCP。說「連接 Supabase」時載入。
---

# 連接 Supabase（Claude Code 版）

1. 安裝：`npm install -g @supabase/mcp-server-supabase`
2. 登入 Supabase → 取得 project ref + API key
3. 用 `claude mcp add supabase -- npx @supabase/mcp-server-supabase --project-ref <ref> --api-key <key>`；沒有 CLI 時手動寫入 `~/.claude.json` 的 `mcpServers` 或專案的 `.mcp.json`（**不是 `settings.json`**）
4. 重啟後驗證：查詢資料庫表格

⚠️ 不把 API key 寫進 CLAUDE.md 或 repo。
