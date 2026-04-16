# 58. 选角子智能体设计

这一篇聚焦：

**为什么选角子智能体不是“推荐演员名单”，而是把角色需求转成候选比较、风险判断与决策支持的关键角色。**

---

## 1. 选角子智能体的核心职责

它至少要承担：

- 角色画像生成
- 候选演员匹配
- 档期与预算风险识别
- 替代方案建议
- 决策依据结构化记录

---

## 2. 一张职责图

```mermaid
flowchart LR
    A[角色需求] --> B[Casting Subagent]
    B --> C[候选匹配]
    B --> D[档期风险]
    B --> E[预算风险]
    B --> F[替代方案]
```

---

## 3. 海外与国内差异

海外成熟流程中，casting director、试镜流程、档期矩阵更标准化。

国内项目中，选角更容易受到：

- 市场因素
- 档期波动
- 资方偏好
- 宣发策略

影响。

所以选角子智能体必须支持：

- 创作适配
- 市场适配
- 执行适配

---

## 4. 一张状态图

```mermaid
stateDiagram-v2
    [*] --> RoleProfile
    RoleProfile --> CandidatePool
    CandidatePool --> Evaluation
    Evaluation --> Shortlist
    Shortlist --> Decision
    Decision --> [*]
```

---

## 5. 与 DeerFlow 的落地映射

可以设计：

- casting subagent
- `RoleProfile` / `CandidateEvaluation` / `CastingDecision` 对象
- 选角 artifacts

---

## 6. 一张类图

```mermaid
classDiagram
    class RoleProfile {
      role_name
      age_range
      traits
      performance_requirements
    }
    class CandidateEvaluation {
      actor_name
      fit_score
      schedule_risk
      budget_risk
    }
    class CastingDecision {
      selected_actor
      alternatives
      rationale
    }

    RoleProfile --> CandidateEvaluation
    CandidateEvaluation --> CastingDecision
```

---

## 7. 第一版实现建议

第一版建议先支持：

- 角色画像
- 候选比较
- 档期风险摘要
- 成本风险摘要
- 替代方案建议

---

## 8. 为什么选角子智能体不能只做“推荐”

因为选角不是搜索问题，而是多目标平衡问题。

平台必须把决策依据结构化，才能真正帮助导演与制片判断。

---

## 9. 这一篇最重要的结论

### 结论一
选角子智能体的本质，是把角色需求转成候选比较、风险判断与决策支持。

### 结论二
国内外差异的关键，在于选角是否被标准化、矩阵化、可追踪。

### 结论三
在导演智能体平台中，casting subagent 应当服务创作、市场与执行三重目标。

---

<!-- movie-visuals:start -->
## 补充图示：换一种视角看本篇

下面这张 旅程图 把“选角子智能体设计”再压缩成一个可快速扫读的结构视图，便于先抓关键关系，再回到正文细节。

```mermaid
journey
    title 选角子智能体设计 的协作旅程
    section 起步
      角色定位: 5: 用户, 平台
      输入边界: 4: Lead Agent
    section 展开
      协作接口: 4: 专业角色
      输出产物: 3: 治理层
    section 收束
      升级路径: 5: 项目团队
```
<!-- movie-visuals:end -->

<!-- movie-doc-nav:start -->
## 文档导航

- 总入口：[README.md](./README.md)
- 阅读地图：[00-reading-map.md](./00-reading-map.md)
- 所在分组：52-60 智能体角色设计
- 上一篇：[57. 排期子智能体设计](./57-scheduling-subagent-design.md)
- 下一篇：[59. 场地子智能体设计](./59-location-subagent-design.md)

### 同组文档
- [52. 导演主智能体设计](./52-director-lead-agent-design.md)
- [53. 制片子智能体设计](./53-producer-subagent-design.md)
- [54. 剧本分析子智能体设计](./54-script-analyst-subagent-design.md)
- [55. 分镜子智能体设计](./55-storyboard-subagent-design.md)
- [56. 预算子智能体设计](./56-budget-subagent-design.md)
- [57. 排期子智能体设计](./57-scheduling-subagent-design.md)
- 58. 选角子智能体设计（当前）
- [59. 场地子智能体设计](./59-location-subagent-design.md)
- [60. 摄影语言子智能体设计](./60-cinematography-language-subagent-design.md)
<!-- movie-doc-nav:end -->
