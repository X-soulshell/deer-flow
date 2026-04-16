# 59. 场地子智能体设计

这一篇聚焦：

**为什么场地子智能体不是“找地方”，而是把场景需求转成许可、成本、后勤与视觉适配判断的关键角色。**

---

## 1. 场地子智能体的核心职责

它至少要承担：

- 场景需求结构化
- 候选场地匹配
- 许可与时间窗口风险识别
- 成本与后勤影响分析
- 场地锁定建议

---

## 2. 一张职责图

```mermaid
flowchart TD
    A[Scene Requirements] --> B[Location Subagent]
    B --> C[候选场地]
    B --> D[许可风险]
    B --> E[成本/后勤影响]
    B --> F[锁定建议]
```

---

## 3. 海外与国内差异

公开资料显示，在中国拍摄时，许可要求会因城市、地点类型、项目属性而变化，公共空间、无人机、敏感地点等往往涉及更复杂审批链，且审批时间不一定符合西方团队预期。[来源：CN Fixer, Film Production Process China](https://cnfixer.com/china/film-production-process-china.html)

这意味着中国项目中的场地子智能体必须更重视：

- 审批链
- 地方协调
- 时间窗口
- 许可不确定性

而海外成熟流程更偏标准化 location management。

---

## 4. 一张时序图

```mermaid
sequenceDiagram
    participant Director
    participant LocationAgent
    participant Producer
    participant DP
    participant LocalPartner

    Director->>LocationAgent: 提供场景与风格需求
    LocationAgent->>Producer: 查询成本与后勤约束
    LocationAgent->>DP: 查询拍摄可行性
    LocationAgent->>LocalPartner: 查询许可与地方要求
    Producer->>LocationAgent: 返回资源约束
    DP->>LocationAgent: 返回摄影可行性
    LocalPartner->>LocationAgent: 返回审批风险
    LocationAgent->>Director: 输出锁定建议
```

---

## 5. 与 DeerFlow 的落地映射

可以设计：

- location subagent
- `LocationCandidate` / `PermitRisk` / `LocationDecision` 对象
- 场地 artifacts

---

## 6. 一张类图

```mermaid
classDiagram
    class LocationCandidate {
      name
      fit_score
      permit_complexity
      logistics_score
    }
    class PermitRisk {
      location_name
      risk_level
      notes
    }
    class LocationDecision {
      selected_location
      alternatives
      rationale
    }

    LocationCandidate --> PermitRisk
    LocationCandidate --> LocationDecision
```

---

## 7. 第一版实现建议

第一版建议先支持：

- 场景需求摘要
- 候选场地匹配
- 许可风险摘要
- 后勤影响摘要
- 锁定建议

---

## 8. 为什么场地子智能体对中国项目尤其重要

因为中国项目中的场地问题，往往不仅是视觉问题，更是审批、协调、时间窗口问题。

这使它成为非常现实、非常高价值的试点模块。

如果把场地判断放进锁定矩阵里看，会更容易看出为什么视觉适配和审批可行性必须一起考虑：

```mermaid
quadrantChart
    title 场地锁定判断矩阵
    x-axis 低视觉适配 --> 高视觉适配
    y-axis 高审批与后勤风险 --> 低审批与后勤风险
    quadrant-1 优先锁定
    quadrant-2 视觉候补
    quadrant-3 暂不投入
    quadrant-4 保留观察
    "场地 A" : [0.86, 0.82]
    "场地 B" : [0.57, 0.76]
    "场地 C" : [0.39, 0.35]
    "场地 D" : [0.83, 0.47]
```

---

## 9. 这一篇最重要的结论

### 结论一
场地子智能体的本质，是把场景需求转成许可、成本、后勤与视觉适配判断。

### 结论二
国内外差异的关键，在于中国项目更强调审批链、地方协调与时间窗口不确定性。

### 结论三
在导演智能体平台中，location subagent 是最适合真实试点落地的专业角色之一。

---

<!-- movie-visuals:start -->
## 补充图示：换一种视角看本篇

下面这张 时序图 把“场地子智能体设计”再压缩成一个可快速扫读的结构视图，便于先抓关键关系，再回到正文细节。

```mermaid
sequenceDiagram
    participant U as 用户/项目
    participant L as Lead Agent
    participant S as 专业角色
    participant G as 治理层
    participant A as 产物/状态

    U->>L: 提出 场地子智能体设计
    L->>S: 角色定位
    S-->>L: 输入边界
    L->>G: 协作接口
    G-->>L: 输出产物
    L->>A: 升级路径
```
<!-- movie-visuals:end -->

<!-- movie-doc-nav:start -->
## 文档导航

- 总入口：[README.md](./README.md)
- 阅读地图：[00-reading-map.md](./00-reading-map.md)
- 所在分组：52-60 智能体角色设计
- 上一篇：[58. 选角子智能体设计](./58-casting-subagent-design.md)
- 下一篇：[60. 摄影语言子智能体设计](./60-cinematography-language-subagent-design.md)

### 同组文档
- [52. 导演主智能体设计](./52-director-lead-agent-design.md)
- [53. 制片子智能体设计](./53-producer-subagent-design.md)
- [54. 剧本分析子智能体设计](./54-script-analyst-subagent-design.md)
- [55. 分镜子智能体设计](./55-storyboard-subagent-design.md)
- [56. 预算子智能体设计](./56-budget-subagent-design.md)
- [57. 排期子智能体设计](./57-scheduling-subagent-design.md)
- [58. 选角子智能体设计](./58-casting-subagent-design.md)
- 59. 场地子智能体设计（当前）
- [60. 摄影语言子智能体设计](./60-cinematography-language-subagent-design.md)
<!-- movie-doc-nav:end -->
