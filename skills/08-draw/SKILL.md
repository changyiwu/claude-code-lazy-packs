---
name: claude-draw
description: OpenAI gpt-image-2 生圖技能（全域可用）。當使用者要求「畫一張」、「生一張圖」、「做一張圖」、「產生圖片」、「畫個封面」、「畫插圖」、「畫示意圖」、「畫分鏡」等任何需要 AI 生成圖像的情境時，請一定要使用此技能。此技能會呼叫本地腳本以 gpt-image-2 模型生圖，自動判斷 quality 等級（預設 low），存檔至當前專案的 slides/generated/ 目錄（若無則建於當下工作目錄）。
---

# 生圖技能（gpt-image-2）

> 前置安裝（API Key／儲值／Individual 驗證／裝 openai 套件）請看懶人包 `08-安裝gpt-image-2生圖.md`。
> 本 skill 是安裝完成後「拿來就能用」的操作版。

## 觸發情境
使用者說出：
- 「畫一張 XX」「生一張圖」「做一張圖」
- 「畫個封面／插圖／示意圖／分鏡」
- 「產生圖片」「幫我生圖」
- 「改這張圖」「修改圖片」「把背景換成 XX」（→ 改圖模式，需提供圖片路徑）

## 腳本位置
`$HOME/.claude/skills/claude-draw/draw.py`（Windows PowerShell 與 macOS/Linux 通用，不要寫死使用者名稱）

## 使用方式
```bash
python "$HOME/.claude/skills/claude-draw/draw.py" "要畫的內容" --name 檔名前綴
```

### 參數
- `prompt`（必填）：要畫什麼
- `--size`：`1024x1024`（方，預設）/ `1536x1024`（橫）/ `1024x1536`（直）
- `--quality`：`low`（預設，NT$0.3）/ `medium`（NT$1.3）/ `high`（NT$5.5）/ `auto`（交給模型決定，費用不可預測）
- `--n`：生成張數 1–8
- `--name`：檔名前綴
- `--outdir`：輸出目錄
- `--edit IMAGE_PATH`：改圖模式（指定來源圖）
- `--mask MASK_PATH`：遮罩圖片（搭配 --edit 使用）

## 判斷 quality 等級的原則
**預設永遠用 `low`**（省錢 + 速度優先）

- **low**（NT$0.3）：**99% 情境**。演講簡報、教學插圖、封面、demo 都夠。
- **medium**（NT$1.3）：通常不用。
- **high**（NT$5.5）：實體印刷、跨語言文字零錯才用。
- **auto**：不要用。等級由模型決定，費用不可預測。

不確定就 **low**，不要自作主張升級。

## 錯誤處理
- `403 Organization must be verified` → 到 platform.openai.com/settings/organization/general 做 Individual 驗證
- `401 Invalid API key` → 檢查 `~/.openai.env`
- `429 Rate limit` → 額度用完，到 Billing 儲值

## 輸出
PNG 檔，格式：`<name>_<YYYYMMDD_HHMMSS>.png`
