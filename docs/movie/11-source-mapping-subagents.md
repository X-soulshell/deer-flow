# 11. 源码对照：子智能体、委派机制与电影部门角色如何落地

这一篇聚焦：

- `task` 工具如何工作
- `SubagentExecutor` 如何执行子任务
- `SubagentConfig` / registry 如何扩展成电影部门角色体系

---

## 1. `task` 工具：当前仓库里的委派入口

当前委派入口在：

- `backend/packages/harness/deerflow/tools/builtins/task_tool.py`

### 关键代码片段

```python
@tool("task", parse_docstring=True)
async def task_tool(..., description: str, prompt: str, subagent_type: str, ...):
    config = get_subagent_config(subagent_type)
    ...
    sandbox_state = runtime.state.get("sandbox")
    thread_data = runtime.state.get("thread_data")
    ...
    tools = get_available_tools(model_name=parent_model, subagent_enabled=False)
    executor = SubagentExecutor(...)
    task_id = executor.execute_async(prompt, task_id=tool_call_id)
```

### 这段代码说明什么

`task` 工具会做五件事：

1. 根据 `subagent_type` 读取子智能体配置
2. 继承父级的 `sandbox` / `thread_data` / `thread_id`
3. 获取子智能体可用工具
4. 创建 `SubagentExecutor`
5. 异步启动子任务

### 对导演智能体的意义

这正好对应电影制作中的：

- 导演发出部门任务
- 部门继承项目上下文
- 部门在自己的上下文里执行
- 结果再回到导演

---

## 2. 为什么 `subagent_enabled=False` 很重要

当前代码里有一行非常关键：

```python
tools = get_available_tools(model_name=parent_model, subagent_enabled=False)
```

### 这意味着什么

子智能体默认拿不到 `task` 工具，因此不会继续无限递归创建下一层子智能体。

### 对导演智能体的意义

这意味着当前架构天然更适合：

- 导演 -> 部门主管

而不是：

- 导演 -> 部门主管 -> 部门主管再无限拆分

对于电影制作来说，这其实是合理的第一阶段结构，因为它更可控。

---

## 3. `SubagentExecutor`：真正执行部门任务的地方

当前执行器在：

- `backend/packages/harness/deerflow/subagents/executor.py`

### 关键代码片段

```python
self.tools = _filter_tools(tools, config.tools, config.disallowed_tools)
...
return create_agent(
    model=model,
    tools=self.tools,
    middleware=middlewares,
    system_prompt=self.config.system_prompt,
    state_schema=ThreadState,
)
```

以及：

```python
state: dict[str, Any] = {
    "messages": [HumanMessage(content=task)],
}
if self.sandbox_state is not None:
    state["sandbox"] = self.sandbox_state
if self.thread_data is not None:
    state["thread_data"] = self.thread_data
```

### 这段代码说明什么

子智能体具备：

- 自己的模型
- 自己的 system prompt
- 自己的工具白名单/黑名单
- 自己的独立消息上下文
- 继承父级的工作区与线程数据

### 对导演智能体的意义

这非常适合映射成：

- 制片子智能体
- 分镜子智能体
- 摄影子智能体
- 后期子智能体

它们共享项目工作区，但不共享完整对话历史。

这正是电影部门协作最需要的“共享项目、隔离上下文”。

---

## 4. 子智能体状态机为什么重要

当前 `SubagentStatus` 包括：

- `PENDING`
- `RUNNING`
- `COMPLETED`
- `FAILED`
- `CANCELLED`
- `TIMED_OUT`

### 对导演智能体的意义

这意味着部门任务天然可以被纳入：

- 任务跟踪
- 超时控制
- 失败重试
- 审核前等待
- 取消与重排

这对于电影制作中的：

- 分镜任务
- 预算任务
- 排期任务
- 审核任务

都非常重要。

---

## 5. `SubagentConfig`：电影部门角色的天然配置模型

当前配置结构在：

- `backend/packages/harness/deerflow/subagents/config.py`

### 关键代码片段

```python
@dataclass
class SubagentConfig:
    name: str
    description: str
    system_prompt: str
    tools: list[str] | None = None
    disallowed_tools: list[str] | None = field(default_factory=lambda: ["task"])
    model: str = "inherit"
    max_turns: int = 50
    timeout_seconds: int = 900
```

### 这段代码说明什么

