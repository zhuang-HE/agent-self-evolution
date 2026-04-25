---
name: skill-evolution
version: 1.4.0
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

⑤ **踩坑经验自动消化引擎（v1.2 新增）**

> **借鉴 Hermes 的经验内化机制**：踩坑经验不应该永远堆积在"踩坑区"——
> 当经验足够多，应该被提炼为 Skill 工作流中的显式步骤，让每次执行自动遵循。

**触发条件**：Skill 的未整合踩坑经验 ≥ 3 条

**消化流程**：

```
扫描踩坑经验列表（过滤掉 [已整合] 标记的）
    ↓
提取每条经验的：场景、工具/API、错误类型、解决方案
    ↓
聚类分析：是否有共同模式？
    │
    ├── 是（≥ 2 条经验共享相同模式）
    │       ↓
    │   提炼为显式工作流步骤：
    │   - 在 Skill 工作流程中插入新步骤
    │   - 步骤描述使用祈使句（"先验证 X 参数格式，再调用 Y"）
    │   - 标注来源经验编号
    │   - 将已消化的经验标记为 [已整合 → Step N]
    │
    └── 否（经验互相独立）
            ↓
        保留在踩坑区，等待更多经验积累
```

**消化示例**：

```markdown
# 消化前（tushare-data Skill 的踩坑区）

- daily / 查询单只股票：ts_code 必须带交易所后缀（000001.SZ），不能只传数字代码
- stk_mins / 分钟线数据：freq 参数只接受 1min/5min/15min/30min/60min
- index_weight / 获取成分股权重：必须传 index_code，不支持 ts_code

# 消化后（提炼为工作流步骤）

### Step 1.5 · 参数预处理（v1.2 从踩坑经验提炼）
在调用任何 Tushare API 之前，必须执行以下参数校验：
1. **股票代码格式化**：如果输入是纯数字（如 000001），自动追加交易所后缀
   - 6/0/3 开头 → .SH（沪市）
   - 0/3 开头 → .SZ（深市）
   - 来源经验：daily / 查询单只股票
2. **时间频率校验**：涉及 freq 参数时，验证值为 1min/5min/15min/30min/60min 之一
   - 来源经验：stk_mins / 分钟线数据
3. **参数名匹配**：指数类接口用 index_code，个股接口用 ts_code，不可混用
   - 来源经验：index_weight / 获取成分股权重

# 踩坑区更新

- daily / 查询单只股票：[已整合 → Step 1.5.1]
- stk_mins / 分钟线数据：[已整合 → Step 1.5.2]
- index_weight / 获取成分股权重：[已整合 → Step 1.5.3]
```

**消化频率**：每次 skill-evolution 审计时检查，或踩坑经验新增后触发

### Phase 3.5 · Registry Sync（注册表同步）（v1.3 新增）

> **问题**：skill-registry.json 是静态创建的，随时间推移会与实际 Skill 产生偏差。
> **方案**：每次审计时自动对比注册表与文件系统，修复不一致。

**同步流程**：

```
读取 skill-registry.json
    ↓
对比文件系统 ~/.workbuddy/skills/ 中的实际 Skill 列表
    ↓
检测以下不一致：
┌─────────────────────────────────────┬──────────────┐
│ 不一致类型                           │ 处理方式      │
├─────────────────────────────────────┼──────────────┤
│ 实际存在但注册表缺失                  │ 自动补充条目   │
│ 注册表存在但实际已删除                │ 从注册表移除   │
│ version 字段与 SKILL.md 不一致        │ 以 SKILL.md   │
│                                      │ 为准，更新注册表│
│ trigger_keywords 与 SKILL.md 不一致   │ 以 SKILL.md   │
│                                      │ 为准，更新注册表│
│ 新 Skill 的 capabilities 未填写       │ 根据 SKILL.md │
│                                      │ 内容自动推断   │
└─────────────────────────────────────┴──────────────┘
    ↓
生成同步报告（纳入 Phase 4 总报告）
```

