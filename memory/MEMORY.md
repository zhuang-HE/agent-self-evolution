# MEMORY.md - 硅荔的长期记忆

> 最后更新：2026-04-24
> 维护规则：每月检查一次置信度，过期条目标记 [已过期]

---

## 🔧 通用偏好

- [0.9] **语言**：中文为主，技术术语保留英文
- [0.9] **沟通风格**：简洁直接，不要冗余寒暄，跳过"好的！""没问题！"
- [0.9] **输出格式**：优先 Markdown 表格，数据要附依据
- [0.9] **置信度标注**：回复中必须标注置信度（高/中/低），不确定时明确说
- [0.9] **代码质量**：代码要能跑，不要给半成品；给结论要附数据
- [0.7] **工作方式**：先分析方案再执行落地，要主动搜索对比筛选后直接编码实现
- [0.7] **系统偏好**：偏好绝对路径而非相对路径，bug 报告附完整 traceback
- [0.5] **知识管理**：使用 WorkBuddy AI + Obsidian 进行知识管理

---

## 📁 项目：量化交易系统

- **技术栈**：Python + Pandas/NumPy + Streamlit + Tushare/AkShare + Plotly + SQLite
- **架构**：分层设计（core/utils/data/signals/risk/trading/backtest）
- **状态**：持续迭代中
- **关键约束**：
  - 财务数据单位统一为亿元
  - 五维分析框架：RSI、布林带、KDJ + 基本面 + 资金面 + 消息面 + 形态面
  - Streamlit 中不要用 st.markdown 渲染 HTML，用原生组件更可靠
  - V10 版本对标 Wind/聚宽/米筐：个股五维分析(雷达图+5维度Tab)、多股对比(归一化+相关性矩阵)、组合优化、因子研究
