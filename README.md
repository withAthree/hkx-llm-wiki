# LLM-Wiki

基于 [Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 模式的个人知识库。

## 三层架构

| 层 | 目录 | 谁维护 | 说明 |
|---|---|---|---|
| 📥 Raw Sources | `raw/` | 我 | 原始素材，只读不改，LLM 只取不写 |
| 🧠 Wiki | `wiki/` | LLM | 结构化知识库，LLM 全权维护 |
| 📋 Schema | `CLAUDE.md` | 我和 LLM | 行为契约，共同演化 |

## 工作流

- **Ingest（摄入）** → 丢素材到 `raw/sources/` → 告诉我 → 我读、总结、更新 wiki
- **Query（查询）** → 直接问 → 我先查 wiki，合成答案 + 引用
- **Lint（健康检查）** → 定期运行 → 检查矛盾、陈旧声明、孤页、数据空白

## 工具

- **Obsidian** → wiki 的 IDE，实时浏览 graph view
- **qmd** → 本地语义搜索（BM25 + 向量 + 重排序）
- **Git** → 版本历史、分支、回滚
