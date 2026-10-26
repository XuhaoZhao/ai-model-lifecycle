# 企业级 Agent 平台：身份体系与委托授权设计（ChatGPT 对话存档）

> **来源**：ChatGPT 分享链接
> **链接**：<https://chatgpt.com/share/6adfd319-aab4-83ec-8fab-7a72ae3eca08>（最新，含第 29 轮）
> **历史版本**：<https://chatgpt.com/share/6aded0bc-51ac-83ec-bb9b-90a6c2b3aaa1>
> **页面标题**：仓库用途介绍
> **存档说明**：2026-10-27 经 Playwright 从分享页 hydration 数据完整提取。页面渲染仅显示部分消息，本档包含全部 **30 轮 / 61 条消息**（约 19.4 万字符；2026-10-27 第二次同步追加第 29 轮）；连续的助手流式分片已按轮合并。文中 `<Entity …>` / `<box …>` 为 ChatGPT 富文本组件标记，保留原样。

---


---

## 👤 用户

这个仓库是干嘛的 [https://github.com/agentgateway/agentgateway](https://github.com/agentgateway/agentgateway)


---

## 🤖 ChatGPT

这个仓库 [<Entity value="agentgateway" category="software" disambig="开源 AI Gateway 与 Agent 通信代理项目"/>](https://github.com/agentgateway/agentgateway)，简单说，就是一个面向 AI Agent 的**统一通信网关（Agent Gateway）**。

它不是用来开发 Agent 的框架，也不是负责运行 Agent 的执行引擎，而是部署在 Agent、LLM、MCP 工具服务等组件之间，统一处理通信、安全策略、流量治理和可观测性。<Cite refs={["turn243987search0","turn243987search4"]}/>

结合你正在研究的企业级 Agent 平台、MCP 鉴权和 AI Security Gateway，这个项目很值得关注：**它试图把传统 API Gateway 的能力扩展到 Agent 之间、Agent 与工具之间，以及 Agent 与大模型之间的通信。**

## 一、它究竟解决什么问题？

<box gap={1} align="center">
  <box border radius="lg" padding={3} width="100%" align="center" gap={1}>
    <icon name="users" size="xl"/>
    **调用方**
    <text color="secondary" size="sm">业务 Agent、AI 应用、其他 Agent</text>
  </box>
  <box align="center" gap={1}>
    <icon name="arrow-down-up" size="xl" color="secondary"/>
  </box>
  <box border radius="xl" padding={4} width="100%" gap={3} background="surface-secondary">
    <title size="lg" textAlign="center">agentgateway</title>
    <grid columns={2} gap={2}>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="shield-check" size="lg"/>
          **安全控制**
          <text size="xs" color="secondary">认证、授权、策略</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="route" size="lg"/>
          **流量治理**
          <text size="xs" color="secondary">路由、限流、故障转移</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="activity" size="lg"/>
          **可观测性**
          <text size="xs" color="secondary">日志、指标、追踪</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="filter" size="lg"/>
          **内容防护**
          <text size="xs" color="secondary">Guardrails、过滤策略</text>
        </box>
      </grid-item>
    </grid>
    <text color="secondary" textAlign="center" size="sm">统一代理与策略执行层</text>
  </box>
  <box align="center">
    <icon name="arrow-down-up" size="xl" color="secondary"/>
  </box>
  <grid columns={3} gap={2}>
    <grid-item>
      <box border radius="lg" padding={2} align="center" gap={1}>
        <icon name="brain-circuit" size="xl"/>
        **LLM**
        <text size="xs" color="secondary">云端或自部署模型</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="lg" padding={2} align="center" gap={1}>
        <icon name="wrench" size="xl"/>
        **MCP**
        <text size="xs" color="secondary">数据库、服务器工具</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="lg" padding={2} align="center" gap={1}>
        <icon name="network" size="xl"/>
        **A2A**
        <text size="xs" color="secondary">其他 Agent</text>
      </box>
    </grid-item>
  </grid>
</box>

以上是它的核心架构思路。项目还支持 Kubernetes 环境下的流量管理，以及不依赖完整 Kubernetes 控制器的独立部署方式。<Cite refs={["turn243987search0","turn243987search4"]}/>

举个实际例子：你的运维 Agent 需要调用公司内部的数据库 MCP、服务器 MCP，同时还需要调用外部大模型。

没有统一网关时，每个 Agent 都可能需要自己处理模型地址、鉴权、调用日志、限流等问题。

引入 agentgateway 后，可以让这些通信经过统一代理，由网关集中执行相应的策略。

这个仓库 [<Entity value="agentgateway" category="software" disambig="开源 AI Gateway 与 Agent 通信代理项目"/>](https://github.com/agentgateway/agentgateway)，简单说，就是一个面向 AI Agent 的**统一通信网关（Agent Gateway）**。

它不是用来开发 Agent 的框架，也不是负责运行 Agent 的执行引擎，而是部署在 Agent、LLM、MCP 工具服务等组件之间，统一处理通信、安全策略、流量治理和可观测性。<Cite refs={["turn243987search0","turn243987search4"]}/>

结合你正在研究的企业级 Agent 平台、MCP 鉴权和 AI Security Gateway，这个项目很值得关注：**它试图把传统 API Gateway 的能力扩展到 Agent 之间、Agent 与工具之间，以及 Agent 与大模型之间的通信。**

## 一、它究竟解决什么问题？

<box gap={1} align="center">
  <box border radius="lg" padding={3} width="100%" align="center" gap={1}>
    <icon name="users" size="xl"/>
    **调用方**
    <text color="secondary" size="sm">业务 Agent、AI 应用、其他 Agent</text>
  </box>
  <box align="center" gap={1}>
    <icon name="arrow-down-up" size="xl" color="secondary"/>
  </box>
  <box border radius="xl" padding={4} width="100%" gap={3} background="surface-secondary">
    <title size="lg" textAlign="center">agentgateway</title>
    <grid columns={2} gap={2}>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="shield-check" size="lg"/>
          **安全控制**
          <text size="xs" color="secondary">认证、授权、策略</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="route" size="lg"/>
          **流量治理**
          <text size="xs" color="secondary">路由、限流、故障转移</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="activity" size="lg"/>
          **可观测性**
          <text size="xs" color="secondary">日志、指标、追踪</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="filter" size="lg"/>
          **内容防护**
          <text size="xs" color="secondary">Guardrails、过滤策略</text>
        </box>
      </grid-item>
    </grid>
    <text color="secondary" textAlign="center" size="sm">统一代理与策略执行层</text>
  </box>
  <box align="center">
    <icon name="arrow-down-up" size="xl" color="secondary"/>
  </box>
  <grid columns={3} gap={2}>
    <grid-item>
      <box border radius="lg" padding={2} align="center" gap={1}>
        <icon name="brain-circuit" size="xl"/>
        **LLM**
        <text size="xs" color="secondary">云端或自部署模型</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="lg" padding={2} align="center" gap={1}>
        <icon name="wrench" size="xl"/>
        **MCP**
        <text size="xs" color="secondary">数据库、服务器工具</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="lg" padding={2} align="center" gap={1}>
        <icon name="network" size="xl"/>
        **A2A**
        <text size="xs" color="secondary">其他 Agent</text>
      </box>
    </grid-item>
  </grid>
</box>

以上是它的核心架构思路。项目还支持 Kubernetes 环境下的流量管理，以及不依赖完整 Kubernetes 控制器的独立部署方式。<Cite refs={["turn243987search0","turn243987search4"]}/>

举个实际例子：你的运维 Agent 需要调用公司内部的数据库 MCP、服务器 MCP，同时还需要调用外部大模型。

没有统一网关时，每个 Agent 都可能需要自己处理模型地址、鉴权、调用日志、限流等问题。

引入 agentgateway 后，可以让这些通信经过统一代理，由网关集中执行相应的策略。

## 二、它有哪些核心能力？

<box gap={3}>
  <row align="start" gap={3}>
    <AsyncImage query="AI LLM gateway architecture routing multiple model providers diagram" aspectRatio="4:3" maxWidth="112px"/>
    <box flex="1" gap={1}>
      **1. LLM Gateway：大模型统一接入**

      将不同模型供应商统一到兼容 OpenAI 的 API 入口，并支持路由、负载均衡、故障转移、预算和调用成本控制。

      <text color="secondary" size="sm">适用：企业同时使用外部模型 API 和内部自部署模型。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <AsyncImage query="MCP model context protocol client server tools architecture diagram" aspectRatio="4:3" maxWidth="112px"/>
    <box flex="1" gap={1}>
      **2. MCP Gateway：工具统一接入**

      连接和聚合多个 MCP 服务，支持不同传输方式、工具发现、OAuth 认证，以及 OpenAPI 接口集成。

      <text color="secondary" size="sm">适用：企业有大量内部 MCP 服务，需要统一接入和治理。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <AsyncImage query="AI agent to agent A2A communication architecture diagram" aspectRatio="4:3" maxWidth="112px"/>
    <box flex="1" gap={1}>
      **3. A2A Gateway：Agent 间通信**

      面向 Agent-to-Agent 协作，处理 Agent 之间的通信、能力发现及任务协作相关流量。

      <text color="secondary" size="sm">适用：企业中存在多个独立 Agent，需要跨系统协作。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <AsyncImage query="AI security gateway authorization policy enforcement observability architecture" aspectRatio="4:3" maxWidth="112px"/>
    <box flex="1" gap={1}>
      **4. Security & Observability：安全与审计基础设施**

      支持 JWT、API Key、OAuth、基于 CEL 的细粒度授权策略、限流、TLS，以及 OpenTelemetry 指标、日志和链路追踪。还支持接入多种内容防护机制。

      <text color="secondary" size="sm">适用：需要统一控制 AI 流量，并对调用过程进行监控和审计的企业。</text>
    </box>
  </row>
</box>

以上能力来自项目 README 和官方文档。需要注意，具体能力是否可用、如何配置，会因部署方式和功能集成情况而有所不同。<Cite refs={["turn243987search0","turn243987search4"]}/>

## 三、它和传统 API Gateway 有什么区别？

| 对比维度 | 传统 API Gateway | agentgateway |
|---|---|---|
| HTTP API 代理 | 核心能力 | 支持 |
| 模型调用路由 | 通常需要额外扩展 | 原生面向 LLM 场景 |
| MCP 协议与工具治理 | 通常需要额外适配 | 核心能力之一 |
| A2A 通信 | 通常需要额外适配 | 原生支持相关协议 |
| AI 内容防护 | 依赖扩展或外部系统 | 提供 Guardrails 集成能力 |
| AI 专属指标与策略 | 需要定制 | 面向 Agent 场景设计 |

它的价值不只是给 API 换一个入口，而是理解 MCP、A2A 等 Agent 通信协议，并将协议相关的路由、安全和治理能力放到代理层。<Cite refs={["turn243987search0","turn243987search4"]}/>

## 四、对你的企业 Agent 平台有什么价值？

我建议把它放到你正在设计的企业 Agent 平台架构里，作为**候选通信网关和策略执行组件**，而不是直接将它当作整个 Agent 平台。

<box border radius="lg" padding={3} gap={3}>
  <box background="surface-secondary" radius="md" padding={3} align="center" gap={1}>
    **业务 Agent Runtime**
    <text color="secondary" size="sm">Agent 执行、任务编排、上下文、工具调用决策</text>
  </box>
  <box align="center">
    <icon name="arrow-down-up" size="xl" color="secondary"/>
  </box>
  <box border radius="md" padding={3} gap={1} align="center">
    **agentgateway**
    <text color="secondary" size="sm">模型与工具通信代理、流量策略、认证授权、可观测性</text>
  </box>
  <grid columns={2} gap={2}>
    <grid-item>
      <box border radius="md" padding={3} gap={1}>
        **LLM 服务**
        <text color="secondary" size="sm">外部 API、内部 DeepSeek 等</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="md" padding={3} gap={1}>
        **MCP 服务**
        <text color="secondary" size="sm">代码仓库、数据库、服务器</text>
      </box>
    </grid-item>
  </grid>
  <box border radius="md" padding={3} gap={1}>
    **独立安全控制层**
    <text color="secondary" size="sm">身份与权限源、审批、敏感数据规则、沙箱隔离、安全审计留存</text>
  </box>
</box>

这里有几个关键判断：

- **MCP 鉴权：** 可以评估让网关统一处理 MCP 入口认证和授权，但需要验证它是否能满足你的用户身份透传、工具级权限及下游凭据管理要求。
- **AI Security Gateway：** 它可以作为重要的技术底座，但接入 Guardrails 不等于已经建立完整的 AI 安全体系。提示词注入检测、敏感信息识别、数据出境控制等，还需要逐项验证和补齐。
- **沙箱隔离：** 它不是 gVisor 或 Agent Sandbox 的替代品。代理层负责通信和策略，沙箱负责限制不可信代码的执行环境。
- **审计：** 有调用日志和链路追踪，不代表自动满足企业审计要求。还要检查用户身份、工具参数、授权决策、执行结果以及日志防篡改和留存机制。

## 五、接下来应该重点研究什么？

官方仓库：[agentgateway](https://github.com/agentgateway/agentgateway)

官方文档：[About & Architecture](https://agentgateway.dev/docs/standalone/latest/documentation/about/)

结合你的场景，我会优先研究这三个问题：

1. **MCP Gateway 的授权模型：** 它能否实现不同业务用户只能调用有权限的 MCP 工具，以及同一个工具的不同资源权限隔离？
2. **策略执行机制：** CEL 策略具体能获取哪些身份、请求和工具上下文？策略是在什么位置执行的？
3. **独立部署与集成：** 能否在不引入 Kubernetes 的情况下部署，并接入现有 Java 后端、身份体系和内部 MCP 服务？

尤其是第二点。企业级 Agent 平台最容易出现的设计缺陷，是网关虽然验证了调用方身份，却无法可靠地把最终用户的身份和权限约束贯穿到具体工具操作中。

**我的判断：agentgateway 值得作为你们企业 AI Gateway 的重点候选进行验证。** 但如果你的目标是建设通用 Agent Runtime，Agent 的执行循环、会话状态、任务调度、沙箱和知识积累仍然需要由其他组件承担。

## 二、它有哪些核心能力？

<box gap={3}>
  <row align="start" gap={3}>
    <AsyncImage query="AI LLM gateway architecture routing multiple model providers diagram" aspectRatio="4:3" maxWidth="112px"/>
    <box flex="1" gap={1}>
      **1. LLM Gateway：大模型统一接入**

      将不同模型供应商统一到兼容 OpenAI 的 API 入口，并支持路由、负载均衡、故障转移、预算和调用成本控制。

      <text color="secondary" size="sm">适用：企业同时使用外部模型 API 和内部自部署模型。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <AsyncImage query="MCP model context protocol client server tools architecture diagram" aspectRatio="4:3" maxWidth="112px"/>
    <box flex="1" gap={1}>
      **2. MCP Gateway：工具统一接入**

      连接和聚合多个 MCP 服务，支持不同传输方式、工具发现、OAuth 认证，以及 OpenAPI 接口集成。

      <text color="secondary" size="sm">适用：企业有大量内部 MCP 服务，需要统一接入和治理。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <AsyncImage query="AI agent to agent A2A communication architecture diagram" aspectRatio="4:3" maxWidth="112px"/>
    <box flex="1" gap={1}>
      **3. A2A Gateway：Agent 间通信**

      面向 Agent-to-Agent 协作，处理 Agent 之间的通信、能力发现及任务协作相关流量。

      <text color="secondary" size="sm">适用：企业中存在多个独立 Agent，需要跨系统协作。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <AsyncImage query="AI security gateway authorization policy enforcement observability architecture" aspectRatio="4:3" maxWidth="112px"/>
    <box flex="1" gap={1}>
      **4. Security & Observability：安全与审计基础设施**

      支持 JWT、API Key、OAuth、基于 CEL 的细粒度授权策略、限流、TLS，以及 OpenTelemetry 指标、日志和链路追踪。还支持接入多种内容防护机制。

      <text color="secondary" size="sm">适用：需要统一控制 AI 流量，并对调用过程进行监控和审计的企业。</text>
    </box>
  </row>
</box>

以上能力来自项目 README 和官方文档。需要注意，具体能力是否可用、如何配置，会因部署方式和功能集成情况而有所不同。<Cite refs={["turn243987search0","turn243987search4"]}/>

## 三、它和传统 API Gateway 有什么区别？

| 对比维度 | 传统 API Gateway | agentgateway |
|---|---|---|
| HTTP API 代理 | 核心能力 | 支持 |
| 模型调用路由 | 通常需要额外扩展 | 原生面向 LLM 场景 |
| MCP 协议与工具治理 | 通常需要额外适配 | 核心能力之一 |
| A2A 通信 | 通常需要额外适配 | 原生支持相关协议 |
| AI 内容防护 | 依赖扩展或外部系统 | 提供 Guardrails 集成能力 |
| AI 专属指标与策略 | 需要定制 | 面向 Agent 场景设计 |

它的价值不只是给 API 换一个入口，而是理解 MCP、A2A 等 Agent 通信协议，并将协议相关的路由、安全和治理能力放到代理层。<Cite refs={["turn243987search0","turn243987search4"]}/>

## 四、对你的企业 Agent 平台有什么价值？

我建议把它放到你正在设计的企业 Agent 平台架构里，作为**候选通信网关和策略执行组件**，而不是直接将它当作整个 Agent 平台。

<box border radius="lg" padding={3} gap={3}>
  <box background="surface-secondary" radius="md" padding={3} align="center" gap={1}>
    **业务 Agent Runtime**
    <text color="secondary" size="sm">Agent 执行、任务编排、上下文、工具调用决策</text>
  </box>
  <box align="center">
    <icon name="arrow-down-up" size="xl" color="secondary"/>
  </box>
  <box border radius="md" padding={3} gap={1} align="center">
    **agentgateway**
    <text color="secondary" size="sm">模型与工具通信代理、流量策略、认证授权、可观测性</text>
  </box>
  <grid columns={2} gap={2}>
    <grid-item>
      <box border radius="md" padding={3} gap={1}>
        **LLM 服务**
        <text color="secondary" size="sm">外部 API、内部 DeepSeek 等</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="md" padding={3} gap={1}>
        **MCP 服务**
        <text color="secondary" size="sm">代码仓库、数据库、服务器</text>
      </box>
    </grid-item>
  </grid>
  <box border radius="md" padding={3} gap={1}>
    **独立安全控制层**
    <text color="secondary" size="sm">身份与权限源、审批、敏感数据规则、沙箱隔离、安全审计留存</text>
  </box>
</box>

这里有几个关键判断：

- **MCP 鉴权：** 可以评估让网关统一处理 MCP 入口认证和授权，但需要验证它是否能满足你的用户身份透传、工具级权限及下游凭据管理要求。
- **AI Security Gateway：** 它可以作为重要的技术底座，但接入 Guardrails 不等于已经建立完整的 AI 安全体系。提示词注入检测、敏感信息识别、数据出境控制等，还需要逐项验证和补齐。
- **沙箱隔离：** 它不是 gVisor 或 Agent Sandbox 的替代品。代理层负责通信和策略，沙箱负责限制不可信代码的执行环境。
- **审计：** 有调用日志和链路追踪，不代表自动满足企业审计要求。还要检查用户身份、工具参数、授权决策、执行结果以及日志防篡改和留存机制。

## 五、接下来应该重点研究什么？

官方仓库：[agentgateway](https://github.com/agentgateway/agentgateway)

官方文档：[About & Architecture](https://agentgateway.dev/docs/standalone/latest/documentation/about/)

结合你的场景，我会优先研究这三个问题：

1. **MCP Gateway 的授权模型：** 它能否实现不同业务用户只能调用有权限的 MCP 工具，以及同一个工具的不同资源权限隔离？
2. **策略执行机制：** CEL 策略具体能获取哪些身份、请求和工具上下文？策略是在什么位置执行的？
3. **独立部署与集成：** 能否在不引入 Kubernetes 的情况下部署，并接入现有 Java 后端、身份体系和内部 MCP 服务？

尤其是第二点。企业级 Agent 平台最容易出现的设计缺陷，是网关虽然验证了调用方身份，却无法可靠地把最终用户的身份和权限约束贯穿到具体工具操作中。

**我的判断：agentgateway 值得作为你们企业 AI Gateway 的重点候选进行验证。** 但如果你的目标是建设通用 Agent Runtime，Agent 的执行循环、会话状态、任务调度、沙箱和知识积累仍然需要由其他组件承担。


---

## 👤 用户

agent的唯一身份应该怎么实现？


---

## 🤖 ChatGPT

我建议把这个问题放在企业级 Agent 平台的身份体系里设计，而不是简单地给每个 Agent 分配一个 UUID。

**Agent 的唯一身份，核心是回答三个不同的问题：**

1. **它是谁？** —— 哪一个 Agent 实例或逻辑 Agent 正在发起调用？
2. **它代表谁？** —— 它是在为哪个用户、业务系统或任务执行操作？
3. **它有权做什么？** —— 它可以调用哪些 MCP 服务、工具，以及访问哪些资源？

这三个问题必须分开。否则，即使每个 Agent 都有唯一 ID，也不能防止一个合法的 Agent 越权访问数据库。

## 一、先区分四种身份

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="fingerprint" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **1. Agent 身份（Agent Identity）**

      标识一个已注册的逻辑 Agent，例如 `ops-assistant`。它的身份跨会话稳定。

      <text color="secondary" size="sm">用途：注册、授权、版本管理、审计归属。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="server" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **2. 工作负载身份（Workload Identity）**

      标识当前实际运行 Agent 的服务或实例，并证明调用确实来自可信的运行环境。

      <text color="secondary" size="sm">用途：服务间认证、实例身份验证、凭据防伪。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="user-round" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **3. 用户身份（User Identity）**

      标识真正发起任务的人，或代表业务系统执行任务的主体。

      <text color="secondary" size="sm">用途：用户权限校验、责任追溯、数据访问控制。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="git-branch" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **4. 会话 / 任务身份（Session / Task Identity）**

      标识某次对话、某个任务或一次执行过程。

      <text color="secondary" size="sm">用途：隔离会话状态、关联调用链、管理任务生命周期。</text>
    </box>
  </row>
</box>

例如，你们的运维 Agent 帮业务同事排查生产问题：

- Agent 身份：`agent:ops-assistant`
- 工作负载身份：当前运行实例的可信身份
- 用户身份：发起排查的业务员工
- 会话身份：本次故障排查会话
- 任务身份：本次查询生产日志的任务

这几种身份不能混为一谈。尤其是会话 ID 不能直接充当 Agent 身份，Agent ID 也不能直接充当授权凭证。

我建议把这个问题放在企业级 Agent 平台的身份体系里设计，而不是简单地给每个 Agent 分配一个 UUID。

**Agent 的唯一身份，核心是回答三个不同的问题：**

1. **它是谁？** —— 哪一个 Agent 实例或逻辑 Agent 正在发起调用？
2. **它代表谁？** —— 它是在为哪个用户、业务系统或任务执行操作？
3. **它有权做什么？** —— 它可以调用哪些 MCP 服务、工具，以及访问哪些资源？

这三个问题必须分开。否则，即使每个 Agent 都有唯一 ID，也不能防止一个合法的 Agent 越权访问数据库。

## 一、先区分四种身份

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="fingerprint" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **1. Agent 身份（Agent Identity）**

      标识一个已注册的逻辑 Agent，例如 `ops-assistant`。它的身份跨会话稳定。

      <text color="secondary" size="sm">用途：注册、授权、版本管理、审计归属。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="server" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **2. 工作负载身份（Workload Identity）**

      标识当前实际运行 Agent 的服务或实例，并证明调用确实来自可信的运行环境。

      <text color="secondary" size="sm">用途：服务间认证、实例身份验证、凭据防伪。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="user-round" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **3. 用户身份（User Identity）**

      标识真正发起任务的人，或代表业务系统执行任务的主体。

      <text color="secondary" size="sm">用途：用户权限校验、责任追溯、数据访问控制。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="git-branch" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **4. 会话 / 任务身份（Session / Task Identity）**

      标识某次对话、某个任务或一次执行过程。

      <text color="secondary" size="sm">用途：隔离会话状态、关联调用链、管理任务生命周期。</text>
    </box>
  </row>
</box>

例如，你们的运维 Agent 帮业务同事排查生产问题：

- Agent 身份：`agent:ops-assistant`
- 工作负载身份：当前运行实例的可信身份
- 用户身份：发起排查的业务员工
- 会话身份：本次故障排查会话
- 任务身份：本次查询生产日志的任务

这几种身份不能混为一谈。尤其是会话 ID 不能直接充当 Agent 身份，Agent ID 也不能直接充当授权凭证。

## 二、推荐的实现方案：注册身份 + 密码学身份

对于你们正在建设的企业 Agent 平台，我建议采用两层机制。

### 第一层：建立 Agent 注册中心

由企业 Agent 平台统一注册和管理 Agent，而不是允许 Agent 自己声明身份。

例如：

| 字段 | 示例 | 作用 |
|---|---|---|
| `agent_id` | `agt_01JXYZ...` | 永久、唯一的逻辑身份 |
| `name` | `ops-assistant` | 人类可读名称 |
| `owner` | `team-infrastructure` | 责任团队 |
| `environment` | `production` | 部署环境 |
| `status` | `ACTIVE` | 生命周期管理 |
| `identity_ref` | `spiffe://corp.example/agent/ops-assistant` | 关联可信运行身份 |
| `policy_id` | `ops-readonly-policy` | 关联授权策略 |

这里的 `agent_id` 可以使用 UUIDv7 或 ULID 生成，也可以由注册中心分配。

**但要注意：唯一 ID 只是标识符，不是身份证明。** 如果一个请求只携带 `agent_id=agt_123`，任何人都可以伪造这个字段。

所以还需要第二层。

### 第二层：为运行中的 Agent 建立可验证身份

有两条主要路线。

<box gap={3}>
  <box border radius="lg" padding={3} gap={2}>
    <row align="center" gap={2}>
      <icon name="key-round" size="xl"/>
      **方案 A：OAuth 2.0 Client Credentials**
      <badge>易落地</badge>
    </row>
    为每个 Agent 服务注册独立的 OAuth Client，由身份提供商签发短期 Access Token。

    调用链路：

    <box background="surface-secondary" radius="md" padding={2} align="center" gap={1}>
      Agent 服务
      <text size="sm" color="secondary">使用自己的客户端凭据获取令牌</text>
    </box>
    <box align="center">
      <icon name="arrow-down" color="secondary" size="lg"/>
    </box>
    <box border radius="md" padding={2} align="center">
      Keycloak
      <text color="secondary" size="sm">验证客户端，签发访问令牌</text>
    </box>
    <box align="center">
      <icon name="arrow-down" color="secondary" size="lg"/>
    </box>
    <box background="surface-secondary" radius="md" padding={2} align="center">
      agentgateway
      <text color="secondary" size="sm">验证令牌，再执行授权策略</text>
    </box>

    Token 可以携带 `sub`、`aud`、`iss`、`exp` 等标准声明，并通过受控的映射关系关联到 `agent_id`。

    优点是方便接入现有 Java 服务和企业 IAM。缺点是必须安全保管客户端凭据，并设计凭据轮换和吊销机制。
  </box>
  <box border radius="lg" padding={3} gap={2}>
    <row align="center" gap={2}>
      <icon name="shield-check" size="xl"/>
      **方案 B：SPIFFE / SPIRE**
      <badge>强工作负载身份</badge>
    </row>
    通过工作负载证明机制，为运行中的服务签发短期、可验证的身份凭证，例如 X.509-SVID，并可使用 mTLS 验证服务之间的通信。

    身份示例：

    `spiffe://corp.example/agent/ops-assistant`

    <text color="secondary" size="sm">优势：减少长期静态密钥，支持自动签发和轮换；尤其适合多服务、多节点及零信任架构。需要部署和维护 SPIRE 等身份基础设施，并配置可信的工作负载证明规则。</text>

    官方资料：[SPIFFE](https://spiffe.io/docs/latest/)、[SPIRE 概念](https://spiffe.io/docs/latest/spire-about/spire-concepts/)。<Cite refs={["turn535898search8","turn535898search1","turn535898search10"]}/>
  </box>
</box>

我的建议是：如果你们现在处于平台建设初期，已有 Keycloak 或企业 OAuth 体系，可以先采用方案 A；如果正在建设更完整的零信任基础设施，且运行环境复杂，再重点评估 SPIFFE/SPIRE。两者也可以组合使用。

## 三、和 agentgateway 如何配合？

这部分尤其重要。

agentgateway 已支持通过 OAuth 2.0 保护 MCP 服务、验证 JWT，并将认证后的声明用于路由级授权、限流等策略。它也支持与 Keycloak 等身份提供商集成。<Cite refs={["turn535898search4","turn535898search11","turn535898search13"]}/>

但需要区分两个动作：

- **认证 Agent：** 证明请求来自哪个可信的 Agent 或工作负载。
- **授权 Agent：** 判断该 Agent 是否有权调用指定工具、访问指定数据。

我建议你们的调用链路是：

<box border radius="lg" padding={3} gap={2} align="center">
  <box background="surface-secondary" radius="md" padding={3} width="100%" align="center">
    **Agent Runtime**
    <text color="secondary" size="sm">持有自身工作负载身份</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg"/>
  <box border radius="md" padding={3} width="100%" align="center" gap={1}>
    **agentgateway**
    <text size="sm">验证凭证、提取身份声明、执行策略</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg"/>
  <box background="surface-secondary" radius="md" padding={3} width="100%" align="center">
    **企业授权服务 / 策略引擎**
    <text color="secondary" size="sm">根据 Agent、用户、工具、资源和任务上下文决策</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg"/>
  <box border radius="md" padding={3} width="100%" align="center">
    **MCP Server**
    <text color="secondary" size="sm">执行工具级及数据级权限检查</text>
  </box>
</box>

例如，`ops-assistant` 可以查询生产日志，但不能因为它拥有 Agent 身份，就自动获得生产数据库的写权限。

如果这次任务是员工 A 发起的，授权策略还应根据该员工的权限和任务限制决定可访问范围，而不是只看 Agent 自身的身份。

## 四、还有一个容易忽略的问题：Agent 的实例身份与逻辑身份

假设 `ops-assistant` 部署了 10 个副本。

- 逻辑 Agent：仍然是同一个 `agent_id`。
- 运行实例：每个副本具有独立、可验证的工作负载身份，或具有能够识别实例的受控身份属性。
- 会话：每个会话具有独立的 `session_id`。
- 任务：每次执行具有独立的 `task_id`。

不要将 Pod 名称、IP 地址或容器 ID 直接当成永久 Agent 身份。这些属性会随着部署和扩缩容而变化。

如果需要追踪到某个具体副本，再把运行时身份和实例元数据关联起来即可。

---

**最后，我想确认一个会直接影响架构选择的问题：你说的 Agent 唯一身份，主要是为了哪一种场景？**

{@body const [purpose,setPurpose] = DIL.useState("security")}
{@body const [runtime,setRuntime] = DIL.useState("mixed")}
<box border radius="xl" padding={3} gap={4}>
  <text weight="medium">1. 你最想解决什么问题？</text>
  <radio-group value={purpose} onChange={setPurpose} direction="col">
    <radio value="registry">给企业注册的每个逻辑 Agent 分配唯一 ID，便于管理和追踪</radio>
    <radio value="security">让 MCP / agentgateway 能可靠识别调用方，实施鉴权与授权</radio>
    <radio value="delegation">让 Agent 调用其他 Agent 时，能够识别调用者及其代表的用户</radio>
    <radio value="all">以上都需要，想建设完整的企业 Agent 身份体系</radio>
  </radio-group>
  <divider color="subtle"/>
  <text weight="medium">2. Agent 主要运行在哪里？</text>
  <radio-group value={runtime} onChange={setRuntime} direction="col">
    <radio value="k8s">Kubernetes / 容器集群</radio>
    <radio value="vm">虚拟机或普通服务器上的 Java 服务</radio>
    <radio value="mixed">混合环境，既有服务端 Agent，也可能有独立沙箱中的 Agent</radio>
  </radio-group>
  <button block onClick={() => GenUI.issueNewTurn("请为企业级 Agent 平台设计唯一身份体系。核心目标："+({"registry":"逻辑 Agent 注册、唯一 ID 和生命周期管理","security":"通过 MCP / agentgateway 可靠识别调用方并实施鉴权授权","delegation":"Agent 间调用时识别调用者及其代表的用户","all":"完整的企业 Agent 身份、认证、授权与委托体系"}[purpose])+"。运行环境："+({"k8s":"Kubernetes / 容器集群","vm":"虚拟机或普通服务器上的 Java 服务","mixed":"混合环境，包括服务端 Agent 和可能的独立沙箱 Agent"}[runtime])+"。请给出可落地的架构、身份模型、凭证签发与校验流程。")}>按这个场景细化方案</button>
</box>

## 二、推荐的实现方案：注册身份 + 密码学身份

对于你们正在建设的企业 Agent 平台，我建议采用两层机制。

### 第一层：建立 Agent 注册中心

由企业 Agent 平台统一注册和管理 Agent，而不是允许 Agent 自己声明身份。

例如：

| 字段 | 示例 | 作用 |
|---|---|---|
| `agent_id` | `agt_01JXYZ...` | 永久、唯一的逻辑身份 |
| `name` | `ops-assistant` | 人类可读名称 |
| `owner` | `team-infrastructure` | 责任团队 |
| `environment` | `production` | 部署环境 |
| `status` | `ACTIVE` | 生命周期管理 |
| `identity_ref` | `spiffe://corp.example/agent/ops-assistant` | 关联可信运行身份 |
| `policy_id` | `ops-readonly-policy` | 关联授权策略 |

这里的 `agent_id` 可以使用 UUIDv7 或 ULID 生成，也可以由注册中心分配。

**但要注意：唯一 ID 只是标识符，不是身份证明。** 如果一个请求只携带 `agent_id=agt_123`，任何人都可以伪造这个字段。

所以还需要第二层。

### 第二层：为运行中的 Agent 建立可验证身份

有两条主要路线。

<box gap={3}>
  <box border radius="lg" padding={3} gap={2}>
    <row align="center" gap={2}>
      <icon name="key-round" size="xl"/>
      **方案 A：OAuth 2.0 Client Credentials**
      <badge>易落地</badge>
    </row>
    为每个 Agent 服务注册独立的 OAuth Client，由身份提供商签发短期 Access Token。

    调用链路：

    <box background="surface-secondary" radius="md" padding={2} align="center" gap={1}>
      Agent 服务
      <text size="sm" color="secondary">使用自己的客户端凭据获取令牌</text>
    </box>
    <box align="center">
      <icon name="arrow-down" color="secondary" size="lg"/>
    </box>
    <box border radius="md" padding={2} align="center">
      Keycloak
      <text color="secondary" size="sm">验证客户端，签发访问令牌</text>
    </box>
    <box align="center">
      <icon name="arrow-down" color="secondary" size="lg"/>
    </box>
    <box background="surface-secondary" radius="md" padding={2} align="center">
      agentgateway
      <text color="secondary" size="sm">验证令牌，再执行授权策略</text>
    </box>

    Token 可以携带 `sub`、`aud`、`iss`、`exp` 等标准声明，并通过受控的映射关系关联到 `agent_id`。

    优点是方便接入现有 Java 服务和企业 IAM。缺点是必须安全保管客户端凭据，并设计凭据轮换和吊销机制。
  </box>
  <box border radius="lg" padding={3} gap={2}>
    <row align="center" gap={2}>
      <icon name="shield-check" size="xl"/>
      **方案 B：SPIFFE / SPIRE**
      <badge>强工作负载身份</badge>
    </row>
    通过工作负载证明机制，为运行中的服务签发短期、可验证的身份凭证，例如 X.509-SVID，并可使用 mTLS 验证服务之间的通信。

    身份示例：

    `spiffe://corp.example/agent/ops-assistant`

    <text color="secondary" size="sm">优势：减少长期静态密钥，支持自动签发和轮换；尤其适合多服务、多节点及零信任架构。需要部署和维护 SPIRE 等身份基础设施，并配置可信的工作负载证明规则。</text>

    官方资料：[SPIFFE](https://spiffe.io/docs/latest/)、[SPIRE 概念](https://spiffe.io/docs/latest/spire-about/spire-concepts/)。<Cite refs={["turn535898search8","turn535898search1","turn535898search10"]}/>
  </box>
</box>

我的建议是：如果你们现在处于平台建设初期，已有 Keycloak 或企业 OAuth 体系，可以先采用方案 A；如果正在建设更完整的零信任基础设施，且运行环境复杂，再重点评估 SPIFFE/SPIRE。两者也可以组合使用。

## 三、和 agentgateway 如何配合？

这部分尤其重要。

agentgateway 已支持通过 OAuth 2.0 保护 MCP 服务、验证 JWT，并将认证后的声明用于路由级授权、限流等策略。它也支持与 Keycloak 等身份提供商集成。<Cite refs={["turn535898search4","turn535898search11","turn535898search13"]}/>

但需要区分两个动作：

- **认证 Agent：** 证明请求来自哪个可信的 Agent 或工作负载。
- **授权 Agent：** 判断该 Agent 是否有权调用指定工具、访问指定数据。

我建议你们的调用链路是：

<box border radius="lg" padding={3} gap={2} align="center">
  <box background="surface-secondary" radius="md" padding={3} width="100%" align="center">
    **Agent Runtime**
    <text color="secondary" size="sm">持有自身工作负载身份</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg"/>
  <box border radius="md" padding={3} width="100%" align="center" gap={1}>
    **agentgateway**
    <text size="sm">验证凭证、提取身份声明、执行策略</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg"/>
  <box background="surface-secondary" radius="md" padding={3} width="100%" align="center">
    **企业授权服务 / 策略引擎**
    <text color="secondary" size="sm">根据 Agent、用户、工具、资源和任务上下文决策</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg"/>
  <box border radius="md" padding={3} width="100%" align="center">
    **MCP Server**
    <text color="secondary" size="sm">执行工具级及数据级权限检查</text>
  </box>
</box>

例如，`ops-assistant` 可以查询生产日志，但不能因为它拥有 Agent 身份，就自动获得生产数据库的写权限。

如果这次任务是员工 A 发起的，授权策略还应根据该员工的权限和任务限制决定可访问范围，而不是只看 Agent 自身的身份。

## 四、还有一个容易忽略的问题：Agent 的实例身份与逻辑身份

假设 `ops-assistant` 部署了 10 个副本。

- 逻辑 Agent：仍然是同一个 `agent_id`。
- 运行实例：每个副本具有独立、可验证的工作负载身份，或具有能够识别实例的受控身份属性。
- 会话：每个会话具有独立的 `session_id`。
- 任务：每次执行具有独立的 `task_id`。

不要将 Pod 名称、IP 地址或容器 ID 直接当成永久 Agent 身份。这些属性会随着部署和扩缩容而变化。

如果需要追踪到某个具体副本，再把运行时身份和实例元数据关联起来即可。

---

**最后，我想确认一个会直接影响架构选择的问题：你说的 Agent 唯一身份，主要是为了哪一种场景？**

{@body const [purpose,setPurpose] = DIL.useState("security")}
{@body const [runtime,setRuntime] = DIL.useState("mixed")}
<box border radius="xl" padding={3} gap={4}>
  <text weight="medium">1. 你最想解决什么问题？</text>
  <radio-group value={purpose} onChange={setPurpose} direction="col">
    <radio value="registry">给企业注册的每个逻辑 Agent 分配唯一 ID，便于管理和追踪</radio>
    <radio value="security">让 MCP / agentgateway 能可靠识别调用方，实施鉴权与授权</radio>
    <radio value="delegation">让 Agent 调用其他 Agent 时，能够识别调用者及其代表的用户</radio>
    <radio value="all">以上都需要，想建设完整的企业 Agent 身份体系</radio>
  </radio-group>
  <divider color="subtle"/>
  <text weight="medium">2. Agent 主要运行在哪里？</text>
  <radio-group value={runtime} onChange={setRuntime} direction="col">
    <radio value="k8s">Kubernetes / 容器集群</radio>
    <radio value="vm">虚拟机或普通服务器上的 Java 服务</radio>
    <radio value="mixed">混合环境，既有服务端 Agent，也可能有独立沙箱中的 Agent</radio>
  </radio-group>
  <button block onClick={() => GenUI.issueNewTurn("请为企业级 Agent 平台设计唯一身份体系。核心目标："+({"registry":"逻辑 Agent 注册、唯一 ID 和生命周期管理","security":"通过 MCP / agentgateway 可靠识别调用方并实施鉴权授权","delegation":"Agent 间调用时识别调用者及其代表的用户","all":"完整的企业 Agent 身份、认证、授权与委托体系"}[purpose])+"。运行环境："+({"k8s":"Kubernetes / 容器集群","vm":"虚拟机或普通服务器上的 Java 服务","mixed":"混合环境，包括服务端 Agent 和可能的独立沙箱 Agent"}[runtime])+"。请给出可落地的架构、身份模型、凭证签发与校验流程。")}>按这个场景细化方案</button>
</box>


---

## 👤 用户

请为企业级 Agent 平台设计唯一身份体系。核心目标：完整的企业 Agent 身份、认证、授权与委托体系。运行环境：混合环境，包括服务端 Agent 和可能的独立沙箱 Agent。请给出可落地的架构、身份模型、凭证签发与校验流程。


---

## 🤖 ChatGPT

# 企业级 Agent 身份、认证、授权与委托体系设计

结合你的场景——企业通用 Agent 平台、混合运行环境、MCP 工具调用、独立沙箱，以及高安全要求——我建议采用一套以 **Workload Identity（工作负载身份）+ OAuth 2.0 Token Exchange（令牌交换）+ 统一授权策略引擎** 为核心的架构。

最重要的设计原则是：

**每个 Agent 有独立身份，每个运行实例有可信身份，每次任务有独立上下文；身份认证、权限决策和用户委托分别处理。**

不要让 Agent 仅靠一个 UUID 证明自己，也不要给所有 Agent 发一个包含全部权限的长期 Token。

## 一、整体架构

<box border radius="xl" padding={3} gap={3}>
  <box background="surface-secondary" radius="lg" padding={3} gap={1} align="center">
    <icon name="users" size="xl"/>
    **用户 / 业务系统**
    <text color="secondary" size="sm">企业 SSO、用户身份、业务系统身份</text>
  </box>
  <box align="center" gap={1}>
    <icon name="arrow-down" size="lg" color="secondary"/>
    <text color="secondary" size="xs">发起任务 / 申请执行权限</text>
  </box>
  <box border radius="lg" padding={3} gap={2}>
    <title size="md" textAlign="center">Agent Platform Control Plane</title>
    <grid columns={2} gap={2}>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="contact-round" size="lg"/>
          **Agent Registry**
          <text color="secondary" size="xs">Agent 注册、版本、所有者、状态</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="key-round" size="lg"/>
          **Identity Service**
          <text color="secondary" size="xs">身份绑定、凭证签发、轮换、吊销</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="shield-check" size="lg"/>
          **Policy Engine**
          <text color="secondary" size="xs">Agent、用户、工具、资源授权</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="clipboard-list" size="lg"/>
          **Audit Service**
          <text color="secondary" size="xs">身份、决策、委托与执行审计</text>
        </box>
      </grid-item>
    </grid>
    <text color="secondary" size="sm" textAlign="center">身份与授权管理平面，不直接承担 Agent 任务执行</text>
  </box>
  <box align="center" gap={1}>
    <icon name="arrow-down-up" size="lg" color="secondary"/>
    <text color="secondary" size="xs">短期凭证 / 策略查询</text>
  </box>
  <box border radius="lg" padding={3} gap={2}>
    <title size="md" textAlign="center">Agent Runtime / Execution Plane</title>
    <grid columns={2} gap={2}>
      <grid-item>
        <box background="surface-secondary" radius="md" padding={3} gap={1}>
          **服务端 Agent**
          <text color="secondary" size="sm">Java 服务、容器、集群副本</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface-secondary" radius="md" padding={3} gap={1}>
          **沙箱 Agent**
          <text color="secondary" size="sm">独立会话、短生命周期执行环境</text>
        </box>
      </grid-item>
    </grid>
    <box align="center">
      <icon name="arrow-down-up" color="secondary" size="lg"/>
    </box>
    <box border radius="md" padding={3} align="center" gap={1}>
      **agentgateway / API Gateway**
      <text color="secondary" size="sm">入口认证、协议代理、策略执行、流量治理</text>
    </box>
    <grid columns={2} gap={2}>
      <grid-item>
        <box border radius="md" padding={2} gap={1} align="center">
          **LLM Gateway**
          <text color="secondary" size="xs">模型 API、模型路由</text>
        </box>
      </grid-item>
      <grid-item>
        <box border radius="md" padding={2} gap={1} align="center">
          **MCP Gateway**
          <text color="secondary" size="xs">工具入口、服务认证</text>
        </box>
      </grid-item>
    </grid>
  </box>
  <box align="center">
    <icon name="arrow-down-up" color="secondary" size="lg"/>
  </box>
  <box background="surface-secondary" radius="lg" padding={3} align="center" gap={1}>
    **企业资源**
    <text color="secondary" size="sm">数据库、代码仓库、服务器、业务 API、其他 Agent</text>
  </box>
</box>

这套架构中，身份服务和授权服务可以复用现有企业 IAM，但不要把 Agent 注册、任务委托和 Agent 特有的授权逻辑全部塞进传统 IAM。

建议分工如下：

- **Keycloak 或企业 IdP：** 用户登录、OAuth 2.0/OIDC、客户端认证、Token 签发。
- **Agent Registry：** 管理逻辑 Agent 的身份、所有者、部署绑定和生命周期。
- **SPIFFE/SPIRE（可选）：** 为运行中的工作负载建立密码学身份，尤其适合动态容器和沙箱。
- **agentgateway：** 执行入口认证、协议代理及其支持的授权策略。
- **授权策略引擎：** 负责细粒度决策，可采用 OPA、Cedar 或自研 PDP；不必同时引入多个。
- **MCP Server 与资源侧：** 对高风险工具和数据执行最终权限检查，避免只依赖网关。

这里有一个关键点：**网关是策略执行点之一，不应成为唯一的安全边界。**

## 二、身份模型：建议把“身份”拆成六类核心对象

不要只建一张 `agent` 表，然后把用户、Token、沙箱和权限都塞进去。建议至少区分以下六类对象。

<text color="secondary" size="sm">以下是建议的企业内部数据模型，不是某个标准规定的固定表结构。</text>

| 对象 | 标识符示例 | 生命周期 |
|---|---|---|
| Agent Definition | `agt_ops_assistant` | 跨版本长期存在 |
| Agent Deployment | `dep_ops_prod` | 随部署配置变化 |
| Workload Instance | SPIFFE ID 或实例记录 ID | 随实例启动、销毁变化 |
| User / Principal | 企业用户 ID 或服务账号 ID | 由企业身份系统管理 |
| Session / Task | `ses_...` / `task_...` | 随会话、任务创建和结束 |
| Delegation Grant | `dlg_...` | 有范围、有期限，可撤销 |

这里还需要一个独立概念：**Resource Identity**。数据库、代码仓库、MCP 服务和具体工具也应有稳定标识符，否则授权策略只能模糊地判断“是否能访问某个服务”，很难控制到具体资源。

### 2.1 Agent Definition：逻辑身份

建议以注册中心分配的不可变 `agent_id` 作为主键。名称可以修改，但 ID 不应因改名、升级版本或重新部署而改变。

```text
agent_id: agt_ops_assistant
name: ops-assistant
owner_team: infrastructure
status: ACTIVE
allowed_environments:
  - staging
  - production
policy_set: ops-assistant-v1
```

Agent Definition 表示“这个 Agent 是谁”，不表示它当前在哪台机器上运行，也不直接证明某次请求来自它。

### 2.2 Deployment 与 Workload：部署身份和运行身份

同一个逻辑 Agent 可能部署在多个环境、启动多个副本，也可能临时创建独立沙箱。

例如：

```text
Agent Definition
  agt_ops_assistant
       |
       +-- Deployment: dep_ops_prod
       |      +-- Instance: spiffe://corp.example/ns/prod/sa/ops
       |      +-- Instance: spiffe://corp.example/ns/prod/sa/ops
       |
       +-- Deployment: dep_ops_staging
              +-- Instance: spiffe://corp.example/ns/staging/sa/ops
```

上面只是结构示意，生产中应让不同实例具有能被可信机制验证的工作负载身份，并把它映射回已登记的 Agent 和 Deployment。

对于共享运行进程的多租户 Agent，不能仅凭进程级 SPIFFE ID 就断定请求属于哪个逻辑 Agent；还必须有可信的任务路由或执行上下文绑定。

### 2.3 用户、任务与委托

一次调用最好能够回答：

```text
谁发起：       user_123
哪个 Agent：   agt_ops_assistant
哪个实例：     workload_identity
哪个任务：     task_456
调用哪个工具： tool.query_production_logs
访问什么资源： resource.app_server_logs
凭什么授权：   policy_decision_789
```

这些字段需要通过可信链路传递或由服务端生成，不能让调用方随意提交一组 HTTP Header 就被当成事实。

## 三、认证凭证：不同用途使用不同凭证

我建议建立三种凭证，不要试图用一张 JWT 解决所有问题。

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="shield-check" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **A. 工作负载凭证**

      用于证明“当前运行的服务或沙箱是谁”。

      推荐：SPIFFE X.509-SVID + mTLS；无法使用 mTLS 的边界，再考虑短期 JWT 或其他受控凭证。

      <text color="secondary" size="sm">签发方：SPIRE 或企业工作负载身份服务。验证方：网关、身份代理或下游服务。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="key-round" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **B. 访问令牌（Access Token）**

      用于证明请求者有权向某个目标服务发起请求，且携带有限的授权上下文。

      推荐：OAuth 2.0 短期、限定受众的 Access Token，采用 JWT 或不透明令牌均可。

      <text color="secondary" size="sm">签发方：Keycloak 或企业授权服务器。验证方：agentgateway、MCP Server、业务 API。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="git-branch" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **C. 委托凭证 / 委托授权记录**

      用于表达某个主体允许 Agent 在限定条件下代表它执行任务。

      推荐：服务端保存 Delegation Grant，并在调用具体资源时签发或交换出受限访问令牌。

      <text color="secondary" size="sm">签发方：经过授权的委托服务。验证方：策略引擎和目标资源服务。</text>
    </box>
  </row>
</box>

SPIFFE 的 X.509-SVID 可以通过证书和受信任的信任域验证工作负载身份；其官方文档建议在适用的场景优先考虑 X.509-SVID，而非容易被复制和重放的 bearer JWT-SVID。<Cite refs={["turn906303search0","turn906303search2","turn906303search3"]}/>

这三种凭证各自解决不同的问题：

- 工作负载凭证回答“请求来自哪个可信执行环境”。
- Access Token 回答“这个请求可以访问哪个目标服务”。
- Delegation Grant 回答“为什么这个 Agent 可以代表某个主体执行任务”。

**特别注意：委托记录不等于访问凭证，工作负载认证成功也不等于业务授权成功。**

## 四、凭证签发与校验流程

### 流程 A：服务端 Agent 启动

以 Java 运维 Agent 为例。

<box gap={2}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2} minWidth="40px" align="center">
      **1**
    </box>
    <box flex="1" gap={1}>
      **平台部署 Agent**

      平台根据已审批的 Deployment 配置启动服务，并将部署与 `agent_id` 绑定。Agent 不允许通过请求参数自行选择任意逻辑身份。
    </box>
  </row>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2} minWidth="40px" align="center">
      **2**
    </box>
    <box flex="1" gap={1}>
      **获取工作负载身份**

      在 Kubernetes 中，SPIRE 可依据受控的 Pod、Service Account 等属性进行工作负载证明；非 Kubernetes 环境则需要配置对应的主机或进程证明机制。不能只凭客户端声称的 SPIFFE ID 签发身份。
    </box>
  </row>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2} minWidth="40px" align="center">
      **3**
    </box>
    <box flex="1" gap={1}>
      **映射至注册中心**

      平台验证该工作负载是否被授权运行 `agt_ops_assistant`，以及它属于哪个 Deployment、环境和租户。
    </box>
  </row>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2} minWidth="40px" align="center">
      **4**
    </box>
    <box flex="1" gap={1}>
      **获取目标访问令牌**

      Agent 使用工作负载身份或专门配置的客户端认证方式，向授权服务器申请面向指定 API 或 MCP 服务的短期 Token。
    </box>
  </row>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2} minWidth="40px" align="center">
      **5**
    </box>
    <box flex="1" gap={1}>
      **目标服务验证并授权**

      验证令牌签名或令牌有效性、`iss`、`aud`、有效期及必要的身份声明，再依据策略判断具体操作是否允许。
    </box>
  </row>
</box>

SPIFFE Workload API 提供了获取 SVID、密钥和信任材料的标准接口；具体如何证明工作负载身份由实现与部署方式决定。<Cite refs={["turn906303search2","turn906303search4"]}/>

### 流程 B：业务用户发起 Agent 任务

这是企业平台中更需要仔细设计的部分。

假设员工 A 要求运维 Agent 查询某台服务器的生产日志。

1. 员工先通过企业 SSO 登录，前端获得面向业务 API 的用户访问令牌。
2. 后端验证用户身份，并创建任务记录，绑定用户、Agent、会话、环境和任务范围。
3. Agent Runtime 使用自己的工作负载身份向平台领取任务。
4. 平台根据用户权限、Agent 权限、任务约束及风险等级，建立有期限的委托授权记录。
5. Agent 调用 MCP Gateway 时，携带面向目标服务的短期访问令牌。
6. MCP Gateway 验证调用方身份和令牌，再根据授权策略判断工具调用是否允许。
7. MCP Server 对目标资源执行必要的二次授权，完成操作后记录审计事件。

其中第 4 步不能仅仅把员工的原始 Token 复制给 Agent。应该让委托关系可验证、可撤销，并且明确限制目标资源和操作范围。

### 流程 C：Agent 调用另一个 Agent

例如，运维 Agent 将日志分析任务委托给日志分析 Agent。

建议把调用链保留为：

```text
user_123
  └── agt_ops_assistant
        └── agt_log_analyzer
              └── tool.search_logs
```

接收方必须能够区分：

- 原始委托主体是谁；
- 当前直接调用者是谁；
- 当前工作负载是什么；
- 当前调用链的委托范围是什么。

可采用 OAuth 2.0 Token Exchange（RFC 8693）交换出面向下游服务的受限 Token，同时在可信审计系统中记录委托链。<Cite refs={["turn906303search1"]}/>

需要注意，RFC 8693 提供的是令牌交换协议，不会自动替你完成权限继承、委托链验证或防止权限扩大。这些规则仍需由企业平台制定和执行。

## 五、Token 应该长什么样？

建议统一访问令牌的核心声明，但按目标服务和使用场景分别签发，不要让所有调用共用一份万能 Token。

例如，下面是一份用于展示身份与委托关系的 JWT 载荷示意：

```json
{
  "iss": "https://id.example.com/realms/enterprise",
  "sub": "agt_ops_assistant",
  "aud": "mcp-gateway",
  "iat": 1792080000,
  "exp": 1792080300,
  "jti": "tok_01JXYZ",
  "client_id": "ops-assistant-runtime",
  "scope": "logs:read",
  "https://corp.example/claims/agent_id": "agt_ops_assistant",
  "https://corp.example/claims/task_id": "task_456",
  "https://corp.example/claims/delegation_id": "dlg_789"
}
```

这是一个**示意载荷，不是可直接用于生产的令牌**。其中时间戳、签名、密钥管理、客户端认证和声明映射都需要实际实现；自定义声明也必须由受信任的签发方生成。

几个重要的设计细节：

- `sub`：表示该 Token 的主体，必须在整个系统中定义清楚其语义。不要有时放用户、有时放 Agent，却没有明确约定。
- `aud`：必须限制目标服务，避免发给 MCP Gateway 的 Token 被拿去调用其他 API。
- `scope`：可以表示粗粒度权限，但数据库、服务器、租户等资源级权限往往还需要进一步判断。
- `task_id` 和 `delegation_id`：用于关联任务及委托记录；它们本身不是权限证明。
- `jti`：用于标识令牌，配合服务端状态或重放防护机制时才有额外安全价值。

如果需要完整表达 OAuth 令牌交换中的委托关系，可以根据 RFC 8693 使用标准的 `act` 声明表达当前代表原始主体行动的 Actor；但实际 Token 格式、嵌套方式和验证规则要与身份提供商及资源服务器的实现保持一致。

不要把完整的权限策略、数据库密码或服务器凭据直接塞进 JWT。令牌应尽可能短小，并且只携带完成授权决策所需的信息。

## 六、委托体系：如何防止 Agent 越权？

我建议把授权拆成两类：**Agent 自身的能力边界**和**本次任务获得的委托权限**。

例如：

| 授权维度 | 运维 Agent 的最大能力 | 本次任务的权限 |
|---|---|---|
| 目标环境 | 测试、生产 | 仅生产 |
| 日志查询 | 允许 | 仅指定应用 |
| 数据库查询 | 只读 | 仅指定数据库及表 |
| 数据库写入 | 禁止 | 禁止 |
| 服务器命令 | 受控白名单 | 仅允许读取日志的命令 |
| 持续时间 | 按平台策略 | 10 分钟后失效 |

本次任务的有效权限应满足：

\[
P_{\text{effective}}
\subseteq
P_{\text{agent}}
\cap
P_{\text{user}}
\cap
P_{\text{task}}
\cap
P_{\text{resource}}
\]

这里的集合交集是一个简化模型：实际策略还需要考虑拒绝规则、环境、资源属性、风险等级和审批要求。

例如，员工有生产日志读取权限，Agent 也被允许执行日志查询，但任务只允许访问应用 A 的日志。那么 Agent 不能利用这个任务去查询应用 B 的日志，更不能借此执行数据库写操作。

### 委托授权记录建议包含哪些字段？

```text
delegation_id
subject_type          // user / service
subject_id
agent_id
task_id
audience              // 目标服务
allowed_actions       // 允许的动作
resource_constraints  // 资源范围
issued_at
expires_at
status                // active / revoked / expired
parent_delegation_id  // 可选：上游委托
```

委托服务在签发访问令牌前，必须检查原始主体是否有权进行这项委托，以及目标 Agent 是否有能力执行相应操作。

**禁止权限扩大：** 下游 Agent 不能因为拥有自己的服务身份，就获得超出上游委托范围的权限。它自己的最大权限和上游委托权限必须同时约束最终结果。

对于高风险操作，例如生产数据库写入、重启服务、修改防火墙，建议增加独立审批或二次确认，而不是完全依赖 Agent 的推理结果。

# 企业级 Agent 身份、认证、授权与委托体系设计

结合你的场景——企业通用 Agent 平台、混合运行环境、MCP 工具调用、独立沙箱，以及高安全要求——我建议采用一套以 **Workload Identity（工作负载身份）+ OAuth 2.0 Token Exchange（令牌交换）+ 统一授权策略引擎** 为核心的架构。

最重要的设计原则是：

**每个 Agent 有独立身份，每个运行实例有可信身份，每次任务有独立上下文；身份认证、权限决策和用户委托分别处理。**

不要让 Agent 仅靠一个 UUID 证明自己，也不要给所有 Agent 发一个包含全部权限的长期 Token。

## 一、整体架构

<box border radius="xl" padding={3} gap={3}>
  <box background="surface-secondary" radius="lg" padding={3} gap={1} align="center">
    <icon name="users" size="xl"/>
    **用户 / 业务系统**
    <text color="secondary" size="sm">企业 SSO、用户身份、业务系统身份</text>
  </box>
  <box align="center" gap={1}>
    <icon name="arrow-down" size="lg" color="secondary"/>
    <text color="secondary" size="xs">发起任务 / 申请执行权限</text>
  </box>
  <box border radius="lg" padding={3} gap={2}>
    <title size="md" textAlign="center">Agent Platform Control Plane</title>
    <grid columns={2} gap={2}>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="contact-round" size="lg"/>
          **Agent Registry**
          <text color="secondary" size="xs">Agent 注册、版本、所有者、状态</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="key-round" size="lg"/>
          **Identity Service**
          <text color="secondary" size="xs">身份绑定、凭证签发、轮换、吊销</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="shield-check" size="lg"/>
          **Policy Engine**
          <text color="secondary" size="xs">Agent、用户、工具、资源授权</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface" radius="md" padding={2} gap={1}>
          <icon name="clipboard-list" size="lg"/>
          **Audit Service**
          <text color="secondary" size="xs">身份、决策、委托与执行审计</text>
        </box>
      </grid-item>
    </grid>
    <text color="secondary" size="sm" textAlign="center">身份与授权管理平面，不直接承担 Agent 任务执行</text>
  </box>
  <box align="center" gap={1}>
    <icon name="arrow-down-up" size="lg" color="secondary"/>
    <text color="secondary" size="xs">短期凭证 / 策略查询</text>
  </box>
  <box border radius="lg" padding={3} gap={2}>
    <title size="md" textAlign="center">Agent Runtime / Execution Plane</title>
    <grid columns={2} gap={2}>
      <grid-item>
        <box background="surface-secondary" radius="md" padding={3} gap={1}>
          **服务端 Agent**
          <text color="secondary" size="sm">Java 服务、容器、集群副本</text>
        </box>
      </grid-item>
      <grid-item>
        <box background="surface-secondary" radius="md" padding={3} gap={1}>
          **沙箱 Agent**
          <text color="secondary" size="sm">独立会话、短生命周期执行环境</text>
        </box>
      </grid-item>
    </grid>
    <box align="center">
      <icon name="arrow-down-up" color="secondary" size="lg"/>
    </box>
    <box border radius="md" padding={3} align="center" gap={1}>
      **agentgateway / API Gateway**
      <text color="secondary" size="sm">入口认证、协议代理、策略执行、流量治理</text>
    </box>
    <grid columns={2} gap={2}>
      <grid-item>
        <box border radius="md" padding={2} gap={1} align="center">
          **LLM Gateway**
          <text color="secondary" size="xs">模型 API、模型路由</text>
        </box>
      </grid-item>
      <grid-item>
        <box border radius="md" padding={2} gap={1} align="center">
          **MCP Gateway**
          <text color="secondary" size="xs">工具入口、服务认证</text>
        </box>
      </grid-item>
    </grid>
  </box>
  <box align="center">
    <icon name="arrow-down-up" color="secondary" size="lg"/>
  </box>
  <box background="surface-secondary" radius="lg" padding={3} align="center" gap={1}>
    **企业资源**
    <text color="secondary" size="sm">数据库、代码仓库、服务器、业务 API、其他 Agent</text>
  </box>
</box>

这套架构中，身份服务和授权服务可以复用现有企业 IAM，但不要把 Agent 注册、任务委托和 Agent 特有的授权逻辑全部塞进传统 IAM。

建议分工如下：

- **Keycloak 或企业 IdP：** 用户登录、OAuth 2.0/OIDC、客户端认证、Token 签发。
- **Agent Registry：** 管理逻辑 Agent 的身份、所有者、部署绑定和生命周期。
- **SPIFFE/SPIRE（可选）：** 为运行中的工作负载建立密码学身份，尤其适合动态容器和沙箱。
- **agentgateway：** 执行入口认证、协议代理及其支持的授权策略。
- **授权策略引擎：** 负责细粒度决策，可采用 OPA、Cedar 或自研 PDP；不必同时引入多个。
- **MCP Server 与资源侧：** 对高风险工具和数据执行最终权限检查，避免只依赖网关。

这里有一个关键点：**网关是策略执行点之一，不应成为唯一的安全边界。**

## 二、身份模型：建议把“身份”拆成六类核心对象

不要只建一张 `agent` 表，然后把用户、Token、沙箱和权限都塞进去。建议至少区分以下六类对象。

<text color="secondary" size="sm">以下是建议的企业内部数据模型，不是某个标准规定的固定表结构。</text>

| 对象 | 标识符示例 | 生命周期 |
|---|---|---|
| Agent Definition | `agt_ops_assistant` | 跨版本长期存在 |
| Agent Deployment | `dep_ops_prod` | 随部署配置变化 |
| Workload Instance | SPIFFE ID 或实例记录 ID | 随实例启动、销毁变化 |
| User / Principal | 企业用户 ID 或服务账号 ID | 由企业身份系统管理 |
| Session / Task | `ses_...` / `task_...` | 随会话、任务创建和结束 |
| Delegation Grant | `dlg_...` | 有范围、有期限，可撤销 |

这里还需要一个独立概念：**Resource Identity**。数据库、代码仓库、MCP 服务和具体工具也应有稳定标识符，否则授权策略只能模糊地判断“是否能访问某个服务”，很难控制到具体资源。

### 2.1 Agent Definition：逻辑身份

建议以注册中心分配的不可变 `agent_id` 作为主键。名称可以修改，但 ID 不应因改名、升级版本或重新部署而改变。

```text
agent_id: agt_ops_assistant
name: ops-assistant
owner_team: infrastructure
status: ACTIVE
allowed_environments:
  - staging
  - production
policy_set: ops-assistant-v1
```

Agent Definition 表示“这个 Agent 是谁”，不表示它当前在哪台机器上运行，也不直接证明某次请求来自它。

### 2.2 Deployment 与 Workload：部署身份和运行身份

同一个逻辑 Agent 可能部署在多个环境、启动多个副本，也可能临时创建独立沙箱。

例如：

```text
Agent Definition
  agt_ops_assistant
       |
       +-- Deployment: dep_ops_prod
       |      +-- Instance: spiffe://corp.example/ns/prod/sa/ops
       |      +-- Instance: spiffe://corp.example/ns/prod/sa/ops
       |
       +-- Deployment: dep_ops_staging
              +-- Instance: spiffe://corp.example/ns/staging/sa/ops
```

上面只是结构示意，生产中应让不同实例具有能被可信机制验证的工作负载身份，并把它映射回已登记的 Agent 和 Deployment。

对于共享运行进程的多租户 Agent，不能仅凭进程级 SPIFFE ID 就断定请求属于哪个逻辑 Agent；还必须有可信的任务路由或执行上下文绑定。

### 2.3 用户、任务与委托

一次调用最好能够回答：

```text
谁发起：       user_123
哪个 Agent：   agt_ops_assistant
哪个实例：     workload_identity
哪个任务：     task_456
调用哪个工具： tool.query_production_logs
访问什么资源： resource.app_server_logs
凭什么授权：   policy_decision_789
```

这些字段需要通过可信链路传递或由服务端生成，不能让调用方随意提交一组 HTTP Header 就被当成事实。

## 三、认证凭证：不同用途使用不同凭证

我建议建立三种凭证，不要试图用一张 JWT 解决所有问题。

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="shield-check" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **A. 工作负载凭证**

      用于证明“当前运行的服务或沙箱是谁”。

      推荐：SPIFFE X.509-SVID + mTLS；无法使用 mTLS 的边界，再考虑短期 JWT 或其他受控凭证。

      <text color="secondary" size="sm">签发方：SPIRE 或企业工作负载身份服务。验证方：网关、身份代理或下游服务。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="key-round" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **B. 访问令牌（Access Token）**

      用于证明请求者有权向某个目标服务发起请求，且携带有限的授权上下文。

      推荐：OAuth 2.0 短期、限定受众的 Access Token，采用 JWT 或不透明令牌均可。

      <text color="secondary" size="sm">签发方：Keycloak 或企业授权服务器。验证方：agentgateway、MCP Server、业务 API。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="git-branch" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **C. 委托凭证 / 委托授权记录**

      用于表达某个主体允许 Agent 在限定条件下代表它执行任务。

      推荐：服务端保存 Delegation Grant，并在调用具体资源时签发或交换出受限访问令牌。

      <text color="secondary" size="sm">签发方：经过授权的委托服务。验证方：策略引擎和目标资源服务。</text>
    </box>
  </row>
</box>

SPIFFE 的 X.509-SVID 可以通过证书和受信任的信任域验证工作负载身份；其官方文档建议在适用的场景优先考虑 X.509-SVID，而非容易被复制和重放的 bearer JWT-SVID。<Cite refs={["turn906303search0","turn906303search2","turn906303search3"]}/>

这三种凭证各自解决不同的问题：

- 工作负载凭证回答“请求来自哪个可信执行环境”。
- Access Token 回答“这个请求可以访问哪个目标服务”。
- Delegation Grant 回答“为什么这个 Agent 可以代表某个主体执行任务”。

**特别注意：委托记录不等于访问凭证，工作负载认证成功也不等于业务授权成功。**

## 四、凭证签发与校验流程

### 流程 A：服务端 Agent 启动

以 Java 运维 Agent 为例。

<box gap={2}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2} minWidth="40px" align="center">
      **1**
    </box>
    <box flex="1" gap={1}>
      **平台部署 Agent**

      平台根据已审批的 Deployment 配置启动服务，并将部署与 `agent_id` 绑定。Agent 不允许通过请求参数自行选择任意逻辑身份。
    </box>
  </row>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2} minWidth="40px" align="center">
      **2**
    </box>
    <box flex="1" gap={1}>
      **获取工作负载身份**

      在 Kubernetes 中，SPIRE 可依据受控的 Pod、Service Account 等属性进行工作负载证明；非 Kubernetes 环境则需要配置对应的主机或进程证明机制。不能只凭客户端声称的 SPIFFE ID 签发身份。
    </box>
  </row>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2} minWidth="40px" align="center">
      **3**
    </box>
    <box flex="1" gap={1}>
      **映射至注册中心**

      平台验证该工作负载是否被授权运行 `agt_ops_assistant`，以及它属于哪个 Deployment、环境和租户。
    </box>
  </row>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2} minWidth="40px" align="center">
      **4**
    </box>
    <box flex="1" gap={1}>
      **获取目标访问令牌**

      Agent 使用工作负载身份或专门配置的客户端认证方式，向授权服务器申请面向指定 API 或 MCP 服务的短期 Token。
    </box>
  </row>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2} minWidth="40px" align="center">
      **5**
    </box>
    <box flex="1" gap={1}>
      **目标服务验证并授权**

      验证令牌签名或令牌有效性、`iss`、`aud`、有效期及必要的身份声明，再依据策略判断具体操作是否允许。
    </box>
  </row>
