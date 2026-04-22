---
name: frontend-outsourcing-guardrails
description: Use when managing frontend outsourcing deliverables, reviewing implementation plans, or checking code compliance for React Hooks, SCSS, Vite, Zustand, and JavaScript security restrictions.
---

# Frontend Outsourcing Guardrails (Simple)

> 中文说明：本技能用于外包前端项目管理，快速判断技术栈是否符合要求，并识别高风险 JavaScript 安全问题。

## Purpose

Use this skill to keep outsourcing frontend delivery aligned with required stack and security baseline.
  
中文注释：用于确保外包交付始终遵循指定技术栈与安全基线，减少返工和安全隐患。

## Required Stack

- React function components + Hooks only
- SCSS only (prefer `.module.scss`)
- Vite as build tool
- Zustand for shared/global state
  
中文注释：
- 仅允许 React 函数组件与 Hooks，不建议 class 组件。
- 样式统一 SCSS，推荐使用 `.module.scss` 避免全局污染。
- 构建工具固定为 Vite，避免多套构建链路并存。
- 跨页面共享状态统一由 Zustand 管理。

## Security Red Lines (Must Reject)

- `eval(...)`
- `new Function(...)`
- string-based `setTimeout("...")` / `setInterval("...")`
- `document.write(...)`
- unsafe `innerHTML` with untrusted input
- `dangerouslySetInnerHTML` without strict sanitization
- prototype pollution patterns (`Object.prototype` changes, unsafe deep merge from external input)
- hardcoded secrets/tokens/private endpoints in frontend code
  
中文注释：
- 以上属于高风险写法，发现任意一项应直接驳回整改。
- 重点关注 XSS、原型污染、动态执行代码、敏感信息泄露四类风险。

## Review Workflow

1. Check stack compliance first.
2. Scan for red-line JavaScript patterns.
3. Confirm lint/test/build/security checks pass.
4. Reject delivery if any red-line issue exists.
  
中文注释：
1. 先核对技术栈是否符合要求。
2. 再扫描是否存在安全红线写法。
3. 最后确认 lint、测试、构建、安全检查结果。
4. 只要出现红线问题，必须驳回，不可带病上线。

## Acceptance Checklist

- Stack matches: React Hooks + SCSS + Vite + Zustand
- No prohibited JavaScript/security patterns
- Lint has no errors
- Build passes
- Basic tests and error handling exist
  
中文注释：
- 验收时按清单逐项勾选，避免“口头通过”。
- 若业务紧急，也不能跳过安全红线检查。

## Reference

- Detailed policy: `前端外包技术规范.md`
  
中文注释：需要完整条款时，查阅同目录下详细规范文档。
