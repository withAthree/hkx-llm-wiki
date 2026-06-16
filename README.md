# LLM-Wiki

基于 [Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 模式的 Obsidian 知识库。

## 三层架构

| 层 | 目录 | 谁维护 | 说明 |
|---|---|---|---|
| 📥 Raw Sources | `raw/` | 你 | 原始素材，只读不改，我只看不写 |
| 🧠 Wiki | `wiki/` | 我 | 结构化知识库，我全权维护 |
| 📋 Schema | `SCHEMA.md` | 你和我 | 行为契约，共同演化 |

## 目录结构

```
📦 LLM-Wiki/
├── 🖼️ assets/                   ← 统一媒体资源（图片/PDF/附件，Obsidian 附件路径）
│
├── 📥 raw/                      ← 原始资料收件箱（你放入，我只看不改）
│   ├── 📄 01-articles/          ← 网页剪藏、技术文章 (.md)
│   ├── 🎓 02-papers/            ← 论文、深度研报、PDF
│   ├── 🎙️ 03-transcripts/       ← 视频/播客转录、会议记录
│   ├── 💡 04-meeting_notes/     ← 头脑风暴、会议纪要
│   └── 🗃️ 09-archive/           ← 已归档（ingest 后我自动移入）
│
├── 🧠 wiki/                     ← 知识编译输出层（我完全写权限）
│   ├── 📑 index.md              ← 全局内容字典（我维护）
│   ├── 📜 log.md                ← 行为流水线（我维护）
│   ├── 🏗️ concepts/             ← 概念：方法论、架构模式、第一性原理
│   ├── 👥 entities/             ← 实体：人名、公司、软件、项目
│   ├── 🔍 sources/              ← 摘要：针对 raw 文件的一对一观点提炼
│   └── 💎 syntheses/            ← 综合：跨来源的深度研究（你同意我才存）
│
├── 🤖 SCHEMA.md                 ← 行为契约（你和我共同演化）
│
└── 🔧 skills/
    ├── ⚙️ ingest/SKILL.md        ← 摄入工作流
    ├── 🔎 query/SKILL.md         ← 查询工作流
    └── 🩺 lint/SKILL.md          ← 健康检查
```

## 使用方式

在 Obsidian 中打开本知识库，直接对话触发操作。

| 你说 | 我做 |
|---|---|
| 丢素材到 `raw/` + 告诉我 | ingest → 读/拆原子/分类/入库/归档 |
| 「查查笔记里关于 xxx」 | query → 搜 wiki 合成答案 |
| 「检查知识库状态」 | lint → 出绿/黄/红灯报告 |

## 工具

- **Obsidian** → wiki 的 IDE，实时浏览 graph view
- **qmd** → 本地语义搜索（BM25 + 向量 + 重排序）
- **Git** → 版本历史、分支、回滚
