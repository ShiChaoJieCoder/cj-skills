# 自定义 Agent Skill（写给「从没写过 Skill」的你）

> **集成到项目**：将本目录 **`frontend-vibe-slice`** 整体复制到你的前端项目下的 **`.cursor/skills/frontend-vibe-slice/`**（`SKILL.md` 必须在 `name` 与文件夹同名的目录里）。在 Cursor **Agent** 输入 **`/`**，搜索 **`frontend-vibe-slice`** 即可调用。若列表里没有，执行 **Developer: Reload Window** 或重启 Cursor。

## 除了 Superpowers，还有什么适合集成自己的 Skill？

| 方式 | 是什么 | 适合你吗 |
| :--- | :--- | :--- |
| **Cursor 原生 Agent Skills** | 在项目里建文件夹 + `SKILL.md`（YAML 头 + 正文），Cursor 启动后会**自动发现**；对话里输入 **`/`** 可手动选 | **最适合新手**：不依赖插件市场，内容完全归你，可提交 Git 给团队用 |
| **Cursor Rules（`.cursor/rules`）** | 短规则、常设约束（Always / Glob） | 适合「永远生效的一句话」；**复杂多步流程**更适合 Skill |
| **Superpowers 插件** | 别人写好的流程库 | 开箱即用；**你自己的业务约定**仍应用上面这种自定义 `SKILL.md` |

