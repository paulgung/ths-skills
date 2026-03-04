# THS Skills

Agent Skills 集合，用于增强 AI Agent 在特定场景下的任务执行能力。

遵循 [Anthropic Agent Skills](https://github.com/anthropics/skills) 规范。

## Skills 列表

### tech-research — 技术调研八维矩阵

对指定技术领域进行系统性调研，覆盖**横向四知道**和**纵向四知道**共八个维度。

**横向四知道**（宏观环境扫描）:
| 维度 | 关注点 |
|------|--------|
| 行业趋势 | 2-3年社会发展与行业演进方向 |
| 竞争对手 | 竞争格局、核心玩家画像与差异化分析 |
| 政策监管 | 国内外政策动态、合规要求与政策机遇 |
| 技术发展 | 技术路线演进、关键突破与瓶颈 |

**纵向四知道**（技术深度剖析）:
| 维度 | 关注点 |
|------|--------|
| 学术前沿 | 顶级论文、理论上限、前沿研究方向 |
| 工业水平 | 全球标杆企业的工程能力与性能基准 |
| 工程架构 | 最优系统设计、核心模块与技术栈 |
| 自身定位 | 行业分位评估、差距分析与发展路线图 |

#### 快速开始

在 AI Agent 中触发：
> "帮我调研一下 [技术领域] 的发展情况"

#### 文件结构

```
skills/tech-research/
├── SKILL.md              # 核心指令文件
├── research-template.md  # 调研报告输出模板
└── examples.md           # 使用示例
```

## 如何使用

### Cursor

将 skill 目录放置在以下位置之一：
- **个人 skill**: `~/.cursor/skills/tech-research/`
- **项目 skill**: `.cursor/skills/tech-research/`

### Claude Code

```bash
/plugin marketplace add [your-repo]
```

### Claude.ai / API

按照 [Anthropic Skills 文档](https://support.claude.com/en/articles/12512180-using-skills-in-claude) 上传 skill 文件夹。

## 贡献

欢迎提交新的 Skill！请遵循以下规范：
1. 每个 Skill 独立一个文件夹，包含 `SKILL.md`
2. `SKILL.md` 需包含 YAML frontmatter（`name` + `description`）
3. 保持 `SKILL.md` 在 500 行以内
4. 详细内容使用附加文件（如 `reference.md`、`examples.md`）

## 许可证

Apache 2.0
