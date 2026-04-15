# DeerFlow 多智能体时序图解

本文用几张由浅入深的时序图，说明 DeerFlow 中“用户 -> 主智能体 -> task 工具 -> 子智能体 -> 回传结果”的完整工作链路。

为了便于阅读，本文把流程拆成四层：

1. 总览图：先看整体协作关系
2. 主智能体决策图：看主智能体何时决定委派
3. 子智能体执行图：看 task 工具如何启动 subagent
4. 回传与收尾图：看结果如何回到主智能体并最终回复用户

---

## 1. 一张图看懂整体流程

```mermaid
sequenceDiagram
    autonumber
    actor User as 用户
    participant Lead as 主智能体 Lead Agent
    participant Task as task 工具
    participant Exec as SubagentExecutor
    participant Sub as 子智能体 Subagent
    participant Tools as 工具/沙箱

    User->>Lead: 提交复杂任务
    Lead->>Lead: 理解需求、规划步骤、判断是否需要委派
    alt 任务可直接完成
        Lead->>Tools: 直接调用普通工具
        Tools-->>Lead: 返回结果
        Lead-->>User: 直接给出最终答复
    else 任务适合拆分
        Lead->>Task: 调用 task(description, prompt, subagent_type)
        Task->>Exec: 创建子智能体执行器
        Exec->>Sub: 启动独立上下文中的子智能体
        Sub->>Tools: 使用工具/沙箱执行子任务
        Tools-->>Sub: 返回中间结果
        Sub-->>Exec: 产出阶段消息与最终结果
        Exec-->>Task: 更新状态
        Task-->>Lead: 返回子任务结果
        Lead->>Lead: 汇总子结果并继续推理
        Lead-->>User: 输出最终答复
    end
```

### 这张图想表达什么

最重要的是两点：

- 主智能体是调度者，不是所有事情都亲自做
- 子智能体不是普通函数，而是一个独立运行的小 agent

也就是说，DeerFlow 的多智能体不是“多个 agent 一起聊天”，而是“主智能体按需委派子任务给子智能体”。

---

## 2. 主智能体视角：什么时候决定调用 task 工具

```mermaid
sequenceDiagram
    autonumber
    actor User as 用户
    participant Lead as 主智能体
    participant MW as Middleware 链
    participant Model as 模型
    participant Tools as 普通工具
    participant Task as task 工具

    User->>Lead: 发起请求
    Lead->>MW: before_* 中间件处理线程、上传、沙箱等上下文
    MW-->>Lead: 注入 thread_data / sandbox / uploads 等状态
    Lead->>Model: 带 system prompt 与工具列表进行推理
    Model->>Model: 判断任务复杂度、上下文长度、是否适合隔离执行

    alt 简单任务
        Model->>Tools: 直接调用普通工具
        Tools-->>Model: 返回结果
        Model-->>Lead: 生成答复
    else 复杂任务 / 适合隔离上下文
        Model->>Task: 调用 task 工具进行委派
        Task-->>Lead: 进入子任务执行流程
    end

    Lead->>MW: after_* 中间件处理摘要、todo、标题、澄清等
    MW-->>Lead: 更新线程状态
    Lead-->>User: 返回当前阶段结果或最终结果
```

### 这一层的关键理解

主智能体并不是一收到请求就开子智能体，而是先经过自己的运行时链路：

- middleware 先准备环境
- 模型结合 prompt 和工具能力做判断
- 只有在“复杂、多步、适合隔离”的情况下，才会调用 `task`

所以 `task` 是一种“按需升级”的执行策略，不是默认路径。

---

## 3. task 工具视角：如何把委派变成真正的子智能体执行

```mermaid
sequenceDiagram
    autonumber
    participant Lead as 主智能体
    participant Task as task 工具
    participant Registry as Subagent Registry
    participant Toolset as get_available_tools
    participant Exec as SubagentExecutor
    participant BG as 后台任务池

    Lead->>Task: task(description, prompt, subagent_type, max_turns?)
    Task->>Registry: 获取 subagent 配置
    Registry-->>Task: 返回 SubagentConfig
    Task->>Task: 读取父上下文中的 sandbox、thread_data、thread_id、model、trace_id
    Task->>Toolset: 获取可用工具（禁用递归 subagent）
    Toolset-->>Task: 返回子智能体工具集
    Task->>Exec: 创建 SubagentExecutor
    Task->>BG: execute_async(prompt, task_id=tool_call_id)
    BG-->>Task: 返回后台任务 ID
    Task-->>Lead: 子任务已启动，进入轮询与流式回传
```

