# Part 5: Beyond Isolation—The Capability Network Revolution of Skills

> "Skills aren't isolated tools; they can invoke Claude Code's capabilities, forming an invocation system"
>
> "From executor to planner, Skill's positioning has fundamentally changed"
>
> "Skill calling Skill turns isolated capabilities into a collaborative ecosystem"

## From Isolation to Interconnection: The Birth of the Invocation System

Previously we discussed the essence of Skills, productization, parameterization, and directory structure. These are all questions of "how to build a Skill." But if we only focus on individual Skills, we'll miss a bigger picture: **Skills can collaborate, and Skills can also invoke Claude Code's native capabilities.**

When I first discovered this, it was like opening a door to a new world. Skills were no longer isolated tools, but a capability network that could invoke each other, be orchestrated, be combined. The power of this network far exceeds the simple addition of individual Skills.

## Three Levels of Invocation Capability

Claude Code provides Skills with three levels of invocation capability, each opening new possibilities.

| Level | Invocation Capability | Core Value | Use Case |
|-------|----------------------|------------|----------|
| **Level 1** | AskUserQuestion | Dynamic interaction capability | Scenarios needing user confirmation, selection |
| **Level 2** | Task mechanism | From executor to planner | Complex task decomposition and orchestration |
| **Level 3** | Skill calls Skill | Form capability network | Collaboration between Skills |

```
┌─────────────────────────────────────────────────┐
│      Claude Code Invocation System Levels       │
├─────────────────────────────────────────────────┤
│                                                 │
│   ┌─────────────────────────────────────┐       │
│   │ Level 3: Skill calls Skill          │       │
│   │ • Capability network                │       │
│   │ • Intelligent recommendation         │       │
│   └─────────────┬───────────────────────┘       │
│                 │ Invoke                          │
│   ┌─────────────┴───────────────────────┐       │
│   │ Level 2: Task mechanism             │       │
│   │ • Task decomposition                │       │
│   │ • Parallel execution                │       │
│   └─────────────┬───────────────────────┘       │
│                 │ Trigger                         │
│   ┌─────────────┴───────────────────────┐       │
│   │ Level 1: AskUserQuestion            │       │
│   │ • Dynamic interaction               │       │
│   │ • User confirmation                 │       │
│   └─────────────────────────────────────┘       │
│                                                 │
└─────────────────────────────────────────────────┘
```

**Level 1 is AskUserQuestion**, giving Skills the capability for dynamic interaction.

**Level 2 is the Task mechanism**, transforming Skills from executors to planners.

**Level 3 is Skill calling Skill**, forming isolated capabilities into a collaborative ecosystem.

Let's look at each one.

## AskUserQuestion: From Passive Response to Active Interaction

I first discovered AskUserQuestion when observing Claude's built-in slash commands. I found these commands had something in common: they would ask users questions during execution, and the question options were dynamically generated based on context. This experience was excellent—users didn't need to provide all information at once, but were gradually guided during execution.

I thought at the time: this experience is so good, can Skills use it too?

The answer was yes. And when I first successfully called AskUserQuestion in a Skill, that excitement was indescribable. My Skill suddenly had the ability to actively communicate and interact with users—it was no longer passively receiving input, but could dynamically ask for information, confirm intent, provide options as needed during execution.

AskUserQuestion's core parameters and capabilities:

| Parameter | Description | Example |
|-----------|-------------|---------|
| **questions** | Question array, each containing title, options, etc. | `[{question: "Select language", options: [...]}]` |
| **header** | Question title/tag, max 12 characters | "Language Selection", "Target Platform" |
| **multiSelect** | Whether to allow multiple selection | true/false |
| **options** | Option array, containing label and description | `[{label: "Chinese", description: "Simplified Chinese"}]` |

### Real Case: idea-to-post's Progressive Questioning

Here's a real case from the GitHub repository `akira82-ai/idea-to-post`, showing how to use AskUserQuestion to implement progressive questioning (excerpt from SKILL.md):

