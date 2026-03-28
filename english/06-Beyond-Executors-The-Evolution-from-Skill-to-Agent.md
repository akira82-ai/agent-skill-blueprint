# Part 6: Beyond Executors—The Evolution from Skill to Agent

> "AutoSkill is a miniature, streamlined version of an Agent"
>
> "From executor to planner, Skill's positioning has fundamentally changed"
>
> "Dynamic task decomposition, intelligent matching, automatic installation, Task orchestration—this is an unprecedented attempt"

## The Birth of a Meta-Skill

In the first five chapters, we discussed the essence of Skills, product thinking, parameterized design, directory structure practice, and invocation systems. These content build a complete cognitive framework: understanding what a Skill is, how to think about it in a productized way, how to design its interfaces, how to organize its internal structure, how to make it invoke Claude Code's capabilities.

But all these discussions revolve around "individual Skills." We assume each Skill is an independent tool, each solving a specific problem. If we ask: can there be a "Skill that manages other Skills"? Can there be a Skill that doesn't do any specific work itself, only responsible for understanding and decomposing user needs, then finding suitable Skills to complete them?

This is where AutoSkill comes from.

**AutoSkill is a "meta-skill"**—it doesn't directly complete any tasks, but is responsible for managing other Skills. It understands user intent, decomposes tasks, matches Skills, generates Tasks, then exits. Remaining work is completed by the Skills and Tasks it scheduled.

When I first got AutoSkill running, I really slammed the table and cheered. In that moment I realized: **this is a miniature, streamlined version of an Agent.** It can understand, plan, match, orchestrate. Aren't these Agent's core capabilities?

Moreover, I dare say: **I haven't seen a second person use it this way.** This isn't bragging, but stating a fact. AutoSkill represents an unprecedented attempt, opening new possibilities for Skills.

**Note**: AutoSkill is currently a proof-of-concept project and practical case. Unlike the `file-organizer` and `idea-to-post` shown in previous chapters, AutoSkill may not yet be publicly released on GitHub (as of this book's writing, the `akira82-ai/AutoSkill` repository doesn't exist). This chapter will focus on explaining AutoSkill's design philosophy and workflow, rather than specific source code implementation.

## AutoSkill's Complete Workflow

AutoSkill's workflow can be decomposed into the following steps:

| Step | Action | Description | Output |
|------|--------|-------------|--------|
| **1** | User inputs task | Natural language describes need | "Collect from three data sources, merge, send" |
| **2** | Intent understanding | Executability over accuracy | Understood intent |
| **3** | Task decomposition | Decompose into independently executable steps | Subtask list |
| **4** | Scan Skills | Dynamically scan installed Skills | Skills list |
| **5** | Match Skills | Semantic match each subtask | Skill-subtask mapping |
| **6** | Auto-install | Automatically download missing Skills | Complete Skills coverage |
| **7** | Generate Tasks | Generate Task for each subtask | Tasks list |
| **8** | AutoSkill exits | Planning complete, exit execution | Tasks start independent execution |
| **9** | Tasks execute | Independent, fault-tolerant, traceable | Final result |

**Step 1: User inputs task.**

Users use natural language to describe what they want to complete. For example "I want to collect data from three different data sources, then merge and process locally, finally send to some system." This description may be vague, incomplete, but that's okay.

**Step 2: Intent understanding.**

AutoSkill uses a simple prompt to understand what the user wants to do. Here's an important design decision: I don't pursue "accuracy" of intent understanding, because intent understanding is extremely complex, no system can guarantee fully understanding user intent.

I pursue "executability." Even if intent understanding has deviation, as long as subsequent execution is done through third-party Skills, users can discover deviation and correct it. Executability is more important than accuracy, because users can correct errors through feedback, but cannot correct an unexecutable system.

**Step 3: Task decomposition.**

AutoSkill decomposes user needs into concrete steps. For example, the above example might be decomposed into:
1. Connect to system A to collect data
2. Perform data cleaning locally
3. Connect to system B for processing
4. Send results to system C

These steps form a workflow, each step can be independently executed.

**Step 4: Scan installed Skills.**

AutoSkill will scan Claude Code's dedicated directory to see what Skills users have installed. This list is dynamic—users can install new Skills anytime, AutoSkill rescans each time it executes.

**Step 5: Match Skill for each subtask.**

This is one of AutoSkill's core capabilities. For each subtask, AutoSkill will read installed Skills' descriptions one by one, performing semantic matching. For example, if subtask is "collect data," AutoSkill will look for Skills whose descriptions contain keywords like "collect," "crawl," "fetch," etc.

Matching results have three possibilities:
- Exactly one Skill matches
- Multiple Skills match
- No Skill matches

**Step 6: Auto-download and install.**

If a subtask has no matching Skill, AutoSkill will try to automatically download. Here's an important security consideration: I only download from skills.sh, and through standard npm commands. Auto-downloading third-party Skills has security risks, so must limit download sources, only get from trusted channels.

