# Movie 导演智能体方案导览

本目录用于系统化说明：如何基于当前 DeerFlow 项目，逐步改造成面向大规模电影制作的导演智能体系统。

这套文档不是单篇长文，而是按“由浅入深、从概念到落地”的方式拆成多份 Markdown，方便你按不同阅读目标进入。

---

## 阅读顺序建议

### 如果你想先快速建立全局认知
按下面顺序阅读：

1. [01-overview.md](./01-overview.md)
2. [02-current-project-mapping.md](./02-current-project-mapping.md)
3. [03-target-architecture.md](./03-target-architecture.md)

这三篇会先回答三个问题：

- 电影导演智能体到底是什么
- 当前 DeerFlow 已经具备哪些基础能力
- 应该如何从现有架构演进到电影制作系统

---

### 如果你更关心业务流程
继续阅读：

4. [04-production-phases.md](./04-production-phases.md)
5. [05-agent-system.md](./05-agent-system.md)
6. [21-traditional-filmmaking-overview.md](./21-traditional-filmmaking-overview.md)
7. [22-non-ai-filmmaking-organization.md](./22-non-ai-filmmaking-organization.md)
8. [23-mapping-traditional-process-to-agent-platform.md](./23-mapping-traditional-process-to-agent-platform.md)
9. [24-deerflow-transformation-roadmap.md](./24-deerflow-transformation-roadmap.md)
37. [37-principal-photography-operations.md](./37-principal-photography-operations.md)
38. [38-call-sheet-and-daily-plan.md](./38-call-sheet-and-daily-plan.md)
39. [39-assistant-director-dispatch-system.md](./39-assistant-director-dispatch-system.md)
40. [40-progress-and-cost-control.md](./40-progress-and-cost-control.md)
41. [41-on-set-escalation-and-decision-making.md](./41-on-set-escalation-and-decision-making.md)
42. [42-performance-direction-and-feedback.md](./42-performance-direction-and-feedback.md)
43. [43-on-set-collaboration-camera-light-sound-vfx.md](./43-on-set-collaboration-camera-light-sound-vfx.md)
44. [44-dailies-output-and-review.md](./44-dailies-output-and-review.md)
51. [51-project-retrospective-and-knowledge-capture.md](./51-project-retrospective-and-knowledge-capture.md)

这几篇重点讲：

- 传统电影制作在没有 AI 时如何真实运转
- 传统岗位、文档、审批如何映射到导演智能体平台
- DeerFlow 应该如何渐进式改造成电影制作系统
- 中期拍摄执行、call sheet、调度、升级与进度成本控制
- 国内外流程差异与系统化制片的真实落地方式
- 项目复盘与知识沉淀如何进入平台闭环

---

### 如果你更关心前期制作落地
继续阅读：

10. [25-script-development-and-lock.md](./25-script-development-and-lock.md)
11. [26-script-breakdown-and-breakdown-sheet.md](./26-script-breakdown-and-breakdown-sheet.md)
12. [27-budgeting-and-line-producer-view.md](./27-budgeting-and-line-producer-view.md)
13. [28-scheduling-and-first-ad-view.md](./28-scheduling-and-first-ad-view.md)
14. [29-casting-and-actor-management.md](./29-casting-and-actor-management.md)
15. [30-location-scouting-and-lock.md](./30-location-scouting-and-lock.md)
31. [31-art-costume-props-collaboration.md](./31-art-costume-props-collaboration.md)
32. [32-cinematography-lighting-vfx-preproduction.md](./32-cinematography-lighting-vfx-preproduction.md)
33. [33-text-storyboard-and-shot-list.md](./33-text-storyboard-and-shot-list.md)
34. [34-static-storyboards-and-moodboards.md](./34-static-storyboards-and-moodboards.md)
35. [35-style-reference-analysis-and-unification.md](./35-style-reference-analysis-and-unification.md)
36. [36-dialogue-design-and-polish.md](./36-dialogue-design-and-polish.md)

这几篇重点讲：

- 锁稿、breakdown、预算、排期、选角、勘景
- 服化道、摄影灯光视效、分镜、氛围图、风格统一、对白润色
- 这些传统前期流程如何映射到智能体角色、对象和工具
- 国内外在前期组织化、文档化、技术预演上的差异

