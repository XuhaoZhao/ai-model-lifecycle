# 别再给每个 Agent 塞一个 Sandbox：下一代 Agent 架构正在重写

> **来源**：微信公众号「人工智能前线」（原创）
> **发布**：2026 年 9 月 30 日 14:54 · 北京
> **链接**：<https://mp.weixin.qq.com/s/0GOYMWUJt20Auol5dRshXg>
> **存档说明**：2026-10-22 经 Playwright 全文抓取，仅供研究参考；原文配图未包含。
> **配套图解**：见同目录 [architecture.html](./architecture.html)

---

过去一年，Agent 领域里最常见的一种架构，是把整个 Agent 放进一个 Sandbox 里运行。

Agent 的上下文、文件系统、Shell、工具，甚至整个执行循环，都待在一个隔离环境里。

这很自然。

因为早期很多 Agent，本来就是从“本地 Coding Agent”演化出来的。

Claude Code、Codex CLI 这类产品，本质上都是一个安装在电脑上的程序，它能访问本机文件、Shell 和工具。

当我们想把它搬到云上时，最直接的办法就是：

**给每个 Agent 一个云端 Sandbox。**

于是，Agent 的扩展问题看起来就变成了：

怎么扩更多 Sandbox。

但作者在连续做了两个产品之后，逐渐意识到：

**这可能是在扩展错误的东西。**

真正需要扩展的，不是 Sandbox 本身，而是一个能够长期运行、可以恢复、可以跨设备、可以独立调度资源的 Agent 系统。

这篇文章讲的，就是作者从“Agent 住在 Sandbox 里”，一步步走向“Sandbox 只是 Agent 临时调用的一种资源”的过程。

## 一、最早的思路：一个 Agent，对应一个 Sandbox

作者最开始做云端 Agent 时，目标非常直接：

**把 Agent 扩起来。**

当时，他已经长期使用本地 Coding Agent。

在这种模式下，Agent 就像一个程序，住在某台电脑里。

它周围就是这台电脑的：

文件。

Shell。

工具。

网络环境。

Manus 关于 Context Engineering 的文章，让“云端版 Agent”这件事变得很具体：

把 Agent 放进一个虚拟机 Sandbox。

Sandbox 的文件系统就成为可以保存和恢复的上下文。

因此，在作者的第一个产品 Anoma 里，他采取了最直接的路线：

**把本地 Agent Loop 原封不动搬进 E2B。**

这时，Sandbox 就成了 Agent 能力的最小单位。

要扩 Agent，就扩 Sandbox。

看上去很合理。

但很快，问题就出现了。

## 二、Sandbox 扩容，并没有真正消除复杂性

使用 E2B，并不意味着你就不再需要管理 Sandbox。

用户越多，仍然意味着：

要创建更多 Sandbox。

要决定哪些 Sandbox 可以复用。

要回收空闲 Sandbox。

还要受 Sandbox 提供商生命周期规则的限制。

与此同时，很多状态又被留在 Sandbox 里。

比如：

会话历史。

执行 Trace。

临时文件。

如果要排查问题，就必须进入持有用户私有数据的环境。

而 API Key 又不能安全地直接放进 Sandbox。

于是，你还需要额外建一个 Gateway，专门代理模型请求。

慢慢地，你会发现一个趋势：

**为了让 Sandbox 里的 Agent 更好用，你不断把职责从 Sandbox 里搬出去。**

于是系统逐渐变成三层：

第一层，是 Sandbox 里的完整 Agent。

第二层，是外部控制平面，负责 Sandbox 生命周期、容量管理和模型流量。

第三层，是真正面向用户的产品界面。

问题在于：

虽然外面多了一层控制平面，但每个 Sandbox 里依然装着一整套 Agent。

所以产品要扩容时，本质上还是在扩“Agent + Sandbox”的完整捆绑单元。

## 三、第二个产品，让问题变得更清楚

到 2026 年 2 月，作者已经意识到，这个架构需要重做。

他的第二个产品 Tetral，不再只是一个固定界面的应用。

目标变成了：

**做一个通用 Agent SDK。**

开发者可以把云端 Agent 嵌入自己的产品，而不用自己写 Agent Loop，也不用自己维护底层基础设施。

一旦目标从“一个应用”变成“一个 Agent 平台”，Sandbox 作为扩展单位的问题就更加明显。

作者考虑过两种路线。

第一种是 Kubernetes。

Kubernetes 很适合运行稳定服务。

