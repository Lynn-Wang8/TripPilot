- [关键决策](memory/key-decisions.md) — 本次项目所有关键设计决策和原则

## 当前状态 (2026-07-08)

### 文档链（已完成 01-05，一致性已校验）
- ✅ `final/01_user_research.md` — 用户调研（3 个真实案例），MVP 场景选择（B+C），Planner 已标 Phase 2
- ✅ `final/02_user_story.md` — 小琳完整旅行故事
- ✅ `final/03_user_journey.md` — AS-IS 用户旅程 + Scenario 1/2/3 → 场景 B/C 映射表
- ✅ `final/04_AI_Value_Proposition+MVP_Scope.md` — 为什么需要 AI + MVP 范围 + 边界判断（5.10.7）
- ✅ `final/05_Agent_Skill_Design.md` — S1-S8 统一 7 段模板，S3/S6/S7 重点展开，S4 标 Phase 2
- ⚠️ `final/06_Page_Spec.md` — P1-P9，**待收窄为 5 页 MVP**

### 原型
- ⚠️ `prototype/index.html` — 当前是用户打字模式，**待改为引导式对话（豆包风格）**
- 🎨 `页面参考/` — 4 张设计意向图

### 冗余文件
- 🗑️ `final/AIAgent_finish.md` — ChatGPT 另一版草稿，与 04 重叠
- 🗑️ `final/A_Travel_Agent_PRD.md` — 空文件

## 本次会话完成的关键改动
1. **04_AI_Value_Proposition**：新增 5.2.3 AI vs OTA 对比表、5.3.4 决策推理链、5.5 架构重构（Memory → 底层能力层）、5.8.4 AI 输出策略、5.10.4 成功指标、5.10.5 目标用户、5.10.6 MVP 不做什么、5.10.7 边界判断
2. **05_Agent_Skill_Design**：全文重写，统一模板（目标→数据流→Prompt→分工→HITL→约束→验收），S3/S7 重点展开 RAG+Function Calling
3. **一致性修复**：S4 Phase 2 三处统一、S2 命名统一为 Preference Modeling、03 末尾加 Scenario→B/C 映射、01 移除 Planner Skill
4. **Function Calling**：从 Phase 2 移回 MVP（基础版：地图跳转/天气查询/航班查询）
5. **PRD v1**：`final/AI_Travel_Agent_PRD.md` — 13 节企业级模板（豆包版），内容全面但偏重
6. **PRD v2（当前）**：`final/TripPilot_PRD.md` — 9 节 AI PM 聚焦版，砍掉数据集/F1/并发，保留 Prompt/约束/HITL/MVP，更适合作业评审

## 下次继续
1. **P0**：重做 Prototype（引导式对话 + C 场景 70% 展示权重 + 豆包风格）
2. **P0**：写 Design Rationale（交付物3：设计阐述 — 设计理由 + AI 边界 + 未来演进）
3. **P1**：清理冗余文件（AIAgent_finish.md, docs/ 旧版文件, AI_Travel_Agent_PRD.md）
4. **P2**：最终格式美化（飞书/Notion/PDF）
