# 前台 AI 问答模块技术业务总结（knowledge）

## 1. 模块定位与业务价值

`src/common/modules/knowledge` 是前台“AI 讲解员”能力的核心模块，支持用户在展厅场景里进行自然语言问答，并结合参考资料给出可追溯回答。

业务价值可概括为 4 点：

1. **提升转化与停留时长**：用户可随时提问，降低信息获取门槛。
2. **降低人工讲解成本**：对高频问题自动应答，减少重复人工咨询。
3. **增强可信度**：回答附带参考资料，降低“AI 胡说”感知。
4. **沉淀热点问题资产**：通过热门问答浏览/点赞，反哺内容运营。

---

## 2. 功能拆解（用户视角）

### 2.1 实时问答链路

- 打开问答弹窗后，用户输入问题并发送。
- 前端先创建/复用会话 `chatSessionId`，再提交问题拿到 `chatMessageId`。
- 前端基于 `chatMessageId` 轮询获取 AI 回答与参考资料。
- UI 中先显示“思考中”，成功后替换为正式答案，失败时展示错误状态。

### 2.2 连续追问与新对话

- 单次会话最多追问 5 次（`ASK_TIMES = 5`）。
- 超过次数后提示“请发起新对话”。
- 点击“新对话”会重置消息、追问次数，并清空会话缓存。

### 2.3 热门问答能力

- 支持分页拉取热门问答列表。
- 支持详情浏览（PV）与点赞（Like）。
- 点赞状态按天做本地缓存，避免重复点赞体验问题。

---

## 3. 技术架构与关键实现

### 3.1 API 分层（Knowledge 与 HotChat 分离）

- `api.ts`：问答主链路
  - `createChatSession()`
  - `submitQuestion()`
  - `getQuestionAnswer()`（轮询接口）
- `hotChat/api.ts`：热门问答能力
  - `getHotChatList()`
  - `likeHotChat()`
  - `hotChatPv()`

这种拆分让“在线问答”和“内容运营”两条链路相对独立，便于后续扩展与灰度。

### 3.2 对话状态模型（前端状态机思路）

核心消息结构（`MessageBlockProps`）：

- `type: 'question' | 'answer'`
- `spinning`：回答生成中
- `content`：最终内容
- `errMsg`：错误信息
- `references`：参考资料列表

每次提问会插入两条消息：

1. 用户问题（`question`）
2. AI 占位回答（`answer + spinning`）

轮询返回后再“原位更新”该回答条目，实现流畅的“先占位后落地”体验。

### 3.3 会话复用与并发控制

`KnowledgeAskDialog` 中通过 `createChatSessionPromise` 缓存 Promise，而不是只缓存结果：

- 避免短时间内多次触发时重复创建会话。
- 同一对话窗口生命周期内，提问复用同一个 `chatSessionId`。
- 新对话时显式置空缓存，确保会话隔离。

这是一个很实用的“前端轻量并发去重”模式。

### 3.4 轮询容错设计

- 固定轮询间隔：2 秒（`POLLING_INTERVAL = 2000`）。
- 最大重试次数：60 次（约 2 分钟超时上限）。
- 弹窗不可见时停止继续轮询（通过 `useLatest(visible)` 判定）。
- 超时与异常均有兜底文案，避免无反馈状态。

### 3.5 输入框交互工程化

`InputBox` 的几个实战点：

- 基于 `react-textarea-autosize` 做多行自增高输入。
- `Enter` 发送、`Shift + Enter` 换行，符合 IM 习惯。
- 字数上限校验（200 字）+ 实时计数 + 超限提示。
- 支持 `initQuestion` 自动发问（用于外部一键提问场景）。
- 忙碌态统一收口：提交中/回答中/次数耗尽时禁用输入。

### 3.6 内容安全与展示

回答渲染采用 `marked + DOMPurify`：

- 支持 Markdown 富文本展示。
- `DOMPurify.sanitize()` 防止 XSS 注入。
- 同时保留参考资料折叠展开能力，兼顾信息密度与可读性。

