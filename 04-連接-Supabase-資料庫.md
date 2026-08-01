---
title: 'Claude Code 懶人包 #04：連接 Supabase 資料庫'
date: '2026-04-04'
type: 懶人包
version: v0.3
status: 已更新（OAuth 流程待實測）
tags:
  - Claude-Code
  - 懶人包
  - Supabase
  - 資料庫
  - MCP
---
# Claude Code 懶人包 #04：連接 Supabase 資料庫

> 版本：v0.3
> 更新日期：2026-08-01

> 📌 **本懶人包可獨立執行**：會自動檢查並安裝所需工具，不需要先看過其他懶人包。你只要確認下方「先備條件」即可開始。

---

## 這個懶人包會幫你做什麼？

讓你的 Claude Code 桌面版能夠直接操控 Supabase 雲端資料庫，包括：
- 用自然語言建立資料表（不需要學 SQL）
- 新增、查詢、修改、刪除資料
- 讓你做的網頁工具能「記住」資料（關掉瀏覽器再開，資料還在）
- 支援多人同時使用同一份資料

---

## 先備條件

在使用這個懶人包之前，請確認：

- [ ] Claude Code 桌面版已安裝且能正常使用（Pro 方案以上）
- [ ] 已有 GitHub 帳號（Supabase 可用 GitHub 登入）
- [ ] 電腦有網路連線

---

## 請 Claude Code 幫我執行以下步驟

> ⚠️ 以下內容是給 Claude Code 讀的操作指令。
> 你只需要把這整份 MD 檔丟給 Claude Code 桌面版的 Code 分頁，它會自動開始執行。
> 遇到需要你手動操作的地方，它會暫停並告訴你該怎麼做。
>
> **所有安裝與設定都在 Claude Code 桌面版內完成，不需要另外打開 PowerShell 或命令提示字元。**
> 如果 Claude Code 桌面版無法執行某個指令，才會引導你到終端機操作。
> 進階使用者也可以直接使用 Claude Code CLI 版本來執行本懶人包。

---

## 階段一：建立 Supabase 帳號與專案

### 步驟零：環境檢查

> 請 Claude 在開始前，先自動確認以下所有項目。
> 如果有任何一項不符合，請先告知使用者問題所在，並引導解決後再繼續。
> **不要跳過任何一項檢查，不要假設環境正常。**

1. **確認作業系統**：執行系統指令確認是 Windows / macOS / Linux，後續所有指令請根據實際的作業系統選擇正確版本執行
2. **確認網路連線正常**
3. **確認能開啟瀏覽器**：OAuth 授權需要在瀏覽器完成
4. **檢查是否有 Claude Code CLI**：執行 `claude --version`
   - 有反應 → 步驟四用 CLI 指令
   - 顯示找不到指令 → 你用的是桌面版，屬正常，步驟四改用手動編輯 JSON
5. **檢查 GitHub CLI 是否已登入**：執行 `gh auth status`（Supabase 可用 GitHub 帳號登入）

> ℹ️ **不需要 Node.js**：現行的 Supabase MCP 是遠端服務，不在本機跑 npx。
> 舊版懶人包要求安裝 Node.js 是因為當時用本地 server，現在可以略過。

> 全部通過後，告知使用者環境狀態並繼續下一步。

---

### 步驟一：註冊 Supabase 帳號

> 🖐️ **需要手動操作**：
> 1. 請使用者開啟瀏覽器，到 https://supabase.com
> 2. 點擊「Start your project」
> 3. 選擇「Continue with GitHub」用 GitHub 帳號登入
> 4. 授權 Supabase 存取 GitHub 帳號

等使用者確認已登入後，繼續下一步。

---

### 步驟二：建立 Supabase 專案

> 🖐️ **需要手動操作**：
> 1. 在 Supabase Dashboard 中，點擊「New Project」
> 2. 設定以下資訊：
>    - **Project name**：建議用 `my-teaching-tools`（或使用者自訂）
>    - **Database Password**：設定一個密碼（請記住，之後會用到）
>    - **Region**：選擇離你最近的地區（東亞建議選 `Northeast Asia (Tokyo)`）
> 3. 點擊「Create new project」
> 4. 等待 1-2 分鐘，專案建立完成

---

### 步驟三：取得專案 ref

> 🖐️ **需要手動操作**：
> 1. 在 Supabase Dashboard 中打開你剛建立的專案
> 2. 看瀏覽器網址列，格式是
>    `https://supabase.com/dashboard/project/abcdefghijklmnop`
> 3. 最後那串英數字（例：`abcdefghijklmnop`）就是 **project ref**，複製起來

請使用者把 project ref 貼到對話中。

> ✅ **不需要任何 API key**：現行的 Supabase MCP 用瀏覽器 OAuth 授權，
> **不要**去複製 service_role key 或 anon key。project ref 本來就會出現在公開的 API 網址裡，不是機密。

