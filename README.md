# Claude Code 懶人包

> 每份懶人包都是一個 MD 檔，丟給 Claude Code 桌面版就能自動完成設定。

---

## 使用方式

### 方式一：直接叫 AI 幫你裝（最簡單）

把這行貼給你的 AI agent（Claude Code / OpenCode / Codex）：

```
這是 Claude Code 懶人包全集 https://github.com/changyiwu/claude-code-lazy-packs
請讀取 repo 內容，列出所有可用的懶人包，問我要裝哪些。
```

AI 會自動讀取 `SKILL.md`（安裝入口），列出所有技能，讓你選擇後自動安裝。

### 方式二：手動下載 MD 檔

1. 下載你要的懶人包（MD 檔）
2. 打開 Claude Code 桌面版（Pro 方案以上），把檔案丟給它
3. AI 自動執行，遇到需要手動操作的地方會暫停指示你

---

## 🌟 每份懶人包都是獨立可執行的

不管你從哪一份進來，都可以直接使用。
每份懶人包都會**自動檢查並安裝**需要的工具（Git、Node.js、uv 等），不需要先執行 #00 環境建置。

---

## 最低先備條件

使用任何懶人包之前，請確認：

- [ ] Claude 帳號已註冊（Pro 方案以上）
- [ ] Claude Code 桌面版已安裝
- [ ] 電腦有網路連線

---

## 懶人包清單

| 編號 | 懶人包名稱 | 狀態 | 說明 |
|------|-----------|------|------|
| 00 | [環境建置](00-環境建置.md) | v0.4 | 安裝 Git、GitHub CLI、uv 等基礎工具 |
| 01 | [連接 Gemini Notebook](01-連接-Gemini-Notebook.md) | v0.6 | 安裝 Gemini Notebook（原 NotebookLM）MCP + 產生簡報與圖表 |
| 02 | [連接 GitHub](02-連接-GitHub.md) | v0.2 | 連接 GitHub + GitHub Pages 教材上線 |
| 03 | [建立第二大腦 Obsidian](03-建立第二大腦-Obsidian.md) | v0.6 | 安裝 Obsidian + MCP 連接 + Google Drive 同步 |
| 04 | [連接 Supabase 資料庫](04-連接-Supabase-資料庫.md) | v0.3 | 連接雲端資料庫，讓程式「記得住」（已改用遠端 MCP + OAuth，不需要 API key） |
| 05 | [連接 Firebase 資料庫](05-連接-Firebase-資料庫.md) | v0.8 | 對老師更友善的資料庫選擇（不會閒置暫停、千人研習撐得住、Firestore MCP 完整 CRUD） |
| 06 | [安裝本地 AI Ollama](06-安裝本地AI-Ollama.md) | v0.3 | 安裝本地 AI，免費、隱私、離線可用 |
| 07 | [設定 Gemini 免費 API](07-設定Gemini免費API.md) | v0.3 | 設定 Gemini 免費 API，不用信用卡 |
| 08 | [把 ChatGPT Image 2.0 裝進 Claude Code](08-安裝gpt-image-2生圖.md) | v0.1 | 全域 `draw` Skill 安裝：OpenAI API Key + Individual 驗證 + `~/.claude/skills/claude-draw/` + 第一張圖驗證。之後在任何專案對 Claude 說「畫一張 XX」就生圖 |

> 懶人包會在不斷實作的過程中持續更新。

---

## 致謝與來源

本專案改編自 **三師爸（宋睿偉）** 的 [claude-code-lazy-packs](https://github.com/mathruffian-dot/claude-code-lazy-packs)，依自身使用需求調整內容與編號。原始著作權歸原作者所有。

## 授權

本專案採用 [MIT License](LICENSE) 授權，歡迎自由使用與分享。
