---
title: "DeepSeek 整合 Slack / Teams 自動化工作流"
description: "說明如何把 DeepSeek API 接入 Slack 或 Microsoft Teams，實現工單摘要、站內搜尋與審批草稿等自動化場景。"
date: 2026-07-02
updated: 2026-07-02
featured: true
tags: ["協作整合"]
layout: "layouts/post.njk"
permalink: "/posts/deepseek-slack-teams-workflow/index.html"
---

員工已習慣在 Slack / Teams 裡完成工作。把 DeepSeek 放進協作工具，比單獨開一個聊天網頁更容易落地。

## 典型用例

| 用例 | 觸發 | DeepSeek 做什麼 |
|------|------|-----------------|
| 工單摘要 | 新 Jira 留言 | 3 句話摘要 + 待辦 |
| 站內搜尋 | `/ask 政策` | RAG 檢索後回答 |
| 審批草稿 | 表單提交 | 生成審批意見模板 |
| 會議跟進 | 會後上傳筆記 | 輸出 Action Items |

## 架構示意

```
Slack/Teams Event → 你的 Bot 服務 →（可選）RAG 檢索 → DeepSeek API → 回覆線程
```

關鍵中間層職責：

- **鑑權**：只處理本企業工作區事件
- **脫敏**：進模型前遮罩員工號、客戶 ID
- **限流**：按頻道或使用者配額，避免 429

## Slack 實作要點

1. 建立 Slack App，訂閱 `app_mention` 或斜線命令
2. 後端用 Bolt / Node 或 Python 接收事件
3. 長任務先回「處理中」，完成後 `chat.postMessage` 更新線程
4. 勿在頻道公開顯示完整 API Key 或原始錯誤堆疊

## Microsoft Teams 要點

- 使用 Bot Framework 註冊 Bot，配置 Messaging endpoint
- Adaptive Card 展示結構化答案（表格、按鈕）
- 注意 Teams 租戶策略，部分地區需合規審批

## 安全與合規

- 對話日誌保留期限與 GDPR / 本地個資法對齊
- 高風險頻道（法務、HR）可禁用外部模型，僅允許內網部署
- API 調用統一走 [國際版接入網關](/posts/deepseek-global-api-onboarding/)

## 相關教程

- [企業 RAG 手冊](/posts/deepseek-enterprise-rag-playbook/)
- [多語言輸出實戰](/posts/deepseek-multilingual-output/)
- [產業 ROI 評估](/posts/deepseek-industry-roi-guide/)
