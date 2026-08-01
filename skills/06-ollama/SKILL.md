---
name: claude-ollama
description: Claude Code 安裝本地 AI Ollama。說「安裝 Ollama」「本地 AI」時載入。
---

# 安裝本地 AI Ollama（Claude Code 版）

1. 安裝：Windows `winget install --id Ollama.Ollama`；macOS/Linux 見 https://ollama.com/download
2. `ollama --version`
3. **先查記憶體再決定模型**：
   - Windows：`(Get-CimInstance Win32_ComputerSystem).TotalPhysicalMemory`
     （**不要用 `wmic`**，Windows 11 已移除該指令）
   - macOS：`sysctl -n hw.memsize`／Linux：`free -h`

   | 記憶體 | 模型 | 大小 |
   |---|---|---|
   | 8GB 以下 | 建議改用 Gemini 免費 API | — |
   | 8–16GB | `gemma4:e2b` | 約 5GB |
   | 16GB 以上 | `gemma4:e4b` | 約 10GB |
   | 32GB 以上 | `gemma4:12b` | 約 7.6GB |
4. `ollama pull <模型>` → `ollama list` 確認 → `ollama run <模型> "請用繁體中文回答：1+1 等於多少？"`

## 網頁呼叫本地模型

API 端點 `http://localhost:11434/api/generate`。瀏覽器呼叫會遇到 CORS，需設定允許來源：

- **Windows（PowerShell）**：`$env:OLLAMA_ORIGINS="*"; ollama serve`
- **macOS/Linux（bash）**：`OLLAMA_ORIGINS=* ollama serve`

⚠️ `OLLAMA_ORIGINS=* ollama serve` 是 bash 語法，在 PowerShell 會失敗。正式對外請改指定來源，不要用 `*`。

> Ollama 沒有官方的 Claude Code MCP server（`@ollama/mcp-server` 在 npm 上不存在）。
> 要讓 Claude 用本地模型，走上面的 HTTP API，不要嘗試加 MCP。

回報：Ollama 版本、記憶體與選用模型、模型清單、繁中回應測試結果。
