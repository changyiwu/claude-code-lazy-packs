# Claude Code 懶人包 #05：連接 Firebase 資料庫

> 版本：v0.9
> 更新日期：2026-07-24

> 📌 **本懶人包可獨立執行**：會自動檢查並安裝所需工具，不需要先看過其他懶人包。你只要確認下方「先備條件」即可開始。

---

## 這個懶人包會幫你做什麼？

讓你的 Claude Code 桌面版能夠直接操控 Firebase 雲端資料庫（Cloud Firestore），包括：
- 用自然語言建立資料集合（不需要學程式）
- 新增、查詢、修改、刪除資料（透過 Firebase MCP 工具）
- 讓你做的網頁工具能「記住」資料（關掉瀏覽器再開，資料還在）
- 支援即時更新（學生輸入後，展示頁面馬上顯示）
- **千人研習也撐得住**（免費並發連線 100 萬）
- **永遠不會閒置暫停**

> 💡 **對老師而言，這是比 Supabase 更友善的預設選擇**。Supabase（懶人包 #04）適合需要重度 SQL 統計分析的場景，但對絕大多數教學工具，Firebase 的限制更少、更省心。

### Firebase vs Supabase，該選哪個？

| 比較項目 | Supabase（懶人包 #04） | Firebase（本懶人包） |
|---------|----------------------|---------------------|
| 資料庫類型 | SQL（像 Excel 表格） | NoSQL（像 JSON 文件） |
| 免費專案數 | 2 個 | 無限 |
| 閒置暫停 | 一週沒用會暫停 | **不會暫停** |
| **並發連線（免費版）** | **200** | **100 萬** |
| 即時更新 | 有 | 更強（onSnapshot 一行搞定） |
| MCP 工具 | 完整（execute_sql） | **完整**（list/query/add/update/delete + auth + messaging + storage） |
| Claude 操作資料 | ✅ | ✅（2026-04-14 驗證全部 CRUD 可用） |

### 場景對照表

| 場景 | 規模 | 推薦 | 原因 |
|------|------|------|------|
| 第一次接觸資料庫的老師 | — | **Firebase** | 限制少、不會暫停、規模彈性大 |
| 班級成績記錄本 | 30 人 | **Firebase** | 不會暫停，MCP 可直接查 |
| 課堂 IRS、即時互動 | 30 人 | **Firebase** | onSnapshot 即時更新最簡單 |
| 即時文字雲、投票牆 | 不限 | **Firebase** | onSnapshot 即時更新 |
| **教師研習千人 IRS** | **1000 人** | **Firebase**（必選） | 並發 100 萬 vs Supabase 200 |
| 重度 SQL 統計分析 | — | Supabase | SQL JOIN/GROUP BY 強大 |
| 已經用 Supabase | — | 繼續用 | 沒必要換 |

> 💡 **最新發現（2026-04-14）**：Firebase MCP 其實有完整的 Firestore CRUD 工具（list、query、add、update、delete），跟 Supabase MCP 一樣能讓 Claude 用自然語言直接操作資料。
>
> 真正的差異是**規模**和**維護**，不是 MCP 能不能用。

## Firebase MCP 能做什麼？

| 類別 | 工具 | 用途 |
|------|------|------|
| 專案管理 | `firebase_list_projects`、`firebase_create_project` | 建立、列出 Firebase 專案 |
| App 管理 | `firebase_create_app`、`firebase_list_apps` | 建立 Web/iOS/Android app |
| SDK 設定 | `firebase_get_sdk_config` | 取得前端用的設定（apiKey 等） |
| 初始化 | `firebase_init` | 初始化 Firestore、Auth、Hosting 等 |
| 安全規則 | `firebase_get_security_rules`、`firebase_validate_security_rules` | 查看 / 驗證 Firestore、Storage、RTDB 規則 |
| **Firestore CRUD** | `firestore_list_collections`、`firestore_query_collection`、`firestore_get_document`、`firestore_add_document`、`firestore_update_document`、`firestore_delete_document`、`firestore_list_documents` | **完整讀寫 Firestore 資料** |
| Auth | `auth_get_users`、`auth_update_user`、`auth_set_sms_region_policy` | Firebase Authentication 管理 |
| Messaging | `messaging_send_message` | 發送 FCM 推播 |
| Storage | `storage_get_object_download_url` | Cloud Storage 物件下載 |
| Realtime Database | `realtimedatabase_get_data`、`realtimedatabase_set_data` | RTDB 讀寫 |
| Remote Config | `remoteconfig_get_template`、`remoteconfig_update_template` | 遠端設定管理 |
| 文件查詢 | `developerknowledge_search_documents` | 查 Firebase 官方文件 |

