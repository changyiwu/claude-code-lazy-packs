# CLAUDE.md — claude-code-lazy-packs

## 這個資料夾是什麼

Claude Code 懶人包全集。每份懶人包是一個 MD 檔，使用者丟給 Claude Code 就能自動完成一項設定（環境建置、MCP 串接、API 設定等）。

- **本機路徑**：`C:\Users\chang\我的雲端硬碟\agents\claude-code-lazy-packs`
- **GitHub**：https://github.com/changyiwu/claude-code-lazy-packs

## 檔案結構

```
claude-code-lazy-packs/
├── 00-環境建置.md
├── 01-連接-NotebookLM.md
├── 02-連接-GitHub.md
├── 03-建立第二大腦-Obsidian.md
├── 04-第二大腦設定指南.md
├── 05-連接-Supabase-資料庫.md
├── 06-連接-Firebase-資料庫.md
├── 07-安裝本地AI-Ollama.md
├── 08-設定Gemini免費API.md
├── 09-安裝gpt-image-2生圖.md
├── skills/              ← 對應的可安裝技能（npx skills add）
│   ├── 00-env-setup/ … 09-draw/
│   └── install-all/     ← 一次安裝全部，不佔編號
├── README.md            ← 使用者入口與懶人包清單
├── SKILL.md             ← AI agent 自動安裝入口
├── TEMPLATE.md          ← 懶人包製作規範
├── CLAUDE.md
└── LICENSE
```

## 編號規則

- 懶人包本體與 `skills/` 資料夾**編號一一對應**（`05-連接-Supabase-資料庫.md` ↔ `skills/05-supabase/`）
- 編號連續、不重號、不使用小數
- 新增懶人包時接續最大編號，不插號
- 非特定主題的技能（如 `install-all`）不佔編號

## 技能命名規則

repo 內的資料夾帶編號（方便對應懶人包），但**技能名稱一律用 `claude-` 前綴**：

| | 格式 | 範例 |
|---|------|------|
| repo 資料夾 | `<編號>-<主題>` | `skills/02-github/` |
| frontmatter `name` | `claude-<主題>` | `name: claude-github` |
| 安裝後的全域資料夾 | `claude-<主題>` | `~/.claude/skills/claude-github/` |

前綴是為了跟同一台電腦上其他 agent 的技能區隔——OpenCode 用 `opencode-`、
Codex 用 `codex-`、AntiGravity 用 `antigravity-`，全部裝在各自的全域技能目錄，
沒有前綴會分不出來是哪個 agent 的。

> ⚠️ 新增技能時，`name:` 一定要是 `claude-<主題>`，且**與安裝後的資料夾同名**
> （其他 agent 的慣例是資料夾名 == `name`，保持一致才好對照）。

## 修改懶人包時的注意事項

新增或修改懶人包後，記得同步更新：

1. `README.md` 的懶人包清單表格
2. `SKILL.md` 的技能對照表
3. `skills/install-all/SKILL.md` 的安裝順序與總項數
4. 其他懶人包裡的交叉引用（`懶人包 #XX`、`[[wikilink]]`、相對路徑連結）

改完在本資料夾執行：

```bash
git add <檔案>
git commit -m "<說明>"
git push origin main
```
