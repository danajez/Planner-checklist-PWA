# Focus Block Planner v1.0

## Purpose

A mobile-first Progressive Web App (PWA) designed for structured workdays using focus blocks.

The app's primary goal is:

> Tell me what I should work on right now, while allowing me to adapt when priorities change.

The system should minimize weekly planning by using recurring templates for Monday, Wednesday, and Friday.

---

# Technology Stack

- React
- TypeScript
- Tailwind CSS
- Progressive Web App (PWA)
- localStorage
- Offline First
- No Backend
- No Account System
- No Authentication

---

# Work Schedule

## Monday

Working hours:

```text
09:00 - 14:00
```

Focus blocks:

```text
09:00 - 09:35
09:50 - 10:25
10:40 - 11:15
11:30 - 12:05
12:20 - 12:55
13:10 - 13:45
```

---

## Wednesday and Friday

Working hours:

```text
09:00 - 16:15
```

Focus blocks:

```text
09:00 - 09:35
09:50 - 10:25
10:40 - 11:15
11:30 - 12:05
12:20 - 12:55
13:10 - 13:45
14:00 - 14:35
14:50 - 15:25
15:40 - 16:15
```

---

# Break Blocks

Each focus block is followed by a 15-minute break.

Users may define recurring break activities.

Examples:

```text
Coffee
Walk
Stretching
Drink Water
Inbox Zero
```

## Break Activity Rules

- Optional
- Can be recurring
- Can be completed
- Do not carry over
- If skipped, automatically expire

---

# Planning Philosophy

The user should not have to plan every week.

The planner uses:

1. Recurring templates
2. One-off tasks
3. Automatic carry-over

---

# Task Types

## Recurring Template Tasks

Examples:

```text
Monday 09:00
Weekly Planning
```

```text
Wednesday 10:40
Documentation
```

Recurring tasks:

- Belong to Monday, Wednesday, or Friday templates
- Automatically reappear every future occurrence
- Can be edited
- Can be deleted

Completing a recurring task instance must not change its template.

---

## One-Off Tasks

Examples:

```text
Renew Hosting
Fix Website Issue
Prepare Annual Report
```

Rules:

- Created manually
- Scheduled for one date
- Never reappear after completion

---

## Carried-Over Tasks

Examples:

```text
Research Competitors
Update Database
Prepare Proposal
```

Rules:

- Created automatically
- Generated from unfinished tasks
- Move to the next workday

---

# Schedule Generation

Every workday the application generates today's schedule automatically.

Formula:

```text
Today's Schedule =
Carried Over Tasks
+
Recurring Template Tasks
+
One-Off Tasks
```

No manual weekly planning is required.

---

# Core Workflow

## Initial Setup

User creates:

- Monday Template
- Wednesday Template
- Friday Template

Templates are expected to be relatively stable.

---

## Daily Workflow

1. Open app
2. View current focus block
3. Complete tasks
4. Reorder tasks when needed
5. Unfinished tasks automatically carry forward

---

# Flexibility Rules

The schedule is not rigid.

Users may:

✅ Reorder tasks

✅ Drag tasks between blocks

✅ Work on later tasks immediately

✅ Complete tasks out of sequence

A block assignment should be treated as a suggestion.

---

# Main Screens

## Today

Displays:

- Current block
- Current task
- Upcoming blocks
- Carried-over tasks
- Completed tasks

Example:

```text
Current Block

09:50 - 10:25

Documentation

[Complete]
```

Upcoming:

```text
10:40  Project Work
11:30  Administration
12:20  Research
```

---

## Planner

Used to manage:

- One-off tasks
- Task assignments
- Task ordering

Supports:

- Drag and drop
- Reordering
- Block reassignment

Example:

```text
☰ Documentation
☰ Administration
☰ Project Work
☰ Research
```

---

## Templates

Dedicated screen for:

### Monday Template

```text
09:00 Weekly Planning
09:50 Documentation
10:40 Administration
```

### Wednesday Template

```text
09:00 Administration
09:50 Research
10:40 Documentation
```

### Friday Template

```text
09:00 Weekly Review
09:50 Cleanup
10:40 Documentation
```

---

## Completed

Displays:

```text
Completed Today

✓ Weekly Planning
✓ Documentation
✓ Admin Work
```

Completed tasks are separated from active work.

---

## Settings

Contains:

- Theme
- Backup/Restore
- Reset Data
- PWA Information

---

# Current Block Indicator

Current focus block should be calculated automatically based on device time.

Example:

```text
Now

10:40 - 11:15

Research Competitors
```

Outside work hours:

```text
No Active Focus Block
```

---

# Task Completion Rules

When a task is completed:

1. Mark status as completed
2. Remove from active schedule
3. Move to Completed section
4. Save automatically

---

# Carry-Over Rules

## Rule 1

Completed tasks never carry over.

---

## Rule 2

Deleted tasks never carry over.

---

## Rule 3

Unfinished tasks always carry over.

---

## Rule 4

Carry-over order is preserved.