> 💡 **`firestore_query_collection` 的 `collection_path` 不要含尾巴 `/`**：寫成 `wordcloud_words` 而非 `wordcloud_words/`，否則會報「Collection id is invalid because it contains /」。

> ⚠️ **上表的 Firestore 工具不會一開始就出現**（2026-07-22 實測）。
> Firebase MCP 啟動時只載入 `core` 與 `developerknowledge` 兩組工具；
> `firestore_*` 要等 MCP 偵測到**一個含 `firebase.json` 的專案目錄**才會註冊。
>
> 如果你發現只有 `firebase_list_projects` 之類的工具、沒有任何 `firestore_*`，
> 用 `firebase_get_environment` 檢查，若顯示 `<NO CONFIG PRESENT>`，就指定專案目錄：
>
> ```
> firebase_update_environment(project_dir="含 firebase.json 的資料夾路徑")
> ```
>
> 設定後 Firestore 工具會立刻出現，**不需要重啟 Claude Code**。
> 這也是為什麼階段一「建立專案 + 寫 firebase.json」不能跳過——
> 就算你的 Google 帳號底下已經有 Firebase 專案，沒有本機專案目錄一樣用不到 Firestore 工具。

---

## 先備條件

在使用這個懶人包之前，請確認：

- [ ] Claude Code 桌面版已安裝且能正常使用（Pro 方案以上）
- [ ] 已有 Google 帳號（Gmail 即可）
- [ ] 電腦有網路連線

---

## 請 Claude Code 幫我執行以下步驟

> ⚠️ 以下內容是給 Claude Code 讀的操作指令。
> 你只需要把這整份 MD 檔丟給 Claude Code 桌面版的 Code 分頁，它會自動開始執行。
> 遇到需要你手動操作的地方，它會暫停並告訴你該怎麼做。
>
> **所有安裝與設定都在 Claude Code 桌面版內完成，不需要另外打開 PowerShell 或命令提示字元。**

---

## 階段一：建立 Firebase 專案

### 步驟零：環境檢查

> 請 Claude 在開始前，先自動確認以下所有項目。
> 如果有任何一項不符合，請先告知使用者問題所在，並引導解決後再繼續。
> **不要跳過任何一項檢查，不要假設環境正常。**

1. **確認作業系統**：執行系統指令確認是 Windows / macOS / Linux，後續所有指令請根據實際的作業系統選擇正確版本執行
2. **確認網路連線正常**
3. **檢查 Node.js 是否已安裝**：執行 `node --version`，如果未安裝：
   - Windows：`winget install --id OpenJS.NodeJS --accept-source-agreements --accept-package-agreements`
   - macOS：`brew install node`
   - Linux：`sudo apt update && sudo apt install nodejs npm -y`
4. **檢查 npx 是否可用**：執行 `npx --version`

> 全部通過後，告知使用者環境狀態並繼續下一步。

---

### 步驟一：建立 Firebase 專案

> 🖐️ **需要手動操作**：
> 1. 請使用者開啟瀏覽器，到 https://console.firebase.google.com
> 2. 點擊「建立專案」（或「Add project」）
> 3. 設定專案名稱（建議用 `my-teaching-tools` 或使用者自訂）
> 4. Google Analytics 可以選擇不啟用（教學工具用不到）
> 5. 點擊「建立專案」，等待建立完成

等使用者確認專案已建立後，繼續下一步。

---

### 步驟二：啟用 Cloud Firestore

