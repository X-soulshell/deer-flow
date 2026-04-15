# 51. 项目复盘与知识沉淀

这一篇聚焦：

**为什么电影项目结束后最容易被忽略的，不是素材，而是经验。**

---

## 1. 复盘真正要解决什么问题

项目复盘不是简单总结“哪里做得好、哪里做得差”，而是要回答：

- 哪些决策有效
- 哪些问题反复出现
- 哪些流程造成返工
- 哪些资源配置最浪费
- 哪些经验可以复用到下一部片

行业复盘方法普遍强调：post-mortem 的关键不是列清单，而是分析为什么会发生，并把经验转成可复用流程。[来源：CG Wire, How To Perform a Post-mortem of Your Finished Production](https://blog.cg-wire.com/how-to-perform-a-post-mortem-of-your-finished-production/)

---

## 2. 一张复盘闭环图

```mermaid
flowchart LR
    A[项目完成] --> B[问题与成功点收集]
    B --> C[原因分析]
    C --> D[流程修订]
    D --> E[知识沉淀]
    E --> F[下个项目复用]
```

---

## 3. 海外成熟流程的特点

海外成熟工业体系通常更强调：

- 项目 post-mortem
- 部门级 lessons learned
- 版本与流程数据留存
- 可复用模板沉淀

这意味着复盘不是情绪表达，而是流程资产化。

---

## 4. 国内常见差异

国内项目中，常见情况是：

- 项目结束后团队快速解散
- 经验停留在核心成员脑中
- 复盘更多是口头交流
- 文档化与知识库沉淀不足

这意味着平台必须把“经验”从人脑中抽出来，变成结构化资产。

---

## 5. 一张时序图

```mermaid
sequenceDiagram
    participant Producer
    participant Director
    participant Departments
    participant PM as Project Memory

    Producer->>Departments: 收集项目问题与亮点
    Director->>Departments: 收集创作与执行反馈
    Departments->>PM: 提交 lessons learned
    PM->>Producer: 汇总流程问题
    PM->>Director: 汇总创作问题
    Producer->>PM: 确认可复用模板
```

---

## 6. 与 DeerFlow 的落地映射

可以设计：

- retrospective subagent
- knowledge-curator subagent
- `LessonLearned` / `RetrospectiveReport` / `ReusableTemplate` 对象
- 项目记忆 artifacts

---

## 7. 一张类图

```mermaid
classDiagram
    class LessonLearned {
      category
      issue
      root_cause
      recommendation
    }
    class RetrospectiveReport {
      project_id
      wins
      failures
      metrics
    }
    class ReusableTemplate {
      template_type
      source_project
      reuse_scope
    }

    RetrospectiveReport --> LessonLearned
    LessonLearned --> ReusableTemplate
```

---

## 8. 第一版实现建议

第一版建议先支持：

- 项目复盘摘要
- 问题分类
- 根因分析摘要
- 可复用模板清单
- 项目记忆归档

---

## 9. 为什么这一步对平台化最关键

因为没有知识沉淀，平台每个项目都要重新学一遍。

而真正的平台化，必须建立在“项目越做越聪明”的基础上。

---

## 10. 这一篇最重要的结论

### 结论一
项目复盘的核心不是总结，而是把经验转成可复用流程资产。

### 结论二
国内外差异的关键，在于复盘是否被制度化、文档化、知识库化。

### 结论三
导演智能体平台应当把复盘与知识沉淀建模成对象、模板和项目记忆系统。