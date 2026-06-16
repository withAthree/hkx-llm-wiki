---
name: "llm-wiki-ingest"
description: "LLM-Wiki 摄入工作流：扫描 raw/，读素材，写摘要+概念+实体页，更新索引，归档"
---

# llm-wiki-ingest

LLM-Wiki 的摄入工作流。用户丢素材到 `raw/` 后，我自主扫描、拆解为原子知识单元、按领域分类入库、归档。

## 核心原则

**原子性：** 一个页面 = 一个不可分割的知识单元。一篇素材拆成多个原子页，各归各位。

**动态分类：** 编译时根据知识内容判断领域/类型，实时创建对应子目录。不预设目录结构。

## 编译约定

### 概念 (wiki/concepts/)
判断知识的抽象领域，动态创建子目录。例如：
- 心理学知识 → `wiki/concepts/psychology/`
- 思维模型 → `wiki/concepts/thinking-models/`
- 效率方法论 → `wiki/concepts/productivity/`

**判断不出或跨领域 → 直接放 `wiki/concepts/` 根目录。**

### 实体 (wiki/entities/)
判断实体的类型，动态创建子目录。例如：
- 人物 → `wiki/entities/people/`
- 书籍/文章 → `wiki/entities/books/`
- 公司/组织 → `wiki/entities/companies/`
- 工具/软件 → `wiki/entities/tools/`

**判断不出或新类型 → 直接放 `wiki/entities/` 根目录。**

### 来源摘要 (wiki/sources/)
始终放根目录，不再分子目录。

### 综合报告 (wiki/syntheses/)
始终放根目录，不再分子目录。

## 步骤

### 1. 自动扫描
扫描 `raw/` 下所有不在 `09-archive/` 的文件。排除 `.gitkeep`。
超过 10 个先问优先级。

### 2. 读取素材
- `.md` → 直接读取
- `.pdf` → 尝试提取文本，失败则记录元信息
- 非中文 → 自动翻译，原文备查
- 其他格式 → 跳过并报告

### 3. 拆解为原子知识单元
读完后拆解为：

**实体** — 人物、公司、工具、书籍等具体名词
**概念** — 方法论、框架、理论、模式等抽象知识
**观点** — 独特主张、论证、结论（仅当足够独立且有归档价值时）

### 4. 创建来源摘要页（薄层）
写入 `wiki/sources/摘要-{文件slug}.md`：

```markdown
---
title: "摘要-文件slug"
type: source
tags: [标签]
sources: [raw/xx/xxx.md]
last_updated: YYYY-MM-DD
---

## 核心主旨
[1-2句话]

## 本素材产出的知识单元
- [[EntityName]] — 实体
- [[ConceptName]] — 概念

## 关联连接
```

### 5. 创建/更新原子知识页
对每个原子单元：

**① 判断领域/类型 → 确定目标子目录**
**② 子目录不存在 → 实时创建**
**③ 页面不存在 → 新建：**

```markdown
---
title: "知识单元名称"
type: entity | concept
domain: 领域名
tags: [标签]
sources: [摘要-slug]
last_updated: YYYY-MM-DD
---

## 定义
[一句话定义]

## 详情
[一个主题，保持聚焦]

## 关联连接
- [[摘要-slug]] — 来源
- [[RelatedPage]] — 相关内容
```

**页面已存在 → 增量合并。** 只加新信息，不重复。

### 6. 冲突处理
当新内容与已有页面冲突时：

1. **暂停** — 停止当前 ingest
2. **报告** — 冲突页面、冲突内容
3. **用户选 A/B/C：**
   - A) 保留两者 → 在页面中标记 `## 知识冲突`
   - B) 新知识覆盖旧知识
   - C) 放弃本次 ingest
4. **继续**

### 7. 更新索引
在 `wiki/index.md` 对应分类下新增条目，标注子目录：

```
### wiki/sources/
- [[摘要-xxx]] — 核心主旨

### wiki/concepts/
- [[目录名/concept-name]] — 定义

### wiki/entities/
- [[目录名/EntityName]] — 定义
```

### 8. 记录日志
```
## [YYYY-MM-DD] ingest | 标题

- 来源：raw/路径
- 新原子页：people/AuthorA, psychology/flow
- 更新页：page3
- 冲突：无
```

### 9. 归档
源文件移入 `raw/09-archive/`。

### 10. 完成后一次性报告
```
📥 Ingest 完成 — 素材名
├── 实体：N 个 (people/AuthorA, tools/Obsidian)
├── 概念：N 个 (psychology/flow, productivity/gtd)
├── 更新：N 个
└── 冲突：无
```

## 约束
- 绝不修改源文件
- 绝不读取 `raw/09-archive/`
- 绝不修改 `SCHEMA.md`
- 一个页面只负责一个知识单元
- 来源中没有的用 `TODO` 标记
- 领域/类型判断不清时，放父目录而非强行分类
