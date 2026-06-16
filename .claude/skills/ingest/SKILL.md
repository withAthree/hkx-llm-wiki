# Ingest Skill

**触发词：** `/ingest`

**流程：**
1. 扫描 `raw/` 下所有未被处理过的文件（即未被归档到 `09-archive/`）
2. 逐一读取文件内容
3. 创建对应摘要页面到 `wiki/sources/`
4. 更新/创建相关概念页面到 `wiki/concepts/`
5. 更新/创建相关实体页面到 `wiki/entities/`
6. 更新 `wiki/index.md`
7. 更新 `wiki/log.md`
8. 将源文件移动到 `raw/09-archive/`