> 🖐️ **需要手動操作**：
> 1. 在 Firebase Console 中，點擊左側「Firestore Database」
> 2. 點擊「建立資料庫」
> 3. Standard 版即可，按下一步
> 4. 選擇 Firestore 位置（東亞建議選 `asia-east1 (Taiwan)` 或 `asia-northeast1 (Tokyo)`）
> 5. **安全性規則選「以正式版模式啟動」**（不要選測試模式！）
> 6. 點擊「建立」

> 💡 **為什麼選正式版模式？**
> - 測試模式預設「任何人可讀寫」，但 30 天後規則自動失效，網頁會壞掉
> - 正式版模式預設「全部禁止」，比較安全
> - **規則請你自行在 Console 設定**——本懶人包不會自動建立或部署任何 Firestore 規則，
>   絕不覆蓋你專案現有的線上規則（見步驟二之二）

等使用者確認 Firestore 已啟用後，繼續下一步。

---

### 步驟二之二：建立本機專案設定（讓 MCP 工具生效，**不動線上規則**）

> 🛡️ **本懶人包不會建立、修改或部署任何 Firestore 安全規則。**
> 你專案現有的線上規則完全不會被動到。這一步只是在本機放一個「專案指標」，
> 讓 Firebase MCP 偵測得到專案、把 `firestore_*` 工具開出來（見本檔開頭的說明）。

請 Claude Code 在使用者的專案資料夾建立以下**兩個**檔案（沒有 `firestore.rules`，因為我們不碰規則）：

**1. `firebase.json`**（空設定即可，只為了讓 MCP 偵測到這是 Firebase 專案目錄）

```json
{}
```

**2. `.firebaserc`**（指定預設專案，請替換成使用者的專案 ID）

```json
{
  "projects": {
    "default": "[使用者的 Firebase 專案 ID]"
  }
}
```

建好後，讓 MCP 指向這個資料夾（不需重啟）：

```
firebase_update_environment(project_dir="含上面兩個檔案的資料夾路徑")
```

之後 `firestore_list_collections`、`firestore_add_document` 等工具就會出現，可以直接讀寫資料。