但频繁创建和销毁 Sandbox 的场景，与传统 Pod 生命周期并不完全匹配。

当 Sandbox 高频创建、释放时，调度和 Pod 生命周期本身就会成为瓶颈。

第二种是自己用 EC2、Firecracker 和控制平面构建 microVM 系统。

这当然可以做。

但结果就是：

你正在自己造一家 Sandbox 提供商。

而且即使造完，Agent 的容量仍然被“你能提供多少台计算机”所限制。

无论走哪条路，Sandbox 调度依然是一个独立的基础设施问题。

这时作者意识到：

**Agent 和 Sandbox 的扩展曲线，本来就不是一回事。**

Agent Runtime 是稳定的、共享的。

它可以承载很多不同任务。

而 Sandbox 是隔离的、突发的、一次性的。

如果把这两者绑在一起扩容，就会让系统天然变重。

所以他决定把它们拆开。

## 四、关键转变：Computer 是 Agent 调用的资源，不是 Agent 居住的地方

这是整篇文章最核心的观点之一：

**一台计算机，应该是 Agent 调用的一种资源，而不是 Agent 所在的地方。**

拆开之后：

Agent Runtime 变成一个长期稳定的服务。

Sandbox 则只是多个外部资源之一。

和它并列的还有：

模型。

记忆。

文件。

凭证。

工具。

Linux、Windows、macOS，也不再代表不同种类的 Agent。

它们只是不同形式的“计算机资源”。

Agent 需要执行 Shell 或文件系统操作时，再调用对应计算环境。

没有需要时，就不必从一开始就给每个会话配一台完整 Sandbox。

这一步改变了整个系统的边界。

接下来的问题就变成：

**Agent Runtime 到底应该负责什么？哪些职责应该被移到外面？**

## 五、重新定义 Runtime：它只负责“计算下一步”

作者对 Agent Runtime 的定义非常明确。

Runtime 是一个持续运行的程序。

它负责解释模型输出，并让 Agent 能够受控地访问：

模型。

网络。

文件。

工具。

子 Agent。

外部世界。

Claude Code 和 Codex 本地其实已经很像 Runtime。

但在云上，如果 Runtime 想成为真正可以替换、可以扩展的计算单元，就必须满足一个条件：

**Agent 的身份、状态和资源不能只存在 Runtime 进程里。**

否则 Runtime 一死，Agent 就跟着死。

因此，所有具有不同生命周期、不同所有权、不同扩展模式的职责，都应该被移到 Runtime 外部。

作者把系统拆成了几个独立部分。

### Gateway：负责模型提供商和外部连接

Runtime 不应该自己处理：

OpenAI、Anthropic 等不同 Provider 的协议差异。

API Key。

OAuth Token。

长连接。

流式响应协议。

这些内容并不决定 Agent 下一步应该做什么。

所以它们被移到 Gateway。

Gateway 负责把不同 Provider 的请求统一成一种标准接口。

而且 Gateway 本身不保存 Agent 会话状态。

这样模型流量就可以独立扩容。

### PostgreSQL：成为真正的状态源

Runtime Pod 随时可能被替换。

所以用户输入、执行进度、工具结果，都必须脱离 Runtime 存活。

作者把 PostgreSQL 作为系统的最终事实来源。

但 Runtime 并不直接操作数据库。

因为如果 Runtime 直接读写数据库，就会和：

数据库 Schema。

事务。

顺序。

所有权。

重试协议。

深度耦合。

于是中间又增加了 Bridge。

### Bridge：负责状态边界和事务

Bridge 负责：

确认某个 Runtime 是否仍然拥有某个 Session。

检查执行顺序。

提交状态变化。

返回提交结果。

Runtime 只声明：

“我下一步想做什么。”

Bridge 决定：

“这个动作现在是否有效，是否可以提交。”

### Queue：负责可靠投递

仅仅保存状态还不够。

一个用户输入被接受以后，还必须确保它最终会被某个 Runtime 消费。

所以 Queue 负责：

任务顺序。

租约。

重试。

取消。

死信。

换句话说：

数据库解决“事实有没有保存”。

Queue 解决“这件事最终有没有被送到正确的执行者”。

### Sandbox Service：负责计算机生命周期

Sandbox 也被从 Runtime 中拿出来。

Runtime 不再负责创建、销毁、恢复 Sandbox。

它只声明：

“这个 Thread 现在需要做一个计算机操作。”

Sandbox Service 再负责：