```
### Questioning Flow (7-10 Rounds of Dialogue + Multi-Stage Search)

┌─────────────────────────────────────────────────┐
│  User inputs idea                                │
└────────────────┬────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────┐
│  [Initial Search] Background information        │
│  collection                                     │
└────────────────┬────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────┐
│  Rounds 1-2: Direction locking                  │
│  (mainly multiple choice)                        │
│  - Goal? Audience? Platform?                     │
└────────────────┬────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────┐
│  Rounds 3-4: Core deep dive                     │
│  (open questions)                                │
│  - What's the core viewpoint?                    │
└────────────────┬────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────┐
│  Rounds 5-6: Real cases (required)              │
│  - When was the most recent time?                │
│  - How did you feel at that moment?              │
└────────────────┬────────────────────────────────┘
                 │
                 ▼
┌─────────────────────────────────────────────────┐
│  Round 7: Emotional resonance (required)        │
│  - Most frustrated/surprised moment?             │
└────────────────┬────────────────────────────────┘
```

This flowchart shows how idea-to-post uses AskUserQuestion (multiple choice) + direct dialogue (open questions) combination to implement 7-10 rounds of progressive questioning. The first three rounds use multiple choice to quickly lock direction, later use open questions to dig deep into content.

**Core design principles of AskUserQuestion:**

1. **Multiple Choice = Skeleton**: Quickly lock direction, reduce user thinking burden
2. **Open Questions = Flesh and Blood**: Dig deep into content, get unique viewpoints and real cases
3. **Based on Previous Round's Answer**: Each round's questions are based on user's previous round's answer, natural transition

