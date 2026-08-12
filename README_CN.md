# GTD Coach - Claude Code 插件

[English](README.md) | [中文](README_CN.md)

个人 GTD（Getting Things Done）教练，将年度目标转化为可执行的每日任务，并自动追踪进度。

## 功能特性

- **目标 → SOP 框架**：将高层目标转化为带有季度里程碑的结构化计划
- **每日任务生成**：根据时间进度生成优先级任务列表
- **分步指导**：将每个任务分解为具体可执行的步骤
- **自动记忆**：进度跨对话保存 - 无需重复上下文
- **复盘系统**：日、周、月、季度复盘模板
- **自适应规划**：根据实际进度调整建议
- **多语言支持**：支持 English、中文、日本語

## 怎么装（没用过命令行也能跟着做）

> 下面每一步都可以**直接复制粘贴**。不用懂什么是「技能」「仓库」「终端」，照做就行。
> 哪一步卡住了，**把报错原样复制给 AI，让它帮你看**——这是最快的解决办法。

### 方式一：WorkBuddy（推荐，不用敲命令）

1. **下载安装 WorkBuddy**，打开它，登录。
2. 在电脑上**新建一个文件夹**，名字随便，比如 `我的AI技能`，放桌面上就行。记住它在哪。
3. **把本项目下载下来**：
   - 在本页面点绿色的 **Code** 按钮 → **Download ZIP**
   - 下载完**解压**，把解压出来的整个文件夹拖进刚才建的 `我的AI技能` 里
4. 打开 WorkBuddy 的**设置 → 技能（Skills）**，点「添加技能目录」，选中 `我的AI技能` 这个文件夹。
5. **重启 WorkBuddy**。
6. 在对话框里输入 **`/gtd 新手`**，回车。

看到它开始问你「想从哪种目标开始」，就是装好了。

> 找不到「技能」入口？在设置里搜 `skill`，或者直接问 WorkBuddy：「我要怎么添加本地技能目录？」
> WorkBuddy 的菜单名称可能随版本变化，但「添加技能目录」这一步的意思是一样的。

### 方式二：Claude Code（会用终端的人）

```bash
git clone https://github.com/iamzifei/gtd-coach-plugin.git
```

把 clone 下来的文件夹放进 Claude Code 的技能目录（`~/.claude/skills/`），重启，输入 `/gtd 新手`。

### 方式三：其他支持技能的 AI 工具（Codex、豆包、Trae 等）

这些工具大多会读一个固定的技能目录，常见的是 `~/.agents/skills`、`~/.codex/skills`。
把本项目文件夹复制进去，重启工具，输入 `/gtd 新手`。

### 不知道装没装上？

在对话框里输入 `/gtd 新手`。**它会自己介绍自己、并带你走一遍**——
不用你想好要问什么，也不用准备任何材料。


## 安装

### 通过 Claude Code CLI

```bash
/plugin marketplace add github:iamzifei/gtd-coach-plugin
```

### 手动安装

```bash
git clone https://github.com/iamzifei/gtd-coach-plugin.git
cd gtd-coach-plugin
claude plugin install .
```

## 使用方法

### 首次设置

只需告诉 Claude 你的目标：

```
我想在12个月内推出我的 SaaS 并达到 $10K MRR。
目前有 MVP，每周可以投入20小时。
```

Claude 将会：
1. 创建结构化的 SOP 框架
2. 生成季度里程碑
3. 保存所有内容到记忆
4. 生成第一天的任务

### 日常使用

简单询问：

```
今天做什么？
```

或用英文：

```
What's today's task?
```

Claude 将会：
1. 从记忆中加载你的目标
2. 显示当前进度
3. 生成今日优先任务
4. 提供任务分解选项

### 结束会话

完成后：

```
结束
```

Claude 将会：
1. 总结已完成的任务
2. 更新记忆中的进度
3. 为下次会话准备上下文

## 命令

| 命令 | 操作 |
|------|------|
| "今天做什么" / "What's today's task?" | 生成每日任务列表 |
| "开始任务X" / "Start task X" | 分解任务为步骤 |
| "完成了" / "Done" | 更新任务状态 |
| "结束" / "End session" | 保存进度，结束会话 |
| "做复盘" / "Do review" | 触发复盘模板 |
| "查看进度" / "Show progress" | 显示目标概览 |
| "重置目标" / "Reset goal" | 清除记忆，重新开始 |

## 记忆结构

插件在 Claude 记忆中存储以下内容：

```
GTD Goal: [你的目标]
GTD Target Date: [截止日期]
GTD Current Phase: [Q1-验证/Q2-扩展/Q3-系统化/Q4-达成]
GTD Progress: [X]%
GTD Last Session: [日期]
GTD Language: [语言偏好]
GTD Weekly Status: [完成的任务]
```

## 文件结构

```
gtd-coach-plugin/
├── .claude-plugin/
│   └── plugin.json          # 插件清单
├── skills/
│   └── gtd-coach/
│       ├── SKILL.md         # 主要技能说明
│       ├── REVIEWS.md       # 复盘模板
│       ├── TEMPLATES.md     # 可复用模板
│       └── EXAMPLES.md      # 示例对话
├── README.md
├── README_CN.md
└── LICENSE
```

## 示例

### 目标设置流程

```
用户: 我想在18个月内将自由职业收入提升到每月 $20K

Claude: ## 你的目标框架

### 目标声明
**在18个月内将自由职业收入从 $X 提升到每月 $20K**

### 季度里程碑
| 季度 | 阶段 | 里程碑 |
|------|------|--------|
| Q1 | 验证 | 首次月入 $5K |
| Q2 | 扩展 | 稳定 $10K |
| Q3 | 系统化 | $15K 并建立系统 |
| Q4-Q6 | 达成 | $20K 目标 |

已保存到记忆！

## 今日任务
...
```

### 老用户流程

```
用户: 今天做什么？

Claude: ## 欢迎回来！

**目标**: 自由职业 → $20K/月
**进度**: 35% | **阶段**: Q2-扩展
**上次会话**: 2天前

**今日任务**
...
```

## 贡献

欢迎贡献！请随时提交 issues 和 pull requests。

## 许可证

MIT 许可证 - 可自由使用、修改和分发。

## 作者

由 [James](https://x.com/JamesAI) 创建

---

**注意**: 本插件使用 Claude 的记忆系统。记忆在同一 Claude 账户内持久化，但清除记忆或使用隐身模式时会重置。
