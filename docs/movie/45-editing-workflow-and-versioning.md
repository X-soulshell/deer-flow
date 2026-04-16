# 45. 剪辑流程与版本推进

这一篇聚焦：

**为什么剪辑不是把素材拼起来，而是通过版本推进不断重构叙事。**

---

## 1. 剪辑流程的本质

剪辑通常不是一次完成，而是经历：

- assembly cut
- rough cut
- fine cut
- locked cut

每一版都在回答不同问题：

- 素材是否完整
- 叙事是否成立
- 节奏是否成立
- 情绪是否成立
- 是否可以进入后续声音、调色、VFX 锁定流程

---

## 2. 一张版本推进图

```mermaid
flowchart LR
    A[Assembly Cut] --> B[Rough Cut]
    B --> C[Fine Cut]
    C --> D[Locked Cut]
    D --> E[进入后期锁定流程]
```

---

## 3. 海外成熟流程的特点

海外成熟工业体系通常更强调：

- 版本命名规范
- review notes 结构化
- 与声音、调色、VFX 的锁定边界清晰
- 版本回退与比较机制稳定

这意味着剪辑流程更容易被系统化管理。

---

## 4. 国内常见差异

国内项目中，常见情况是：

- 剪辑与导演反馈往返更频繁
- 某些项目在送审前仍有较多结构调整
- 版本记录可能分散在文件夹、聊天记录、口头沟通中

这意味着平台必须支持“版本对象 + 审核意见 + 锁定状态”。

---

## 5. 一张时序图

```mermaid
sequenceDiagram
    participant Editor
    participant Director
    participant Producer
    participant Sound
    participant Color

    Editor->>Director: 提交新版本
    Director->>Editor: 提供叙事与节奏反馈
    Producer->>Director: 提供时长与市场反馈
    Director->>Editor: 确认修改方向
    Editor->>Sound: 通知接近锁定版本
    Editor->>Color: 通知接近锁定版本
```

---

## 6. 与 DeerFlow 的落地映射

可以设计：

- editor subagent
- post-supervisor subagent
- `EditVersion` / `ReviewNote` / `LockStatus` 对象
- 版本比较 artifacts

---

## 7. 一张状态图

```mermaid
stateDiagram-v2
    [*] --> Assembly
    Assembly --> RoughCut
    RoughCut --> FineCut
    FineCut --> LockedCut
    LockedCut --> SoundPost
    LockedCut --> ColorPost
    LockedCut --> VFXFinal
    VFXFinal --> DeliveryReady
    DeliveryReady --> [*]
```

---

## 8. 第一版实现建议

第一版建议先支持：

- 剪辑版本记录
- review notes 结构化记录
- 锁定状态标记
- 版本差异摘要
- 后续部门依赖提示

---

## 9. 为什么版本推进是后期核心能力

因为后期不是单次产出，而是多轮判断与收敛。

版本推进能力，决定了后期是否可控。

---

## 10. 这一篇最重要的结论

### 结论一
剪辑流程本质上是通过版本推进不断重构叙事。

### 结论二
国内外差异的关键，在于版本管理、review notes 和锁定边界是否清晰。

### 结论三
导演智能体平台应当把剪辑流程建模成版本对象、审核对象和锁定状态机。

---

<!-- movie-visuals:start -->
## 补充图示：换一种视角看本篇

下面这张 状态图 把“剪辑流程与版本推进”再压缩成一个可快速扫读的结构视图，便于先抓关键关系，再回到正文细节。

```mermaid
stateDiagram-v2
    state "拍摄调度" as S1
    state "现场协同" as S2
    state "版本回看" as S3
    state "后期整合" as S4
    state "交付复盘" as S5

    [*] --> S1
    S1 --> S2
    S2 --> S3
    S3 --> S4
    S4 --> S5
    S5 --> [*]
```
<!-- movie-visuals:end -->

<!-- movie-doc-nav:start -->
## 文档导航

- 总入口：[README.md](./README.md)
- 阅读地图：[00-reading-map.md](./00-reading-map.md)
- 所在分组：37-51 拍摄执行、后期与发行
- 上一篇：[44. dailies、出片与审核](./44-dailies-output-and-review.md)
- 下一篇：[46. 配音、配乐、音效协同](./46-adr-music-sound-collaboration.md)

### 同组文档
- [37. principal photography 现场组织](./37-principal-photography-operations.md)
- [38. call sheet 与每日拍摄计划](./38-call-sheet-and-daily-plan.md)
- [39. 助理导演调度系统](./39-assistant-director-dispatch-system.md)
- [40. 进度控制与成本控制](./40-progress-and-cost-control.md)
- [41. 现场问题升级与决策机制](./41-on-set-escalation-and-decision-making.md)
- [42. 演员表演指导与导演反馈](./42-performance-direction-and-feedback.md)
- [43. 摄影、灯光、录音、视效现场协同](./43-on-set-collaboration-camera-light-sound-vfx.md)
- [44. dailies、出片与审核](./44-dailies-output-and-review.md)
- 45. 剪辑流程与版本推进（当前）
- [46. 配音、配乐、音效协同](./46-adr-music-sound-collaboration.md)
- [47. 调色流程与视觉统一](./47-color-grading-and-visual-consistency.md)
- [48. VFX 后期协同与交付](./48-vfx-post-collaboration-and-delivery.md)
- [49. 审核流、版本管理与发布包](./49-review-flow-versioning-and-release-package.md)
- [50. 宣发素材与发行协同](./50-marketing-assets-and-distribution-collaboration.md)
- [51. 项目复盘与知识沉淀](./51-project-retrospective-and-knowledge-capture.md)
<!-- movie-doc-nav:end -->
