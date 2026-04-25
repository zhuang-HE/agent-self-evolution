# 🧬 Agent Self-Evolution Framework

> 借鉴 [Hermes Agent](https://github.com/nousresearch/hermes-agent) 自进化理念，为 WorkBuddy/OpenClaw 类 AI Agent 构建的自我优化闭环系统。

一个 AI Agent 的能力上限，不取决于模型本身，而取决于它**从自己的执行历史中学习**的能力。

---

## 🎯 这是什么

本项目的核心思路：**让 Agent 从"靠人工注入知识"进化到"从执行轨迹中自动学习"**。

我们为 AI Agent 设计了**六个核心 Skill**，形成完整的自进化闭环：

```
用户请求 → 🔮 意图预判（self-improvement v2.3）
              ↓ 扫描 skill-registry.json（~1KB）
         📋 工具自省（workflow-loop v1.3）→ 匹配最优 Skill
              ↓ 🪶 按压力分级加载（L0-L3）
         🔄 跨会话模式检测 → 识别重复模式 → 主动推荐
              ↓
任务执行 → ⚡ 并行调用优化（workflow-loop v1.3）→ 减少等待
              ↓ 📡 上下文压力感知（strategic-compact v1.2）
         完成 → 🔗 强制联动 → 自动触发审查/踩坑/候选
              ↓
         self-improvement（自我反思 + 质量评分 v2.3）
              ↓
         ├── 🆕 Skill 自动创建（v2.2）→ 提取模式→生成模板→用户审核
         ├── memory-consolidation（记忆整理）
         │     └── 📋 记忆索引（memory-index.json）→ 按需检索
         ├── skill-evolution（技能进化）
         │     └── 🆕 踩坑经验消化（v1.2）+ 注册表同步（v1.3）
         ├── 💰 Token 预算 + 上下文压力管理（strategic-compact v1.2）
         ├── 📊 质量评分 → memory-index.json quality_history
         └── 📋 Skill 注册表更新
              ↓
         下次执行时更聪明、更高效 ←←←←←←←←←
```

---

## 📂 项目结构

```
agent-self-evolution/
├── README.md                          # 你正在读的文件
├── LICENSE                            # MIT 协议
├── docs/
│   └── hermes-analysis.md             # Hermes Agent 借鉴分析报告
├── skills/
│   ├── self-improvement/
│   │   └── SKILL.md                   # 自我优化 Skill（v2.3）
│   ├── workflow-loop/
│   │   └── SKILL.md                   # 工作流闭环 Skill（v1.3）
│   ├── memory-consolidation/
│   │   └── SKILL.md                   # 记忆整理 Skill（v2.2）
│   ├── skill-evolution/
│   │   └── SKILL.md                   # 技能进化 Skill（v1.3）
│   └── strategic-compact/
│       └── SKILL.md                   # 上下文压缩 Skill（v1.2）
├── memory/
│   ├── MEMORY.md                      # 长期记忆模板（含示例数据）
│   ├── memory-index.json              # 记忆索引 + 跨会话模式 + 质量历史
│   └── skill-registry.json            # Skill 元数据注册表
└── ROADMAP.md                         # 进化路线图
```

---

## 🧠 三大核心 Skill

### 1. self-improvement — 自我优化

**定位**：任务完成后的强制反思，是整个闭环的入口。

**核心能力**：
- ✅ 四维度反思（执行质量 / 用户理解 / 知识漏洞 / 记忆系统）
- ✅ **自动触发**（≥ 5 个工具调用的任务必须触发，不再仅"建议"）
- ✅ **Skill 候选识别**（借鉴 Hermes：完成后自动判断是否应创建 Skill）
- ✅ **Skill 自动创建 MVP**（v2.2：提取模式→生成模板→用户审核→注册）
- ✅ **跨会话模式检测**（与 memory-index.json 对比，发现重复任务模式）
- ✅ **质量反馈回路**（v2.3：5 维评分 + 历史趋势追踪）
- ✅ 置信度评估 + 项目隔离写入

### 2. memory-consolidation — 记忆整理

**定位**：模拟人类睡眠巩固记忆，将短期日志提炼为长期记忆。

**核心能力**：
- ✅ 四阶段工作流（Orient → Gather → Consolidate → Prune）
- ✅ 项目隔离分区（防止跨项目知识污染）
- ✅ **关键词预检机制**（避免全量读取 MEMORY.md 浪费 token）
- ✅ **记忆摘要压缩**（> 50KB 时自动触发，借鉴 Hermes FTS5 + LLM 摘要）
- ✅ 置信度驱动的记忆晋升与降级

### 3. skill-evolution — 技能进化

**定位**：定期审计所有 Skill 的健康度，自动修复和优化。

**核心能力**：
- ✅ 四阶段审计（Scan → Analyze → Evolve → Report）
- ✅ 触发词覆盖率自动补充
- ✅ **踩坑经验即时追加**（会话中 ≥ 2 次重试后成功，不等审计）
- ✅ **经验自动消化**（同一 Skill 踩坑 ≥ 3 条时，提炼为显式工作流步骤）
- ✅ 健康评分体系（⭐⭐⭐⭐⭐ 评级）

---

## 📊 与 Hermes Agent 的对比

| 维度 | Hermes Agent | 本项目 |
|------|-------------|--------|
| 记忆检索 | FTS5 全文索引 + LLM 摘要 | Markdown 文件 + 关键词预检 |
| Skill 创建 | 完全自动 | 候选识别 → 用户审核（更可控） |
| 经验积累 | 自动提取 + 自动优化 | 即时追加 + 定期消化 |
| 自进化 | DSPy + GEPA 提示词优化 | Skill 触发词 + 踩坑经验迭代 |
| 数据飞轮 | RL 训练轨迹 → 微调模型 | ❌ 不适用（云端产品限制） |
| 部署要求 | 自托管（你的服务器） | 纯文件，任何 Agent 平台可用 |

**核心差异**：Hermes 追求全自动，我们追求**可控的自进化**——每一步都保留人工审核节点。

---

## 🚀 快速开始

### 作为 WorkBuddy Skill 使用

1. 将 `skills/` 目录下的三个 Skill 复制到 `~/.workbuddy/skills/`
2. 将 `memory/MEMORY.md` 复制到 `{workspace}/.workbuddy/memory/MEMORY.md`
3. 在下次会话中，Agent 会自动读取记忆并按 Skill 规则运行

### 作为独立参考使用

三个 `SKILL.md` 是纯 Markdown 格式，任何 AI Agent 框架都可以参考其设计理念来实现自进化能力。

---

## 🔮 进化路线图

### 已完成 ✅
1. 记忆系统点火：从空壳 → 结构化长期记忆
2. 自动触发强化：从"建议" → "必须触发"
3. Skill 候选识别：任务完成后自动判断
4. 踩坑经验即时化：不再等审计
5. 记忆按需检索：关键词预检机制

### 下一步 📋
1. **记忆索引结构化**（中优先级）：JSON 索引文件替代全量 Markdown 读取
2. **Skill 自动创建 MVP**（高优先级）：完成复杂任务 → 提取模式 → 生成模板 → 用户审核
3. **踩坑经验自动消化**（中优先级）：≥ 3 条经验自动提炼为工作流步骤
4. **跨会话模式检测**（低优先级）：会话开始时检测重复模式

---

## 📄 License

MIT License — 自由使用、修改、分发。

---

## 🙏 致谢

- [Hermes Agent](https://github.com/nousresearch/hermes-agent) by Nous Research — 自进化理念的核心灵感来源
- [WorkBuddy](https://workbuddy.ai) — Agent 运行平台