### 这一层的关键理解

`task` 工具不是自己完成任务，它更像一个“委派网关”。

它主要做五件事：

1. 选择子智能体类型
2. 继承父级必要上下文
3. 过滤子智能体可用工具
4. 创建执行器
5. 把执行放到后台线程/事件循环中

这里有一个很重要的设计：

**子智能体默认拿不到 `task` 工具，因此不会继续无限递归创建下一层子智能体。**

这让整个系统保持在“主智能体 -> 子智能体”的单层委派结构里，更可控。

---

## 4. 子智能体视角：在独立上下文中如何执行子任务

```mermaid
sequenceDiagram
    autonumber
    participant Task as task 工具
    participant Exec as SubagentExecutor
    participant Sub as 子智能体
    participant Tools as 工具集
    participant Sandbox as 沙箱/工作目录

    Task->>Exec: 提交 prompt 与父级上下文
    Exec->>Exec: 解析模型、工具白名单/黑名单、超时、最大轮数
    Exec->>Sub: create_agent(model, tools, middleware, system_prompt)
    Exec->>Exec: 构造初始 state，仅放入当前子任务消息
    Exec->>Sub: astream(state, config, context)

    loop 子智能体多轮推理
        Sub->>Sub: 思考下一步
        alt 需要工具
            Sub->>Tools: 调用工具
            Tools->>Sandbox: 读写文件 / 执行命令 / 搜索
            Sandbox-->>Tools: 返回结果
            Tools-->>Sub: 工具结果
        else 不需要工具
            Sub->>Sub: 继续内部推理
        end
    end

    Sub-->>Exec: 输出 AI 消息与最终结果
    Exec-->>Task: 更新任务状态
```

### 这一层的关键理解

子智能体虽然是被委派出来的，但它本身仍然是一个完整 agent：

- 有自己的模型
- 有自己的 system prompt
- 有自己的工具集
- 有自己的最大轮数
- 有自己的超时控制
- 有自己的执行状态

但它和主智能体最大的区别是：

**它不继承主对话的完整消息历史，只拿到当前子任务 prompt。**

这就是“上下文隔离”的核心。

---

## 5. 回传视角：子智能体的中间过程和最终结果如何返回

```mermaid
sequenceDiagram
    autonumber
    participant Sub as 子智能体
    participant Exec as SubagentExecutor
    participant Task as task 工具
    participant Lead as 主智能体
    actor User as 用户

    Sub-->>Exec: 产生新的 AI 消息
    Exec-->>Task: 写入 ai_messages / status

    loop 轮询后台状态
        Task->>Exec: 查询任务状态
        Exec-->>Task: RUNNING / COMPLETED / FAILED / TIMED_OUT
        alt 仍在运行
            Task-->>Lead: task_running 事件
            Lead-->>User: 前端可见中间进度
        else 已完成
            Task-->>Lead: task_completed + result
        else 失败或超时
            Task-->>Lead: task_failed / task_timed_out
        end
    end

    Lead->>Lead: 汇总子任务结果，决定是否继续推理或继续调用工具
    Lead-->>User: 输出最终答复
```

### 这一层的关键理解

这里不是“子智能体结束后一次性返回”，而是：

- 后台持续执行
- task 工具持续轮询
- 中间消息可以流式暴露
- 最终结果再交给主智能体整合

所以用户看到的体验通常是：

1. 主智能体发起一个子任务
2. 子任务在运行中
3. 中间过程不断出现
4. 最终结果被主智能体吸收并总结

---

## 6. 一张更完整的深度图：把状态、上下文和控制点都放进去

