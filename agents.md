# claude-code-lazy-packs（專案藍圖）

> 本檔為跨 Agent 通用的專案藍圖（AGENTS.md 開放標準）。任何 Agent 的每個 session 都應先讀本檔＋`handoff.md`。
> Claude Code 不讀 `agents.md`，改由 `CLAUDE.md` 的 `@agents.md` import 本檔；Claude 專屬的補充寫在 `CLAUDE.md`。

## 專案簡介

公開發布的 Claude Code 懶人包全集。每份懶人包是一個編號 MD 檔，使用者丟給 Claude Code 就能自動完成一項設定（環境建置、MCP 串接、API 設定等）；每份懶人包在 `skills/` 有對應的可安裝技能。改編自原作者三師爸的 `mathruffian-dot/claude-code-lazy-packs`，README 已具名致謝。

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

## 三個檔案的職責（依「時效性」分家，不是依「詳細程度」）

| 檔案 | 時效 | 寫入方式 | 放什麼 |
|------|------|---------|--------|
| `handoff.md` | **只對下一個 session 有效**，過期即丟 | 每次收工整份重寫 | 做到哪、下一步、**這次**的暫時 workaround |
| `agents.md`（本檔） | **長期有效**，每個 session 都適用 | 只有規則本身變了才改 | 目標、路線圖、常設規則、結構 |
| Obsidian／`git log` | **歷史**：發生過什麼、為什麼 | 只增不刪 | 決策紀錄、踩坑完整版、逐次進度 |

驗收標準：**`handoff.md` 整份刪掉，不應損失任何長期資訊**——會的話代表該升級進本檔卻沒升級。

**本檔不要出現的東西**：❌ `## 最近進度`／逐次工作紀錄、❌ 決策理由與踩坑完整版。2026-08-03 移除了 `## 最近進度`，內容逐條比對後已在 L3 筆記的〈🗓️ 最近更動紀錄〉——**是主動移除，不是遺漏，不要補回來**。踩過的坑只把**結論**收斂成一條祈使句寫進〈工作約定〉，原因留 L3。

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
- **遠端 MCP 的 JSON 一定要寫 `"type": "http"`**，只有 `url` 會被當成 stdio server 而直接跳過
- **桌面版 Claude Code 沒有 `claude` CLI**，MCP 設定要用 `~/.claude.json` 或專案的 `.mcp.json`，**不可寫入 `settings.json`**
- **看到指令裡還是 `notebooklm` 不要當成漏改**（見 #01 開頭的對照表）：套件名、CLI、認證目錄都沒有跟著更名
- **`git rm` 在 Windows 會留下空資料夾**，刪技能後記得用 `Remove-Item` 檢查並清掉
- **階段九（Supabase 遠端 MCP 的 OAuth 實機驗證）已取消**，標的已下架，不用再補跑；階段四對 #06 #07 的修正隨檔案一起消失，要復活的話內容在 `0537ab8`
- repo 為公開專案，不提交 `.mcp.json`、本機路徑、API key 或 token
