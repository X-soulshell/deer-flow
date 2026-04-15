# 37. principal photography 现场组织

这一篇聚焦：

**为什么拍摄现场不是“导演喊开始”，而是一个高成本、高并发、高约束的执行系统。**

---

## 1. 现场组织的本质

进入 principal photography 后，项目进入成本最敏感阶段。

现场组织的核心目标是：

- 保证拍摄按计划推进
- 保证素材质量
- 保证安全与秩序
- 保证跨部门协同
- 保证问题快速升级与决策

---

## 2. 一张现场组织图

```mermaid
flowchart TD
    A[Call Sheet] --> B[现场到位]
    B --> C[部门准备]
    C --> D[导演/摄影确认]
    D --> E[拍摄执行]
    E --> F[素材检查]
    F --> G[收工复盘]
```

---

## 3. 海外成熟流程的特点

海外成熟工业体系通常更强调：

- call sheet 严格执行
- union / department boundary 清晰
- 安全与工时规则明确
- 现场问题升级路径清晰

这意味着现场组织更像一个标准化执行系统。

---

## 4. 国内常见差异

国内项目中，常见情况是：

- 现场灵活调整更多
- 导演与制片的即时协调更频繁
- 某些项目依赖核心团队经验维持秩序
- 当系统化工具不足时，信息同步成本更高

但大型项目和工业化项目中，现场组织也在快速标准化。

---

## 5. 一张泳道图

```mermaid
flowchart LR
    A[制片组] --> E[现场执行]
    B[导演组] --> E
    C[摄影灯光组] --> E
    D[美术服化道] --> E
    E --> F[素材与进度反馈]
```

---

## 6. 与 DeerFlow 的落地映射

可以设计：

- assistant-director subagent
- daily-operations subagent
- `CallSheet` / `DailyShootStatus` 对象
- 现场问题升级 artifacts
- dailies review artifacts

---

## 7. 一张状态机图

```mermaid
stateDiagram-v2
    [*] --> CrewCall
    CrewCall --> Setup
    Setup --> Blocking
    Blocking --> Shoot
    Shoot --> ReviewTake
    ReviewTake --> Shoot
    ReviewTake --> Wrap
    Wrap --> DailyReport
    DailyReport --> [*]
```

---

## 8. 第一版实现建议

第一版不需要做完整片场系统，但建议支持：

- 每日拍摄目标摘要
- 现场风险摘要
- 部门准备检查清单
- 收工复盘摘要
- 问题升级记录

---

## 9. 为什么这一步对平台化很关键

因为只有进入现场组织层，导演智能体平台才真正从“前期策划工具”升级为“制作操作系统”。

---

## 10. 这一篇最重要的结论

### 结论一
拍摄现场是一个高成本、高并发、高约束的执行系统。

### 结论二
国内外差异的关键，在于现场标准化程度与信息同步机制。

### 结论三
导演智能体平台必须逐步进入现场组织层，才能真正支撑大规模制作。