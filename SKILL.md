---
name: claude-code-lazy-packs
description: Claude Code 懶人包全集 — 環境建置、MCP 串接、技能安裝。說「Claude Code 懶人包」「安裝懶人包」時載入。
---

# Claude Code 懶人包 — AI Agent 自動安裝入口

當使用者給你這個 repo 網址並說要安裝時：

## 步驟一：列出可用懶人包

| 編號 | Skill 名稱 | 說明 |
|------|-----------|------|
| 00 | `00-env-setup` | 安裝 uv 等基礎工具 |
| 01 | `01-gemini-notebook` | 連接 Gemini Notebook（原 NotebookLM）MCP |
| 02 | `02-github` | 連接 GitHub CLI + push 驗證 |
| 03 | `03-obsidian` | 連接 Obsidian MCPVault |
| 04 | `04-firebase` | 連接 Firebase 資料庫 |
| 05 | `05-draw` | 安裝 gpt-image-2 生圖 skill |
| — | `install-all` | 一次安裝全部 |

## 步驟二：讓使用者選擇

問：「你要安裝哪些？輸入全部或編號組合（例如 00, 01, 03）。」

## 步驟三：依序安裝

```bash
npx skills add changyiwu/claude-code-lazy-packs --skill <資料夾名稱> -g -y
```

> 📌 上表的「Skill 名稱」是 **repo 資料夾名**（帶編號，對應懶人包）。
> 每支技能的 frontmatter `name` 則是 `claude-<主題>`（例如 `02-github` → `claude-github`），
> 安裝到全域後資料夾也應該叫 `claude-<主題>`，才不會跟 OpenCode / Codex / AntiGravity
> 的同名技能混在一起。

若無法使用 `npx skills add`，改手動把 `skills/<資料夾名稱>/SKILL.md`
複製到 `~/.claude/skills/claude-<主題>/SKILL.md`。

## 步驟四：回報

每項回報 ✅/⚠️/❌，最後列總表。
