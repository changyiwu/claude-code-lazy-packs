# claude-code-lazy-packs（專案藍圖）

> 本檔為跨 Agent 通用的專案藍圖（AGENTS.md 開放標準）。任何 Agent 的每個 session 都應先讀本檔＋`handoff.md`。
> Claude Code 不讀 `agents.md`，改由 `CLAUDE.md` 的 `@agents.md` import 本檔；Claude 專屬的補充寫在 `CLAUDE.md`。

## 專案簡介

公開發布的 Claude Code 懶人包全集。每份懶人包是一個編號 MD 檔，使用者丟給 Claude Code 就能自動完成一項設定（環境建置、MCP 串接、API 設定等）；每份懶人包在 `skills/` 有對應的可安裝技能。改編自三師爸（宋睿偉）的 `mathruffian-dot/claude-code-lazy-packs`，README 已具名致謝。

## 關鍵時程

（無固定截止日，持續維護）

## 目標與路線圖

- [x] 階段一：移除班級工具懶人包、全倉重新編號為連續 00–09
- [x] 階段二：修正過時的 MCP 設定說明（`settings.json` → `~/.claude.json`／`.mcp.json`）
- [x] 階段三：技能命名統一 `claude-` 前綴，與其他 agent 區隔
- [x] 階段四：實測 #04 Supabase、#06 Ollama、#07 Gemini 並修正（三份皆升 v0.3；`settings.json` 過時指引已於階段二清乾淨，本次確認無殘留）
- [ ] 階段五：決定 `TEMPLATE.md` 的版本編號規則要沿用原作者還是改成自己的規範
- [x] 階段六：移除「第二大腦設定指南」懶人包，原 05–09 依序遞補為 04–08
- [x] 階段七：跟進 NotebookLM → Gemini Notebook 更名（#01 檔名、技能名、全域副本）
- [ ] 階段八：定期巡檢各懶人包引用的外部產品／套件是否改名或搬家（本次更名是被動發現的）
- [x] ~~階段九：補跑 #04 Supabase 遠端 MCP 的 OAuth 實機驗證~~（**已取消**：Supabase 懶人包於階段十一下架，無需驗證）
- [ ] 階段十（評估中）：四個 agent 懶人包的共用內容抽成「單一來源＋產生器」，對外維持四個公開 repo（本次擱置，待階段四之後再議）
- [x] 階段十一：下架 Supabase／Ollama／Gemini 免費 API 三份懶人包與技能，原 05→04、08→05 遞補，全倉編號回到連續 00–05

## 資料夾結構

```
claude-code-lazy-packs/
├── 00-環境建置.md
├── 01-連接-Gemini-Notebook.md
├── 02-連接-GitHub.md
├── 03-建立第二大腦-Obsidian.md
├── 04-連接-Firebase-資料庫.md
├── 05-安裝gpt-image-2生圖.md
├── skills/              ← 對應的可安裝技能（npx skills add）
│   ├── 00-env-setup/ 01-gemini-notebook/ 02-github/ 03-obsidian/
│   ├── 04-firebase/ 05-draw/
│   └── install-all/     ← 一次安裝全部，不佔編號
├── README.md            ← 使用者入口與懶人包清單
├── SKILL.md             ← AI agent 自動安裝入口
├── TEMPLATE.md          ← 懶人包製作規範
├── agents.md            ← 本檔（跨 Agent 專案藍圖）
├── handoff.md           ← 交接檔
├── CLAUDE.md            ← 橋接檔（@agents.md）＋ Claude Code 專屬補充
└── LICENSE              ← MIT，著作權屬原作者，不可移除
```

## 同步層級（本專案初始化至第 3 層級）

| 層級 | 平台 | 位置 | 讀取時機 |
|------|------|------|---------|
| L1 | 本地（GDrive） | `agents.md`＋`handoff.md`＋`CLAUDE.md`（橋接） | 每個 session |
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
- **LICENSE 不可動**：MIT 要求保留原作者著作權聲明；`05-安裝gpt-image-2生圖.md` 結尾的作者與頻道資訊同理

## 最近進度

- 2026-08-02：階段十一完成。下架 Supabase／Ollama／Gemini 免費 API 三份懶人包與技能，Firebase 05→04、生圖 08→05 遞補，編號回到連續 00–05；Firebase 包升 v1.0（移除指向已刪 Supabase 包的死連結，比較表保留為純產品對照）；install-all 總項數 9→6；階段四才實測過的三份中有兩份在此下架，階段九（Supabase OAuth 實測）連帶取消。
- 2026-08-01：階段四完成。#04 Supabase 階段二全面改寫（官方已改遠端 MCP + OAuth，移除 service_role key 流程）、#06 Ollama 修 `wmic` 與 CORS 語法、#07 Gemini 修 zsh 與模型代號；三份技能檔同步，其中兩個原有硬錯誤（`gemini-2.0-flash` 已停服、`@ollama/mcp-server` 不存在於 npm）。
- 2026-08-01：NotebookLM 更名 Gemini Notebook，#01 懶人包升 v0.6、技能改名 `claude-gemini-notebook`，四個 agent 全域副本對齊。
- 2026-07-22：完成 NotebookLM 懶人包 v0.5 的 nlm 0.9.0 實測資訊、桌面版 Claude Code 限制與 MCP 說明同步。
- 2026-07-22：移除 #04 第二大腦設定指南，原 05–09 遞補為 04–08；生圖技能全域路徑統一為 `claude-draw`。
