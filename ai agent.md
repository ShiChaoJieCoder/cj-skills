# 吃透 AI Agent 开发 — 三篇核心笔记（面试用）

来源（本地 PDF，对应 Sitor 课程）：

- 《搞定 Agent 六大支柱：今天出个 Manus 明天出个 OpenClaw，你到底应该学什么？》→ [agent-landscape](https://sitor.ai/courses/agent-fundamentals/1-agent-landscape)
- 《从 ChatBot 到 Agent：一个 while 循环，凭什么让 AI 从「能聊天」变成「能干活」？》→ [chatbot-to-agent](https://sitor.ai/courses/agent-fundamentals/2-chatbot-to-agent)
- 《2026 年了，你的 Agent 架构还停留在 LangChain 时代吗？》→ [framework-reality](https://sitor.ai/courses/agent-fundamentals/4-framework-reality)

**说明：** 下节「六大支柱」为 PDF 文本层原样摘录（含页眉页脚与日期链接）；第 3、8 页中的框架图已从 PDF 内嵌图导出为 PNG，插在对应页正文之后。

---

## 一、《搞定 Agent 六大支柱》PDF 全文摘录 + 配图

**PDF 文件名：** `搞定 Agent 六大支柱：今天出个 Manus 明天出个 OpenClaw，你到底应该学什么？ — 吃透 AI Agent 开发.pdf`

### 正文（按页）

#### 第 1 页

```
搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到
底应该学什么？
约  20 分钟
AI 私教
专属  1v1 AI 私教，围绕本节内容深度教学
进⼊私教
最近这⼤半年，搞技术的朋友应该都有⼀个感受： Agent 这个词，铺天盖地。
Claude Code 出了  /loop 定时任务， OpenClaw 的  GitHub star 飙到了  25 万但紧接着爆出了⾼危
RCE 漏洞， Cursor 搞出了  Automations 和⾃托管  Agent ， OpenAI Codex 出了桌⾯客户端 …… 社
交媒体上，每个产品都说⾃⼰是 " 真正的  AI Agent" ，每篇⽂章都在教你 " 如何构建  Agent" ，信息量
⼤到让⼈窒息。
有不少读者朋友跟我说过类似的话：
" 我知道  Agent 很重要，但我不知道从哪学起。感觉每天都在出新东⻄，学不完。 "
说实话，我特别理解这种状态。因为我也曾经经历过。
不过这些年下来，⾃⼰也做了⼀些  Agent 产品，也⽐较深⼊地研究过  Claude Code 、
OpenClaw 、 Manus 这些项⽬的原理和设计，踩了不少坑之后，我慢慢有了⼀个⽐较清晰的认知：
这些产品表⾯上形态各异，底层全在解决同⼀组问题。
⽽且这组问题的数量，远⽐你想象的少。
⼀次  Agent 调⽤背后发⽣了什么
我⽤⼀个你可能每天都在做的场景来说明。
搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到底应该学什么？
2026/4/19 16:44 搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到底应该学什么？  — 吃透  AI Agent 开发
https://sitor.ai/courses/agent-fundamentals/1-agent-landscape 1/10
```

#### 第 2 页

```
假设你在终端⾥跟  Claude Code 说：
帮我把  src/utils.ts ⾥的  formatDate 函数重构⼀下，⽤  dayjs 替换  moment
然后你按下回⻋。从这⼀刻开始，到  Claude Code 跟你说 " 搞定了 " ，中间发⽣了什么？
第⼀件事：它得决定先做什么。
Claude Code 不会上来就改代码。它会先 " 想 " ⼀下：我需要先读⼀下  utils.ts 看看  formatDate ⻓
什么样，然后看看  package.json ⾥有没有  dayjs ，没有的话要装⼀下，然后再改代码，改完跑⼀
下测试 ……
这个 " 想⼀步、做⼀步、看⼀步 " 的循环，就是  Agent Loop。它是  Agent 的⼼跳 —— 没有这个循
环， AI 就只是⼀个问答机器。
第⼆件事：它得能读⽂件、跑命令。
光 " 想 " 没⽤，得有⼿。 Claude Code 需要调⽤  Read ⼯具读⽂件、调⽤  Bash ⼯具跑  pnpm add
dayjs 、调⽤  Edit ⼯具改代码。
这些⼯具的定义、调度、权限管理、并发控制，组成了  Tool System。它是  Agent 的⼿脚 —— 没
有⼯具， Agent 就是个只会说不会做的嘴炮。
第三件事：它得记住前⾯做了什么。
假设这个重构涉及  15 个⽂件， Claude Code 改到第  10 个⽂件的时候，它需要记住前⾯  9 个⽂件
改了什么、⽤了什么模式、有没有遇到什么坑。但上下⽂窗⼝是有限的 —— 前⾯的信息太多了，塞
不下了怎么办？
这就是  Context Engineering，上下⽂⼯程。它是  Agent 的⼤脑供⾎系统 —— 决定  Agent 能 " 看
到 " 多少、 " 记住 " 多少。这块我认为是整个  Agent 开发⾥最被低估、也最有含⾦量的部分。
第四件事：它得记住你是谁。
你上次跟  Claude Code 说过 " 这个项⽬⽤  pnpm 不要⽤  npm" ，它会记住。下次你开⼀个新的会
话，它还能记得。
这是  Memory 系统—— 跨会话的⻓期记忆。和  Context Engineering 不同， Context 管的是单次
会话内的信息， Memory 管的是跨会话的持久化。
2026/4/19 16:44 搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到底应该学什么？  — 吃透  AI Agent 开发
https://sitor.ai/courses/agent-fundamentals/1-agent-landscape 2/10
```

#### 第 3 页

```
第五件事：复杂任务，⼀个  Agent 搞不定。
如果你让  Claude Code 探索⼀个⼏万⾏的代码库，它可能会  fork 出⼀个⼦  Agent ，让⼦  Agent
在⼲净的上下⽂⾥去探索，探索完把结果压缩成⼀两千  token 的摘要传回来。这样主  Agent 的上下
⽂不会被撑爆。
这就是  Multi-Agent—— 不是为了 " 分⻆⾊ " 搞什么  PM + 设计师  + 开发者的  cosplay ，⽽是为了分
隔上下⽂。
第六件事：权限、 Hook 、重试、优雅退出 ……
Agent 要跑  rm -rf  的时候，谁来拦？ API 挂了怎么重试？⽤户按了  Ctrl+C 怎么优雅退出？模型
陷⼊死循环怎么检测？
这些 " 包裹在模型外⾯ " 的⼯程系统，就是  Harness Engineering。它是  Agent 的⻣架 —— 模型再
强，没有⼀个好的⻣架，也跑不起来。
六⼤⽀柱
你看，就这⼀次看似简单的⼯具调⽤，背后牵扯了六个⼤的系统：
2026/4/19 16:44 搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到底应该学什么？  — 吃透  AI Agent 开发
https://sitor.ai/courses/agent-fundamentals/1-agent-landscape 3/10
```

![PDF 第 3 页嵌入图：六大支柱总览框架](agent-six-pillars-assets/page3_img1.png)

#### 第 4 页

```
⽀柱 ⼀句话 ⼈体类⽐
Agent Loop 想⼀步、做⼀步、看⼀步的循环 ⼼跳
Tool System 读⽂件、跑命令、调  API ⼿脚
Context Engineering 管理有限的上下⽂窗⼝ ⼤脑供⾎
Memory 跨会话的⻓期记忆 ⻓期记忆
Multi-Agent 拆任务、分上下⽂ 团队协作
Harness Engineering 权限、重试、 Hook 、⽣命周期 ⻣架
记住这六个词。 后⾯不管出什么新的  Agent 产品、新的框架、新的论⽂，你都可以⽤这六个维度
去拆解它。它在  Agent Loop 上做了什么创新？ Context Engineering 怎么处理的？⼯具系统是什
么设计？
这就是为什么我说 " 学  Agent 不要追产品、热点，要学底层问题 " 。产品会过时，但问题不会。
六⼤⽀柱，每个都有多深？
你可能觉得，这六个词听起来挺简单的嘛，有什么好学的？
那我给你展开聊聊，你感受⼀下每个⽀柱背后的⽔有多深。
Agent Loop ：不只是个  while 循环
最简单的  Agent Loop 就是⼀个  while(true) { think → act → observe }  循环。 10 ⾏代码就
能写出来。
但实际跑起来你会发现⼀堆问题：
模型说到⼀半被截断了怎么办？ Claude Code 的做法是  3 次递进恢复 —— 第⼀次把  max_tokens
设成  8192 希望它能说完，不⾏就升到  64K ，再不⾏就注⼊⼀条 " 你的输出被截断了，请继续 " 的消
息。超过  3 次就放弃。
Agent 陷⼊死循环怎么办？ OpenClaw 设计了  4 种检测器：同⼀个⼯具反复调⽤（ Generic
Repeat ）、⽆进展的轮询（ Known Poll ）、两个⼯具交替调⽤（ Ping-Pong ）、全局熔断（ 30 次重
复强制停⽌）。不是⼀种检测就够了，实际场景中循环的模式五花⼋⻔。
2026/4/19 16:44 搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到底应该学什么？  — 吃透  AI Agent 开发
https://sitor.ai/courses/agent-fundamentals/1-agent-landscape 4/10
```

#### 第 5 页

```
API 挂了怎么办？ Claude Code 有三层重试：内层  API 级重试（最多  10 次指数退避）、中层流式
失败降级为⾮流式、外层快速模式失败降级为标准模式。 OpenClaw 更夸张 —— 重试预算最多  160
次，还有各种模型  Provider 相互兜底，具体我们先不展开了，后⾯的课程会展开。
就⼀个循环 ——Agent Loop ，展开来讲可以讲很多篇。
Tool System ：⼯具越多越不准
你可能觉得⼯具系统就是定义⼏个函数让模型调嘛，有什么难的。
实际上，⼯具数量⼀上去，问题就来了。 10 个⼯具以内，模型选择准确率  > 95% ； 30 个⼯具，降
到  ~85% ； 50 个以上，不做特殊处理就是灾难。
怎么解决？ Claude Code ⽤了⼀个叫  Deferred Tool Loading 的设计 —— 初始  prompt ⾥只放⼯
具名和⼀句话描述，不放完整的  JSON Schema 。模型需要⽤某个⼯具的时候，先通过
ToolSearch 按需加载完整定义。效果：初始  prompt 瘦身  30-40% ，缓存命中率⼤幅提升。
Manus ⾛了另⼀条路 —— ⼲脆只保留不到  20 个原⼦⼯具，复杂操作全部通过写脚本放  sandbox
⾥跑。⼯具定义只占  3K token ，⽽不是  30K 。
还有⼀个容易忽略的问题：⼯具定义⼀旦改了，后⾯所有的  KV Cache 全部失效。 这意味着你不
能动态增删⼯具 —— 看起来省了  token ，实际上缓存失效的成本可能是  10 倍。 Manus 的做法是
"Mask Don't Remove" ：不删⼯具定义，只标记哪些不可⽤，并通过⽐较巧妙的⽅式来限制模型的
Action Space ，后续我们也会好好拆解。
Context Engineering ：最被低估的护城河
这⼀块我认为是整个  Agent 开发⾥技术含量最⾼的部分。
⼀个数据帮你感受⼀下：⼀个典型的  Agent 任务， 50 次⼯具调⽤，每次平均  2000 token 的结
果，光⼯具输出就是  10 万  token 。再加上  system prompt 、⼯具定义、对话历史 ——200K 的窗
⼝说满就满。
⽽且上下⽂不是越⻓越好。学术界有⼀个叫  "Lost in the Middle" 的现象：模型对上下⽂头部和尾
部的信息注意⼒最强，中间的内容会被逐渐忽略。你往上下⽂⾥塞的东⻄越多，模型对中间信息
的 " 记忆⼒ " 就越差。
所以上下⽂管理的核⼼不是 " 怎么塞更多 " ，⽽是怎么让模型在有限的注意⼒预算内看到最关键的信
息。
2026/4/19 16:44 搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到底应该学什么？  — 吃透  AI Agent 开发
https://sitor.ai/courses/agent-fundamentals/1-agent-landscape 5/10
```

#### 第 6 页

```
我们在课程⾥会⽤五个维度来拆解  Context Engineering ：
维度 做什么
Offload （卸载） 把信息搬到上下⽂之外 —— ⽂件系统、 sandbox 、脚本
Reduce （压缩） 就地缩⼩（ Compaction ）或⽤摘要替换（ Summarization ）
Retrieve （检索） 从外部按需取回 ——RAG 、⽂件读取、 ToolSearch
Isolate （隔离） 拆成多个独⽴上下⽂ ——Multi-Agent
Cache （缓存） 复⽤已有计算结果 ——KV Cache 、 Prompt Cache
有意思的是，⾏业对这五个维度并没有共识。 Manus 重度使⽤  Offload 和  Cache ，但不太⿎励压
缩 —— 因为信息丢失⻛险太⼤。没有银弹，每个维度都有  trade-off 。
还有⼀个点：压缩不等于删除。 好的上下⽂管理是可恢复的 —— 信息不是真丢了，只是暂时移出了
模型的视野。
Memory ：简单⽅案反⽽更好
跨会话记忆听起来应该很复杂 —— 向量数据库、语义检索、 embedding 模型 ……
但  Claude Code ⽤的⽅案简单到让⼈意外：就是⼀个  MEMORY.md ⽂件。
模型⾃⼰⽤  Write ⼯具把值得记住的信息写进去，下次会话开始的时候读回来。没有向量数据库，
没有  embedding ，就是纯⽂本⽂件。
反过来， OpenClaw ⾛的是⽐较重的路线 ——SQLite + 向量混合检索， BM25 关键词权重  30% +
向量权重  70% ，还有  MMR 去重和时间衰减。
哪个更好？取决于你的场景。 Claude Code ⾯对的是单⽤户、记忆量不⼤的场景，⽂件⽅案简单可
调试，⼈类可直接编辑。 OpenClaw ⾯对的是多⽤户、⼤量记忆的场景，需要语义搜索能⼒。
没有最好的⽅案，只有最合适的⽅案。 这个判断⼒，⽐学会某个具体技术更重要。
Multi-Agent ：不是分⻆⾊，是分上下⽂
市⾯上很多  Multi-Agent 的教程都在教你搞 " ⻆⾊扮演 "—— ⼀个  Agent 当  PM ，⼀个当开发，⼀个
当测试。
2026/4/19 16:44 搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到底应该学什么？  — 吃透  AI Agent 开发
https://sitor.ai/courses/agent-fundamentals/1-agent-landscape 6/10
```

#### 第 7 页

```
但真的，业界⽤的  Multi-Agent ，核⼼价值根本不在这⾥。
Manus 的联合创始⼈  Peak 说了⼀句很精辟的话： " ⼈类按⻆⾊组织是因为认知限制。 LLM 不⼀定
有这些限制。 "
⼦  Agent 的主要⽬标不是按⻆⾊分⼯，⽽是隔离上下⽂。 让每个⼦  Agent 在⼲净的窗⼝⾥⼯作，
不被其他任务的信息污染。
Claude Code 的⼦  Agent 设计就是这个思路 ——Explore Agent 去探索代码库，在⾃⼰的上下⽂
⾥可以读⼏万  token 的⽂件，最后只给⽗  Agent 返回⼀两千  token 的摘要。⽗  Agent 的上下⽂保
持⼲净。
还有⼀个更硬核的场景： Claude Code 的  Worktree 隔离。⼦  Agent 在⼀个独⽴的  git worktree
⾥⼯作，⽂件系统都是隔离的。做完了再  merge 回来。这才是⽣产级的  Multi-Agent 。
Harness Engineering ：模型外⾯那层壳
最后这个，可能是最不性感但最重要的。
Harness 翻译过程可以叫 “ 壳 ” ，也可以叫环境，它的设计可能⽐模型选择的影响更⼤。
举个具体的例⼦。 OpenClaw 今年爆出了  CVSS 8.8 的⾼危  RCE 漏洞， 3 万多个实例受影响，
800 多个恶意  Skill 混进了  ClawHub 市场。你猜猜是啥  原因？默认没开认证。
这不是模型的问题，是  Harness 的问题。权限系统设计得不够严格，⼀个漏洞就能让整个  Agent
变成攻击⼯具。
Claude Code 在这⽅⾯做得很细： 6 种权限模式、 Bash 命令专⽤⻛险分类器、危险操作模式检测
（rm -rf / 、 fork bomb 、 sudo ）、连续被拒会⾃动降级为⼿动确认。还有  14 种  Hook 类型让⽤
户能在不改源码的情况下定制⾏为。
这些东⻄虽然看起来不够酷炫，但没有它们， Agent 就是⼀个随时可能失控的定时炸弹。
学的顺序很重要
这六个⽀柱不是平⾏的，它们之间有依赖关系。我建议的学习路径是从内到外：
2026/4/19 16:44 搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到底应该学什么？  — 吃透  AI Agent 开发
https://sitor.ai/courses/agent-fundamentals/1-agent-landscape 7/10
```

#### 第 8 页

```
1. Agent Loop （⼼跳）：先理解  Agent 的最⼩可运⾏单元
2. Tool System （⼿脚）： Agent 能 " 做事 " 了，你才能观察后⾯的问题
3. Context Engineering （供⾎）： Agent 跑起来之后，上下⽂爆了怎么办？这是深⽔区的⼊⼝。
4. Memory （记忆）：单次会话、跨会话，上下⽂记忆怎么来管理？
5. Multi-Agent （协作）：⼀个  Agent 不够⽤，怎么拆成多个？
6. Harness Engineering （⻣架）：上⾯都搞定了，怎么包成⼀个⽣产级系统？
每⼀层都建⽴在前⼀层的基础上。你不理解  Agent Loop ，就没法理解为什么  Context
Engineering 这么重要；你不理解  Context Engineering ，就不理解为什么  Multi-Agent 的核⼼价
值是 " 分上下⽂ " ⽽不是 " 分⻆⾊ " 。
这⻔课怎么组织的
说⽩了，这⻔课就是沿着上⾯这条路径，⼀层⼀层往上搭。
每⼀篇我都会从⼀个⽣产环境⾥真实会遇到的问题出发 —— 不是编⼀个教科书式的例⼦，⽽是你做
Agent 真的会踩到的坑。然后去看  Claude Code 、 OpenClaw 、 Manus 这些百万级⽤户的  AI 产品
是怎么解决这个问题的。最后提炼出可复⽤的设计原则。
2026/4/19 16:44 搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到底应该学什么？  — 吃透  AI Agent 开发
https://sitor.ai/courses/agent-fundamentals/1-agent-landscape 8/10
```

![PDF 第 8 页嵌入图：六大支柱学习顺序 / 依赖关系](agent-six-pillars-assets/page8_img1.png)

#### 第 9 页

```
⼏个你可能关⼼的问题：
需要什么基础？
有前端或全栈开发经验就⾏。 TypeScript 能看懂，命令⾏能⽤， API 调过就够了。不需要机器学习
背景、不需要  AI 背景。下⼀篇我会专⻔讲做  Agent 需要了解的⼤模型底层知识，不多，但必须知
道。
学完能做什么？
不管市⾯上出什么新的  Agent 框架、新的产品，你都能快速看懂它的架构，知道它在六⼤⽀柱的哪
些维度上做了什么取舍。你也能⾃⼰从零搭⼀个  Agent 系统 —— 不是调  LangChain 的  API ，是真
正理解每⼀个⼯程决策背后的  why 。
根据我的调研，学习这⻔课程⼤部分都是命中了下⾯这些诉求：
如果你有类似的诉求，那这⻔课程还是适合你来学习的。
跟别的  Agent 课有什么不同？
⼤部分  Agent 课教你⽤某个框架 ——" ⽤  LangChain 和  Langraph 搭⼀个  Agent" 、 " ⽤  Pi 写⼀个
Agent" 。框架⼀更新，课就过时了。
这⻔课不教你具体怎么⽤框架，⽽是教你理解框架背后在解决什么问题。我会带你从  Claude
Code 、 OpenClaw 、 Manus 等等底层设计原理的⻆度跟⼤家拆解，让你们彻底搞明⽩到底怎么才
能驾驭⼀个真实的  agent 系统。
温馨提示：安装  App 并开启通知
课程平台⽀持  PWA （渐进式  Web 应⽤），安装后拥有原⽣  App 体验，包括桌⾯图标、离线访问、
每⽇复习提醒和课程更新推送。
强烈建议安装并开启通知，这样你能收到复习提醒和课程更新通知，帮助巩固所学知识。
原来的技术栈快过时了，想要在  AI 时代提升⾃⼰，增强⾃⼰技术上的能⼒，提升竞争⼒·
公司业务有需要， agent 开发是⼀个刚需·
想要找⼯作⾯试，需要系统的资料来学习·
2026/4/19 16:44 搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到底应该学什么？  — 吃透  AI Agent 开发
https://sitor.ai/courses/agent-fundamentals/1-agent-landscape 9/10
```

#### 第 10 页

```
iOS
1. ⽤  Safari 打开  https://sitor.ai （仅  Safari ⽀持， Chrome/Firefox 不⾏）
2. 点击底部⼯具栏的  分享按钮（⽅框  + 向上箭头图标）
3. 滑动菜单找到  「添加到主屏幕」，点击确认
4. 从主屏幕打开  Sitor ，进⼊后点击右上⻆  设置（⻮轮图标） →  「开启通知」
5. 在弹出的系统权限对话框中点击「允许」
桌⾯（ Chrome / Edge ）
1. 打开  https://sitor.ai
2. 地址栏右侧会出现安装图标（或⻚⾯顶部弹出安装横幅），点击安装
3. 安装后点击设置  →  「开启通知」  →  允许权限
注意：⽬前推送通知⽀持  iOS （需添加到主屏幕）和桌⾯浏览器（ Chrome / Edge ），安卓暂
不⽀持。
后⾯的路
然后我们就正式进⼊  Agent 的世界。从⼀个  while(true) 循环开始，⼀步⼀步搭起来。
已读完·课程进度  2/33
上⼀篇
我为什么要做这⻔课，以及我是谁
下⼀篇 · 第⼀章：认知校准
从  ChatBot 到  Agent ：⼀个  while 循环，凭什么让  AI 从 " 能聊天 " 变成 " 能⼲活 " ？
2026/4/19 16:44 搞定  Agent 六⼤⽀柱：今天出个  Manus 明天出个  OpenClaw ，你到底应该学什么？  — 吃透  AI Agent 开发
https://sitor.ai/courses/agent-fundamentals/1-agent-landscape 10/10
```

---

## 二、从 ChatBot 到 Agent：谁掌控循环

**三种形态：**

- **ChatBot**：人发一句、模型答一句，**人掌控循环**。
- **Copilot**：模型给建议，**人按 Tab 才接受**，仍是人掌控循环。
- **Agent**：模型多轮自主执行，**AI 掌控循环**，人只在关键处审批或否决。

**本质区别：** 差距往往不在模型能力，而在**谁控制循环**。Anthropic 的区分：**Workflow** = LLM 走在预定义代码路径里；**Agent** = LLM **自己决定**流程与工具使用。

**最小模型：** `while(true) { think → act → observe }`，即 **ReAct（Reasoning + Acting + Observation）**。停下来的常见判断：模型不再发起 tool call。  
**Observe 易被忽略：** 必须把工具结果写回 `messages`，否则后续决策是盲的。

**生产级 Loop 还会包含：** 上下文压缩（Snip / 摘要等多档）、流式响应、流式工具执行（边说边执行、只读可并发/写需串行等）、多种退出原因、错误反馈质量、状态追踪、用 Generator 做「边跑边展示」等。

**为何近年 Agent 爆发（文中「三层能力」+ 基础设施）：** 知识（约 2022）→ 推理/规划（约 2024 末）→ **长期多轮迭代**（约 2025）；配合更大上下文、原生 Function Calling、MCP、Skills 等。

---

## 三、2026 年了，还要不要「LangChain 时代」的架构？

**LangChain 做对了什么（历史价值）：** 降低入门、用 Chain 建立早期心智模型、连接器生态丰富，适合 **RAG 等步骤相对固定的流水线**。

**为何与 Agent 天然拧巴：**

- **Chain = 线性、预定义**；**Agent = 运行时才知道下一步**，核心是循环，不是一条链。
- **抽象层多** → 定制与调试成本高（「抽象税」）。
- **原型到生产**：精细控制、可观测、错误恢复与业务策略常与「大而全框架」目标冲突；文中引用调研：大量团队最终拆除 LangChain。

**LangGraph：** 用**图/状态机**替代链，支持循环与分支，方向对；但简单编排往往等价于少量 `if` + `while`，复杂时在**节点内部**（流式、并发安全、上下文压缩）仍要自研；且与 LangChain 生态绑定深。

**头部产品常见做法：**

- **Claude Code**：核心 Agent Loop **手写**（如 TypeScript async generator），仅依赖官方 SDK；工具、流式执行、上下文压缩等多为自研。
- **OpenClaw**：可能用 **pi-agent-core** 等薄层（消息格式、工具抽象、会话骨架），**上下文引擎、工具策略、Memory** 等仍自研。

**Vercel AI SDK 的定位：** **API 适配层**（统一多厂商接口、流式等），**不是** Agent 框架；不替你写循环、不替你管上下文与工具编排。文中引用：**做 Agent 就是普通编程 — if、循环、switch**；需要的是好用的 API 适配层 + **自己掌控 Agent 核心逻辑**。

**课程侧技术选型表述：** **Vercel AI SDK 管 API 层 + 自研 Agent 核心**；先懂原理再学框架，才能判断框架在替你做哪一层、牺牲了什么。

**篇末小结（原文观点浓缩）：**

- LangChain 像 AI 领域的 jQuery：早期降低门槛，但模型与开发者成熟后，**核心价值在下降**。
- 生产级 Agent 的循环控制、上下文、工具编排**高度场景化**，通用大框架易成障碍。
- **API 适配层 ≠ Agent 框架**；学 **think / act / observe** 比学某个框架 API 更持久。

---

## 四、面试时可用的「一段话」（自行改口癖）

我最近系统在看 Agent 的**底层共性**，而不是追单个产品：核心是 **Agent Loop（ReAct）**——模型在循环里**自主**决定下一步；真正拉开差距的是 **Tool System、Context Engineering、Memory、必要时 Multi-Agent、以及外层的 Harness（权限、重试、生命周期）**。我也在关注 **框架与自研的边界**：大框架适合标准化 RAG/简单工具链，复杂生产往往要在**循环内部**做精细控制，所以很多产品会用 **API 适配层 + 自研 Loop**。接下来我希望**自己动手实现一版 Agent**：先把 `while` 循环与工具闭环跑通，再逐级加上下文管理与工程化外壳，把原理落到代码里。

---

*第二节、第三节为对另两篇 PDF 的归纳；第一节为《六大支柱》全文摘录与配图，细节以原 PDF 为准。*
