# Awesome-Personal-Skills

本仓库主要收集一些个人觉得比较实用的skills，包括日常学习和新环境部署等等。如有帮助的话您的话我不胜荣幸。

## 目录约定

- 通用 skills 放在 [`.agents/skills/`](./.agents/skills/) 下，作为跨客户端共享的 canonical 入口。
- Claude Code 通过 [`.claude/skills/`](./.claude/skills/) 下的薄 wrapper 自动发现 skills；wrapper 只负责转发到 `.agents/skills/`，不复制完整工作流。
- OpenAI API local shell 使用时，应显式把 `.agents/skills/<skill-name>` 作为 skill path 传入。
- [`.learn/`](./.learn/) 目录继续保留为学习工作流的资源目录，并由 [`.agents/skills/learn/`](./.agents/skills/learn/) 包装成标准 skill。

## skills来源

1. [博客学习](./.agents/skills/learn/): 此 skill 由保留的 [`.learn/`](./.learn/readme.md) 工作流包装而来，原始内容来自 [Awesome-ML-SYS-Tutorial](https://github.com/zhaochenyang20/Awesome-ML-SYS-Tutorial).
2. [user-env-deployer](./.agents/skills/user-env-deployer/): 此 skill 来源自 sglang 项目里 docker 开发环境的有关配置.
3. [arxiv-paper-reader](./.agents/skills/read-arxiv-paper/): 此 skill 来源自 karpathy 的 [nanochat](https://github.com/karpathy/nanochat) 项目.

