---
name: skill-evolution
version: 1.1.0
description: >
  Skill 自动进化管理器，审计所有 Skill 的触发词覆盖率和踩坑经验，自动优化更新。
  触发词：skill 进化、skill 升级、优化 skill、审计 skill、skill 自进化、
  触发词优化、技能升级、技能进化、帮我优化技能、skill 管理、
  更新触发词、检查 skill、skill health、技能健康检查
complexity: ⭐⭐⭐
tools:
  - read_file
  - replace_in_file
  - list_dir
  - write_to_file
---

# Skill Evolution - 技能自动进化管理器

## 🎯 技能定位

对 WorkBuddy 中所有已安装的 Skill 进行周期性健康审计，自动识别和修复：
- 触发词覆盖不足（Skill 应该被激活但没激活）
- 踩坑经验积累（反复踩同一个坑说明 Skill 知识不足）
- Skill 描述过时或不准确
- 缺失自进化模块的老版本 Skill

---

## 📂 Skill 存储路径

```
用户级 Skill：C:\Users\{username}\.workbuddy\skills\{skill-name}\SKILL.md
项目级 Skill：{workspace}\.workbuddy\skills\{skill-name}\SKILL.md
```

---

## 📋 四阶段审计流程

### Phase 1 · Scan（扫描）
```
1. list_dir 扫描 ~/.workbuddy/skills/ 获取所有已安装 Skill
2. 逐一 read_file 读取每个 SKILL.md
3. 建立 Skill 清单：名称、版本、触发词数量、是否含进化模块
4. 标记需要关注的 Skill（触发词 < 5 个 / 无踩坑经验区域 / 无自进化规则）
```

### Phase 2 · Analyze（分析）
```
1. 读取工作记忆 .workbuddy/memory/ 目录中的日志
2. 提取用户手动调用 Skill 的场景（说明触发词未覆盖）
3. 提取多次尝试才成功的操作（说明缺乏踩坑经验）
4. 提取用户的新偏好和工作场景（用于丰富触发词）
5. 生成每个 Skill 的"进化建议清单"
```

### Phase 3 · Evolve（进化）

针对每个有改进空间的 Skill，执行：

① 补充触发词
   - 将分析出的新触发词追加到 SKILL.md 的 description 字段
   - 触发词要通用（"查股价"而非"查茅台股价"）
   - 每次补充不超过 5 个，避免触发词膨胀

② 补充踩坑经验（借鉴 Hermes 自动经验提取）
   - 将从日志中提取的经验教训追加到"踩坑经验"区域
   - 格式：`- 场景描述：一句话经验`
   - 去重：不添加语义相同的经验
   - **自动检测触发**：当一个 Skill 在会话中被调用且出现 ≥ 2 次重试/失败后成功，
     会话结束时应自动（而非等待审计）追加踩坑经验到对应 Skill

③ 为老版本 Skill 添加进化模块
   - 如果 SKILL.md 缺少"触发词自进化规则"区域，添加标准模板
   - 如果缺少"踩坑经验"区域，添加标准模板

④ **Skill 自优化闭环（借鉴 Hermes 自我优化）**
   - 当 Skill 的踩坑经验 ≥ 3 条时，自动分析经验是否有共同模式
   - 如有模式，将经验提炼为 Skill 工作流中的显式步骤（而非仅停留在踩坑区）
   - 标记被消化的经验为 `[已整合]`，避免重复触发

### Phase 4 · Report（报告）
```
生成进化报告，包含：
- 审计的 Skill 总数
- 每个 Skill 的变更摘要
- 新增触发词列表
- 新增踩坑经验列表
- 下次建议审计时间
```

---

## 📊 进化报告格式