### 3.7 组件抽象复用

`KnowledgeDialogBase` 负责通用弹窗骨架：

- 统一标题、关闭行为、消息滚动区域。
- 通过 `askInfo/hotChatInfo` 插槽组合不同能力。
- AI 问答弹窗和热门问答详情弹窗复用同一容器，减少重复 UI 代码。

---

## 4. 可对面试官重点强调的“亮点回答”

### 亮点 1：不是“纯接口对接”，而是完整问答状态闭环

可答法：

> 我们把 AI 问答做成了完整状态机体验：用户提问后先展示占位回答，再通过轮询回填结果；失败和超时有统一兜底；回答附带参考资料，最终形成“可用、可读、可信”的闭环，而不只是简单把接口数据打印出来。

### 亮点 2：前端层面的并发与会话治理

可答法：

> 会话创建用 Promise 级缓存，解决同窗口内重复创建 session 的并发问题；新对话再显式重置，保证上下文隔离。这类治理能显著减少无效请求和串会话风险。

### 亮点 3：安全与体验并重

可答法：

> AI 返回内容支持 Markdown，但渲染前会做 DOMPurify 清洗，避免 XSS。交互上提供思考中态、字符校验、追问次数控制和自动滚动，整体更接近真实生产可用标准。

### 亮点 4：业务数据回流能力

可答法：

> 我们不只做问答，还做了热门问答的浏览和点赞闭环，并把点赞行为做了按天本地去重，这样既能做运营看板，也能反向指导知识库优化。

---

## 5. 当前实现的边界与可优化点（面试加分项）

1. **轮询可升级为流式输出**：SSE/WebSocket 能进一步降低等待感。
2. **请求取消机制可加强**：弹窗关闭时可结合 AbortController 主动中断在途请求。
3. **消息 key 可改为稳定 id**：目前 `MessageBlock` 使用 `index` 作为 key，复杂更新场景建议改为稳定主键。
4. **错误分类可更细**：区分网络错误、鉴权错误、模型超时，便于精细化提示与监控。
5. **可观测性补齐**：建议增加提问成功率、首 token 延迟、完整响应时长、超时率等埋点。

---

## 6. 面试高频 Q&A（可直接复述）

### Q1：你们前端在 AI 项目里做了什么，不只是调接口吗？

A：前端承担了完整的会话与状态治理，包括会话创建复用、轮询策略、占位态到结果态切换、错误兜底、内容安全渲染、追问次数限制和热门问题运营闭环，不是“接口透传”。

### Q2：为什么用轮询，不直接上流式？

A：当时后端给的是异步任务+查询结果模型，轮询接入成本更低、交付更快；同时我们设置了轮询上限和超时兜底。后续如果追求更强实时性，可以平滑升级到 SSE。

### Q3：如何保证 AI 回答展示安全？

A：Markdown 渲染后会经过 `DOMPurify.sanitize`，避免脚本注入；同时参考资料与免责声明帮助用户正确理解 AI 输出边界。

### Q4：如何避免重复点赞或刷赞体验问题？

A：点赞后在本地存储按天记录已点赞 `hotChatId`，当天内标记已赞，减少重复操作和误触，兼顾体验与后端压力。

---

## 7. 涉及的关键文件（便于快速回看）

- `src/common/modules/knowledge/api.ts`
- `src/common/modules/knowledge/dialog/KnowledgeAskDialog.tsx`
- `src/common/modules/knowledge/dialog/KnowledgeDialogBase.tsx`
- `src/common/modules/knowledge/dialog/InputBox.tsx`
- `src/common/modules/knowledge/dialog/MessageBlock.tsx`
- `src/common/modules/knowledge/utils/parseMd.ts`
- `src/common/modules/knowledge/hotChat/api.ts`
- `src/common/modules/knowledge/hotChat/useHotChatList.ts`
- `src/common/modules/knowledge/hotChat/KnowledgeHotChatDialog.tsx`