---

### 如果你更关心后期与发行落地
继续阅读：

45. [45-editing-workflow-and-versioning.md](./45-editing-workflow-and-versioning.md)
46. [46-adr-music-sound-collaboration.md](./46-adr-music-sound-collaboration.md)
47. [47-color-grading-and-visual-consistency.md](./47-color-grading-and-visual-consistency.md)
48. [48-vfx-post-collaboration-and-delivery.md](./48-vfx-post-collaboration-and-delivery.md)
49. [49-review-flow-versioning-and-release-package.md](./49-review-flow-versioning-and-release-package.md)
50. [50-marketing-assets-and-distribution-collaboration.md](./50-marketing-assets-and-distribution-collaboration.md)

这几篇重点讲：

- 剪辑、声音、调色、VFX、审核流、发布包、宣发与发行
- 国内外在后期版本管理、审核机制、交付标准上的差异
- 中国送审、龙标、DCP 技审等现实约束如何映射到平台设计

---

### 如果你更关心智能体角色设计
继续阅读：

52. [52-director-lead-agent-design.md](./52-director-lead-agent-design.md)
53. [53-producer-subagent-design.md](./53-producer-subagent-design.md)
54. [54-script-analyst-subagent-design.md](./54-script-analyst-subagent-design.md)
55. [55-storyboard-subagent-design.md](./55-storyboard-subagent-design.md)
56. [56-budget-subagent-design.md](./56-budget-subagent-design.md)
57. [57-scheduling-subagent-design.md](./57-scheduling-subagent-design.md)
58. [58-casting-subagent-design.md](./58-casting-subagent-design.md)
59. [59-location-subagent-design.md](./59-location-subagent-design.md)
60. [60-cinematography-language-subagent-design.md](./60-cinematography-language-subagent-design.md)

这几篇重点讲：

- 导演主智能体与各专业子智能体的职责边界
- 国内外流程差异如何影响角色设计
- 这些角色如何映射到 DeerFlow 的 Lead Agent、task、state、artifacts

---

### 如果你更关心系统落地
继续阅读：

16. [06-data-models.md](./06-data-models.md)
17. [07-tools-memory-skills.md](./07-tools-memory-skills.md)
18. [08-roadmap.md](./08-roadmap.md)

这三篇重点讲：

- 项目对象模型怎么设计
- 工具、记忆、技能系统怎么扩展
- 如何分阶段实施，避免一开始做得过重

---

### 如果你更关心“方案如何落到当前源码”
继续阅读：

19. [09-source-mapping-overview.md](./09-source-mapping-overview.md)
20. [10-source-mapping-agent-runtime.md](./10-source-mapping-agent-runtime.md)
21. [11-source-mapping-subagents.md](./11-source-mapping-subagents.md)
22. [12-source-mapping-state-and-config.md](./12-source-mapping-state-and-config.md)

这四篇重点讲：

- 每个方案点在当前仓库里对应哪些代码入口
- 当前 Lead Agent、task、Subagent、ThreadState、AgentConfig 分别能承接什么
- 哪些地方适合做最小改造，哪些地方适合做中长期扩展

---

### 如果你想直接看体系化设计稿
继续阅读：

23. [13-system-blueprint.md](./13-system-blueprint.md)
24. [14-implementation-draft.md](./14-implementation-draft.md)
25. [20-master-plan-50-docs.md](./20-master-plan-50-docs.md)

这几篇重点讲：

- 平台级系统蓝图
- 从当前仓库出发的具体实施草案
- 50+ 文档体系的总规划

---

### 如果你想把 A / B / C 三组内容一次看全
继续阅读：

26. [15-a-code-design-draft.md](./15-a-code-design-draft.md)
27. [16-b-interfaces-and-data-contracts.md](./16-b-interfaces-and-data-contracts.md)
28. [17-c-first-code-drop-plan.md](./17-c-first-code-drop-plan.md)

这三篇分别对应：

- A：代码级设计草案
- B：接口与数据结构草案
- C：第一版代码落地方案

---

### 如果你想继续看“方案1 / 方案2”的带图细稿
继续阅读：