```markdown
# Skill 进化报告

**审计时间**：YYYY-MM-DD HH:mm
**审计范围**：用户级 Skill（~/.workbuddy/skills/）
**Skill 总数**：N 个

---

## 变更摘要

| Skill | 新增触发词 | 新增踩坑经验 | 添加进化模块 |
|-------|-----------|-------------|-------------|
| code-review | 3 个 | 1 条 | - |
| web-research | 2 个 | 0 条 | - |

---

## 详细变更

### code-review
新增触发词：帮我review、看看这段代码、有没有bug
新增踩坑经验：
- Python 代码审查：f-string 中的复杂表达式不要超过 3 层嵌套，否则可读性极差

---

## 健康评分

| Skill | 触发词数 | 踩坑经验数 | 进化模块 | 健康度 |
|-------|---------|-----------|---------|--------|
| code-review | 15 | 3 | ✅ | ⭐⭐⭐⭐⭐ |
| web-research | 12 | 0 | ✅ | ⭐⭐⭐⭐ |

---

**下次建议审计**：{下周同一时间}
```

---

## 🏥 Skill 健康度标准

| 健康度 | 触发词数 | 踩坑经验 | 进化模块 |
|--------|---------|---------|---------|
| ⭐⭐⭐⭐⭐ 优秀 | ≥ 10 个 | ≥ 3 条 | ✅ |
| ⭐⭐⭐⭐ 良好 | 6-9 个 | 1-2 条 | ✅ |
| ⭐⭐⭐ 合格 | 4-5 个 | 0 条 | ✅ |
| ⭐⭐ 待改善 | 2-3 个 | 0 条 | ❌ |
| ⭐ 需重建 | < 2 个 | 0 条 | ❌ |

---

## ⚙️ 自动进化模块标准模板

当 Skill 缺少进化模块时，添加以下标准模板：

```markdown
---

## 🔄 触发词自进化规则

当用户输入某种表述但本 Skill 未被自动激活时，完成任务后**必须**执行：
1. 分析用户原始请求中的关键表述
2. 将其抽象为通用触发词（避免过于具体的个例）
3. 用 replace_in_file 工具将触发词追加到本文件 YAML frontmatter `description` 字段末尾
4. 不得重复添加已存在的触发词

---

## 📚 踩坑经验

> 由 AI 在实际调用中自动积累，**请勿手动删除**。
> 规则：凡经过 2 次及以上尝试才成功的情况必须追加。格式：`- 场景：经验要点`

（暂无记录）
```

---

## ⚙️ 核心规则

1. **只追加不覆盖**：所有修改只追加内容，不删除现有触发词或经验
2. **通用化表述**：新触发词要可复用，不能过于具体
3. **去重检查**：添加前检查是否已存在语义相同的内容
4. **透明变更**：每次进化后必须生成进化报告
5. **尊重原结构**：不修改 Skill 的核心工作流程，只丰富元数据

---

## 🔍 Search Before Building（强制步骤）

在开始审计前，执行三层搜索检查：

**Layer 1 - 项目内搜索**：
```bash
# 检查工作记忆是否有最近的进化记录
cat .workbuddy/memory/*.md | grep -i "skill\|进化"
# 检查是否有上次的进化报告
ls .workbuddy/skill-evolution/ 2>/dev/null
cat .workbuddy/memory/$(date +%Y-%m-%d).md 2>/dev/null | grep -A5 -B5 "skill"
```

**Layer 2 - 查找现有方案**：
- 检查 skill-evolution 是否已有进化报告模板
- 检查其他 Skill 是否有可复用的模式
- 查找 `~/.workbuddy/skills/` 下所有 Skill 列表

**Layer 3 - 如无现有方案**：
- 定义审计优先级：高使用频率 > 踩坑经验缺失 > 触发词不足
- 确定需要关注的 Skill 类别

---

## 🚨 Escalation Rules（升级机制）

**必须立即 STOP 并升级的情况**：