</box>

SPIFFE Workload API 提供了获取 SVID、密钥和信任材料的标准接口；具体如何证明工作负载身份由实现与部署方式决定。<Cite refs={["turn906303search2","turn906303search4"]}/>

### 流程 B：业务用户发起 Agent 任务

这是企业平台中更需要仔细设计的部分。

假设员工 A 要求运维 Agent 查询某台服务器的生产日志。

1. 员工先通过企业 SSO 登录，前端获得面向业务 API 的用户访问令牌。
2. 后端验证用户身份，并创建任务记录，绑定用户、Agent、会话、环境和任务范围。
3. Agent Runtime 使用自己的工作负载身份向平台领取任务。
4. 平台根据用户权限、Agent 权限、任务约束及风险等级，建立有期限的委托授权记录。
5. Agent 调用 MCP Gateway 时，携带面向目标服务的短期访问令牌。
6. MCP Gateway 验证调用方身份和令牌，再根据授权策略判断工具调用是否允许。
7. MCP Server 对目标资源执行必要的二次授权，完成操作后记录审计事件。

其中第 4 步不能仅仅把员工的原始 Token 复制给 Agent。应该让委托关系可验证、可撤销，并且明确限制目标资源和操作范围。

### 流程 C：Agent 调用另一个 Agent

例如，运维 Agent 将日志分析任务委托给日志分析 Agent。

建议把调用链保留为：

```text
user_123
  └── agt_ops_assistant
        └── agt_log_analyzer
              └── tool.search_logs
```

接收方必须能够区分：

- 原始委托主体是谁；
- 当前直接调用者是谁；
- 当前工作负载是什么；
- 当前调用链的委托范围是什么。

可采用 OAuth 2.0 Token Exchange（RFC 8693）交换出面向下游服务的受限 Token，同时在可信审计系统中记录委托链。<Cite refs={["turn906303search1"]}/>

需要注意，RFC 8693 提供的是令牌交换协议，不会自动替你完成权限继承、委托链验证或防止权限扩大。这些规则仍需由企业平台制定和执行。

## 五、Token 应该长什么样？

建议统一访问令牌的核心声明，但按目标服务和使用场景分别签发，不要让所有调用共用一份万能 Token。

例如，下面是一份用于展示身份与委托关系的 JWT 载荷示意：

```json
{
  "iss": "https://id.example.com/realms/enterprise",
  "sub": "agt_ops_assistant",
  "aud": "mcp-gateway",
  "iat": 1792080000,
  "exp": 1792080300,
  "jti": "tok_01JXYZ",
  "client_id": "ops-assistant-runtime",
  "scope": "logs:read",
  "https://corp.example/claims/agent_id": "agt_ops_assistant",
  "https://corp.example/claims/task_id": "task_456",
  "https://corp.example/claims/delegation_id": "dlg_789"
}
```