每个子智能体已经支持：

- 名称
- 职责描述
- 系统提示词
- 工具白名单
- 禁用工具
- 模型继承或覆盖
- 最大轮数
- 超时

### 对导演智能体的意义

这几乎就是电影部门角色配置的最小可用模型。

---

## 6. `registry.py`：电影角色注册表的天然入口

当前 registry 在：

- `backend/packages/harness/deerflow/subagents/registry.py`

### 关键代码片段

```python
config = BUILTIN_SUBAGENTS.get(name)
...
effective_timeout = app_config.get_timeout_for(name)
effective_max_turns = app_config.get_max_turns_for(name, config.max_turns)
...
return config
```

### 这段代码说明什么

当前 registry 已经支持：

- 通过名字查找子智能体
- 从配置覆盖 timeout / max_turns / model
- 暴露可用子智能体列表

### 对导演智能体的意义

这意味着你可以直接把电影部门角色注册进去，例如：

- `producer`
- `script-analyst`
- `budget-controller`
- `casting-director`
- `location-manager`
- `storyboard`
- `cinematography`
- `lighting`
- `vfx-supervisor`
- `editor`
- `sound-designer`
- `colorist`
- `marketing-strategist`

---

## 7. 设计稿：电影部门子智能体注册草案

建议第一批先做 6 个：

| 名称 | 职责 | 推荐工具 |
|------|------|----------|
| `producer` | 前期筹备、资源协调、风险识别 | 预算、排期、资源工具 |
| `script-analyst` | 剧本结构、角色、场景拆解 | 剧本解析工具 |
| `budget-controller` | 预算估算、版本比较、偏差分析 | 预算工具 |
| `storyboard` | 文字分镜、镜头表、提示词包 | 分镜工具 |
| `cinematography` | 摄影语言、镜头建议、机位建议 | 摄影工具 |
| `editor` | 后期版本规划、审核意见整理 | 后期工具 |

### 第二批再扩展
- `assistant-director`
- `casting-director`
- `location-manager`
- `lighting`
- `vfx-supervisor`
- `sound-designer`
- `colorist`
- `marketing-strategist`

---

## 8. 这一篇最重要的结论

### 结论一
`task` 工具已经是电影部门任务委派的天然入口。

### 结论二
`SubagentExecutor` 已经具备“共享项目上下文、隔离部门上下文”的关键能力。

### 结论三
`SubagentConfig` 和 `registry.py` 已经足以承接电影部门角色注册，只需要做领域化扩展。

---

---

<!-- movie-doc-nav:start -->
## 文档导航

- 总入口：[README.md](./README.md)
- 阅读地图：[00-reading-map.md](./00-reading-map.md)
- 所在分组：09-19 源码映射与实施草案
- 上一篇：[10. 源码对照：主智能体与运行时链如何承接导演智能体](./10-source-mapping-agent-runtime.md)
- 下一篇：[12. 源码对照：状态、配置与工厂扩展如何承接电影项目系统](./12-source-mapping-state-and-config.md)

### 同组文档
- [09. 源码对照总览：导演智能体方案如何映射到当前仓库](./09-source-mapping-overview.md)
- [10. 源码对照：主智能体与运行时链如何承接导演智能体](./10-source-mapping-agent-runtime.md)
- 11. 源码对照：子智能体、委派机制与电影部门角色如何落地（当前）
- [12. 源码对照：状态、配置与工厂扩展如何承接电影项目系统](./12-source-mapping-state-and-config.md)
- [13. 体系化设计稿：导演智能体平台系统蓝图](./13-system-blueprint.md)
- [14. 实施设计稿：从当前仓库出发的具体改造草案](./14-implementation-draft.md)
- [15A. 代码级设计草案：导演智能体第一批代码改造方案](./15-a-code-design-draft.md)
- [16B. 接口与数据结构草案：导演智能体的对象、状态与契约设计](./16-b-interfaces-and-data-contracts.md)
- [17C. 第一版代码落地方案：从文档走向最小可实现代码](./17-c-first-code-drop-plan.md)
- [18. 方案1细稿：可直接开发的 Markdown 细稿集合](./18-solution-1-detailed-md-drafts.md)
- [19. 方案2细稿：最小 MVP 代码实现路径与模块关系图](./19-solution-2-mvp-implementation-path.md)
<!-- movie-doc-nav:end -->