是否复用已有机器。

是否启动。

是否新建。

是否替换。

以及真正执行命令。

### Public API 和 Event Stream：负责产品接口

用户请求由 Public API 接收。

外部客户端需要看的状态更新，则通过 Event Stream 暴露。

这两个部分都不真正执行 Agent。

它们只是 Agent 系统的产品接口。

## 六、当所有外围职责都拆出去之后，Runtime 只剩下一件事

最后，Runtime 真正保留下来的，就是：

**对 Session 和 Thread 进行计算。**

作者定义了三个层次。

### Workspace：长期资源空间

Workspace 保存一些长期存在的资源。

比如：

文件。

Memory。

代码仓库。

凭证。

Agent Version。

### Session：一个持续任务

Session 代表一个完整的 Agent 工作过程。

它包含：

所使用的 Agent Version。

资源配置。

历史。

生命周期。

### Thread：Session 内的一条独立执行路径

每个 Session 一开始有一个 Root Thread。

如果 Agent 创建 Subagent，就会出现 Child Thread。

每个 Thread 都有自己独立的执行顺序和上下文。

这样，Session 可以同时拥有多条执行路径。

## 七、Reducer：真正决定“下一步做什么”的核心

对于每个 Thread，系统都会重建当前状态。

然后把状态交给一个纯粹的 Reducer。

Reducer 不访问数据库。

不调用网络。

不直接执行工具。

它只做一件事：

**根据当前状态，推导下一步有效动作。**

这个动作可能是：

调用模型。

调用工具。

等待更多输入。

结束任务。

也就是说，Runtime Pod 本质上只是一个“可替换的计算宿主”。

它可以同时让多个 Session 和 Thread 保持热状态。

但它并不真正拥有这些 Session。

如果 Runtime Pod 消失，系统只需要在另一台 Pod 上重新加载状态，然后继续计算。

这就是“Durable Agent Work”的基础。

## 八、关键原则：先记录，再执行

如果 Runtime 随时可以消失，就会出现一个非常危险的问题：

假设一个操作已经发出，但 Runtime 在中途挂了。

新的 Runtime 接手以后，怎么知道：

这个动作从来没开始？

正在执行？

还是已经执行完，只是结果还没记录？

如果靠猜，很容易出现两种错误：

要么漏执行。

要么把一个具有外部副作用的操作执行两次。

所以作者采用了类似 Write-Ahead Log 的原则：

**任何会推进 Agent 状态、或者制造外部副作用的动作，都必须先被记录，再真正执行。**

比如：

准备调用模型。

先提交一条“即将调用模型”的声明。

Bridge 确认并写入数据库。

然后 Runtime 才能真正向 Gateway 发请求。

工具调用也是一样。

先记录：

Agent 想调用什么工具。

只有记录成功后，Sandbox Service 或其他外部服务才会真正执行。

这样，Runtime 即使中途消失，新的 Runtime 也可以从已提交的事实中恢复。

它不需要恢复原来的进程。

它只需要恢复：

**这个 Agent 已经正式走到哪一步。**

## 九、为什么需要稳定身份和幂等性？

每一个声明都有稳定 ID。

这个 ID 本质上就是幂等键。

如果 Runtime 因为网络问题没有收到确认，它可以重新提交同一个声明。

Bridge 会发现：

这不是一个新动作。

而是已经存在的动作。

于是返回之前保存的结果。

这样就避免了重复创建状态和重复执行。

如果有人试图用同一个 ID 提交不同内容，系统则直接拒绝。

这让“恢复”从模糊猜测，变成基于已提交记录的确定过程。

## 十、Session Log 不只是聊天记录

作者强调：

Session Log 不是简单的 Conversation History。

它是一个完整的逻辑执行记录。

里面包括：

用户输入。

模型请求边界。

工具调用。

工具结果。

中断。

结束。

每个 Thread 都保持自己的顺序。

Session 级别则有一个统一事件序列。

同时，系统还维护一份专门给模型加载的 Context Projection。

也就是说：

**执行历史和模型上下文不是一回事。**

模型为了节约上下文，可能只加载压缩后的 Summary。

但原始执行记录仍然完整保留。

这很重要。

因为过去发生的历史，未来还可以变成可搜索的上下文。

而不是要求一份 Summary 永远承载整个过去。

## 十一、可靠投递：保存了，不等于已经送达

即使 Agent 状态已经持久化，还有一个问题：

如果用户发来消息时，没有 Runtime Pod 可用怎么办？

