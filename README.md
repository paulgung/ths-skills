# THS Skills

A collection of skills for AI coding agents. Skills are packaged instructions that extend agent capabilities for structured technology research workflows.

Skills follow the [Agent Skills](https://github.com/anthropics/skills) format and are compatible with Cursor.

## Install a Skill

Using the skills CLI:

```bash
npx skills add pxgung/ths-skills
```

### Source Formats

```bash
# GitHub shorthand
npx skills add pxgung/ths-skills

# Full GitHub URL
npx skills add https://github.com/pxgung/ths-skills

# Direct path to a specific skill
npx skills add https://github.com/pxgung/ths-skills/tree/main/skills/four-knowns

# Local path
npx skills add ./ths-skills
```

### Manual Installation (Cursor)

If you prefer not to use the CLI, copy the skill directly:

```bash
git clone https://github.com/pxgung/ths-skills ths-skills

# Install globally
cp -r ths-skills/skills/four-knowns ~/.cursor/skills/
```

### Installation Scope

| Scope | Location | Use Case |
|-------|----------|----------|
| Project | `./.agents/skills/` | Committed with project, team use |
| Global | `~/.cursor/skills/` | Available across all projects |

Use `-g` or `--global` with the skills CLI for global installation.

## Available Skills

### four-knowns

对指定技术领域进行"四个知道"系统性调研，覆盖横向四知道和纵向四知道共八个维度。

**Use when:**
- "帮我调研一下 [技术领域] 的发展情况"
- "对 [领域] 做一份四个知道分析"
- "做一份技术调研 / 竞品分析 / 行业分析"

**Features:**

**横向四知道**（宏观环境扫描）:

| 维度 | 关注点 |
|------|--------|
| （1）社会发展2-3年行业的趋势 | 市场规模、增长预测、拐点信号、投融资动态 |
| （2）竞争对手的情况 | 竞争格局、玩家画像、差异化对比、商业模式 |
| （3）政策监控方向 | 国内外政策、行业标准、合规红线、政策机遇 |
| （4）技术发展 | 技术路线图、关键突破、瓶颈、开源生态 |

**纵向四知道**（技术深度剖析）:

| 维度 | 关注点 |
|------|--------|
| （1）主要的paper，理论上能达到的，学术前沿 | 顶级论文、理论极限、前沿方向、关键研究组 |
| （2）工业水平，最牛掰的公司达到的工程架构，市场上流行的 | 标杆企业 Top 5、性能基准、市场主流方案 |
| （3）牛掰的架构，主要的工程模块 | 最优架构设计、核心模块拆解、技术栈推荐 |
| （4）我们技术在行业中的位置，可以达到的程度 | 行业分位评估、差距分析、发展路线图 |

**Output structure:**

调研报告按八个维度逐一输出，每个维度包含调研要点和结构化表格，最终产出综合 SWOT 分析和分阶段行动建议。

## Usage

Skills are loaded on demand. When the user's message matches the skill's trigger phrases, the agent reads `SKILL.md` and applies its instructions.

Examples:

- `帮我调研一下大语言模型领域的发展情况`
- `对自动驾驶感知系统做一份四个知道分析`
- `我们在做联邦学习，帮我做一份深度技术调研`

## What are Agent Skills?

Agent skills are reusable instruction sets that extend your coding agent's capabilities. They are defined in `SKILL.md` files with YAML frontmatter containing a `name` and `description`. The agent loads a skill when the user's request matches the description.

Skills let agents perform specialized tasks like:

- Structured technology research across 8 dimensions
- Industry trend analysis and competitor benchmarking
- Academic frontier tracking with paper references
- Engineering architecture review and positioning assessment

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
│   └── four-knowns/
│       └── SKILL.md        # Agent instructions
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