This design makes dialogue both structured (won't go off track) and flexible (can dig deep).

AskUserQuestion has three core capabilities:

**Dynamic question generation.** Generate questions to ask in real-time based on current context. For example, my file-organizer during execution first asks user "what's the source directory," then based on user's answer, asks "what's the target directory." Questions aren't pre-written, but dynamically generated based on execution flow.

The following is real code excerpted from the GitHub repository `akira82-ai/file-organizer`, showing how to use AskUserQuestion to ask for source directory:

```yaml
#### 2.2. Ask User
Use `AskUserQuestion` to ask user:
- If `source_history` is not empty, add history records as options (marked "recently used")
- Provide common directory options (download folder, desktop, etc.)
- Provide "enter new path" option

**Example options:**
```
- "/Users/xxx/Downloads" (recently used)
- "/Users/xxx/Desktop" (history)
- "Download folder" (~/Downloads)
- "Desktop" (~/Desktop)
- "Documents" (~/Documents)
- "Enter new path"
```

### 3. Determine target directory (if not specified)

#### 3.1. Read target directory history records
```bash
python3 scripts/manage_history.py read --type target
```

#### 3.2. Ask user
Use `AskUserQuestion` to ask user:
- If `target_history` is not empty, add history records as options (marked "recently used")
- Provide common directory options (documents folder, desktop, etc.)
- Provide "enter new path" option
```

This design demonstrates three practical techniques of AskUserQuestion:
1. **Use history records**: Read user's historical choices from local files, prioritize showing "recently used" options
2. **Provide shortcut options**: Common directories (~/Downloads, ~/Desktop) as fixed options
3. **Preserve flexibility**: Always provide "enter new path" option, letting users freely input

**Dynamic option generation.** Option content can also be adjusted based on scenarios. For example, I have a translation Skill that dynamically generates different options based on user's selected source and target languages. This flexibility makes interaction more natural.

**Control option position.** This is an easily overlooked but very important capability. You can control through code certain options always appearing in specific positions—like always putting "cancel" option last, or always putting "recommended option" first. This control has big impact on user experience.

I heavily use AskUserQuestion in my own Skills. idea-to-post uses it for multi-round dialogue exploration, gradually developing a vague idea into a complete article; file-organizer uses it to ask for source directory, target directory, and let user confirm classification plan; translation Skill uses it to show translation progress, letting users know how much longer to wait.

These practices made me deeply realize: **good interaction isn't one-time information collection, but a dynamic, gradually deepening communication process.**

## Task Mechanism: From Executor to Planner

If AskUserQuestion makes Skills "smarter," then the Task mechanism makes Skills "more powerful"—and this power is reflected in an unexpected dimension: Skills are no longer executors, but become planners.

How did this transformation happen?

The trigger was the news that Claude Code upgraded todo to task. When I saw this news, I had an inspiration: since task is so powerful, can Skills generate tasks, then let tasks execute themselves?

The answer is yes, it really works.

| Comparison Dimension | Traditional Mode | Task Mode |
|---------------------|------------------|-----------|
| **Skill Role** | Executor | Planner |
| **Work Method** | Execute all work itself | Exit after decomposing tasks |
| **Stability** | Any error fails everything | Independent fault tolerance per task |
| **Responsibility** | Understand+Execute+Output | Understand+Plan+Orchestrate |
| **Scalability** | Single Skill runs independently | Multi-task parallel collaboration |

**Under traditional mode**, Skills execute all work themselves. They understand user intent, execute tasks, output results. Everything is under Skill's control.

**Under new mode**, Skills are only responsible for planning and orchestration: they understand user intent, decompose into tasks, match required Skills, generate tasks, then—Skills exit. Remaining work is independently executed by tasks.

```
┌─────────────────────────────────────────────────┐
│           Task Mode Workflow                     │
├─────────────────────────────────────────────────┤
│                                                 │
│   User input                                    │
│      │                                          │
│      ▼                                          │
│   ┌─────────────────┐                           │
│   │   AutoSkill     │                           │
│   │   (Planner)     │                           │
│   │                 │                           │
│   │ • Understand    │                           │
│   │   intent        │                           │
│   │ • Decompose     │                           │
│   │   tasks         │                           │
│   │ • Match Skills  │                           │
│   └────────┬────────┘                           │
│            │ Generate Task                      │
│            ▼                                    │
│   ┌─────────────────────────────────────┐       │
│   │       Task 1, Task 2, Task 3...     │       │
│   │                                     │       │
│   │   • Independent execution            │       │
│   │   • Parallel operation               │       │
│   │   • Fault tolerance and recovery     │       │
│   └────────┬────────────────────────────┘       │
│            │                                    │
│            ▼                                    │
│      Return to user                             │
│                                                 │
└─────────────────────────────────────────────────┘
```

This transformation seems simple but has far-reaching implications.

**First is stability improvement.** Task is Claude Code's native mechanism, very robust. By comparison, when Skills execute complex workflows themselves, any link erroring can cause the entire process to fail. And the Task mechanism has perfect fault tolerance—if a task errors, users can correct it, then continue execution; subsequent tasks aren't affected, operating gradually.

**Second is responsibility clarification.** Skills focus on what they're good at: understanding intent, planning tasks, orchestrating resources. Tasks focus on what they're good at: stable execution, fault tolerance and recovery, progress tracking. This division makes the entire system more efficient.

**Finally is enhanced scalability.** When Skills generate tasks and exit, tasks can run independently, be monitored, be managed. This means one Skill can trigger multiple tasks simultaneously, they can execute in parallel, depend on each other, form complex task networks.

When I first let AutoSkill successfully generate and execute tasks, I really slammed the table and cheered. In that moment I realized: **AutoSkill is a miniature, streamlined version of an Agent.** It can understand intent, decompose tasks, match tools, orchestrate execution. Aren't these Agent's core capabilities?

### Real Case: insights-zh's Task Sub-Agent Invocation

To more concretely demonstrate the practical value of the Task mechanism, here's a real case from `insights-zh`. This skill needs to translate large amounts of HTML text blocks (usually 300+ blocks). If directly calling translation in the main session, it causes nested session problems, translation results hard to pass back to main session.

The solution is: Use the Task tool to launch independent sub-agents to execute translation tasks. The following is the execution flow from GitHub `akira82-ai/insights-zh` (excerpt):

```yaml
### Step 2: Batch Translation (Show Progress)

Use batch translation mode, display progress in real-time in main session:

1. **Start batch translation**:
   ```bash
   python3 ~/.claude/skills/insights-zh/scripts/auto_batch.py
   ```

2. **View progress and pending translation content**:
   Output example:
   ```
   [██████░░░] 50.3% (140/309)

   Pending translation batch: 15 lines
   ═══════════════════════════════════════════════════════════

   1. Conversation Insights
   2. Dashboard Overview
   3. Time Period
   4. Last 7 days
   5. Total conversations
   ...
   ```

3. **Provide translation**:
   - Translate the displayed 15 lines of content
   - One translation per line
   - Keep proper nouns and technical terms unchanged

4. **Update TSV file**:
   Use the following command to update translation:
   ```bash
   python3 ~/.claude/skills/insights-zh/scripts/auto_batch.py --update <start_idx> <trans1> <trans2> ...
   ```

5. **Repeat steps 1-4** until displaying:
   ```
   ✅ All lines translation completed!
   ```

6. **Continue to step 3**: Merge translation results
```

This design shows a clever architecture: **main session responsible for control and display, sub-session responsible for executing translation.**

Main session calls `auto_batch.py` script, script displays current progress and pending translation content. After main session provides translation, script updates TSV file. This process cycles until all content is translated. Then main session calls another script to merge translation results back into HTML.

What's the advantage of this design?

**Avoid nested session problems.** If directly calling translation skill in main session, forms nested session, translation results hard to pass. Using Task tool to launch sub-agent, sub-agent independently executes translation, results directly written to TSV file, main session just reads file.

**Progress visible and controllable.** Main session always controls translation progress, users can see real-time feedback like "translated 140/309." If a batch translation isn't satisfactory, can re-translate that batch.

**Strong fault tolerance.** If a translation sub-agent errors, it only affects the current batch. Main session can detect error, restart that batch's translation. Subsequent completed batches aren't affected.

## Background Execution: From Synchronous to Asynchronous Orchestration

The Task mechanism is powerful, but it has an implicit premise: **all execution is synchronous.** When a Skill starts a task, it either waits for the result to return, or continues executing subsequent steps itself. But if a step takes a long time—waiting for compilation to finish, waiting for user to authorize in a browser, waiting for a server to start up—synchronous waiting blocks the entire conversation, severely degrading user experience.

Claude Code provides a mechanism that breaks through the synchronous limitation: Background Commands.

### The run_in_background Mechanism

The Bash tool has a `run_in_background` parameter. By default it's `false`, meaning commands execute synchronously and block until complete. When set to `true`, the command executes asynchronously in the background, immediately returns a task_id, and the main conversation isn't blocked.

What does this mean? It means you can start a long-running operation, then continue chatting with Claude and doing other things. When that operation finishes, you'll receive a notification.

```
┌─────────────────────────────────────────────────┐
│       Synchronous vs Background Execution        │
├─────────────────────────────────────────────────┤
│                                                 │
│  Synchronous (default):                         │
│  User → Start command → [████blocking wait████] │
│                          ↓                      │
│                     Return result               │
│                                                 │
│  Background (run_in_background: true):           │
│  User → Start command → Return task_id          │
│       │                                         │
│       ├── Continue normal chat ← User can do    │
│       │   other things simultaneously           │
│       │                                         │
│       └── ... background command running ...    │
│                          ↓                      │
│              Auto receive completion notice     │
│                                                 │
└─────────────────────────────────────────────────┘
```

### Completion Notification Mechanism

When a background command finishes, the system automatically injects a notification message into the conversation. You don't need to poll or manually check—the system proactively pushes it to you.

The notification includes the command name and exit code:
- `exit code 0` means success
- Non-zero exit code means failure

After Claude sees this notification, it can decide on the next action based on the context from earlier in the conversation. This is the most interesting capability of background commands—**chained task triggering**.

### Chained Task Triggering

Imagine you instruct Claude like this: "After the background compilation finishes, automatically run tests."

The flow works like this:
1. Claude starts compilation (`run_in_background: true`), continues chatting with you
2. Compilation runs in the background while you do other things
3. Compilation completes, system injects notification: `Background command "..." completed (exit code 0)`
4. Claude sees the notification, recalls you said "run tests after compilation finishes," and automatically starts the tests

This pattern is essentially **LLM-driven event response**. It's not a traditional webhook or event listener—there's no standalone trigger mechanism. Instead, it relies on the LLM "reading" the notification message in the next conversation turn and taking action based on context.

### Real Case: Feishu OAuth Authorization

My own lark series Skills use this mechanism. Feishu OAuth authorization requires the user to complete a login operation in the browser—a classic long-running, user-involved process.

With synchronous execution, the user would be stuck waiting, the entire conversation blocked. But with background commands:
1. Skill initiates Feishu OAuth authorization, command runs in background
2. Skill prompts user: "Please open the link in your browser to complete authorization"
3. User goes to the browser while continuing to work in Claude Code
4. After authorization completes, background command detects this and exits automatically (exit code 0)
5. Claude Code receives completion notification, Skill continues subsequent flow

This experience feels very natural—the user isn't locked into a waiting state.

### Limitations

Every mechanism has its costs, and background commands are no exception. To be honest, there are several limitations to be aware of:

**Context window dependency.** If the conversation grows too long and gets compressed before the background task finishes, Claude may lose the original instruction (like "run tests after compilation"), and won't automatically trigger the next step.

**Session survival dependency.** If the Claude Code session exits, no one is left to pick up the background command's result. It's not an independent process—its lifecycle is bound to the session.

**Not a webhook.** There's no standalone event trigger, making it less reliable than traditional event-driven architectures. If Claude is busy with something else or doesn't correctly understand the notification's context, it may not take the expected action.

**Permission control.** Background commands still require user authorization before execution—they don't bypass security checks.

### Alternative Approach Comparison

Depending on different use cases, Claude Code offers multiple asynchronous/scheduled execution approaches:

| Approach | Mechanism | Persistence | Best For |
|----------|-----------|-------------|----------|
| **run_in_background** | Async execution, auto-notify | Session-scoped | Waiting for compilation, authorization, etc. |
| **&& chaining** | Sequential execution in single command | Immediate | build && test && deploy |
| **CronCreate** | Scheduled tasks (persistable) | Session / Durable | Periodic checks, recurring tasks |
| **/loop + Skill** | Periodically re-execute a Skill | Session (3-day expiry) | Monitoring, polling |
| **Hooks** | Event hooks in settings.json | Persistent | Event-triggered automation |

Selection advice is simple: **use what's sufficient.** For waiting on a short task, use `run_in_background`; for periodic execution, use `/loop` or CronCreate; for cross-session persistence, use Hooks or Durable Cron. Don't use something just because you can—choose the simplest approach that works.

### Relationship with the Three-Level Invocation System

Background execution doesn't change the three-level invocation system we discussed earlier—it adds an "asynchronous variant" at the Task level. The Task mechanism focuses on **"decomposition and orchestration"**—breaking complex tasks into smaller ones and executing them in parallel. Background execution focuses on **"waiting and responding"**—letting time-consuming tasks proceed without blocking the main flow, then automatically picking up when they finish. The two complement each other, making Skills' orchestration capabilities more complete.

## Skill Calling Skill: Implicit Invocation System

The third level is Skills calling each other. This is the most easily overlooked but most interesting capability.

| Invocation Method | Trigger | User Experience | Use Case |
|-------------------|---------|-----------------|----------|
| **Explicit Invocation** | User | Specify Skill precisely, full control | User clearly knows which Skill to use |
| **Implicit Invocation** | Skill | Auto-match Skill, seamless | User unsure which Skill to use |

**Explicit invocation** everyone is familiar with: users directly invoke a specific Skill through slash command. Like inputting `/organize`, file-organizer starts executing.

**Implicit invocation** is different: users didn't specify which Skill to invoke at all, another Skill automatically decides which Skill to invoke based on context.

```
┌─────────────────────────────────────────────────┐
│      AutoSkill Implicit Invocation Workflow     │
├─────────────────────────────────────────────────┤
│                                                 │
│   User input: "I want to write an article"      │
│      │                                          │
│      ▼                                          │
│   ┌─────────────────┐                           │
│   │   AutoSkill     │                           │
│   │                 │                           │
│   │  1. Read installed Skills list             │       │
│   │  2. Read descriptions one by one           │       │
│   │  3. Semantic match: write/写作/创作      │       │
│   │  4. Select best matching Skill             │       │
│   └────────┬────────┘                           │
│            │ Implicit invocation                │
│            ▼                                    │
│   ┌─────────────────┐                           │
│   │  idea-to-post   │                           │
│   │                 │                           │
│   │  • Start content creation flow             │       │
│   │  • Multi-round dialogue explore ideas      │       │
│   │  • Generate complete article              │       │
│   └─────────────────┘                           │
│                                                 │
└─────────────────────────────────────────────────┘
```

How does this mechanism work?

Take AutoSkill as an example. When user says "I want to write an article," AutoSkill will:
1. Read the list of installed Skills
2. Read their descriptions one by one
3. Perform semantic matching: which Skills' descriptions contain keywords like "write," "写作," "创作"?
4. Select the best matching Skill
5. Implicitly invoke it

The entire process is transparent to users. They just express a need: "write article." The system automatically finds the most suitable Skill to complete this need.

What's the value of implicit invocation?

**First is lowering usage barriers.** Users don't need to remember what Skills exist, don't need to know each Skill's name. They just need to express their needs, system automatically finds suitable tools. It's like telling an assistant "help me arrange a meeting," assistant will choose whether to use calendar or email themselves, you don't need to care.

**Second is forming a capability network.** When Skills can invoke each other, they're no longer islands, but an interconnected network. A Skill can serve as another Skill's "subroutine," multiple Skills can combine into more powerful capabilities. This composability makes the entire ecosystem's value grow exponentially.

**Finally is implementing intelligent recommendation.** The system can intelligently select the most suitable Skill based on user's usage habits, Skills' usage frequency, task matching degree. This experience of "you don't need to ask, I already know what you want" is true productization.

Of course, there's still an unsolved problem: when multiple Skills all match user needs, which should be chosen?

For example, user says "write an article," might match idea-to-post (idea to content creation), article-writer (article writing tool), blog-generator (blog generator). Which to choose?

My suggestion is: **prioritize the user's most commonly used Skill, if frequency is the same, use AskUserQuestion to let user choose.** This question is worth deep thought, because it relates to details of user experience.

## The Imagination Space of the Invocation System

When we put these three levels of invocation capability together, we'll find a bigger picture emerging.

| Capability Level | Possibilities Opened | Representative Cases |
|------------------|---------------------|---------------------|
| **AskUserQuestion** | Dynamic interaction, gradual guidance | idea-to-post multi-round dialogue |
| **Task mechanism** | Task decomposition, parallel execution | AutoSkill task orchestration |
| **Skill calling Skill** | Capability combination, intelligent recommendation | AutoSkill intelligent Skill matching |

AskUserQuestion gives Skills dynamic interaction capability, Task mechanism transforms Skills from executors to planners, Skill calling Skill forms isolated capabilities into a collaborative ecosystem. These three together evolve Skills from "tool collection" to "capability network."

```
┌─────────────────────────────────────────────────┐
│         Skills Capability Network Diagram        │
├─────────────────────────────────────────────────┤
│                                                 │
│              ┌──────────────┐                   │
│              │  AutoSkill   │                   │
│              │  (Coordinator)│                   │
│              └──────┬───────┘                   │
│                     │ Invoke                    │
│         ┌───────────┼───────────┐               │
│         ▼           ▼           ▼               │
│   ┌──────────┐ ┌──────────┐ ┌──────────┐        │
│   │  Skill A │ │  Skill B │ │  Skill C │        │
│   │          │ │          │ │          │        │
│   │ UseAskUs │ │ UseTask  │ │ CallSkill│        │
│   │ erQuestion│ │ mechanism│ │ D        │        │
│   └──────────┘ └──────────┘ └─────┬────┘        │
│                              │                  │
│                              ▼                  │
│                       ┌──────────┐              │
│                       │  Skill D │              │
│                       │          │              │
│                       │  Execute │              │
│                       └──────────┘              │
│                                                 │
│   Each Skill is a node in the capability network│
│   They invoke each other, collaborate, combine   │
│   dynamically                                   │
│                                                 │
└─────────────────────────────────────────────────┘
```

The power of this network lies in: it's not pre-designed, but dynamically combined. Each user need can trigger different Skills combinations, forming different execution paths. This flexibility and adaptability far exceeds any pre-designed workflow.

Moreover, this network is constantly growing. Each time you add a new Skill, you add a new capability node to the network. This node can be invoked by other Skills, and can also invoke other Skills. This composability makes the entire ecosystem's value grow exponentially—this isn't linear addition, but network effect.

Imagine future possibilities: when you have a capability network with hundreds of Skills, users only need to express a vague intent, and the system can automatically match the most suitable Skills combination, dynamically generate execution plan, then execute stably through the Task mechanism. This is no longer the concept of "tools," but a true "intelligent assistant."

And this is the true charm of Claude Skills.

---