**Step 7: Generate Tasks.**

When all subtasks have matched Skills, AutoSkill will generate a Task for each subtask, then feed these Tasks into Claude Code's Task mechanism.

**Step 8: AutoSkill exits.**

This is AutoSkill's biggest difference from traditional Skills. Traditional Skills execute until task completion, but AutoSkill exits after generating Tasks. Remaining work is independently executed by Tasks.

**Step 9: Tasks start independent execution.**

Tasks execute step by step in the order AutoSkill designed. If a Task errors, users can correct it, then continue execution. Subsequent Tasks aren't affected. This fault tolerance is the Task mechanism's advantage, and also the reason AutoSkill chose Task rather than executing itself.

```
┌─────────────────────────────────────────────────┐
│      AutoSkill Complete Workflow Diagram         │
├─────────────────────────────────────────────────┤
│                                                 │
│   User input: "Collect from three data sources, │
│   merge, send"                                  │
│      │                                          │
│      ▼                                          │
│   ┌─────────────────────────────────────┐       │
│   │   1. Intent understanding            │       │
│   │   2. Task decomposition → Subtasks   │       │
│   └───────────────┬─────────────────────┘       │
│                   │                             │
│                   ▼                             │
│   ┌─────────────────────────────────────┐       │
│   │   3. Scan installed Skills          │       │
│   │   4. Semantic match each subtask    │       │
│   └───────────────┬─────────────────────┘       │
│                   │                             │
│      ┌────────────┼────────────┐                │
│      ▼            ▼            ▼                │
│   ┌────────┐ ┌────────┐ ┌────────┐             │
│   │Has match│ │Multiple │ │No match│             │
│   │        │ │matches  │ │        │             │
│   │Direct  │ │Freq.   │ │Auto    │             │
│   │use     │ │select  │ │install │             │
│   └───┬────┘ └───┬────┘ └───┬────┘             │
│       │           │           │                 │
│       └───────────┴───────────┘                 │
│                   │                             │
│                   ▼                             │
│   ┌─────────────────────────────────────┐       │
│   │   5. Generate Tasks                 │       │
│   │   6. AutoSkill exits                │       │
│   └───────────────┬─────────────────────┘       │
│                   │                             │
│                   ▼                             │
│   ┌─────────────────────────────────────┐       │
│   │   7. Tasks execute independently    │       │
│   │   • Fault tolerance and recovery    │       │
│   │   • Traceable                       │       │
│   │   • Gradual completion              │       │
│   └─────────────────────────────────────┘       │
│                                                 │
└─────────────────────────────────────────────────┘
```

## An Unsolved Challenge: How to Choose When Multiple Skills Match?

AutoSkill's design has many technical difficulties, but the biggest challenge is: when multiple Skills all match user needs, which should be chosen?

For example: user says "write an article." AutoSkill might match three Skills:
- idea-to-post: description is "idea to content creation"
- article-writer: description is "article writing tool"
- blog-generator: description is "blog generator"

Which to choose?

This question seems simple, but actually very complex. I've thought about several possible solutions:

| Solution | Description | Advantage | Disadvantage |
|----------|-------------|-----------|--------------|
| **Relevance scoring** | Calculate similarity, pick highest score | Automated, no user intervention | Scoring algorithm hard to design accurately |
| **Usage frequency** | Prioritize most commonly used Skill | Matches user habits | Fails on first use, when frequency equal |
| **Let user choose** | Confirm with AskUserQuestion | Most reliable, accurate | Increases operational burden |
| **Combine use** | Call multiple Skills for comparison | Users can compare | High cost, users may not want to see |
| **Frequency+selection** | Prioritize frequency, ask if first use | Balance automation and accuracy | Need to record user history |

**Solution 1: Relevance scoring.** Calculate each Skill's description's similarity to user intent, pick highest score. This approach's difficulty lies in how to design scoring algorithm—simple keyword matching isn't accurate enough, complex semantic matching is hard to implement.

**Solution 2: Usage frequency.** Prioritize user's most commonly used Skill. The logic is: user's most commonly used Skill is likely user's most satisfied. But the problem is: what to do on first use? What if two Skills have same usage frequency?

**Solution 3: Let user choose.** Use AskUserQuestion to let user confirm. This solution is most reliable, but increases user's operational burden. And if users need to choose every time, AutoSkill's "automatic" feature is discounted.

**Solution 4: Combine use.** Simultaneously call multiple Skills, let users compare results. This approach's problem is cost—calling multiple Skills needs more time and resources, and users may not want to see multiple results.

My current inclination is: **usage frequency + AskUserQuestion.** Prioritize user's most commonly used Skill; if first use or same frequency, use AskUserQuestion to let user choose once, then remember user's choice, won't ask next time.

This question is worth deep thought, because it relates to AutoSkill's user experience. If you have better ideas, welcome to tell me.

## Why Use Tasks Instead of Skill Executing Itself?

