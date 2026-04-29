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

### 运行时效率优化（v1.3）
- [x] 🆕 Skill 注册表自动同步（skill-evolution v1.3）：审计时自动对比注册表与文件系统，修复不一致
- [x] 🆕 上下文压力感知（strategic-compact v1.2）：代理指标推断上下文压力，4 级响应策略
- [x] 🆕 Skill 精简加载（workflow-loop v1.3）：L0-L3 分级加载，按上下文压力选择级别
- [x] 🆕 质量反馈回路（self-improvement v2.3）：5 维评分体系 + 历史趋势追踪
- [x] 🔧 版本号同步修正：所有 Skill 版本号与实际内容对齐

### 数据治理与实战准备（v1.4）
- [x] 🆕 注册表版本首同步（v1.4 实战）：skill-registry.json 4 个版本偏差修正 + capabilities 同步 + registry_sync_log
- [x] 🆕 跨会话模式增强（v1.4）：key_lessons 字段补充 + 新增"自进化系统迭代"模式 + 频率更新
- [x] 🆕 自动触发决策树（self-improvement v2.4）：6 级优先级触发流程 + 工具调用计数规则
- [x] 🆕 依赖健康检查（skill-evolution v1.4）：审计时自动验证 depends_on 环境变量/依赖可用性
- [x] 🆕 索引自动重建（memory-consolidation v2.3）：整理完成后全量重建 memory-index.json 行号

### 从规则走向状态（v1.5）
- [x] 🆕 注册表即时同步（workflow-loop v1.4）：Skill 变更后立即同步注册表，不等审计
- [x] 🆕 联动验证清单（workflow-loop v1.4）：任务开始预判联动需求，完成后验证执行
- [x] 🆕 审计轨迹记录（memory-consolidation v2.4）：audit_history 追踪每次自进化操作
- [x] 🆕 Skill 健康度评分（skill-evolution v1.5）：5 维加权评分 + 健康等级 + 趋势追踪
- [x] 🆕 依赖检查实战验证（v1.5）：tushare-data ✅ / stock-analyst ✅ / 地图 Skill ⚠️ Key 未配置

### Token 瘦身（v1.6）
- [x] 🆕 memory-consolidation v3.0：430→69 行（-84%），详细协议移入 references/detailed-protocols.md
- [x] 🆕 workflow-loop v2.1：175→74 行（-58%），详细流程图移入 references/closed-loop-details.md
- [x] 🆕 skill-evolution v2.1：187→106 行（-43%），审计细节已在 references/detailed-audit-flows.md
- [x] 🆕 self-improvement v2.6：230→118 行（-49%），评分/模板已在 references/detailed-flows-and-templates.md
- [x] 🆕 strategic-compact v2.1：152→90 行（-41%），微调精简

### 借鉴 Hermes Agent 双文（v1.7）
- [x] 🆕 memory-consolidation v4.0：MEMORY.md 容量硬限制 ≤4000 chars（借鉴 Hermes 2200 chars 限制设计）
- [x] 🆕 memory-consolidation v4.0：声明式事实 vs 操作步骤分离（Memory 存事实，Skill 存步骤）
- [x] 🆕 self-improvement v3.0：D5 序列模式维度（跨 turn 模式检测，借鉴 Hermes 双重视角审计）
- [x] 🆕 self-improvement v3.0：Skill 候选扩展触发（跨会话重复 + D5 模式识别均可触发）
- [x] 🆕 self-improvement v3.0：效率审计 addendum（借鉴 Hermes SECURITY_ADDENDUM 设计）
- [x] 🆕 workflow-loop v3.0：Skill 即时 Patch 规则（发现不符立即修正，patch 优先于重写）
- [x] 🆕 skill-evolution v2.2：Skill 生命周期管理（last_used / use_count / status 优胜劣汰）

---

## 📋 下一步计划

### 高优先级

#### 1. 实战验证
**目标**：在真实任务中验证 v1.2-v1.5 所有新能力的端到端效果

**验收标准**：
- [ ] Skill 自动创建流程端到端跑通
- [ ] 记忆索引按需加载确实减少重复读取
- [ ] 质量评分产出有意义的数据点
- [ ] 上下文压力感知触发正确的响应策略
- [x] 依赖健康检查能发现真实配置问题（v1.5 已验证：地图 Key ⚠️）
- [ ] 索引重建后行号准确
- [x] 联动验证清单能在任务中实际执行
- [ ] 注册表即时同步在每次 Skill 变更后立即执行

### 中优先级

#### 2. 数据驱动改进
**目标**：积累 ≥ 10 次质量评分后，分析系统性短板

**实现路径**：
```
积累质量评分 → 分析各维度平均分 → 识别持续低分维度
  → 针对性创建改进 Skill 或更新规则
  → 追踪改进效果
```

#### 3. 健康度趋势分析
**目标**：积累 ≥ 3 次健康度评分后，追踪 Skill 健康度变化趋势

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
| v1.3 运行时效率优化 | 2026-04-25 ✅ | 注册表同步 + 压力感知 + 精简加载 + 质量反馈 + 版本修正 |
| v1.4 数据治理与实战准备 | 2026-04-25 ✅ | 注册表首同步 + 模式增强 + 触发决策树 + 依赖检查 + 索引重建 |
| v1.5 从规则走向状态 | 2026-04-25 ✅ | 即时同步 + 联动验证 + 审计轨迹 + 健康度评分 + 依赖验证 |
| v1.6 Token 瘦身 | 2026-04-27 ✅ | 5 个核心 SKILL.md 总计 1113→457 行（-59%），详细内容移入 references/ |
| v1.7 借鉴 Hermes 双文 | 2026-04-29 ✅ | 容量硬限制 + 声明式分离 + D5序列模式 + 即时Patch + 生命周期管理 + 效率审计 |