这是一个**示意载荷，不是可直接用于生产的令牌**。其中时间戳、签名、密钥管理、客户端认证和声明映射都需要实际实现；自定义声明也必须由受信任的签发方生成。

几个重要的设计细节：

- `sub`：表示该 Token 的主体，必须在整个系统中定义清楚其语义。不要有时放用户、有时放 Agent，却没有明确约定。
- `aud`：必须限制目标服务，避免发给 MCP Gateway 的 Token 被拿去调用其他 API。
- `scope`：可以表示粗粒度权限，但数据库、服务器、租户等资源级权限往往还需要进一步判断。
- `task_id` 和 `delegation_id`：用于关联任务及委托记录；它们本身不是权限证明。
- `jti`：用于标识令牌，配合服务端状态或重放防护机制时才有额外安全价值。

如果需要完整表达 OAuth 令牌交换中的委托关系，可以根据 RFC 8693 使用标准的 `act` 声明表达当前代表原始主体行动的 Actor；但实际 Token 格式、嵌套方式和验证规则要与身份提供商及资源服务器的实现保持一致。

不要把完整的权限策略、数据库密码或服务器凭据直接塞进 JWT。令牌应尽可能短小，并且只携带完成授权决策所需的信息。

## 六、委托体系：如何防止 Agent 越权？

我建议把授权拆成两类：**Agent 自身的能力边界**和**本次任务获得的委托权限**。

例如：

| 授权维度 | 运维 Agent 的最大能力 | 本次任务的权限 |
|---|---|---|
| 目标环境 | 测试、生产 | 仅生产 |
| 日志查询 | 允许 | 仅指定应用 |
| 数据库查询 | 只读 | 仅指定数据库及表 |
| 数据库写入 | 禁止 | 禁止 |
| 服务器命令 | 受控白名单 | 仅允许读取日志的命令 |
| 持续时间 | 按平台策略 | 10 分钟后失效 |

本次任务的有效权限应满足：

\[
P_{\text{effective}}
\subseteq
P_{\text{agent}}
\cap
P_{\text{user}}
\cap
P_{\text{task}}
\cap
P_{\text{resource}}
\]

这里的集合交集是一个简化模型：实际策略还需要考虑拒绝规则、环境、资源属性、风险等级和审批要求。

例如，员工有生产日志读取权限，Agent 也被允许执行日志查询，但任务只允许访问应用 A 的日志。那么 Agent 不能利用这个任务去查询应用 B 的日志，更不能借此执行数据库写操作。

### 委托授权记录建议包含哪些字段？

```text
delegation_id
subject_type          // user / service
subject_id
agent_id
task_id
audience              // 目标服务
allowed_actions       // 允许的动作
resource_constraints  // 资源范围
issued_at
expires_at
status                // active / revoked / expired
parent_delegation_id  // 可选：上游委托
```

委托服务在签发访问令牌前，必须检查原始主体是否有权进行这项委托，以及目标 Agent 是否有能力执行相应操作。

**禁止权限扩大：** 下游 Agent 不能因为拥有自己的服务身份，就获得超出上游委托范围的权限。它自己的最大权限和上游委托权限必须同时约束最终结果。

对于高风险操作，例如生产数据库写入、重启服务、修改防火墙，建议增加独立审批或二次确认，而不是完全依赖 Agent 的推理结果。

## 七、独立沙箱 Agent：身份设计要特别处理

这是混合运行环境中最容易出现漏洞的地方。

假设平台为每个用户会话启动一个独立沙箱，沙箱内运行 Agent 代码。不能简单地把长期的企业凭据放进容器环境变量里，否则一旦沙箱被攻破，攻击者可能窃取凭据并在沙箱销毁后继续使用。

建议采用下面的机制。

<box border radius="lg" padding={3} gap={3}>
  <box background="surface-secondary" radius="md" padding={3} align="center" gap={1}>
    **Agent Platform**
    <text color="secondary" size="sm">创建会话、分配身份、限制权限和生命周期</text>
  </box>
  <box align="center">
    <icon name="arrow-down" size="lg" color="secondary"/>
  </box>
  <box border radius="md" padding={3} gap={1} align="center">
    **Sandbox Identity Broker**
    <text color="secondary" size="sm">验证沙箱身份、绑定任务、签发短期凭证</text>
  </box>
  <box align="center">
    <icon name="arrow-down" size="lg" color="secondary"/>
  </box>
  <box background="surface-secondary" radius="md" padding={3} gap={1} align="center">
    **独立沙箱**
    <text color="secondary" size="sm">每个会话或安全隔离单元具有独立运行身份</text>
  </box>
  <box align="center">
    <icon name="arrow-down-up" size="lg" color="secondary"/>
  </box>
  <box border radius="md" padding={3} align="center" gap={1}>
    **agentgateway → MCP Server**
    <text color="secondary" size="sm">验证凭证、校验委托范围、执行资源级授权</text>
  </box>
</box>

我建议执行以下规则：

1. 沙箱创建时，由可信的控制面分配 `sandbox_id`，绑定 `agent_id`、`session_id`、`task_id` 和租户。
2. 沙箱通过受控的身份代理获取短期工作负载凭证，不能自行声明任意身份来申请证书。
3. 每个任务使用受限的短期访问令牌，只允许访问本次任务所需的目标和操作。
4. 沙箱结束、任务取消或风险事件发生时，停止签发新凭证、撤销委托，并清理运行环境。
5. 对需要立即生效的吊销，不能只依赖 JWT 自然过期。需要通过在线授权检查、令牌状态检查或短期凭证等机制缩短风险窗口。

如果沙箱是通过 gVisor、Kubernetes Pod 或其他运行时启动的，底层运行时负责隔离执行环境；身份代理负责证明沙箱的可信来源。二者需要建立可信绑定，不能因为创建了一个独立 Pod 就自动认定它是可信的 Agent。

对于普通服务器上的 Java Agent，可以使用 SPIRE 工作负载身份；对于无法安全接入 SPIRE 的独立沙箱，可以采用由可信宿主代理提供的受限凭证服务，但必须保证沙箱不能冒充其他沙箱申请身份。

## 八、权限应该在哪里真正执行？

这是整个设计的关键。身份体系如果只在入口验证一次，后续的工具服务仍可能被绕过。

| 层次 | 必须执行的控制 |
|---|---|
| Agent Platform API | 验证用户身份、Agent 注册状态、任务归属和委托权限 |
| 身份签发服务 | 验证申请者身份、签发资格、目标受众、委托范围及有效期 |
| agentgateway | 验证请求凭证、限制 MCP 工具访问、执行路由级策略 |
| MCP Server | 校验工具权限、资源范围、参数约束和高风险操作规则 |
| 数据库 / 服务器 | 使用独立的最小权限账号、数据库授权或操作系统权限做最终限制 |
| 审计系统 | 记录身份链、授权结果、工具操作、审批及执行结果 |

agentgateway 的官方文档支持通过 CEL 表达授权策略，并在 MCP 请求上下文中按工具等属性进行访问控制。<Cite refs={["turn906303search6","turn906303search12"]}/>

但有两个需要特别强调的限制：

- 如果所有 MCP 请求都由一个具有超级权限的后端账号执行，仅在网关限制工具名称还不够。必须保证后端账号、资源代理或数据库自身也能落实资源级限制。
- 如果下游服务接受调用方自己提交的 `X-Agent-ID`、`X-User-ID` 等普通 Header 作为身份依据，就会出现伪造身份的风险。身份上下文必须来自经过验证的凭证，或者由可信网关以不可绕过的方式注入，并在网络和服务权限层阻止直接绕过网关。

## 九、审计体系：让每次操作都能追责

建议把身份事件和业务执行事件统一关联起来，至少记录：

```text
event_id
timestamp
tenant_id
principal_id
agent_id
workload_identity
session_id
task_id
delegation_id
target_service
tool_name
resource_id
action
policy_version
authorization_decision
approval_id
execution_result
trace_id
```

敏感工具参数需要根据数据分级进行脱敏或摘要处理，不能为了审计而把所有生产数据、访问令牌和数据库凭据原样写入日志。

同时应做到：

- 审计日志由 Agent 无法修改的独立服务接收。
- 记录授权决策所使用的策略版本，便于事后复现。
- 对身份签发、权限变更、委托建立和吊销保留审计事件。
- 将任务执行记录和模型推理内容区分开，避免不必要地记录敏感思维链或业务数据。
- 对高风险操作记录审批人与最终执行主体。

## 十、如何分阶段落地？

不要一开始就建设复杂的自研身份基础设施。我建议分三期实施。

<box gap={3}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="lg" padding={3} minWidth="76px" align="center" gap={1}>
      <title size="lg">P1</title>
      <text size="xs" color="secondary">基础身份</text>
    </box>
    <box flex="1" gap={1}>
      **统一注册、认证和工具授权**

      - 建立 Agent Registry 和不可变 `agent_id`。
      - 使用 Keycloak 或现有企业 IdP 管理 OAuth 客户端。
      - 为 Agent 服务分配独立身份与短期访问令牌。
      - 在 agentgateway 和 MCP Server 建立基础认证、授权和审计。

      <text color="secondary" size="sm">验收目标：不能通过伪造 Agent ID 或直接绕过网关来获得工具权限。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="lg" padding={3} minWidth="76px" align="center" gap={1}>
      <title size="lg">P2</title>
      <text size="xs" color="secondary">身份委托</text>
    </box>
    <box flex="1" gap={1}>
      **建立用户、Agent、任务三方委托**

      - 实现 Delegation Grant。
      - 实现面向目标服务的受限 Token 签发或令牌交换。
      - 对用户权限、Agent 能力和任务范围进行组合授权。
      - 对 Agent-to-Agent 调用记录完整委托链。

      <text color="secondary" size="sm">验收目标：下游 Agent 无法扩大上游授权范围，任务结束后不能继续使用原有权限。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="lg" padding={3} minWidth="76px" align="center" gap={1}>
      <title size="lg">P3</title>
      <text size="xs" color="secondary">动态身份</text>
    </box>
    <box flex="1" gap={1}>
      **工作负载身份、沙箱和高风险操作控制**

      - 在适用的运行环境接入 SPIFFE/SPIRE。
      - 为沙箱建立身份与会话的可信绑定。
      - 实现动态凭证轮换、紧急吊销和资源级权限隔离。
      - 对生产写操作引入审批、执行约束及独立审计。

      <text color="secondary" size="sm">验收目标：沙箱不能冒充其他 Agent，凭证泄漏不会自动带来无限期、无限范围的访问权限。</text>
    </box>
  </row>
</box>

## 我的最终建议

对你们的企业通用 Agent 平台，我会优先采用以下组合：

- **Keycloak：** 企业用户身份、OAuth 客户端和访问令牌。
- **Agent Registry：** 逻辑 Agent 的唯一身份与生命周期管理。
- **SPIFFE/SPIRE：** 为适用的动态工作负载提供可验证的运行身份。
- **OAuth 2.0 Token Exchange：** 支撑面向下游服务的受限访问令牌及委托链。
- **agentgateway：** 统一通信入口与部分策略执行。
- **OPA 或 Cedar：** 作为可选的集中式授权决策引擎，选一种即可。
- **MCP Server 和资源侧：** 落实最终的工具权限与数据权限。

其中最值得优先确定的架构决策是：**企业平台是否要建立自己的 Delegation Service（委托服务），而不是完全依赖 Keycloak 或 agentgateway。**

我的倾向是建立一个轻量的委托服务，维护任务级授权、委托关系和撤销状态；让 Keycloak、SPIRE 和 agentgateway 各自做好身份签发、工作负载证明和通信策略执行。这样更容易把企业特有的业务权限、Agent 之间的调用关系和沙箱生命周期统一起来，同时避免把整个 Agent 身份体系绑死在某个网关产品上。

## 七、独立沙箱 Agent：身份设计要特别处理

这是混合运行环境中最容易出现漏洞的地方。

假设平台为每个用户会话启动一个独立沙箱，沙箱内运行 Agent 代码。不能简单地把长期的企业凭据放进容器环境变量里，否则一旦沙箱被攻破，攻击者可能窃取凭据并在沙箱销毁后继续使用。

建议采用下面的机制。

<box border radius="lg" padding={3} gap={3}>
  <box background="surface-secondary" radius="md" padding={3} align="center" gap={1}>
    **Agent Platform**
    <text color="secondary" size="sm">创建会话、分配身份、限制权限和生命周期</text>
  </box>
  <box align="center">
    <icon name="arrow-down" size="lg" color="secondary"/>
  </box>
  <box border radius="md" padding={3} gap={1} align="center">
    **Sandbox Identity Broker**
    <text color="secondary" size="sm">验证沙箱身份、绑定任务、签发短期凭证</text>
  </box>
  <box align="center">
    <icon name="arrow-down" size="lg" color="secondary"/>
  </box>
  <box background="surface-secondary" radius="md" padding={3} gap={1} align="center">
    **独立沙箱**
    <text color="secondary" size="sm">每个会话或安全隔离单元具有独立运行身份</text>
  </box>
  <box align="center">
    <icon name="arrow-down-up" size="lg" color="secondary"/>
  </box>
  <box border radius="md" padding={3} align="center" gap={1}>
    **agentgateway → MCP Server**
    <text color="secondary" size="sm">验证凭证、校验委托范围、执行资源级授权</text>
  </box>
</box>

我建议执行以下规则：

1. 沙箱创建时，由可信的控制面分配 `sandbox_id`，绑定 `agent_id`、`session_id`、`task_id` 和租户。
2. 沙箱通过受控的身份代理获取短期工作负载凭证，不能自行声明任意身份来申请证书。
3. 每个任务使用受限的短期访问令牌，只允许访问本次任务所需的目标和操作。
4. 沙箱结束、任务取消或风险事件发生时，停止签发新凭证、撤销委托，并清理运行环境。
5. 对需要立即生效的吊销，不能只依赖 JWT 自然过期。需要通过在线授权检查、令牌状态检查或短期凭证等机制缩短风险窗口。

如果沙箱是通过 gVisor、Kubernetes Pod 或其他运行时启动的，底层运行时负责隔离执行环境；身份代理负责证明沙箱的可信来源。二者需要建立可信绑定，不能因为创建了一个独立 Pod 就自动认定它是可信的 Agent。

对于普通服务器上的 Java Agent，可以使用 SPIRE 工作负载身份；对于无法安全接入 SPIRE 的独立沙箱，可以采用由可信宿主代理提供的受限凭证服务，但必须保证沙箱不能冒充其他沙箱申请身份。

## 八、权限应该在哪里真正执行？

这是整个设计的关键。身份体系如果只在入口验证一次，后续的工具服务仍可能被绕过。

| 层次 | 必须执行的控制 |
|---|---|
| Agent Platform API | 验证用户身份、Agent 注册状态、任务归属和委托权限 |
| 身份签发服务 | 验证申请者身份、签发资格、目标受众、委托范围及有效期 |
| agentgateway | 验证请求凭证、限制 MCP 工具访问、执行路由级策略 |
| MCP Server | 校验工具权限、资源范围、参数约束和高风险操作规则 |
| 数据库 / 服务器 | 使用独立的最小权限账号、数据库授权或操作系统权限做最终限制 |
| 审计系统 | 记录身份链、授权结果、工具操作、审批及执行结果 |

agentgateway 的官方文档支持通过 CEL 表达授权策略，并在 MCP 请求上下文中按工具等属性进行访问控制。<Cite refs={["turn906303search6","turn906303search12"]}/>

但有两个需要特别强调的限制：

- 如果所有 MCP 请求都由一个具有超级权限的后端账号执行，仅在网关限制工具名称还不够。必须保证后端账号、资源代理或数据库自身也能落实资源级限制。
- 如果下游服务接受调用方自己提交的 `X-Agent-ID`、`X-User-ID` 等普通 Header 作为身份依据，就会出现伪造身份的风险。身份上下文必须来自经过验证的凭证，或者由可信网关以不可绕过的方式注入，并在网络和服务权限层阻止直接绕过网关。

## 九、审计体系：让每次操作都能追责

建议把身份事件和业务执行事件统一关联起来，至少记录：

```text
event_id
timestamp
tenant_id
principal_id
agent_id
workload_identity
session_id
task_id
delegation_id
target_service
tool_name
resource_id
action
policy_version
authorization_decision
approval_id
execution_result
trace_id
```

敏感工具参数需要根据数据分级进行脱敏或摘要处理，不能为了审计而把所有生产数据、访问令牌和数据库凭据原样写入日志。

同时应做到：

- 审计日志由 Agent 无法修改的独立服务接收。
- 记录授权决策所使用的策略版本，便于事后复现。
- 对身份签发、权限变更、委托建立和吊销保留审计事件。
- 将任务执行记录和模型推理内容区分开，避免不必要地记录敏感思维链或业务数据。
- 对高风险操作记录审批人与最终执行主体。

## 十、如何分阶段落地？

不要一开始就建设复杂的自研身份基础设施。我建议分三期实施。

<box gap={3}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="lg" padding={3} minWidth="76px" align="center" gap={1}>
      <title size="lg">P1</title>
      <text size="xs" color="secondary">基础身份</text>
    </box>
    <box flex="1" gap={1}>
      **统一注册、认证和工具授权**

      - 建立 Agent Registry 和不可变 `agent_id`。
      - 使用 Keycloak 或现有企业 IdP 管理 OAuth 客户端。
      - 为 Agent 服务分配独立身份与短期访问令牌。
      - 在 agentgateway 和 MCP Server 建立基础认证、授权和审计。

      <text color="secondary" size="sm">验收目标：不能通过伪造 Agent ID 或直接绕过网关来获得工具权限。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="lg" padding={3} minWidth="76px" align="center" gap={1}>
      <title size="lg">P2</title>
      <text size="xs" color="secondary">身份委托</text>
    </box>
    <box flex="1" gap={1}>
      **建立用户、Agent、任务三方委托**

      - 实现 Delegation Grant。
      - 实现面向目标服务的受限 Token 签发或令牌交换。
      - 对用户权限、Agent 能力和任务范围进行组合授权。
      - 对 Agent-to-Agent 调用记录完整委托链。

      <text color="secondary" size="sm">验收目标：下游 Agent 无法扩大上游授权范围，任务结束后不能继续使用原有权限。</text>
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="lg" padding={3} minWidth="76px" align="center" gap={1}>
      <title size="lg">P3</title>
      <text size="xs" color="secondary">动态身份</text>
    </box>
    <box flex="1" gap={1}>
      **工作负载身份、沙箱和高风险操作控制**

      - 在适用的运行环境接入 SPIFFE/SPIRE。
      - 为沙箱建立身份与会话的可信绑定。
      - 实现动态凭证轮换、紧急吊销和资源级权限隔离。
      - 对生产写操作引入审批、执行约束及独立审计。

      <text color="secondary" size="sm">验收目标：沙箱不能冒充其他 Agent，凭证泄漏不会自动带来无限期、无限范围的访问权限。</text>
    </box>
  </row>
</box>

## 我的最终建议

对你们的企业通用 Agent 平台，我会优先采用以下组合：

- **Keycloak：** 企业用户身份、OAuth 客户端和访问令牌。
- **Agent Registry：** 逻辑 Agent 的唯一身份与生命周期管理。
- **SPIFFE/SPIRE：** 为适用的动态工作负载提供可验证的运行身份。
- **OAuth 2.0 Token Exchange：** 支撑面向下游服务的受限访问令牌及委托链。
- **agentgateway：** 统一通信入口与部分策略执行。
- **OPA 或 Cedar：** 作为可选的集中式授权决策引擎，选一种即可。
- **MCP Server 和资源侧：** 落实最终的工具权限与数据权限。

其中最值得优先确定的架构决策是：**企业平台是否要建立自己的 Delegation Service（委托服务），而不是完全依赖 Keycloak 或 agentgateway。**

我的倾向是建立一个轻量的委托服务，维护任务级授权、委托关系和撤销状态；让 Keycloak、SPIRE 和 agentgateway 各自做好身份签发、工作负载证明和通信策略执行。这样更容易把企业特有的业务权限、Agent 之间的调用关系和沙箱生命周期统一起来，同时避免把整个 Agent 身份体系绑死在某个网关产品上。


---

## 👤 用户

我只是一名普通程序员，一些安全领域的专业术语对于我来说太陌生，这段话在说什么 C. 委托凭证 / 委托授权记录
用于表达某个主体允许 Agent 在限定条件下代表它执行任务。
推荐：服务端保存 Delegation Grant，并在调用具体资源时签发或交换出受限访问令牌。


---

## 🤖 ChatGPT

你可以先把它理解成一个很熟悉的场景：**公司让你帮同事查一个问题，同事给了你有限的权限，但不代表你可以拿着他的身份做任何事情。**

我们用你正在开发的运维 Agent 举个例子，一步一步拆开。

## 1. 先看一个实际场景

业务同事小王发现系统报错，于是找运维 Agent 帮忙排查。

小王有权限查看 A 系统的日志，但没有权限修改生产数据库。

<box border radius="lg" padding={3} gap={3} align="center">
  <box background="surface-secondary" radius="lg" padding={3} width="100%" align="center" gap={1}>
    <icon name="user-round" size="xl"/>
    **小王**
    <text color="secondary" size="sm">请求排查 A 系统故障</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg"/>
  <box border radius="lg" padding={3} width="100%" align="center" gap={1}>
    <icon name="bot" size="xl"/>
    **运维 Agent**
    <text color="secondary" size="sm">替小王执行排查任务</text>
  </box>
  <grid columns={2} gap={2}>
    <grid-item>
      <box background="surface" border radius="md" padding={3} gap={1}>
        <icon name="check-circle" color="success" size="lg"/>
        **允许**
        <text size="sm">查询 A 系统日志</text>
      </box>
    </grid-item>
    <grid-item>
      <box background="surface" border radius="md" padding={3} gap={1}>
        <icon name="x-circle" color="danger" size="lg"/>
        **禁止**
        <text size="sm">修改生产数据库</text>
      </box>
    </grid-item>
  </grid>
</box>

问题来了：Agent 是一个程序，它怎么知道自己能做什么、不能做什么？

这就是“委托授权”要解决的问题。

## 2. 什么是 Delegation Grant？

先别管英文，我们把它翻译成：**一张有使用范围和有效期的授权单。**

例如，平台给这次任务创建一条授权记录：

| 字段 | 示例值 | 含义 |
|---|---|---|
| 授权编号 | `grant-001` | 这张授权单的唯一编号 |
| 授权人 | 小王 | 谁允许这次操作 |
| 执行者 | 运维 Agent | 谁来干活 |
| 允许操作 | 查询日志 | 可以干什么 |
| 允许范围 | A 系统 | 可以操作什么 |
| 有效期 | 10 分钟 | 什么时候失效 |
| 状态 | 有效 | 是否已经撤销 |

这条记录就叫 **Delegation Grant（委托授权记录）**。

注意：它不一定是一份文件，也不一定是一个 Token。实际实现中，它可以就是数据库中的一条记录。

比如，你可以在 Java 项目里设计一张表：

```sql
CREATE TABLE agent_delegation (
    id              VARCHAR(64) PRIMARY KEY,
    user_id         VARCHAR(64) NOT NULL,
    agent_id        VARCHAR(64) NOT NULL,
    task_id         VARCHAR(64) NOT NULL,
    resource_scope  VARCHAR(512) NOT NULL,
    allowed_actions VARCHAR(512) NOT NULL,
    expires_at      TIMESTAMP NOT NULL,
    status          VARCHAR(16) NOT NULL
);
```

这只是帮助你理解的数据模型。生产实现还需要考虑权限结构、撤销、并发、审计等问题。

## 3. 那为什么还要有“访问令牌”？

这是原文中比较绕的地方。

假设 Agent 要查询日志，需要经过 MCP Gateway。

Gateway 收到请求后，必须判断：**这个 Agent 凭什么查询 A 系统的日志？**

一种实现方式是让 Agent 携带一个短期访问令牌，也就是 Access Token。

你可以把两者的关系理解成：

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="database" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **Delegation Grant：后台授权记录**

      平台的数据库里记录了：小王允许运维 Agent 在 10 分钟内查询 A 系统的日志。

      它是授权的依据，平台可以检查它是否仍然有效。
    </box>
  </row>
  <divider color="subtle"/>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="ticket" size="xl"/>
    </box>
    <box flex="1" gap={1}>
      **Access Token：访问凭证**

      Agent 每次调用工具时，携带这个凭证，证明自己获得了面向目标服务的访问授权。

      它有有效期和访问范围，不应该允许 Agent 随意修改。
    </box>
  </row>
</box>

它们并不是必须同时存在的两样东西，但在企业平台中，将授权记录和实际访问凭证分开管理，通常更容易实现权限撤销、审计和细粒度控制。

## 4. 原文说的“签发或交换”又是什么意思？

其实就是两种拿到访问凭证的方式。

**方式 A：签发。**

1. Agent 向平台申请执行某个任务。
2. 平台检查小王的权限，以及 Agent 是否被允许执行这类任务。
3. 平台创建授权记录。
4. 平台根据授权结果，生成一张短期访问凭证。
5. Agent 使用凭证调用日志查询工具。

**方式 B：交换。**

Agent 已经持有一张证明自己身份的凭证，但这张凭证不能直接用于调用日志服务。

于是，Agent 把现有凭证交给授权服务器，申请换取一张专门用于日志服务的访问凭证。

就像你进入公司时拿到的工牌，不能直接当成机房门禁卡；你需要根据自己的身份和权限，获得机房的访问许可。

技术上，这种令牌交换可以使用 OAuth 2.0 Token Exchange 标准。

## 5. 作为 Java 程序员，你现在应该怎么理解？

先不用急着学习 OAuth、SPIFFE 这些技术。

你只需要记住下面这条业务流程：

```text
用户发起任务
    ↓
平台检查用户有没有权限
    ↓
平台检查 Agent 有没有执行能力
    ↓
创建本次任务的授权记录
    ↓
Agent 调用 MCP 工具
    ↓
网关或工具服务检查授权
    ↓
允许执行 / 拒绝执行
    ↓
记录审计日志
```

这里最关键的不是 Token，而是**平台不能因为 Agent 是可信程序，就认为它执行的所有操作都是合法的。**

每次操作都应该受到用户权限、Agent 能力和当前任务范围的共同约束。

等你理解了这条业务流程，再去学习如何用 Keycloak 签发 Token、如何用 Java 校验 JWT，以及如何通过 agentgateway 实现工具授权，就会容易很多。

如果你愿意，我们下一步可以直接用 **Java + Spring Boot 设计一个最小可运行的委托授权服务**，先不引入 SPIFFE 这类你暂时不需要的复杂组件。


---

## 👤 用户

我觉着agentregistry更加重要，起码把agent先管理起来


---

## 🤖 ChatGPT

我赞同，而且我觉得这更符合你现在的开发顺序。

**先把 Agent Registry 建起来，让企业里的 Agent 有统一的登记、管理和生命周期，再逐步增加认证、授权、委托能力。**

否则，连企业里到底有哪些 Agent、谁开发的、部署在哪里、能调用什么工具都不知道，就直接设计复杂的身份体系，容易陷入过度设计。

## 一、Agent Registry 应该管理什么？

我建议先把它定位成：**企业 Agent 的统一资产目录和管理中心。**

<box border radius="xl" padding={3} gap={3}>
  <box background="surface-secondary" radius="lg" padding={3} gap={1}>
    <row align="center" gap={2}>
      <icon name="library" size="xl"/>
      **Agent Registry**
    </row>
    <text color="secondary" size="sm">统一登记、查询、审核和管理企业 Agent</text>
  </box>
  <grid columns={2} gap={2}>
    <grid-item>
      <box border radius="md" padding={3} gap={1}>
        <icon name="fingerprint" size="lg"/>
        **身份管理**
        <text color="secondary" size="sm">唯一 ID、名称、描述、所有者</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="md" padding={3} gap={1}>
        <icon name="package" size="lg"/>
        **版本管理**
        <text color="secondary" size="sm">版本、运行时、发布状态</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="md" padding={3} gap={1}>
        <icon name="server" size="lg"/>
        **部署管理**
        <text color="secondary" size="sm">环境、运行位置、实例状态</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="md" padding={3} gap={1}>
        <icon name="wrench" size="lg"/>
        **能力登记**
        <text color="secondary" size="sm">MCP 服务、工具、模型依赖</text>
      </box>
    </grid-item>
  </grid>
</box>

这里有一个重要边界：Registry 记录 Agent 声明了哪些能力、获准使用哪些能力，但**登记了某个工具，不等于已经获得调用权限**。真正调用时仍需经过授权检查。

## 二、第一版的数据模型怎么设计？

我建议不要一开始就建十几张表。先设计下面四张核心表，足以支撑第一阶段。

### 1. `agent_definition`：Agent 基本信息

这是整个 Registry 的核心。

| 字段 | 含义 |
|---|---|
| `agent_id` | 全局唯一、不可变的 Agent ID |
| `name` | Agent 名称 |
| `description` | 用途描述 |
| `owner_team` | 负责团队 |
| `owner_user` | 负责人 |
| `status` | 注册、启用、停用、退役等状态 |
| `created_at` | 创建时间 |
| `updated_at` | 更新时间 |

例如：

```json
{
  "agent_id": "agt_ops_assistant",
  "name": "运维助手",
  "description": "协助业务人员排查系统故障",
  "owner_team": "infrastructure",
  "status": "ACTIVE"
}
```

`agent_id` 创建后不应因改名、升级或重新部署而改变。

### 2. `agent_version`：版本管理

同一个 Agent 可以有多个版本，但只有一个逻辑身份。

| 字段 | 含义 |
|---|---|
| `agent_id` | 所属 Agent |
| `version` | 版本号 |
| `runtime_type` | Java 服务、容器、沙箱等 |
| `artifact_uri` | 部署包或镜像地址 |
| `config_version` | 配置版本 |
| `status` | 开发、测试、已发布、已停用 |

这里要注意：版本记录最好引用不可变的构建产物，例如容器镜像 digest，而不是仅记录一个可以被覆盖的镜像标签。

### 3. `agent_deployment`：部署信息

一个 Agent 可以部署在开发、测试、生产等多个环境。

| 字段 | 含义 |
|---|---|
| `deployment_id` | 部署记录 ID |
| `agent_id` | 逻辑 Agent ID |
| `version` | 当前部署版本 |
| `environment` | 环境 |
| `runtime_location` | 部署平台或集群 |
| `status` | 部署、运行、异常、停止 |

部署记录与实际运行状态最好分开理解：Registry 保存期望部署和部署关系，运行时监控系统提供实时状态，避免把 Registry 变成一个不断写入心跳的监控数据库。

### 4. `agent_capability`：能力登记

描述 Agent 需要或提供哪些能力。

例如：

```json
{
  "agent_id": "agt_ops_assistant",
  "capabilities": [
    "mcp:server-logs",
    "mcp:database-read",
    "repository:search"
  ]
}
```

实际数据库里建议规范化存储，并区分：

- `declared_capabilities`：Agent 声明需要的能力。
- `approved_capabilities`：平台审核批准的能力。
- `effective_permissions`：当前实际生效的权限，由授权系统计算。

不要把这三者混成一个字段。

## 三、Agent Registry 与认证系统是什么关系？

我建议分两步建设。

<box gap={2}>
  <box border radius="lg" padding={3} gap={2}>
    <row align="center" gap={2}>
      <badge color="success">第一阶段</badge>
      **先建 Registry**
    </row>
    - 创建、查询、修改 Agent。
    - 管理负责人、版本和部署。
    - 登记能力及审核状态。
    - 支持启用、停用和退役。
  </box>
  <box align="center">
    <icon name="arrow-down" size="xl" color="secondary"/>
  </box>
  <box border radius="lg" padding={3} gap={2}>
    <row align="center" gap={2}>
      <badge>第二阶段</badge>
      **接入身份与授权**
    </row>
    - 为已登记 Agent 绑定可信运行身份。
    - 通过 Keycloak 等系统签发访问凭证。
    - 将 Registry 中的 Agent 与 OAuth 客户端或 SPIFFE 身份关联。
    - 根据批准的能力配置授权策略。
  </box>
</box>

这样，Registry 不必自己实现密码学认证，也不需要替代 Keycloak。它是企业 Agent 的身份登记源，认证系统负责证明请求者的身份。

## 四、我建议你现在就做的 API

第一版可以围绕这些接口开发：

| 接口 | 用途 |
|---|---|
| `POST /api/agents` | 注册 Agent |
| `GET /api/agents` | 查询、分页 |
| `GET /api/agents/{agentId}` | 查看详情 |
| `PATCH /api/agents/{agentId}` | 修改基本信息 |
| `POST /api/agents/{agentId}/versions` | 登记新版本 |
| `POST /api/agents/{agentId}/deployments` | 登记部署 |
| `POST /api/agents/{agentId}/disable` | 停用 Agent |
| `GET /api/agents/{agentId}/capabilities` | 查看能力清单 |

修改、停用、发布等操作需要进行管理权限检查，并留下审计记录。特别是停用 Agent，后续还应该能够联动阻止它获取新凭证或接受新任务，而不只是修改 Registry 中的状态字段。

**我的建议是先把 Registry 做成一个独立的 Spring Boot 服务，配套一套简单的管理页面和清晰的数据模型。** 暂时不需要引入复杂的分布式身份协议。

等 Registry 的概念和边界稳定之后，再把它与 agentgateway、Keycloak、沙箱运行时连接起来，会更自然。

下一步最值得讨论的是：**Agent Registry 究竟只做“登记目录”，还是同时负责 Agent 的注册审批、发布、部署和运行状态管理？** 这会直接决定第一版的服务边界和数据模型。


---

## 👤 用户

Agent Registry 究竟只做“登记目录”，还是同时负责 Agent 的注册审批、发布、部署和运行状态管理？ 你觉得呢？


---

## 🤖 ChatGPT

我建议：**Agent Registry 不要只做登记目录，但也不要把 Agent 的部署和运行管理全部塞进去。**

对于你们要建设的企业级 Agent 平台，我更倾向于把它做成一个 **Agent 管理中心（Agent Management Plane）**：统一管理 Agent 从创建、审批到发布的整个生命周期，同时把实际部署执行交给独立的部署系统。

这样既能先把 Agent 管理起来，也不会一开始就把系统做得过于复杂。

## 一、我建议的职责划分

<box border radius="xl" padding={4} gap={3}>
  <box background="rgba(59,130,246,0.10)" radius="lg" padding={3} gap={1}>
    <title size="lg">Agent Registry / 管理中心</title>
    <text color="secondary" size="sm">管理 Agent 的身份、定义、版本、审批和生命周期</text>
    <grid columns={2} gap={2}>
      <grid-item>
        <row align="center" gap={2}>
          <icon name="check-circle" color="success" size="sm" />
          Agent 登记与唯一 ID
        </row>
      </grid-item>
      <grid-item>
        <row align="center" gap={2}>
          <icon name="check-circle" color="success" size="sm" />
          版本与配置管理
        </row>
      </grid-item>
      <grid-item>
        <row align="center" gap={2}>
          <icon name="check-circle" color="success" size="sm" />
          注册与发布审批
        </row>
      </grid-item>
      <grid-item>
        <row align="center" gap={2}>
          <icon name="check-circle" color="success" size="sm" />
          权限声明与审批记录
        </row>
      </grid-item>
      <grid-item>
        <row align="center" gap={2}>
          <icon name="check-circle" color="success" size="sm" />
          部署记录与目标状态
        </row>
      </grid-item>
      <grid-item>
        <row align="center" gap={2}>
          <icon name="check-circle" color="success" size="sm" />
          运行状态汇总
        </row>
      </grid-item>
    </grid>
  </box>
  <box align="center" gap={1}>
    <icon name="arrow-down-up" size="xl" color="secondary" />
    <text color="secondary" size="sm">发布指令、部署结果、健康状态</text>
  </box>
  <grid columns={2} gap={3}>
    <grid-item>
      <box border radius="lg" padding={3} gap={2} height="100%">
        <icon name="workflow" size="xl" />
        **部署系统**
        <text size="sm">CI/CD、Kubernetes、沙箱调度器</text>
        <text color="secondary" size="sm">负责真正创建、升级、停止运行实例。</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="lg" padding={3} gap={2} height="100%">
        <icon name="activity" size="xl" />
        **运行监控系统**
        <text size="sm">探活、指标、日志、告警</text>
        <text color="secondary" size="sm">负责采集运行事实并反馈给管理中心。</text>
      </box>
    </grid-item>
  </grid>
</box>

这里有一个关键区别：**Registry 可以管理部署和运行状态，但不必亲自执行部署，也不必亲自采集全部监控数据。**

