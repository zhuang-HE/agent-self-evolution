# 🗺️ 进化路线图

> 最后更新：2026-04-24

---

## ✅ 已完成（v1.0）

### 核心框架搭建
- [x] 设计三层 Skill 闭环架构（self-improvement → memory-consolidation → skill-evolution）
- [x] self-improvement v2.1：自动触发强化 + Skill 候选识别 + 跨会话模式检测
- [x] memory-consolidation v2.1：关键词预检 + 记忆摘要压缩
- [x] skill-evolution v1.2：踩坑经验即时追加 + 经验自动消化

### 自身架构改进（v1.1）
- [x] 🔮 意图预判层（self-improvement v2.1）：从被动触发词匹配升级为主动预判用户意图
- [x] ⚡ 工具调用并行优化（workflow-loop v1.1）：批量并行发出无依赖调用，减少串行等待
- [x] 🔗 Skill 强制联动（workflow-loop v1.1）：关键任务路径自动触发下游 Skill
- [x] 💰 Token 预算管理（strategic-compact v1.1）：任务预算估算 + 效率监控 + 快速失败

### 记忆系统点火
- [x] MEMORY.md 结构化模板（项目分区 + 置信度标记）
- [x] 日期日志模板
- [x] 从 SOUL.md / USER.md / 系统记忆中提取初始数据

### Hermes 借鉴
- [x] Hermes Agent 记忆系统分析
- [x] Hermes Agent Skill 自动创建机制分析
- [x] Hermes Agent 自进化闭环分析
- [x] 借鉴分析报告输出

### Hermes 深度迁移（v1.2）
- [x] 🆕 Skill 自动创建 MVP（self-improvement v2.2）：从执行轨迹提取模式→生成 SKILL.md 模板→用户审核
- [x] 🆕 记忆索引结构化（memory-consolidation v2.2）：JSON 索引文件实现按需检索，取代全量读取
- [x] 🆕 踩坑经验自动消化（skill-evolution v1.2）：≥3 条经验→聚类分析→提炼为工作流显式步骤
- [x] 🆕 跨会话模式检测（self-improvement v2.2）：会话开始→匹配历史模式→主动推荐 Skill
- [x] 🆕 Skill 元数据注册表（workflow-loop v1.2）：skill-registry.json 实现轻量级工具自省
- [x] 🆕 记忆索引文件（memory-index.json）：MEMORY.md 分区索引 + 跨会话模式索引

---

## 📋 下一步计划

### 高优先级

#### 1. 实战验证
**目标**：在真实任务中验证 v1.2 所有新能力的端到端效果

---

## ⏸️ 暂不适用

| 功能 | 原因 | 前提条件 |
|------|------|---------|
| RL 数据飞轮 | WorkBuddy 是云端产品，无法微调底层模型 | 支持自定义模型 |
| 提示词自动优化（DSPy + GEPA） | 需要 API 成本预算 | 确定预算方案 |
| FTS5 全文索引 | 需要 SQLite 数据库层 | 记忆系统迁移到数据库 |

---

## 📅 里程碑

| 里程碑 | 预期时间 | 核心交付 |
|--------|---------|---------|
| v1.0 基础闭环 | 2026-04-24 ✅ | 三个核心 Skill + 记忆模板 |
| v1.1 架构改进 | 2026-04-24 ✅ | 意图预判 + 并行优化 + 强制联动 + 预算管理 |
| v1.2 Hermes 深度迁移 | 2026-04-24 ✅ | Skill自动创建 + 记忆索引 + 经验消化 + 模式检测 + 工具自省 |
| v1.3 实战验证 | TBD | 真实任务端到端验证 + 指标收集 |
