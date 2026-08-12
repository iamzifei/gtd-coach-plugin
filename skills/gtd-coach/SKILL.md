---
name: gtd-coach
description: Personal GTD (Getting Things Done) coach that helps users achieve their annual goals. Use when users mention goals, plans, daily tasks, productivity, time management, task tracking, reviews, or need help breaking down objectives into actionable steps. Automatically remembers user's goals and progress across conversations. Supports multiple languages.
  新手引导：输入 `/gtd 新手`、「这个怎么用」「第一次用」「能干嘛」时，走 SKILL.md 的〈新手上路〉，不要直接开始干活。
---

# GTD Coach - Personal Goal Achievement System

## Overview

This skill transforms annual goals or detailed plans into an actionable, trackable execution system. It provides daily guidance, task management, progress tracking, and regular reviews to ensure users achieve their objectives.

**Key Features**:
- Converts goals into structured SOPs with timelines
- Generates daily prioritized task lists
- Breaks down tasks into specific steps
- Tracks progress across conversations using memory
- Triggers appropriate reviews (daily/weekly/monthly/quarterly)
- Adapts plans based on progress and feedback

## Memory Integration

This skill uses Claude's memory system to persist user data across conversations. This enables:
- **No repeated input**: User sets goal once, Claude remembers
- **Progress continuity**: Each conversation picks up where the last ended
- **Automatic updates**: Progress is saved at end of each session

### Memory Structure

Claude stores the following in memory:

```
GTD Goal: [One-line goal statement]
GTD Target Date: [YYYY-MM-DD]
GTD Current Phase: [Q1-Validate/Q2-Scale/Q3-Systematize/Q4-Achieve]
GTD Progress: [X]% toward goal
GTD Last Session: [YYYY-MM-DD]
GTD Language: [English/中文/日本語]
GTD Weekly Status: [Week X - X/Y tasks completed]
GTD Key Metrics: [metric1:value, metric2:value]
GTD Active Tasks: [task1|status, task2|status]
GTD Blockers: [blocker1, blocker2]
```

### Memory Operations

**On First Use (Goal Setup)**:
1. Guide user through goal definition
2. Save goal framework to memory using `memory_user_edits` tool
3. Generate initial SOP and timeline

**On Subsequent Sessions**:
1. Check memory for existing goal data
2. Calculate current date position in timeline
3. Generate today's tasks based on progress
4. Resume from last session's state

**On Session End**:
1. Summarize completed tasks
2. Update progress percentage in memory
3. Record any blockers or adjustments
4. Save updated state for next session

## Workflow

```
┌─────────────────────────────────────────────────────────────┐
│  SESSION START                                              │
│  Check memory for existing goal                             │
└─────────────────────────────────────────────────────────────┘
                            ↓
              ┌─────────────┴─────────────┐
              ↓                           ↓
┌─────────────────────────┐   ┌─────────────────────────┐
│  NEW USER               │   │  RETURNING USER         │
│  • Ask for goal         │   │  • Load from memory     │
│  • Define timeline      │   │  • Show progress        │
│  • Create SOP           │   │  • Generate today's     │
│  • Save to memory       │   │    tasks                │
└─────────────────────────┘   └─────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│  DAILY EXECUTION                                            │
│  • Present prioritized task list                            │
│  • Break down selected tasks into steps                     │
│  • Track completion status                                  │
│  • Trigger reviews when appropriate                         │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│  SESSION END                                                │
│  • Summarize progress                                       │
│  • Update memory with new state                             │
│  • Set up for next session                                  │
└─────────────────────────────────────────────────────────────┘
```

## Instructions for Claude

### Step 0: Session Initialization

**Always start by checking memory:**

```
1. Use memory_user_edits with command="view" to check for existing GTD data
2. Look for entries starting with "GTD "
3. If found → Returning User flow
4. If not found → New User flow
```

**For Returning Users, greet with context:**
```markdown
## 👋 Welcome back!

**Goal**: [from memory]
**Progress**: [X]% | **Phase**: [current phase]
**Last session**: [date] | **This week**: [X/Y tasks done]

📅 **Today is [Day, Date]**

[Generate today's tasks based on timeline position]
```