29. [18-solution-1-detailed-md-drafts.md](./18-solution-1-detailed-md-drafts.md)
30. [19-solution-2-mvp-implementation-path.md](./19-solution-2-mvp-implementation-path.md)

这两篇分别对应：

- 方案1：可直接开发的 md 细稿集合
- 方案2：最小 MVP 代码实现路径与模块关系图

---

## 文档结构

| 文件 | 作用 | 适合谁看 |
|------|------|----------|
| [01-overview.md](./01-overview.md) | 总体目标、核心概念、系统定位 | 所有人 |
| [02-current-project-mapping.md](./02-current-project-mapping.md) | 当前 DeerFlow 架构与电影导演智能体的映射关系 | 架构师、研发 |
| [03-target-architecture.md](./03-target-architecture.md) | 目标系统架构图、分层设计、主从多智能体结构 | 架构师、技术负责人 |
| [04-production-phases.md](./04-production-phases.md) | 前期 / 中期 / 后期的阶段工作流设计 | 制片、导演、产品 |
| [05-agent-system.md](./05-agent-system.md) | 导演智能体、部门智能体、执行智能体的角色体系 | 架构师、产品、研发 |
| [06-data-models.md](./06-data-models.md) | Project / Script / Budget / Schedule / ShotPlan / Review 等数据模型 | 后端、产品 |
| [07-tools-memory-skills.md](./07-tools-memory-skills.md) | 工具层、记忆层、技能层的扩展方案 | 后端、算法、平台 |
| [08-roadmap.md](./08-roadmap.md) | 分阶段实施路线、MVP 范围、风险与优先级 | 技术负责人、项目经理 |
| [09-source-mapping-overview.md](./09-source-mapping-overview.md) | 源码对照总览，建立方案与仓库入口的映射 | 架构师、研发 |
| [10-source-mapping-agent-runtime.md](./10-source-mapping-agent-runtime.md) | 主智能体与 middleware 运行时的源码对照 | 后端、架构师 |
| [11-source-mapping-subagents.md](./11-source-mapping-subagents.md) | task、SubagentExecutor、registry 的源码对照 | 后端、算法、架构师 |
| [12-source-mapping-state-and-config.md](./12-source-mapping-state-and-config.md) | ThreadState、AgentConfig、factory 的源码对照 | 后端、平台 |
| [13-system-blueprint.md](./13-system-blueprint.md) | 平台级系统蓝图 | 技术负责人、架构师、产品 |
| [14-implementation-draft.md](./14-implementation-draft.md) | 从当前仓库出发的实施设计稿 | 技术负责人、后端、平台 |
| [15-a-code-design-draft.md](./15-a-code-design-draft.md) | A 组：代码级设计草案 | 后端、架构师、技术负责人 |
| [16-b-interfaces-and-data-contracts.md](./16-b-interfaces-and-data-contracts.md) | B 组：接口与数据结构草案 | 后端、平台、产品 |
| [17-c-first-code-drop-plan.md](./17-c-first-code-drop-plan.md) | C 组：第一版代码落地方案 | 后端、技术负责人、实施团队 |
| [18-solution-1-detailed-md-drafts.md](./18-solution-1-detailed-md-drafts.md) | 方案1：可直接开发的 md 细稿集合 | 后端、架构师、实施团队 |
| [19-solution-2-mvp-implementation-path.md](./19-solution-2-mvp-implementation-path.md) | 方案2：最小 MVP 实现路径与模块关系图 | 后端、技术负责人、实施团队 |
| [20-master-plan-50-docs.md](./20-master-plan-50-docs.md) | 50+ 文档总规划 | 架构师、技术负责人、产品 |
| [21-traditional-filmmaking-overview.md](./21-traditional-filmmaking-overview.md) | 传统电影制作全流程总览 | 所有人 |
| [22-non-ai-filmmaking-organization.md](./22-non-ai-filmmaking-organization.md) | 无 AI 电影制作的组织结构 | 架构师、产品、研发 |
| [23-mapping-traditional-process-to-agent-platform.md](./23-mapping-traditional-process-to-agent-platform.md) | 传统流程到导演智能体平台的映射方法 | 架构师、研发 |
| [24-deerflow-transformation-roadmap.md](./24-deerflow-transformation-roadmap.md) | DeerFlow 改造总路线图 | 技术负责人、架构师 |
| [25-script-development-and-lock.md](./25-script-development-and-lock.md) | 剧本开发与锁稿 | 编剧、导演、制片、研发 |
| [26-script-breakdown-and-breakdown-sheet.md](./26-script-breakdown-and-breakdown-sheet.md) | 剧本拆解与 breakdown sheet | 制片、后端、平台 |
| [27-budgeting-and-line-producer-view.md](./27-budgeting-and-line-producer-view.md) | 预算体系与 line producer 视角 | 制片、后端、技术负责人 |
| [28-scheduling-and-first-ad-view.md](./28-scheduling-and-first-ad-view.md) | 排期体系与 1st AD 视角 | 助理导演、后端、平台 |
| [29-casting-and-actor-management.md](./29-casting-and-actor-management.md) | 选角流程与演员管理 | 导演、制片、产品 |
| [30-location-scouting-and-lock.md](./30-location-scouting-and-lock.md) | 场地勘景与场地锁定 | 制片、摄影、后端 |
| [31-art-costume-props-collaboration.md](./31-art-costume-props-collaboration.md) | 美术、服装、道具协同 | 美术、服装、道具、研发 |
| [32-cinematography-lighting-vfx-preproduction.md](./32-cinematography-lighting-vfx-preproduction.md) | 摄影、灯光、视效前期协同 | 摄影、灯光、视效、平台 |
| [33-text-storyboard-and-shot-list.md](./33-text-storyboard-and-shot-list.md) | 文字分镜与镜头表 | 导演、摄影、后端 |
| [34-static-storyboards-and-moodboards.md](./34-static-storyboards-and-moodboards.md) | 静态分镜图与氛围图 | 导演、美术、平台 |
| [35-style-reference-analysis-and-unification.md](./35-style-reference-analysis-and-unification.md) | 风格参考分析与风格统一 | 导演、摄影、美术、平台 |
| [36-dialogue-design-and-polish.md](./36-dialogue-design-and-polish.md) | 对白设计与润色 | 编剧、导演、产品 |
| [37-principal-photography-operations.md](./37-principal-photography-operations.md) | principal photography 现场组织 | 制片、助理导演、平台 |
| [38-call-sheet-and-daily-plan.md](./38-call-sheet-and-daily-plan.md) | call sheet 与每日拍摄计划 | 助理导演、制片、后端 |
| [39-assistant-director-dispatch-system.md](./39-assistant-director-dispatch-system.md) | 助理导演调度系统 | 助理导演、平台、后端 |
| [40-progress-and-cost-control.md](./40-progress-and-cost-control.md) | 进度控制与成本控制 | 制片、技术负责人、平台 |
| [41-on-set-escalation-and-decision-making.md](./41-on-set-escalation-and-decision-making.md) | 现场问题升级与决策机制 | 制片、助理导演、平台 |
| [42-performance-direction-and-feedback.md](./42-performance-direction-and-feedback.md) | 演员表演指导与导演反馈 | 导演、演员、产品 |
| [43-on-set-collaboration-camera-light-sound-vfx.md](./43-on-set-collaboration-camera-light-sound-vfx.md) | 摄影、灯光、录音、视效现场协同 | 摄影、录音、视效、平台 |
| [44-dailies-output-and-review.md](./44-dailies-output-and-review.md) | dailies、出片与审核 | 导演、摄影、后期、平台 |
| [45-editing-workflow-and-versioning.md](./45-editing-workflow-and-versioning.md) | 剪辑流程与版本推进 | 剪辑、导演、后期总监 |
| [46-adr-music-sound-collaboration.md](./46-adr-music-sound-collaboration.md) | 配音、配乐、音效协同 | 声音、配乐、后期 |
| [47-color-grading-and-visual-consistency.md](./47-color-grading-and-visual-consistency.md) | 调色流程与视觉统一 | 调色、导演、后期 |
| [48-vfx-post-collaboration-and-delivery.md](./48-vfx-post-collaboration-and-delivery.md) | VFX 后期协同与交付 | 视效、后期总监、平台 |
| [49-review-flow-versioning-and-release-package.md](./49-review-flow-versioning-and-release-package.md) | 审核流、版本管理与发布包 | 制片、法务、发行、平台 |
| [50-marketing-assets-and-distribution-collaboration.md](./50-marketing-assets-and-distribution-collaboration.md) | 宣发素材与发行协同 | 宣发、发行、制片 |
| [51-project-retrospective-and-knowledge-capture.md](./51-project-retrospective-and-knowledge-capture.md) | 项目复盘与知识沉淀 | 制片、导演、平台 |
| [52-director-lead-agent-design.md](./52-director-lead-agent-design.md) | 导演主智能体设计 | 架构师、后端、产品 |
| [53-producer-subagent-design.md](./53-producer-subagent-design.md) | 制片子智能体设计 | 制片、架构师、平台 |
| [54-script-analyst-subagent-design.md](./54-script-analyst-subagent-design.md) | 剧本分析子智能体设计 | 编剧、后端、平台 |
| [55-storyboard-subagent-design.md](./55-storyboard-subagent-design.md) | 分镜子智能体设计 | 导演、摄影、平台 |
| [56-budget-subagent-design.md](./56-budget-subagent-design.md) | 预算子智能体设计 | 制片、后端、平台 |
| [57-scheduling-subagent-design.md](./57-scheduling-subagent-design.md) | 排期子智能体设计 | 助理导演、平台、后端 |
| [58-casting-subagent-design.md](./58-casting-subagent-design.md) | 选角子智能体设计 | 导演、制片、产品 |
| [59-location-subagent-design.md](./59-location-subagent-design.md) | 场地子智能体设计 | 制片、摄影、平台 |
| [60-cinematography-language-subagent-design.md](./60-cinematography-language-subagent-design.md) | 摄影语言子智能体设计 | 摄影、导演、平台 |

