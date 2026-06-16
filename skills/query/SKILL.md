---
name: "llm-wiki-query"
description: "LLM-Wiki 查询工作流：查索引，读相关页面，合成带引用的回答，有价值结果存档"
---

# llm-wiki-query

LLM-Wiki 的查询工作流。当用户询问关于 wiki、知识库、笔记、记录中的内容时触发。

## 触发条件

用户说出以下任一内容时触发：

- 「我的笔记里关于 xxx 是怎么说的」
- 「过去我对 xxx 的决策是什么」
- 「查询 xxx 相关的知识」
- 提及「wiki」「知识库」「笔记」「记录」等关键词

未触发时，按日常对话处理，不走此工作流。

## 核心规则

**禁止凭模型记忆回答。** 必须先检索 wiki 知识库，基于已有内容合成答案。

**通用知识兜底。** 如果 wiki 中无相关内容，先声明「wiki 里没找到相关记录」，如需再补充通用知识。

## 步骤

### 1. 读索引
读取 `wiki/index.md`，了解 wiki 全貌。

### 2. 搜索定位
使用 qmd 做语义搜索，覆盖整个 `wiki/`（含动态子目录）：

```
qmd query "用户的问题"
```

如 qmd 不可用，逐层检索：`wiki/index.md` → 各分类目录。

读取前 N 个最相关的页面（1-5 个）。

### 3. 合成答案
基于 wiki 内容回答：

- **结论先行** — 先说 wiki 里有什么，再展开
- **双链引用** — `[[页面路径]]` 标注信息来源
- **引用原文** — 使用 Markdown 块引用 `> 原文`
- **找不到走兜底** — 声明后再决定是否给通用知识回答

### 4. 询问是否存档
如果回答具有分析对比性或总结性（超过 2 个段落），主动问用户：

> 这个总结有价值，要存到 wiki/syntheses/ 吗？

用户同意后，创建 synthesis 页面：

```markdown
---
title: "synthesis-标题"
type: synthesis
tags: [标签]
sources: [引用的wiki页面路径]
last_updated: YYYY-MM-DD
---

# 标题

## 结论

## 分析

## 引用

## 关联连接
```

并在 `wiki/index.md` 的 Syntheses 分类下注册。

### 5. 记录日志
在 `wiki/log.md` 追加：

```
## [YYYY-MM-DD] query | 问题摘要

- 触发：用户要求查笔记/wiki
- 搜索：qmd 命中 N 个结果 / 目录检索
- 引用：psychology/flow, people/AuthorA
- 存档：syntheses/synthesis-标题 / 否
- 兜底：否 / 是
```

## 约束
- 仅触发条件满足时执行
- 必须先检索 wiki，禁止凭模型记忆回答
- wiki 无相关内容时必须声明
- 存档前必须问用户
