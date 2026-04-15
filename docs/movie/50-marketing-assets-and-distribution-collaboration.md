# 50. 宣发素材与发行协同

这一篇聚焦：

**为什么宣发不是电影做完之后才开始，而是从后期末端就要与版本、素材、发行节奏联动。**

---

## 1. 宣发素材真正依赖什么

宣发素材通常依赖：

- 稳定版本画面
- 海报与剧照
- trailer / teaser
- 角色物料
- 发行节奏
- 平台与院线要求

所以宣发不是独立部门的末端工作，而是与后期版本推进强耦合。

---

## 2. 一张协同图

```mermaid
flowchart LR
    A[后期稳定版本] --> B[宣发素材提取]
    B --> C[海报/剧照]
    B --> D[预告片]
    B --> E[角色物料]
    C --> F[发行协同]
    D --> F
    E --> F
```

---

## 3. 海外成熟流程的特点

海外成熟工业体系通常更强调：

- marketing deliverables 清单化
- trailer / poster / social assets 的版本管理
- 与发行窗口、平台规范联动

这意味着宣发素材更容易被纳入交付系统。

---

## 4. 国内常见差异

国内项目中，常见情况是：

- 宣发节奏与送审、定档、平台窗口耦合更紧
- 某些物料会在版本尚未完全稳定时提前制作
- 物料修改频率较高

这意味着平台需要支持“素材版本 + 发行节点 + 审核状态”的联动。

---

## 5. 一张时序图

```mermaid
sequenceDiagram
    participant PostSup as Post Supervisor
    participant Marketing
    participant Distributor
    participant Director
    participant Legal

    PostSup->>Marketing: 提供可用版本与素材池
    Marketing->>Director: 提交海报/预告方向
    Director->>Marketing: 确认创作方向
    Legal->>Marketing: 确认可用素材边界
    Marketing->>Distributor: 提交发行物料
    Distributor->>Marketing: 反馈渠道要求
```

---

## 6. 与 DeerFlow 的落地映射

可以设计：

- marketing-coordinator subagent
- distribution-coordinator subagent
- `MarketingAsset` / `TrailerVersion` / `ReleaseWindow` 对象
- 宣发物料 artifacts

---

## 7. 一张状态图

```mermaid
stateDiagram-v2
    [*] --> AssetDraft
    AssetDraft --> InternalReview
    InternalReview --> LegalCheck
    LegalCheck --> DistributorReview
    DistributorReview --> ScheduledRelease
    ScheduledRelease --> Published
    Published --> [*]
```

---

## 8. 第一版实现建议

第一版建议先支持：

- 宣发素材清单
- trailer / poster 版本记录
- 发行窗口记录
- 渠道要求摘要
- 素材可用性检查

---

## 9. 为什么宣发协同是平台化最后一公里

因为它把“影片完成”真正转化为“市场进入”。

如果平台不能管理宣发与发行协同，就无法覆盖电影项目的完整生命周期。

---

## 10. 这一篇最重要的结论

### 结论一
宣发素材与发行协同，本质上是后期版本、市场节奏与渠道要求的联动系统。

### 结论二
国内外差异的关键，在于宣发是否与审核、定档、渠道交付形成稳定联动。

### 结论三
导演智能体平台应当把宣发与发行建模成素材对象、版本对象和发布状态机。