---

## 階段二：連接 MCP

> ℹ️ **注意連接方式已經改變**：Supabase 官方現在提供**遠端（remote）MCP server**，
> 用瀏覽器 OAuth 授權，不需要在本機跑 npx、也不需要任何 key。
> 舊教學裡 `npx -y @supabase/mcp-server-supabase --supabase-url ... --supabase-service-role-key ...` 的寫法
> **參數已經不存在**，照打會出現 `ERR_PARSE_ARGS_UNKNOWN_OPTION`。

### 步驟四：加入 Supabase MCP Server

**如果有 Claude Code CLI**（終端機打 `claude --version` 有反應）：

```bash
claude mcp add --scope user --transport http supabase "https://mcp.supabase.com/mcp?project_ref=[使用者的project_ref]&read_only=true"
```

> 把 `[使用者的project_ref]` 換成步驟三取得的值。
> `read_only=true` 先用唯讀模式，確認一切正常後再視需要拿掉（見步驟六）。

**如果是 Claude Code 桌面版**（沒有 `claude` 指令，這是正常的）：

手動編輯 `~/.claude.json`，在 `mcpServers` 區塊加入：

```json
{
  "mcpServers": {
    "supabase": {
      "type": "http",
      "url": "https://mcp.supabase.com/mcp?project_ref=[使用者的project_ref]&read_only=true"
    }
  }
}
```

> ⚠️ **三個常見錯誤**：
> - **不要寫進 `settings.json`**：該 schema 不接受 `mcpServers`，會被驗證擋下並報 `Unrecognized field: mcpServers`
> - **`type` 不能省略**：只寫 `url` 沒寫 `type`，Claude Code 會當成 stdio server 而跳過，並報
>   `MCP server "supabase" has a "url" but no "type"`
> - 保留檔案原有內容，只新增這個 server，不要覆蓋其他 MCP 設定
>
> 只想在單一專案使用的話，改寫在專案根目錄的 `.mcp.json`（格式相同）。

---

### 步驟五：重啟並完成 OAuth 授權

> 🖐️ **需要手動操作**：請使用者完全關閉 Claude Code 桌面版，然後重新開啟。

重新開啟後，執行 `/mcp` 指令進行授權：

1. 在 Claude Code 輸入 `/mcp`
2. 選擇 `supabase`，會跳出瀏覽器要求授權
3. 用你的 Supabase 帳號登入並同意授權
4. 回到 Claude Code，狀態應顯示已連線

授權完成後，測試連接：

1. 請 Claude 查詢資料庫中有哪些資料表（新專案應該是空的，這是正常的）
2. 如果能成功查詢（即使結果是空的），代表連接成功

> 如果連接失敗，請檢查：
> - project ref 是否正確（對照 Dashboard 網址列）
> - 設定是否寫在 `~/.claude.json` 而非 `settings.json`
> - JSON 裡是否漏了 `"type": "http"`
> - 是否完全關閉再重開 Claude Code（不是只關視窗）

---

### 步驟六：開啟寫入權限並做功能測試

步驟四設定的是**唯讀模式**，只能查詢、不能建表。要讓 Claude 幫你建資料表，先解除唯讀：

> 🖐️ **需要手動操作**：把設定裡的 `&read_only=true` 拿掉（CLI 版可先 `claude mcp remove supabase` 再重加），
> 然後完全關閉並重開 Claude Code。

> 💡 **建議做法**：日常查詢資料保持 `read_only=true`，只有在需要建表或改資料時才暫時解除。
> 這樣可以避免 AI 誤刪資料。

解除唯讀後，執行一個完整的測試：

1. 建立一個測試資料表：
   - 表名：`test_table`
   - 欄位：`id`（自動遞增）、`name`（文字）、`created_at`（時間戳）

2. 新增一筆測試資料：
   - name: 「Supabase 連接測試成功」

3. 查詢資料，確認能讀取到剛才新增的資料

4. 詢問使用者是否刪除測試資料表，取得同意後再刪除

5. 告知使用者：「連接測試成功！接下來設定自動防暫停。」

---

### 步驟七：設定自動防暫停排程

> Supabase 免費專案閒置一週會自動暫停。設定一個每週自動查詢，就能永遠防止暫停。

請使用 Claude Code 的排程功能，建立一個每週自動執行的任務：

- **任務內容**：列出 Supabase 資料庫的資料表清單，確認資料庫回應正常
  （列表查詢即使在空資料庫也會成功，不依賴特定資料表存在，唯讀模式下也能執行）
- **排程頻率**：每週執行一次
- **目的**：防止 Supabase 免費專案因閒置而暫停

設定完成後，告知使用者：
「✅ 全部完成！你的 Claude Code 已成功連接 Supabase 資料庫，並設定了每週自動防暫停。」

---

## 完成！接下來你可以這樣用

