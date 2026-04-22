---

## name: frontend-vibe-slice
description: 在本仓库做 React+Vite 功能开发时，按页面纵向切片交付；约束 Zustand 与目录，禁止擅自改构建配置。在用户要加页面、加路由、接接口或说「开发一个模块」时使用。

# Frontend Vibe Slice（个人约定）

## When to Use

- 新增或修改**一个可独立演示的路由/页面**时。
- 需要同时涉及：`pages/`、`*.module.scss`、可选 `stores/`、`services/` 时。
- 用户强调「小步」「可验收」「不要动全局配置」时。

## Instructions

1. **范围**：一次对话只完成 **一个纵向切片**（从路由到主交互可走通）。若需求过大，先列出拆分，再只做其中一块。
2. **技术栈**：React 函数组件 + Hooks；全局状态用 **Zustand**（按领域单文件 store）；样式用 **SCSS + CSS Modules**（`*.module.scss`）；构建以本仓库已有 **Vite/Webpack** 为准。
3. **禁止**：未经用户明确要求，**不要修改** `vite.config.`*、`webpack.config.*`、`package.json` 里的构建脚本与依赖（避免「顺手升级」）。
4. **数据流**：接口请求经 `services/`（或项目已有 api 层）；组件内不复制粘贴多处裸 `fetch`。
5. **Zustand**：新状态先判断能否用页面级 `useState`；确需跨组件再建或扩展 `stores/`，并在回复中说明「为何进全局」。
6. **验收**：交付前自检——路由可打开、主路径无控制台 error、加载/错误态有基本区分（若该页面有请求）。

## Output

- 列出本次新增或修改的**文件路径**。
- 若创建了 store，用一句话说明 **store 职责** 与 **不会放进去的数据**（例如不把整表缓存无限堆全局）。