---

## 这套方案的核心思想

这套方案的核心不是“做一个会聊电影的 AI”，而是：

- 做一个能管理电影项目的总导演智能体
- 做一组能承担部门职责的专业子智能体
- 做一套能承载剧本、预算、排期、镜头、版本、审核的项目对象系统
- 做一条贯穿前期、中期、后期的阶段化工作流

换句话说，目标不是一个聊天机器人，而是一个电影制作 AI 操作系统。

---

## 与当前项目的关系

当前 DeerFlow 已经具备几个非常关键的基础：

- Lead Agent 主智能体入口
- `task` 工具驱动的子智能体委派机制
- ThreadState 线程状态
- MemoryMiddleware 记忆能力
- Skills 注入机制
- Sandbox 工作区与文件产物能力
- 自定义 agent 配置能力

因此，这次改造不是推倒重来，而是在现有多智能体底座上做行业化升级。

---

## 推荐阅读方式

### 方式一：产品视角
先看：

- [01-overview.md](./01-overview.md)
- [04-production-phases.md](./04-production-phases.md)
- [08-roadmap.md](./08-roadmap.md)
- [21-traditional-filmmaking-overview.md](./21-traditional-filmmaking-overview.md)
- [22-non-ai-filmmaking-organization.md](./22-non-ai-filmmaking-organization.md)
- [37-principal-photography-operations.md](./37-principal-photography-operations.md)
- [40-progress-and-cost-control.md](./40-progress-and-cost-control.md)
- [45-editing-workflow-and-versioning.md](./45-editing-workflow-and-versioning.md)
- [49-review-flow-versioning-and-release-package.md](./49-review-flow-versioning-and-release-package.md)
- [50-marketing-assets-and-distribution-collaboration.md](./50-marketing-assets-and-distribution-collaboration.md)
- [51-project-retrospective-and-knowledge-capture.md](./51-project-retrospective-and-knowledge-capture.md)

