---
title: "DeepSeek 企業知識庫 RAG 落地手冊"
description: "從文檔切片、向量檢索到 DeepSeek 生成答案，整理企業內部知識庫 RAG 的最小可行架構與常見踩坑。"
date: 2026-07-02
updated: 2026-07-02
featured: true
tags: ["企業 RAG"]
layout: "layouts/post.njk"
permalink: "/posts/deepseek-enterprise-rag-playbook/index.html"
---

企業導入 DeepSeek 最常見的場景不是「聊天」，而是**用內部文檔回答員工問題**。RAG（檢索增強生成）是這類場景的標準架構。

## 最小可行架構

```
員工提問 → 向量檢索（Top-K 片段）→ 組裝 Prompt → DeepSeek 生成 → 附引用來源
```

組件選型示例（可按團隊技術棧替換）：

- **向量庫**：pgvector、Milvus、Weaviate
- **Embedding**：與檢索模型一致的嵌入服務
- **生成**：DeepSeek API（回答層）
- **網關**：統一鑑權、審計、限流

## 文檔準備：決定成敗的 80%

1. **清洗**：去掉頁眉頁腳、重複模板、過期版本
2. **切片**：按段落或標題切，單塊建議 300–800 字
3. **元數據**：部門、版本號、生效日期、語言
4. **權限**：檢索前過濾用戶可見文檔 ID

## Prompt 模板（帶引用）

```
你是企業內部助手。僅根據以下「參考資料」回答，資料不足時說「內部文檔未覆蓋」。
每個結論後標註 [來源: 文檔標題 § 章節]。
使用香港繁體中文。

參考資料：
{retrieved_chunks}

問題：{user_query}
```

## 常見踩坑

| 問題 | 原因 | 修正 |
|------|------|------|
| 回答正確但來源錯 | 切片太碎 | 加大上下文或合併相鄰塊 |
| 幻覺政策條款 | 檢索未命中 | 降低 temperature，強制引用 |
| 英文文檔回答中文混亂 | 語言未約束 | 見 [多語言輸出指南](/posts/deepseek-multilingual-output/) |
| 延遲高 | K 過大 | Top-K 從 5 降到 3，預先摘要長文 |

## 私有化 vs 雲端 API

- **高度敏感**：本地推理 + 內網向量庫，API 不出網
- **一般內部 FAQ**：雲端 API + 脫敏文檔 + 網關審計

## 相關教程

- [國際版 API 接入](/posts/deepseek-global-api-onboarding/)
- [多語言輸出實戰](/posts/deepseek-multilingual-output/)
- [產業 ROI 評估](/posts/deepseek-industry-roi-guide/)