> 🔒 **規則要自己在 Console 設**：Firestore 的安全規則請到
> [Firebase Console → Firestore → 規則](https://console.firebase.google.com) 自行設定。
> 例如某個集合要開放課堂 demo 讀寫，就在 Console 幫該集合加上 `allow read, write: if true`
> （demo 用，正式服務要改成登入或班級碼限制）。
>
> ⚠️ **為什麼不自動部署？** 自動部署會用範本規則**整份覆蓋**你專案現有的規則，
> 很可能弄壞你既有的 app。所以本懶人包一律不碰規則。
>
> 💡 如果你之後真的想讓 Claude 幫忙改規則，請**明確要求**，而且 Claude 會：
> 1. 先用 `firebase_get_security_rules` 讀出你目前的線上規則給你看
> 2. 跟你確認要改成什麼、影響哪些集合
> 3. 得到你同意後才動作——**絕不自動覆蓋**

---

## 階段二：連接 MCP

### 步驟三：安裝 Firebase MCP Server

執行以下指令安裝 Firebase MCP Server：

```bash
claude mcp add firebase --scope user -- npx -y firebase-tools@latest mcp
```

> ⚠️ **桌面版沒有 `claude` CLI 時**，手動寫入 `~/.claude.json` 的最上層
> （保留檔案原有內容，只新增這段）。**不要寫進 `settings.json`**，
> 現行 schema 不接受 `mcpServers`，會回 `Unrecognized field: mcpServers`：
>
> ```json
> {
>   "mcpServers": {
>     "firebase": {
>       "command": "C:/Users/[使用者]/AppData/Roaming/npm/firebase.cmd",
>       "args": ["mcp"]
>     }
>   }
> }
> ```
>
> 💡 若已全域安裝 firebase-tools（`npm i -g firebase-tools`），直接指向 `firebase.cmd`
> 比用 `npx -y firebase-tools@latest` 好——後者每次啟動都會重新解析套件，較慢也較易失敗。
> macOS / Linux 的 command 用 `firebase`，路徑可用 `which firebase` 查。

---

### 步驟四：登入 Firebase CLI

執行以下指令登入 Google 帳號：

```bash
npx -y firebase-tools@latest login
```

> 🖐️ **需要手動操作**：瀏覽器會自動開啟 Google 登入頁面，請使用者選擇帳號並授權。

登入成功後，驗證登入狀態：

```bash
npx -y firebase-tools@latest projects:list
```

應該能看到剛才建立的專案。

> ⚠️ **注意**：`firebase login` 必須在**互動式終端**執行。如果 Claude Code 對話中無法完成（有時會卡住），請使用者打開 cmd / PowerShell 手動跑一次 `npx firebase-tools login`。

---

### 步驟五：重啟 Claude Code 並驗證

> 🖐️ **需要手動操作**：請使用者完全關閉 Claude Code 桌面版，然後重新開啟。

重新開啟後，測試 Firebase 連接是否成功：

1. 嘗試列出所有 Firebase 專案
2. 嘗試查詢 Firestore 中有哪些集合（新專案應該是空的，這是正常的）
3. 如果能成功查詢（即使結果是空的），代表連接成功

> 如果連接失敗，請檢查：
> - 是否已完成 `firebase login`
> - Node.js 是否正常安裝
> - 重新執行步驟三和步驟四

---

### 步驟六：功能測試

連接成功後，執行一個完整的測試：

1. 在 Firestore 建立一個測試集合：
   - 集合名稱：`test_collection`
   - 新增一筆文件，欄位：`message`（文字）、`created_at`（時間戳）
   - message 值：「Firebase 連接測試成功」

2. 查詢 `test_collection`，確認能讀取到剛才新增的資料

3. 刪除測試文件和集合

4. 告知使用者：「連接測試成功！Firebase 已準備好使用。」

---

## 完成！接下來你可以這樣用

| 你說的話 | Claude + Firebase 會做的事 |
|----------|-----------------------------|
| 「幫我做一個即時文字雲網頁，連接 Firebase」 | 產生前端 + 連接 Firestore + 即時更新 |
| 「幫我做一個課堂投票工具」 | 產生網頁 + 前端連 Firestore（規則由你在 Console 開放該集合，懶人包不代改） |
| 「幫我把這個工具推到 GitHub Pages」 | 上線（搭配 #02 GitHub 懶人包） |
| 「看一下我 Firestore 現在的規則」 | 呼叫 `firebase_get_security_rules` 讀出目前規則（唯讀，不修改） |
| 「查一下 wordcloud_words 有幾筆、列出最熱門的 5 個關鍵字」 | 直接呼叫 `firestore_query_collection` 撈資料統計 |
| 「刪掉所有測試資料」 | 呼叫 `firestore_delete_document` |
| 「幫我加一筆示範資料」 | 呼叫 `firestore_add_document` |
| 「幫我查 Firebase 官方文件有沒有 OOO 的做法」 | 查詢官方文件 |

> 💡 **資料操作的方式**：
> - **網頁前端**：用 Firebase JS SDK（ESM import + onSnapshot 即時更新）
> - **Claude 用自然語言查**：透過 Firebase MCP 的 `firestore_query_collection`、`firestore_add_document` 等工具直接操作
>
> Firebase MCP 有完整的 Firestore CRUD 工具，使用體驗跟 Supabase MCP 一致。

---

## 如果安裝失敗，如何重來

對 Claude Code 說：
「Firebase 懶人包執行失敗了，幫我檢查哪裡出問題，重新處理。」

Claude 會自動：
1. 檢查 MCP 連接狀態
2. 確認 Firebase CLI 登入是否正常
3. 找出問題並修復

如果需要完全重置 MCP 連接：

```bash
claude mcp remove firebase
```

然後從步驟三重新開始。

如果需要重新登入 Firebase：

```bash
npx -y firebase-tools@latest logout
npx -y firebase-tools@latest login
```

---

## 常見問題

| 問題 | 解法 |
|------|------|
| `npx: command not found` | 確認 Node.js 已安裝，重啟 Claude Code |
| 連接後查詢失敗 | 確認已執行 `firebase login` 並成功登入 |
| 看不到 Firestore 資料 | 確認已在 Firebase Console 啟用 Cloud Firestore |
| 安全性規則過期 | 表示用了「測試模式」。到 Firebase Console → Firestore → 規則，改成白名單或登入限制（本懶人包不自動改規則） |
| 寫入失敗：Permission denied | Firestore 規則沒允許這個集合。到 Console 幫該集合加上規則（demo 可先 `allow read, write: if true`，正式服務要改成登入或班級碼限制） |
| `Collection id is invalid because it contains /` | `firestore_query_collection` 的 `collection_path` 不能含尾巴 `/`，寫成 `wordcloud_words` 不要 `wordcloud_words/` |
| GitHub Pages 無法啟用 | 免費方案不支援私有 repo 的 Pages，需要將 repo 改成公開 |
| `firebase login` 在 Claude Code 對話中卡住 | 請使用者打開 cmd / PowerShell 手動跑一次 `npx firebase-tools login` |

---

## 免費方案說明（Spark 方案）

| 項目 | 免費額度 | 老師夠用嗎？ |
|------|---------|------------|
| Firestore 儲存 | 1 GB | ✅ 綽綽有餘 |
| Firestore 讀取 | 50,000 次/天 | ✅ 一個班級用不完 |
| Firestore 寫入 | 20,000 次/天 | ✅ 足夠 |
| 專案數量 | 無限 | ✅✅ 比 Supabase 的 2 個好很多 |
| Authentication | 10,000 月活 | ✅ 用不完 |
| 閒置暫停 | 不會暫停 | ✅✅ 不用設防暫停排程 |

> 💡 Firebase 免費方案不會因為閒置而暫停，不需要像 Supabase 一樣設定防暫停排程。

---

## 安全與隱私提醒

- **不要在公開的程式碼中放 Firebase Admin 憑證**
- 前端網頁使用的 Firebase Config（apiKey 等）是設計給前端使用的，可以公開
- 示範時使用假資料，不要放真實學生個資
- **去識別化**：正式使用時，只存座號不存學生真名
- **老師查資料**：直接在 Claude Code 中用自然語言查詢（透過 MCP）

### 建議架構

| 頁面 | 功能 | 需要登入 |
|------|------|---------|
| 學生提交頁 | 答題、輸入文字 | 不需要 |
| 公開展示頁 | 文字雲、即時投票結果 | 不需要 |
| 老師管理 | Claude + MCP 直接查詢 | 不需要（本機操作） |

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-04-12 | v0.1 | 初版 |
| 2026-04-14 | v0.2 | 實作驗證完成，補充 Firebase MCP 工具清單與限制 |
| 2026-04-14 | v0.3 | 改為「正式版模式 + 白名單規則」流程，避免 30 天測試模式過期問題 |
| 2026-04-14 | v0.4 | 規則設定改為 CLI 自動部署，完全免進 Console |
| 2026-04-14 | v0.5 | 加入並發連線比較（Supabase 200 vs Firebase 100 萬）與場景對照表，明確建議千人研習用 Firebase |
| 2026-04-14 | v0.6 | 確認 MCP 可讓 Claude 用自然語言查 Firestore 資料；重新定位為「對老師更友善的預設選擇」 |
| 2026-04-14 | v0.7 | **發現 Firebase MCP 其實有完整 Firestore CRUD 工具**（list/query/add/update/delete），全面修正之前錯誤陳述 |
| 2026-07-24 | v0.9 | **改為完全不碰線上規則**：步驟二之二不再建立/部署 `firestore.rules`，只建 `firebase.json` + `.firebaserc` 讓 MCP 的 `firestore_*` 工具生效；規則改由使用者自行在 Console 設定，若要 Claude 協助改規則會先讀現況並確認、絕不自動覆蓋 |

---

## 相關連結

- [Firebase 官網](https://firebase.google.com)
- [Firebase MCP Server 官方文件](https://firebase.google.com/docs/ai-assistance/mcp-server)
- [Cloud Firestore 文件](https://firebase.google.com/docs/firestore)
- [懶人包 #04：連接 Supabase 資料庫](04-連接-Supabase-資料庫.md)
- [懶人包 #02：連接 GitHub](02-連接-GitHub.md)（教材上線用）
