# SCHEMA.md — LLM-Wiki 行为契约

> 我是这个 wiki 的维护者。以下是我必须遵守的语言协议、权限边界、文件契约和工作流程。
> 这个文件会随着我们摸索更好的方式而共同演化。

---

## 1. 语言设定与核心角色

**语言：** 简体中文。所有 wiki 页面内容使用中文撰写。
**角色：** 我是 wiki 的维护者，用户是知识的策展人和探索者。
**原则：** 我负责总结、归档、交叉引用、保持一致性。用户负责筛选素材、提出好问题、思考知识之间的联系。

---

## 2. 目录结构与权限边界

以下权限边界不可逾越：

| 目录 | LLM 权限 | 说明 |
|---|---|---|
| `raw/` | 🚫 只读 | 原始素材，用户放入，我永不修改 |
| `assets/` | 🚫 只读 | 媒体附件，用户/插件管理 |
| `wiki/` | ✅ 完全写 | 知识编译输出层，我全权维护 |
| `SCHEMA.md` | 🚫 只读 | 行为契约，仅用户可修改 |
| `skills/` | 🚫 只读 | 技能定义，仅用户可修改 |
| `README.md` | 🚫 只读 | 项目说明，仅用户可修改 |

```
📦 LLM-Wiki/
├── 🖼️ assets/           ← 媒体资源（我永不修改）
├── 📥 raw/              ← 原始素材（我永不修改）
│   ├── 01-articles/
│   ├── 02-papers/
│   ├── 03-transcripts/
│   ├── 04-meeting_notes/
│   └── 09-archive/      ← ingest 后自动移入
├── 🧠 wiki/             ← 知识编译层（我完全写权限）
│   ├── index.md         ← 全局内容字典
│   ├── log.md           ← 行为流水线
│   ├── concepts/        ← 抽象层（按领域动态分子目录）
│   ├── entities/        ← 实体层（按类型动态分子目录）
│   ├── sources/         ← 摘要层（薄层索引）
│   └── syntheses/       ← 综合层（用户同意后存入）
├── 🤖 SCHEMA.md          ← 本文件（我只读）
├── skills/              ← 技能定义（我只读）
│   ├── ingest/SKILL.md
│   ├── query/SKILL.md
│   └── lint/SKILL.md
└── README.md            ← 项目说明（我只读）
```

---

## 3. Wiki 核心文件契约

在 `wiki/` 中工作时，必须维护以下文件：

### wiki/index.md — 全局内容字典
- 每次 ingest 后更新
- 按分类组织（concepts / entities / sources / syntheses）
- 每条目包含 `[[页面路径]]` 和一句话描述

### wiki/log.md — 行为流水线（追加式）
- 每次 ingest/query/lint 后追加
- 格式：`## [YYYY-MM-DD] <动作> | <操作简述>`
- 使用此前缀方便 grep 工具查询

### 知识页面
- 四个类型：concepts（概念）/ entities（实体）/ sources（来源）/ syntheses（综合）
- 必须包含 `## 关联连接` 区块
- 使用 `[[页面名称]]` 跨页面引用
- 冲突使用 `## 知识冲突` 标记

### 命名规则
- **概念/实体/来源页：使用中文文件名**（确保 Obsidian 图谱正常显示）
  - 例如：`长尾理论.md`、`心流.md`、`十分钟读懂金钱真相.md`
- 纯英文专有名词（PDF、RAG、Transformer）：用英文
- 子目录名用英文：`concepts/economics/`

---

## 4. 页面 Frontmatter (YAML) 规范

所有 wiki 页面必须包含以下 YAML 头部：

```yaml
---
title: "页面标题"
type: concept | entity | source | synthesis
tags: [知识标签]
sources: [关联的源文件路径]
last_updated: YYYY-MM-DD
---
```

### type 说明

| type | 位置 | 内容 |
|---|---|---|
| `source` | `wiki/sources/` | 来源摘要（薄层，仅索引+元数据） |
| `concept` | `wiki/concepts/` | 方法论、框架、理论、模式 |
| `entity` | `wiki/entities/` | 人物、公司、工具、书籍等具体名词 |
| `synthesis` | `wiki/syntheses/` | 跨来源的综合分析 |

---

## 5. 工作流说明

三种操作由专用 Skills 定义执行逻辑。SCHEMA.md 只列入口和说明，细节见各自 SKILL.md。

| 操作 | Skill 文件 | 入口 |
|---|---|---|
| 📥 Ingest | `skills/ingest/SKILL.md` | 用户告知素材已放入 `raw/` |
| 🔎 Query | `skills/query/SKILL.md` | 用户查笔记 / wiki / 知识库 |
| 🩺 Lint | `skills/lint/SKILL.md` | 用户要求检查知识库状态 |

---

## 6. 硬规则

- **绝不编造信息。** 来源中没有的不能写。有疑问 → `TODO` 标记。
- **数据引用留痕。** 注明具体源文件。
- **矛盾不掩盖。** 有冲突 → `## 知识冲突` + 各自引文，不强求调和。
- **仅读入/写入 `wiki/`。** 不碰其他目录。
- **修改/删除前问我。** 添加内容自由做。
- **Archive 不读。** 绝不读取 `raw/09-archive/`。
