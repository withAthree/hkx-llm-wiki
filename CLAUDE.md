# CLAUDE.md — LLM-Wiki 行为契约

> 我是这个 wiki 的维护者 (LLM)。以下是必须遵守的结构约定和工作流程。
> 这个文件会随着我们摸索更好的方式而共同演化。

---

## 1. 目录结构

```
assets/                    ← 统一媒体资源（图片/PDF/附件，Obsidian 附件路径）
│
raw/                       ← 原始资料收件箱（我放入，LLM 只读不改）
├── 01-articles/           ← 网页剪藏、技术文章 (.md)
├── 02-papers/             ← 论文、深度研报、PDF
├── 03-transcripts/        ← 视频/播客转录、会议记录
├── 04-meeting_notes/      ← 头脑风暴、会议纪要
└── 09-archive/            ← 已归档（ingest 成功后自动移入）
│
wiki/                      ← 知识编译输出层（LLM 完全写权限）
├── index.md               ← 全局内容字典
├── log.md                 ← 行为流水线（grep-friendly）
├── concepts/              ← 抽象层：方法论、架构模式、第一性原理
├── entities/              ← 实体层：人名、公司、软件、项目
├── sources/               ← 摘要层：raw 文件的一对一观点提炼
└── syntheses/             ← 综合层：深度研究报告
│
CLAUDE.md                  ← 本文件
├── .claude/skills/        ← Agent Skill 中心
    ├── ingest/            ← /ingest 工作流
    ├── query/             ← /query 工作流
    └── lint/              ← /lint 健康检查
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

## 3. Ingest 工作流

**触发：** 我放入素材 + 说 `/ingest`

1. **扫描 raw/** — 找非归档文件（不在 `09-archive/` 的）
2. **读素材** — 理解内容
3. **讨论** — 主动问我核心要点和疑问
4. **写摘要** → `wiki/sources/`
5. **更新/创建概念页** → `wiki/concepts/`
6. **更新/创建实体页** → `wiki/entities/`
7. **更新 `wiki/index.md`**
8. **更新 `wiki/log.md`** — 格式：`## [YYYY-MM-DD] ingest | 标题`
9. **归档** — 源文件移入 `raw/09-archive/`

**一条素材可能触及 5-15 个页面。全部更新完才算完成。**

## 4. Query 工作流

**触发：** `我提问`

1. **查 `wiki/index.md`** → 定位相关页面
2. **读取 1-5 个相关页面**
3. **合成带引用的回答**
4. **有价值的分析** → 写入 `wiki/syntheses/`（不吃灰）
5. **更新 `wiki/log.md`**

## 5. Lint 工作流

**触发：** `我要求 /lint`

1. 🔗 死链 — `[[wikilink]]` 指向不存在的页面
2. 🕳️ 孤页 — 无入链的页面
3. ⚔️ 矛盾 — `contradicts` 关系对
4. 🗑️ 陈旧 — 被 `supersedes` 替代但未更新的内容
5. 📋 索引 — `wiki/index.md` 是否完整
6. 💡 空白 — 高频引用但无独立页面的概念

**修复前必须确认。**

## 6. 硬规则

- **绝不编造信息。** 来源中没有的不能写。有疑问 → `TODO` 标记。
- **数据引用留痕。** 注明具体源文件和位置。
- **矛盾不掩盖。** 有冲突 → `rel: contradicts` + 各自引文，不强行调和。
- **好答案要存档。** 有价值的分析 → `wiki/syntheses/`，不留在聊天历史里。
- **只能改 `wiki/` 下文件。** 不碰 `raw/` 和 `CLAUDE.md`（除非我要求）。
- **修改/删除前问我。** 添加内容自由做。