例如，Registry 可以记录“Agent v1.2 应当部署到生产环境”，而 Kubernetes 或沙箱调度器负责实际启动它。启动成功后，再将实际状态反馈回来。

## 二、把 Agent 生命周期管理起来

我建议先定义以下状态：

| 阶段 | 状态示例 | Registry 负责什么 |
|---|---|---|
| 创建 | 草稿、已登记 | 分配唯一 ID，记录负责人和用途 |
| 审批 | 待审批、已通过、已拒绝 | 管理审批流程和审批记录 |
| 发布 | 待发布、已发布、已下架 | 管理版本及发布状态 |
| 部署 | 待部署、部署中、部署失败 | 发起部署，记录期望状态和结果 |
| 运行 | 健康、异常、未知 | 展示外部系统反馈的实际状态 |
| 停用 | 停用中、已停用 | 发起停止或禁用，保留审计记录 |

注意，**发布成功不等于部署成功，部署成功也不等于运行健康**。这些状态应该分别维护，不能用一个 `status` 字段包办全部。

## 三、数据库怎么设计？

第一阶段，我会优先建设以下几张核心表：

<box border radius="lg" gap={0}>
  <row padding={3} gap={3} align="start">
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="bot" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **`agent_definition`**
      <text color="secondary" size="sm">Agent 基础档案：唯一 ID、名称、用途、所属团队、负责人、生命周期状态。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row padding={3} gap={3} align="start">
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="git-branch" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **`agent_version`**
      <text color="secondary" size="sm">版本号、代码或镜像地址、配置版本、运行时类型、版本审批状态。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row padding={3} gap={3} align="start">
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="clipboard-check" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **`agent_approval`**
      <text color="secondary" size="sm">审批对象、审批人、审批结论、意见和时间。后续可以扩展为多级审批。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row padding={3} gap={3} align="start">
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="container" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **`agent_deployment`**
      <text color="secondary" size="sm">部署目标、版本、环境、部署系统标识、期望状态、实际状态、最近健康检查时间。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row padding={3} gap={3} align="start">
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="shield-check" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **`agent_capability`**
      <text color="secondary" size="sm">Agent 声明需要什么能力、平台批准了什么能力。它不是最终授权凭证。</text>
    </box>
  </row>
</box>

第一阶段不必一口气建设复杂的流程引擎。审批记录、版本发布、部署记录和状态同步先用普通 Spring Boot 服务就可以实现。

## 四、为什么这对你们的企业级 Agent 平台很重要？

结合你们未来可能同时运行服务端 Agent 和独立沙箱 Agent 的情况，Registry 应当让平台随时能回答这些问题：

- **它是谁？** 唯一的 `agent_id`，而不是依靠 Agent 自报名称。
- **谁负责它？** 哪个团队、哪个负责人。
- **它运行的是什么版本？** 哪个代码版本、镜像和配置。
- **它被允许做什么？** 声明的能力与经过审批的能力分别是什么。
- **它在哪里运行？** 哪个环境、哪个部署实例或哪个沙箱会话。
- **它现在是否正常？** 实际运行状态以及最近一次状态更新时间。
- **出了问题能否追溯？** 谁批准了发布、谁发起部署、发生了什么变更。

不过，要特别划清一条边界：**Registry 中登记了某个 Agent，或者审批通过了它的能力，并不意味着它自动获得了访问数据库、服务器或其他 MCP 服务的权限。** 真正调用资源时，仍然需要鉴权和授权系统执行检查。

## 五、我的最终建议

第一阶段就把目标定为：

**Agent Registry = 统一档案 + 生命周期管理 + 审批发布 + 部署状态汇总。**

实际部署由外部部署系统执行；实际权限由独立的授权机制控制；详细日志和监控由运行平台负责。

这样 Registry 不会退化成一个简单的 CRUD 后台，也不会膨胀成一个什么都自己做的巨型平台。

我认为下一步最值得先确定的是：**Agent 的运行实例究竟是什么**。对于普通服务端 Agent，它可能是一个服务实例；对于按会话创建的沙箱 Agent，它可能是一个沙箱会话中的运行实例。这会直接影响 `agent_deployment` 的模型设计。


---

## 👤 用户

nacos能不能做agent注册中心


---

## 🤖 ChatGPT

**能，而且值得认真考虑。** 特别是你们本身就在使用 Java 技术栈、已经有 Nacos 基础设施的情况下，不一定需要从零开发 Agent Registry。

不过，这里有个重要变化：**Nacos 3.x 已经把 AI Registry 作为正式能力方向**，不再只是传统的微服务注册中心。官方文档介绍，它可以管理 Agent、MCP Server、Skill、Prompt 和 AgentSpec 等 AI 资源，也支持相关资源的版本、可见性、生命周期和运行时发现。<Cite refs={["turn888445search2","turn888445search0"]}/>

所以，我们需要区分两件事：

- **传统 Nacos 注册中心**：主要解决服务实例注册、发现、健康状态和配置管理。
- **Nacos AI Registry**：面向 Agent 等 AI 资源的登记、管理与发现。

如果你准备建设企业级 Agent 平台，我会优先评估 Nacos 3.x 的 AI Registry，而不是直接拿传统的微服务注册功能来改造。

**能，而且值得认真考虑。** 特别是你们本身就在使用 Java 技术栈、已经有 Nacos 基础设施的情况下，不一定需要从零开发 Agent Registry。

不过，这里有个重要变化：**Nacos 3.x 已经把 AI Registry 作为正式能力方向**，不再只是传统的微服务注册中心。官方文档介绍，它可以管理 Agent、MCP Server、Skill、Prompt 和 AgentSpec 等 AI 资源，也支持相关资源的版本、可见性、生命周期和运行时发现。<Cite refs={["turn888445search2","turn888445search0"]}/>

所以，我们需要区分两件事：

- **传统 Nacos 注册中心**：主要解决服务实例注册、发现、健康状态和配置管理。
- **Nacos AI Registry**：面向 Agent 等 AI 资源的登记、管理与发现。

如果你准备建设企业级 Agent 平台，我会优先评估 Nacos 3.x 的 AI Registry，而不是直接拿传统的微服务注册功能来改造。

## 二、它能覆盖你之前规划的哪些功能？

| 你需要的能力 | Nacos AI Registry 的适用性 |
|---|---|
| Agent 统一登记、查询、发现 | **适合** |
| Agent 唯一标识 | 支持命名空间内按名称唯一标识 |
| 版本管理 | 支持 AgentCard 多版本及默认发布版本 |
| 测试、生产等环境隔离 | 可用 Namespace 隔离 |
| Agent 运行实例的健康监控 | 需要结合传统服务发现或外部运行监控评估 |
| 企业自定义注册审批流程 | 需要验证现有功能，不应默认认为已满足 |
| Agent 沙箱创建、销毁、资源配额 | 不属于 Agent Registry 的核心职责 |
| 细粒度工具权限、用户委托授权 | 仍需要独立授权机制 |

这里尤其需要注意：Nacos 文档中的 Agent Registry 主要围绕 **A2A AgentCard 的注册、发现、命名空间和版本管理**。它不是自动部署任意 Agent 的运行平台。<Cite refs={["turn535210view0","turn535210view2"]}/>

官方文档：

