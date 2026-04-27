---
name: workflow-loop
version: 2.1.0
description: >
  标准工作流闭环模板库（STOM v2.0 瘦身版）。
  提供4种常用开发流程的标准化定义（Bug修复/功能开发/研究调研/代码审查）。
  详细流程图与模板移至 references/closed-loop-details.md。
references:
  - references/closed-loop-details.md
tags: [工作流, 工作流闭环, 流程模板, 标准流程, Bug修复流程, 开发流程, code-review流程, 复盘流程, 研究流程, workflow]
triggers:
  - 工作流
  - 工作流闭环
  - 流程模板
  - 标准流程
  - Bug修复流程
  - 开发流程
  - code-review 流程
  - 复盘流程
  - 研究流程
  - workflow
  - 工作流模板
  - 闭环
  - 标准开发流程
---

# Workflow Loop - 标准工作流闭环

## 🎯 定位

4 种标准闭环，每个内置 Search Before Building、Completion Protocol、Escalation、强制联动。

> **详细参考**（完整流程图、命令示例、输出模板、注册表同步、联动验证、加载成本分级、并行调用原则）→ `references/closed-loop-details.md`

---

## 📦 四个闭环

| 闭环 | 公式 | 复杂度 |
|------|------|--------|
| 🐛 Bug修复 | `investigate → root_cause → fix → verify → commit` | ⭐⭐ |
| 🚀 功能开发 | `plan → search → implement → review → test → ship` | ⭐⭐⭐ |
| 📖 研究调研 | `understand → search → synthesize → deliver` | ⭐⭐ |
| 🔍 代码审查 | `prepare → review → report → follow_up` | ⭐⭐ |

> 每个闭环的完整步骤表 + Escalation Rules → 见 references 对应章节

---

## 🔗 强制联动矩阵

| 前置任务 | 联动 Skill | 等级 |
|---------|-----------|------|
| 代码修改（≥3文件/核心逻辑）| code-review | L2 建议 |
| 同一 API 失败 ≥ 2 次 | self-improvement 踩坑 | L1 自动 |
| 复杂任务完成（≥5步）| self-improvement 候选 | L2 建议 |
| 功能/Bug 闭环完成 | code-review + self-improvement | L2 建议 |
| 触发词未命中 | skill-evolution | L1 自动 |
| 工作记忆 > 30 天 | memory-consolidation | L2 建议 |

**等级：** L1 自动执行（无需确认）/ L2 自动建议（需确认）/ L3 条件触发
**防重复：** 同一规则每会话最多 1 次；用户拒绝不再建议

---

## ⚡ 并行调用

- 批量读取：多文件一个 turn 并行
- 搜索合并：无关搜索并行
- 依赖串行：必须等前一步结果的才串行
- 批次控制：第 1 批 5-10 个（收集）→ 后续 3-5 个（实现）→ 最后 1-3 个（验证）

---

## 📊 Completion Protocol

| 状态 | 含义 |
|------|------|
| **DONE** | 全部完成，目标达成 |
| **DONE_WITH_CONCERNS** | 完成但有非阻塞遗留 |
| **BLOCKED** | 阻塞，需用户决策 |
| **NEEDS_CONTEXT** | 缺少信息 |

---

## 🚨 通用 Escalation

同一问题 3 次失败 → STOP | 安全敏感 → STOP | 需求模糊 → STOP

---

## 🔄 触发词自进化

用户新表述触发工作流时：分析 → 追加到 frontmatter triggers → 简洁通用 → 去重。

---

## 📚 踩坑经验

> AI 自动积累，请勿手动删除。格式：`- 场景：经验要点`

（暂无记录）