### 方式二：架构视角
先看：

- [02-current-project-mapping.md](./02-current-project-mapping.md)
- [03-target-architecture.md](./03-target-architecture.md)
- [05-agent-system.md](./05-agent-system.md)
- [06-data-models.md](./06-data-models.md)
- [13-system-blueprint.md](./13-system-blueprint.md)
- [15-a-code-design-draft.md](./15-a-code-design-draft.md)
- [18-solution-1-detailed-md-drafts.md](./18-solution-1-detailed-md-drafts.md)
- [20-master-plan-50-docs.md](./20-master-plan-50-docs.md)
- [23-mapping-traditional-process-to-agent-platform.md](./23-mapping-traditional-process-to-agent-platform.md)
- [24-deerflow-transformation-roadmap.md](./24-deerflow-transformation-roadmap.md)
- [31-art-costume-props-collaboration.md](./31-art-costume-props-collaboration.md)
- [32-cinematography-lighting-vfx-preproduction.md](./32-cinematography-lighting-vfx-preproduction.md)
- [35-style-reference-analysis-and-unification.md](./35-style-reference-analysis-and-unification.md)
- [45-editing-workflow-and-versioning.md](./45-editing-workflow-and-versioning.md)
- [49-review-flow-versioning-and-release-package.md](./49-review-flow-versioning-and-release-package.md)
- [52-director-lead-agent-design.md](./52-director-lead-agent-design.md)
- [53-producer-subagent-design.md](./53-producer-subagent-design.md)

