---
name: claude-gemini
description: Claude Code 設定 Gemini 免費 API。說「設定 Gemini」「Gemini API」時載入。
---

# 設定 Gemini 免費 API（Claude Code 版）

1. 到 https://aistudio.google.com/apikey 建立免費 API key（不需信用卡）
2. 存進環境變數，不要寫在程式碼裡：
   - **Windows**：`setx GEMINI_API_KEY "你的key"`（設定後需重啟 Claude Code）
   - **macOS**：寫入 `~/.zshrc`（Catalina 後預設 zsh）
   - **Linux**：寫入 `~/.bashrc`
   - 先用 `echo $SHELL` 確認，**寫錯檔案不會生效**
3. 測試：

   ```bash
   curl "https://generativelanguage.googleapis.com/v1beta/models/gemini-3.6-flash:generateContent?key=$GEMINI_API_KEY" \
     -H "Content-Type: application/json" \
     -d '{"contents":[{"parts":[{"text":"請用繁體中文回答：1+1 等於多少？"}]}]}'
   ```

## 模型選擇

| 模型 | 場景 |
|---|---|
| `gemini-3.6-flash` | 速度與智慧平衡，一般用途首選 |
| `gemini-3.5-flash-lite` | 最快最省，大量簡單請求 |
| `gemini-2.5-flash` | 仍可用，相容既有程式 |
| `gemini-2.5-pro` | 複雜推理 |

⚠️ **`gemini-2.0-flash` 與 `2.0-flash-lite` 已停止服務**，舊程式碼要改掉，否則回傳 404。
模型清單以 https://ai.google.dev/gemini-api/docs/models 為準。

⚠️ API key 不放進 HTML/JS 前端原始碼、不推上 GitHub。正式工具透過後端代理呼叫。

回報：key 是否成功寫入環境變數、測試回應結果、使用的模型代號。