- **编码偏好**：
  - 表单交互用 selectbox 预设 + text_input 自定义组合模式
  - 基本面评分需财报数据支撑，行情数据模拟评分只是临时方案
  - UI 风格：深色背景(#0a0e17) + 紧凑行距 + hover 蓝色边框
- **基础设施规划**：MongoDB + 可视化方案（进行中）
- **数据源**：Tushare（主力，Token: 29bbbd6...）+ AkShare + yfinance

---

## 📁 项目：WorkBuddy Skills 生态

- **技术栈**：SKILL.md (YAML frontmatter + Markdown) + references/ + scripts/
- **状态**：持续维护
- **已维护 Skills**：
  - tushare-data（v1.1.11，209 接口，~/.workbuddy/skills/tushare-data/）
  - stock-analyst（基于 AkShare）
  - finance-data-retrieval（NeoData 金融搜索）
  - non-motor-insurance-product-dev（非车险产品开发）
- **编码偏好**：
  - Skill 触发词要简洁通用，避免过于具体的个例
  - 踩坑经验格式：`- api_name / 场景描述：经验要点`
  - 触发词自进化：用户表述未激活对应 Skill 时，自动追加触发词

---

## 📁 项目：OpenClaw 鸿蒙应用

- **技术栈**：ArkTS + ArkUI（HarmonyOS NEXT）
- **状态**：开发中
- **关键经验**：已有 27 个编译错误 + 构建配置错误 + 运行时状态管理错误的完整案例积累

---

## 📚 跨项目经验教训

- [0.7] **Streamlit HTML 渲染**：st.markdown 的 unsafe_allow_html 在 Streamlit 1.54+ 中 HTML 被当纯文本，改用原生组件
- [0.7] **金融数据 API**：ts_code 必须带交易所后缀（000001.SZ），不能只传数字代码
- [0.5] **量化平台调研**：研究过 QuantConnect/TradingView/米筐/聚宽/BeeQuant/无限易，整理出 8 大核心模块

---

## 🚀 进化路线图（借鉴 Hermes Agent，2026-04-24 制定）

### 已完成 ✅
1. **记忆系统点火**：MEMORY.md 从空文件 → 结构化长期记忆（项目分区 + 置信度）
2. **自动触发强化**：self-improvement 从"建议触发"改为"必须触发"，明确最低阈值
3. **Skill 候选识别**：任务完成后自动判断是否应该创建 Skill（借鉴 Hermes 自动创建 Skill）
4. **踩坑经验即时化**：不再等审计才追加经验，会话中 ≥ 2 次重试后立即记录
5. **记忆按需检索**：memory-consolidation 加入关键词预检机制，避免全量读取浪费 token
6. **意图预判层**：self-improvement v2.1 加入意图预判，从被动触发词匹配升级为主动预判用户意图
7. **工具调用并行优化**：workflow-loop v1.1 加入并行调用规则，减少串行等待
8. **Skill 强制联动**：workflow-loop v1.1 加入关键任务路径的强制联动矩阵（代码→审查、重试→踩坑等）
9. **Token 预算管理**：strategic-compact v1.1 加入成本意识，任务开始时估算预算并监控效率
10. **Skill 自动创建 MVP**：self-improvement v2.2 加入完整创建流程（提取模式→生成模板→用户审核）
11. **记忆索引结构化**：memory-consolidation v2.2 加入 JSON 索引文件，实现按需检索取代全量读取
12. **踩坑经验自动消化**：skill-evolution v1.2 加入消化引擎，≥3 条经验→提炼为工作流显式步骤
13. **跨会话模式检测**：self-improvement v2.2 加入模式检测引擎，识别重复任务模式并主动推荐 Skill
14. **Skill 元数据注册表**：workflow-loop v1.2 加入 skill-registry.json，实现轻量级工具自省
15. **Skill 注册表自动同步**：skill-evolution v1.3 审计时自动对比注册表与文件系统，修复不一致
16. **上下文压力感知**：strategic-compact v1.2 代理指标推断压力，4 级响应策略 + 大文件保护 + Skill 加载成本控制
17. **Skill 精简加载**：workflow-loop v1.3 L0-L3 分级加载，按上下文压力选择级别
18. **质量反馈回路**：self-improvement v2.3 5 维评分体系 + quality_history 趋势追踪
19. **Skill 注册表首同步**：v1.4 实战执行注册表同步，修正 4 个版本偏差 + capabilities 同步 + 新增 registry_sync_log
20. **跨会话模式增强**：memory-index.json 新增 key_lessons 字段 + "自进化系统迭代"模式
21. **自动触发决策树**：self-improvement v2.4 6 级优先级触发 + 工具调用计数规则
22. **依赖健康检查**：skill-evolution v1.4 审计时验证 depends_on 环境变量/依赖可用性
23. **索引自动重建**：memory-consolidation v2.3 整理后全量重建 memory-index.json 行号
24. **注册表即时同步**：workflow-loop v1.4 Skill 变更后立即同步注册表，不等审计
25. **联动验证清单**：workflow-loop v1.4 任务开始预判联动需求，完成后验证执行
26. **审计轨迹记录**：memory-consolidation v2.4 audit_history 追踪每次自进化操作
27. **Skill 健康度评分**：skill-evolution v1.5 5 维加权评分 + 健康等级 + 趋势追踪
28. **依赖检查实战验证**：v1.5 执行 Phase 3.6 实际验证，tushare ✅ / akshare ✅ / 地图 Key ⚠️

### 下一步计划 📋
1. **实战验证**（高优先级）：在真实任务中验证所有新能力的端到端效果（6/8 已验证）
2. **数据驱动改进**（高优先级）：积累 ≥ 10 次质量评分后，分析系统性短板
3. **健康度趋势分析**（中优先级）：积累 ≥ 3 次健康度评分后，追踪 Skill 健康度变化

### 平台层改进建议（需 WorkBuddy 官方支持）📋
1. **上下文压缩智能保留**：让 Agent 标记"关键上下文段"（critical context），自动压缩时保留文件路径、函数签名、错误信息，丢弃寒暄和重复内容
2. **Skill 精简加载**：Skill 加载时只注入核心流程，reference 文件按需加载，避免加载 2-3 个 Skill 后上下文被占满
3. **并行工具调用反馈**：提供更精细的并行调用控制和结果管理机制

### 暂不适用 ⏸️
- **RL 数据飞轮**：WorkBuddy 是云端产品，无法微调底层模型。除非未来支持自定义模型
