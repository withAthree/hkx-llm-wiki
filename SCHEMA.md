# SCHEMA.md — LLM-Wiki 行为契约

> 我是这个 wiki 的维护者。以下是必须遵守的结构约定和工作流程。
> 这个文件会随着我们摸索更好的方式而共同演化。

---

## 1. 目录结构

```
assets/                    ← 统一媒体资源（图片/PDF/附件，Obsidian 附件路径）
│
raw/                       ← 原始资料收件箱（我放入，我/LLM 只读不改）
├── 01-articles/           ← 网页剪藏、技术文章 (.md)
├── 02-papers/             ← 论文、深度研报、PDF
├── 03-transcripts/        ← 视频/播客转录、会议记录
├── 04-meeting_notes/      ← 头脑风暴、会议纪要
└── 09-archive/            ← 已归档（ingest 成功后自动移入）
│
wiki/                      ← 知识编译输出层（LLM 完全写权限）
├── index.md               ← 全局内容字典
├── log.md                 ← 行为流水线（grep-friendly）
├── concepts/              ← 抽象层（按领域分子目录）
│   ├── psychology/        ← 心理、认知、焦虑管理
│   ├── thinking-models/   ← 思维模型、认知升级
│   ├── productivity/      ← 效率、方法论
│   ├── finance/           ← 理财、金钱观
│   └── technology/        ← 技术、编程、AI
├── entities/              ← 实体层（按类型分子目录）
│   ├── people/            ← 人物
│   ├── books/             ← 书籍、文章
│   ├── companies/         ← 公司、组织
│   └── tools/             ← 工具、软件
├── sources/               ← 摘要层（薄层索引）
└── syntheses/             ← 综合层（深度研究报告）
│
SCHEMA.md                  ← 本文件（行为契约，我和 LLM 共同演化）
```

## 2. 页面约定

### 命名规则
- 英文小写 + 连字符：`attention-is-all-you-need.md`
- 中文概念：英译或拼音：`growth-mindset.md`
- 避免空格、中文路径、特殊字符

### 页面格式

```markdown
---
tags: [tag1, tag2]
related:
  - page: "another-page"
    rel: supports | contradicts | extends | supersedes | references
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources: ["source-file.md"]
---

# 标题

## 核心要点

## 详情

## 来源
```

### 关系类型 (rel)
| 类型 | 含义 |
|---|---|
| `supports` | 支持 |
| `contradicts` | 矛盾 |
| `extends` | 扩展/深化 |
| `supersedes` | 替代/推翻 |
| `references` | 提及/引用 |

### WikiLinks
- 用 `[[页面名]]` 跨页面引用
- 已有概念 → 链接，不重复写
- 新概念 → 先判断是否值得建独立页，再做决定

## 3. 工作流

三种操作用 OpenClaw skill 实现，对话触发：

| 操作 | 技能 | 触发方式 |
|---|---|---|
| 📥 摄入 | `llm-wiki-ingest` | 我丢素材到 `raw/` → 你告诉我 |
| 🔎 查询 | `llm-wiki-query` | 你直接提问 |
| 🩺 健康检查 | `llm-wiki-lint` | 你说「跑个 lint」 |

技能文件定义具体步骤，这里只写 wiki 层面的规矩。

## 4. 硬规则

- **绝不编造信息。** 来源中没有的不能写。有疑问 → `TODO` 标记。
- **数据引用留痕。** 注明具体源文件和位置。
- **矛盾不掩盖。** 有冲突 → `rel: contradicts` + 各自引文，不强行调和。
- **好答案要存档。** 有价值的分析 → `wiki/syntheses/`，不留在聊天历史里。
- **只能改 `wiki/` 下文件。** 不碰 `raw/`（除非我要求归档）。
- **修改/删除前问我。** 添加内容自由做。