作者采用类似 Outbox 的方式。

当消息被接受时，同一个数据库事务里会同时创建：

输入记录。

Inbox 记录。

Queue Job。

只要这次事务成功，Public API 就可以告诉客户端：

请求已经接受。

至于什么时候真正送到 Runtime，可以异步完成。

如果通知丢失，也没关系。

因为 Queue Job 仍然在 PostgreSQL 里。

后续 Polling 仍然可以发现。

系统不依赖一次性的通知来保证消息存在。

## 十二、为什么同一个 Thread 仍然要严格按顺序？

对同一个上下文而言，后面的输入不能随便超车。

如果一个消息仍在投递或者重试，普通后续消息就不能绕过去。

否则同一个 Agent Context 会出现顺序错乱。

但是其他 Thread 可以继续并行。

所以这里就出现了 Agent 系统非常核心的一个设计原则：

**同一条上下文严格有序，不同执行路径尽可能并行。**

中断则作为特殊消息处理。

它不能被普通消息堵住。

## 十三、模型 API Key 不应该进入 Runtime，更不应该进入 Sandbox

作者对凭证边界非常重视。

Runtime 发起模型请求时，请求里不包含 API Key 或 OAuth Token。

Gateway 会验证：

这个 Runtime Pod 的 Kubernetes 身份。

它当前是否仍然拥有目标 Session 和 Thread。

然后再由 Gateway 加载该 Session 选择的凭证。

如果凭证缺失、被撤销、已归档或无法解密，请求应该直接失败。

不能悄悄回退到其他凭证。

OAuth 刷新也发生在 Gateway。

新的 Token 只会被注入外部 Provider 请求。

明文凭证不会进入：

Runtime。

Bridge。

Session Log。

Sandbox。

这就把“Agent 能使用某个能力”和“Agent 实际持有密钥”彻底分开。

## 十四、MCP 工具也应该采用相同的边界

MCP 同样如此。

Runtime 可以看到：

工具名称。

描述。

输入 Schema。

但看不到：

Server Credential。

连接细节。

真正调用 MCP Tool 时：

Runtime 先声明它想调用哪个工具。

Connector 再验证权限、注入凭证、执行请求。

结果先经过 Bridge。

最后才能进入 Agent Context。

整个系统的理念是一致的：

**Runtime 声明需要什么能力。**

**真正持有连接和凭证的是外部服务。**

## 十五、Computer 只是资源，而且应该按需分配

很多能力根本不需要完整电脑。

比如：

调用模型。

Web 请求。

MCP。

Memory 操作。

真正需要执行环境的，主要是：

Shell。

文件系统。

所以 Computer 不应该在 Session 创建时就默认分配。

而应该在第一次真的需要时再分配。

这就是 Lazy Allocation。

如果已经有兼容的 Computer 正在运行，就复用。

否则 Sandbox Service 可以：

启动。

创建。

替换。

准备文件。

注入必要状态。

然后再执行命令。

多个同时到来的操作，如果都需要同一个冷启动 Computer，也可以共享同一次激活过程。

而不是重复启动多个环境。

## 十六、Sandbox 甚至未必永远是最合适的 Computer 实现

当前 Tetral 会给一个 Session 懒加载一个 Daytona Computer。

但作者强调：

这只是当前实现，不是最终抽象。

未来可能出现不同类型的执行资源。

比如：

普通文件访问，可能只需要虚拟文件系统。

一个很小的隔离任务，只需要轻量容器。

强隔离的不可信代码，需要 Sandbox。

平台相关任务，则可能必须运行在 Linux、macOS 或 Windows 上。

所以真正的抽象应该是：

**Agent 请求一种计算资源。**

而不是：

**每个 Agent 天生绑定一个 Sandbox。**

## 十七、Agent 真正需要扩展的是两个维度

让 Computer 可以独立调度，并不能让同一条上下文同时向两个方向推进。

因为两个并发模型请求如果读取完全相同的边界，就会产生互相竞争的“下一状态”。

所以真正的并行需要独立执行路径。

Tetral 把扩展分成两个维度。

### 维度一：Thread

一个任务内部，可以创建多个 Child Thread。

每个 Thread 有独立上下文和执行路径。

这适合：

主 Agent 继续工作时，让 Subagent 并行检查实现。

不同调查方向同时推进。

一条分支做审计，另一条做设计。

### 维度二：Session

不同完整任务之间，则使用不同 Session。

比如：

代码实现。

