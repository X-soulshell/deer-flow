# 39. 助理导演调度系统

这一篇聚焦：

**为什么助理导演系统是把导演意图转成现场秩序的关键中枢。**

---

## 1. 助理导演系统的本质

助理导演系统不是“辅助导演创作”，而是：

- 维护现场秩序
- 推进拍摄节奏
- 协调演员与部门到位
- 处理时间冲突
- 升级现场问题

它本质上是执行调度系统。

---

## 2. 一张调度图

```mermaid
flowchart TD
    A[导演目标] --> B[1st AD 调度]
    B --> C[演员调度]
    B --> D[部门调度]
    B --> E[场景切换]
    B --> F[时间控制]
    C --> G[现场秩序]
    D --> G
    E --> G
    F --> G
```

---

## 3. 海外成熟流程的特点

海外成熟工业体系中，1st AD 的职责边界通常更清晰，现场秩序、工时、安全、节奏推进都更制度化。

这意味着助理导演系统更容易被标准化建模。

---

## 4. 国内常见差异

国内项目中，助理导演系统也很关键，但常见情况是：

- 现场协调更依赖核心人员经验
- 导演、执行制片、现场副导演之间即时沟通更多
- 某些项目的调度信息分散在群聊、表格、口头通知中

这意味着平台需要支持“结构化调度 + 即时变更”。

---

## 5. 一张时序图

```mermaid
sequenceDiagram
    participant Director
    participant AD
    participant Cast
    participant Departments
    participant Producer

    Director->>AD: 确认当前场景优先级
    AD->>Cast: 通知到位与准备
    AD->>Departments: 通知机位/灯位/道具准备
    Departments->>AD: 反馈准备状态
    AD->>Producer: 反馈时间与资源风险
    Producer->>Director: 升级关键冲突
    Director->>AD: 确认调整方案
```

---

## 6. 与 DeerFlow 的落地映射

可以设计：

- assistant-director subagent
- dispatch coordinator subagent
- `DispatchTask` / `DepartmentReadiness` / `EscalationEvent` 对象
- 现场调度 artifacts

---

## 7. 一张状态图

```mermaid
stateDiagram-v2
    [*] --> PendingDispatch
    PendingDispatch --> CastReady
    PendingDispatch --> DepartmentReady
    CastReady --> SceneReady
    DepartmentReady --> SceneReady
    SceneReady --> Shooting
    Shooting --> DelayEscalation
    DelayEscalation --> Rescheduled
    Shooting --> Completed
    Rescheduled --> Completed
    Completed --> [*]
```

---

## 8. 第一版实现建议

第一版建议先支持：

- 调度任务清单
- 部门准备状态
- 延误风险提示
- 升级事件记录
- 调整建议摘要

---

## 9. 为什么助理导演系统是平台化关键能力

因为它是把“计划”变成“秩序”的关键层。

没有调度系统，平台就无法真正进入现场执行。

---

## 10. 这一篇最重要的结论

### 结论一
助理导演系统是把导演意图转成现场秩序的关键中枢。

### 结论二
国内外差异的关键，在于调度是否被标准化、结构化、可追踪。

### 结论三
在导演智能体平台中，assistant-director 应当是中期制作阶段的核心角色。

---

---

<!-- movie-doc-nav:start -->
## 文档导航

- 总入口：[README.md](./README.md)
- 阅读地图：[00-reading-map.md](./00-reading-map.md)
- 所在分组：37-51 拍摄执行、后期与发行
- 上一篇：[38. call sheet 与每日拍摄计划](./38-call-sheet-and-daily-plan.md)
- 下一篇：[40. 进度控制与成本控制](./40-progress-and-cost-control.md)

### 同组文档
- [37. principal photography 现场组织](./37-principal-photography-operations.md)
- [38. call sheet 与每日拍摄计划](./38-call-sheet-and-daily-plan.md)
- 39. 助理导演调度系统（当前）
- [40. 进度控制与成本控制](./40-progress-and-cost-control.md)
- [41. 现场问题升级与决策机制](./41-on-set-escalation-and-decision-making.md)
- [42. 演员表演指导与导演反馈](./42-performance-direction-and-feedback.md)
- [43. 摄影、灯光、录音、视效现场协同](./43-on-set-collaboration-camera-light-sound-vfx.md)
- [44. dailies、出片与审核](./44-dailies-output-and-review.md)
- [45. 剪辑流程与版本推进](./45-editing-workflow-and-versioning.md)
- [46. 配音、配乐、音效协同](./46-adr-music-sound-collaboration.md)
- [47. 调色流程与视觉统一](./47-color-grading-and-visual-consistency.md)
- [48. VFX 后期协同与交付](./48-vfx-post-collaboration-and-delivery.md)
- [49. 审核流、版本管理与发布包](./49-review-flow-versioning-and-release-package.md)
- [50. 宣发素材与发行协同](./50-marketing-assets-and-distribution-collaboration.md)
- [51. 项目复盘与知识沉淀](./51-project-retrospective-and-knowledge-capture.md)
<!-- movie-doc-nav:end -->
