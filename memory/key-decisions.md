---
name: key-decisions
description: 本次项目的关键设计决策、原则和 MVP 边界
metadata:
  type: project
---

# 关键设计决策

## 产品定位
- **一句话**：AI 的价值不是提供更多旅行信息，而是在复杂、不确定的旅行环境中，帮助用户完成更高质量的决策，并主动处理变化。
- **传统 OTA**：搜索 → 筛选 → 购买（信息检索问题）
- **AI Agent**：理解 → 推理 → 决策 → 执行 → 调整（复杂决策问题）

## MVP 范围
- **场景 B（Decision Agent）**：S1 需求收集 + S2 偏好建模 + S3 多维对比。验证「AI 帮用户做更好的选择」
- **场景 C（Emergency Agent）**：S5 监控 + S6 影响分析 + S7 动态重规划 + S8 执行引导。验证「AI 从被动工具升级为主动助手」
- **Function Calling 基础版**：地图跳转、天气查询、航班查询（Deep Link，不直接支付）
- **Phase 2**：S4 行程规划、Memory 跨行程沉淀、S9 规则校验、Function Calling 深度集成
- **Phase 3**：多目的地对比、多人协同、手表联动

## AI 设计原则
### 为什么必须用 AI
- 传统规则无法理解自然语言中的隐含偏好和多目标权衡
- AI 的不可替代价值：跨维度推理 + 偏好加权解释 + 反直觉发现 + 上下文感知告警分级

### S1-S8 统一模板（7 段）
1. Skill 目标（用户问题 → Skill 目标 → 为什么用 AI）
2. Data Flow（Input → Processing → Output，标注数据来源）
3. Prompt 设计（表格化：角色/输入/思考步骤/输出格式/红线）
4. Agent 处理分工（LLM/Memory/Rule Engine 各负责什么）
5. Human in the Loop（用户在哪个节点参与决策）
6. Constraint（如何防止幻觉，Prompt 约束 vs 程序硬约束）
7. 验收标准（checklist）

### Human in the Loop 原则
- AI 推荐但不决定——任何选择最终由用户确认
- S2 让用户排序偏好，AI 不猜权重（Rank Sum 公式计算）
- S6 只陈述客观影响 + 提供选项，不替用户判断"好不好"
- S3 两个目的地按钮完全平等，AI 不替用户选

### LLM 能力边界
- LLM 管"想"（推理、解释、生成），系统管"记"（存储）和"验"（校验）
- S9 纯规则引擎：时间/地理/状态/衔接/预算/数据真实性 6 项校验
- 所有实时数据走 API，LLM 只用训练数据理解语义，不编造数字
- 输出字段标注来源：user_input / memory / llm_inferred

### RAG 策略
- 景点结构化信息 → 关系型 DB（SQL 精确查询）
- 机票/酒店/天气 → API 实时查询
- 攻略经验文本 → RAG 向量检索（标注来源链接）
- 用户偏好 → Key-Value / localStorage
- 不用微调：微调解决"行为风格"，RAG 解决"外部动态知识"

### AI 输出可信度分级
- 事实信息 → 引用数据源 + 时间戳
- 预测信息 → 标注概率 + 不确定性
- 推荐信息 → 解释依据，不直接说"选这个"
- 安全信息 → 强制告警，不用模糊措辞

## 平台与视觉
- 手机端 Web App（PWA），375px 设计基准
- 森林绿 #2D9F89 主色调，温暖可信赖的旅行管家风格
- 豆包式对话交互（引导式提问，非用户自由打字）
- 禁止：AI 科技风、霓虹、赛博、机器人元素

## 文档链
```
01_user_research → 02_user_story → 03_user_journey → 04_AI_Value_Proposition+MVP_Scope → 05_Agent_Skill_Design → 06_Page_Spec → Prototype
```
