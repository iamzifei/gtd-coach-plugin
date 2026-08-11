# GTD Coach - Claude Code Plugin

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/H2T024VDBG)

[English](README.md) | [中文](README_CN.md)

Personal GTD (Getting Things Done) coach that transforms annual goals into actionable daily tasks with automatic progress tracking.

## Features

- **Goal → SOP Framework**: Convert high-level goals into structured plans with quarterly milestones
- **Daily Task Generation**: Prioritized task lists based on your timeline position
- **Step-by-Step Guidance**: Break down each task into specific actionable steps
- **Automatic Memory**: Progress persists across conversations - no need to repeat context
- **Review System**: Daily, weekly, monthly, and quarterly review templates
- **Adaptive Planning**: Adjusts recommendations based on actual progress
- **Multi-language**: Supports English, 中文, 日本語

## Installation

### Via Claude Code CLI

```bash
/plugin marketplace add github:iamzifei/gtd-coach-plugin
```

### Manual Installation

```bash
git clone https://github.com/iamzifei/gtd-coach-plugin.git
cd gtd-coach-plugin
claude plugin install .
```

## Usage

### First Time Setup

Just tell Claude your goal:

```
I want to launch my SaaS and reach $10K MRR in 12 months.
Currently have an MVP, can spend 20 hours/week on this.
```

Claude will:
1. Create a structured SOP framework
2. Generate quarterly milestones
3. Save everything to memory
4. Generate your first day's tasks

### Daily Use

Simply ask:

```
What's today's task?
```

or in Chinese:

```
今天做什么？
```

Claude will:
1. Load your goal from memory
2. Show your current progress
3. Generate today's prioritized tasks
4. Offer to break down any task into steps

### Session End

When you're done:

```
End session
```

Claude will:
1. Summarize completed tasks
2. Update progress in memory
3. Prepare context for next session

## Commands

| Command | Action |
|---------|--------|
| "What's today's task?" / "今天做什么" | Generate daily task list |
| "Start task X" / "开始任务X" | Break down task into steps |
| "Done" / "完成了" | Update task status |
| "End session" / "结束" | Save progress, end session |
| "Do review" / "做复盘" | Trigger review template |
| "Show progress" / "查看进度" | Display goal overview |
| "Reset goal" / "重置目标" | Clear memory, start fresh |

## Memory Structure

The plugin stores these in Claude's memory:

```
GTD Goal: [Your goal]
GTD Target Date: [Deadline]
GTD Current Phase: [Q1-Validate/Q2-Scale/Q3-Systematize/Q4-Achieve]
GTD Progress: [X]%
GTD Last Session: [Date]
GTD Language: [Preference]
GTD Weekly Status: [Tasks completed]
```

## File Structure

```
gtd-coach-plugin/
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest
├── skills/
│   └── gtd-coach/
│       ├── SKILL.md         # Main skill instructions
│       ├── REVIEWS.md       # Review templates
│       ├── TEMPLATES.md     # Reusable templates
│       └── EXAMPLES.md      # Example conversations
├── README.md
├── README_CN.md
└── LICENSE
```

## Examples

### Goal Setup Flow

```
User: I want to grow my freelance income to $20K/month in 18 months

Claude: ## Your Goal Framework

### Goal Statement
**Grow freelance income from $X to $20K/month within 18 months**

### Quarterly Milestones
| Quarter | Phase | Milestone |
|---------|-------|-----------|
| Q1 | Validate | First $5K month |
| Q2 | Scale | Consistent $10K |
| Q3 | Systematize | $15K with systems |
| Q4-Q6 | Achieve | $20K target |

Saved to memory!

## Today's Tasks
...
```

### Returning User Flow

```
User: What's today's task?

Claude: ## Welcome back!

**Goal**: Freelance → $20K/month
**Progress**: 35% | **Phase**: Q2-Scale
**Last session**: 2 days ago

**Today's Tasks**
...
```

## Contributing

Contributions welcome! Please feel free to submit issues and pull requests.

## License

MIT License - feel free to use, modify, and distribute.

## Author

Created by [James](https://x.com/JamesAI)

---

**Note**: This plugin uses Claude's memory system. Memory persists within the same Claude account but resets if you clear memory or use incognito mode.
