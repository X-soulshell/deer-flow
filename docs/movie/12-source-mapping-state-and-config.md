# 12. 源码对照：状态、配置与工厂扩展如何承接电影项目系统

这一篇聚焦：

- `ThreadState` 为什么是电影项目状态的起点
- 自定义 agent 配置为什么适合承接导演人格与角色配置
- `create_deerflow_agent` 为什么适合做体系化扩展工厂

---

## 1. `ThreadState`：当前项目状态容器的起点

当前状态定义在：

- `backend/packages/harness/deerflow/agents/thread_state.py`

### 关键代码片段

```python
class ThreadState(AgentState):
    sandbox: NotRequired[SandboxState | None]
    thread_data: NotRequired[ThreadDataState | None]
    title: NotRequired[str | None]
    artifacts: Annotated[list[str], merge_artifacts]
    todos: NotRequired[list | None]
    uploaded_files: NotRequired[list[dict] | None]
    viewed_images: Annotated[dict[str, ViewedImageData], merge_viewed_images]
```

### 这段代码说明什么

当前状态已经支持：

- 工作区上下文
- 文件产物
- 任务清单
- 上传文件
- 图片理解结果

### 对导演智能体的意义

这说明当前系统已经有“项目上下文容器”，只是还没有电影制作语义。

也就是说，电影项目状态的第一步，不是新造一个状态系统，而是扩展 `ThreadState`。

---

## 2. 为什么 `thread_data` 和 `artifacts` 很关键

当前状态里有：

- `thread_data`
- `artifacts`

### 对导演智能体的意义

这两个字段天然适合承接：

- 项目工作目录
- 阶段产物目录
- 剧本拆解表
- 预算表
- 排期表
- 镜头表
- 审核记录
- 版本说明

所以电影制作系统的“资产层”并不是从零开始，而是可以直接建立在当前工作区与产物机制上。

---

## 3. 设计稿：ThreadState 扩展草案

建议第一阶段在状态中增加：

```text
project
phase
script_state
budget_state
schedule_state
casting_state
location_state
shot_plan_state
review_state
asset_versions
approvals
```

### 设计原则

- MVP 阶段先放在 ThreadState
- 中长期再迁移到独立项目存储层

### 为什么这样做

因为这样改造成本最低，而且最容易和当前运行时集成。

---

## 4. 自定义 agent 配置：导演人格与角色配置的入口

当前自定义 agent 配置在：

- `backend/packages/harness/deerflow/config/agents_config.py`

### 关键代码片段

```python
class AgentConfig(BaseModel):
    name: str
    description: str = ""
    model: str | None = None
    tool_groups: list[str] | None = None
    skills: list[str] | None = None
```

以及：

```python
def load_agent_soul(agent_name: str | None) -> str | None:
    ...
    soul_path = agent_dir / SOUL_FILENAME
    ...
```

### 这段代码说明什么

当前 agent 已经支持：

- 名称
- 描述
- 模型
- 工具组
- 技能组
- SOUL 人格文件

### 对导演智能体的意义

这意味着你可以直接定义：

- `director`
- `producer`
- `storyboard`
- `budget`

并为它们分别配置：

- 人格
- 职责
- 工具范围
- 技能范围
- 模型偏好

---

## 5. `setup_agent`：自定义 agent 创建机制的意义

当前 `setup_agent` 工具在：

- `backend/packages/harness/deerflow/tools/builtins/setup_agent_tool.py`

### 关键代码片段

```python
config_data: dict = {"name": agent_name}
if description:
    config_data["description"] = description
...
soul_file = agent_dir / "SOUL.md"
soul_file.write_text(soul, encoding="utf-8")
```

### 这段代码说明什么

当前系统已经支持通过工具创建 agent 配置与 SOUL 文件。

### 对导演智能体的意义

这意味着未来甚至可以让系统自己辅助创建：

- 导演 agent
- 制片 agent
- 某个项目专属 agent

虽然第一阶段未必需要自动创建，但这个扩展点非常有价值。

---

## 6. `create_deerflow_agent`：体系化设计稿的工厂入口

当前工厂在：

