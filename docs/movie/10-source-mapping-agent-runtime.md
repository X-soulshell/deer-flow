# 10. 源码对照：主智能体与运行时链如何承接导演智能体

这一篇聚焦两个问题：

1. 当前主智能体是怎么创建的
2. 当前运行时链如何承接导演智能体的阶段控制与治理

---

## 1. 主智能体入口：`make_lead_agent`

当前主智能体入口在：

- `backend/packages/harness/deerflow/agents/lead_agent/agent.py`

核心函数是 `make_lead_agent(config)`。

### 关键代码片段

```python
cfg = config.get("configurable", {})

thinking_enabled = cfg.get("thinking_enabled", True)
requested_model_name: str | None = cfg.get("model_name") or cfg.get("model")
is_plan_mode = cfg.get("is_plan_mode", False)
subagent_enabled = cfg.get("subagent_enabled", False)
max_concurrent_subagents = cfg.get("max_concurrent_subagents", 3)
agent_name = cfg.get("agent_name")

agent_config = load_agent_config(agent_name) if not is_bootstrap else None
...
return create_agent(
    model=create_chat_model(...),
    tools=get_available_tools(...),
    middleware=_build_middlewares(...),
    system_prompt=apply_prompt_template(...),
    state_schema=ThreadState,
)
```

### 这段代码说明什么

这说明当前主智能体已经支持：

- 按 `agent_name` 加载不同 agent 配置
- 按 `model_name` 切换模型
- 按 `is_plan_mode` 开启计划模式
- 按 `subagent_enabled` 开启子智能体委派
- 按 `max_concurrent_subagents` 控制并发

### 对导演智能体的意义

这几乎就是导演智能体最需要的控制面：

- `agent_name=director`
- `is_plan_mode=true` 用于阶段任务清单
- `subagent_enabled=true` 用于部门委派
- `max_concurrent_subagents` 用于控制部门并发

也就是说，**总导演智能体的主入口已经存在。**

---

## 2. Middleware 链：导演智能体的运行时治理骨架

当前 `_build_middlewares(...)` 会组装主 agent 的运行时链。

### 关键代码片段

```python
middlewares = build_lead_runtime_middlewares(lazy_init=True)

summarization_middleware = _create_summarization_middleware()
if summarization_middleware is not None:
    middlewares.append(summarization_middleware)

is_plan_mode = config.get("configurable", {}).get("is_plan_mode", False)
todo_list_middleware = _create_todo_list_middleware(is_plan_mode)
if todo_list_middleware is not None:
    middlewares.append(todo_list_middleware)

middlewares.append(TitleMiddleware())
middlewares.append(MemoryMiddleware(agent_name=agent_name))
...
if subagent_enabled:
    middlewares.append(SubagentLimitMiddleware(max_concurrent=max_concurrent_subagents))
...
middlewares.append(ClarificationMiddleware())
```

### 这段代码说明什么

当前主 agent 已经具备：

- 计划任务管理
- 记忆写回
- 子智能体并发限制
- 澄清拦截
- 上下文压缩
- 标题生成

### 对导演智能体的意义

这条链可以直接承接：

- 阶段任务清单
- 项目记忆
- 部门并发控制
- 长上下文压缩
- 关键澄清节点

换句话说，导演智能体的“治理层”已经有了，只是语义还不是电影制作。

---

## 3. Middleware 执行顺序为什么重要

当前文档已经明确了 middleware 执行顺序。

### 关键文档位置

- `backend/docs/middleware-execution-flow.md`

其中主 agent 的链路包括：

- ThreadDataMiddleware
- UploadsMiddleware
- SandboxMiddleware
- SummarizationMiddleware
- TodoMiddleware
- TitleMiddleware
- MemoryMiddleware
- ViewImageMiddleware
- SubagentLimitMiddleware
- LoopDetectionMiddleware
- ClarificationMiddleware

### 对导演智能体的意义

这意味着你可以把电影制作的阶段控制嵌入到现有顺序里，而不是另起一套运行时。

例如：

- 在 Todo 层承接阶段任务
- 在 Memory 层承接项目记忆
- 在 Sandbox 层承接项目资产目录
- 在 Clarification 层承接关键审批前澄清

---

## 4. 为什么 `is_plan_mode` 对导演智能体特别重要

电影制作不是一次性回答，而是多阶段、多任务推进。

