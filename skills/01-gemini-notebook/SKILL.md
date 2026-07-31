---
name: claude-gemini-notebook
description: Claude Code 連接 Gemini Notebook（原 NotebookLM）MCP。說「連接 Gemini Notebook」或「連接 NotebookLM」時載入。
---

# 連接 Gemini Notebook（原 NotebookLM，Claude Code 版）

> 產品 2026-07-16 由 NotebookLM 更名為 Gemini Notebook，但套件名、CLI 指令、
> 認證目錄、MCP server 名稱**全部沿用 `notebooklm`**，以下指令照打即可。

## 步驟

### 1. 安裝
```bash
uv tool install notebooklm-mcp-cli
# 或 pip install notebooklm-mcp-cli
nlm --version
```

### 2. 登入
```bash
nlm login
```
（瀏覽器會開啟 Google 登入頁面）

驗證：
```bash
nlm doctor
```

### 3. 註冊 MCP
```bash
nlm setup add claude-code
```

⚠️ **桌面版一定會失敗**：`nlm setup` 是靠呼叫 `claude` CLI 寫設定的，桌面版沒有 CLI，
會顯示 `Warning: 'claude' command not found in PATH`，並建議手動加到 `~/.claude/settings.json`
——**那個建議是錯的**，現行 schema 不接受 `mcpServers`。

改為手動寫入 `~/.claude.json` 最上層（保留原有內容，只新增 `mcpServers`）：
```json
"notebooklm": {
  "command": "C:/Users/[使用者]/.local/bin/notebooklm-mcp.exe",
  "args": ["--transport", "stdio"]
}
```
路徑用 `nlm doctor` 查（會直接印出 `notebooklm-mcp` 實際位置）；macOS/Linux 通常是
`~/.local/bin/notebooklm-mcp`。`--transport stdio` 是預設值，省略亦可。

### 4. 驗證
重啟 Claude Code 後，問：「請列出我所有的 Gemini Notebook 筆記本。」

回報格式：nlm 版本、登入狀態（nlm doctor）、MCP 設定、筆記本讀取測試結果。

## 如果失敗
- 手動編輯 `~/.claude.json` 的 `mcpServers` 區塊（使用者層）或專案根目錄的 `.mcp.json`；`settings.json` 不接受 `mcpServers`
- 移除：`nlm setup remove claude-code`；`uv tool uninstall notebooklm-mcp-cli`；`nlm logout`
- 從步驟 1 重來
