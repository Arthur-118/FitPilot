# FitPilot — outline 配套 PPT 大纲（图解版）

<!-- 用途：outline 提交的配套 Powerpoint（02 Guidelines 要求），非 Final 现场演示。
     PPT 使命：把 outline 的每一项"讲得更细"——用图解/截图/代码片段/UI mockups 说明。
     节奏：课程 14 周（与老师确认后按 14 周写工作计划，避免出现 week 15）。
     对齐评分（04 Rubric）：Layout&Slides 10%、Related Apps 15%、Open Source/Tools 15%、Features 25%、Plan 15%、Proposed UI 20%。
     页标题用英文（PPT 直接采用），要点用中文（准备讲稿用）。
     ⚠️ 每页"图解"标记的就是制作 PPT 时必放的图（mockups/流程图/公式图/截图）。 -->

---

## P1 · 封面
**Title: FitPilot — Your AI Fitness Coach that Sees, Thinks and Tracks**

- 副标题：Weather-Aware, Recovery-Driven, Pose-Checking Fitness Coach for Android
- 课程名 + 学期 + 小组成员（姓名/学号）
- 图解：App 图标 + 三会口号（会看/会想/会记）

---

## P2 · 简要 App 想法（对应用例 About the App · 02 要求①）
**Idea in One Sentence: Decide, Watch, Track**

- 一句话：不是让你自己挑课，而是"今天练什么、怎么做标准、练完记住"三件事全包的个人 AI 教练
- 三个能力支柱 1-2-3 简述（会想 / 会看 / 会记）
- **图解：三支柱 → 闭环小图**（Input→Decide→Feedback→Store 回环）

---

## P3 · 为什么存在缺口（About the App 里的 Niche）
**The Gap: No One Combines These**

- 痛点三连：不知道练什么 / 没人看动作 / 数据不互通
- 一句话定位：全本地、可解释的教练闭环
- **图解：痛点 → 解决对照图（3 行：痛点→我们的解法）**

---

## P4 · 相关 App 分析（02 要求② · Rubric 15%）
**Related Apps: What They Do & Where They Fail**

| App | Features/Components | Gap |
|---|---|---|
| Fitbod | 按肌肉恢复生成计划 | 无天气、无实时姿势反馈 |
| Keep（国内） | 课程+伴练评分 | 跑步/力量割裂、AI 走服务端 |
| Good Habits ❤基座 | 本地姿态计数 90+ 动作 | 只计数、无推荐无语音 |
| HeartRunner | BLE 心率+GPS+语音 | 仅跑步 |

- 每行说明其 features/functionalities（评分要求"描述细节"）
- **图解：4 宫格对比表 + 每 App 一句话注解**

---

## P5 · 可复用的开源代码（02 要求③）
**Open Source We Build Upon**

- Good Habits（主基座）：姿态识别管线、Room 数据层、Compose UI 模式 —— 直接复用点标注
- HeartRunner（MIT）：BLE 心率连接 → 跑步场景参考其 BLE GATT 代码
- wger：仅参考数据结构，不复制代码（AGPL 说明，避免版权问题）
- 动作库：本地打包（Good Habits 内置 + 开放 JSON 数据）
- **图解：Good Habits 仓库结构截图，圈出复用的模块**（README / 关键目录）

---

## P6 · 新增功能总览（02 要求④ 前言 → Rubric Features 25%）
**New Features at a Glance**

- 8 项新增 vs Good Habits 基座：推荐引擎 / 恢复评分 / 姿势标准判定 / 跳绳 / TTS 语音体系 / HIIT+拉伸 / 统一闭环 / 天气+AQI
- 每项一句话 + Relevance（对应评分哪一维：Input/Process/Store/Output）
- **图解：特征地图（分四类：会想/会看/会记/会存）或功能分层图**

---

## P7 · 功能详解 1：推荐引擎（Processing 得分主力）
**Feature: Equipment-Aware Daily Recommendation Engine**

- 输入：Onboarding 问卷（场景/设备/目标/频率/经验）+ 天气 AQI + 恢复分
- 处理：动作库标签化 → **硬过滤**（if 设备不匹配剔除）→ **加权评分**排序取前 3
- 独特性：可解释的每一步（"为什么推荐卧推"能回答）
- **图解：过滤漏斗 + 加权求和公式图**（Σ weight×score → 排序）

---

## P8 · 功能详解 2：恢复评分
**Feature: Recovery Score (0–100)**