文档。

安全审计。

知识维护。

它们可能需要完全不同的：

根 Agent。

上下文。

Agent Version。

凭证。

生命周期。

但这些 Session 仍然可以共享同一个 Workspace 里的长期资源。

所以作者总结：

**Thread 用来扩同一个任务里的并行路径。**

**Session 用来扩不同任务。**

## 十八、Subagent 不应该只是“进程里的一个对象”

如果 Child Agent 只存在内存里，会出现两个典型问题：

Runtime 挂了之后，Child Agent 消失。

请求响应丢失后，可能重复创建 Child Agent。

所以创建 Child Agent 也必须遵守同一个持久化原则。

先把 Spawn 操作提交。

同一个事务里完成：

创建 Child Thread。

建立 Parent-Child 关系。

保存初始 Context。

保存 Instruction。

创建 Inbox。

创建 Delivery Job。

只有这些都提交成功，主 Agent 才真正得到 Child Thread ID。

如果重试同一个 Spawn，系统会返回同一个 Child Thread，而不是重复创建。

Parent 和 Child 之后仍然是两个独立上下文。

Parent 后续收到的新消息，不会自动进入 Child。

Child 也不能直接改 Parent Context。

它们之间所有通信，都必须重新通过有序投递。

## 十九、权限隔离和授权，是两个完全不同的问题

把凭证放在 Gateway 外面，只解决了一半问题。

它解决的是：

**Agent 有没有资格访问某种能力。**

但没有解决：

**这一次具体操作，到底可不可以执行。**

这是两个概念。

Credential Isolation 是能力访问边界。

Authorization 是动作批准边界。

比如：

一个 Agent 拥有 GitHub Token。

不代表它提出的每一个 GitHub 操作都应该被允许。

所以真正的授权决策，必须发生在执行前。

而且应该放在 Agent 自己之外。

因为：

**不能让提出操作的 Agent，同时成为自己操作是否安全的最终裁判。**

## 二十、为什么授权必须发生在外部边界？

作者引用 OpenAI 在 Hugging Face 上发生过的事件来说明：

Agent 自己识别“这件事是否危险”，和系统真正阻止危险操作，是两回事。

这并不意味着加一个 LLM Reviewer 就一定能防止事故。

真正重要的是：

**任何 Tool Call 在影响外部世界之前，都必须跨过一道独立授权边界。**

Tetral 设计了 approve_for_me 模式。

当模型提出一个高风险工具调用时：

Runtime 不会直接执行。

而是先进入 Tool Gate。

Gate 根据：

当前 Session 的 Approval Mode。

Tool Catalog。

这类工具的 Policy。

判断是否必须经过独立审核。

这时，操作还只是一个“提案”。

还没有真正成为可执行 Tool Call。

## 二十一、Reviewer 也不能和主 Agent 混在一起

审核本身会作为一个独立 Thread 运行。

Reviewer 会看到：

待执行动作。

相关上下文。

主 Agent 当前输出。

同一轮里其他 Tool Call。

实际 Policy。

但这些内容都只是“证据”。

不是指令。

这点非常重要。

因为：

文件。

网页。

Tool Result。

甚至主 Agent 的输出。

都可能包含 Prompt Injection。

所以平台自己的 Reviewer Policy 必须和被审核内容分离。

Reviewer 最终只返回：

风险等级。

上下文里是否存在用户授权。

允许或拒绝。

原因。

如果输出格式异常，应该视为审核失败。

而不是默认允许。

## 二十二、Reviewer 的意见，也不能直接变成权限

Reviewer 给出 allow，并不等于操作立刻可以执行。

这个判断先要被 Bridge 写入 PostgreSQL。

只有记录成功后，Runtime 才能再次运行 Tool Gate。

这一次，Gate 会拿到已经提交的审核结果。

然后再决定：

允许。

拒绝。

或升级到用户审批。

只有这次二次 Gate 判断通过以后，真正的 Tool Use 才会被提交。

提交之后，执行任务才允许进入 Queue。

也就是说：

**审核结果本身不是权限。**

**被系统正式记录并重新验证后的结果，才是权限。**

作者把这个边界设计得非常保守。

如果 Runtime 在“提案已经产生，但授权尚未完成”时崩溃，那么这个提案不会在恢复后自动继续执行。

宁可牺牲这一步的恢复能力，也不能让一个失去上下文的孤儿授权继续生效。

## 二十三、下一代扩展问题，不再只是“多跑几个 Agent”

