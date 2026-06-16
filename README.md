# LLM-Wiki

基于 [Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 模式的 Obsidian 知识库。

## 三层架构

| 层 | 目录 | 谁维护 | 说明 |
|---|---|---|---|
| 📥 Raw Sources | `raw/` | 我 | 原始素材，只读不改，LLM 只取不写 |
| 🧠 Wiki | `wiki/` | LLM | 结构化知识库，LLM 全权维护 |
| 📋 Schema | `SCHEMA.md` | 我和 LLM | 行为契约，共同演化 |

## 目录结构

```
📦 LLM-Wiki/
├── 🖼️ assets/                   ← 统一媒体资源（图片/PDF/附件，Obsidian 附件路径）
│
├── 📥 raw/                      ← 原始资料收件箱（我放入，LLM 只读不改）
│   ├── 📄 01-articles/          ← 网页剪藏、技术文章 (.md)
│   ├── 🎓 02-papers/            ← 论文、深度研报、PDF
│   ├── 🎙️ 03-transcripts/       ← 视频/播客转录、会议记录
│   ├── 💡 04-meeting_notes/     ← 头脑风暴、会议纪要
│   └── 🗃️ 09-archive/           ← 已归档（ingest 后自动移入）
│
├── 🧠 wiki/                     ← 知识编译输出层（LLM 完全写权限）
│   ├── 📑 index.md              ← 全局内容字典
│   ├── 📜 log.md                ← 行为流水线（grep-friendly）
│   ├── 🏗️ concepts/             ← 抽象层：方法论、架构模式、第一性原理
│   ├── 👥 entities/             ← 实体层：人名、公司、软件、项目
│   ├── 🔍 sources/              ← 摘要层：raw 文件的一对一观点提炼
│   └── 💎 syntheses/            ← 综合层：深度研究报告
│
├── 🤖 SCHEMA.md                 ← 行为契约（我和 LLM 共同演化）
│
└── 🔧 OpenClaw Skills
    ├── ⚙️ llm-wiki-ingest        ← 摄入工作流
    ├── 🔎 llm-wiki-query         ← 查询工作流
    └── 🩺 llm-wiki-lint          ← 健康检查
```

## 使用方式

在 Obsidian 中打开本知识库，直接对话触发操作。

| 你说 | 我做 |
|---|---|
| 丢素材到 `raw/` + 告诉我 | ingest → 读/总结/索引/归档 |
| 问问题 | query → 查 wiki 合成答案，有价值则存档 |
| 「跑个 lint」 | lint → 检查矛盾/孤页/死链 |

## 工具

- **Obsidian** → wiki 的 IDE，实时浏览 graph view
- **qmd** → 本地语义搜索（BM25 + 向量 + 重排序）
- **Git** → 版本历史、分支、回滚
