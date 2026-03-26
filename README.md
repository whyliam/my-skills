# My Skills

Personal Agent Skills for daily use. These skills follow the [Agent Skills specification](https://agentskills.io/specification) so they can be used by any skills-compatible agent, including Claude Code and Codex CLI.

## Installation

### npx skills

```
npx skills add git@github.com:whyliam/my-skills.git
```

### Manually

#### Claude Code

Add the contents of this repo to a `/.claude` folder in the root of your project. See more in the [official Claude Skills documentation](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview).

#### Codex CLI

Copy the `skills/` directory into your Codex skills path (typically `~/.codex/skills`). See the [Agent Skills specification](https://agentskills.io/specification) for the standard skill format.

#### OpenCode

Clone the entire repo into the OpenCode skills directory (`~/.opencode/skills/`):

```sh
git clone https://github.com/whyliam/my-skills.git ~/.opencode/skills/my-skills
```

Do not copy only the inner `skills/` folder — clone the full repo so the directory structure is `~/.opencode/skills/my-skills/skills/<skill-name>/SKILL.md`.

OpenCode auto-discovers all `SKILL.md` files under `~/.opencode/skills/`. No changes to `opencode.json` or any config file are needed. Skills become available after restarting OpenCode.

## Skills

| Skill | Description |
|-------|-------------|
| [humanizer-zh](skills/humanizer-zh) | 去除文本中的 AI 生成痕迹，使其更自然、更像人类书写 From |
| [kancolle-infographic](skills/kancolle-infographic) | 生成舰队Collection岛风风格的信息图和流程图 From [QOrder](https://docs.qoder.com/zh/qoderwork/skills)  |
| [kirito-writing-style](skills/kirito-writing-style) | Kirito 博客写作风格指南，撰写技术博客、公众号文章 From [QOrder](https://docs.qoder.com/zh/qoderwork/skills)  |
| [notion-infographic](skills/notion-infographic) | 根据参考文稿批量生成 Notion 风格手绘信息图组图 From [QOrder](https://docs.qoder.com/zh/qoderwork/skills)  |