当 Agent 工作能够跨越单一进程、单一计算机之后，扩展问题就彻底变了。

扩的已经不只是 Runtime 容量。

还包括：

Agent 身份。

授权。

历史。

资源。

工具。

凭证。

外部副作用。

执行环境。

这些东西都要随着 Agent 工作量一起扩。

所以系统真正要解决的，是：

**如何让身份、状态、执行和资源彼此独立，但仍然保持连续性和可控性。**

Tetral 的设计是：

Agent 在 Sandbox 外。

Credential 在 Agent 和 Sandbox 之外。

Computer 只是临时资源。

这样：

执行可以失败。

Runtime 可以替换。

Computer 可以更换。

身份和历史仍然不丢。

## 二十四、Agent 需要成为一个真正“可寻址的参与者”

当 Agent Identity 被从某个具体客户端或电脑里拿出来之后，Agent 就可以跨界面持续存在。

同一个 Agent 可以出现在：

Terminal。

浏览器。

手机。

GitHub。

团队产品。

关闭某个客户端，并不会结束 Agent Session。

也不会把 Agent 从一台电脑“迁移”到另一台。

因为真正连续的是：

**有序的 Session History。**

Identity 还带来责任链。

某个外部操作可以追溯到：

哪个 Agent Version。

哪个 Session。

哪个 Thread。

哪条 Policy。

哪次 Review。

用户或组织仍然承担最终责任。

但 Agent 本身开始成为一个可以识别、可以追踪的行动主体。

作者举了几个类似趋势：

RoboBun 让 GitHub 上的工作归因到公开 Agent Identity。

Raft 把具名、带状态的 Agent 放在同一个多 Agent Workspace。

Claude Tag 给 Slack Channel 一个异步 Claude，并允许团队把工作并行委派给多个 Claude。

这些产品界面不同。

但它们有共同变化：

**Agent 不再只是藏在用户账号背后的“自动补全能力”。**

它开始变成一个可寻址的参与者。

## 二十五、身份最终会和“有限授权”绑定在一起

当 Agent 有独立身份以后，就可以给它受限权限。

比如：

创建账号。

拿到 API Token。

购买域名。

部署服务。

申请一个只对特定商家和特定金额有效的付款凭证。

重点是：

Agent 并不需要看到真实信用卡信息。

用户或组织仍然是 Principal，也就是最终授权主体。

Agent 只获得：

有限的。

可追踪的。

可撤销的授权。

这才是 Agent 真正进入长期生产环境所需要的身份模型。

## 二十六、CLI 不应该承担所有云端能力

作者认为，CLI 很适合那些天然绑定某台操作系统、某个仓库、某个编译器或调试器的任务。

但很多事情根本不应该因为“Agent 要做”就先启动一台电脑。

比如：

读一个 Issue。

发一条消息。

调用一个 SaaS API。

这些能力更适合通过 MCP 暴露。

Connector 负责带上 Session Credential。

Runtime 的 Tool Gate 负责策略和审批。

因此，作者预期未来：

CLI 继续负责 Computer-native Work。

MCP 则成为 Cloud-native Capability 的主要接口。

## 二十七、多 Agent 不等于协作更有效

文章后面讨论了另一个很重要的问题：

Agent 数量增加，并不自动意味着系统会更好。

很多多 Agent 系统只是：

更多 Agent。

更多 Token。

更多消息。

更多噪声。

甚至共享 Coding Swarm 越大，最终真正能合并的工作比例反而更低。

Tetral 的一个核心下注是：

**先对抗式评审，再合并。**

也就是说：

在工作进入共享 Workspace 之前，其他 Agent 必须主动挑战它的结论和证据。

不是简单“同意”。

而是去找：

哪里错了。

证据是否足够。

结论是否站得住。

最后再由人类决定是否接受。

## 二十八、真正应该共享的，不是 Conversation，而是 Accepted Artifact

作者对比了两种多 Agent 协作结构。

第一种是 Conversation-centered。

多个 Agent 在一个共享 Channel 里聊天。

但每个 Agent 的 Working Memory 会逐渐分叉。

消息可以帮助协调。

但并不会形成真正稳定的共同认知。

第二种是 Artifact-centered。

每个 Session 都从同一个已经接受的 Workspace Version 出发。

然后独立探索。

产出 Proposal。

互相 Review。

再经过 Human Review。

最后只有真正被接受的结果，才会 Merge 进入下一个 Workspace Version。

作者的观点是：