- 四原料 if 分档：睡眠(0.40)/静息心率(0.25)/疲劳(0.20)/训练量(0.15)
- 数据来源分级：表单打卡 / BLE 心率（可选）/ 本地历史
- 输出：分档驱动强度；连续 3 天 <40 → 建议休息
- **图解：四权重 → 求和 → 分档强度表（数学框图）**

---

## P9 · 功能详解 3：姿态教练（AI 得分点）
**Feature: On-Device Pose Coach (TF Lite + MoveNet)**

- 为什么本地 AI：离线、免费、可解释（符合"勿提交无法解释的 AI 代码"）
- 计数 + 关节角度判定（肩/肘）标准与否 → TTS 纠正
- 扩展：深蹲/平板/跳绳
- **图解：关键点骨架叠加示意 + 关节角度标注图**

---

## P10 · 功能详解 4：跑步 + 统一闭环
**Feature: Running Session & The Closed Loop**

- 跑步：GPS 里程/配速 + BLE 心率曲线 + 语音里程碑
- 闭环：今日训练 → 入库 → 恢复分更新 → 明日推荐强度调整
- AQI 超标 → 自动改室内（国内痛点）
- **图解：闭环回路动画/箭头图 + 跑步实时屏小样**

---

## P11 · 图解页：Proposed UI 1 — Onboarding（Rubric UI 20%）
**UI Mockup: Onboarding Wizard**

- 5 问向导流程：场景 → 设备 → 目标 → 频率 → 经验
- 设计要点：一步一屏、进度条、选择卡片、中文
- **图解：mockup 图 ×2-3（步骤1-2-3），标 Material 组件与可访问性点**

---

## P12 · 图解页：Proposed UI 2 — 首页今日计划
**UI Mockup: Today's Plan Home**

- 恢复评分圆环 + 天气/AQI 卡（含"为何改室内"解释文案）+ 推荐卡列表（动作/组×次/器械）+ 一键开始
- **图解：完整首页 mockup + 推荐卡构成标注**

---

## P13 · 图解页：Proposed UI 3 — 力量训练会话
**UI Mockup: Strength Session (Pose Overlay)**

- 摄像头 + 骨架叠加 + 大计数 + 动作状态(标准/警告) + TTS 播报
- 结束页：本组次统计
- **图解：训练中 mockup + 结束统计页 mockup**

---

## P14 · 图解页：Proposed UI 4 — 跑步页 + 统计页
**UI Mockup: Running Page & Unified Stats**

- 跑步页：实时心率曲线/距离配速/地图轨迹
- 统计页：统一图表（训练量/距离/恢复趋势/连续天数），按场景筛选
- **图解：两屏 mockup 并排 + 图表类型选择说明**

---

## P15 · 技术栈与开源工具（Rubric Tools 15%）
**Tool Stack: Free · On-Device · Mainland-Accessible**

- 表格：用途 / 选型 / 为什么（QWeather、TF Lite、Room、Compose、BLE、TTS、高德/osmdroid、Retrofit）
- 强调：全链路免费优先、离线优先、演示零风险
- **图解：技术栈表格 + 集成关系图（数据流向）**

---

## P16 · 工作计划（02 要求⑤ → Rubric Plan 15%）
**Work Plan: Week 6 → Week 14**

- 里程碑：Alpha(wk7 末) / Beta(wk10 末) / Final(wk14)
- 分阶段任务简表（对应 outline 第 8 节 timeline）
- 风险与对策：无网络/无BLE/无相机权限降级方案
- 加分项：跳绳/HIIT/拉伸（wk13 if time）
- **图解：甘特条 + 三里程碑标记**

---

## P17 · 结束页
**Thank You / Questions**

- 一句话回顾 + App 图标 + 成员
- （可加）GitHub repo 链接，符合 00 指南"提交第一页放仓库链接"的要求

---

<!-- 制作备注：
1. P11-P14 是 UI 20% 得分关键：必须是完整、自洽、体现 Material 3 的 mockup，
   颜色/布局/可访问性一致，不要放线稿示意。
2. 03 exemplar 提示每阶段更新累积：Alpha/Beta 提交时在 P7-P10 补真实截图与代码片段。
3. 全片逻辑流 = idea → related apps → features+图解 → UI mockups → tools → plan，对齐 04 的 Layout 评分。
4. PPT 随 outline 一起提交，无需做成最终演示稿。 -->