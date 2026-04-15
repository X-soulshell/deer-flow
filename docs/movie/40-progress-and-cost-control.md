# 40. 进度控制与成本控制

这一篇聚焦：

**为什么进度控制与成本控制不是两个系统，而是一套联动的生产控制系统。**

---

## 1. 为什么进度与成本必须联动

电影项目里，时间几乎总是直接转化为成本。

每增加一天拍摄，通常都会增加：

- 人工
- 设备
- 场地
- 交通
- 后勤
- 保险与管理成本

所以进度偏差如果不被及时识别，就会迅速变成成本失控。

---

## 2. 一张联动图

```mermaid
flowchart LR
    A[拍摄进度] --> B[工时变化]
    B --> C[成本变化]
    C --> D[预算偏差]
    D --> E[调整决策]
    E --> A
```

---

## 3. 海外成熟流程的特点

海外成熟工业体系通常更强调：

- 预算与排期同步编制
- 每日进度复盘
- 关键路径监控
- contingency 使用规则
- 工时与安全规则

这意味着进度与成本控制更容易形成闭环。

---

## 4. 国内常见差异

国内项目中，常见情况是：

- 进度与成本数据分散
- 现场调整频繁
- 风险预警滞后
- 依赖经验判断而非系统指标

但系统化制片实践正在推动这一点改善，尤其在中大型项目中更明显。

---

## 5. 一张甘特式逻辑图

```mermaid
flowchart TD
    A[计划进度] --> B[实际进度]
    B --> C{是否偏差}
    C -->|否| D[继续执行]
    C -->|是| E[偏差分析]
    E --> F[成本影响评估]
    F --> G[调整方案]
    G --> D
```

---

## 6. 与 DeerFlow 的落地映射

可以设计：

- budget-controller subagent
- assistant-director subagent
- producer subagent
- `ProgressSnapshot` / `CostSnapshot` / `VarianceReport` 对象
- 风险预警 artifacts

---

## 7. 一张类图

```mermaid
classDiagram
    class ProgressSnapshot {
      date
      planned_completion
      actual_completion
      blockers
    }
    class CostSnapshot {
      date
      planned_cost
      actual_cost
      variance
    }
    class VarianceReport {
      variance_type
      impact
      mitigation
      owner
    }

    ProgressSnapshot --> VarianceReport
    CostSnapshot --> VarianceReport
```

---

## 8. 国内外差异对平台设计的启示

如果平台要适配国内外项目，必须支持：

- 标准化日报 / 周报
- 快速偏差分析
- 预算与排期联动
- 适合压缩工期项目的快速预警模式

---

## 9. 第一版实现建议

第一版建议先支持：

- 进度偏差摘要
- 成本偏差摘要
- 高风险场景标记
- 调整建议
- 风险升级记录

---

## 10. 这一篇最重要的结论

### 结论一
进度控制与成本控制本质上是一套联动的生产控制系统。

### 结论二
国内外差异的关键，在于偏差是否被结构化记录、分析和升级。

### 结论三
导演智能体平台必须具备进度-成本联动分析能力，才能真正支撑大规模制作。