**聊天可以协调工作。**

**只有被正式接纳的 Artifact，才应该改变未来 Agent 所共同信任的事实。**

这和软件工程非常像。

讨论可以很多。

但真正进入主干的，只有经过审核的 Commit。

## 二十九、为什么这对未来评测和训练也很重要？

如果每一次 Agent 行为都有完整轨迹，那么这些轨迹就可以反过来变成 Eval Data。

比如：

被拒绝的 Tool Call。

被 Reviewer 推翻的判断。

验证失败。

人工修正。

这些都能告诉我们：

模型具体在哪里做错了。

同一个任务使用不同 Agent Version 执行，还可以比较不同模型真实行为差异。

这样，真实部署轨迹就可以成为未来：

评测。

对齐。

训练。

的重要数据。

当然，前提是：

Provenance，也就是来源链条仍然完整。

私有数据被清理。

人类判断明确标记了什么是成功、什么是失败。

作者强调：

训练可以减少危险提案。

但真实外部副作用仍然不能只依赖模型“学会安全”。

它仍然需要：

确定性 Policy。

验证。

内容检查。

分类器。

独立 Review。

组织规则。

必要时人工升级。

## 三十、真正的下一代扩展瓶颈，会不断向外围移动

模型能力越强，外围系统承受的压力越大。

Agent 工作时间越长，并行路径越多，就会产生更多：

Runtime Transition。

测试。

Computer。

文件。

网络请求。

Session Trace。

外部副作用。

最终，最短板的那个系统，就会成为新的扩展瓶颈。

这也是整篇文章标题里的：

**The Next Scaling Problem。**

问题不再只是模型推理能扩多少。

而是 Agent 系统周围所有基础设施，能不能跟上。

## 三十一、甚至编译器和工具链，都可能先成为瓶颈

文章提到一个很容易被忽略的事实。

Agent 写代码的速度上去之后：

Edit。

Build。

Test。

这些传统软件工具链的吞吐也会被推到极限。

作者举了两个例子。

Bun 从 Zig 转向 Rust，其中一个原因就是为了降低内存、提高吞吐，并解决稳定性和内存安全问题。

TypeScript 开始原生 Go 实现，也是因为原来的实现已经无法很好扩展到最大的代码库。

早期版本速度大约提升 10 倍，同时显著降低内存。

在作者自己的开发服务器上：

Tetral 某些集成路径跑一次要 7—8 分钟。

完整验证一次要约半小时。

这说明：

**未来限制 Agent 开发速度的，未必先是模型。**

也可能是：

编译。

测试。

验证。

传统开发基础设施。

## 三十二、Sandbox 本身也会成为独立扩展问题

Sandbox 和稳定服务的负载形态完全不同。

它们经常是：

短生命周期。

突发创建。

暂停。

恢复。

释放。

回收。

这意味着系统不仅要关心启动速度，还要关心：

批量创建速度。

挂载资源速度。

Resume 延迟。

高频调度。

控制面并发。

当规模上去以后，传统服务编排体系不一定是最优解。

作者因此认为：

**完整 Sandbox 不应该成为每一种操作的默认边界。**

简单文件访问也许只需要虚拟文件系统。

不可信代码才需要强隔离。

平台相关工作才需要对应操作系统。

真正的问题不是：

“能同时跑多少 Sandbox？”

而是：

**有多少工作真的必须经过完整 Computer？**

## 三十三、未来也许需要比 Sandbox 更轻的计算原语

Actor System 提供了另一个方向。

它们可以做到：

轻量。

可寻址。

可以睡眠。

可以唤醒。

不需要每次都准备一整台电脑。

这种模式未来怎么映射到：

Session。

Thread。

Resource Worker。

作者还没有给出最终答案。

但至少有一个结论已经很明确：

**完整 Sandbox，不应该是所有 Agent 操作的默认执行边界。**

## 三十四、执行环境可以消失，但产出必须留下

一个 Disposable Environment 真正有价值的前提是：

它死了以后，工作结果仍然存在。

这又带出新的资源边界：

Storage。

Session Event。

Tool Result。

日志。

都要进入有序的 Session History。

而：

代码。

文档。

Build Artifact。

则必须回到：

代码仓库。

对象存储。

Workspace File。

只有这样，下一个执行环境才能继续接上。

Agent 可以是临时计算。

但工作成果不能是临时的。

## 三十五、Agent 系统最终可能比“模型 API”更像产品本体

文章最后提到一个很有意思的变化。