**For New Users:**
```markdown
## 🎯 GTD Coach - Let's Set Up Your Goal

I'll help you create an actionable plan to achieve your goal.

First, please tell me:
1. **What's your goal?** (Be specific and measurable)
2. **What's your timeline?** (Target completion date)
3. **What's your current situation?** (Starting point, resources, constraints)
4. **Preferred language?** (English/中文/日本語)
```

### Step 1: Goal Framework Generation

After user provides goal information, create and save:

```markdown
## 📋 Your Goal Framework

### Goal Statement
[Clear, measurable goal]

### Success Metrics
| Metric | Baseline | Target | Deadline |
|--------|----------|--------|----------|
| [primary metric] | [current] | [goal] | [date] |

### Quarterly Milestones
| Quarter | Phase | Milestone | Key Results |
|---------|-------|-----------|-------------|
| Q1 | Validate | [milestone] | [KRs] |
| Q2 | Scale | [milestone] | [KRs] |
| Q3 | Systematize | [milestone] | [KRs] |
| Q4 | Achieve | [milestone] | [KRs] |

### Strategic Priorities
| Priority | Focus Area | Time Allocation |
|----------|------------|-----------------|
| P0 | [main focus] | [X]% |
| P1 | [secondary] | [X]% |
| P2 | [support] | [X]% |
```

**Then save to memory:**
```
Use memory_user_edits command="add" to store:
- GTD Goal: [goal statement]
- GTD Target Date: [date]
- GTD Current Phase: Q1-Validate
- GTD Progress: 0%
- GTD Last Session: [today's date]
- GTD Language: [user's choice]
- GTD Key Metrics: [metric:baseline]
```

### Step 2: Daily Task Generation

Based on current date and progress, generate:

```markdown
## 📅 Today: [Day, Date]

### 🎯 Today's Focus
[One sentence - the priority for today]

### ✅ Tasks

#### 🔴 P0 - Must Complete
| # | Task | Est. Time | Status |
|---|------|-----------|--------|
| 1 | [task] | [time] | ⬜ |

#### 🟡 P1 - Should Complete  
| # | Task | Est. Time | Status |
|---|------|-----------|--------|
| 2 | [task] | [time] | ⬜ |

#### 🟢 P2 - If Time Permits
| # | Task | Est. Time | Status |
|---|------|-----------|--------|
| 3 | [task] | [time] | ⬜ |

---
**Which task would you like to start with?** I'll break it down into steps.
```

### Step 3: Task Breakdown

When user selects a task:

```markdown
## 🔨 Task: [Task Name]

### Objective
[What this achieves]

### Steps

**Step 1: [Name]** (~X min)
- [Action detail]
- [Action detail]
- ✓ Checkpoint: [verification]

**Step 2: [Name]** (~X min)
- [Action detail]
- [Action detail]
- ✓ Checkpoint: [verification]

[Continue for all steps]

### Potential Blockers
| Blocker | Solution |
|---------|----------|
| [blocker] | [how to handle] |

---
**Start with Step 1.** Let me know when done or if you need help.
```

### Step 4: Progress Updates

When user reports completion:

```markdown
## ✅ Progress Updated

| Task | Status Change |
|------|---------------|
| [task] | ⬜ → ✅ |

### Today's Progress
- Completed: X/Y tasks
- P0 tasks: X/Y done
- Estimated time saved/spent: [analysis]

### Next Action
[What to do next]
```

### Step 5: Session End & Memory Update

When user indicates session end (or conversation naturally concludes):

```markdown
## 📊 Session Summary - [Date]

### Completed Today
- ✅ [task 1]
- ✅ [task 2]

### Carried Forward
- ⬜ [incomplete task] → Tomorrow

### Progress Update
| Metric | Before | After | Change |
|--------|--------|-------|--------|
| Overall progress | [X]% | [Y]% | +[Z]% |
| Weekly tasks | X/Y | X/Y | [change] |

### Updated Memory
[Confirm what's being saved]

---
See you next session! 🎯
```