### 方式三：实现视角
先看：

- [02-current-project-mapping.md](./02-current-project-mapping.md)
- [06-data-models.md](./06-data-models.md)
- [07-tools-memory-skills.md](./07-tools-memory-skills.md)
- [08-roadmap.md](./08-roadmap.md)
- [10-source-mapping-agent-runtime.md](./10-source-mapping-agent-runtime.md)
- [11-source-mapping-subagents.md](./11-source-mapping-subagents.md)
- [12-source-mapping-state-and-config.md](./12-source-mapping-state-and-config.md)
- [14-implementation-draft.md](./14-implementation-draft.md)
- [16-b-interfaces-and-data-contracts.md](./16-b-interfaces-and-data-contracts.md)
- [17-c-first-code-drop-plan.md](./17-c-first-code-drop-plan.md)
- [19-solution-2-mvp-implementation-path.md](./19-solution-2-mvp-implementation-path.md)
- [24-deerflow-transformation-roadmap.md](./24-deerflow-transformation-roadmap.md)
- [25-script-development-and-lock.md](./25-script-development-and-lock.md)
- [26-script-breakdown-and-breakdown-sheet.md](./26-script-breakdown-and-breakdown-sheet.md)
- [27-budgeting-and-line-producer-view.md](./27-budgeting-and-line-producer-view.md)
- [28-scheduling-and-first-ad-view.md](./28-scheduling-and-first-ad-view.md)
- [33-text-storyboard-and-shot-list.md](./33-text-storyboard-and-shot-list.md)
- [38-call-sheet-and-daily-plan.md](./38-call-sheet-and-daily-plan.md)
- [39-assistant-director-dispatch-system.md](./39-assistant-director-dispatch-system.md)
- [40-progress-and-cost-control.md](./40-progress-and-cost-control.md)
- [44-dailies-output-and-review.md](./44-dailies-output-and-review.md)
- [45-editing-workflow-and-versioning.md](./45-editing-workflow-and-versioning.md)
- [47-color-grading-and-visual-consistency.md](./47-color-grading-and-visual-consistency.md)
- [48-vfx-post-collaboration-and-delivery.md](./48-vfx-post-collaboration-and-delivery.md)
- [49-review-flow-versioning-and-release-package.md](./49-review-flow-versioning-and-release-package.md)
- [52-director-lead-agent-design.md](./52-director-lead-agent-design.md)
- [54-script-analyst-subagent-design.md](./54-script-analyst-subagent-design.md)
- [55-storyboard-subagent-design.md](./55-storyboard-subagent-design.md)
- [56-budget-subagent-design.md](./56-budget-subagent-design.md)
- [57-scheduling-subagent-design.md](./57-scheduling-subagent-design.md)
- [58-casting-subagent-design.md](./58-casting-subagent-design.md)
- [59-location-subagent-design.md](./59-location-subagent-design.md)
- [60-cinematography-language-subagent-design.md](./60-cinematography-language-subagent-design.md)

---

## 一句话总结

**Movie 目录描述的是：如何把 DeerFlow 从通用多智能体工作流系统，演进成面向大规模电影制作的导演智能体平台，并把方案直接映射到当前仓库的源码结构、对象契约、开发细稿、传统电影工业流程、国内外差异、审核交付机制、项目复盘与智能体角色设计上。**