| 你說的話 | Claude + Supabase 會做的事 |
|----------|--------------------------|
| 「幫我建一個學生成績的資料表」 | 自動建立資料表 + 設定欄位 |
| 「幫我新增一筆學生成績」 | 插入資料到資料庫 |
| 「幫我查詢全班數學成績的平均」 | 查詢資料庫並計算 |
| 「幫我做一個成績管理網頁，連接資料庫」 | 產生前端 + 連接 Supabase + 資料持久化 |
| 「幫我把這個工具推到 GitHub Pages」 | 上線（搭配懶人包 #02 的 GitHub） |

---

## 如果安裝失敗，如何重來

對 Claude Code 說：
「Supabase 懶人包執行失敗了，幫我檢查哪裡出問題，重新處理。」

Claude 會自動：
1. 檢查 MCP 連接狀態
2. 確認 project ref 與 OAuth 授權狀態
3. 找出問題並修復

如果需要完全重置 MCP 連接：

```bash
claude mcp remove supabase
```

桌面版沒有 CLI，改為手動把 `~/.claude.json` 裡的 `supabase` 區塊刪掉。然後從步驟四重新開始。

---

## 常見問題

| 問題 | 解法 |
|------|------|
| `ERR_PARSE_ARGS_UNKNOWN_OPTION` | 你用到舊教學的 `--supabase-url` / `--supabase-service-role-key`，這兩個參數已不存在。改用步驟四的遠端 MCP 寫法 |
| `Unrecognized field: mcpServers` | 寫錯檔案了。MCP 設定要放 `~/.claude.json` 或專案的 `.mcp.json`，不能放 `settings.json` |
| `has a "url" but no "type"` | JSON 裡漏了 `"type": "http"`，補上即可 |
| `claude: command not found` | 桌面版沒有 CLI，屬正常。改用步驟四的手動 JSON 寫法 |
| `/mcp` 授權後仍顯示未連線 | 完全關閉 Claude Code（不是只關視窗）再重開，然後重跑 `/mcp` |
| 查詢正常但建表失敗 | 還在唯讀模式。拿掉網址裡的 `&read_only=true` 並重啟（見步驟六） |
| Supabase 專案顯示「Paused」 | 懶人包已設定每週自動防暫停。如果仍然暫停，到 Dashboard 點「Restore」即可 |
| 找不到 project ref | 打開 Dashboard 的專案頁，網址 `/dashboard/project/` 後面那串就是 |
| （實作後持續補充） | |

---

## 免費方案說明

| 項目 | 免費額度 | 老師夠用嗎？ |
|------|---------|------------|
| 資料庫儲存 | 500 MB | ✅ 一個班級的成績記錄遠遠不到 |
| 檔案儲存 | 1 GB | ✅ 足夠 |
| 月活用戶 | 50,000 | ✅ 全校師生都夠用 |
| API 請求 | 無限 | ✅ 不用擔心 |
| 專案數量 | 2 個 | ⚠️ 免費只能有 2 個專案 |

> ⚠️ 免費專案閒置一週會自動暫停，重新啟動即可，資料不會消失。

---

## 安全提醒

- **不需要也不要提供 service_role key**：現行的 MCP 用 OAuth 授權，任何要你把 service_role key 貼進對話的教學都是舊的。service_role key 有完整資料庫權限，一旦外流等於整個資料庫被拿走
- **平常保持 `read_only=true`**：需要建表或改資料時才暫時解除，用完改回來
- 示範時使用假資料（假名、假成績），不要放真實學生個資
- 如果程式碼要連 Supabase，前端只能用 anon key + Row Level Security，**service_role key 永遠只能待在後端**
- Supabase MCP 適合開發和測試，如果要正式使用需注意隱私法規

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-04-04 | v0.1 | 初版 |
| 2026-04-04 | v0.2 | 加入環境檢查、復原機制、安全提醒、免費方案說明 |
| 2026-08-01 | v0.3 | **階段二全面改寫**：Supabase 官方已改為遠端 MCP（`https://mcp.supabase.com/mcp`）+ 瀏覽器 OAuth。移除 service_role key 相關步驟（MCP 從不使用該 key，且原本要求貼進對話有安全疑慮），改為只需 project ref；補上桌面版手動寫 `~/.claude.json` 的 fallback 與 `"type": "http"` 提醒；預設唯讀並說明如何解除。<br>⚠️ 遠端 MCP 的 OAuth 流程依官方文件撰寫，**尚未實機驗證** |

---

## 相關連結

- [Supabase 官網](https://supabase.com)
- [Supabase MCP 官方文件](https://supabase.com/docs/guides/getting-started/mcp)
- [Supabase MCP GitHub](https://github.com/supabase-community/supabase-mcp)
- [[02-連接-GitHub|懶人包 #02：連接 GitHub]]
- [[README|Claude Code 懶人包索引]]
