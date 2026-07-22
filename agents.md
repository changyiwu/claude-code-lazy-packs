# claude-code-lazy-packs（專案藍圖）

> 本檔為跨 Agent 通用的專案藍圖（AGENTS.md 開放標準）。任何 Agent 的每個 session 都應先讀本檔＋`handoff.md`。
> Claude Code 專屬的操作規範另見 `CLAUDE.md`。

## 專案簡介

公開發布的 Claude Code 懶人包全集。每份懶人包是一個編號 MD 檔，使用者丟給 Claude Code 就能自動完成一項設定（環境建置、MCP 串接、API 設定等）；每份懶人包在 `skills/` 有對應的可安裝技能。改編自三師爸（宋睿偉）的 `mathruffian-dot/claude-code-lazy-packs`，README 已具名致謝。

## 關鍵時程

（無固定截止日，持續維護）

## 目標與路線圖

- [x] 階段一：移除班級工具懶人包、全倉重新編號為連續 00–09
- [x] 階段二：修正過時的 MCP 設定說明（`settings.json` → `~/.claude.json`／`.mcp.json`）
- [x] 階段三：技能命名統一 `claude-` 前綴，與其他 agent 區隔
- [ ] 階段四：實測剩餘懶人包 #04 Supabase、#06 Ollama、#07 Gemini（很可能也有過時的 MCP 指引）
- [ ] 階段五：決定 `TEMPLATE.md` 的版本編號規則要沿用原作者還是改成自己的規範
- [x] 階段六：移除「第二大腦設定指南」懶人包，原 05–09 依序遞補為 04–08

## 資料夾結構

```
claude-code-lazy-packs/
├── 00-環境建置.md
├── 01-連接-NotebookLM.md
├── 02-連接-GitHub.md
├── 03-建立第二大腦-Obsidian.md
├── 04-連接-Supabase-資料庫.md
├── 05-連接-Firebase-資料庫.md
├── 06-安裝本地AI-Ollama.md
├── 07-設定Gemini免費API.md
├── 08-安裝gpt-image-2生圖.md
├── skills/              ← 對應的可安裝技能（npx skills add）
│   ├── 00-env-setup/ 01-notebooklm/ 02-github/ 03-obsidian/
│   ├── 04-supabase/ 05-firebase/ 06-ollama/ 07-gemini/
│   ├── 08-draw/
│   └── install-all/     ← 一次安裝全部，不佔編號
├── README.md            ← 使用者入口與懶人包清單
├── SKILL.md             ← AI agent 自動安裝入口
├── TEMPLATE.md          ← 懶人包製作規範
├── agents.md            ← 本檔（跨 Agent 專案藍圖）
├── handoff.md           ← 交接檔
├── CLAUDE.md            ← Claude Code 專屬規範
└── LICENSE              ← MIT，著作權屬原作者，不可移除
```

## 同步層級（本專案初始化至第 3 層級）

| 層級 | 平台 | 位置 | 讀取時機 |
|------|------|------|---------|
| L1 | 本地（GDrive） | `agents.md`＋`handoff.md` | 每個 session |
| L2 | GitHub | [changyiwu/claude-code-lazy-packs](https://github.com/changyiwu/claude-code-lazy-packs)（**public**，刻意公開） | 指定時 |
| L3 | Obsidian | `claude-code-lazy-packs/專案工作流程.md` | 有需要時 |

## 工作約定

- 任何 Agent、任何電腦：**開工先讀 `handoff.md`，收工必更新 `handoff.md`**
- 修改共用檔案前先讀最新內容，避免覆蓋其他 Agent 的變更
- 所有回應與文件使用繁體中文
- 修改前先確認計畫，優先保留原有資料結構

### 本專案特有規則

- **編號一一對應**：懶人包 `NN-主題.md` ↔ `skills/NN-主題/`，編號連續、不重號、不用小數；新增時接續最大編號，不插號
- **技能命名**：repo 資料夾帶編號，但 frontmatter `name` 與安裝後的全域資料夾一律 `claude-<主題>`（例：`skills/02-github/` → `name: claude-github` → `~/.claude/skills/claude-github/`）
- **改懶人包必同步四處**：`README.md` 清單表、`SKILL.md` 對照表、`skills/install-all/SKILL.md` 的順序與總項數、其他懶人包的交叉引用
- **LICENSE 不可動**：MIT 要求保留原作者著作權聲明；`08-安裝gpt-image-2生圖.md` 結尾的作者與頻道資訊同理

## 最近進度

- 2026-07-22：完成 NotebookLM 懶人包 v0.5 的 nlm 0.9.0 實測資訊、桌面版 Claude Code 限制與 MCP 說明同步。