**自动推断 capabilities 的规则**：

```
从 SKILL.md 中提取：
1. description 中的功能描述 → 能力关键词
2. 📋 工作流程的步骤名称 → 核心能力
3. 工具列表（tools 字段）→ 技术能力
4. 去重 + 抽象化（如 "帮我写API文档" → "文档生成"）
```

**注册表维护的触发时机汇总**：

| 时机 | 动作 |
|------|------|
| skill-evolution 审计 | Phase 3.5 自动同步 |
| Skill 自动创建完成 | 追加新条目到注册表 |
| Skill 卸载/删除 | 从注册表移除 |
| Skill 版本升级 | 更新 version + trigger_keywords |
| 记忆索引更新 | 如涉及 Skill 类别变化，同步更新 |

### Phase 3.6 · Dependency Health Check（依赖健康检查）（v1.4 新增）

> **问题**：skill-registry.json 中的 `depends_on` 字段是静态的，没有机制检查依赖是否就绪。
> **方案**：审计时验证关键依赖的可用性，标记不可用的 Skill。

**检查流程**：

```
扫描 skill-registry.json 中所有 depends_on 不为空的 Skill
    ↓
分类检查：

┌─────────────────────────────────┬──────────────────┬──────────────────────┐
│ 依赖类型                         │ 检查方式          │ 不可用时处理           │
├─────────────────────────────────┼──────────────────┼──────────────────────┤
│ 环境变量（如 TUSHARE_TOKEN）     │ 检查系统环境变量    │ 报告：⚠️ 缺少配置     │
│                                 │ 是否已设置         │ 建议：用户需配置       │
├─────────────────────────────────┼──────────────────┼──────────────────────┤
│ 外部工具（如 AkShare、Node.js） │ 尝试执行 --version │ 报告：❌ 未安装        │
│                                 │ 命令验证           │ 建议：提供安装指引     │
├─────────────────────────────────┼──────────────────┼──────────────────────┤
│ API Key（如 TMAP_*_KEY）        │ 检查系统环境变量    │ 报告：⚠️ 缺少 Key     │
│                                 │ 是否已设置         │ 建议：注册并配置       │
├─────────────────────────────────┼──────────────────┼──────────────────────┤
│ MCP Server（如 mcp-connector）  │ 检查 mcp.json     │ 报告：⚠️ 未配置       │
│                                 │ 是否有对应条目      │ 建议：安装 MCP Server  │
└─────────────────────────────────┴──────────────────┴──────────────────────┘
    ↓
生成依赖健康报告（纳入 Phase 4 总报告）
```

**检查命令参考**：

| 依赖 | 验证命令 |
|------|---------|
| Python 包（AkShare 等） | `python -c "import akshare; print(akshare.__version__)"` |
| Node.js 依赖 | `node -e "try{require('pkg')}catch(e){process.exit(1)}"` |
| 环境变量 | PowerShell: `[System.Environment]::GetEnvironmentVariable('VAR_NAME')` |
| MCP 配置 | 读取 `~/.workbuddy/mcp.json` 检查对应 server 条目 |

**依赖状态标记**：

在 skill-registry.json 中为每个 Skill 增加 `dependency_status` 字段（仅在审计后更新）：

```json
{
  "name": "tushare-data",
  "depends_on": ["TUSHARE_TOKEN 环境变量"],
  "dependency_status": {
    "TUSHARE_TOKEN": "✅ 已配置",
    "last_checked": "2026-04-25"
  }
}
```

**审计优先级调整**：

依赖不健康的 Skill 应该**优先审计**，因为：
- 用户可能因为配置问题而无法使用 Skill，但不知道问题出在哪
- 审计时可以顺便提供配置指引，提升用户体验

### Phase 4 · Report（报告）
```
生成进化报告，包含：
- 审计的 Skill 总数
- 注册表同步结果（新增/移除/更新的 Skill 数）
- 依赖健康检查结果（可用/不可用/未配置）
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
