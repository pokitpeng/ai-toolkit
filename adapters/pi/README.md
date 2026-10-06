# Pi 接入

仓库通过根目录 `package.json` 暴露共享 skills 和 prompts，无需将核心内容改成 Pi 专用格式。

## 从 Git 安装

```bash
pi install git:github.com/pokitpeng/ai-toolkit
```

## 本地开发

```bash
git clone https://github.com/pokitpeng/ai-toolkit.git
cd ai-toolkit
pi install .
```

以上两种方式择一使用。本地方式直接加载工作目录中的资源，适合持续编辑；默认写入个人 Pi 配置。若只希望当前项目使用，请在目标项目中运行 `pi install -l /absolute/path/to/ai-toolkit`，并审阅项目内容后再授予信任。

编辑资源后，在正在运行的 Pi 会话中执行 `/reload`。

## 资源映射

| 仓库位置 | Pi 行为 |
| --- | --- |
| `skills/<name>/SKILL.md` | 发现技能，按需加载；可用 `/skill:<name>` 显式调用 |
| `prompts/<name>.md` | 作为 `/<name>` 提示词模板加载 |
| `agents/` | 不会自动加载，需接入子代理扩展 |
| `adapters/pi/` | 当前仅有说明文档，不作为运行时资源加载 |

目录中的 `.gitkeep` 仅用于保留空目录，不是实际资源。

## 子代理与扩展

Pi package 原生资源类型包括 skills、prompts、extensions 和 themes，不包含 agents。

如以后采用 Pi 的 subagent 示例或其他子代理扩展，需要按该扩展要求转换角色元数据并配置发现路径。Pi 的 subagent 示例默认读取 `~/.pi/agent/agents/*.md`；不能仅创建本仓库的 `agents/` 就期望自动生效。

Pi 专用扩展可以放入本目录下的 `extensions/`，实现并审阅后再添加到根目录 `package.json` 的 `pi.extensions`。不要把文档目录整体声明为扩展入口。

`private: true` 用于防止误发布到 npm，不影响 Git 或本地安装。
