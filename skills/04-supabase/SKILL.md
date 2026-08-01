---
name: claude-supabase
description: Claude Code 連接 Supabase MCP。說「連接 Supabase」時載入。
---

# 連接 Supabase（Claude Code 版）

Supabase 官方已改用**遠端 MCP + 瀏覽器 OAuth**，不需要在本機跑 npx，也不需要任何 key。

1. 到 Supabase Dashboard 打開專案，從網址 `…/dashboard/project/<ref>` 取得 **project ref**
2. 加入 MCP：

   ```bash
   claude mcp add --scope user --transport http supabase "https://mcp.supabase.com/mcp?project_ref=<ref>&read_only=true"
   ```

   桌面版沒有 CLI 時，手動寫入 `~/.claude.json` 的 `mcpServers`（或專案的 `.mcp.json`）：

   ```json
   "supabase": { "type": "http", "url": "https://mcp.supabase.com/mcp?project_ref=<ref>&read_only=true" }
   ```

   - **不是 `settings.json`**（該 schema 不接受 `mcpServers`）
   - **`"type": "http"` 不可省略**，只有 `url` 會被當成 stdio server 而跳過
3. 完全重啟 Claude Code → 執行 `/mcp` 選 supabase 完成 OAuth 授權
4. 驗證：查詢資料庫表格（新專案是空的，屬正常）
5. 需要建表或寫入時，才拿掉網址的 `&read_only=true` 並重啟；用完改回唯讀

⚠️ **不要使用 service_role key**。舊教學的 `--supabase-url` / `--supabase-service-role-key` 參數已不存在，
照打會噴 `ERR_PARSE_ARGS_UNKNOWN_OPTION`；MCP 從不使用 service_role key，該 key 外流等同整個資料庫被拿走。

回報：project ref（非機密）、OAuth 授權狀態、唯讀或可寫、查詢測試結果。
