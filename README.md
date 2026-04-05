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
| [baoyu-article-illustrator](skills/baoyu-article-illustrator) | 文章配图师：分析文章结构，识别需要配图的位置，生成插图 |
| [baoyu-xhs-images](skills/baoyu-xhs-images) | 小红书图片生成：10 种视觉风格 + 8 种布局，生成社交媒体信息图 |
| [create-skill](skills/create-skill) | 创建技能：引导用户创建有效的 Agent Skills |
| [docx](skills/docx) | Word 文档工具包：内容提取、文档生成、页面操作、表单处理 |
| [find-skills](skills/find-skills) | 发现技能：帮助用户发现和安装代理技能 |
| [follow-builders](skills/follow-builders) | AI Builders Digest：监控顶级 AI 创业者，生成精简摘要 |
| [frontend-design](skills/frontend-design) | 前端设计：创建独特、生产级别的高质量前端界面 |
| [humanizer-zh](skills/humanizer-zh) | 去除 AI 痕迹：使文本更自然、更像人类书写 |
| [install-skill-dependency](skills/install-skill-dependency) | 安装技能依赖：诊断并修复已安装技能所需的缺失依赖 |
| [json-canvas](skills/json-canvas) | JSON Canvas：创建和编辑 Obsidian Canvas 文件 |
| [kancolle-infographic](skills/kancolle-infographic) | 舰队Collection信息图：生成岛风风格的信息图和流程图 |
| [kirito-writing-style](skills/kirito-writing-style) | Kirito 写作风格：技术博客写作风格指南 |
| [medical-records-assistant](skills/medical-records-assistant) | 医疗档案助手：医疗档案管理与检查报告分析 |
| [notion-infographic](skills/notion-infographic) | Notion 信息图：批量生成 Notion 风格手绘信息图 |
| [obsidian-bases](skills/obsidian-bases) | Obsidian Bases：创建和编辑 Obsidian Bases 数据库视图 |
| [obsidian-cli](skills/obsidian-cli) | Obsidian CLI：命令行与 Obsidian vault 交互 |
| [obsidian-markdown](skills/obsidian-markdown) | Obsidian Markdown：创建和编辑 Obsidian 风格的 Markdown |
| [pdf](skills/pdf) | PDF 工具包：内容提取、文档生成、表单处理 |
| [pptx](skills/pptx) | PowerPoint 工具包：幻灯片生成、内容修改、演示分析 |
| [punkfarm](skills/punkfarm) | PunkFarm 赛博农场：赛博农场日常操作指南 |
| [think-before-act](skills/think-before-act) | 战略执行教练：防止盲目执行的苏格拉底式教练 |
| [xlsx](skills/xlsx) | Excel 工具包：数据提取、文档生成、公式处理 |
