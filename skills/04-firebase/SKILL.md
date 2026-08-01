---
name: claude-firebase
description: Claude Code 連接 Firebase MCP。說「連接 Firebase」時載入。
---

# 連接 Firebase（Claude Code 版）

1. `npm install -g firebase-tools` → `firebase login`
2. 在專案目錄放 `firebase.json`（可空 `{}`）+ `.firebaserc`（指定專案 ID），讓 MCP 偵測到專案。**不要跑會覆蓋規則的部署**
3. 用 `claude mcp add firebase -- npx -y firebase-tools@latest mcp`；沒有 CLI 時手動寫入 `~/.claude.json` 的 `mcpServers` 或專案的 `.mcp.json`（**不是 `settings.json`**）
4. 重啟後驗證：列出專案；用 `firebase_update_environment` 指向專案目錄讓 `firestore_*` 工具出現

⚠️ Admin SDK 憑證不可公開，學生資料只存代號。
🛡️ **不動線上規則**：本流程不建立/部署 `firestore.rules`，安全規則請自行在 Firebase Console 設定。
