# 34. 静态分镜图与氛围图

这一篇聚焦：

**为什么静态分镜图与氛围图，不只是“好看”，而是跨部门统一视觉理解的关键媒介。**

---

## 1. 静态分镜图与氛围图分别解决什么问题

- 静态分镜图：解决镜头构图、动作关系、空间关系
- 氛围图：解决色调、材质、情绪、世界观质感

两者结合，才能让导演意图被不同部门共同理解。

---

## 2. 一张双轨图

```mermaid
flowchart LR
    A[剧本与风格] --> B[静态分镜图]
    A --> C[氛围图]
    B --> D[镜头理解统一]
    C --> E[视觉气质统一]
    D --> F[跨部门执行一致]
    E --> F
```

---

## 3. 为什么它们在真实项目里重要

因为不同部门对文字的理解可能不同，但对图像的理解更容易对齐。

例如：

- 摄影看构图与光线
- 美术看空间与材质
- 服装看色彩与角色状态
- 灯光看氛围与层次
- VFX 看后期合成空间

---

## 4. 一张泳道图

```mermaid
flowchart TD
    A[导演意图] --> B[静态分镜图]
    A --> C[氛围图]
    B --> D[摄影理解]
    B --> E[动作理解]
    C --> F[美术理解]
    C --> G[服装理解]
    C --> H[灯光理解]
    D --> I[统一执行]
    E --> I
    F --> I
    G --> I
    H --> I
```

---

## 5. 国内外差异

海外成熟流程中，moodboard、lookbook、storyboard 往往更早进入部门协同；复杂项目会形成完整视觉开发包。

国内项目中，常见情况是：

- 导演与摄影先形成视觉方向
- 其他部门在后续逐步跟进
- 图像资料有时分散在不同工具和群聊中

这意味着平台需要把图像资料结构化，而不是散落在文件夹里。

---

## 6. 与 DeerFlow 的落地映射

可以设计：

- style-research subagent
- storyboard subagent
- `StyleBoard` / `MoodBoard` / `StoryboardFrame` 对象
- 图像引用与版本记录
- 视觉审核 artifacts

---

## 7. 一张状态图

```mermaid
stateDiagram-v2
    [*] --> DraftMoodboard
    DraftMoodboard --> ReviewedMoodboard
    ReviewedMoodboard --> LockedMoodboard
    LockedMoodboard --> StoryboardFrames
    StoryboardFrames --> DepartmentAlignment
    DepartmentAlignment --> ShootReady
    ShootReady --> [*]
```

---

## 8. 第一版实现建议

第一版建议先支持：

- 场景级氛围关键词
- 静态分镜图提示词包
- 风格参考图集索引
- 视觉统一性检查
- 图像版本记录

---

## 9. 为什么这一步很适合 AI 辅助

因为 AI 很适合：

- 快速生成多版视觉方向
- 快速生成分镜草图提示词
- 快速做风格聚类与参考整理

但最终的视觉判断仍应由导演与核心部门负责人确认。

---

## 10. 这一篇最重要的结论

### 结论一
静态分镜图与氛围图，是跨部门统一视觉理解的关键媒介。

### 结论二
国内外差异的关键，在于视觉开发资料是否被系统化管理。

### 结论三
导演智能体平台应当把图像资料纳入对象、版本和审核体系。