@agents.md

<!--
  本檔是「橋接檔」：Claude Code 只讀 CLAUDE.md，不讀 agents.md，
  所以用第一行的 @agents.md 把跨 Agent 專案藍圖 import 進來。
  專案內容（簡介、資料夾結構、編號規則、路線圖）一律寫在 agents.md，
  這裡只放 Claude Code 專屬的補充，避免兩份分叉。
-->

# CLAUDE.md — claude-code-lazy-packs

專案藍圖已由上面的 `@agents.md` 載入。以下是 Claude Code 專屬補充。

## 本機路徑

`C:\Users\chang\我的雲端硬碟\agents\claude-code-lazy-packs`

## 技能命名前綴的理由

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

## 改完的收尾

同步檢查清單（README 清單表、SKILL.md 對照表、install-all 的順序與總項數、交叉引用）見 `agents.md` 的「本專案特有規則」。改完在本資料夾執行：

```bash
git add <檔案>
git commit -m "<說明>"
git push origin main
```
