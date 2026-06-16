# Query Skill

**触发词：** `/query <问题>`

**流程：**
1. 读 `wiki/index.md` 定位相关页面
2. 读取 1-5 个最相关的页面
3. 合成带双链引用的回答
4. 有价值的分析结果 → 写入 `wiki/syntheses/`
5. 更新 `wiki/log.md`
