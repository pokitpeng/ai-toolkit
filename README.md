# AI Toolkit

个人 AI 资源库：集中维护可复用的 skills、agents 和 prompts，通过适配层接入不同 AI 工具。

## 目录

```text
ai-toolkit/
├── skills/          # Agent Skills 格式的技能及配套资源
├── agents/          # 工具无关的角色定义
├── prompts/         # 工具无关的提示词
├── adapters/
│   └── pi/          # Pi 接入说明及未来的专用配置
├── scripts/         # 未来的安装、同步和校验脚本
└── package.json     # 可选的 Pi package 安装入口
```

## 内容约定

### Skills

每个技能使用独立目录，遵循 [Agent Skills 规范](https://agentskills.io/specification)：

```text
skills/<skill-name>/
├── SKILL.md
├── scripts/         # 可选
├── references/      # 可选
└── assets/          # 可选
```

`SKILL.md` 示例（按实际用途替换后再添加）：

```markdown
---
name: skill-name
description: 说明技能做什么，以及何时使用。
---

# 技能名称

写明输入、执行步骤、限制和预期输出。
```

名称使用小写字母、数字和连字符，并与目录名保持一致。资源路径相对于技能目录；需要额外依赖时明确记录安装方法。遵循规范不代表所有工具都支持全部字段。

### Agents

在 `agents/<role>.md` 中记录角色职责、输入、工作流程、边界和输出格式。

这里的 Markdown 是共享内容，不是跨工具统一的可执行配置。模型标识、工具权限、子代理调用方式等放入 `adapters/<tool>/`，由适配层处理。

### Prompts

在 `prompts/<name>.md` 中保存可直接使用的提示词，不依赖特定工具的变量语法。有平台专用参数或语法的版本放入适配层。

不要在 `prompts/` 中放说明性 README，避免被工具识别为可调用提示词。

### Adapters 与 Scripts

适配层只维护工具特有的配置、扩展和接入说明，尽量引用共享内容，避免复制后分叉。脚本按需添加；涉及覆盖本地配置时应明确提示并提供备份方式。

## 使用

- 通用：按目标工具的能力引用或复制所需资源。
- Pi：参见 [Pi 接入说明](adapters/pi/README.md)。根目录的 `package.json` 只提供安装入口，不限制资源被其他工具使用。

### 可用技能

| 技能 | 用途 |
| --- | --- |
| [architecture-docs](skills/architecture-docs/SKILL.md) | 基于项目实际情况编写或更新系统架构文档，涵盖组件、接口、数据模型和开发约束 |

Pi 安装本仓库后，可使用 `/skill:architecture-docs`，或附带要求，例如：

```text
/skill:architecture-docs 为当前项目更新架构文档，重点检查部署和故障处理设计
```

其他支持 Agent Skills 的工具可按各自的发现方式加载 `skills/architecture-docs/`。

## 安全

不要提交 API Key、认证文件、真实 `.env`、会话历史、日志或个人敏感数据。示例配置应使用占位符；`.gitignore` 不能代替提交前检查。

技能可能包含脚本，扩展可能执行代码，使用前应审阅来源与权限。新增第三方内容时保留其许可证和来源说明。