当前 `is_plan_mode` 会启用 TodoMiddleware，这意味着主智能体可以：

- 拆解复杂任务
- 跟踪任务状态
- 显示阶段进度
- 在执行中动态修订计划

### 对导演智能体的意义

这非常适合承接：

- 前期筹备清单
- 中期拍摄任务清单
- 后期交付清单

所以导演智能体不应该把 Todo 视为附属功能，而应该把它视为阶段工作流的基础执行层。

---

## 5. 为什么 `MemoryMiddleware` 是项目记忆的入口

当前主 agent 会挂：

```python
middlewares.append(MemoryMiddleware(agent_name=agent_name))
```

这说明记忆已经支持按 agent 维度隔离。

### 对导演智能体的意义

后续可以把 `director` 的记忆升级成：

- 导演风格偏好
- 当前项目阶段
- 当前剧本版本
- 当前预算版本
- 当前审核结论

也就是说，当前代码已经有“项目记忆插槽”。

---

## 6. 为什么 `SubagentLimitMiddleware` 很关键

电影制作是多部门协作，但不能无限并发。

当前代码里：

```python
if subagent_enabled:
    max_concurrent_subagents = config.get("configurable", {}).get("max_concurrent_subagents", 3)
    middlewares.append(SubagentLimitMiddleware(max_concurrent=max_concurrent_subagents))
```

### 对导演智能体的意义

这意味着系统已经支持：

- 控制同时启动多少部门子智能体
- 避免上下文和成本失控
- 保持导演主控节奏

对于电影制作这种高成本协作场景，这是非常重要的治理点。

---

## 7. 设计稿：导演主智能体运行时草案

建议把导演主智能体定义成：

### Director Lead Agent
- `agent_name`: `director`
- `subagent_enabled`: `true`
- `is_plan_mode`: `true`
- `max_concurrent_subagents`: 3 或 4
- `skills`: 电影制作方法论 skills
- `tool_groups`: 电影制作工具组

### 运行时职责
- 统一创作目标
- 维护阶段任务清单
- 维护项目记忆
- 决定是否委派给部门子智能体
- 汇总部门结果并做最终判断

---

## 8. 这一篇最重要的结论

### 结论一
`make_lead_agent` 已经是导演主智能体的天然入口。

### 结论二
当前 middleware 链已经具备导演智能体所需的大部分治理能力。

### 结论三
导演智能体的第一步，不是重写运行时，而是把当前运行时语义升级成电影制作语义。

---

---

<!-- movie-doc-nav:start -->
## 文档导航

- 总入口：[README.md](./README.md)
- 阅读地图：[00-reading-map.md](./00-reading-map.md)
- 所在分组：09-19 源码映射与实施草案
- 上一篇：[09. 源码对照总览：导演智能体方案如何映射到当前仓库](./09-source-mapping-overview.md)
- 下一篇：[11. 源码对照：子智能体、委派机制与电影部门角色如何落地](./11-source-mapping-subagents.md)

### 同组文档
- [09. 源码对照总览：导演智能体方案如何映射到当前仓库](./09-source-mapping-overview.md)
- 10. 源码对照：主智能体与运行时链如何承接导演智能体（当前）
- [11. 源码对照：子智能体、委派机制与电影部门角色如何落地](./11-source-mapping-subagents.md)
- [12. 源码对照：状态、配置与工厂扩展如何承接电影项目系统](./12-source-mapping-state-and-config.md)
- [13. 体系化设计稿：导演智能体平台系统蓝图](./13-system-blueprint.md)
- [14. 实施设计稿：从当前仓库出发的具体改造草案](./14-implementation-draft.md)
- [15A. 代码级设计草案：导演智能体第一批代码改造方案](./15-a-code-design-draft.md)
- [16B. 接口与数据结构草案：导演智能体的对象、状态与契约设计](./16-b-interfaces-and-data-contracts.md)
- [17C. 第一版代码落地方案：从文档走向最小可实现代码](./17-c-first-code-drop-plan.md)
- [18. 方案1细稿：可直接开发的 Markdown 细稿集合](./18-solution-1-detailed-md-drafts.md)
- [19. 方案2细稿：最小 MVP 代码实现路径与模块关系图](./19-solution-2-mvp-implementation-path.md)
<!-- movie-doc-nav:end -->