```mermaid
sequenceDiagram
    autonumber
    actor User as 用户
    participant Lead as Lead Agent
    participant MW as 主智能体 Middleware
    participant Task as task 工具
    participant Registry as Subagent 配置/注册表
    participant Exec as SubagentExecutor
    participant Sub as Subagent
    participant SubMW as 子智能体 Middleware
    participant Sandbox as 共享沙箱与线程目录
    participant Stream as 流式事件

    User->>Lead: 复杂请求
    Lead->>MW: 初始化 thread_data / sandbox / uploads / todo / memory 等
    MW-->>Lead: 返回增强后的线程状态
    Lead->>Lead: 判断是否需要委派

    alt 不委派
        Lead->>Sandbox: 直接使用普通工具完成任务
        Sandbox-->>Lead: 返回结果
        Lead-->>User: 最终答复
    else 委派给子智能体
        Lead->>Task: 调用 task(description, prompt, subagent_type)
        Task->>Registry: 读取子智能体配置
        Registry-->>Task: system_prompt / model / timeout / max_turns
        Task->>Task: 继承父级 sandbox、thread_data、thread_id、trace_id
        Task->>Exec: 创建执行器
        Exec->>Sub: 创建独立 agent
        Exec->>SubMW: 装配子智能体运行时中间件
        SubMW-->>Sub: 注入最小必要运行时能力
        Exec->>Sub: 以独立消息上下文启动子任务

        loop 子任务执行
            Sub->>Sandbox: 调用工具、读写文件、执行命令
            Sandbox-->>Sub: 返回工具结果
            Sub-->>Exec: 产生中间消息
            Exec-->>Task: 更新后台状态
            Task-->>Stream: 发出 task_running 事件
            Stream-->>User: 展示子任务进度
        end

        Sub-->>Exec: 最终结果
        Exec-->>Task: 标记 completed
        Task-->>Lead: 返回 Task Succeeded. Result: ...
        Lead->>Lead: 汇总子结果，继续主链路推理
        Lead->>MW: after_* 更新标题、记忆、摘要等
        MW-->>Lead: 写回线程状态
        Lead-->>User: 最终答复
    end
```

### 这张深度图强调了三件事

#### 1. 主智能体掌握全局状态
主智能体负责线程级状态管理，例如：

- thread_data
- sandbox
- todos
- title
- memory
- artifacts

#### 2. 子智能体掌握局部任务
子智能体只关心当前被委派的 prompt，不负责整条会话的全局治理。

#### 3. task 工具是桥梁
它连接了：

- 主智能体的委派意图
- 子智能体的实际执行
- 中间状态的流式回传
- 最终结果的回收

---

## 7. 用一句话概括整个时序

```text
用户提出复杂任务
-> 主智能体先在全局上下文中理解和规划
-> 若适合拆分，则调用 task 工具
-> task 工具创建并启动子智能体
-> 子智能体在独立上下文中使用工具完成子任务
-> task 工具轮询并流式回传中间状态
-> 最终结果回到主智能体
-> 主智能体整合后回复用户
```

---

## 8. 阅读这些图时最值得记住的几个结论

### 结论 1：这是“主从式”多智能体，不是对等协商式
主智能体负责调度，子智能体负责执行。

### 结论 2：子智能体是独立 agent，不是普通函数
它有自己的模型、prompt、工具和执行生命周期。

### 结论 3：上下文隔离是核心价值
子智能体不继承主对话全量历史，因此更聚焦、更省上下文。

### 结论 4：共享沙箱让协作变得可落地
虽然上下文隔离，但主子智能体仍可共享线程目录和沙箱环境，从而协同处理文件和命令执行。

### 结论 5：task 工具是整个多智能体机制的桥梁
没有 `task`，就没有从主智能体到子智能体的正式委派链路。

---

## 9. 对应代码位置

如果你想继续顺着代码读，建议按这个顺序：

1. `backend/packages/harness/deerflow/agents/lead_agent/agent.py`
2. `backend/packages/harness/deerflow/tools/builtins/task_tool.py`
3. `backend/packages/harness/deerflow/subagents/executor.py`
4. `backend/packages/harness/deerflow/subagents/registry.py`
5. `backend/packages/harness/deerflow/subagents/config.py`
6. `backend/packages/harness/deerflow/agents/thread_state.py`

---

## 10. 最后给你的阅读建议

如果你是从源码角度理解 DeerFlow，多智能体部分最容易混淆的点有两个：

- `task` 是工具，不是 agent 本身
- `SubagentExecutor` 才是真正把“委派”变成“独立 agent 执行”的地方

所以最好的理解顺序是：

**先看主智能体为什么决定委派，再看 task 如何搭桥，最后看 SubagentExecutor 如何真正跑起来。**
