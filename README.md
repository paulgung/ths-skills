# THS Skills

一个面向 AI coding agent 的 **Agent Skills** 仓库示例集合。Skill 本质是一组可复用的指令（通常在 `SKILL.md` 里），用于让 Agent 在特定请求下自动采用更稳定的流程与产出结构。

Skills follow the [Agent Skills](https://github.com/anthropics/skills) format and are compatible with Cursor.

## 快速开始（安装）

使用 `skills` CLI 安装（推荐）：

```bash
npx skills add paulgung/ths-skills
```

### 常见安装来源（Source formats）

```bash
# GitHub shorthand
npx skills add paulgung/ths-skills

# Full GitHub URL
npx skills add https://github.com/paulgung/ths-skills

# Direct path to a specific skill
npx skills add https://github.com/paulgung/ths-skills/tree/main/skills/four-knowns

# Local path
npx skills add ./ths-skills
```

### Manual Installation (Cursor)

If you prefer not to use the CLI, copy the skill directly:

```bash
git clone https://github.com/paulgung/ths-skills ths-skills

# Install a specific skill globally
cp -r ths-skills/skills/four-knowns ~/.cursor/skills/
cp -r ths-skills/skills/company-analysis ~/.cursor/skills/
```

### 安装范围（Installation scope）

| Scope | Location | Use Case |
|-------|----------|----------|
| Project | `./.agents/skills/` | Committed with project, team use |
| Global | `~/.cursor/skills/` | Available across all projects |

Use `-g` or `--global` with the skills CLI for global installation.

## 如何使用（在对话中触发）

Skill 是“按需加载”的：当你的提问与 Skill 的描述/触发语句匹配时，Agent 会读取对应的 `SKILL.md` 并按其中流程输出。

示例（本仓库相关）：

- `帮我调研一下大语言模型领域的发展情况`
- `对自动驾驶感知系统做一份四个知道分析`
- `我们在做联邦学习，帮我做一份深度技术调研`
- `帮我分析一下 NASDAQ:GOOGL 这家公司`
- `拆解一下苹果的商业模式和护城河`
- `做一份腾讯的公司深度分析`
- `帮我给这个函数写注释`
- `审查一下 src/api/ 目录的注释质量`

## 本仓库包含的 Skills（索引）

每个 Skill 的详细触发方式与输出结构请看对应目录下的 `SKILL.md` 和 `README.md`。

| Skill | 目录 | 用途 |
|-------|------|------|
| 四个知道 | `skills/four-knowns/` | 技术调研 / 行业分析 / 竞品分析 |
| 公司深度分析 | `skills/company-analysis/` | 上市公司多维度分析（导师视角） |
| 代码注释指南 | `skills/code-comments/` | 注释哲学 / 各层级写法 / 质量审计 |

## What are Agent Skills?

Agent skills are reusable instruction sets that extend your coding agent's capabilities. They are defined in `SKILL.md` files with YAML frontmatter containing a `name` and `description`. The agent loads a skill when the user's request matches the description.

Skills let agents perform specialized tasks like:

- Structured technology research across 8 dimensions
- Industry trend analysis and competitor benchmarking
- Academic frontier tracking with paper references
- Engineering architecture review and positioning assessment
- Multi-dimensional company analysis with mentor perspective
- Business model teardown and moat assessment

## Supported Agents

This repository targets **Cursor**. Skills use the Agent Skills specification and may work with other agents that support it.

| Agent | Install Path |
|-------|-------------|
| Cursor (global) | `~/.cursor/skills/` |
| Cursor (project) | `.agents/skills/` |

## Repository Structure

```
ths-skills/
├── skills/
│   ├── four-knowns/
│   │   ├── SKILL.md        # 四个知道·技术调研
│   │   └── README.md
│   ├── company-analysis/
│   │   └── SKILL.md        # 公司深度分析·导师视角
│   └── code-comments/
│       ├── SKILL.md        # 代码注释指南·注释哲学与审计
│       └── README.md
└── README.md
```

## Contributing

欢迎提交新的 Skill！请遵循以下规范：

1. 每个 Skill 独立一个文件夹，包含 `SKILL.md`
2. `SKILL.md` 需包含 YAML frontmatter（`name` + `description`）
3. 保持 `SKILL.md` 在 500 行以内
4. 详细内容使用附加文件（如 `reference.md`）

## License

MIT