Example:

```text
Task A
Task B
Task C
```

Appears in the same order next workday.

---

## Rule 5

Carry-over tasks appear before template tasks.

Example:

```text
Carried Over

Research
Documentation

Today's Tasks

Administration
Review
```

---

## Rule 6

Friday carries over to Monday.

Example:

```text
Friday

Research
Documentation
```

becomes:

```text
Monday

Research
Documentation
```

---

## Rule 7

Recurring template tasks continue to regenerate normally, even when unfinished copies exist.

Example:

Monday unfinished:

```text
Documentation
```

Wednesday:

```text
Carried Over

Documentation
```

Next Monday:

```text
Documentation
```

appears again from the Monday template.

---

## Rule 8

Carry-over tasks remain the same task instance.

Only the date changes.

---

## Rule 9

Track carry-over count.

Example:

```text
Research Competitors

Carry Over Count = 3
```

---

# Missed Workday Processing

Workdays:

```text
Monday
Wednesday
Friday
```

All other days are ignored.

---

## One Missed Day

Example:

```text
Opened Monday

Skipped Wednesday

Opened Friday
```

Result:

```text
Friday

Carried Over

Research
Documentation
```

No tasks are lost.

---

## Multiple Missed Days

Example:

```text
Opened Monday

Skipped Wednesday

Skipped Friday

Opened next Monday
```

Result:

```text
Monday

Carried Over

Research
Documentation
Prepare Proposal
```

Nothing is lost.

---

## Long Absence

Example:

```text
Last Opened January

Opened April
```

The system automatically processes all missed workdays.

No unfinished task is lost.

---

## Startup Order

Application startup sequence:

```text
1. Load localStorage

2. Determine current workday

3. Process missed workdays

4. Apply carry-over logic

5. Generate today schedule

6. Render user interface
```

---

# Data Model

## WorkDay

```typescript
type WorkDay =
  | "monday"
  | "wednesday"
  | "friday";
```

---

## TaskStatus

```typescript
type TaskStatus =
  | "active"
  | "completed"
  | "deleted";
```

---

## Task

```typescript
interface Task {
  id: string;

  title: string;

  source:
    | "template"
    | "oneoff";

  workDate: string;

  originalWorkDate: string;

  blockId: string | null;

  status: TaskStatus;

  carryOverCount: number;

  createdAt: string;

  completedAt?: string;
}
```

---

## Template Task

```typescript
interface TemplateTask {
  id: string;

  day: WorkDay;

  blockId: string;

  title: string;
}
```

---

## Focus Block

```typescript
interface FocusBlock {
  id: string;

  day: WorkDay;

  start: string;

  end: string;
}
```

---

## Application State

```typescript
interface AppState {
  lastProcessedWorkDate: string | null;
}
```

---

# LocalStorage Structure

```typescript
{
  settings: {},
  templates: {},
  tasks: [],
  completedTasks: [],
  appState: {}
}
```

All changes must be saved automatically.

---

# Navigation

Bottom navigation:

```text
Today
Planner
Templates
Completed
Settings
```

---

# User Interface Requirements

The UI must be:

- Mobile-first
- Touch-friendly
- Fast
- Clean
- Minimal
- Responsive

Requirements:

✅ Large tap targets

✅ Drag-and-drop support

✅ Sticky current block card

✅ Dark mode support

✅ Offline support

---

# PWA Requirements

The application must:

✅ Be installable

✅ Work offline

✅ Include service worker

✅ Include web manifest

✅ Support standalone mode

✅ Support Android home screen installation

✅ Save data locally

---

# Version 1 Features

## Templates

✅ Monday Template

✅ Wednesday Template

✅ Friday Template

✅ Recurring Tasks

✅ Recurring Break Activities

---

## Task Management

✅ Create Task

✅ Edit Task

✅ Delete Task

✅ Drag and Drop

✅ Reorder Tasks

✅ Assign to Blocks

✅ One-Off Tasks

---

## Scheduling

✅ Automatic Schedule Generation

✅ Current Block Detection

✅ Carry-Over Processing

✅ Missed-Day Processing

---

## Completion

✅ Complete Tasks

✅ Completed Section

✅ Carry-Over Counter

---

## Technical

✅ LocalStorage

✅ Offline Support

✅ Mobile UI

✅ Installable PWA

✅ Dark Mode

---

# Version 1 Exclusions

❌ Login

❌ User Accounts

❌ Cloud Sync

❌ Calendar Integration

❌ Email Integration

❌ Notifications

❌ AI Task Suggestions

❌ Weekly Statistics

❌ Completion Percentages

❌ Time Tracking

❌ Team Collaboration

❌ Multi-User Support

---

# Success Criteria

The user only needs to:

1. Create Monday, Wednesday, and Friday templates once.
2. Optionally create one-off tasks.
3. Open the app and instantly see the current task.
4. Reorder work throughout the day.
5. Never lose unfinished work.
6. Reuse the same recurring planning structure indefinitely.
7. Have the app continue working completely offline.