过去模型配置里，人们最关心：

temperature。

top_k。

top_p。

但在 Claude Opus 4.6 之后，这些参数正在被弱化。

新的 Agent Interface 更强调：

Effort。

Tools。

Skills。

MCP Server。

Context。

Policy。

Orchestration。

也就是说：

未来真正需要配置的，可能不再只是一个“模型”。

而是一个包含模型在内的完整 Agent System。

模型只是其中一个组成部分。

## 三十六、Tetral 现在仍然只是 Alpha

作者对当前系统状态非常坦诚。

Tetral 目前只是运行在个人 k3s Cluster 上的 Alpha。

并不是已经验证过的大规模生产云平台。

目前还有很多问题没有解决。

比如：

Runtime Placement 还没有真正考虑容量。

Backpressure 还没有把入口控制、Queue 压力和扩缩容完整联动起来。

多节点长期恢复能力还没有被充分验证。

Runtime 发布新版本时，还无法优雅 Drain，正在执行的 Turn 可能会中断。

这些问题都指向下一步工作。

未来需要：

按 Runtime 容量分配 Session。

支持跨节点恢复。

让 Queue 压力直接控制接入和 Worker 数量。

加快 Computer 激活和挂载。

提供更轻量的文件访问。

支持自托管 Provider。

让一个 Agent 同时协调多台 Computer。

小型部署还应该能够把多个逻辑服务合并到更少进程。

而大型生产部署则能够把它们分别扩容。

## 三十七、最终方向：Agent-native Cloud

作者最后给 Tetral 定义的产品方向是：

**Hosted Agent System Service。**

也就是说：

产品团队只需要定义：

工具。

Reviewer。

Thread Policy。

资源。

模型。

产品界面。

而不需要从头重建：

会话连续性。

身份。

授权。

恢复。

资源调度。

底层系统应该自动提供这些能力。

同一套 Agent Cloud 还应该支持：

更安全的外部执行。

跨多个客户端、多个计算机的产品。

完整的 Session Evidence。

事后分析。

Eval。

未来训练。

## 三十八、实现会变，但三个核心下注不会变

作者认为，未来真正落地以后，很多具体技术实现一定会改变。

有些服务可能合并。

Runtime 可能不再使用 Bun。

Storage 可能拆开。

Queue、Bridge 甚至 Kubernetes 拓扑都可能被替换。

但有三个核心判断不会变。

**第一，Agent 是 Cloud Native 的。**

它不应该天然属于某台本地电脑。

**第二，需要扩展的是整个 Agent System，而不只是模型。**

真正的瓶颈会在身份、状态、执行、工具、存储、安全和资源之间不断迁移。

**第三，Runtime 应该继续以 Agent 的计算为中心，而外围系统负责保存连续性和控制。**

Runtime 负责：

现在下一步该做什么。

系统负责：

你是谁。

你做到哪一步。

你能访问什么。

你的历史是什么。

你的动作是否被允许。

## 最后：下一代 Agent 的核心问题，已经从“模型够不够强”转向“系统能不能承载它”

这篇文章真正想表达的，并不是：

“Sandbox 不好。”

Sandbox 依然非常重要。

问题在于：

**不应该让 Sandbox 同时承担 Agent 的身份、状态、运行时和生命周期。**

如果 Agent 要从一个一次性工具，变成一个可以跨设备、跨环境、持续存在的“数字工作者”，那么系统就必须把这些东西拆开：

计算。

状态。

身份。

凭证。

授权。

历史。

资源。

工具。

外部副作用。

模型只是其中一个组件。

Sandbox 也只是其中一种资源。

真正需要扩展的，是这整个 Agent System。

这就是作者所说的：

**The Next Scaling Problem。**

过去的问题是：

一个模型能不能完成任务。

现在的问题开始变成：

**当模型越来越能干时，我们有没有一套足够可靠的系统，让它长期、并行、安全地工作。**

Tetral 目前已经以 MIT License 开源。

作者希望产品开发者、基础设施工程师、安全研究者和模型提供商一起挑战它的边界，构建新的 Runtime、接入新的资源，并探索当前系统还没有预料到的产品形态。

这套架构是否最终正确，还需要真实负载验证。

但至少，它把问题重新定义清楚了：

**真正的下一代 Agent Cloud，不应该是一堆装着 Agent 的 Sandbox。**

**而应该是一套以 Agent 为中心、让所有计算资源围绕它动态组织的云系统。**
