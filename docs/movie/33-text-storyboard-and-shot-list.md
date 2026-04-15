# 33. 文字分镜与镜头表

这一篇聚焦：

**为什么文字分镜与镜头表，是导演创作语言进入生产系统的关键桥梁。**

---

## 1. 文字分镜与镜头表的区别

- 文字分镜更偏导演意图与镜头叙述
- 镜头表更偏执行清单与拍摄组织

两者不是二选一，而是前后衔接关系。

---

## 2. 一张关系图

```mermaid
flowchart TD
    A[剧本场景] --> B[文字分镜]
    B --> C[镜头表]
    C --> D[拍摄执行]
    D --> E[后期素材组织]
```

---

## 3. 文字分镜在真实项目中的作用

文字分镜通常回答：

- 这一场戏怎么拍
- 情绪如何推进
- 视角如何切换
- 角色关系如何被镜头表达
- 节奏如何变化

它更接近导演语言。

---

## 4. 镜头表在真实项目中的作用

镜头表通常回答：

- 拍哪些镜头
- 每个镜头的景别、机位、运动方式
- 哪些演员在场
- 哪些道具、灯光、特效需要准备
- 哪些镜头优先拍

它更接近执行语言。

---

## 5. 一张时序图

```mermaid
sequenceDiagram
    participant Director
    participant StoryboardAgent
    participant DP
    participant AD
    participant Producer

    Director->>StoryboardAgent: 提供场景目标与情绪
    StoryboardAgent->>DP: 输出文字分镜
    DP->>StoryboardAgent: 反馈镜头可行性
    StoryboardAgent->>AD: 生成镜头表草案
    AD->>Producer: 反馈排期与资源影响
    Producer->>Director: 汇总执行约束
    Director->>StoryboardAgent: 确认最终镜头方案
```

---

## 6. 国内外差异

海外成熟流程中，文字分镜、shot list、previs 往往衔接更紧密，复杂镜头会更早进入技术预演。

国内项目中，常见情况是：

- 文字分镜更依赖导演个人表达
- 镜头表可能在更靠近拍摄时才稳定
- 复杂镜头的技术拆解有时滞后

这意味着平台要支持“早规划”和“晚收敛”两种模式。

---

## 7. 与 DeerFlow 的落地映射

可以设计：

- storyboard subagent
- cinematography subagent
- `ShotPlan` 对象
- `StoryboardPromptPack` artifacts
- 镜头复杂度与优先级字段

---

## 8. 一张类图

```mermaid
classDiagram
    class StoryboardText {
      scene_id
      emotional_goal
      visual_description
      rhythm_notes
    }
    class ShotPlan {
      shot_id
      scene_id
      shot_type
      movement
      cast
      props
      priority
    }
    class StoryboardPromptPack {
      scene_id
      prompts
      style_keywords
    }

    StoryboardText --> ShotPlan
    ShotPlan --> StoryboardPromptPack
```

---

## 9. 第一版实现建议

第一版建议先支持：

- 场景级文字分镜生成
- 镜头表草案生成
- 镜头复杂度标记
- 分镜提示词包输出
- 导演确认点标记

---

## 10. 这一篇最重要的结论

### 结论一
文字分镜与镜头表，是导演语言进入生产系统的关键桥梁。

### 结论二
国内外差异的关键，在于镜头规划前置程度与技术预演深度。

### 结论三
在导演智能体平台中，分镜模块应当同时服务创作表达与执行组织。