- [Nacos Agent Registry](https://www.nacos.io/en/docs/latest/manual/user/ai/agent-registry/)
- [Nacos AI Registry 总览](https://www.nacos.io/en/docs/latest/manual/user/ai/ai-registry-overview/)

## 三、如果是我，会怎么落地？

我会采用这个组合，而不是重新造一个完整的注册中心：

<box border radius="xl" padding={3} gap={2} align="center">
  <box background="rgba(59,130,246,0.10)" radius="lg" padding={3} width="100%" align="center">
    **企业 Agent 管理后台**
    <text color="secondary" size="sm">负责人、审批、上架下架、审计、企业自定义字段</text>
  </box>
  <icon name="arrow-down" size="lg" color="secondary" />
  <box border radius="lg" padding={3} width="100%" align="center">
    **Nacos AI Registry**
    <text color="secondary" size="sm">Agent 登记、版本、发现、资源元数据</text>
  </box>
  <icon name="arrow-down" size="lg" color="secondary" />
  <grid columns={2} gap={2}>
    <grid-item>
      <box background="surface-secondary" radius="md" padding={3} align="center" gap={1}>
        <icon name="workflow" size="lg" />
        **Agent Runtime**
        <text color="secondary" size="xs">执行 Agent 任务</text>
      </box>
    </grid-item>
    <grid-item>
      <box background="surface-secondary" radius="md" padding={3} align="center" gap={1}>
        <icon name="shield-check" size="lg" />
        **授权与安全网关**
        <text color="secondary" size="xs">鉴权、权限检查、审计</text>
      </box>
    </grid-item>
  </grid>
</box>

企业管理后台负责你们特有的治理流程，Nacos 负责成熟的注册发现能力；后续如果要接入不同语言、不同运行时的 Agent，也可以通过一层统一的管理 API 隔离底层实现。

**我的建议是先检查你们现有 Nacos 的版本。** 官方文档标明，Agent Registry 从 Nacos 3.1.0 开始提供；如果你们还在使用 Nacos 2.x，就不能直接假设已有这套 AI Registry 能力。<Cite refs={["turn535210view0","turn470555view0"]}/>

如果现有版本允许升级，我会先用 Nacos 完成 Agent 的基础登记和发现，再开发薄薄一层企业管理服务，重点补足审批、权限治理和部署编排，而不是另起炉灶造注册中心。

## 二、它能覆盖你之前规划的哪些功能？

| 你需要的能力 | Nacos AI Registry 的适用性 |
|---|---|
| Agent 统一登记、查询、发现 | **适合** |
| Agent 唯一标识 | 支持命名空间内按名称唯一标识 |
| 版本管理 | 支持 AgentCard 多版本及默认发布版本 |
| 测试、生产等环境隔离 | 可用 Namespace 隔离 |
| Agent 运行实例的健康监控 | 需要结合传统服务发现或外部运行监控评估 |
| 企业自定义注册审批流程 | 需要验证现有功能，不应默认认为已满足 |
| Agent 沙箱创建、销毁、资源配额 | 不属于 Agent Registry 的核心职责 |
| 细粒度工具权限、用户委托授权 | 仍需要独立授权机制 |

这里尤其需要注意：Nacos 文档中的 Agent Registry 主要围绕 **A2A AgentCard 的注册、发现、命名空间和版本管理**。它不是自动部署任意 Agent 的运行平台。<Cite refs={["turn535210view0","turn535210view2"]}/>

官方文档：

- [Nacos Agent Registry](https://www.nacos.io/en/docs/latest/manual/user/ai/agent-registry/)
- [Nacos AI Registry 总览](https://www.nacos.io/en/docs/latest/manual/user/ai/ai-registry-overview/)

## 三、如果是我，会怎么落地？

我会采用这个组合，而不是重新造一个完整的注册中心：

<box border radius="xl" padding={3} gap={2} align="center">
  <box background="rgba(59,130,246,0.10)" radius="lg" padding={3} width="100%" align="center">
    **企业 Agent 管理后台**
    <text color="secondary" size="sm">负责人、审批、上架下架、审计、企业自定义字段</text>
  </box>
  <icon name="arrow-down" size="lg" color="secondary" />
  <box border radius="lg" padding={3} width="100%" align="center">
    **Nacos AI Registry**
    <text color="secondary" size="sm">Agent 登记、版本、发现、资源元数据</text>
  </box>
  <icon name="arrow-down" size="lg" color="secondary" />
  <grid columns={2} gap={2}>
    <grid-item>
      <box background="surface-secondary" radius="md" padding={3} align="center" gap={1}>
        <icon name="workflow" size="lg" />
        **Agent Runtime**
        <text color="secondary" size="xs">执行 Agent 任务</text>
      </box>
    </grid-item>
    <grid-item>
      <box background="surface-secondary" radius="md" padding={3} align="center" gap={1}>
        <icon name="shield-check" size="lg" />
        **授权与安全网关**
        <text color="secondary" size="xs">鉴权、权限检查、审计</text>
      </box>
    </grid-item>
  </grid>
</box>

企业管理后台负责你们特有的治理流程，Nacos 负责成熟的注册发现能力；后续如果要接入不同语言、不同运行时的 Agent，也可以通过一层统一的管理 API 隔离底层实现。

**我的建议是先检查你们现有 Nacos 的版本。** 官方文档标明，Agent Registry 从 Nacos 3.1.0 开始提供；如果你们还在使用 Nacos 2.x，就不能直接假设已有这套 AI Registry 能力。<Cite refs={["turn535210view0","turn470555view0"]}/>

如果现有版本允许升级，我会先用 Nacos 完成 Agent 的基础登记和发现，再开发薄薄一层企业管理服务，重点补足审批、权限治理和部署编排，而不是另起炉灶造注册中心。


---

## 👤 用户

**nacos能解决agent的唯一身份问题吗**


---

## 🤖 ChatGPT

## 四、但它解决的是哪一种“唯一身份”？

Nacos 官方文档明确规定，Agent 的身份由 `namespaceId + agentName` 唯一确定；不同命名空间可以有同名 Agent。<Cite refs={["turn203759search1","turn203759search4"]}/>

这能解决**登记层面的唯一标识**，但不等于解决了身份认证。

| 你要解决的问题 | Nacos 能否单独解决 |
|---|---|
| 平台中唯一定位一个 Agent | 能 |
| 区分同一 Agent 的不同版本 | 能 |
| 判断当前调用者是不是这个 Agent | 不能仅靠注册记录 |
| 防止其他服务冒充这个 Agent | 需要认证凭证和服务端校验 |
| 判断这个 Agent 能否访问某个 MCP 或数据库 | 需要授权策略 |

举个例子：你注册了一个 `ops-agent`。

- Nacos 可以确认：`prod` 命名空间下的 `ops-agent` 是哪个登记对象。
- 但某个恶意服务如果自己在请求头中填写 `Agent-ID: ops-agent`，**不能因为它填了这个 ID，就认定它真的有权代表这个 Agent 调用服务器工具。**

## 五、对你们平台，我建议这样做

1. **先让 Nacos 负责 Agent 的唯一登记身份**，例如 `namespaceId + agentName`。
2. 企业自己的 Agent 管理服务维护负责人、审批记录、业务归属等治理信息，并关联 Nacos 身份。
3. 后续引入认证时，为实际运行的 Agent 服务或沙箱实例分配可验证的凭证。
4. 调用 MCP、服务器和数据库工具时，验证调用者身份，再检查它被允许执行的操作。

这里我还建议你提前做一个设计决定：如果未来 Agent 会频繁创建和销毁沙箱实例，**不要把每个运行实例都当成一个新的逻辑 Agent**。例如，`ops-agent` 是逻辑身份，而它在某个任务中启动的沙箱实例应有独立的 `instance_id` 或 `session_id`，并关联回 `ops-agent`。

这样你就同时拥有了稳定的 Agent 身份和可追踪的运行实例身份，不会把注册中心变成动态实例的杂乱集合。


---

## 👤 用户

Agent 沙箱创建、销毁、资源配额。这个属于传统nacos的定位吗


---

## 🤖 ChatGPT

不是。**Agent 沙箱的创建、销毁和资源配额，不属于传统 Nacos 注册中心的核心职责。**

你可以把它们理解为三个不同层次：

- **Nacos**：负责登记和发现服务，知道某个 Agent 或服务在哪里。
- **沙箱管理平台**：负责创建、销毁、暂停和恢复 Agent 的隔离运行环境。
- **容器编排与资源管理系统**：负责 CPU、内存、存储等资源的分配与限制。

它们可以协同工作，但不应该把职责混在一起。

不是。**Agent 沙箱的创建、销毁和资源配额，不属于传统 Nacos 注册中心的核心职责。**

你可以把它们理解为三个不同层次：

- **Nacos**：负责登记和发现服务，知道某个 Agent 或服务在哪里。
- **沙箱管理平台**：负责创建、销毁、暂停和恢复 Agent 的隔离运行环境。
- **容器编排与资源管理系统**：负责 CPU、内存、存储等资源的分配与限制。

它们可以协同工作，但不应该把职责混在一起。

## 一、这三个能力应该由谁负责？

| 能力 | 建议归属 | 具体职责 |
|---|---|---|
| Agent 注册、发现 | Nacos AI Registry | 知道有哪些 Agent、版本和调用入口 |
| 沙箱创建、销毁 | **Sandbox Manager** | 接收创建请求，管理沙箱生命周期 |
| CPU、内存、存储配额 | Kubernetes / 容器运行时 | 对实际资源使用施加限制 |
| 沙箱安全隔离 | gVisor 等隔离运行时 + 容器安全策略 | 降低沙箱逃逸和越权访问风险 |
| Agent 会话与沙箱关联 | Agent 平台 | 确保会话、任务和沙箱实例能够相互追踪 |

Nacos 的核心定位是服务发现、配置管理和 AI 资源登记；它不负责直接编排沙箱生命周期。<Cite refs={["turn353036search0","turn353036search2"]}/>

如果你想参考一个更贴近这个场景的开源项目，可以看 Kubernetes SIG 的 [Agent Sandbox](https://agent-sandbox.sigs.k8s.io/docs/)。它提供了面向有状态、隔离运行环境的 Kubernetes 原生管理机制，并支持模板、沙箱申请和预热池等能力。<Cite refs={["turn353036search7"]}/>

## 二、结合你的企业 Agent 平台，我会这样设计

<box border radius="xl" padding={3} gap={2} align="center">
  <box background="rgba(59,130,246,0.10)" radius="lg" padding={3} width="100%" align="center">
    **Agent 平台 / Sandbox Manager**
    <text color="secondary" size="sm">鉴权、创建请求、生命周期、超时回收、会话关联</text>
  </box>
  <icon name="arrow-down" size="lg" color="secondary" />
  <box border radius="lg" padding={3} width="100%" align="center">
    **Kubernetes / 沙箱调度层**
    <text color="secondary" size="sm">Pod 调度、CPU/内存限制、存储、网络策略</text>
  </box>
  <icon name="arrow-down" size="lg" color="secondary" />
  <box background="surface-secondary" radius="lg" padding={3} width="100%" align="center">
    **隔离运行时**
    <text color="secondary" size="sm">例如 gVisor，提供更强的执行隔离</text>
  </box>
  <divider />
  <row align="center" gap={2}>
    <icon name="database" size="lg" />
    <box flex="1" gap={1}>
      **Nacos AI Registry**
      <text color="secondary" size="sm">作为旁路的 Agent 登记与发现服务，不负责创建底层沙箱。</text>
    </box>
  </row>
</box>

举个例子：业务同事开启一个运维 Agent 会话，平台先检查权限和资源配额，再要求 Sandbox Manager 创建隔离环境。环境启动后，平台记录 `session_id`、`sandbox_id` 和 `agent_id` 的关联；会话结束或超时后，Sandbox Manager 负责回收环境。

**我的建议：先用 Nacos 管 Agent，用独立的 Sandbox Manager 管沙箱。** 以后如果你们决定使用 Kubernetes，可以评估 Agent Sandbox；如果暂时不使用 Kubernetes，也可以让 Sandbox Manager 对接容器运行时或其他沙箱后端。这样不会让整个 Agent 平台被某一种基础设施绑定。

## 一、这三个能力应该由谁负责？

| 能力 | 建议归属 | 具体职责 |
|---|---|---|
| Agent 注册、发现 | Nacos AI Registry | 知道有哪些 Agent、版本和调用入口 |
| 沙箱创建、销毁 | **Sandbox Manager** | 接收创建请求，管理沙箱生命周期 |
| CPU、内存、存储配额 | Kubernetes / 容器运行时 | 对实际资源使用施加限制 |
| 沙箱安全隔离 | gVisor 等隔离运行时 + 容器安全策略 | 降低沙箱逃逸和越权访问风险 |
| Agent 会话与沙箱关联 | Agent 平台 | 确保会话、任务和沙箱实例能够相互追踪 |

Nacos 的核心定位是服务发现、配置管理和 AI 资源登记；它不负责直接编排沙箱生命周期。<Cite refs={["turn353036search0","turn353036search2"]}/>

如果你想参考一个更贴近这个场景的开源项目，可以看 Kubernetes SIG 的 [Agent Sandbox](https://agent-sandbox.sigs.k8s.io/docs/)。它提供了面向有状态、隔离运行环境的 Kubernetes 原生管理机制，并支持模板、沙箱申请和预热池等能力。<Cite refs={["turn353036search7"]}/>

## 二、结合你的企业 Agent 平台，我会这样设计

<box border radius="xl" padding={3} gap={2} align="center">
  <box background="rgba(59,130,246,0.10)" radius="lg" padding={3} width="100%" align="center">
    **Agent 平台 / Sandbox Manager**
    <text color="secondary" size="sm">鉴权、创建请求、生命周期、超时回收、会话关联</text>
  </box>
  <icon name="arrow-down" size="lg" color="secondary" />
  <box border radius="lg" padding={3} width="100%" align="center">
    **Kubernetes / 沙箱调度层**
    <text color="secondary" size="sm">Pod 调度、CPU/内存限制、存储、网络策略</text>
  </box>
  <icon name="arrow-down" size="lg" color="secondary" />
  <box background="surface-secondary" radius="lg" padding={3} width="100%" align="center">
    **隔离运行时**
    <text color="secondary" size="sm">例如 gVisor，提供更强的执行隔离</text>
  </box>
  <divider />
  <row align="center" gap={2}>
    <icon name="database" size="lg" />
    <box flex="1" gap={1}>
      **Nacos AI Registry**
      <text color="secondary" size="sm">作为旁路的 Agent 登记与发现服务，不负责创建底层沙箱。</text>
    </box>
  </row>
</box>

举个例子：业务同事开启一个运维 Agent 会话，平台先检查权限和资源配额，再要求 Sandbox Manager 创建隔离环境。环境启动后，平台记录 `session_id`、`sandbox_id` 和 `agent_id` 的关联；会话结束或超时后，Sandbox Manager 负责回收环境。

**我的建议：先用 Nacos 管 Agent，用独立的 Sandbox Manager 管沙箱。** 以后如果你们决定使用 Kubernetes，可以评估 Agent Sandbox；如果暂时不使用 Kubernetes，也可以让 Sandbox Manager 对接容器运行时或其他沙箱后端。这样不会让整个 Agent 平台被某一种基础设施绑定。


---

## 👤 用户

我们现在回到安全的问题，你说 “我的倾向是建立一个轻量的委托服务，维护任务级授权、委托关系和撤销状态；让 Keycloak、SPIRE 和 agentgateway 各自做好身份签发、工作负载证明和通信策略执行。这样更容易把企业特有的业务权限、Agent 之间的调用关系和沙箱生命周期统一起来，同时避免把整个 Agent 身份体系绑死在某个网关产品上。” 你来帮我做设计吧，我要建立这个


---

## 🤖 ChatGPT

我建议把它建设成一个独立的 **Agent 委托授权服务（Agent Delegation Service）**，作为企业 Agent 平台的安全核心组件之一。

我们先不急着堆 Keycloak、SPIRE、agentgateway 这些基础设施，而是把最关键的业务规则设计清楚：**谁可以授权哪个 Agent，在什么任务范围内，代表谁访问哪些资源、执行哪些操作，以及什么时候必须停止。**

结合你们的技术背景，我建议第一版采用 Java + Spring Boot 实现，先把委托关系、授权校验、撤销和审计做好，再逐步接入身份基础设施和通信网关。

## 一、先明确这个服务要解决什么问题

举个你正在建设的运维 Agent 场景。

业务同事小王发现一个页面报错，向运维 Agent 求助。Agent 需要查看应用日志、查询数据库，甚至可能执行一些修复操作。

这里不能因为 Agent 已经通过身份认证，就允许它访问所有服务器和数据库。平台需要额外回答：

- 谁发起了任务？
- 这个人是否有权请求这类操作？
- 这个 Agent 是否被批准执行这些操作？
- 本次任务具体允许访问哪些系统、数据和工具？
- 任务结束、超时或被撤销后，Agent 是否会立即失去这部分权限？

因此，我们要分清三个概念：

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="fingerprint" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **身份认证：你是谁？**
      <text color="secondary" size="sm">由 Keycloak、工作负载身份等机制验证用户和 Agent 的身份。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="file-check" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **委托授权：你这次被允许做什么？**
      <text color="secondary" size="sm">由我们要建设的服务维护任务级授权、资源范围、有效期和撤销状态。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="shield-check" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **执行拦截：实际调用是否合规？**
      <text color="secondary" size="sm">由 agentgateway、MCP 服务端和目标资源的授权检查共同执行，不能只靠 Agent 自觉遵守。</text>
    </box>
  </row>
</box>

这里有个重要原则：**委托服务负责决定和记录授权，不应成为所有业务请求都必须经过的单点代理。** 实际调用可以由网关或目标服务执行校验，但必须使用可信的身份凭证，并且不能只相信 Agent 自己提交的 `agent_id`。

## 二、整体架构：把委托授权做成独立服务

我建议先把系统拆成五个组件。它们可以部署在不同进程中，但第一阶段不必每个都单独开发。

<box border radius="xl" padding={3} gap={3} align="center">
  <box border radius="lg" padding={3} width="100%" align="center" gap={1}>
    <icon name="user-round" size="xl" />
    **业务用户 / 上游 Agent**
    <text color="secondary" size="sm">发起任务，例如排查某个应用的故障</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg" />
  <box background="rgba(59,130,246,0.10)" radius="lg" padding={3} width="100%" align="center" gap={1}>
    **Agent Delegation Service**
    <text color="secondary" size="sm">创建委托、检查授权范围、撤销授权、记录审计</text>
  </box>
  <grid columns={2} gap={2}>
    <grid-item>
      <box border radius="lg" padding={3} gap={1} height="100%">
        <icon name="database" size="lg" />
        **委托数据库**
        <text color="secondary" size="sm">授权范围、有效期、状态、调用关系</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="lg" padding={3} gap={1} height="100%">
        <icon name="key-round" size="lg" />
        **身份基础设施**
        <text color="secondary" size="sm">Keycloak 验证用户；后续可接入 SPIRE 验证工作负载</text>
      </box>
    </grid-item>
  </grid>
  <icon name="arrow-down" color="secondary" size="lg" />
  <box background="rgba(16,185,129,0.09)" radius="lg" padding={3} width="100%" align="center" gap={1}>
    **调用执行层**
    <text color="secondary" size="sm">agentgateway → MCP Server → 服务器 / 数据库 / 业务系统</text>
    <text color="secondary" size="sm">在实际执行点验证身份、委托和操作权限</text>
  </box>
</box>

各组件的边界如下：

- **Keycloak**：验证用户身份，并提供用户或服务的访问凭证。
- **SPIRE / SPIFFE**：在需要时证明运行中的服务或沙箱实例的工作负载身份，并为服务间安全通信提供身份基础。<Cite refs={["turn684803search3","turn684803search4"]}/>
- **Agent Delegation Service**：维护企业特有的任务授权关系，这是我们要建设的核心。
- **agentgateway**：拦截请求，执行通信和授权策略；它支持针对 MCP 工具等请求实施授权规则，也支持接入外部授权服务。<Cite refs={["turn684803search1","turn684803search11"]}/>
- **MCP Server / 目标系统**：对高风险操作进行最终权限检查，避免有人绕过网关直接调用后端。

## 三、先设计核心数据模型

第一版我建议至少有四张业务表。以下是概念设计，不是最终 DDL。

### 1. `delegation_grant`：委托主表

一条记录表示一次授权，例如“允许运维 Agent 在 30 分钟内排查应用 A”。

| 字段 | 含义 |
|---|---|
| `grant_id` | 全局唯一委托 ID |
| `grantor_subject` | 授权主体，例如用户 ID |
| `agent_id` | 被授权的逻辑 Agent |
| `task_id` | 本次任务 ID |
| `status` | ACTIVE、REVOKED、EXPIRED |
| `valid_from` / `expires_at` | 生效和过期时间 |
| `created_at` / `revoked_at` | 创建和撤销时间 |

`grantor_subject` 不应只存一个显示名称，而应保存可由身份系统稳定识别的主体标识，例如 `issuer + subject`。

### 2. `delegation_scope`：委托范围

描述这次委托**具体允许访问什么、执行什么**。

| 字段 | 示例 |
|---|---|
| `grant_id` | `dg-123` |
| `resource_type` | `application` |
| `resource_id` | `app-A` |
| `action` | `logs.read` |
| `constraints` | 只允许查询最近 30 分钟的日志 |

资源和操作必须采用平台可校验的结构化标识，不要只保存“允许排查故障”这样的自然语言描述。

### 3. `delegation_actor`：参与调用的主体

用来表示一次任务中的调用链关系。

例如：

`用户 → ops-agent → log-agent → logs-mcp`

记录每个参与者的身份、角色、父级调用者，以及它是否被允许参与这条调用链。

### 4. `delegation_audit`：授权审计

记录委托创建、批准、使用、拒绝和撤销等事件。审计记录应追加写入，并限制普通业务服务修改或删除历史记录的能力。

另外，Agent 的注册信息、版本和基础能力应继续由 Agent Registry 管理，不必在委托服务中重复维护一份完整的 Agent 档案。

我建议把它建设成一个独立的 **Agent 委托授权服务（Agent Delegation Service）**，作为企业 Agent 平台的安全核心组件之一。

我们先不急着堆 Keycloak、SPIRE、agentgateway 这些基础设施，而是把最关键的业务规则设计清楚：**谁可以授权哪个 Agent，在什么任务范围内，代表谁访问哪些资源、执行哪些操作，以及什么时候必须停止。**

结合你们的技术背景，我建议第一版采用 Java + Spring Boot 实现，先把委托关系、授权校验、撤销和审计做好，再逐步接入身份基础设施和通信网关。

## 一、先明确这个服务要解决什么问题

举个你正在建设的运维 Agent 场景。

业务同事小王发现一个页面报错，向运维 Agent 求助。Agent 需要查看应用日志、查询数据库，甚至可能执行一些修复操作。

这里不能因为 Agent 已经通过身份认证，就允许它访问所有服务器和数据库。平台需要额外回答：

- 谁发起了任务？
- 这个人是否有权请求这类操作？
- 这个 Agent 是否被批准执行这些操作？
- 本次任务具体允许访问哪些系统、数据和工具？
- 任务结束、超时或被撤销后，Agent 是否会立即失去这部分权限？

因此，我们要分清三个概念：

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="fingerprint" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **身份认证：你是谁？**
      <text color="secondary" size="sm">由 Keycloak、工作负载身份等机制验证用户和 Agent 的身份。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="file-check" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **委托授权：你这次被允许做什么？**
      <text color="secondary" size="sm">由我们要建设的服务维护任务级授权、资源范围、有效期和撤销状态。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="shield-check" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **执行拦截：实际调用是否合规？**
      <text color="secondary" size="sm">由 agentgateway、MCP 服务端和目标资源的授权检查共同执行，不能只靠 Agent 自觉遵守。</text>
    </box>
  </row>
</box>

这里有个重要原则：**委托服务负责决定和记录授权，不应成为所有业务请求都必须经过的单点代理。** 实际调用可以由网关或目标服务执行校验，但必须使用可信的身份凭证，并且不能只相信 Agent 自己提交的 `agent_id`。

## 二、整体架构：把委托授权做成独立服务

我建议先把系统拆成五个组件。它们可以部署在不同进程中，但第一阶段不必每个都单独开发。

<box border radius="xl" padding={3} gap={3} align="center">
  <box border radius="lg" padding={3} width="100%" align="center" gap={1}>
    <icon name="user-round" size="xl" />
    **业务用户 / 上游 Agent**
    <text color="secondary" size="sm">发起任务，例如排查某个应用的故障</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg" />
  <box background="rgba(59,130,246,0.10)" radius="lg" padding={3} width="100%" align="center" gap={1}>
    **Agent Delegation Service**
    <text color="secondary" size="sm">创建委托、检查授权范围、撤销授权、记录审计</text>
  </box>
  <grid columns={2} gap={2}>
    <grid-item>
      <box border radius="lg" padding={3} gap={1} height="100%">
        <icon name="database" size="lg" />
        **委托数据库**
        <text color="secondary" size="sm">授权范围、有效期、状态、调用关系</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="lg" padding={3} gap={1} height="100%">
        <icon name="key-round" size="lg" />
        **身份基础设施**
        <text color="secondary" size="sm">Keycloak 验证用户；后续可接入 SPIRE 验证工作负载</text>
      </box>
    </grid-item>
  </grid>
  <icon name="arrow-down" color="secondary" size="lg" />
  <box background="rgba(16,185,129,0.09)" radius="lg" padding={3} width="100%" align="center" gap={1}>
    **调用执行层**
    <text color="secondary" size="sm">agentgateway → MCP Server → 服务器 / 数据库 / 业务系统</text>
    <text color="secondary" size="sm">在实际执行点验证身份、委托和操作权限</text>
  </box>
</box>

各组件的边界如下：

- **Keycloak**：验证用户身份，并提供用户或服务的访问凭证。
- **SPIRE / SPIFFE**：在需要时证明运行中的服务或沙箱实例的工作负载身份，并为服务间安全通信提供身份基础。<Cite refs={["turn684803search3","turn684803search4"]}/>
- **Agent Delegation Service**：维护企业特有的任务授权关系，这是我们要建设的核心。
- **agentgateway**：拦截请求，执行通信和授权策略；它支持针对 MCP 工具等请求实施授权规则，也支持接入外部授权服务。<Cite refs={["turn684803search1","turn684803search11"]}/>
- **MCP Server / 目标系统**：对高风险操作进行最终权限检查，避免有人绕过网关直接调用后端。

## 三、先设计核心数据模型

第一版我建议至少有四张业务表。以下是概念设计，不是最终 DDL。

### 1. `delegation_grant`：委托主表

一条记录表示一次授权，例如“允许运维 Agent 在 30 分钟内排查应用 A”。

| 字段 | 含义 |
|---|---|
| `grant_id` | 全局唯一委托 ID |
| `grantor_subject` | 授权主体，例如用户 ID |
| `agent_id` | 被授权的逻辑 Agent |
| `task_id` | 本次任务 ID |
| `status` | ACTIVE、REVOKED、EXPIRED |
| `valid_from` / `expires_at` | 生效和过期时间 |
| `created_at` / `revoked_at` | 创建和撤销时间 |

`grantor_subject` 不应只存一个显示名称，而应保存可由身份系统稳定识别的主体标识，例如 `issuer + subject`。

### 2. `delegation_scope`：委托范围

描述这次委托**具体允许访问什么、执行什么**。

| 字段 | 示例 |
|---|---|
| `grant_id` | `dg-123` |
| `resource_type` | `application` |
| `resource_id` | `app-A` |
| `action` | `logs.read` |
| `constraints` | 只允许查询最近 30 分钟的日志 |

资源和操作必须采用平台可校验的结构化标识，不要只保存“允许排查故障”这样的自然语言描述。

### 3. `delegation_actor`：参与调用的主体

用来表示一次任务中的调用链关系。

例如：

`用户 → ops-agent → log-agent → logs-mcp`

记录每个参与者的身份、角色、父级调用者，以及它是否被允许参与这条调用链。

### 4. `delegation_audit`：授权审计

记录委托创建、批准、使用、拒绝和撤销等事件。审计记录应追加写入，并限制普通业务服务修改或删除历史记录的能力。

另外，Agent 的注册信息、版本和基础能力应继续由 Agent Registry 管理，不必在委托服务中重复维护一份完整的 Agent 档案。

## 四、授权流程应该怎么运行？

我们用一个完整例子走一遍。

业务同事申请排查 `app-A` 的页面报错，运维 Agent 需要查询日志。

<CodeBlock language="mermaid">
sequenceDiagram
    participant U as 用户
    participant A as 运维 Agent
    participant D as 委托授权服务
    participant G as agentgateway
    participant M as 日志 MCP

    U->>A: 请求排查 app-A
    A->>D: 申请任务级委托
    D->>D: 检查用户权限、Agent能力与资源范围
    D-->>A: 返回 grant_id
    A->>G: 请求 logs.read，携带可信凭证和任务标识
    G->>D: 校验委托是否有效、范围是否匹配
    D-->>G: 允许或拒绝
    alt 授权通过
        G->>M: 转发受控请求
        M->>M: 校验调用身份与资源权限
        M-->>G: 返回日志
        G-->>A: 返回结果
    else 授权失败
        G-->>A: 拒绝请求
    end
</CodeBlock>

这里有三个必须坚持的安全规则：

1. **不能扩大用户权限。** 用户只能委托自己有权使用的资源和操作。
2. **不能扩大 Agent 权限。** 即使用户允许查询日志，Agent 也必须具有平台审批通过的日志查询能力。
3. **不能只在任务开始时检查一次。** 任务运行期间，每次敏感工具调用都要校验当前授权是否仍然有效。

如果任务中途被撤销，下一次调用就必须被拒绝；对于已经开始执行的长时间操作，还需要设计取消机制。

## 五、委托服务提供什么 API？

第一版可以采用普通 Spring Boot REST API。

| 接口 | 用途 |
|---|---|
| `POST /api/delegations` | 申请创建委托 |
| `GET /api/delegations/{grantId}` | 查询委托详情 |
| `POST /api/delegations/{grantId}/revoke` | 撤销委托 |
| `POST /api/authorizations/check` | 判断一次具体调用是否允许 |
| `GET /api/delegations/{grantId}/audit` | 查询授权审计事件 |

例如，网关要检查 Agent 是否可以查询应用 A 的日志，可以发送这样的授权请求：

```json
{
  "grantId": "dg-123",
  "agentId": "ops-agent",
  "taskId": "task-456",
  "resource": "application:app-A",
  "action": "logs.read"
}
```

这只是授权检查所需的数据示例。生产实现中，`agentId`、调用者身份和任务归属必须来自已验证的凭证或可信的服务端上下文，不能直接相信请求体中的字段。

返回值可以采用 `ALLOW` 或 `DENY`，并附带内部原因码。不要把内部敏感信息暴露给调用方。

## 六、授权凭证怎么设计？

这是整个方案里比较容易踩坑的地方。

我建议第一版**先不自行设计复杂的 Token Exchange 协议**，而是把委托记录与调用凭证分开。

- `grant_id`：委托记录的引用，不是秘密，也不能单独作为授权凭证。
- 工作负载凭证：证明请求来自哪个服务或 Agent 运行环境。
- 任务级访问凭证：在需要时证明这次请求对应哪项委托，以及允许访问的范围。

初期可以让网关通过受保护的内部接口在线查询委托服务。这样撤销后能够较快生效，不必一开始就解决所有 Token 撤销和分发问题。

后续如果需要签发短期访问令牌，再考虑采用标准令牌交换机制。无论哪种方案，**令牌有效不代表委托必然有效**：敏感调用还要考虑撤销状态、令牌受众、有效期和授权范围。

## 七、第一阶段应该做哪些，不做哪些？

我建议把 MVP 控制在以下范围：

<box border radius="lg" padding={3} gap={2}>
  {@body const items = [
    "建立委托主表、范围表和审计表",
    "接入现有用户身份，识别可信的调用主体",
    "实现委托申请、查询、撤销接口",
    "实现任务级授权检查接口",
    "接入一个 agentgateway 路由和一个只读日志 MCP",
    "覆盖越权访问、过期、撤销和伪造身份的测试"
  ]}
  {@body const [done,setDone] = DIL.useState([])}
  <row align="center" justify="between">
    <title size="md">MVP 实施清单</title>
    <text color="secondary" size="sm">{done.length}/{items.length}</text>
  </row>
  {#each items as item,i}
    <checkbox key={item} checked={done.includes(i)} onChange={v=>setDone(prev=>v?[...prev,i]:prev.filter(x=>x!==i))}>{item}</checkbox>
  {/each}
</box>

第一阶段暂时不必引入 SPIRE，也不必实现 Agent 之间任意多级委托。先把用户到 Agent、Agent 到 MCP 的一跳委托做安全、可验证，再扩展多 Agent 调用链和沙箱身份绑定。

---

## 八、我建议接下来先确定一个关键设计

**谁有权创建一条委托？**

这会决定委托服务的权限模型和后续实现复杂度。

<box border radius="lg" padding={3} gap={3}>
  {@body const [mode,setMode] = DIL.useState("user")}
  <radio-group value={mode} onChange={setMode} direction="col">
    <radio value="user">
      **模式 A：用户授权为主**
      <text color="secondary" size="sm">用户发起任务，服务端检查用户权限和 Agent 能力后创建委托。适合先落地运维助手。</text>
    </radio>
    <radio value="platform">
      **模式 B：平台策略自动授权**
      <text color="secondary" size="sm">平台根据预先审批的策略自动创建委托，用户不需要逐项授权。适合固定业务流程和自动化任务。</text>
    </radio>
    <radio value="hybrid">
      **模式 C：两者结合**
      <text color="secondary" size="sm">普通只读操作自动授权，高风险操作要求额外审批或人工确认。更适合逐步演进的企业平台。</text>
    </radio>
  </radio-group>
  <button block onClick={() => GenUI.issueNewTurn("Agent Delegation Service 设计继续。委托创建授权模式选择："+({"user":"模式 A：用户授权为主","platform":"模式 B：平台策略自动授权","hybrid":"模式 C：普通只读操作自动授权，高风险操作额外审批或人工确认"}[mode])+"。请据此细化权限模型、数据库和 Spring Boot 实现。")}>按此模式继续设计</button>
</box>

我的倾向是 **模式 C**，但可以先以模式 A 的最小闭环实现，再逐步增加自动授权规则。这样不会一开始就把企业权限治理做得过于复杂。

## 四、授权流程应该怎么运行？

我们用一个完整例子走一遍。

业务同事申请排查 `app-A` 的页面报错，运维 Agent 需要查询日志。

<CodeBlock language="mermaid">
sequenceDiagram
    participant U as 用户
    participant A as 运维 Agent
    participant D as 委托授权服务
    participant G as agentgateway
    participant M as 日志 MCP

    U->>A: 请求排查 app-A
    A->>D: 申请任务级委托
    D->>D: 检查用户权限、Agent能力与资源范围
    D-->>A: 返回 grant_id
    A->>G: 请求 logs.read，携带可信凭证和任务标识
    G->>D: 校验委托是否有效、范围是否匹配
    D-->>G: 允许或拒绝
    alt 授权通过
        G->>M: 转发受控请求
        M->>M: 校验调用身份与资源权限
        M-->>G: 返回日志
        G-->>A: 返回结果
    else 授权失败
        G-->>A: 拒绝请求
    end
</CodeBlock>

这里有三个必须坚持的安全规则：

1. **不能扩大用户权限。** 用户只能委托自己有权使用的资源和操作。
2. **不能扩大 Agent 权限。** 即使用户允许查询日志，Agent 也必须具有平台审批通过的日志查询能力。
3. **不能只在任务开始时检查一次。** 任务运行期间，每次敏感工具调用都要校验当前授权是否仍然有效。

如果任务中途被撤销，下一次调用就必须被拒绝；对于已经开始执行的长时间操作，还需要设计取消机制。

## 五、委托服务提供什么 API？

第一版可以采用普通 Spring Boot REST API。

| 接口 | 用途 |
|---|---|
| `POST /api/delegations` | 申请创建委托 |
| `GET /api/delegations/{grantId}` | 查询委托详情 |
| `POST /api/delegations/{grantId}/revoke` | 撤销委托 |
| `POST /api/authorizations/check` | 判断一次具体调用是否允许 |
| `GET /api/delegations/{grantId}/audit` | 查询授权审计事件 |

例如，网关要检查 Agent 是否可以查询应用 A 的日志，可以发送这样的授权请求：

```json
{
  "grantId": "dg-123",
  "agentId": "ops-agent",
  "taskId": "task-456",
  "resource": "application:app-A",
  "action": "logs.read"
}
```

这只是授权检查所需的数据示例。生产实现中，`agentId`、调用者身份和任务归属必须来自已验证的凭证或可信的服务端上下文，不能直接相信请求体中的字段。

返回值可以采用 `ALLOW` 或 `DENY`，并附带内部原因码。不要把内部敏感信息暴露给调用方。

## 六、授权凭证怎么设计？

这是整个方案里比较容易踩坑的地方。

我建议第一版**先不自行设计复杂的 Token Exchange 协议**，而是把委托记录与调用凭证分开。

- `grant_id`：委托记录的引用，不是秘密，也不能单独作为授权凭证。
- 工作负载凭证：证明请求来自哪个服务或 Agent 运行环境。
- 任务级访问凭证：在需要时证明这次请求对应哪项委托，以及允许访问的范围。

初期可以让网关通过受保护的内部接口在线查询委托服务。这样撤销后能够较快生效，不必一开始就解决所有 Token 撤销和分发问题。

后续如果需要签发短期访问令牌，再考虑采用标准令牌交换机制。无论哪种方案，**令牌有效不代表委托必然有效**：敏感调用还要考虑撤销状态、令牌受众、有效期和授权范围。

## 七、第一阶段应该做哪些，不做哪些？

我建议把 MVP 控制在以下范围：

<box border radius="lg" padding={3} gap={2}>
  {@body const items = [
    "建立委托主表、范围表和审计表",
    "接入现有用户身份，识别可信的调用主体",
    "实现委托申请、查询、撤销接口",
    "实现任务级授权检查接口",
    "接入一个 agentgateway 路由和一个只读日志 MCP",
    "覆盖越权访问、过期、撤销和伪造身份的测试"
  ]}
  {@body const [done,setDone] = DIL.useState([])}
  <row align="center" justify="between">
    <title size="md">MVP 实施清单</title>
    <text color="secondary" size="sm">{done.length}/{items.length}</text>
  </row>
  {#each items as item,i}
    <checkbox key={item} checked={done.includes(i)} onChange={v=>setDone(prev=>v?[...prev,i]:prev.filter(x=>x!==i))}>{item}</checkbox>
  {/each}
</box>

第一阶段暂时不必引入 SPIRE，也不必实现 Agent 之间任意多级委托。先把用户到 Agent、Agent 到 MCP 的一跳委托做安全、可验证，再扩展多 Agent 调用链和沙箱身份绑定。

---

## 八、我建议接下来先确定一个关键设计

**谁有权创建一条委托？**

这会决定委托服务的权限模型和后续实现复杂度。

<box border radius="lg" padding={3} gap={3}>
  {@body const [mode,setMode] = DIL.useState("user")}
  <radio-group value={mode} onChange={setMode} direction="col">
    <radio value="user">
      **模式 A：用户授权为主**
      <text color="secondary" size="sm">用户发起任务，服务端检查用户权限和 Agent 能力后创建委托。适合先落地运维助手。</text>
    </radio>
    <radio value="platform">
      **模式 B：平台策略自动授权**
      <text color="secondary" size="sm">平台根据预先审批的策略自动创建委托，用户不需要逐项授权。适合固定业务流程和自动化任务。</text>
    </radio>
    <radio value="hybrid">
      **模式 C：两者结合**
      <text color="secondary" size="sm">普通只读操作自动授权，高风险操作要求额外审批或人工确认。更适合逐步演进的企业平台。</text>
    </radio>
  </radio-group>
  <button block onClick={() => GenUI.issueNewTurn("Agent Delegation Service 设计继续。委托创建授权模式选择："+({"user":"模式 A：用户授权为主","platform":"模式 B：平台策略自动授权","hybrid":"模式 C：普通只读操作自动授权，高风险操作额外审批或人工确认"}[mode])+"。请据此细化权限模型、数据库和 Spring Boot 实现。")}>按此模式继续设计</button>
</box>

我的倾向是 **模式 C**，但可以先以模式 A 的最小闭环实现，再逐步增加自动授权规则。这样不会一开始就把企业权限治理做得过于复杂。


---

## 👤 用户

Agent Delegation Service 设计继续。委托创建授权模式选择：模式 C：普通只读操作自动授权，高风险操作额外审批或人工确认。请据此细化权限模型、数据库和 Spring Boot 实现。


---

## 🤖 ChatGPT

我们就按模式 C 来设计：**普通只读操作自动授权，高风险操作必须经过额外审批或人工确认。**

我建议把 Agent Delegation Service 做成一个独立的 Spring Boot 服务。第一阶段不需要把它建设成庞大的策略引擎，但必须把权限边界、授权状态和执行检查设计正确，否则以后接入 agentgateway、MCP、沙箱时会很难补救。

最重要的设计原则是：

> **Agent 能力审批、任务委托授权、实际操作执行，是三个不同的关卡。三个关卡全部通过，操作才允许执行。**

例如，运维 Agent 已获准查询应用日志，并不意味着每个用户都能让它查询任意应用；用户有权查询应用 A，也不意味着 Agent 可以重启应用 A。

# 一、总体架构

<box border radius="xl" padding={3} gap={2} align="center">
  <box background="surface-secondary" radius="lg" padding={3} width="100%" align="center" gap={1}>
    **用户 / 上游 Agent**
    <text color="secondary" size="sm">提交任务，例如排查 app-A 页面报错</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg" />
  <box border radius="lg" padding={3} width="100%" align="center" gap={1}>
    **Agent Registry + 企业权限目录**
    <text color="secondary" size="sm">Agent 是否存在、版本是否获批、用户对资源有什么权限</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg" />
  <box background="rgba(59,130,246,0.10)" radius="lg" padding={3} width="100%" align="center" gap={1}>
    **Agent Delegation Service**
    <text color="secondary" size="sm">判断风险等级、创建委托、审批、撤销、审计</text>
  </box>
  <grid columns={2} gap={2}>
    <grid-item>
      <box border radius="lg" padding={3} align="center" gap={1}>
        <icon name="check-circle" size="lg" color="success" />
        **低风险**
        <text color="secondary" size="sm">符合规则，自动创建委托</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="lg" padding={3} align="center" gap={1}>
        <icon name="user-check" size="lg" />
        **高风险**
        <text color="secondary" size="sm">进入审批，获批后激活委托</text>
      </box>
    </grid-item>
  </grid>
  <icon name="arrow-down" color="secondary" size="lg" />
  <box background="rgba(16,185,129,0.09)" radius="lg" padding={3} width="100%" align="center" gap={1}>
    **agentgateway / MCP Server**
    <text color="secondary" size="sm">每次工具调用校验身份、委托状态、资源范围和操作权限</text>
  </box>
</box>

这里有个前提：用户的基础权限、Agent 已批准的能力、资源的风险等级都必须有可信来源。Delegation Service 可以读取这些信息，但不要直接相信 Agent 在请求中自报的权限。

---

# 二、权限模型：把权限拆成三层

## 1. Agent 能力：这个 Agent 被允许做什么？

由 Agent Registry 或平台审批流程维护。

例如：

| Agent | 能力 | 状态 |
|---|---|---|
| `ops-agent` | `logs.read` | 已批准 |
| `ops-agent` | `database.query_readonly` | 已批准 |
| `ops-agent` | `service.restart` | 待审批 |
| `ops-agent` | `database.execute_write` | 未批准 |

能力需要关联适用的资源类型、环境和约束。不能因为批准了 `logs.read`，就允许读取所有应用、所有环境的日志。

## 2. 用户权限：这个用户本来能做什么？

从企业现有的权限系统中读取，可能是 IAM、业务系统权限或你们自己的资源授权服务。

例如：

- 小王有权查看 `app-A` 的日志。
- 小王无权访问生产数据库。
- 小王有权查看测试环境，但没有生产环境的重启权限。

如果现阶段没有统一的用户资源权限服务，第一版可以定义一个适配接口，再用现有业务权限 API 实现；不要在委托服务中偷偷复制一套长期不一致的权限数据。

## 3. 委托权限：这一次任务允许做什么？

由 Delegation Service 创建并维护。

例如：

```json
{
  "grantId": "dg-123",
  "taskId": "task-456",
  "agentId": "ops-agent",
  "resources": ["application:app-A"],
  "actions": ["logs.read"],
  "expiresAt": "2026-10-15T22:30:00+08:00"
}
```

这代表本次任务的权限边界，而不是 Agent 的永久权限。

最终允许条件可以概括为：

\[
\text{ALLOW} =
I \land C \land U \land G \land R \land P
\]

其中：

- \(I\)：调用者身份可信。
- \(C\)：Agent 的能力已批准。
- \(U\)：授权主体拥有相应基础权限。
- \(G\)：委托有效且未撤销。
- \(R\)：资源、环境和操作符合委托范围。
- \(P\)：高风险操作所需的审批已完成。

任何一项不满足，都应拒绝执行。

# 三、数据库设计

建议第一版采用 MySQL。它足以支撑委托管理、审批流程和审计，不需要一开始引入专门的策略数据库。

核心表建议分成五张，再加一张可选的自动授权规则表。

| 表 | 主要职责 |
|---|---|
| `delegation_grant` | 记录一项委托及其生命周期 |
| `delegation_scope` | 记录允许访问的资源和操作 |
| `delegation_approval` | 记录高风险操作的审批要求与结果 |
| `delegation_audit` | 追加记录授权和执行事件 |
| `delegation_policy` | 管理自动授权规则和风险策略，可在第一版简化 |

另外，调用方身份、Agent 能力和用户权限应通过可信的身份系统、Registry 或企业权限系统获得，不必在本服务里复制完整的主体档案。

## 1. 委托主表

```sql
CREATE TABLE delegation_grant (
    id              BIGINT PRIMARY KEY AUTO_INCREMENT,
    grant_id        VARCHAR(64) NOT NULL,
    task_id         VARCHAR(64) NOT NULL,
    grantor_issuer  VARCHAR(255) NOT NULL,
    grantor_subject VARCHAR(255) NOT NULL,
    agent_id        VARCHAR(128) NOT NULL,

    status          VARCHAR(24) NOT NULL,
    risk_level      VARCHAR(16) NOT NULL,
    policy_version  VARCHAR(64) NOT NULL,

    created_at      DATETIME(3) NOT NULL,
    valid_from      DATETIME(3) NOT NULL,
    expires_at      DATETIME(3) NOT NULL,
    revoked_at      DATETIME(3) NULL,
    revoke_reason   VARCHAR(512) NULL,

    version         BIGINT NOT NULL DEFAULT 0,

    UNIQUE KEY uk_grant_id (grant_id),
    KEY idx_task_id (task_id),
    KEY idx_grantor (grantor_issuer, grantor_subject),
    KEY idx_agent_status (agent_id, status),
    KEY idx_expiry (status, expires_at)
);
```

几个关键设计：

- `grant_id` 是对外使用的全局唯一委托标识，不直接暴露数据库自增 ID。
- `grantor_issuer + grantor_subject` 共同标识授权主体，避免不同身份系统中相同的 `subject` 被误认为同一人。
- `policy_version` 保存创建委托时所依据的策略版本，方便审计和解释授权决定。
- `version` 用于乐观锁，避免并发撤销、审批或更新产生覆盖问题。
- `expires_at` 必须有明确的有效期，不能创建永不过期的任务委托。

`status` 建议至少包括：

`PENDING_APPROVAL`、`ACTIVE`、`REVOKED`、`EXPIRED`、`DENIED`。

其中 `EXPIRED` 既可以通过定时任务更新，也可以在每次授权检查时根据时间即时判断。**不能因为定时任务还没更新状态，就让过期委托继续生效。**

## 2. 委托范围表

```sql
CREATE TABLE delegation_scope (
    id             BIGINT PRIMARY KEY AUTO_INCREMENT,
    grant_id       VARCHAR(64) NOT NULL,
    resource_type  VARCHAR(64) NOT NULL,
    resource_id    VARCHAR(255) NOT NULL,
    action         VARCHAR(128) NOT NULL,
    constraints_json JSON NULL,

    created_at     DATETIME(3) NOT NULL,

    UNIQUE KEY uk_scope (
        grant_id, resource_type, resource_id, action
    ),
    KEY idx_scope_lookup (
        grant_id, resource_type, resource_id, action
    )
);
```

例如一条范围记录可以是：

- 资源类型：`application`
- 资源 ID：`app-A`
- 操作：`logs.read`
- 约束：只允许查询最近 30 分钟的日志，最多返回 1000 条。

约束字段可以用 JSON 保存，但必须由服务端根据对应操作的规则解析和验证。不能把任意 JSON 直接交给 Agent 自行解释。

对于涉及金额、数据库条件、目标环境等重要参数的操作，应进一步定义明确的参数 Schema，而不是依赖自然语言约束。

## 3. 审批表

```sql
CREATE TABLE delegation_approval (
    id                BIGINT PRIMARY KEY AUTO_INCREMENT,
    approval_id       VARCHAR(64) NOT NULL,
    grant_id          VARCHAR(64) NOT NULL,
    approval_type     VARCHAR(32) NOT NULL,
    status            VARCHAR(24) NOT NULL,

    requested_by      VARCHAR(255) NOT NULL,
    approver_subject  VARCHAR(255) NULL,
    decision_reason   VARCHAR(1024) NULL,

    request_hash      VARCHAR(128) NOT NULL,
    requested_at      DATETIME(3) NOT NULL,
    decided_at        DATETIME(3) NULL,
    expires_at        DATETIME(3) NOT NULL,

    UNIQUE KEY uk_approval_id (approval_id),
    KEY idx_grant_id (grant_id, status)
);
```

`request_hash` 用于绑定被审批的具体操作参数。审批通过后，如果 Agent 修改目标服务器、数据库、SQL 或其他关键参数，就不能继续复用原审批结果。

审批人身份必须来自可信认证上下文，不能由请求者自己填写一个 `approver_subject` 来完成审批。高风险操作还应避免申请人审批自己的申请。

## 4. 审计表

```sql
CREATE TABLE delegation_audit (
    id             BIGINT PRIMARY KEY AUTO_INCREMENT,
    event_id       VARCHAR(64) NOT NULL,
    grant_id       VARCHAR(64) NULL,
    task_id        VARCHAR(64) NULL,
    actor_subject  VARCHAR(255) NULL,

    event_type     VARCHAR(64) NOT NULL,
    decision       VARCHAR(16) NULL,
    reason_code    VARCHAR(64) NULL,
    resource_type  VARCHAR(64) NULL,
    resource_id    VARCHAR(255) NULL,
    action         VARCHAR(128) NULL,

    trace_id       VARCHAR(128) NULL,
    created_at     DATETIME(3) NOT NULL,

    UNIQUE KEY uk_event_id (event_id),
    KEY idx_audit_grant (grant_id, created_at),
    KEY idx_audit_task (task_id, created_at)
);
```

审计事件包括委托创建、审批通过或拒绝、授权检查允许或拒绝、撤销、过期，以及高风险操作的执行结果。

注意：授权检查请求可能非常频繁。可以按安全要求区分授权决策日志和完整的业务执行审计，避免把所有高频日志都同步写进同一个数据库造成瓶颈。重要审计记录应具备防篡改、访问控制和保留期限机制。

---

# 四、自动授权和人工审批规则

我不建议把“只读”简单等同于“低风险”。

例如，查看一条普通应用日志和批量导出包含大量客户敏感信息的日志，虽然都是读取操作，风险却完全不同。

第一版可以从规则表或代码中的集中策略开始，后续再将规则外置。

| 操作示例 | 默认处理 | 附加条件 |
|---|---|---|
| 查看指定应用的近期普通日志 | 自动授权 | 用户有查看权限，Agent 能力已批准 |
| 查询受限范围内的业务数据 | 自动授权或审批 | 根据数据敏感级别和查询范围决定 |
| 批量导出客户敏感数据 | 人工审批 | 数据权限、导出范围、用途和数量限制 |
| 重启生产服务 | 人工确认或审批 | 目标环境、影响范围和操作参数必须明确 |
| 执行生产数据库写操作 | 默认拒绝或严格审批 | 独立的数据库执行权限和风险控制 |
| 删除数据、修改权限、关闭安全防护 | 默认拒绝 | 不应仅凭普通任务委托放行 |

风险等级由服务端根据标准化的 `resource_type + action + environment + data_classification` 等属性计算，不能由 Agent 自行传入 `risk_level=LOW`。

## 授权创建的判断顺序

<CodeBlock language="mermaid">
flowchart TD
    A[收到委托申请] --> B{调用者身份可信?}
    B -- 否 --> X[拒绝]
    B -- 是 --> C{Agent 已登记且能力已批准?}
    C -- 否 --> X
    C -- 是 --> D{授权主体有基础权限?}
    D -- 否 --> X
    D -- 是 --> E[计算风险并匹配策略]
    E --> F{是否满足自动授权条件?}
    F -- 是 --> G[创建 ACTIVE 委托]
    F -- 否 --> H{是否允许进入审批流程?}
    H -- 否 --> X
    H -- 是 --> I[创建 PENDING_APPROVAL 委托]
    I --> J{审批通过且申请未过期?}
    J -- 是 --> K[激活委托]
    J -- 否 --> X
    G --> L[记录审计]
    K --> L
</CodeBlock>

有一个重要区别：**高风险操作的审批不应该给整个 Agent 发放一张长期通行证。** 应当绑定具体任务、资源、动作和关键参数，且在审批通过后重新检查授权主体的权限、Agent 能力和委托有效期。

# 五、Spring Boot 服务怎么组织？

第一版建议使用 Java 21 + Spring Boot 3.x（如果你们现有技术栈有统一版本，优先跟随公司标准），使用 Spring Security 处理认证、MyBatis 或 Spring Data JPA 访问数据库。

不需要微服务拆得很细，一个独立 Spring Boot 应用就够了。

## 1. 模块结构

```text
agent-delegation-service/
├── identity/
│   ├── AuthenticatedPrincipal.java
│   └── IdentityResolver.java
├── agent/
│   └── AgentCapabilityClient.java
├── policy/
│   ├── RiskClassifier.java
│   ├── AuthorizationPolicy.java
│   └── PolicyEvaluator.java
├── delegation/
│   ├── DelegationController.java
│   ├── DelegationService.java
│   ├── DelegationRepository.java
│   └── DelegationStateMachine.java
├── approval/
│   ├── ApprovalController.java
│   └── ApprovalService.java
├── authorization/
│   ├── AuthorizationController.java
│   └── AuthorizationCheckService.java
├── audit/
│   └── AuditService.java
└── integration/
    ├── KeycloakClient.java
    ├── AgentRegistryClient.java
    └── GatewayIntegration.java
```

这里的 `AgentCapabilityClient`、`AgentRegistryClient` 都是调用外部系统的适配器，不应该把它们的业务数据和委托记录混在一起。

## 2. API 设计

建议先固定下面这些接口：

| API | 作用 |
|---|---|
| `POST /api/v1/delegations` | 创建委托申请，自动决定激活或进入审批 |
| `GET /api/v1/delegations/{grantId}` | 查询委托状态和范围 |
| `POST /api/v1/delegations/{grantId}/revoke` | 撤销委托 |
| `POST /api/v1/approvals/{approvalId}/approve` | 审批通过 |
| `POST /api/v1/approvals/{approvalId}/reject` | 审批拒绝 |
| `POST /api/v1/authorization/check` | 检查一次具体工具调用是否允许 |

创建委托的请求可以类似这样：

```json
{
  "taskId": "task-456",
  "agentId": "ops-agent",
  "requestedScopes": [
    {
      "resourceType": "application",
      "resourceId": "app-A",
      "action": "logs.read",
      "constraints": {
        "maxLookbackMinutes": 30,
        "maxResults": 1000
      }
    }
  ]
}
```

这里的请求只是表达“我想做什么”，不是授权结果。服务端必须重新验证用户身份、Agent 能力、资源权限和风险规则。

创建接口还应支持幂等键，避免网络重试造成重复委托。

## 3. 核心服务代码

下面展示的是关键逻辑的简化版本，省略了具体的 Repository 实现、参数校验和异常映射。

```java
@Service
@RequiredArgsConstructor
public class DelegationService {

    private final IdentityResolver identityResolver;
    private final AgentCapabilityClient capabilityClient;
    private final BusinessPermissionClient permissionClient;
    private final RiskClassifier riskClassifier;
    private final PolicyEvaluator policyEvaluator;
    private final DelegationRepository repository;
    private final AuditService auditService;

    @Transactional
    public CreateDelegationResult create(
            CreateDelegationRequest request) {

        var principal = identityResolver.currentPrincipal();

        // 1. 验证 Agent 的能力已获批准
        capabilityClient.requireApprovedCapabilities(
                request.agentId(), request.requestedScopes());

        // 2. 验证用户对每个资源和操作具有基础权限
        permissionClient.requirePermissions(
                principal, request.requestedScopes());

        // 3. 根据服务端规则计算风险
        var risk = riskClassifier.classify(
                request.requestedScopes());

        // 4. 评估是否允许自动授权或必须审批
        var decision = policyEvaluator.evaluate(
                principal, request, risk);

        // 5. 在事务中持久化委托、范围及审计事件
        return repository.createAccordingToDecision(
                principal, request, risk, decision);
    }
}
```

这段代码有两个值得特别注意的地方。

第一，`RiskClassifier` 和 `PolicyEvaluator` 应是服务端可信逻辑，不能使用请求中的风险等级作为决策依据。

第二，创建委托的数据库写入、范围写入和关键审计事件应具有一致性。若审计写入外部系统，可以采用事务 Outbox 等模式，避免业务已成功但审计事件丢失。

实际实现时，`repository.createAccordingToDecision` 应被拆成明确的事务逻辑，而不是把所有职责永久塞进一个 Repository。

---

# 六、最重要的接口：每次工具调用如何授权？

这是整个服务真正的安全边界。

创建委托只能说明一项任务曾经获得授权；它不能证明下一次工具调用仍然合法。

一次授权检查至少要确认：

1. 调用方的身份凭证真实有效。
2. 当前工作负载确实对应已登记的 Agent。
3. `grant_id` 对应的委托属于当前任务和授权主体。
4. 委托处于有效状态，且没有过期或撤销。
5. 请求的资源、操作、环境和约束均在授权范围内。
6. 对于高风险操作，审批已通过，且批准的操作参数与实际请求一致。

接口可定义为：

```java
public record AuthorizationCheckRequest(
        String grantId,
        String taskId,
        String resourceType,
        String resourceId,
        String action,
        Map<String, Object> parameters
) {}

public record AuthorizationDecision(
        boolean allowed,
        String reasonCode,
        String grantId
) {}
```

真正的调用身份应从经过验证的 JWT、mTLS 工作负载凭证或可信网关上下文中取得，而不是从这个请求对象里读取 `agentId` 或 `userId`。

授权服务的核心逻辑可以是：

```java
public AuthorizationDecision check(
        VerifiedCaller caller,
        AuthorizationCheckRequest request) {

    var grant = grantRepository.findByGrantId(
            request.grantId());

    if (grant == null || !grant.isActiveAt(clock.instant())) {
        return deny("GRANT_INACTIVE");
    }

    if (!caller.matchesAgent(grant.getAgentId())) {
        return deny("CALLER_MISMATCH");
    }

    if (!grant.getTaskId().equals(request.taskId())) {
        return deny("TASK_MISMATCH");
    }

    if (!scopeMatcher.matches(
            grant, request.resourceType(),
            request.resourceId(), request.action(),
            request.parameters())) {
        return deny("SCOPE_MISMATCH");
    }

    if (!approvalService.isSatisfied(grant, request)) {
        return deny("APPROVAL_REQUIRED");
    }

    return allow(grant.getGrantId());
}
```

这是用于说明检查顺序的示例，不能直接当作生产代码。生产环境还需要重新验证调用主体与任务的关联、Agent 能力状态、必要的用户权限，以及参数规范化和高风险操作审批绑定。

**任何检查出现异常、身份无法验证或必要的权限服务不可用，都应默认拒绝敏感操作，而不是降级为允许。**

对于低风险读取操作，可以在确认权限变化风险可接受的前提下使用短 TTL 缓存；高风险操作和撤销检查应采用更严格的实时校验策略。

# 七、审批、撤销和并发问题怎么处理？

这三个问题建议第一版就设计好，否则上线后补救成本很高。

## 1. 审批状态机

审批不能只是一张表里写个 `approved=true`。

建议使用以下状态流转：

<CodeBlock language="mermaid">
stateDiagram-v2
    [*] --> PENDING_APPROVAL
    PENDING_APPROVAL --> ACTIVE: 审批通过且条件仍有效
    PENDING_APPROVAL --> DENIED: 审批拒绝
    PENDING_APPROVAL --> EXPIRED: 审批超时
    ACTIVE --> REVOKED: 撤销
    ACTIVE --> EXPIRED: 到期
    DENIED --> [*]
    REVOKED --> [*]
    EXPIRED --> [*]
</CodeBlock>

注意几个约束：

- 审批只能由有权限的审批人执行。
- 审批通过时重新检查申请是否过期、用户权限是否仍有效、Agent 能力是否仍获批。
- 已撤销的委托不能直接重新激活；需要重新申请。
- 审批结果必须绑定具体的委托范围和关键操作参数。
- 状态更新使用数据库事务和乐观锁，避免两个并发审批或撤销请求相互覆盖。

## 2. 撤销应该怎样生效？

例如，用户正在使用运维 Agent，突然取消任务。

撤销接口应在事务中将委托更新为 `REVOKED`，记录撤销人、时间和原因，并产生可追踪的审计事件。

但这里有一个现实问题：如果 agentgateway 缓存了之前的 `ALLOW` 决策，单纯更新数据库并不能保证下一次调用立即被拒绝。

因此第一版建议：

- 对高风险操作，每次执行前实时检查授权。
- 对普通读取操作，缓存时间要短，并明确撤销传播的最大延迟。
- 撤销时发送失效通知，清理相关缓存。
- 对正在执行的长时间任务，通知 Agent Runtime 取消任务，并由执行端处理取消。
- 对无法安全中断的操作，必须定义清晰的执行状态和补偿措施。

不要承诺所有撤销都能瞬间终止已经发出的网络请求或已经执行的 SQL。撤销通常可以阻止后续操作，但无法自动撤回已经完成的操作。

## 3. 避免审批和撤销竞态

假设审批人正在批准一项生产操作，而用户同时撤销了委托。

如果审批逻辑只读取一次状态，再无条件更新为 `ACTIVE`，就可能把已经撤销的授权重新激活。

可以用带状态条件的更新实现原子状态迁移：

```sql
UPDATE delegation_grant
SET status = 'ACTIVE',
    version = version + 1
WHERE grant_id = ?
  AND status = 'PENDING_APPROVAL'
  AND expires_at > CURRENT_TIMESTAMP(3)
  AND version = ?;
```

应用必须检查受影响行数：如果不是 1，说明状态已变化或版本冲突，需要拒绝本次状态迁移并重新读取状态。

审批和撤销应采用相同原则，不能通过普通的无条件 `UPDATE` 修改生命周期状态。

---

# 八、怎么接入 agentgateway 和 MCP？

我建议先采用**在线授权检查**，暂时不急于设计自己的 Token Exchange 协议。

<box border radius="lg" padding={3} gap={2}>
  <row align="center" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="key-round" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第一步：验证身份**
      <text color="secondary" size="sm">网关验证调用者凭证，确定可信的 Agent 工作负载身份。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="center" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="shield-check" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第二步：检查委托**
      <text color="secondary" size="sm">根据可信上下文中的委托 ID、任务 ID、资源和操作调用授权检查接口。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="center" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="check-circle" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第三步：执行或拒绝**
      <text color="secondary" size="sm">仅当授权检查通过时转发请求；高风险操作还要在最终执行端验证。</text>
    </box>
  </row>
</box>

这里有个实现细节：不能让普通 Agent 通过任意请求头伪造可信的用户身份、工作负载身份或审批结果。网关应移除外部传入的同名可信头，再从经过验证的凭证中建立内部上下文。

此外，agentgateway 的具体外部授权接口、请求上下文传递方式和缓存能力需要按你们实际选用的版本验证。不要在没有验证插件或扩展能力的情况下，假设它能自动完成全部委托逻辑。

如果后续采用短期令牌，令牌只应表达受限授权，并限制签发者、受众、有效期、操作范围和委托关联。高风险操作仍应检查实时撤销状态。

我们就按模式 C 来设计：**普通只读操作自动授权，高风险操作必须经过额外审批或人工确认。**

我建议把 Agent Delegation Service 做成一个独立的 Spring Boot 服务。第一阶段不需要把它建设成庞大的策略引擎，但必须把权限边界、授权状态和执行检查设计正确，否则以后接入 agentgateway、MCP、沙箱时会很难补救。

最重要的设计原则是：

> **Agent 能力审批、任务委托授权、实际操作执行，是三个不同的关卡。三个关卡全部通过，操作才允许执行。**

例如，运维 Agent 已获准查询应用日志，并不意味着每个用户都能让它查询任意应用；用户有权查询应用 A，也不意味着 Agent 可以重启应用 A。

# 一、总体架构

<box border radius="xl" padding={3} gap={2} align="center">
  <box background="surface-secondary" radius="lg" padding={3} width="100%" align="center" gap={1}>
    **用户 / 上游 Agent**
    <text color="secondary" size="sm">提交任务，例如排查 app-A 页面报错</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg" />
  <box border radius="lg" padding={3} width="100%" align="center" gap={1}>
    **Agent Registry + 企业权限目录**
    <text color="secondary" size="sm">Agent 是否存在、版本是否获批、用户对资源有什么权限</text>
  </box>
  <icon name="arrow-down" color="secondary" size="lg" />
  <box background="rgba(59,130,246,0.10)" radius="lg" padding={3} width="100%" align="center" gap={1}>
    **Agent Delegation Service**
    <text color="secondary" size="sm">判断风险等级、创建委托、审批、撤销、审计</text>
  </box>
  <grid columns={2} gap={2}>
    <grid-item>
      <box border radius="lg" padding={3} align="center" gap={1}>
        <icon name="check-circle" size="lg" color="success" />
        **低风险**
        <text color="secondary" size="sm">符合规则，自动创建委托</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="lg" padding={3} align="center" gap={1}>
        <icon name="user-check" size="lg" />
        **高风险**
        <text color="secondary" size="sm">进入审批，获批后激活委托</text>
      </box>
    </grid-item>
  </grid>
  <icon name="arrow-down" color="secondary" size="lg" />
  <box background="rgba(16,185,129,0.09)" radius="lg" padding={3} width="100%" align="center" gap={1}>
    **agentgateway / MCP Server**
    <text color="secondary" size="sm">每次工具调用校验身份、委托状态、资源范围和操作权限</text>
  </box>
</box>

这里有个前提：用户的基础权限、Agent 已批准的能力、资源的风险等级都必须有可信来源。Delegation Service 可以读取这些信息，但不要直接相信 Agent 在请求中自报的权限。

---

# 二、权限模型：把权限拆成三层

## 1. Agent 能力：这个 Agent 被允许做什么？

由 Agent Registry 或平台审批流程维护。

例如：

| Agent | 能力 | 状态 |
|---|---|---|
| `ops-agent` | `logs.read` | 已批准 |
| `ops-agent` | `database.query_readonly` | 已批准 |
| `ops-agent` | `service.restart` | 待审批 |
| `ops-agent` | `database.execute_write` | 未批准 |

能力需要关联适用的资源类型、环境和约束。不能因为批准了 `logs.read`，就允许读取所有应用、所有环境的日志。

## 2. 用户权限：这个用户本来能做什么？

从企业现有的权限系统中读取，可能是 IAM、业务系统权限或你们自己的资源授权服务。

例如：

- 小王有权查看 `app-A` 的日志。
- 小王无权访问生产数据库。
- 小王有权查看测试环境，但没有生产环境的重启权限。

如果现阶段没有统一的用户资源权限服务，第一版可以定义一个适配接口，再用现有业务权限 API 实现；不要在委托服务中偷偷复制一套长期不一致的权限数据。

## 3. 委托权限：这一次任务允许做什么？

由 Delegation Service 创建并维护。

例如：

```json
{
  "grantId": "dg-123",
  "taskId": "task-456",
  "agentId": "ops-agent",
  "resources": ["application:app-A"],
  "actions": ["logs.read"],
  "expiresAt": "2026-10-15T22:30:00+08:00"
}
```

这代表本次任务的权限边界，而不是 Agent 的永久权限。

最终允许条件可以概括为：

\[
\text{ALLOW} =
I \land C \land U \land G \land R \land P
\]

其中：

- \(I\)：调用者身份可信。
- \(C\)：Agent 的能力已批准。
- \(U\)：授权主体拥有相应基础权限。
- \(G\)：委托有效且未撤销。
- \(R\)：资源、环境和操作符合委托范围。
- \(P\)：高风险操作所需的审批已完成。

任何一项不满足，都应拒绝执行。

# 三、数据库设计

建议第一版采用 MySQL。它足以支撑委托管理、审批流程和审计，不需要一开始引入专门的策略数据库。

核心表建议分成五张，再加一张可选的自动授权规则表。

| 表 | 主要职责 |
|---|---|
| `delegation_grant` | 记录一项委托及其生命周期 |
| `delegation_scope` | 记录允许访问的资源和操作 |
| `delegation_approval` | 记录高风险操作的审批要求与结果 |
| `delegation_audit` | 追加记录授权和执行事件 |
| `delegation_policy` | 管理自动授权规则和风险策略，可在第一版简化 |

另外，调用方身份、Agent 能力和用户权限应通过可信的身份系统、Registry 或企业权限系统获得，不必在本服务里复制完整的主体档案。

## 1. 委托主表

```sql
CREATE TABLE delegation_grant (
    id              BIGINT PRIMARY KEY AUTO_INCREMENT,
    grant_id        VARCHAR(64) NOT NULL,
    task_id         VARCHAR(64) NOT NULL,
    grantor_issuer  VARCHAR(255) NOT NULL,
    grantor_subject VARCHAR(255) NOT NULL,
    agent_id        VARCHAR(128) NOT NULL,

    status          VARCHAR(24) NOT NULL,
    risk_level      VARCHAR(16) NOT NULL,
    policy_version  VARCHAR(64) NOT NULL,

    created_at      DATETIME(3) NOT NULL,
    valid_from      DATETIME(3) NOT NULL,
    expires_at      DATETIME(3) NOT NULL,
    revoked_at      DATETIME(3) NULL,
    revoke_reason   VARCHAR(512) NULL,

    version         BIGINT NOT NULL DEFAULT 0,

    UNIQUE KEY uk_grant_id (grant_id),
    KEY idx_task_id (task_id),
    KEY idx_grantor (grantor_issuer, grantor_subject),
    KEY idx_agent_status (agent_id, status),
    KEY idx_expiry (status, expires_at)
);
```

几个关键设计：

- `grant_id` 是对外使用的全局唯一委托标识，不直接暴露数据库自增 ID。
- `grantor_issuer + grantor_subject` 共同标识授权主体，避免不同身份系统中相同的 `subject` 被误认为同一人。
- `policy_version` 保存创建委托时所依据的策略版本，方便审计和解释授权决定。
- `version` 用于乐观锁，避免并发撤销、审批或更新产生覆盖问题。
- `expires_at` 必须有明确的有效期，不能创建永不过期的任务委托。

`status` 建议至少包括：

`PENDING_APPROVAL`、`ACTIVE`、`REVOKED`、`EXPIRED`、`DENIED`。

其中 `EXPIRED` 既可以通过定时任务更新，也可以在每次授权检查时根据时间即时判断。**不能因为定时任务还没更新状态，就让过期委托继续生效。**

## 2. 委托范围表

```sql
CREATE TABLE delegation_scope (
    id             BIGINT PRIMARY KEY AUTO_INCREMENT,
    grant_id       VARCHAR(64) NOT NULL,
    resource_type  VARCHAR(64) NOT NULL,
    resource_id    VARCHAR(255) NOT NULL,
    action         VARCHAR(128) NOT NULL,
    constraints_json JSON NULL,

    created_at     DATETIME(3) NOT NULL,

    UNIQUE KEY uk_scope (
        grant_id, resource_type, resource_id, action
    ),
    KEY idx_scope_lookup (
        grant_id, resource_type, resource_id, action
    )
);
```

例如一条范围记录可以是：

- 资源类型：`application`
- 资源 ID：`app-A`
- 操作：`logs.read`
- 约束：只允许查询最近 30 分钟的日志，最多返回 1000 条。

约束字段可以用 JSON 保存，但必须由服务端根据对应操作的规则解析和验证。不能把任意 JSON 直接交给 Agent 自行解释。

对于涉及金额、数据库条件、目标环境等重要参数的操作，应进一步定义明确的参数 Schema，而不是依赖自然语言约束。

## 3. 审批表

```sql
CREATE TABLE delegation_approval (
    id                BIGINT PRIMARY KEY AUTO_INCREMENT,
    approval_id       VARCHAR(64) NOT NULL,
    grant_id          VARCHAR(64) NOT NULL,
    approval_type     VARCHAR(32) NOT NULL,
    status            VARCHAR(24) NOT NULL,

    requested_by      VARCHAR(255) NOT NULL,
    approver_subject  VARCHAR(255) NULL,
    decision_reason   VARCHAR(1024) NULL,

    request_hash      VARCHAR(128) NOT NULL,
    requested_at      DATETIME(3) NOT NULL,
    decided_at        DATETIME(3) NULL,
    expires_at        DATETIME(3) NOT NULL,

    UNIQUE KEY uk_approval_id (approval_id),
    KEY idx_grant_id (grant_id, status)
);
```

`request_hash` 用于绑定被审批的具体操作参数。审批通过后，如果 Agent 修改目标服务器、数据库、SQL 或其他关键参数，就不能继续复用原审批结果。

审批人身份必须来自可信认证上下文，不能由请求者自己填写一个 `approver_subject` 来完成审批。高风险操作还应避免申请人审批自己的申请。

## 4. 审计表

```sql
CREATE TABLE delegation_audit (
    id             BIGINT PRIMARY KEY AUTO_INCREMENT,
    event_id       VARCHAR(64) NOT NULL,
    grant_id       VARCHAR(64) NULL,
    task_id        VARCHAR(64) NULL,
    actor_subject  VARCHAR(255) NULL,

    event_type     VARCHAR(64) NOT NULL,
    decision       VARCHAR(16) NULL,
    reason_code    VARCHAR(64) NULL,
    resource_type  VARCHAR(64) NULL,
    resource_id    VARCHAR(255) NULL,
    action         VARCHAR(128) NULL,

    trace_id       VARCHAR(128) NULL,
    created_at     DATETIME(3) NOT NULL,

    UNIQUE KEY uk_event_id (event_id),
    KEY idx_audit_grant (grant_id, created_at),
    KEY idx_audit_task (task_id, created_at)
);
```

审计事件包括委托创建、审批通过或拒绝、授权检查允许或拒绝、撤销、过期，以及高风险操作的执行结果。

注意：授权检查请求可能非常频繁。可以按安全要求区分授权决策日志和完整的业务执行审计，避免把所有高频日志都同步写进同一个数据库造成瓶颈。重要审计记录应具备防篡改、访问控制和保留期限机制。

---

# 四、自动授权和人工审批规则

我不建议把“只读”简单等同于“低风险”。

例如，查看一条普通应用日志和批量导出包含大量客户敏感信息的日志，虽然都是读取操作，风险却完全不同。

第一版可以从规则表或代码中的集中策略开始，后续再将规则外置。

| 操作示例 | 默认处理 | 附加条件 |
|---|---|---|
| 查看指定应用的近期普通日志 | 自动授权 | 用户有查看权限，Agent 能力已批准 |
| 查询受限范围内的业务数据 | 自动授权或审批 | 根据数据敏感级别和查询范围决定 |
| 批量导出客户敏感数据 | 人工审批 | 数据权限、导出范围、用途和数量限制 |
| 重启生产服务 | 人工确认或审批 | 目标环境、影响范围和操作参数必须明确 |
| 执行生产数据库写操作 | 默认拒绝或严格审批 | 独立的数据库执行权限和风险控制 |
| 删除数据、修改权限、关闭安全防护 | 默认拒绝 | 不应仅凭普通任务委托放行 |

风险等级由服务端根据标准化的 `resource_type + action + environment + data_classification` 等属性计算，不能由 Agent 自行传入 `risk_level=LOW`。

## 授权创建的判断顺序

<CodeBlock language="mermaid">
flowchart TD
    A[收到委托申请] --> B{调用者身份可信?}
    B -- 否 --> X[拒绝]
    B -- 是 --> C{Agent 已登记且能力已批准?}
    C -- 否 --> X
    C -- 是 --> D{授权主体有基础权限?}
    D -- 否 --> X
    D -- 是 --> E[计算风险并匹配策略]
    E --> F{是否满足自动授权条件?}
    F -- 是 --> G[创建 ACTIVE 委托]
    F -- 否 --> H{是否允许进入审批流程?}
    H -- 否 --> X
    H -- 是 --> I[创建 PENDING_APPROVAL 委托]
    I --> J{审批通过且申请未过期?}
    J -- 是 --> K[激活委托]
    J -- 否 --> X
    G --> L[记录审计]
    K --> L
</CodeBlock>

有一个重要区别：**高风险操作的审批不应该给整个 Agent 发放一张长期通行证。** 应当绑定具体任务、资源、动作和关键参数，且在审批通过后重新检查授权主体的权限、Agent 能力和委托有效期。

# 五、Spring Boot 服务怎么组织？

第一版建议使用 Java 21 + Spring Boot 3.x（如果你们现有技术栈有统一版本，优先跟随公司标准），使用 Spring Security 处理认证、MyBatis 或 Spring Data JPA 访问数据库。

不需要微服务拆得很细，一个独立 Spring Boot 应用就够了。

## 1. 模块结构

```text
agent-delegation-service/
├── identity/
│   ├── AuthenticatedPrincipal.java
│   └── IdentityResolver.java
├── agent/
│   └── AgentCapabilityClient.java
├── policy/
│   ├── RiskClassifier.java
│   ├── AuthorizationPolicy.java
│   └── PolicyEvaluator.java
├── delegation/
│   ├── DelegationController.java
│   ├── DelegationService.java
│   ├── DelegationRepository.java
│   └── DelegationStateMachine.java
├── approval/
│   ├── ApprovalController.java
│   └── ApprovalService.java
├── authorization/
│   ├── AuthorizationController.java
│   └── AuthorizationCheckService.java
├── audit/
│   └── AuditService.java
└── integration/
    ├── KeycloakClient.java
    ├── AgentRegistryClient.java
    └── GatewayIntegration.java
```

这里的 `AgentCapabilityClient`、`AgentRegistryClient` 都是调用外部系统的适配器，不应该把它们的业务数据和委托记录混在一起。

## 2. API 设计

建议先固定下面这些接口：

| API | 作用 |
|---|---|
| `POST /api/v1/delegations` | 创建委托申请，自动决定激活或进入审批 |
| `GET /api/v1/delegations/{grantId}` | 查询委托状态和范围 |
| `POST /api/v1/delegations/{grantId}/revoke` | 撤销委托 |
| `POST /api/v1/approvals/{approvalId}/approve` | 审批通过 |
| `POST /api/v1/approvals/{approvalId}/reject` | 审批拒绝 |
| `POST /api/v1/authorization/check` | 检查一次具体工具调用是否允许 |

创建委托的请求可以类似这样：

```json
{
  "taskId": "task-456",
  "agentId": "ops-agent",
  "requestedScopes": [
    {
      "resourceType": "application",
      "resourceId": "app-A",
      "action": "logs.read",
      "constraints": {
        "maxLookbackMinutes": 30,
        "maxResults": 1000
      }
    }
  ]
}
```

这里的请求只是表达“我想做什么”，不是授权结果。服务端必须重新验证用户身份、Agent 能力、资源权限和风险规则。

创建接口还应支持幂等键，避免网络重试造成重复委托。

## 3. 核心服务代码

下面展示的是关键逻辑的简化版本，省略了具体的 Repository 实现、参数校验和异常映射。

```java
@Service
@RequiredArgsConstructor
public class DelegationService {

    private final IdentityResolver identityResolver;
    private final AgentCapabilityClient capabilityClient;
    private final BusinessPermissionClient permissionClient;
    private final RiskClassifier riskClassifier;
    private final PolicyEvaluator policyEvaluator;
    private final DelegationRepository repository;
    private final AuditService auditService;

    @Transactional
    public CreateDelegationResult create(
            CreateDelegationRequest request) {

        var principal = identityResolver.currentPrincipal();

        // 1. 验证 Agent 的能力已获批准
        capabilityClient.requireApprovedCapabilities(
                request.agentId(), request.requestedScopes());

        // 2. 验证用户对每个资源和操作具有基础权限
        permissionClient.requirePermissions(
                principal, request.requestedScopes());

        // 3. 根据服务端规则计算风险
        var risk = riskClassifier.classify(
                request.requestedScopes());

        // 4. 评估是否允许自动授权或必须审批
        var decision = policyEvaluator.evaluate(
                principal, request, risk);

        // 5. 在事务中持久化委托、范围及审计事件
        return repository.createAccordingToDecision(
                principal, request, risk, decision);
    }
}
```

这段代码有两个值得特别注意的地方。

第一，`RiskClassifier` 和 `PolicyEvaluator` 应是服务端可信逻辑，不能使用请求中的风险等级作为决策依据。

第二，创建委托的数据库写入、范围写入和关键审计事件应具有一致性。若审计写入外部系统，可以采用事务 Outbox 等模式，避免业务已成功但审计事件丢失。

实际实现时，`repository.createAccordingToDecision` 应被拆成明确的事务逻辑，而不是把所有职责永久塞进一个 Repository。

---

# 六、最重要的接口：每次工具调用如何授权？

这是整个服务真正的安全边界。

创建委托只能说明一项任务曾经获得授权；它不能证明下一次工具调用仍然合法。

一次授权检查至少要确认：

1. 调用方的身份凭证真实有效。
2. 当前工作负载确实对应已登记的 Agent。
3. `grant_id` 对应的委托属于当前任务和授权主体。
4. 委托处于有效状态，且没有过期或撤销。
5. 请求的资源、操作、环境和约束均在授权范围内。
6. 对于高风险操作，审批已通过，且批准的操作参数与实际请求一致。

接口可定义为：

```java
public record AuthorizationCheckRequest(
        String grantId,
        String taskId,
        String resourceType,
        String resourceId,
        String action,
        Map<String, Object> parameters
) {}

public record AuthorizationDecision(
        boolean allowed,
        String reasonCode,
        String grantId
) {}
```

真正的调用身份应从经过验证的 JWT、mTLS 工作负载凭证或可信网关上下文中取得，而不是从这个请求对象里读取 `agentId` 或 `userId`。

授权服务的核心逻辑可以是：

```java
public AuthorizationDecision check(
        VerifiedCaller caller,
        AuthorizationCheckRequest request) {

    var grant = grantRepository.findByGrantId(
            request.grantId());

    if (grant == null || !grant.isActiveAt(clock.instant())) {
        return deny("GRANT_INACTIVE");
    }

    if (!caller.matchesAgent(grant.getAgentId())) {
        return deny("CALLER_MISMATCH");
    }

    if (!grant.getTaskId().equals(request.taskId())) {
        return deny("TASK_MISMATCH");
    }

    if (!scopeMatcher.matches(
            grant, request.resourceType(),
            request.resourceId(), request.action(),
            request.parameters())) {
        return deny("SCOPE_MISMATCH");
    }

    if (!approvalService.isSatisfied(grant, request)) {
        return deny("APPROVAL_REQUIRED");
    }

    return allow(grant.getGrantId());
}
```

这是用于说明检查顺序的示例，不能直接当作生产代码。生产环境还需要重新验证调用主体与任务的关联、Agent 能力状态、必要的用户权限，以及参数规范化和高风险操作审批绑定。

**任何检查出现异常、身份无法验证或必要的权限服务不可用，都应默认拒绝敏感操作，而不是降级为允许。**

对于低风险读取操作，可以在确认权限变化风险可接受的前提下使用短 TTL 缓存；高风险操作和撤销检查应采用更严格的实时校验策略。

# 七、审批、撤销和并发问题怎么处理？

这三个问题建议第一版就设计好，否则上线后补救成本很高。

## 1. 审批状态机

审批不能只是一张表里写个 `approved=true`。

建议使用以下状态流转：

<CodeBlock language="mermaid">
stateDiagram-v2
    [*] --> PENDING_APPROVAL
    PENDING_APPROVAL --> ACTIVE: 审批通过且条件仍有效
    PENDING_APPROVAL --> DENIED: 审批拒绝
    PENDING_APPROVAL --> EXPIRED: 审批超时
    ACTIVE --> REVOKED: 撤销
    ACTIVE --> EXPIRED: 到期
    DENIED --> [*]
    REVOKED --> [*]
    EXPIRED --> [*]
</CodeBlock>

注意几个约束：

- 审批只能由有权限的审批人执行。
- 审批通过时重新检查申请是否过期、用户权限是否仍有效、Agent 能力是否仍获批。
- 已撤销的委托不能直接重新激活；需要重新申请。
- 审批结果必须绑定具体的委托范围和关键操作参数。
- 状态更新使用数据库事务和乐观锁，避免两个并发审批或撤销请求相互覆盖。

## 2. 撤销应该怎样生效？

例如，用户正在使用运维 Agent，突然取消任务。

撤销接口应在事务中将委托更新为 `REVOKED`，记录撤销人、时间和原因，并产生可追踪的审计事件。

但这里有一个现实问题：如果 agentgateway 缓存了之前的 `ALLOW` 决策，单纯更新数据库并不能保证下一次调用立即被拒绝。

因此第一版建议：

- 对高风险操作，每次执行前实时检查授权。
- 对普通读取操作，缓存时间要短，并明确撤销传播的最大延迟。
- 撤销时发送失效通知，清理相关缓存。
- 对正在执行的长时间任务，通知 Agent Runtime 取消任务，并由执行端处理取消。
- 对无法安全中断的操作，必须定义清晰的执行状态和补偿措施。

不要承诺所有撤销都能瞬间终止已经发出的网络请求或已经执行的 SQL。撤销通常可以阻止后续操作，但无法自动撤回已经完成的操作。

## 3. 避免审批和撤销竞态

假设审批人正在批准一项生产操作，而用户同时撤销了委托。

如果审批逻辑只读取一次状态，再无条件更新为 `ACTIVE`，就可能把已经撤销的授权重新激活。

可以用带状态条件的更新实现原子状态迁移：

```sql
UPDATE delegation_grant
SET status = 'ACTIVE',
    version = version + 1
WHERE grant_id = ?
  AND status = 'PENDING_APPROVAL'
  AND expires_at > CURRENT_TIMESTAMP(3)
  AND version = ?;
```

应用必须检查受影响行数：如果不是 1，说明状态已变化或版本冲突，需要拒绝本次状态迁移并重新读取状态。

审批和撤销应采用相同原则，不能通过普通的无条件 `UPDATE` 修改生命周期状态。

---

# 八、怎么接入 agentgateway 和 MCP？

我建议先采用**在线授权检查**，暂时不急于设计自己的 Token Exchange 协议。

<box border radius="lg" padding={3} gap={2}>
  <row align="center" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="key-round" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第一步：验证身份**
      <text color="secondary" size="sm">网关验证调用者凭证，确定可信的 Agent 工作负载身份。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="center" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="shield-check" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第二步：检查委托**
      <text color="secondary" size="sm">根据可信上下文中的委托 ID、任务 ID、资源和操作调用授权检查接口。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="center" gap={3}>
    <box background="surface-secondary" radius="md" padding={2}>
      <icon name="check-circle" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第三步：执行或拒绝**
      <text color="secondary" size="sm">仅当授权检查通过时转发请求；高风险操作还要在最终执行端验证。</text>
    </box>
  </row>
</box>

这里有个实现细节：不能让普通 Agent 通过任意请求头伪造可信的用户身份、工作负载身份或审批结果。网关应移除外部传入的同名可信头，再从经过验证的凭证中建立内部上下文。

此外，agentgateway 的具体外部授权接口、请求上下文传递方式和缓存能力需要按你们实际选用的版本验证。不要在没有验证插件或扩展能力的情况下，假设它能自动完成全部委托逻辑。

如果后续采用短期令牌，令牌只应表达受限授权，并限制签发者、受众、有效期、操作范围和委托关联。高风险操作仍应检查实时撤销状态。

# 九、建议按三个阶段实施

不要一开始就同时建设完整的身份、授权、网关、沙箱体系。可以按以下顺序逐步落地。

<list connector="solid" marker="number" gap={3}>
  <list-item>
    **阶段 1：委托授权闭环**

    <badge color="success">优先实施</badge>

    - 创建委托、查询、撤销。
    - 实现低风险自动授权和高风险审批。
    - 完成资源范围匹配、状态机和审计。
    - 用一个只读日志 MCP 完成端到端验证。
  </list-item>
  <list-item>
    **阶段 2：接入企业身份和通信层**

    - 接入 Keycloak 或现有企业身份系统。
    - 让 agentgateway 执行可信身份验证和授权拦截。
    - 在 MCP Server 端增加必要的独立校验。
    - 建立撤销通知、缓存失效和授权决策监控。
  </list-item>
  <list-item>
    **阶段 3：扩展到多 Agent 和沙箱**

    - 引入 SPIRE/SPIFFE 等工作负载身份机制。
    - 将 `agent_id`、`instance_id`、`session_id` 和 `task_id` 关联起来。
    - 支持受限的 Agent 间委托和任务取消。
    - 增加生产变更、批量数据访问等高风险操作的专门审批规则。
  </list-item>
</list>

## 十、我建议现在先确定实现环境

下面几个信息会直接影响数据库 DDL、Spring Security 配置，以及 agentgateway 的接入方式。可以先给出大致情况，不必所有问题都回答。

<AskUserDetails
  invitation="补充一下你们现有的技术环境，我可以据此把上面的方案继续细化成可落地的 Spring Boot 工程设计。"
  button_label="补充技术环境"
  locale_override="zh-CN"
  questions={[
    {
      question: "委托服务准备使用哪种数据库？",
      type: "single_select",
      options: ["MySQL", "PostgreSQL", "公司统一数据库，尚未确定"],
      free_text_placeholder: "其他数据库或版本"
    },
    {
      question: "目前用户和服务身份认证的基础是什么？",
      type: "single_select",
      options: ["已有 Keycloak", "公司统一 SSO / IAM", "暂时没有统一身份系统"],
      free_text_placeholder: "现有认证方式"
    },
    {
      question: "Agent 和工具调用准备如何运行？",
      type: "multi_select",
      options: ["agentgateway + MCP", "Java 服务端 Agent", "Kubernetes 沙箱", "非 Kubernetes 独立沙箱"],
      free_text_placeholder: "其他运行方式或当前进展"
    }
  ]}
/>

有了这些信息，下一步就可以把设计收敛到具体工程：表结构、实体和状态机、事务边界、接口 DTO、Spring Security 认证上下文，以及授权检查的集成测试。

# 九、建议按三个阶段实施

不要一开始就同时建设完整的身份、授权、网关、沙箱体系。可以按以下顺序逐步落地。

<list connector="solid" marker="number" gap={3}>
  <list-item>
    **阶段 1：委托授权闭环**

    <badge color="success">优先实施</badge>

    - 创建委托、查询、撤销。
    - 实现低风险自动授权和高风险审批。
    - 完成资源范围匹配、状态机和审计。
    - 用一个只读日志 MCP 完成端到端验证。
  </list-item>
  <list-item>
    **阶段 2：接入企业身份和通信层**

    - 接入 Keycloak 或现有企业身份系统。
    - 让 agentgateway 执行可信身份验证和授权拦截。
    - 在 MCP Server 端增加必要的独立校验。
    - 建立撤销通知、缓存失效和授权决策监控。
  </list-item>
  <list-item>
    **阶段 3：扩展到多 Agent 和沙箱**

    - 引入 SPIRE/SPIFFE 等工作负载身份机制。
    - 将 `agent_id`、`instance_id`、`session_id` 和 `task_id` 关联起来。
    - 支持受限的 Agent 间委托和任务取消。
    - 增加生产变更、批量数据访问等高风险操作的专门审批规则。
  </list-item>
</list>

## 十、我建议现在先确定实现环境

下面几个信息会直接影响数据库 DDL、Spring Security 配置，以及 agentgateway 的接入方式。可以先给出大致情况，不必所有问题都回答。

<AskUserDetails
  invitation="补充一下你们现有的技术环境，我可以据此把上面的方案继续细化成可落地的 Spring Boot 工程设计。"
  button_label="补充技术环境"
  locale_override="zh-CN"
  questions={[
    {
      question: "委托服务准备使用哪种数据库？",
      type: "single_select",
      options: ["MySQL", "PostgreSQL", "公司统一数据库，尚未确定"],
      free_text_placeholder: "其他数据库或版本"
    },
    {
      question: "目前用户和服务身份认证的基础是什么？",
      type: "single_select",
      options: ["已有 Keycloak", "公司统一 SSO / IAM", "暂时没有统一身份系统"],
      free_text_placeholder: "现有认证方式"
    },
    {
      question: "Agent 和工具调用准备如何运行？",
      type: "multi_select",
      options: ["agentgateway + MCP", "Java 服务端 Agent", "Kubernetes 沙箱", "非 Kubernetes 独立沙箱"],
      free_text_placeholder: "其他运行方式或当前进展"
    }
  ]}
/>

有了这些信息，下一步就可以把设计收敛到具体工程：表结构、实体和状态机、事务边界、接口 DTO、Spring Security 认证上下文，以及授权检查的集成测试。


---

## 👤 用户

> 委托服务准备使用哪种数据库？
MySQL

> 目前用户和服务身份认证的基础是什么？
公司统一 SSO / IAM

> Agent 和工具调用准备如何运行？
Java 服务端 Agent, agentgateway + MCP


---

## 🤖 ChatGPT

很好，这三个条件已经足够把方案收敛到一套具体的工程设计了。

我建议你的第一版采用：

- **后端：** Java 21 + Spring Boot 3.x。
- **数据库：** MySQL，保存委托、授权范围、审批和审计。
- **身份认证：** 接入公司现有 SSO / IAM，不再另建一套用户身份系统。
- **调用链：** Java 服务端 Agent → agentgateway → MCP Server。
- **授权核心：** 独立的 Agent Delegation Service，负责任务级授权、风险判断、审批和撤销。

第一阶段不引入 SPIRE，也不要求立即实现 Token Exchange。先把委托授权闭环跑通。

不过有一条边界必须先确定：**SSO 证明用户是谁，不一定能直接告诉我们用户是否有权操作某个业务资源。** 需要确认现有 IAM 是否能查询资源级权限；如果不能，就通过适配接口接入现有业务权限系统，暂时没有统一权限源的资源则默认拒绝，而不是自动放行。

很好，这三个条件已经足够把方案收敛到一套具体的工程设计了。

我建议你的第一版采用：

- **后端：** Java 21 + Spring Boot 3.x。
- **数据库：** MySQL，保存委托、授权范围、审批和审计。
- **身份认证：** 接入公司现有 SSO / IAM，不再另建一套用户身份系统。
- **调用链：** Java 服务端 Agent → agentgateway → MCP Server。
- **授权核心：** 独立的 Agent Delegation Service，负责任务级授权、风险判断、审批和撤销。

第一阶段不引入 SPIRE，也不要求立即实现 Token Exchange。先把委托授权闭环跑通。

不过有一条边界必须先确定：**SSO 证明用户是谁，不一定能直接告诉我们用户是否有权操作某个业务资源。** 需要确认现有 IAM 是否能查询资源级权限；如果不能，就通过适配接口接入现有业务权限系统，暂时没有统一权限源的资源则默认拒绝，而不是自动放行。

## 下一步：先把工程的安全边界定死

我建议先按这个默认方案实施：

1. 委托服务只接受经过认证的用户请求，或经过认证的服务间请求。
2. Java Agent 不持有任意数据库或服务器的长期凭证；工具调用统一经过受控的 gateway/MCP 路径。
3. 每个工具调用都携带可验证的任务上下文和委托引用，网关及执行端不能信任 Agent 自报的身份字段。
4. MySQL 中的委托状态是授权检查的重要依据；撤销必须能阻止后续调用。
5. 高风险审批绑定具体操作参数，参数变化就重新审批。
6. 审计记录可追溯，但不记录不必要的客户敏感数据、访问令牌或密钥。

然后我们就可以开始写工程，而不是继续停留在概念架构上。

**我建议下一步直接输出一份可实施的工程蓝图**：完整 MySQL DDL、状态机、Spring Boot 模块与依赖、接口请求/响应、认证适配器接口，以及针对撤销竞态和越权场景的测试用例。这样你可以直接按模块开始开发。

## 下一步：先把工程的安全边界定死

我建议先按这个默认方案实施：

1. 委托服务只接受经过认证的用户请求，或经过认证的服务间请求。
2. Java Agent 不持有任意数据库或服务器的长期凭证；工具调用统一经过受控的 gateway/MCP 路径。
3. 每个工具调用都携带可验证的任务上下文和委托引用，网关及执行端不能信任 Agent 自报的身份字段。
4. MySQL 中的委托状态是授权检查的重要依据；撤销必须能阻止后续调用。
5. 高风险审批绑定具体操作参数，参数变化就重新审批。
6. 审计记录可追溯，但不记录不必要的客户敏感数据、访问令牌或密钥。

然后我们就可以开始写工程，而不是继续停留在概念架构上。

**我建议下一步直接输出一份可实施的工程蓝图**：完整 MySQL DDL、状态机、Spring Boot 模块与依赖、接口请求/响应、认证适配器接口，以及针对撤销竞态和越权场景的测试用例。这样你可以直接按模块开始开发。


---

## 👤 用户

是的，请继续


---

## 🤖 ChatGPT

我们直接进入工程设计。这份蓝图以**能开始编码、能做安全测试、后续能接入 agentgateway 和 MCP** 为目标，不再展开大而全的安全理论。

先定四个实现原则：

1. 委托服务是独立的 Spring Boot 应用，使用 MySQL 持久化授权状态。
2. 公司 SSO / IAM 负责认证；委托服务通过适配器获取用户权限，不自己发明用户身份。
3. 低风险操作可以自动授权，高风险操作必须经过审批；没有明确匹配规则时默认拒绝。
4. agentgateway 和 MCP Server 都要有安全检查，不能把委托服务当成一个“检查一次就永久放行”的系统。

# 一、整体工程结构

<box border radius="xl" padding={3} gap={2}>
  <box background="rgba(59,130,246,0.10)" radius="lg" padding={3} align="center" gap={1}>
    **公司 SSO / IAM**
    <text color="secondary" size="sm">用户登录、服务身份认证、企业权限查询</text>
  </box>
  <box align="center">
    <icon name="arrow-down" color="secondary" size="lg" />
  </box>
  <box border radius="lg" padding={3} align="center" gap={1}>
    **Agent Delegation Service**
    <text color="secondary" size="sm">Spring Boot · REST API · 授权策略 · 审批 · 撤销 · 审计</text>
  </box>
  <box align="center">
    <icon name="arrow-down" color="secondary" size="lg" />
  </box>
  <box background="surface-secondary" radius="lg" padding={3} align="center" gap={1}>
    **MySQL**
    <text color="secondary" size="sm">委托、资源范围、审批、策略版本、审计事件</text>
  </box>
  <divider />
  <grid columns={2} gap={2}>
    <grid-item>
      <box border radius="lg" padding={3} gap={1}>
        <icon name="workflow" size="lg" />
        **agentgateway**
        <text color="secondary" size="sm">请求拦截、调用身份验证、授权检查</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="lg" padding={3} gap={1}>
        <icon name="server" size="lg" />
        **MCP Server**
        <text color="secondary" size="sm">执行前复核、资源级权限控制</text>
      </box>
    </grid-item>
  </grid>
</box>

这里要特别说明：委托服务不负责运行 Agent，也不负责执行 MCP 工具。它负责决定某次调用是否符合授权条件，并提供可以被调用方验证的决策依据。

## 推荐技术栈

| 组件 | 选择 |
|---|---|
| Java | Java 21 |
| 框架 | Spring Boot 3.x |
| 安全 | Spring Security OAuth2 Resource Server，按公司 IAM 的协议适配 |
| 持久化 | MySQL 8.x + Flyway |
| 数据访问 | Spring JDBC 或 MyBatis |
| API 文档 | Springdoc OpenAPI |
| 测试 | JUnit 5 + Testcontainers MySQL |
| 事件与审计 | 第一版数据库事务；后续需要时增加 Outbox 和消息队列 |

我会优先考虑 Spring JDBC 或 MyBatis，而不是一开始就大量依赖 ORM 自动处理状态变更。这个服务最重要的是明确的 SQL 条件、事务边界和并发控制。

# 二、MySQL 数据库设计

第一版先使用五张核心表。为了避免一个任务里同时包含读操作和高风险操作时，状态语义变得混乱，我建议**每条委托只包含一组风险等级一致、授权规则一致的操作范围**。同一任务需要读日志和重启服务时，可以建立两条关联同一个 `task_id` 的委托。

## 1. 委托主表 `delegation_grant`

```sql
CREATE TABLE delegation_grant (
    id BIGINT NOT NULL AUTO_INCREMENT,
    grant_id VARCHAR(64) NOT NULL,
    task_id VARCHAR(64) NOT NULL,

    grantor_issuer VARCHAR(255) NOT NULL,
    grantor_subject VARCHAR(255) NOT NULL,
    agent_id VARCHAR(128) NOT NULL,

    status VARCHAR(24) NOT NULL,
    risk_level VARCHAR(16) NOT NULL,
    policy_version VARCHAR(64) NOT NULL,

    created_at DATETIME(3) NOT NULL,
    valid_from DATETIME(3) NOT NULL,
    expires_at DATETIME(3) NOT NULL,
    revoked_at DATETIME(3) DEFAULT NULL,
    revoke_reason VARCHAR(512) DEFAULT NULL,

    version BIGINT NOT NULL DEFAULT 0,

    PRIMARY KEY (id),
    UNIQUE KEY uk_grant_id (grant_id),
    KEY idx_task_id (task_id),
    KEY idx_grantor (grantor_issuer, grantor_subject),
    KEY idx_agent_status (agent_id, status),
    KEY idx_status_expiry (status, expires_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

关键字段：

- `grant_id`：对外使用的委托标识，采用服务端生成的高熵随机 ID。
- `task_id`：同一任务可能有多条委托。
- `grantor_issuer + grantor_subject`：标识授权主体，避免不同身份系统的相同用户 ID 混淆。
- `agent_id`：逻辑 Agent 身份，不是某个 Java 进程的身份。
- `version`：用于乐观锁，防止审批、撤销等并发更新互相覆盖。

建议状态枚举：

| 状态 | 含义 |
|---|---|
| `PENDING_APPROVAL` | 等待审批 |
| `ACTIVE` | 当前可用，但每次调用仍须检查范围 |
| `REVOKED` | 已撤销，不允许恢复 |
| `EXPIRED` | 已过期 |
| `DENIED` | 申请被拒绝 |

即使状态仍然是 `ACTIVE`，只要 `expires_at` 已经过去，也必须拒绝调用。定时任务只是负责清理和更新展示状态，不能承担安全判断。

## 2. 授权范围表 `delegation_scope`

```sql
CREATE TABLE delegation_scope (
    id BIGINT NOT NULL AUTO_INCREMENT,
    grant_id VARCHAR(64) NOT NULL,

    resource_type VARCHAR(64) NOT NULL,
    resource_id VARCHAR(255) NOT NULL,
    action VARCHAR(128) NOT NULL,
    constraints_json JSON DEFAULT NULL,

    created_at DATETIME(3) NOT NULL,

    PRIMARY KEY (id),
    UNIQUE KEY uk_grant_scope (
        grant_id, resource_type, resource_id, action
    ),
    KEY idx_scope_lookup (
        grant_id, resource_type, resource_id, action
    )
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

示例：

| resource_type | resource_id | action | 约束 |
|---|---|---|---|
| `application` | `app-A` | `logs.read` | 最多回溯 30 分钟 |
| `database` | `db-test-01` | `database.query_readonly` | 仅允许指定只读查询类型 |

这里有个重要实现细节：**`constraints_json` 不是一段交给大模型解释的自然语言，而是由服务端按操作类型定义 Schema、校验和执行的结构化约束。**

例如，`logs.read` 的查询时间范围、最大返回条数，都应该有服务端强制执行的上限。仅仅在委托记录里写了“最多查询 30 分钟”，但日志 MCP 不检查时间范围，并不能构成真正的安全限制。

## 3. 审批表 `delegation_approval`

```sql
CREATE TABLE delegation_approval (
    id BIGINT NOT NULL AUTO_INCREMENT,
    approval_id VARCHAR(64) NOT NULL,
    grant_id VARCHAR(64) NOT NULL,

    approval_type VARCHAR(32) NOT NULL,
    status VARCHAR(24) NOT NULL,

    requested_by VARCHAR(255) NOT NULL,
    approver_issuer VARCHAR(255) DEFAULT NULL,
    approver_subject VARCHAR(255) DEFAULT NULL,

    request_hash VARCHAR(128) NOT NULL,
    decision_reason VARCHAR(1024) DEFAULT NULL,

    requested_at DATETIME(3) NOT NULL,
    decided_at DATETIME(3) DEFAULT NULL,
    expires_at DATETIME(3) NOT NULL,

    PRIMARY KEY (id),
    UNIQUE KEY uk_approval_id (approval_id),
    KEY idx_grant_status (grant_id, status)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

`request_hash` 绑定需要审批的具体操作内容。服务端应对规范化后的资源、操作、参数和关键约束计算摘要，不能直接对顺序不固定的 JSON 原文计算后就认为语义一致。

如果申请人更换目标服务器、SQL 或操作参数，必须重新计算摘要；摘要不匹配就不能复用旧审批。

## 4. 策略表 `delegation_policy`

```sql
CREATE TABLE delegation_policy (
    id BIGINT NOT NULL AUTO_INCREMENT,
    policy_id VARCHAR(128) NOT NULL,
    policy_version VARCHAR(64) NOT NULL,

    resource_type VARCHAR(64) NOT NULL,
    action VARCHAR(128) NOT NULL,
    environment VARCHAR(32) NOT NULL,
    data_classification VARCHAR(32) NOT NULL,

    risk_level VARCHAR(16) NOT NULL,
    decision_mode VARCHAR(32) NOT NULL,
    max_ttl_seconds INT NOT NULL,
    enabled TINYINT(1) NOT NULL DEFAULT 1,

    created_at DATETIME(3) NOT NULL,
    updated_at DATETIME(3) NOT NULL,

    PRIMARY KEY (id),
    UNIQUE KEY uk_policy_version (policy_id, policy_version),
    KEY idx_policy_match (
        resource_type, action, environment,
        data_classification, enabled
    )
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

`decision_mode` 可以先支持：

- `AUTO_ALLOW`：满足全部基础条件后自动授权。
- `REQUIRE_APPROVAL`：必须审批。
- `DENY`：禁止此类委托。

**没有匹配规则时默认拒绝。** 多条规则同时匹配时，第一版建议使用明确的优先级字段或严格的匹配唯一性约束，不要依赖数据库返回顺序决定授权。

## 5. 审计表 `delegation_audit`

```sql
CREATE TABLE delegation_audit (
    id BIGINT NOT NULL AUTO_INCREMENT,
    event_id VARCHAR(64) NOT NULL,
    grant_id VARCHAR(64) DEFAULT NULL,
    task_id VARCHAR(64) DEFAULT NULL,

    actor_issuer VARCHAR(255) DEFAULT NULL,
    actor_subject VARCHAR(255) DEFAULT NULL,
    event_type VARCHAR(64) NOT NULL,
    decision VARCHAR(16) DEFAULT NULL,
    reason_code VARCHAR(64) DEFAULT NULL,

    resource_type VARCHAR(64) DEFAULT NULL,
    resource_id VARCHAR(255) DEFAULT NULL,
    action VARCHAR(128) DEFAULT NULL,
    trace_id VARCHAR(128) DEFAULT NULL,

    created_at DATETIME(3) NOT NULL,

    PRIMARY KEY (id),
    UNIQUE KEY uk_event_id (event_id),
    KEY idx_grant_time (grant_id, created_at),
    KEY idx_task_time (task_id, created_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

审计表应限制应用账号的修改和删除权限，并根据企业要求设置备份、保留和防篡改机制。不要把访问令牌、密码、完整客户敏感数据或未经脱敏的 SQL 参数直接写入审计记录。

第一版可以将授权决策和关键状态变更同步写入同一 MySQL 事务。未来如果需要将审计发送到外部平台，再增加事务 Outbox，避免数据库提交成功但事件发送失败。

# 三、API 契约

建议统一使用 `/api/v1`，将“业务申请”和“安全决策”分成不同接口。

| 方法 | 路径 | 用途 |
|---|---|---|
| POST | `/api/v1/delegations` | 申请委托 |
| GET | `/api/v1/delegations/{grantId}` | 查询委托 |
| POST | `/api/v1/delegations/{grantId}/revoke` | 撤销委托 |
| POST | `/api/v1/approvals/{approvalId}/approve` | 审批通过 |
| POST | `/api/v1/approvals/{approvalId}/reject` | 审批拒绝 |
| POST | `/api/v1/authorization/check` | 检查工具调用是否允许 |

创建委托的请求示例：

```json
{
  "taskId": "task-456",
  "agentId": "ops-agent",
  "requestedScopes": [
    {
      "resourceType": "application",
      "resourceId": "app-A",
      "action": "logs.read",
      "constraints": {
        "maxLookbackMinutes": 30,
        "maxResults": 1000
      }
    }
  ]
}
```

成功响应示例：

```json
{
  "grantId": "dg-example",
  "taskId": "task-456",
  "status": "ACTIVE",
  "riskLevel": "LOW",
  "expiresAt": "2026-10-15T22:30:00+08:00"
}
```

这里的 ID 仅为示例。正式实现时应由服务端生成，不接受调用者指定 `grantId` 或自行指定风险等级。

如果是高风险操作，创建接口返回 `PENDING_APPROVAL`，而不是 `ACTIVE`。

建议使用统一响应错误码，例如：

- `IDENTITY_INVALID`
- `AGENT_NOT_APPROVED`
- `USER_PERMISSION_DENIED`
- `POLICY_NOT_FOUND`
- `APPROVAL_REQUIRED`
- `GRANT_INACTIVE`
- `SCOPE_MISMATCH`
- `APPROVAL_MISMATCH`

客户端只应获得必要的错误信息；内部拒绝原因可以写入受保护的审计记录。

# 四、Spring Boot 项目模块

可以从一个 Maven 单体服务开始，不需要拆成多个微服务。

```text
agent-delegation-service/
├── src/main/java/com/company/agentdelegation/
│   ├── config/
│   ├── security/
│   │   ├── AuthenticatedPrincipal.java
│   │   ├── IamPrincipalMapper.java
│   │   └── SecurityConfig.java
│   ├── identity/
│   │   ├── IdentityProvider.java
│   │   └── UserPermissionProvider.java
│   ├── registry/
│   │   └── AgentRegistryClient.java
│   ├── policy/
│   │   ├── RiskClassifier.java
│   │   ├── PolicyEvaluator.java
│   │   └── ScopeMatcher.java
│   ├── delegation/
│   │   ├── DelegationController.java
│   │   ├── DelegationService.java
│   │   ├── DelegationRepository.java
│   │   └── DelegationStateMachine.java
│   ├── approval/
│   ├── authorization/
│   └── audit/
└── src/test/
```

模块职责：

- `security`：验证公司 IAM 凭证，把可信身份转换成内部统一主体。
- `identity`：查询用户基础权限、资源访问权限。
- `registry`：检查 Agent 是否存在，以及对应版本和能力是否已获批准。
- `policy`：识别风险等级、匹配策略、比较资源和操作范围。
- `delegation`：创建、查询、撤销委托，管理状态机。
- `approval`：验证审批人权限，绑定被批准的具体操作。
- `authorization`：为 gateway 和 MCP 调用提供统一检查入口。
- `audit`：记录授权决策与状态变化。

第一版可以用接口隔离 IAM 和 Agent Registry，不必为这些系统创建复杂的领域模型。

## 身份适配器的设计

```java
public interface UserPermissionProvider {

    boolean hasPermission(
            AuthenticatedPrincipal principal,
            String resourceType,
            String resourceId,
            String action);
}
```

这个接口只是一个起点。生产环境更适合增加批量检查、超时、错误分类和权限版本等能力。

还要注意：**权限查询失败与权限不足是两种不同的情况，但在安全决策上都不能放行。** 可以分别返回 `PERMISSION_DENIED` 和 `PERMISSION_PROVIDER_UNAVAILABLE`，方便告警和排障。

`AuthenticatedPrincipal` 应包含从可信认证结果提取的主体标识、签发方和必要的组织信息。不要直接使用请求体中传来的用户名作为授权主体。

# 五、核心业务流程怎么实现

## 1. 创建委托

创建委托不是简单地插入一行数据库。建议按下面的顺序执行：

1. 验证调用者身份，确认是用户本人或经过认证的上游服务。
2. 从可信上下文获取用户身份；校验 Agent ID 和版本状态。
3. 对每个资源和操作查询用户基础权限、Agent 已批准能力。
4. 通过服务端策略计算风险等级，限制最长有效期。
5. 低风险申请创建 `ACTIVE` 委托；需要审批的申请创建 `PENDING_APPROVAL` 委托。
6. 在事务中保存委托、范围、必要的审批记录和审计事件。
7. 返回委托 ID 和状态。

其中第 3、4 步可能依赖外部系统。**不要在开启 MySQL 长事务后再调用远程 IAM 或 Registry**，否则外部接口慢时会长时间占用数据库连接和锁。应先完成外部检查，再开启短事务保存结果，并在后续授权检查时重新验证关键状态。

为避免请求重试导致重复创建，建议第一版支持 `Idempotency-Key`，并将它与可信调用主体、请求摘要关联。相同键但请求内容不同，必须拒绝。

## 2. 高风险审批

审批接口必须先验证当前审批人有审批权限，并检查是否允许审批自己的申请。

然后：

1. 读取待审批委托和审批记录。
2. 校验审批仍然有效，且状态为 `PENDING`。
3. 校验 `request_hash` 与被批准的操作一致。
4. 重新检查用户权限、Agent 能力和策略是否仍然有效。
5. 使用事务和条件更新完成状态迁移。
6. 记录审批决定及审计事件。

审批通过不应该直接跳过授权检查。即使审批已经完成，调用时仍需检查委托是否过期、是否撤销、目标资源和参数是否匹配。

## 3. 授权检查

建议网关每次准备执行受控工具调用时提交以下信息：

```json
{
  "grantId": "dg-example",
  "taskId": "task-456",
  "resourceType": "application",
  "resourceId": "app-A",
  "action": "logs.read",
  "parameters": {
    "lookbackMinutes": 10,
    "maxResults": 100
  }
}
```

但是请求里的这些字段只代表“请求者声称要执行什么”，并不能证明它的身份。

因此授权检查服务还必须获得一个经过验证的调用者上下文，例如：

- 由网关验证后建立的可信身份上下文；
- 或者由网关使用自己的工作负载凭证向授权服务发起的内部请求，并通过受保护的上下文传递机制关联原始调用者。

具体选哪一种，要根据 agentgateway 的实际授权扩展能力确定。不能允许 Agent 直接伪造内部身份头，也不能因为请求来自网关就跳过对原始 Agent 身份的验证。

## 4. 撤销

撤销操作应在数据库事务中完成状态更新并写入审计事件。状态更新采用带条件的 SQL，确保只有 `ACTIVE` 或明确允许撤销的状态才能转为 `REVOKED`。

撤销成功后，还需要：

- 通知网关清理相关授权缓存；
- 阻止之后的调用；
- 通知 Agent Runtime 取消任务；
- 对无法中断的操作保留执行状态并进行追踪。

如果系统使用授权缓存，就必须定义明确的最大撤销延迟。高风险操作建议实时校验撤销状态；普通读取操作可以在经过评估后使用短 TTL 缓存。

我们直接进入工程设计。这份蓝图以**能开始编码、能做安全测试、后续能接入 agentgateway 和 MCP** 为目标，不再展开大而全的安全理论。

先定四个实现原则：

1. 委托服务是独立的 Spring Boot 应用，使用 MySQL 持久化授权状态。
2. 公司 SSO / IAM 负责认证；委托服务通过适配器获取用户权限，不自己发明用户身份。
3. 低风险操作可以自动授权，高风险操作必须经过审批；没有明确匹配规则时默认拒绝。
4. agentgateway 和 MCP Server 都要有安全检查，不能把委托服务当成一个“检查一次就永久放行”的系统。

# 一、整体工程结构

<box border radius="xl" padding={3} gap={2}>
  <box background="rgba(59,130,246,0.10)" radius="lg" padding={3} align="center" gap={1}>
    **公司 SSO / IAM**
    <text color="secondary" size="sm">用户登录、服务身份认证、企业权限查询</text>
  </box>
  <box align="center">
    <icon name="arrow-down" color="secondary" size="lg" />
  </box>
  <box border radius="lg" padding={3} align="center" gap={1}>
    **Agent Delegation Service**
    <text color="secondary" size="sm">Spring Boot · REST API · 授权策略 · 审批 · 撤销 · 审计</text>
  </box>
  <box align="center">
    <icon name="arrow-down" color="secondary" size="lg" />
  </box>
  <box background="surface-secondary" radius="lg" padding={3} align="center" gap={1}>
    **MySQL**
    <text color="secondary" size="sm">委托、资源范围、审批、策略版本、审计事件</text>
  </box>
  <divider />
  <grid columns={2} gap={2}>
    <grid-item>
      <box border radius="lg" padding={3} gap={1}>
        <icon name="workflow" size="lg" />
        **agentgateway**
        <text color="secondary" size="sm">请求拦截、调用身份验证、授权检查</text>
      </box>
    </grid-item>
    <grid-item>
      <box border radius="lg" padding={3} gap={1}>
        <icon name="server" size="lg" />
        **MCP Server**
        <text color="secondary" size="sm">执行前复核、资源级权限控制</text>
      </box>
    </grid-item>
  </grid>
</box>

这里要特别说明：委托服务不负责运行 Agent，也不负责执行 MCP 工具。它负责决定某次调用是否符合授权条件，并提供可以被调用方验证的决策依据。

## 推荐技术栈

| 组件 | 选择 |
|---|---|
| Java | Java 21 |
| 框架 | Spring Boot 3.x |
| 安全 | Spring Security OAuth2 Resource Server，按公司 IAM 的协议适配 |
| 持久化 | MySQL 8.x + Flyway |
| 数据访问 | Spring JDBC 或 MyBatis |
| API 文档 | Springdoc OpenAPI |
| 测试 | JUnit 5 + Testcontainers MySQL |
| 事件与审计 | 第一版数据库事务；后续需要时增加 Outbox 和消息队列 |

我会优先考虑 Spring JDBC 或 MyBatis，而不是一开始就大量依赖 ORM 自动处理状态变更。这个服务最重要的是明确的 SQL 条件、事务边界和并发控制。

# 二、MySQL 数据库设计

第一版先使用五张核心表。为了避免一个任务里同时包含读操作和高风险操作时，状态语义变得混乱，我建议**每条委托只包含一组风险等级一致、授权规则一致的操作范围**。同一任务需要读日志和重启服务时，可以建立两条关联同一个 `task_id` 的委托。

## 1. 委托主表 `delegation_grant`

```sql
CREATE TABLE delegation_grant (
    id BIGINT NOT NULL AUTO_INCREMENT,
    grant_id VARCHAR(64) NOT NULL,
    task_id VARCHAR(64) NOT NULL,

    grantor_issuer VARCHAR(255) NOT NULL,
    grantor_subject VARCHAR(255) NOT NULL,
    agent_id VARCHAR(128) NOT NULL,

    status VARCHAR(24) NOT NULL,
    risk_level VARCHAR(16) NOT NULL,
    policy_version VARCHAR(64) NOT NULL,

    created_at DATETIME(3) NOT NULL,
    valid_from DATETIME(3) NOT NULL,
    expires_at DATETIME(3) NOT NULL,
    revoked_at DATETIME(3) DEFAULT NULL,
    revoke_reason VARCHAR(512) DEFAULT NULL,

    version BIGINT NOT NULL DEFAULT 0,

    PRIMARY KEY (id),
    UNIQUE KEY uk_grant_id (grant_id),
    KEY idx_task_id (task_id),
    KEY idx_grantor (grantor_issuer, grantor_subject),
    KEY idx_agent_status (agent_id, status),
    KEY idx_status_expiry (status, expires_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

关键字段：

- `grant_id`：对外使用的委托标识，采用服务端生成的高熵随机 ID。
- `task_id`：同一任务可能有多条委托。
- `grantor_issuer + grantor_subject`：标识授权主体，避免不同身份系统的相同用户 ID 混淆。
- `agent_id`：逻辑 Agent 身份，不是某个 Java 进程的身份。
- `version`：用于乐观锁，防止审批、撤销等并发更新互相覆盖。

建议状态枚举：

| 状态 | 含义 |
|---|---|
| `PENDING_APPROVAL` | 等待审批 |
| `ACTIVE` | 当前可用，但每次调用仍须检查范围 |
| `REVOKED` | 已撤销，不允许恢复 |
| `EXPIRED` | 已过期 |
| `DENIED` | 申请被拒绝 |

即使状态仍然是 `ACTIVE`，只要 `expires_at` 已经过去，也必须拒绝调用。定时任务只是负责清理和更新展示状态，不能承担安全判断。

## 2. 授权范围表 `delegation_scope`

```sql
CREATE TABLE delegation_scope (
    id BIGINT NOT NULL AUTO_INCREMENT,
    grant_id VARCHAR(64) NOT NULL,

    resource_type VARCHAR(64) NOT NULL,
    resource_id VARCHAR(255) NOT NULL,
    action VARCHAR(128) NOT NULL,
    constraints_json JSON DEFAULT NULL,

    created_at DATETIME(3) NOT NULL,

    PRIMARY KEY (id),
    UNIQUE KEY uk_grant_scope (
        grant_id, resource_type, resource_id, action
    ),
    KEY idx_scope_lookup (
        grant_id, resource_type, resource_id, action
    )
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

示例：

| resource_type | resource_id | action | 约束 |
|---|---|---|---|
| `application` | `app-A` | `logs.read` | 最多回溯 30 分钟 |
| `database` | `db-test-01` | `database.query_readonly` | 仅允许指定只读查询类型 |

这里有个重要实现细节：**`constraints_json` 不是一段交给大模型解释的自然语言，而是由服务端按操作类型定义 Schema、校验和执行的结构化约束。**

例如，`logs.read` 的查询时间范围、最大返回条数，都应该有服务端强制执行的上限。仅仅在委托记录里写了“最多查询 30 分钟”，但日志 MCP 不检查时间范围，并不能构成真正的安全限制。

## 3. 审批表 `delegation_approval`

```sql
CREATE TABLE delegation_approval (
    id BIGINT NOT NULL AUTO_INCREMENT,
    approval_id VARCHAR(64) NOT NULL,
    grant_id VARCHAR(64) NOT NULL,

    approval_type VARCHAR(32) NOT NULL,
    status VARCHAR(24) NOT NULL,

    requested_by VARCHAR(255) NOT NULL,
    approver_issuer VARCHAR(255) DEFAULT NULL,
    approver_subject VARCHAR(255) DEFAULT NULL,

    request_hash VARCHAR(128) NOT NULL,
    decision_reason VARCHAR(1024) DEFAULT NULL,

    requested_at DATETIME(3) NOT NULL,
    decided_at DATETIME(3) DEFAULT NULL,
    expires_at DATETIME(3) NOT NULL,

    PRIMARY KEY (id),
    UNIQUE KEY uk_approval_id (approval_id),
    KEY idx_grant_status (grant_id, status)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

`request_hash` 绑定需要审批的具体操作内容。服务端应对规范化后的资源、操作、参数和关键约束计算摘要，不能直接对顺序不固定的 JSON 原文计算后就认为语义一致。

如果申请人更换目标服务器、SQL 或操作参数，必须重新计算摘要；摘要不匹配就不能复用旧审批。

## 4. 策略表 `delegation_policy`

```sql
CREATE TABLE delegation_policy (
    id BIGINT NOT NULL AUTO_INCREMENT,
    policy_id VARCHAR(128) NOT NULL,
    policy_version VARCHAR(64) NOT NULL,

    resource_type VARCHAR(64) NOT NULL,
    action VARCHAR(128) NOT NULL,
    environment VARCHAR(32) NOT NULL,
    data_classification VARCHAR(32) NOT NULL,

    risk_level VARCHAR(16) NOT NULL,
    decision_mode VARCHAR(32) NOT NULL,
    max_ttl_seconds INT NOT NULL,
    enabled TINYINT(1) NOT NULL DEFAULT 1,

    created_at DATETIME(3) NOT NULL,
    updated_at DATETIME(3) NOT NULL,

    PRIMARY KEY (id),
    UNIQUE KEY uk_policy_version (policy_id, policy_version),
    KEY idx_policy_match (
        resource_type, action, environment,
        data_classification, enabled
    )
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

`decision_mode` 可以先支持：

- `AUTO_ALLOW`：满足全部基础条件后自动授权。
- `REQUIRE_APPROVAL`：必须审批。
- `DENY`：禁止此类委托。

**没有匹配规则时默认拒绝。** 多条规则同时匹配时，第一版建议使用明确的优先级字段或严格的匹配唯一性约束，不要依赖数据库返回顺序决定授权。

## 5. 审计表 `delegation_audit`

```sql
CREATE TABLE delegation_audit (
    id BIGINT NOT NULL AUTO_INCREMENT,
    event_id VARCHAR(64) NOT NULL,
    grant_id VARCHAR(64) DEFAULT NULL,
    task_id VARCHAR(64) DEFAULT NULL,

    actor_issuer VARCHAR(255) DEFAULT NULL,
    actor_subject VARCHAR(255) DEFAULT NULL,
    event_type VARCHAR(64) NOT NULL,
    decision VARCHAR(16) DEFAULT NULL,
    reason_code VARCHAR(64) DEFAULT NULL,

    resource_type VARCHAR(64) DEFAULT NULL,
    resource_id VARCHAR(255) DEFAULT NULL,
    action VARCHAR(128) DEFAULT NULL,
    trace_id VARCHAR(128) DEFAULT NULL,

    created_at DATETIME(3) NOT NULL,

    PRIMARY KEY (id),
    UNIQUE KEY uk_event_id (event_id),
    KEY idx_grant_time (grant_id, created_at),
    KEY idx_task_time (task_id, created_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

审计表应限制应用账号的修改和删除权限，并根据企业要求设置备份、保留和防篡改机制。不要把访问令牌、密码、完整客户敏感数据或未经脱敏的 SQL 参数直接写入审计记录。

第一版可以将授权决策和关键状态变更同步写入同一 MySQL 事务。未来如果需要将审计发送到外部平台，再增加事务 Outbox，避免数据库提交成功但事件发送失败。

# 三、API 契约

建议统一使用 `/api/v1`，将“业务申请”和“安全决策”分成不同接口。

| 方法 | 路径 | 用途 |
|---|---|---|
| POST | `/api/v1/delegations` | 申请委托 |
| GET | `/api/v1/delegations/{grantId}` | 查询委托 |
| POST | `/api/v1/delegations/{grantId}/revoke` | 撤销委托 |
| POST | `/api/v1/approvals/{approvalId}/approve` | 审批通过 |
| POST | `/api/v1/approvals/{approvalId}/reject` | 审批拒绝 |
| POST | `/api/v1/authorization/check` | 检查工具调用是否允许 |

创建委托的请求示例：

```json
{
  "taskId": "task-456",
  "agentId": "ops-agent",
  "requestedScopes": [
    {
      "resourceType": "application",
      "resourceId": "app-A",
      "action": "logs.read",
      "constraints": {
        "maxLookbackMinutes": 30,
        "maxResults": 1000
      }
    }
  ]
}
```

成功响应示例：

```json
{
  "grantId": "dg-example",
  "taskId": "task-456",
  "status": "ACTIVE",
  "riskLevel": "LOW",
  "expiresAt": "2026-10-15T22:30:00+08:00"
}
```

这里的 ID 仅为示例。正式实现时应由服务端生成，不接受调用者指定 `grantId` 或自行指定风险等级。

如果是高风险操作，创建接口返回 `PENDING_APPROVAL`，而不是 `ACTIVE`。

建议使用统一响应错误码，例如：

- `IDENTITY_INVALID`
- `AGENT_NOT_APPROVED`
- `USER_PERMISSION_DENIED`
- `POLICY_NOT_FOUND`
- `APPROVAL_REQUIRED`
- `GRANT_INACTIVE`
- `SCOPE_MISMATCH`
- `APPROVAL_MISMATCH`

客户端只应获得必要的错误信息；内部拒绝原因可以写入受保护的审计记录。

# 四、Spring Boot 项目模块

可以从一个 Maven 单体服务开始，不需要拆成多个微服务。

```text
agent-delegation-service/
├── src/main/java/com/company/agentdelegation/
│   ├── config/
│   ├── security/
│   │   ├── AuthenticatedPrincipal.java
│   │   ├── IamPrincipalMapper.java
│   │   └── SecurityConfig.java
│   ├── identity/
│   │   ├── IdentityProvider.java
│   │   └── UserPermissionProvider.java
│   ├── registry/
│   │   └── AgentRegistryClient.java
│   ├── policy/
│   │   ├── RiskClassifier.java
│   │   ├── PolicyEvaluator.java
│   │   └── ScopeMatcher.java
│   ├── delegation/
│   │   ├── DelegationController.java
│   │   ├── DelegationService.java
│   │   ├── DelegationRepository.java
│   │   └── DelegationStateMachine.java
│   ├── approval/
│   ├── authorization/
│   └── audit/
└── src/test/
```

模块职责：

- `security`：验证公司 IAM 凭证，把可信身份转换成内部统一主体。
- `identity`：查询用户基础权限、资源访问权限。
- `registry`：检查 Agent 是否存在，以及对应版本和能力是否已获批准。
- `policy`：识别风险等级、匹配策略、比较资源和操作范围。
- `delegation`：创建、查询、撤销委托，管理状态机。
- `approval`：验证审批人权限，绑定被批准的具体操作。
- `authorization`：为 gateway 和 MCP 调用提供统一检查入口。
- `audit`：记录授权决策与状态变化。

第一版可以用接口隔离 IAM 和 Agent Registry，不必为这些系统创建复杂的领域模型。

## 身份适配器的设计

```java
public interface UserPermissionProvider {

    boolean hasPermission(
            AuthenticatedPrincipal principal,
            String resourceType,
            String resourceId,
            String action);
}
```

这个接口只是一个起点。生产环境更适合增加批量检查、超时、错误分类和权限版本等能力。

还要注意：**权限查询失败与权限不足是两种不同的情况，但在安全决策上都不能放行。** 可以分别返回 `PERMISSION_DENIED` 和 `PERMISSION_PROVIDER_UNAVAILABLE`，方便告警和排障。

`AuthenticatedPrincipal` 应包含从可信认证结果提取的主体标识、签发方和必要的组织信息。不要直接使用请求体中传来的用户名作为授权主体。

# 五、核心业务流程怎么实现

## 1. 创建委托

创建委托不是简单地插入一行数据库。建议按下面的顺序执行：

1. 验证调用者身份，确认是用户本人或经过认证的上游服务。
2. 从可信上下文获取用户身份；校验 Agent ID 和版本状态。
3. 对每个资源和操作查询用户基础权限、Agent 已批准能力。
4. 通过服务端策略计算风险等级，限制最长有效期。
5. 低风险申请创建 `ACTIVE` 委托；需要审批的申请创建 `PENDING_APPROVAL` 委托。
6. 在事务中保存委托、范围、必要的审批记录和审计事件。
7. 返回委托 ID 和状态。

其中第 3、4 步可能依赖外部系统。**不要在开启 MySQL 长事务后再调用远程 IAM 或 Registry**，否则外部接口慢时会长时间占用数据库连接和锁。应先完成外部检查，再开启短事务保存结果，并在后续授权检查时重新验证关键状态。

为避免请求重试导致重复创建，建议第一版支持 `Idempotency-Key`，并将它与可信调用主体、请求摘要关联。相同键但请求内容不同，必须拒绝。

## 2. 高风险审批

审批接口必须先验证当前审批人有审批权限，并检查是否允许审批自己的申请。

然后：

1. 读取待审批委托和审批记录。
2. 校验审批仍然有效，且状态为 `PENDING`。
3. 校验 `request_hash` 与被批准的操作一致。
4. 重新检查用户权限、Agent 能力和策略是否仍然有效。
5. 使用事务和条件更新完成状态迁移。
6. 记录审批决定及审计事件。

审批通过不应该直接跳过授权检查。即使审批已经完成，调用时仍需检查委托是否过期、是否撤销、目标资源和参数是否匹配。

## 3. 授权检查

建议网关每次准备执行受控工具调用时提交以下信息：

```json
{
  "grantId": "dg-example",
  "taskId": "task-456",
  "resourceType": "application",
  "resourceId": "app-A",
  "action": "logs.read",
  "parameters": {
    "lookbackMinutes": 10,
    "maxResults": 100
  }
}
```

但是请求里的这些字段只代表“请求者声称要执行什么”，并不能证明它的身份。

因此授权检查服务还必须获得一个经过验证的调用者上下文，例如：

- 由网关验证后建立的可信身份上下文；
- 或者由网关使用自己的工作负载凭证向授权服务发起的内部请求，并通过受保护的上下文传递机制关联原始调用者。

具体选哪一种，要根据 agentgateway 的实际授权扩展能力确定。不能允许 Agent 直接伪造内部身份头，也不能因为请求来自网关就跳过对原始 Agent 身份的验证。

## 4. 撤销

撤销操作应在数据库事务中完成状态更新并写入审计事件。状态更新采用带条件的 SQL，确保只有 `ACTIVE` 或明确允许撤销的状态才能转为 `REVOKED`。

撤销成功后，还需要：

- 通知网关清理相关授权缓存；
- 阻止之后的调用；
- 通知 Agent Runtime 取消任务；
- 对无法中断的操作保留执行状态并进行追踪。

如果系统使用授权缓存，就必须定义明确的最大撤销延迟。高风险操作建议实时校验撤销状态；普通读取操作可以在经过评估后使用短 TTL 缓存。

# 六、关键安全测试：这些用例必须通过

单元测试只证明代码按预期运行，安全测试要验证攻击者无法绕过边界。

| 测试场景 | 预期结果 |
|---|---|
| 未登录用户申请委托 | 拒绝 |
| 用户伪造其他人的 subject | 拒绝或忽略伪造字段 |
| Agent 未登记或能力未批准 | 拒绝 |
| 用户有 `app-A` 权限，却申请访问 `app-B` | 拒绝 |
| 委托只允许 `logs.read`，却请求 `service.restart` | 拒绝 |
| 委托已经过期，但数据库状态仍为 `ACTIVE` | 拒绝 |
| 委托被撤销，网关再次调用 | 拒绝 |
| 审批通过后修改 SQL 或目标资源 | 重新审批或拒绝 |
| 申请人试图审批自己的高风险操作 | 按策略拒绝 |
| IAM 超时或不可用 | 拒绝受保护操作 |
| 两个请求同时审批和撤销 | 不得重新激活已撤销委托 |
| 重复提交同一创建请求 | 幂等返回，不重复创建 |
| 直接绕过 gateway 调用 MCP | MCP 的独立防护仍然生效 |

并发测试尤其重要。比如一个线程审批，另一个线程撤销，必须确保最终状态只可能是符合状态机规则的结果，而不是由最后一次无条件更新决定。

建议使用 Testcontainers 启动真实 MySQL 测试事务和锁行为，不要只用 H2 代替。

# 七、接入 agentgateway 和 MCP 的实施顺序

第一版先打通一条完整链路，不必马上覆盖所有工具。

<list marker="number" gap={2}>
  <list-item>建立一个只读日志 MCP Server，定义 `logs.read` 的资源和参数规范。</list-item>
  <list-item>在 Delegation Service 中配置 `logs.read` 的低风险自动授权规则。</list-item>
  <list-item>让 Java Agent 通过 gateway 调用 MCP，并关联任务和委托 ID。</list-item>
  <list-item>让 gateway 验证调用身份并调用授权检查接口。</list-item>
  <list-item>在 MCP Server 中增加独立的资源权限校验，避免网关被绕过后失去保护。</list-item>
  <list-item>验证过期、撤销、越权和 IAM 故障场景，再加入需要审批的 `service.restart` 操作。</list-item>
</list>

这里需要做一次真实的 agentgateway 版本验证：它能否将经过验证的调用者身份、委托 ID 和任务上下文安全地传给外部授权服务，以及如何处理授权缓存。这些细节决定最终适配器的实现方式，不能仅凭通用架构假设。

# 八、实施优先级

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <badge color="success">P0</badge>
    <box flex="1" gap={1}>
      **授权核心**
      <text size="sm">MySQL DDL、状态机、策略判断、创建/撤销/检查接口、基础审计。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <badge>P1</badge>
    <box flex="1" gap={1}>
      **身份与审批集成**
      <text size="sm">接入公司 SSO / IAM、Agent Registry、高风险审批流程。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <badge>P2</badge>
    <box flex="1" gap={1}>
      **运行时安全**
      <text size="sm">agentgateway 外部授权、MCP 端校验、撤销通知、审计监控和后续沙箱身份关联。</text>
    </box>
  </row>
</box>

我的建议是先把 P0 做成可运行的 Spring Boot 工程，再按真实调用链推进 P1 和 P2。这样可以尽早验证授权模型，而不是先搭出很多基础设施，却还不知道它们能否正确保护一次实际的工具调用。

**下一步最有价值的工作，是把这份蓝图变成完整的工程骨架**：Maven `pom.xml`、上述五张表的 Flyway 迁移脚本、实体与 DTO、状态机、Repository、Controller，以及能够运行的 MySQL 集成测试。

# 六、关键安全测试：这些用例必须通过

单元测试只证明代码按预期运行，安全测试要验证攻击者无法绕过边界。

| 测试场景 | 预期结果 |
|---|---|
| 未登录用户申请委托 | 拒绝 |
| 用户伪造其他人的 subject | 拒绝或忽略伪造字段 |
| Agent 未登记或能力未批准 | 拒绝 |
| 用户有 `app-A` 权限，却申请访问 `app-B` | 拒绝 |
| 委托只允许 `logs.read`，却请求 `service.restart` | 拒绝 |
| 委托已经过期，但数据库状态仍为 `ACTIVE` | 拒绝 |
| 委托被撤销，网关再次调用 | 拒绝 |
| 审批通过后修改 SQL 或目标资源 | 重新审批或拒绝 |
| 申请人试图审批自己的高风险操作 | 按策略拒绝 |
| IAM 超时或不可用 | 拒绝受保护操作 |
| 两个请求同时审批和撤销 | 不得重新激活已撤销委托 |
| 重复提交同一创建请求 | 幂等返回，不重复创建 |
| 直接绕过 gateway 调用 MCP | MCP 的独立防护仍然生效 |

并发测试尤其重要。比如一个线程审批，另一个线程撤销，必须确保最终状态只可能是符合状态机规则的结果，而不是由最后一次无条件更新决定。

建议使用 Testcontainers 启动真实 MySQL 测试事务和锁行为，不要只用 H2 代替。

# 七、接入 agentgateway 和 MCP 的实施顺序

第一版先打通一条完整链路，不必马上覆盖所有工具。

<list marker="number" gap={2}>
  <list-item>建立一个只读日志 MCP Server，定义 `logs.read` 的资源和参数规范。</list-item>
  <list-item>在 Delegation Service 中配置 `logs.read` 的低风险自动授权规则。</list-item>
  <list-item>让 Java Agent 通过 gateway 调用 MCP，并关联任务和委托 ID。</list-item>
  <list-item>让 gateway 验证调用身份并调用授权检查接口。</list-item>
  <list-item>在 MCP Server 中增加独立的资源权限校验，避免网关被绕过后失去保护。</list-item>
  <list-item>验证过期、撤销、越权和 IAM 故障场景，再加入需要审批的 `service.restart` 操作。</list-item>
</list>

这里需要做一次真实的 agentgateway 版本验证：它能否将经过验证的调用者身份、委托 ID 和任务上下文安全地传给外部授权服务，以及如何处理授权缓存。这些细节决定最终适配器的实现方式，不能仅凭通用架构假设。

# 八、实施优先级

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <badge color="success">P0</badge>
    <box flex="1" gap={1}>
      **授权核心**
      <text size="sm">MySQL DDL、状态机、策略判断、创建/撤销/检查接口、基础审计。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <badge>P1</badge>
    <box flex="1" gap={1}>
      **身份与审批集成**
      <text size="sm">接入公司 SSO / IAM、Agent Registry、高风险审批流程。</text>
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <badge>P2</badge>
    <box flex="1" gap={1}>
      **运行时安全**
      <text size="sm">agentgateway 外部授权、MCP 端校验、撤销通知、审计监控和后续沙箱身份关联。</text>
    </box>
  </row>
</box>

我的建议是先把 P0 做成可运行的 Spring Boot 工程，再按真实调用链推进 P1 和 P2。这样可以尽早验证授权模型，而不是先搭出很多基础设施，却还不知道它们能否正确保护一次实际的工具调用。

**下一步最有价值的工作，是把这份蓝图变成完整的工程骨架**：Maven `pom.xml`、上述五张表的 Flyway 迁移脚本、实体与 DTO、状态机、Repository、Controller，以及能够运行的 MySQL 集成测试。


---

## 👤 用户

有一种权限控制场景是 mcp网关，网关控制某个agent对mcp是否有调用权限，由网关作为中心统一配置，一直是走授权体系，先请求iam，然后再请求权限系统，再直接判断agent是否可以直接调用mcp，这两种做法有没有区别，边界在哪里？


---

## 🤖 ChatGPT

有区别，而且这两种做法的边界，恰好是企业 Agent 平台权限设计中一个很重要的问题。

我先把两种方案用更直白的方式区分开：

- **方案 A：授权体系决策。** MCP 网关收到调用请求后，先通过 IAM 确认调用主体身份，再向权限系统查询授权关系，最后决定是否允许调用。
- **方案 B：网关本地决策。** MCP 网关统一维护 Agent 到 MCP 的授权配置，请求到来后识别 Agent，直接根据本地配置决定是否放行。

这里有个关键点：**IAM 认证和权限判断不是一回事。** IAM 解决“你是谁”，权限系统解决“你有权做什么”。网关本地配置也能实现权限判断，但不代表它天然具备完整的身份认证和授权治理能力。

## 一、两种方案的核心区别

<box border radius="lg" padding={3} gap={3}>
  <title size="md">方案 A：IAM + 权限系统</title>
  <box background="surface-secondary" radius="md" padding={3} align="center">
    Agent 发起 MCP 调用
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box border radius="md" padding={3} align="center">
    MCP 网关验证身份（IAM / 令牌）
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box border radius="md" padding={3} align="center">
    权限系统判断：该 Agent 是否被授权调用该 MCP？
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box background="surface" border radius="md" padding={3} align="center">
    网关执行放行或拒绝
  </box>
  <text color="secondary" size="sm">授权关系主要由权限系统维护，网关负责执行决策。</text>
</box>

<box border radius="lg" padding={3} gap={3}>
  <title size="md">方案 B：网关本地策略</title>
  <box background="surface-secondary" radius="md" padding={3} align="center">
    Agent 发起 MCP 调用
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box border radius="md" padding={3} align="center">
    MCP 网关验证身份（IAM / 可信令牌）
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box border radius="md" padding={3} align="center">
    查询网关本地授权配置
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box background="surface" border radius="md" padding={3} align="center">
    网关执行放行或拒绝
  </box>
  <text color="secondary" size="sm">授权关系由网关侧维护或缓存，网关直接执行决策。</text>
</box>

注意：**方案 B 不一定意味着绕过 IAM。** 网关完全可以使用 IAM 签发的令牌认证 Agent，只把“Agent 能调用哪些 MCP”这类权限放在网关管理。

有区别，而且这两种做法的边界，恰好是企业 Agent 平台权限设计中一个很重要的问题。

我先把两种方案用更直白的方式区分开：

- **方案 A：授权体系决策。** MCP 网关收到调用请求后，先通过 IAM 确认调用主体身份，再向权限系统查询授权关系，最后决定是否允许调用。
- **方案 B：网关本地决策。** MCP 网关统一维护 Agent 到 MCP 的授权配置，请求到来后识别 Agent，直接根据本地配置决定是否放行。

这里有个关键点：**IAM 认证和权限判断不是一回事。** IAM 解决“你是谁”，权限系统解决“你有权做什么”。网关本地配置也能实现权限判断，但不代表它天然具备完整的身份认证和授权治理能力。

## 一、两种方案的核心区别

<box border radius="lg" padding={3} gap={3}>
  <title size="md">方案 A：IAM + 权限系统</title>
  <box background="surface-secondary" radius="md" padding={3} align="center">
    Agent 发起 MCP 调用
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box border radius="md" padding={3} align="center">
    MCP 网关验证身份（IAM / 令牌）
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box border radius="md" padding={3} align="center">
    权限系统判断：该 Agent 是否被授权调用该 MCP？
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box background="surface" border radius="md" padding={3} align="center">
    网关执行放行或拒绝
  </box>
  <text color="secondary" size="sm">授权关系主要由权限系统维护，网关负责执行决策。</text>
</box>

<box border radius="lg" padding={3} gap={3}>
  <title size="md">方案 B：网关本地策略</title>
  <box background="surface-secondary" radius="md" padding={3} align="center">
    Agent 发起 MCP 调用
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box border radius="md" padding={3} align="center">
    MCP 网关验证身份（IAM / 可信令牌）
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box border radius="md" padding={3} align="center">
    查询网关本地授权配置
  </box>
  <row justify="center">
    <icon name="arrow-down" size="lg" />
  </row>
  <box background="surface" border radius="md" padding={3} align="center">
    网关执行放行或拒绝
  </box>
  <text color="secondary" size="sm">授权关系由网关侧维护或缓存，网关直接执行决策。</text>
</box>

注意：**方案 B 不一定意味着绕过 IAM。** 网关完全可以使用 IAM 签发的令牌认证 Agent，只把“Agent 能调用哪些 MCP”这类权限放在网关管理。

## 二、真正的边界在哪里？

不能简单理解成“权限系统比较正规，网关配置比较简单”。更准确的区分是：**谁负责定义授权规则、谁负责维护授权关系、谁负责执行授权决策。**

| 对比维度 | IAM + 权限系统 | 网关本地配置 |
|---|---|---|
| 身份认证 | 通常由 IAM 负责 | 仍然应该验证可信身份 |
| 授权关系存在哪里 | 统一权限系统 | 网关配置或网关策略库 |
| 谁决定是否允许调用 | 权限系统提供决策，网关执行 | 网关自己决策 |
| 权限是否可被其他系统复用 | 通常较容易 | 通常局限于网关，除非另行开放 |
| 复杂权限规则 | 更适合多业务、多维度授权 | 适合明确、简单的调用准入规则 |
| 权限生命周期 | 可统一处理申请、审批、回收、审计 | 需要网关自行建设或集成治理能力 |
| 运行时依赖 | 取决于是否实时查询权限系统 | 本地决策时可减少远程依赖 |
| 适用场景 | 企业统一权限治理 | MCP 接入准入和集中管控 |

这里还有一个容易忽略的情况：**网关本地配置也可以是一个真正的授权系统。** 如果它有独立的授权数据模型、审批流程、权限回收、审计和策略管理，它就不只是几条配置，而是在承担一个特定领域的授权管理职责。

反过来，如果权限系统只负责返回一个简单的 `allow/deny`，它也不一定比网关本地配置更完善。

## 三、放到 Agent 平台中，应该如何划分？

我建议把权限拆成三个层次，而不是让所有权限都经过同一个系统判断。

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" size="44px" align="center" justify="center">
      <icon name="shield-check" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第一层：身份可信**

      <text color="secondary" size="sm">IAM / 身份基础设施</text>

      解决“这个请求究竟来自哪个 Agent、哪个用户或哪个工作负载”。网关必须从可信令牌或可信的工作负载身份中确定调用者，不能直接相信请求体里自报的 `agentId`。
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" size="44px" align="center" justify="center">
      <icon name="network" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第二层：Agent 到 MCP 的接入授权**

      <text color="secondary" size="sm">MCP 网关策略 / 统一授权系统</text>

      解决“这个 Agent 是否有资格调用这个 MCP 服务或工具”。例如，`ops-agent` 可以调用日志查询 MCP，但不能调用生产环境的数据库变更 MCP。

      如果只有少量、简单的授权关系，可以直接由网关统一配置；如果多个网关、平台和业务系统需要复用这些授权关系，就更适合由统一权限系统维护，网关负责执行。
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" size="44px" align="center" justify="center">
      <icon name="lock-keyhole" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第三层：具体操作的业务授权**

      <text color="secondary" size="sm">MCP 服务 / 业务权限系统 / Delegation Service</text>

      解决“当前任务是否可以执行这项具体操作”。例如，Agent 虽然可以调用数据库 MCP，但是否能查询某张表、读取某个客户的数据，或者执行 `UPDATE`，还需要进一步判断。

      如果 Agent 是代表某个用户执行任务，还要检查用户权限和委托给 Agent 的权限范围，不能因为 Agent 有 MCP 调用权，就继承了所有用户权限。
    </box>
  </row>
</box>

## 四、举个具体例子

假设你有一个运维 Agent，它能使用日志 MCP、数据库 MCP 和服务器操作 MCP。

| 请求 | 网关接入授权 | 具体操作授权 |
|---|---|---|
| 查询应用日志 | 允许调用日志 MCP | 还要检查允许查询的应用、环境和日志范围 |
| 查询生产数据库 | 允许调用数据库 MCP | 还要检查库表、数据范围和用户权限 |
| 重启生产服务 | 允许调用服务器操作 MCP | 还要检查任务委托、操作风险及审批状态 |

这能说明一个很重要的边界：

**“Agent 能不能调用 MCP”与“Agent 能不能通过 MCP 做这件事”不是同一个问题。**

网关很适合控制前者；后者可能需要业务系统或专门的授权服务参与。对于高风险操作，不能仅仅因为网关放行了 MCP 调用，就允许执行。

## 五、我会怎么设计你们的系统？

结合企业内部的 Agent 平台，我建议先把架构设计成这样：

- **IAM**：负责用户、服务身份和身份认证。
- **MCP 网关**：负责统一入口、身份校验、Agent 到 MCP 的准入控制、限流和审计。
- **权限系统**：当授权规则需要跨系统复用、统一审批和回收时，作为授权关系的权威来源。
- **Agent Delegation Service**：负责具体任务中的委托授权、授权范围、有效期、审批与撤销。
- **MCP 服务端**：对敏感数据和高风险业务操作做最终权限校验，避免调用绕过网关时失去保护。

不需要为了架构完整，就让每次 MCP 请求都依次远程调用 IAM、权限系统和 Delegation Service。身份令牌可以在符合安全要求的前提下本地验证；稳定的准入权限可以缓存；而撤销时效要求高、风险高的操作，应实时或通过具备明确一致性保证的机制校验。

最终需要明确的是：**每类权限只能有清晰的权威来源，但执行检查可以分布在多个位置。** 网关可以缓存授权决策，也可以本地执行简单策略；不能因此造成多个系统各自维护一份互相冲突的授权事实。

如果你们现在讨论的仅仅是“管理员在网关页面上勾选哪些 Agent 可以访问哪些 MCP”，我会倾向于先让网关承担这一层准入控制，而不是一开始就把它接入复杂的统一权限体系。等到出现跨系统复用、用户级权限继承、任务级委托、审批与撤销等需求，再明确哪些授权关系应该由独立的权限服务维护。

---

如果你愿意，我可以：

- 继续讲解实施优先级和后续步骤
- 示范如何创建审批接口代码示例
- 解释授权检查API具体调用流程

## 二、真正的边界在哪里？

不能简单理解成“权限系统比较正规，网关配置比较简单”。更准确的区分是：**谁负责定义授权规则、谁负责维护授权关系、谁负责执行授权决策。**

| 对比维度 | IAM + 权限系统 | 网关本地配置 |
|---|---|---|
| 身份认证 | 通常由 IAM 负责 | 仍然应该验证可信身份 |
| 授权关系存在哪里 | 统一权限系统 | 网关配置或网关策略库 |
| 谁决定是否允许调用 | 权限系统提供决策，网关执行 | 网关自己决策 |
| 权限是否可被其他系统复用 | 通常较容易 | 通常局限于网关，除非另行开放 |
| 复杂权限规则 | 更适合多业务、多维度授权 | 适合明确、简单的调用准入规则 |
| 权限生命周期 | 可统一处理申请、审批、回收、审计 | 需要网关自行建设或集成治理能力 |
| 运行时依赖 | 取决于是否实时查询权限系统 | 本地决策时可减少远程依赖 |
| 适用场景 | 企业统一权限治理 | MCP 接入准入和集中管控 |

这里还有一个容易忽略的情况：**网关本地配置也可以是一个真正的授权系统。** 如果它有独立的授权数据模型、审批流程、权限回收、审计和策略管理，它就不只是几条配置，而是在承担一个特定领域的授权管理职责。

反过来，如果权限系统只负责返回一个简单的 `allow/deny`，它也不一定比网关本地配置更完善。

## 三、放到 Agent 平台中，应该如何划分？

我建议把权限拆成三个层次，而不是让所有权限都经过同一个系统判断。

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" size="44px" align="center" justify="center">
      <icon name="shield-check" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第一层：身份可信**

      <text color="secondary" size="sm">IAM / 身份基础设施</text>

      解决“这个请求究竟来自哪个 Agent、哪个用户或哪个工作负载”。网关必须从可信令牌或可信的工作负载身份中确定调用者，不能直接相信请求体里自报的 `agentId`。
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" size="44px" align="center" justify="center">
      <icon name="network" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第二层：Agent 到 MCP 的接入授权**

      <text color="secondary" size="sm">MCP 网关策略 / 统一授权系统</text>

      解决“这个 Agent 是否有资格调用这个 MCP 服务或工具”。例如，`ops-agent` 可以调用日志查询 MCP，但不能调用生产环境的数据库变更 MCP。

      如果只有少量、简单的授权关系，可以直接由网关统一配置；如果多个网关、平台和业务系统需要复用这些授权关系，就更适合由统一权限系统维护，网关负责执行。
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <box background="surface-secondary" radius="md" size="44px" align="center" justify="center">
      <icon name="lock-keyhole" size="xl" />
    </box>
    <box flex="1" gap={1}>
      **第三层：具体操作的业务授权**

      <text color="secondary" size="sm">MCP 服务 / 业务权限系统 / Delegation Service</text>

      解决“当前任务是否可以执行这项具体操作”。例如，Agent 虽然可以调用数据库 MCP，但是否能查询某张表、读取某个客户的数据，或者执行 `UPDATE`，还需要进一步判断。

      如果 Agent 是代表某个用户执行任务，还要检查用户权限和委托给 Agent 的权限范围，不能因为 Agent 有 MCP 调用权，就继承了所有用户权限。
    </box>
  </row>
</box>

## 四、举个具体例子

假设你有一个运维 Agent，它能使用日志 MCP、数据库 MCP 和服务器操作 MCP。

| 请求 | 网关接入授权 | 具体操作授权 |
|---|---|---|
| 查询应用日志 | 允许调用日志 MCP | 还要检查允许查询的应用、环境和日志范围 |
| 查询生产数据库 | 允许调用数据库 MCP | 还要检查库表、数据范围和用户权限 |
| 重启生产服务 | 允许调用服务器操作 MCP | 还要检查任务委托、操作风险及审批状态 |

这能说明一个很重要的边界：

**“Agent 能不能调用 MCP”与“Agent 能不能通过 MCP 做这件事”不是同一个问题。**

网关很适合控制前者；后者可能需要业务系统或专门的授权服务参与。对于高风险操作，不能仅仅因为网关放行了 MCP 调用，就允许执行。

## 五、我会怎么设计你们的系统？

结合企业内部的 Agent 平台，我建议先把架构设计成这样：

- **IAM**：负责用户、服务身份和身份认证。
- **MCP 网关**：负责统一入口、身份校验、Agent 到 MCP 的准入控制、限流和审计。
- **权限系统**：当授权规则需要跨系统复用、统一审批和回收时，作为授权关系的权威来源。
- **Agent Delegation Service**：负责具体任务中的委托授权、授权范围、有效期、审批与撤销。
- **MCP 服务端**：对敏感数据和高风险业务操作做最终权限校验，避免调用绕过网关时失去保护。

不需要为了架构完整，就让每次 MCP 请求都依次远程调用 IAM、权限系统和 Delegation Service。身份令牌可以在符合安全要求的前提下本地验证；稳定的准入权限可以缓存；而撤销时效要求高、风险高的操作，应实时或通过具备明确一致性保证的机制校验。

最终需要明确的是：**每类权限只能有清晰的权威来源，但执行检查可以分布在多个位置。** 网关可以缓存授权决策，也可以本地执行简单策略；不能因此造成多个系统各自维护一份互相冲突的授权事实。

如果你们现在讨论的仅仅是“管理员在网关页面上勾选哪些 Agent 可以访问哪些 MCP”，我会倾向于先让网关承担这一层准入控制，而不是一开始就把它接入复杂的统一权限体系。等到出现跨系统复用、用户级权限继承、任务级委托、审批与撤销等需求，再明确哪些授权关系应该由独立的权限服务维护。

---

## 👤 用户

我现在又有了新的疑问，agent需要唯一的身份吗，需要给它在IAM建立独立的身份吗？ 有意义吗，还是说我只需要识别用户身份，所有的鉴权都基于用户身份？


---

## 🤖 ChatGPT

我认为，**企业级 Agent 平台需要识别 Agent 自身的身份，但不一定要给每一个 Agent 都在现有 IAM 中创建一个独立账号。** 这两件事要分开考虑。

你提出的两种思路，实际上对应两种不同的安全模型：

- **只识别用户：** Agent 被视为用户的一个执行工具，所有权限都从用户身份出发。
- **同时识别用户和 Agent：** 用户决定“要做什么”，Agent 的身份说明“是谁在执行”，系统再判断这次执行是否被允许。

对于你正在设计的企业 Agent 平台，我更推荐第二种。原因不是为了给 Agent 多发一个 ID，而是因为 Agent 作为独立的软件执行主体，有自己的能力、运行环境、调用链和安全边界。

## 一、为什么只识别用户不够？

假设一名员工登录运维平台，发起了查询生产日志的请求。

这个请求经过了：

用户 → 运维 Agent → 日志 MCP → 日志服务。

如果系统只记录用户身份，日志服务看到的可能始终是同一个员工。但实际上，系统还需要回答：

- 是哪个 Agent 发起了查询？是经过审核的运维 Agent，还是一个未经批准的实验 Agent？
- 这个 Agent 是否有资格调用日志 MCP？
- 它是否正在执行这个用户授权的任务？
- 如果 Agent 被发现存在漏洞，能否只禁用它，而不影响用户正常使用其他业务？
- 如果某个 Agent 被滥用，能否查出它发起了哪些调用？

**用户身份回答的是“谁提出了需求”，Agent 身份回答的是“哪个执行主体在行动”。** 两者不能相互替代。

当然，如果系统只有一个完全受控的 Agent，所有操作都由固定后端代码执行，短期内也可以不为每个 Agent 建立独立的 IAM 身份。但随着 Agent 数量、权限和运行环境增加，这种简化会逐渐成为限制。

我认为，**企业级 Agent 平台需要识别 Agent 自身的身份，但不一定要给每一个 Agent 都在现有 IAM 中创建一个独立账号。** 这两件事要分开考虑。

你提出的两种思路，实际上对应两种不同的安全模型：

- **只识别用户：** Agent 被视为用户的一个执行工具，所有权限都从用户身份出发。
- **同时识别用户和 Agent：** 用户决定“要做什么”，Agent 的身份说明“是谁在执行”，系统再判断这次执行是否被允许。

对于你正在设计的企业 Agent 平台，我更推荐第二种。原因不是为了给 Agent 多发一个 ID，而是因为 Agent 作为独立的软件执行主体，有自己的能力、运行环境、调用链和安全边界。

## 一、为什么只识别用户不够？

假设一名员工登录运维平台，发起了查询生产日志的请求。

这个请求经过了：

用户 → 运维 Agent → 日志 MCP → 日志服务。

如果系统只记录用户身份，日志服务看到的可能始终是同一个员工。但实际上，系统还需要回答：

- 是哪个 Agent 发起了查询？是经过审核的运维 Agent，还是一个未经批准的实验 Agent？
- 这个 Agent 是否有资格调用日志 MCP？
- 它是否正在执行这个用户授权的任务？
- 如果 Agent 被发现存在漏洞，能否只禁用它，而不影响用户正常使用其他业务？
- 如果某个 Agent 被滥用，能否查出它发起了哪些调用？

**用户身份回答的是“谁提出了需求”，Agent 身份回答的是“哪个执行主体在行动”。** 两者不能相互替代。

当然，如果系统只有一个完全受控的 Agent，所有操作都由固定后端代码执行，短期内也可以不为每个 Agent 建立独立的 IAM 身份。但随着 Agent 数量、权限和运行环境增加，这种简化会逐渐成为限制。

## 二、Agent 身份是不是必须放进 IAM？

不一定。这里要区分三个概念。

| 概念 | 解决什么问题 | 例子 |
|---|---|---|
| Agent 唯一标识 | 我们说的是哪个 Agent？ | `agent-ops-001` |
| Agent 身份凭证 | 如何证明当前调用确实来自这个 Agent？ | 经过验证的令牌、工作负载证书 |
| Agent 授权 | 这个 Agent 被允许做什么？ | 可以调用日志 MCP，但不能执行生产数据库变更 |

这三个概念经常被混在一起。

例如，你可以在 Agent Registry 中登记 `agent-ops-001`，让它成为全局唯一的逻辑标识。但**仅仅有这个 ID，并不能证明调用者真的就是这个 Agent**。如果任何调用者都可以在 HTTP 请求里填写这个 ID，那么它就只是一个标签，不是可信身份。

同样，给 Agent 在 IAM 创建一个账号，也不自动意味着权限设计就正确了。

我建议按下面的方式区分：

- **Agent Registry**：管理 Agent 的逻辑身份、名称、版本、负责人、能力和生命周期。
- **IAM / 工作负载身份系统**：在需要时，为 Agent 的运行实例提供可验证的身份凭证。
- **授权系统**：判断这个身份能否调用某个 MCP、访问某类资源或执行某项操作。

因此，Agent 需要唯一身份，不等于每个 Agent 都必须是 IAM 中的一条独立用户记录。

## 三、如果所有鉴权都基于用户身份，会有什么问题？

最核心的问题是：你无法独立约束和管理 Agent 这个执行主体。

举个例子：

<box border radius="lg" padding={3} gap={3}>
  <title size="md">方案 A：只看用户</title>
  <box background="surface-secondary" radius="md" padding={3} align="center">
    员工 U
  </box>
  <row justify="center">
    <icon name="arrow-down" />
  </row>
  <box border radius="md" padding={3} align="center">
    Agent A / Agent B
  </box>
  <row justify="center">
    <icon name="arrow-down" />
  </row>
  <box border radius="md" padding={3} align="center">
    MCP 网关只检查员工 U 的权限
  </box>
  <text color="secondary" size="sm">问题：如果员工有权限，但 Agent 本身不应该执行某项操作，仅靠用户身份就很难表达这一限制。</text>
  <divider />
  <title size="md">方案 B：同时检查用户与 Agent</title>
  <box background="surface-secondary" radius="md" padding={3} align="center">
    员工 U：提出需求、提供用户权限上下文
  </box>
  <row justify="center">
    <icon name="plus" />
  </row>
  <box background="surface-secondary" radius="md" padding={3} align="center">
    Agent A：可信执行身份、已获准的能力
  </box>
  <row justify="center">
    <icon name="arrow-down" />
  </row>
  <box border radius="md" padding={3} align="center">
    授权决策：用户权限 + Agent 能力 + 任务授权 + 安全策略
  </box>
  <text color="secondary" size="sm">优势：既可以限制用户能做什么，也可以限制 Agent 能通过哪些工具做什么。</text>
</box>

但要注意：**同时检查用户和 Agent，不等于把两者的权限简单相加。** 一般应采用限制性组合。

例如，用户有生产数据库的读取权限，但某个 Agent 只被批准调用测试环境数据库 MCP，那么这个 Agent 就不应该因为用户权限较高而获得生产环境访问能力。

## 四、我推荐的权限模型

对于企业 Agent 平台，可以先采用以下原则：

\[
\text{最终允许}
=
\text{身份可信}
\land
\text{Agent能力允许}
\land
\text{用户权限允许}
\land
\text{任务授权有效}
\land
\text{安全策略通过}
\]

其中并非每种场景都必须检查所有项目。例如，没有用户参与的定时运维任务，就没有用户权限这一项，但必须有明确的服务身份和任务授权策略。

不同 Agent 类型，可以采取不同的身份策略：

| Agent 类型 | 是否需要独立运行身份 | 建议 |
|---|---|---|
| 长期运行的企业业务 Agent | 通常需要 | 为 Agent 服务或工作负载建立可信身份 |
| 高权限运维 Agent | 强烈建议 | 独立身份、严格能力限制、完整审计 |
| 动态创建的临时子 Agent | 需要可验证的执行身份，但不必创建永久 IAM 用户 | 通过运行时身份或受控的委托机制识别 |
| 仅在进程内部执行的简单子流程 | 不一定需要独立凭证 | 可以继承受控运行环境身份，同时保留独立的逻辑 ID 和调用链记录 |

这里还有一个重要的区别：Agent 的逻辑身份可以长期存在，而它的运行实例可能不断创建、销毁。不要把每次对话、每个任务或者每个临时子 Agent 都变成一个永久 IAM 账号。

## 五、结合你们的平台，具体怎么落地？

你们已有统一 IAM，正在建设 Agent Registry、MCP 网关和 Delegation Service。我建议不要一开始就改造 IAM 的用户模型，而是分层建设：

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <icon name="users" size="xl" color="secondary" />
    <box flex="1" gap={1}>
      **统一 IAM**

      管理员工身份、登录认证，以及现有的用户和服务身份。
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <icon name="boxes" size="xl" color="secondary" />
    <box flex="1" gap={1}>
      **Agent Registry**

      为每个逻辑 Agent 分配稳定且唯一的 `agent_id`，管理其负责人、状态、版本和允许的能力。
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <icon name="key-round" size="xl" color="secondary" />
    <box flex="1" gap={1}>
      **运行时身份**

      对需要独立识别的 Agent 工作负载，建立可验证的身份凭证。后续可以评估 SPIFFE/SPIRE 等机制，但不必在第一阶段就引入。
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <icon name="shield-check" size="xl" color="secondary" />
    <box flex="1" gap={1}>
      **MCP 网关 + Delegation Service**

      网关识别可信调用者并执行 MCP 准入控制；Delegation Service 管理某个用户任务授予 Agent 的具体权限、范围、有效期、审批和撤销。
    </box>
  </row>
</box>

这样，你不必一开始就把所有 Agent 注册成 IAM 用户，却能建立清晰的身份和权限边界。等到需要工作负载级身份、跨主机调用或者临时子 Agent 隔离时，再扩展运行时身份机制。

**我的最终建议是：Agent 必须有稳定、唯一、可追踪的逻辑身份；需要独立信任边界的 Agent，还必须有可验证的运行时身份。至于是否在现有 IAM 中创建独立身份，要根据 Agent 的运行方式和权限风险决定，而不是一刀切。**

---

如果你愿意，我可以：

- 详细说明审批流程中的并发控制实现
- 展开Agentgateway与委托服务的身份验证细节
- 描述实际接入时的授权缓存策略和风险管理

## 二、Agent 身份是不是必须放进 IAM？

不一定。这里要区分三个概念。

| 概念 | 解决什么问题 | 例子 |
|---|---|---|
| Agent 唯一标识 | 我们说的是哪个 Agent？ | `agent-ops-001` |
| Agent 身份凭证 | 如何证明当前调用确实来自这个 Agent？ | 经过验证的令牌、工作负载证书 |
| Agent 授权 | 这个 Agent 被允许做什么？ | 可以调用日志 MCP，但不能执行生产数据库变更 |

这三个概念经常被混在一起。

例如，你可以在 Agent Registry 中登记 `agent-ops-001`，让它成为全局唯一的逻辑标识。但**仅仅有这个 ID，并不能证明调用者真的就是这个 Agent**。如果任何调用者都可以在 HTTP 请求里填写这个 ID，那么它就只是一个标签，不是可信身份。

同样，给 Agent 在 IAM 创建一个账号，也不自动意味着权限设计就正确了。

我建议按下面的方式区分：

- **Agent Registry**：管理 Agent 的逻辑身份、名称、版本、负责人、能力和生命周期。
- **IAM / 工作负载身份系统**：在需要时，为 Agent 的运行实例提供可验证的身份凭证。
- **授权系统**：判断这个身份能否调用某个 MCP、访问某类资源或执行某项操作。

因此，Agent 需要唯一身份，不等于每个 Agent 都必须是 IAM 中的一条独立用户记录。

## 三、如果所有鉴权都基于用户身份，会有什么问题？

最核心的问题是：你无法独立约束和管理 Agent 这个执行主体。

举个例子：

<box border radius="lg" padding={3} gap={3}>
  <title size="md">方案 A：只看用户</title>
  <box background="surface-secondary" radius="md" padding={3} align="center">
    员工 U
  </box>
  <row justify="center">
    <icon name="arrow-down" />
  </row>
  <box border radius="md" padding={3} align="center">
    Agent A / Agent B
  </box>
  <row justify="center">
    <icon name="arrow-down" />
  </row>
  <box border radius="md" padding={3} align="center">
    MCP 网关只检查员工 U 的权限
  </box>
  <text color="secondary" size="sm">问题：如果员工有权限，但 Agent 本身不应该执行某项操作，仅靠用户身份就很难表达这一限制。</text>
  <divider />
  <title size="md">方案 B：同时检查用户与 Agent</title>
  <box background="surface-secondary" radius="md" padding={3} align="center">
    员工 U：提出需求、提供用户权限上下文
  </box>
  <row justify="center">
    <icon name="plus" />
  </row>
  <box background="surface-secondary" radius="md" padding={3} align="center">
    Agent A：可信执行身份、已获准的能力
  </box>
  <row justify="center">
    <icon name="arrow-down" />
  </row>
  <box border radius="md" padding={3} align="center">
    授权决策：用户权限 + Agent 能力 + 任务授权 + 安全策略
  </box>
  <text color="secondary" size="sm">优势：既可以限制用户能做什么，也可以限制 Agent 能通过哪些工具做什么。</text>
</box>

但要注意：**同时检查用户和 Agent，不等于把两者的权限简单相加。** 一般应采用限制性组合。

例如，用户有生产数据库的读取权限，但某个 Agent 只被批准调用测试环境数据库 MCP，那么这个 Agent 就不应该因为用户权限较高而获得生产环境访问能力。

## 四、我推荐的权限模型

对于企业 Agent 平台，可以先采用以下原则：

\[
\text{最终允许}
=
\text{身份可信}
\land
\text{Agent能力允许}
\land
\text{用户权限允许}
\land
\text{任务授权有效}
\land
\text{安全策略通过}
\]

其中并非每种场景都必须检查所有项目。例如，没有用户参与的定时运维任务，就没有用户权限这一项，但必须有明确的服务身份和任务授权策略。

不同 Agent 类型，可以采取不同的身份策略：

| Agent 类型 | 是否需要独立运行身份 | 建议 |
|---|---|---|
| 长期运行的企业业务 Agent | 通常需要 | 为 Agent 服务或工作负载建立可信身份 |
| 高权限运维 Agent | 强烈建议 | 独立身份、严格能力限制、完整审计 |
| 动态创建的临时子 Agent | 需要可验证的执行身份，但不必创建永久 IAM 用户 | 通过运行时身份或受控的委托机制识别 |
| 仅在进程内部执行的简单子流程 | 不一定需要独立凭证 | 可以继承受控运行环境身份，同时保留独立的逻辑 ID 和调用链记录 |

这里还有一个重要的区别：Agent 的逻辑身份可以长期存在，而它的运行实例可能不断创建、销毁。不要把每次对话、每个任务或者每个临时子 Agent 都变成一个永久 IAM 账号。

## 五、结合你们的平台，具体怎么落地？

你们已有统一 IAM，正在建设 Agent Registry、MCP 网关和 Delegation Service。我建议不要一开始就改造 IAM 的用户模型，而是分层建设：

<box border radius="lg" padding={3} gap={3}>
  <row align="start" gap={3}>
    <icon name="users" size="xl" color="secondary" />
    <box flex="1" gap={1}>
      **统一 IAM**

      管理员工身份、登录认证，以及现有的用户和服务身份。
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <icon name="boxes" size="xl" color="secondary" />
    <box flex="1" gap={1}>
      **Agent Registry**

      为每个逻辑 Agent 分配稳定且唯一的 `agent_id`，管理其负责人、状态、版本和允许的能力。
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <icon name="key-round" size="xl" color="secondary" />
    <box flex="1" gap={1}>
      **运行时身份**

      对需要独立识别的 Agent 工作负载，建立可验证的身份凭证。后续可以评估 SPIFFE/SPIRE 等机制，但不必在第一阶段就引入。
    </box>
  </row>
  <divider color="subtle" />
  <row align="start" gap={3}>
    <icon name="shield-check" size="xl" color="secondary" />
    <box flex="1" gap={1}>
      **MCP 网关 + Delegation Service**

      网关识别可信调用者并执行 MCP 准入控制；Delegation Service 管理某个用户任务授予 Agent 的具体权限、范围、有效期、审批和撤销。
    </box>
  </row>
</box>

这样，你不必一开始就把所有 Agent 注册成 IAM 用户，却能建立清晰的身份和权限边界。等到需要工作负载级身份、跨主机调用或者临时子 Agent 隔离时，再扩展运行时身份机制。

**我的最终建议是：Agent 必须有稳定、唯一、可追踪的逻辑身份；需要独立信任边界的 Agent，还必须有可验证的运行时身份。至于是否在现有 IAM 中创建独立身份，要根据 Agent 的运行方式和权限风险决定，而不是一刀切。**
