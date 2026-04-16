# 49. 审核流、版本管理与发布包

这一篇聚焦：

**为什么电影项目的最后阶段，不是“导出一个文件”，而是审核、版本、技术交付与合规的综合系统。**

---

## 1. 审核流真正解决什么问题

审核流通常解决：

- 内容是否通过
- 技术是否达标
- 版本是否一致
- 交付包是否完整
- 是否满足发行与放映要求

所以审核流不是附属流程，而是影片能否真正进入市场的最后控制层。

---

## 2. 一张审核流总图

```mermaid
flowchart TD
    A[Locked / Final Version] --> B[内容审核]
    B --> C[技术审核]
    C --> D[发布包制作]
    D --> E[发行准备]
```

---

## 3. 中国流程的现实特点

中国电影项目在发行前，通常需要经历较明确的送审与公映许可流程。公开资料显示，影片在通过内容审查后，需要申领公映许可证片头（龙标）；之后还需提交最终放映母版等材料进行技术审查，DCP 母版是关键交付物之一。[来源：国家电影局《电影片送审须知》](https://www.chinafilm.gov.cn/bsfw/bsxz/201910/t20191008_23978.html)；[来源：历信科技《电影送审完整流程指南》](https://www.dcpmk.com/news/film-submission-guide)

这意味着中国项目的后期与发行之间，审核流和技术交付耦合更紧。

---

## 4. 海外成熟流程的特点

海外成熟工业体系中，虽然不一定存在与“龙标”完全对应的统一流程，但通常会更强调：

- 发行版本规范
- 多格式母版管理
- 法务与版权清单
- 平台 / 院线交付标准

这意味着海外流程更偏“交付标准化”，中国流程更偏“审核 + 交付双重耦合”。

---

## 5. 一张国内外差异图

```mermaid
mindmap
  root((审核与发布差异))
    中国
      内容审查
      龙标
      技术审查
      DCP 送检
      合规耦合更强
    海外
      发行标准
      多母版管理
      法务清单
      平台交付规范
      标准化交付更强
```

---

## 6. 一张时序图

```mermaid
sequenceDiagram
    participant PostSup as Post Supervisor
    participant Legal
    participant Review
    participant Delivery
    participant Distributor

    PostSup->>Review: 提交最终版本
    Review->>PostSup: 返回修改或通过意见
    PostSup->>Delivery: 制作最终发布包
    Legal->>Delivery: 提供法务与版权材料
    Delivery->>Distributor: 提交发行包
```

---

## 7. 与 DeerFlow 的落地映射

可以设计：

- review-coordinator subagent
- delivery-manager subagent
- `ApprovalRecord` / `ReleasePackage` / `ComplianceChecklist` 对象
- 审核与交付 artifacts

---

## 8. 一张类图

```mermaid
classDiagram
    class ApprovalRecord {
      stage
      status
      notes
      required_changes
    }
    class ReleasePackage {
      package_id
      masters
      subtitles
      posters
      legal_docs
    }
    class ComplianceChecklist {
      item
      status
      owner
    }

    ApprovalRecord --> ReleasePackage
    ReleasePackage --> ComplianceChecklist
```

---

## 9. 第一版实现建议

第一版建议先支持：

- 审核状态记录
- 修改意见记录
- 发布包清单
- 合规检查清单
- 交付完成状态

---

## 10. 这一篇最重要的结论

### 结论一
审核流、版本管理与发布包，本质上是影片进入市场前的最后控制系统。

### 结论二
国内外差异的关键，在于中国更强调审核与交付耦合，海外更强调标准化交付与多版本管理。

### 结论三
导演智能体平台应当把审核、合规、发布包建模成对象、状态和清单流程。

---

<!-- movie-visuals:start -->
## 补充图示：换一种视角看本篇

下面这张 状态图 把“审核流、版本管理与发布包”再压缩成一个可快速扫读的结构视图，便于先抓关键关系，再回到正文细节。

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
- 上一篇：[48. VFX 后期协同与交付](./48-vfx-post-collaboration-and-delivery.md)
- 下一篇：[50. 宣发素材与发行协同](./50-marketing-assets-and-distribution-collaboration.md)

### 同组文档
- [37. principal photography 现场组织](./37-principal-photography-operations.md)
- [38. call sheet 与每日拍摄计划](./38-call-sheet-and-daily-plan.md)
- [39. 助理导演调度系统](./39-assistant-director-dispatch-system.md)
- [40. 进度控制与成本控制](./40-progress-and-cost-control.md)
- [41. 现场问题升级与决策机制](./41-on-set-escalation-and-decision-making.md)
- [42. 演员表演指导与导演反馈](./42-performance-direction-and-feedback.md)
- [43. 摄影、灯光、录音、视效现场协同](./43-on-set-collaboration-camera-light-sound-vfx.md)
- [44. dailies、出片与审核](./44-dailies-output-and-review.md)
- [45. 剪辑流程与版本推进](./45-editing-workflow-and-versioning.md)
- [46. 配音、配乐、音效协同](./46-adr-music-sound-collaboration.md)
- [47. 调色流程与视觉统一](./47-color-grading-and-visual-consistency.md)
- [48. VFX 后期协同与交付](./48-vfx-post-collaboration-and-delivery.md)
- 49. 审核流、版本管理与发布包（当前）
- [50. 宣发素材与发行协同](./50-marketing-assets-and-distribution-collaboration.md)
- [51. 项目复盘与知识沉淀](./51-project-retrospective-and-knowledge-capture.md)
<!-- movie-doc-nav:end -->