A common question is: why must AutoSkill generate Tasks then exit? Why not let AutoSkill execute all work itself?

The answer is simple: **complex workflow fault tolerance.**

If a Skill needs to execute a complex workflow—like the "collect from three data sources, process, send" process mentioned above, it needs to cross scenarios, cross business, cross domains. Any link erroring, the entire process might fail. Users don't know where it errored, don't know how to correct.

But if Tasks execute, the situation is completely different:
- Each Task is independent, after error users can correct it, then continue execution
- Subsequent Tasks aren't affected, can operate gradually
- Task execution process is recorded, users can trace what happened at each step

This fault tolerance is the advantage of Claude Code's Task mechanism, and also the reason AutoSkill chose Task.

More importantly, this design lets AutoSkill focus on what it's good at: understanding intent, decomposing tasks, matching Skills, orchestrating Tasks. And leave execution to Tasks. This division makes the entire system clearer, more reliable.

## AutoSkill's Unique Value

Finally, I want to talk about AutoSkill's unique value—why say it's "an unprecedented attempt"?

| Unique Value | Traditional Way | AutoSkill Way | Agent Feature Correspondence |
|--------------|-----------------|---------------|------------------------------|
| **Dynamic task decomposition** | Predefined workflow, hardcoded steps | Dynamically generate based on user input | Understanding and planning capability |
| **Intelligent Skill matching** | Fixed call to predefined tools | Semantic match, auto-select | Tool selection capability |
| **Auto-install Skill** | Manual install, manual config | Discover missing, auto-download | Self-extension capability |
| **Task orchestration** | Skill executes to the end | Generate then exit, Task executes independently | Planning and execution separation |

**First, dynamic task decomposition.** Traditional automation tools all have predefined workflows, each step is hardcoded. But AutoSkill dynamically generates workflows based on user input. Each time user's needs differ, generated workflow differs. This flexibility is unprecedented.

**Second, intelligent Skill matching.** AutoSkill doesn't simply call predefined Skills, but based on subtask content, intelligently matches the most suitable Skill. Matching is based on semantic understanding, not fixed rules. This "understand need → find tool" capability is one of Agent's core features.

**Third, auto-install Skill.** When AutoSkill finds a missing Skill, it will automatically download and install. This gives AutoSkill "self-extension" capability—it can not only use existing tools, but also acquire new tools. Of course, for security reasons, currently only downloads from skills.sh, but this direction has huge imagination space.

**Fourth, Task orchestration.** AutoSkill exits after generating Tasks, letting Tasks execute independently. This "generate and forget" mode makes AutoSkill a pure "planner" and "orchestrator," not "executor." This positioning change is fundamental.

```
┌─────────────────────────────────────────────────┐
│       AutoSkill's Positioning Transition        │
├─────────────────────────────────────────────────┤
│                                                 │
│   Traditional Skill's positioning:              │
│   ┌─────────────────────────────────────┐       │
│   │                                     │       │
│   │   ┌─────────┐    ┌─────────┐       │       │
│   │   │Understand│ →  │Execute  │       │       │
│   │   │intent   │    │task     │       │       │
│   │   └─────────┘    └─────────┘       │       │
│   │                                     │       │
│   │   Both planner and executor          │       │
│   └─────────────────────────────────────┘       │
│                                                 │
│               → Transform →                     │
│                                                 │
│   AutoSkill's positioning:                      │
│   ┌─────────────────────────────────────┐       │
│   │                                     │       │
│   │   ┌─────────┐    ┌─────────┐       │       │
│   │   │Understand│ →  │Generate │       │       │
│   │   │intent   │    │Task     │       │       │
│   │   │Decompose│    │Exit     │       │       │
│   │   │task     │    │execute  │       │       │
│   │   └─────────┘    └─────────┘       │       │
│   │         │                          │       │
│   │         ▼                          │       │
│   │   ┌─────────────────────────────┐  │       │
│   │   │     Tasks Execute            │  │       │
│   │   │     Independently            │  │       │
│   │   └─────────────────────────────┘  │       │
│   │                                     │       │
│   │   Pure planner and orchestrator      │       │
│   └─────────────────────────────────────┘       │
│                                                 │
└─────────────────────────────────────────────────┘
```

These four characteristics together make AutoSkill a true "meta-skill"—it manages other Skills, but itself does no specific work. This design appears for the first time in Claude Skills ecosystem, at least I haven't seen a second person use it this way.

## In Closing

AutoSkill is an experimental project. It still has many imperfections—like the selection problem when multiple Skills match, like intent understanding accuracy, like auto-download security. But its value isn't in being perfect, but in opening a door.

Behind this door is the possibility of Skills evolving from "tools" to "Agents." AutoSkill proves: a Skill can understand intent, decompose tasks, match tools, orchestrate execution. Aren't these Agent's core capabilities?

If after reading this chapter, you start thinking about how to give your Skills these capabilities too, then AutoSkill's mission is complete.

---
