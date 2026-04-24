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

### 下一步计划 📋
1. **记忆索引结构化**（中优先级）：MEMORY.md > 50KB 时引入 JSON 索引文件，实现类似 FTS5 的按需检索
2. **Skill 自动创建 MVP**（高优先级）：实现最简版的"完成复杂任务 → 提取模式 → 生成 SKILL.md 模板 → 用户审核"
3. **踩坑经验自动消化**（中优先级）：当同一 Skill 踩坑经验 ≥ 3 条时，自动提炼为工作流显式步骤
4. **跨会话模式检测**（低优先级）：每次会话开始时与 MEMORY.md 对比，检测重复任务模式

### 暂不适用 ⏸️
- **RL 数据飞轮**：WorkBuddy 是云端产品，无法微调底层模型。除非未来支持自定义模型