官方约定：技能目录可以是 **`.cursor/skills/`**、**`.agents/skills/`**，或用户级 `~/.cursor/skills/` 等；每个技能是一个**文件夹**，里面必须有 **`SKILL.md`**。标准见 [Agent Skills](https://agentskills.io/) 与 Cursor 文档。

---

## 面试可以怎么说（30 秒）

> 除了用 Superpowers 那种现成流程，我在仓库里加了一个 **自定义 Cursor Skill**：把「前端按路由切片、Zustand 边界、不动构建配置」写进 `SKILL.md`，**版本可控、团队可复用**。需要时我在 Agent 里用 **`/`** 调这个技能，或减少 AI 乱改目录、乱加状态管理方案。

---

## 怎么用（3 步）

1. 在仓库根目录建目录：`.cursor/skills/frontend-vibe-slice/`（文件夹名要和下面 `name` **一致**）。
2. 把下面「可直接保存的 `SKILL.md`」整段保存为：`.cursor/skills/frontend-vibe-slice/SKILL.md`。
3. 重启 Cursor 或等索引刷新；在 **Agent** 输入 **`/`**，搜索 **`frontend-vibe-slice`** 或描述里的关键词。

可选：在 `SKILL.md` 的 frontmatter 里加 `disable-model-invocation: true`，则**只有**手动 `/` 才会注入，不会自动套用。

---

## 日常使用：怎么真正「用起来」

| 步骤 | 做什么 |
| :--- | :--- |
| 1 | 用 Cursor 打开**已放好** `.cursor/skills/frontend-vibe-slice/SKILL.md` 的那个仓库（根目录要对）。 |
| 2 | 打开 **Agent**（或 Composer Agent，以你当前 Cursor 界面为准）。 |
| 3 | 在输入框里输入 **`/`**，在列表里搜 **`frontend-vibe-slice`** 或 **slice / vibe / frontend**，选中该 Skill（等于把这份约定注入本轮上下文）。 |
| 4 | **同一轮消息里**继续打字：你要做的具体事（见下方「使用案例」）。不要只选 Skill 不说话。 |
| 5 | 发送后，Agent 会按 `SKILL.md` 里的 Instructions 约束来改代码；若你发现它仍乱改 `vite.config`，再补一句：「按 skill 禁止动构建配置」。 |

**自动触发（可选）**：不设 `disable-model-invocation: true` 时，Cursor 可能根据你的 **description** 判断「这和加页面、接接口有关」就自动带上该 Skill；**想稳一点**就每次手动 `/` 选一次。

**和 Rules 搭配**：`.cursor/rules` 里写「永远遵守的短句」，Skill 里写「这一类的完整切片流程」；面试可以说：**Rules 是底线，Skill 是场景化剧本**。

---

## 使用案例（可直接复制改一改就用）

下面假设你已经用 **`/`** 选中了 **`frontend-vibe-slice`**，在同一轮里粘贴或改写后面的用户消息即可。

### 案例 1：新加一个页面 + 路由（最常见）

**场景**：要在 `/settings` 做一个「设置页」，先能打开、有标题，样式用 CSS Modules。

**你发给 Agent 的话**：

```text
已启用 frontend-vibe-slice。请新增路由 /settings 对应设置页：页面放在 pages（或我们项目约定目录），配好 React Router；样式用同名 module.scss；先不要接真实接口，占位文案即可。不要改 vite 配置。
```

**你期望 Agent 的行为**：只动页面、路由、样式相关文件；不升级依赖、不改 `vite.config.ts`。

---

### 案例 2：给一个页面接接口（services + 可选 store）

**场景**：登录页要调 `POST /api/login`，错误要提示。

**你发给 Agent 的话**：

```text
已启用 frontend-vibe-slice。在现有登录页接登录接口：请求封装进 services，页面里调封装函数；若需要跨页面共享 token 再用 Zustand，否则用页面 state。列出改动文件路径。不要动 package.json 里依赖。
```

**你期望 Agent 的行为**：先判断 token 要不要全局；请求集中进 `services/`，不在组件里到处写 `fetch`。

---

### 案例 3：只改样式 / 组件结构，不动业务逻辑

**场景**：首页布局要改成上下结构，不改接口。

**你发给 Agent 的话**：

```text
已启用 frontend-vibe-slice。只重构首页布局与 module.scss，业务逻辑和 services 不要动；不新增全局 store。
```

**你期望 Agent 的行为**：切片范围缩在「展示层」，避免顺手改数据层。

---

### 案例 4：和 Superpowers 串着用（先想清再落地）

**场景**：你先用 `writing-plans` 写好了计划文档，现在要按其中「任务 3」只做一块前端。

**你发给 Agent 的话**：

```text
已启用 frontend-vibe-slice。按计划文档 docs/xxx.md 里的「任务 3」只做前端部分：对应路由 /report，纵向切片交付；与计划冲突时以本 skill 的目录与 Zustand 边界为准。
```

**你期望 Agent 的行为**：单次对话只完成计划里那一块，不一口气实现整个计划。

---

### 案例 5：面试演示（30 秒操作）

口头顺序可以说给面试官听：

1. 「这是我们仓库里的自定义 Skill 文件路径。」（指着 `.cursor/skills/frontend-vibe-slice/SKILL.md`）  
2. 「我开 Agent，输入斜杠，选这个 skill。」  
3. 「然后我发一条具体需求，比如加设置页路由。」  
4. 「这样 AI 会按我写的约束来改，而不是乱动构建配置。」  

---

## 可直接保存的 `SKILL.md`（示例：个人前端切片约定）

把从 `---` 到文末的内容，保存为文件：  
**`.cursor/skills/frontend-vibe-slice/SKILL.md`**

```markdown
---
name: frontend-vibe-slice
description: 在本仓库做 React+Vite 功能开发时，按页面纵向切片交付；约束 Zustand 与目录，禁止擅自改构建配置。在用户要加页面、加路由、接接口或说「开发一个模块」时使用。
---

# Frontend Vibe Slice（个人约定）

## When to Use

- 新增或修改**一个可独立演示的路由/页面**时。
- 需要同时涉及：`pages/`、`*.module.scss`、可选 `stores/`、`services/` 时。
- 用户强调「小步」「可验收」「不要动全局配置」时。

## Instructions

1. **范围**：一次对话只完成 **一个纵向切片**（从路由到主交互可走通）。若需求过大，先列出拆分，再只做其中一块。
2. **技术栈**：React 函数组件 + Hooks；全局状态用 **Zustand**（按领域单文件 store）；样式用 **SCSS + CSS Modules**（`*.module.scss`）；构建以本仓库已有 **Vite/Webpack** 为准。
3. **禁止**：未经用户明确要求，**不要修改** `vite.config.*`、`webpack.config.*`、`package.json` 里的构建脚本与依赖（避免「顺手升级」）。
4. **数据流**：接口请求经 `services/`（或项目已有 api 层）；组件内不复制粘贴多处裸 `fetch`。
5. **Zustand**：新状态先判断能否用页面级 `useState`；确需跨组件再建或扩展 `stores/`，并在回复中说明「为何进全局」。
6. **验收**：交付前自检——路由可打开、主路径无控制台 error、加载/错误态有基本区分（若该页面有请求）。

## Output

- 列出本次新增或修改的**文件路径**。
- 若创建了 store，用一句话说明 **store 职责** 与 **不会放进去的数据**（例如不把整表缓存无限堆全局）。
```

---

## 和 Superpowers 的关系（面试可补一句）

- **Superpowers**：通用方法论（brainstorming、writing-plans 等）。
- **本 Skill**：你项目里的 **「前端切片 + 栈约束」** 专用补丁；两者可以同时用——先 Superpowers 想清楚，再 `frontend-vibe-slice` 约束落地方式。

---

*若改名：同时修改文件夹名、`name` 字段与 `description` 里出现的名称，保持一致。*