- `backend/packages/harness/deerflow/agents/factory.py`

### 关键代码片段

```python
def create_deerflow_agent(
    model,
    tools=None,
    *,
    system_prompt=None,
    middleware=None,
    features=None,
    extra_middleware=None,
    plan_mode=False,
    state_schema=None,
    checkpointer=None,
    name="default",
)
```

以及：

```python
effective_middleware, extra_tools = _assemble_from_features(...)
...
return create_agent(
    model=model,
    tools=effective_tools or None,
    middleware=effective_middleware,
    system_prompt=system_prompt,
    state_schema=effective_state,
    checkpointer=checkpointer,
    name=name,
)
```

### 这段代码说明什么

当前仓库已经有一个“纯参数工厂”，可以不依赖 YAML，直接从代码装配 agent。

### 对导演智能体的意义

这非常适合后续做：

- `create_director_agent(...)`
- `create_producer_agent(...)`
- `create_storyboard_agent(...)`
- `create_movie_project_agent(...)`

也就是说，体系化设计稿完全可以落到一个新的电影制作 agent factory 上。

---

## 7. 设计稿：电影制作工厂层草案

建议新增一层工厂封装，例如：

### `create_director_agent(...)`
负责：
- 选择导演默认模型
- 注入导演 skills
- 注入电影制作 tool groups
- 开启 plan mode
- 开启 subagent
- 使用扩展后的 MovieThreadState

### `create_department_agent(role, ...)`
负责：
- 根据角色选择 prompt
- 根据角色选择工具白名单
- 根据角色选择模型与超时

### `create_movie_project_runtime(...)`
负责：
- 绑定项目对象
- 绑定阶段状态
- 绑定审批流
- 绑定版本管理

---

## 8. 为什么状态、配置、工厂必须一起改

如果只改状态，不改配置：
- 角色无法清晰区分

如果只改配置，不改工厂：
- 体系化装配会很混乱

如果只改工厂，不改状态：
- 系统仍然没有项目语义

所以这三者必须一起设计。

---

## 9. 这一篇最重要的结论

### 结论一
`ThreadState` 是电影项目状态的起点。

### 结论二
`AgentConfig + SOUL` 是导演人格与角色配置的天然入口。

### 结论三
`create_deerflow_agent` 是体系化电影制作工厂的最佳扩展点。

---

---

<!-- movie-doc-nav:start -->
## 文档导航

- 总入口：[README.md](./README.md)
- 阅读地图：[00-reading-map.md](./00-reading-map.md)
- 所在分组：09-19 源码映射与实施草案
- 上一篇：[11. 源码对照：子智能体、委派机制与电影部门角色如何落地](./11-source-mapping-subagents.md)
- 下一篇：[13. 体系化设计稿：导演智能体平台系统蓝图](./13-system-blueprint.md)

### 同组文档
- [09. 源码对照总览：导演智能体方案如何映射到当前仓库](./09-source-mapping-overview.md)
- [10. 源码对照：主智能体与运行时链如何承接导演智能体](./10-source-mapping-agent-runtime.md)
- [11. 源码对照：子智能体、委派机制与电影部门角色如何落地](./11-source-mapping-subagents.md)
- 12. 源码对照：状态、配置与工厂扩展如何承接电影项目系统（当前）
- [13. 体系化设计稿：导演智能体平台系统蓝图](./13-system-blueprint.md)
- [14. 实施设计稿：从当前仓库出发的具体改造草案](./14-implementation-draft.md)
- [15A. 代码级设计草案：导演智能体第一批代码改造方案](./15-a-code-design-draft.md)
- [16B. 接口与数据结构草案：导演智能体的对象、状态与契约设计](./16-b-interfaces-and-data-contracts.md)
- [17C. 第一版代码落地方案：从文档走向最小可实现代码](./17-c-first-code-drop-plan.md)
- [18. 方案1细稿：可直接开发的 Markdown 细稿集合](./18-solution-1-detailed-md-drafts.md)
- [19. 方案2细稿：最小 MVP 代码实现路径与模块关系图](./19-solution-2-mvp-implementation-path.md)
<!-- movie-doc-nav:end -->
