# cj-skills

个人维护的 **Cursor Agent Skills** 集合，按技能分子目录存放。

## 目录说明

| 文件夹 | 说明 |
| :--- | :--- |
| [`frontend-vibe-slice/`](./frontend-vibe-slice/) | React + Zustand + SCSS + Vite/Webpack 下的「纵向切片」开发约定；含 `SKILL.md` 与使用文档 |

## 如何用到自己的项目里

1. 克隆本仓库（或只复制某一技能文件夹）。
2. 将例如 `frontend-vibe-slice` **整个目录**复制到你的前端项目：  
   **`<你的项目>/.cursor/skills/frontend-vibe-slice/`**  
   确保 **`SKILL.md`** 在该路径下，且与 YAML 里 `name: frontend-vibe-slice` 一致。
3. 在 Cursor **Agent** 中输入 **`/`**，搜索 **`frontend-vibe-slice`** 选用。

详见各子目录内的 `frontend-vibe-slice-skill.md`。

## 规范参考

- [Agent Skills](https://agentskills.io/)
- [Cursor 文档 · Skills](https://cursor.com/docs/context/skills)