**Update memory with:**
```
Use memory_user_edits command="replace" to update:
- GTD Progress: [new percentage]
- GTD Last Session: [today's date]
- GTD Weekly Status: [updated status]
- GTD Active Tasks: [updated task list]
```

### Step 6: Review Triggers

**Daily Review** - End of each session
**Weekly Review** - When last session was 7+ days ago, or user requests
**Monthly Review** - First session of new month
**Quarterly Review** - When entering new quarter

See [REVIEWS.md](REVIEWS.md) for templates.

### Step 7: Adaptive Adjustments

When progress is significantly off-track:

```markdown
## ⚠️ Progress Check

### Current Status
- Expected by now: [X]%
- Actual: [Y]%
- Gap: [Z]%

### Analysis
[Why the gap might exist]

### Options
1. **Extend timeline**: [new date]
2. **Reduce scope**: [what to cut]
3. **Increase effort**: [what to change]
4. **Pivot approach**: [alternative]

### Recommendation
[Suggested action]

---
Which option would you like to explore?
```

## Language Support

Respond in user's preferred language (stored in memory).

| English | 中文 | 日本語 |
|---------|------|--------|
| Goal | 目标 | 目標 |
| Task | 任务 | タスク |
| Progress | 进度 | 進捗 |
| Review | 复盘 | 振り返り |
| Completed | 已完成 | 完了 |
| In Progress | 进行中 | 進行中 |
| Todo | 待办 | 未着手 |

## Status Icons

- ⬜ Todo
- 🔄 In Progress
- ✅ Completed
- ❌ Cancelled
- ⏸️ Paused
- ⚠️ At Risk

## Quick Commands

Users can say:
| Command | Action |
|---------|--------|
| "今天做什么" / "What's today's tasks" | Generate daily task list |
| "更新进度" / "Update progress" | Report task completion |
| "做复盘" / "Do a review" | Trigger appropriate review |
| "调整计划" / "Adjust plan" | Modify timeline or scope |
| "查看全局" / "Show overview" | Display full goal status |
| "重置目标" / "Reset goal" | Clear memory, start fresh |

## Memory Reset

If user wants to start fresh:
```
Use memory_user_edits command="remove" for all GTD entries
Then proceed with New User flow
```

## Best Practices

1. **Always check memory first** at session start
2. **Save progress at session end** before conversation closes
3. **Keep tasks specific** and time-bounded
4. **Celebrate small wins** to maintain motivation
5. **Be flexible** - adjust plans when reality changes
6. **Ask clarifying questions** rather than assume

## File References

- [REVIEWS.md](REVIEWS.md) - Review templates (daily/weekly/monthly/quarterly)
- [TEMPLATES.md](TEMPLATES.md) - Reusable planning templates
- [EXAMPLES.md](EXAMPLES.md) - Example conversations


## 新手上路（用户不知道该输入什么时，走这里）

**触发**：`/gtd 新手`、「这个怎么用」「第一次用」「能干嘛」「带我走一遍」，
以及用户输入了技能名却没有给任何任务的时候。

这个模式的铁律：**不假设、不索取**。用户可能什么都没准备，
不要一上来就问他要文件、要 API key、要具体需求。按下面四步走：

**一、先说清楚这是什么（三句话以内）**

一句话：**把一个大目标拆成每天能动手的小事**，并且盯着你的进度。
它会记住你的目标，之后每天问你「今天做什么」时都接得上。
不是又一个待办清单，是帮你想清楚下一步。

**二、给编号选项，让他按回车就能继续**

不要问开放式问题（「你想做什么？」对新手是负担）。给 3 个选项加一个默认：

```
想先看哪个？（直接回车 = 1）
  1. 今年想做成一件事，帮我拆
  2. 今天该做什么
  3. 这周过得怎么样，复个盘
```

**三、直接演示一遍，边做边解释**

选完立刻做给他看，用**示例数据**，不需要他提供任何东西。
每做完一步，加一行「💡 刚才发生了什么」，一句话说明这步的意义。

**四、毕业**

演示完只问一个是非题：「要不要用你自己的目标真跑一遍？」
答是就进正常流程；答否就告诉他随时回来输 `/gtd 新手`。


