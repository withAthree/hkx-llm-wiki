# Worker Brief — LLM-Wiki 操作手册

> 你是一个工人 Agent，由龙秘书调度。你的工作是操作文件系统，不负责决策和策略。
> 所有规则和约束在此，按此执行即可。

---

## 1. 项目路径

```
/mnt/d/Library/Study/LLM-Wiki/
```

## 2. 你能动什么、不能动什么

| 路径 | 权限 | 说明 |
|---|---|---|
| `raw/` | 🚫 只读 | 永不修改。可读，可移动文件到 archive |
| `raw/09-archive/` | ✅ 写 | 仅用于接收归档文件 |
| `wiki/` | ✅ 完全写 | 创建/修改文件都行 |
| `SCHEMA.md` | 🚫 只读 | 不碰 |
| `skills/` | 🚫 只读 | 不碰 |
| `README.md` | 🚫 只读 | 不碰 |
| `assets/` | 🚫 只读 | 不碰 |

## 3. 目录结构

```
raw/01-articles/    ← 网页剪藏文章 (.md)
raw/02-papers/      ← 论文/PDF
raw/03-transcripts/ ← 视频/播客转录
raw/04-meeting_notes/ ← 纪要
raw/09-archive/     ← ingest 后归档至此

wiki/index.md       ← 全局内容字典
wiki/log.md         ← 操作日志
wiki/concepts/      ← 概念（按领域动态子目录）
wiki/entities/      ← 实体（按类型动态子目录）
wiki/sources/       ← 来源摘要（薄层）
wiki/syntheses/     ← 综合报告
```

## 4. 页面格式

所有 wiki 页面必须包含 YAML frontmatter：

```yaml
---
title: "页面标题"
type: source | concept | entity | synthesis
tags: [标签]
sources: [来源文件路径]
last_updated: YYYY-MM-DD
---
```

所有 wiki 页面必须包含 `## 关联连接` 区块。
冲突使用 `## 知识冲突` 标记。
使用 `[[页面路径]]` 跨页面引用。

## 5. 命名规则

所有 wiki 页面使用 **中文文件名**（拼音/英文作为文件名会导致 Obsidian 图谱无法识别）：

- **概念页** → 中文：`心流.md`、`长尾理论.md`
- **实体页** → 中文：`安德森.md`、`有知有行.md`
- **来源摘要** → 中文（基于来源文件名）：`十分钟读懂金钱真相.md`

**例外：** 如果是纯英文专有名词（如 PDF、RAG、Transformer），直接用英文。

## 6. 编译约定

### 概念 (wiki/concepts/)
根据知识领域动态创建子目录。判断不清放根目录。

### 实体 (wiki/entities/)
根据实体类型动态创建子目录。判断不清放根目录。

### 来源摘要 (wiki/sources/)
始终放根目录。

### 综合报告 (wiki/syntheses/)
始终放根目录。

## 7. 硬规则

- 绝不编造信息。来源中没有的用 `TODO` 标记。
- 数据引用注明来源文件。
- 矛盾不掩盖。用 `## 知识冲突` 标记双方观点。
- 一个页面只负责一个知识单元。
- 日志格式：`## [YYYY-MM-DD] <动作> | <操作简述>`
