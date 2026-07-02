---
title: "DeepSeek 多語言輸出：中英印地語 Prompt 實戰"
description: "面向南亞與東南亞業務，說明如何用 DeepSeek 穩定輸出繁中、英文與 Hindi 內容，並避免混語與文化誤譯。"
date: 2026-07-02
updated: 2026-07-02
featured: true
tags: ["多語言"]
layout: "layouts/post.njk"
permalink: "/posts/deepseek-multilingual-output/index.html"
---

國際團隊用 DeepSeek 時，失敗模式往往不是翻譯錯，而是**同一段裡中英印混用**或**語氣不符合當地市場**。

## 語言鎖定三件套

1. **目標語言寫在首行**：`請僅使用英文回答，專有名詞保留原文。`
2. **給一個輸出示例**（Few-Shot）：比長篇描述更有效
3. **禁止項**：`不要輸出簡體中文` / `不要使用羅馬拼音混寫 Hindi`

## 場景模板

**繁中 → 英文（B2B 郵件）**

```
將下面繁體中文郵件改寫為正式商務英文，100–120 詞，保留數字與專有名詞：
---
[正文]
```

**英文 → Hindi（客服短回覆）**

```
將下面英文客服回覆翻譯為 Hindi（天城文），語氣禮貌、句子短，適合 WhatsApp：
---
[英文原文]
```

**三語對照 FAQ**

```
根據下面中文 FAQ，輸出 JSON 陣列，每項含 zh-HK、en、hi 三個字段，不要添加未提供的問答：
---
[FAQ 原文]
```

## 質量檢查表

- [ ] 是否出現未要求的第四種語言
- [ ] 貨幣、日期格式是否符合目標市場（INR / HKD / USD）
- [ ] 宗教、節日、地名是否需本地化審核
- [ ] 法律/合規條款是否標註「需人工審閱」

## 與 RAG 結合

多語言知識庫應在切片元數據標註 `lang`，檢索時**只召回與用戶語言一致的塊**，再交 DeepSeek 生成。詳見 [企業 RAG 手冊](/posts/deepseek-enterprise-rag-playbook/)。

## 相關教程

- [國際版 API 接入](/posts/deepseek-global-api-onboarding/)
- [Slack / Teams 整合](/posts/deepseek-slack-teams-workflow/)
- [產業 ROI 評估](/posts/deepseek-industry-roi-guide/)