| 触发条件 | 处理方式 |
|---------|---------|
| Skill 文件损坏或无法读取 | STOP，报告问题，跳过该 Skill |
| 发现安全漏洞（如硬编码凭证） | STOP，报告漏洞位置 |
| Skill 冲突（同名 Skill 多个版本） | STOP，要求用户解决冲突 |
| 涉及 skill-creator 自身修改 | STOP，改用手动方式 |
| 3次进化尝试均失败 | STOP，报告失败原因 |

**Escalation 格式**：
```
STATUS: BLOCKED
REASON: [问题描述]
AFFECTED_SKILL: [受影响的 Skill]
ATTEMPTED: [尝试了什么]
RECOMMENDATION: [建议用户做什么]
```

---

## 📦 Workflow Loop（工作流闭环）

### 上游：定期触发 / 手动触发

| 上游场景 | 触发时机 |
|---------|---------|
| 周期性审计 | 建议每周一次 |
| 用户要求 | 用户说"审计 Skill"、"优化 Skill" |
| 发现问题 | 其他 Skill 执行中发现问题 |

### 下游：进化完成后

| 下游 skill | 联动方式 |
|-----------|---------|
| `self-improvement` | 记录 Skill 进化的踩坑经验 |
| `workflow-loop` | 如果需要创建新 Skill，触发 skill-creator |
| `MEMORY.md` | 持久化重要发现 |

### Skill 进化标准闭环
```
用户触发 → skill-evolution 审计 → 分析 → 进化 → 报告 → self-improvement 自检
                                                    ↓
                                              发现新需求 → skill-creator
```

### 审计优先级
```markdown
## Skill 审计优先级

P0 - 核心 Skill（每次审计必看）：
- skill-creator（创建新 Skill 的源头）
- workflow-loop（工作流定义）
- code-review（质量保障）

P1 - 高频 Skill（根据使用频率）：
- web-research
- documentation
- git-workflow

P2 - 低频 Skill（按需审计）：
- finance-data
- stock-analyst
- 其他专业 Skill
```

---

## ✅ Completion Protocol（完成状态协议）

| 状态 | 含义 |
|------|------|
| **DONE** | 审计完成，所有计划进化已执行 |
| **DONE_WITH_CONCERNS** | 审计完成，但部分 Skill 无法进化 |
| **BLOCKED** | 审计中断，报告阻塞原因 |
| **NEEDS_CONTEXT** | 需要用户提供更多信息 |

### 进化完成报告
```markdown
## Skill Evolution 完成报告

STATUS: DONE

**审计时间**：{datetime}
**审计范围**：{N} 个 Skill
**成功进化**：{N} 个
**跳过**：{N} 个

### 变更摘要

| Skill | 操作 | 详情 |
|-------|------|------|
| xxx | +触发词 | 3 个新触发词 |
| yyy | +踩坑 | 1 条新经验 |

### 健康评分变化

| Skill | 之前 | 之后 |
|-------|------|------|
| xxx | ⭐⭐⭐ | ⭐⭐⭐⭐ |

**下次建议审计**：{date}
```

---

## 🔄 触发词自进化规则

当用户输入某种表述但本 Skill 未被自动激活时，完成任务后**必须**执行：
1. 分析用户原始请求中的关键表述
2. 将其抽象为通用触发词（避免过于具体的个例）
3. 用 replace_in_file 工具将触发词追加到本文件 YAML frontmatter `description` 字段末尾
4. 不得重复添加已存在的触发词

---

## 📚 踩坑经验

> 由 AI 在实际调用中自动积累，**请勿手动删除**。
> 规则：凡经过 2 次及以上尝试才成功的情况必须追加。格式：`- 场景：经验要点`

- 踩坑经验自动化 / 会话内即时追加：原设计需要等 skill-evolution 审计才能追加经验，太慢。改为：Skill 调用中出现 ≥ 2 次重试后成功，会话结束时直接追加，不等审计
- Skill 候选识别 / 从执行轨迹生成：Hermes 的核心优势是自动从完成任务中创建 Skill，WorkBuddy 目前纯手动。应在 self-improvement 中加入候选识别，降低 Skill 创建门槛
