# Part 2: Beyond Tools—The Productization Leap of Skills

> "Tools are for their inventors; products are for end users"
>
> "Don't ask 'can it be used,' ask 'do users enjoy using it'"
>
> "Technical thinking asks 'can it be implemented'; product thinking asks 'can users use it'"

This is the most important chapter in this book, and the core idea I want to convey.

If you only read one part of this book, I recommend it be this one. Because all the content that follows—parameterized design, directory structure practice, invocation systems, task orchestration—can be seen as concrete implementations of "product thinking" at different levels. Once you understand productization, you understand the essence of Claude Skills.

## Redefining Skill: From Tool to Product

Most people's understanding of Skills stays at the "tool" level. This is natural—after all, it seems like a way to add new capabilities to Claude, like a plugin, an extension, an auxiliary tool. Under this cognition, people think a Skill is just a packaged prompt—write SKILL.md, and if it works, that's enough.

But this cognition has a fatal flaw: it treats Skills as technical artifacts, not products.

**What's the difference between a tool and a product?** Tools are for their inventors, or for people in the same technical circle; products are for end users, who may know nothing about technology and only need it to "work, work well, and be enjoyable to use." Tools only need to "solve problems"; products need to "solve problems beautifully." Tools are "done when written"; products are "continuously evolving organisms."

To more clearly demonstrate this difference, here's a comparison:

| Dimension | Tool | Product |
|-----------|------|---------|
| **Target Users** | Inventor themselves, or same technical circle | End users, may not know technology |
| **Core Requirement** | Works if it can be used | Must be enjoyable to use |
| **Completion Standard** | Can solve problems | Solves problems beautifully |
| **Lifecycle** | Done when written | Continuously evolving organism |
| **Design Focus** | Feature implementation | User experience |

When you treat Skills as products, your entire way of thinking changes.

### Encapsulation: Hide Complexity, Present Simplicity

The most important characteristic of a product is encapsulation. Users don't need to know how it's implemented, they only need to know how to use it. Just like when you use an app on your phone, you don't think about what programming language was used, what database architecture, what encryption algorithm—you only care whether it can solve your problem.

Skills should be the same. When users invoke your Skill, they shouldn't need to understand internal working principles. They don't need to know you used a complex script to calculate file MD5 values to detect duplicates, nor do they need to know you put hundreds of lines of professional knowledge documents in the references directory. They only need to provide necessary input and get expected output.

This encapsulation is not just technical, but cognitive. An excellent product hides complexity, letting users achieve goals in the simplest way. When I designed the file-organizer Skill, I deeply felt this.

The following is an excerpt from file-organizer's SKILL.md, showing how good encapsulation lets users provide minimal input. This code is from the GitHub repository `akira82-ai/file-organizer`, the latest version battle-tested:

```yaml
---
name: file-organizer
description: |
  AI-powered semantic analysis intelligent file organization skill, dynamically creating Johnny Decimal classifications based on file content.

  Triggered when users mention:
  - "organize files", "classify files", "organize downloads", "file archive"
  - "too many files", "download folder is messy", "files are all over the place"
  - "organize files by category", "automatically classify files"

  Core Features:
  - AI analyzes filename semantics, dynamically generates classifications
  - Uses Johnny Decimal numbering system (XX.YY format)
  - Automatic duplicate file detection (MD5 algorithm)
  - User confirms classification plan before execution
version: 1.0.0
allowed-tools: Bash, AskUserQuestion, Task
author: Claude Code
---

# File Organizer Skill

## Startup Banner

When the skill starts, first display the following welcome message to the user:

```
═══════════════════════════════════════════════════════════════
                  ▌ Intelligent File Organizer ▐
               AI-Driven File Classification System
═══════════════════════════════════════════════════════════════
  Leishu │ WeChat: AIRay1015 │ github.com/akira82-ai
  ───────────────────────────────────────────────────────────
  📁 Johnny Decimal Intelligent Classification System
  🤖 AI Semantic Analysis, Dynamic Classification Generation
  🔍 Built-in Duplicate File Detection (MD5)
  ✨ Preview and Confirm Before Execution, Safe and Reliable
═══════════════════════════════════════════════════════════════

Skill started...
```

Note the encapsulation of this design: users don't need to understand the specific implementation of the classification algorithm, don't need to know how AI analyzes filenames, don't need to manually specify where each file should go. Users only need to provide source and target directories, and the rest—classification, matching, moving—is automatically handled by the Skill. The description field also contains a list of trigger words, allowing the system to automatically recognize when to invoke this Skill—this is also part of encapsulation.

### Another Case: insights-zh Brand Identity

Brand identity is reflected not only in banners but also throughout the skill's design consistency. The following startup banner from `insights-zh` shows how to establish a clear brand image:

```
═══════════════════════════════════════════════════════════════
▌ Insights Chinese Translation ▐
Claude Code Report Localization Tool
═══════════════════════════════════════════════════════════════
Leishu │ WeChat: AIRay1015 │ github.com/akira82-ai
────────────────────────────────────────────────────────────
📄 TSV Format Text Extraction, Avoid JSON Escaping Issues
🤖 Task Sub-Agent Translation, Avoid Nested Session Errors
🎨 Preserve Complete HTML Structure, CSS Styles, JS Code
✨ Automatically Check Report Age, Smart Update Prompts
🌐 Translate User-Visible Text, Preserve Technical Terms
═══════════════════════════════════════════════════════════════

Skill started...
```

This banner design demonstrates three core elements of product thinking:

**Unified Visual Style.** All banners use the same border style, same layout structure, same emoji usage rules. This consistency lets users recognize at a glance "this is Leishu's skill."

**Clear Value Communication.** Each line conveys a specific value point—"TSV format" solves technical problems, "Task sub-agent" solves architectural problems, "automatic check" solves user experience problems. Users don't need to read documentation to quickly understand what this skill can do.

**Professional Brand Identity.** Author information, contact method, GitHub repository address—all complete. This isn't to show off, but to build trust—users know who made this skill, who to contact when problems arise, where to look for updates.

### Independence: Clear Boundaries, Distributable

A product must be able to exist independently. It has clear boundaries—knows what to do and what not to do; has clear inputs and outputs—knows what to receive from the outside and what to provide to the outside; has its own lifecycle—can be tested, iterated, distributed.

Skill independence is reflected at multiple levels. First is functional boundary: a Skill should only do one thing, and do it well. Not try to be a "universal tool." Second is standardization of inputs and outputs: what users need to provide, what they can expect to receive—these should be clear. Finally is distributability: your Skill shouldn't only run on your own machine, but should be installable on other people's Claude Code and work properly.

This independence makes Skills entities that can be "traded." You can share it on GitHub, others can download and use it; you can publish it on clawhub, strangers can discover and install it; you can even build communities for it, collect feedback, continuously iterate. This is completely consistent with the development path of a software product.

### Value Proposition: What Problem to Solve, Who to Serve, Why You

Every successful product has a clear value proposition: What problem does it solve? Who does it serve? Why should users choose it over other solutions?

Skills also need to think about this problem. Don't say "it's a tool, it works if it can be used," but ask yourself: What is the core pain point this Skill solves? Who are its target users? What specific needs do they have? Compared to other solutions—whether other Skills or traditional software—what is my unique value?

Take the translation Skill I developed as an example. There are many translation tools on the market, but my Skill focuses on one scenario: translating large amounts of scattered content. Its value proposition is "turn translation, a repetitive task, into a one-click operation, and provide progress feedback so users know how much longer to wait." This value proposition is clear: serve users who need to translate large amounts of content, solve the "waiting anxiety" pain point, provide progress visualization as a unique feature.

### Knowledge Sedimentation: Sustainable Evolution Containing Private Knowledge

This is a point I particularly want to emphasize. The value of many Skills lies not in "what they do," but in "what they know."

The reason my translation Skill works well is not because its prompt is written particularly well, but because I put a large amount of accumulated professional terminology, translation habits, and format specifications in the references directory. This knowledge is sedimented from my years of work, it's privatized and valuable. When the Skill is invoked, this knowledge is loaded in, guiding Claude to generate translation results that meet my requirements.

This is the value of "knowledge sedimentation." Your Skill can become a carrier of your professional knowledge—it not only executes tasks, but also thinks like you, handles details like you, makes judgments like you. This transforms the Skill from a "tool" into a "your digital avatar."

Even more beautiful, this "digital avatar" can continuously evolve. Each use generates new experience, new cases, new best practices, which can be integrated back into the Skill, making it increasingly powerful. This self-evolution capability is a unique value under product thinking.

---

To facilitate memory, I've summarized the four core elements of products as follows:

| Product Element | Core Meaning | Reflection in Skills |
|-----------------|--------------|---------------------|
| **Encapsulation** | Hide complexity, present simplicity | Users only need input, don't need to understand internal implementation |
| **Independence** | Clear boundaries, distributable | Clear functional boundaries, can be independently tested, iterated, distributed |
| **Value Proposition** | What to solve, who to serve, why you | Clear pain point positioning, target users, unique value |
| **Knowledge Sedimentation** | Sustainable evolution containing private knowledge | references directory transforms professional knowledge into inheritable assets |

```
┌─────────────────────────────────────────────────┐
│              Skill as Product                    │
├─────────────────────────────────────────────────┤
│                                                  |
│   ┌─────────┐  ┌─────────┐  ┌─────────┐         │
│   │Encapsu- │  │Inde-    │  │Value    │         │
│   │lation   │  │pendence │  │Proposi- │         │
│   │         │  │         │  │tion     │         │
│   │Hide     │  │Clear    │  │Unique   │         │
│   │Complex  │  │Bound-   │  │Value    │         │
│   └─────────┘  └─────────┘  └─────────┘         │
│                                                  |
│   ┌───────────────────────────────────────┐     │
│   │  Knowledge Sedimentation (Sustainable │     │
│   │           Evolution Capability)       │     │
│   └───────────────────────────────────────┘     │
│                                                  |
└─────────────────────────────────────────────────┘
```

## Four Dimensions of Productization Management

If managing Skills as products, what dimensions need attention? I've summarized four core dimensions: brand identity, user experience, version management, naming conventions. These four dimensions form the skeleton of productization management.

### Brand Identity: Building Trust and Recognition

You may have noticed that each of my Skills has a banner, saying "Leishu | WeChat: AIRay1015." This isn't narcissism, but well-considered product logic.

**Build Trust.** When users use a Skill, they're actually trusting the Skill's author—trusting it can correctly complete tasks, trusting it won't do harm, trusting someone can be found if problems arise. Adding personal brand information tells users: "I made this, I'm responsible for it." This transparency quickly builds trust, especially when the Skill ecosystem is still early and users are wary of unfamiliar Skills.

**Increase Recognition.** There may be many similar Skills on clawhub—like three different "file organization" tools. How do users choose? Those with clear brand identity are easier to remember and spread. When users see somewhere "Leishu's file-organizer works well," next time they can recognize this Skill.

**Establish Connection.** Adding contact information isn't to harass users, but to establish normal feedback channels. Users can find you when they encounter problems, you can notify users when you have new versions—this bidirectional connection enables the product to continuously evolve.

Of course, brand identity doesn't equal self-promotion. It should be professional, restrained, and serve user value. A simple signature, a clear contact method, a concise product description—enough.

### User Experience: The Overlooked "Last Mile"

A common problem with technical products is: powerful features, terrible experience. Developers spend lots of energy on "making it work," but ignore "making it enjoyable to use."

Product thinking requires us to think in reverse: **Don't ask "can it be used," ask "do users enjoy using it."**

I encountered a real scenario when using the translation Skill: need to translate over three hundred scattered pieces of content, each piece not long but huge in quantity. I invoked the Skill, then waited. One minute passed, two minutes passed, three minutes passed—I had no idea how it was progressing, wasn't even sure if it was stuck. That waiting anxiety was terrible.

Later I improved it: after translating each piece, display progress once. Now users see "Translating 45/300...", "Translating 46/300...", clearly knowing how much longer to wait. This small improvement changed the experience from "anxious waiting" to "controllable process"—huge difference.

User experience isn't an ethereal concept, it's reflected in every detail: Are input prompts clear? Are error messages friendly? Is there progress feedback? Is there confirmation after completion? These details added together determine whether users are willing to use your Skill long-term.

### Version Management: Profound Lessons After Potholes

Version management may be the most familiar "productization practice" for technical developers, but precisely here, I've stepped in quite a few potholes.

Early on, I wrote Skills as "done when written," changing whatever came to mind, with no version awareness at all. Until one day, I released a Skill update, changed parameter format, then suddenly received a bunch of feedback: "My Skill doesn't work anymore!" Turns out many people were using the old version, my update broke their usage.

That lesson was profound. I started introducing Semantic Versioning.

**Version Format:** `MAJOR.MINOR.PATCH`

| Version Type | Example | Trigger Condition | User Impact |
|--------------|---------|-------------------|-------------|
| **MAJOR** | 1.0.0 → 2.0.0 | Incompatible changes | Breaking update, need to adjust usage |
| **MINOR** | 1.0.0 → 1.1.0 | New features, backward compatible | Normal upgrade, no adjustment needed |
| **PATCH** | 1.0.0 → 1.0.1 | Bug fixes, backward compatible | Safe upgrade, recommend update |

**Skill-specific upgrade scenarios:**

| Version Type | Trigger Condition |
|--------------|-------------------|
| **MAJOR** | Parameter format change, major output format change, dependent tool version requirement change, removing existing features |
| **MINOR** | Adding optional parameters, adding feature modules, performance optimization, user experience improvements |
| **PATCH** | Bug fixes, copy optimization, compatibility improvements |

This specification comes from traditional software engineering, but applies equally to Skills. The benefits it brings are huge: users can tell at a glance "will this update affect me," developers have clear principles guiding "when to upgrade which number." Don't underestimate this specification, it's the foundation for products to continuously evolve and users to continuously trust.

### Naming Conventions: Making Your Skill Discoverable

The last dimension is naming. A good name should let users: quickly understand what it does, remember it, find it when needed.

The naming convention I adopt is: `authorname-skillname`. Like `akira82-file-organizer`. This convention has several benefits: first, avoid naming conflicts—many people may make file organization tools, but adding author prefix distinguishes them; second, establish brand association—users see Skills starting with `akira82-`, know they're my work, quality assured; third, facilitate search—users searching "file-organizer" can find all similar Skills, then choose based on author trust.

Of course, this isn't the only solution. You can also use functional category prefixes (like `dev-`, `writing-`), or more creative names. But whatever solution you use, remember: **the name is the user's first contact with the product, it deserves serious treatment.**

## From Technical Thinking to Product Thinking: A Necessary Cognitive Leap

After writing all this, the core is actually only one: **treat Skills as products, not tools.**

This cognitive leap doesn't happen automatically, it requires deliberate practice. My own transformation happened in an instant: when I shared a Skill I wrote with a friend to use, then watched him ask confused "how do I use it?" In that moment I suddenly realized: what I found smooth and taken for granted, for others might be completely incomprehensible.

```
┌──────────────┐                    ┌──────────────┐
│ Technical    │     Transform      │  Product     │
│  Thinking    │  ────────────→     │  Thinking    │
│              │                     │              │
└──────────────┘                    └──────────────┘
       │                                  │
   Focus:                              Focus:
   • Feature                       • User experience
   implementation                 • Usage barrier
   • Code efficiency               • First experience
   • Any bugs?
       │                                  │
  Core question:                     Core question:
  "Can it be made?"                  "Can users use it?"
```

**The fundamental difference between technical thinking and product thinking is: technical thinking asks "can it be implemented," product thinking asks "can users use it."**

To more clearly demonstrate this difference, here's a comparison:

| Thinking Dimension | Technical Thinking | Product Thinking |
|-------------------|-------------------|------------------|
| **Core Question** | Can it be implemented? | Can users use it? |
| **Focus** | Prompt precision, algorithm efficiency, feature completeness | Usage scenarios, usage barriers, error feedback, first experience |
| **Positioning** | Foundation of product | The product itself |
| **Success Standard** | Features work | Users satisfied |

Under technical thinking, you care about: is this prompt precise enough? Is this script's algorithm efficient enough? Does this feature have bugs? These are of course important, but they're the foundation of the product, not the product itself.

Under product thinking, you care about: in what scenarios do users need to use it? What confusion will they encounter? How do I lower usage barriers? When errors occur, how to provide friendly feedback? How to ensure users succeed on first use? These are the questions that determine product life or death.

There's no shortcut to this transformation. The only way is: **don't just develop skills for yourself, develop for others.** Share your Skills, let real users use them, collect their feedback, and you'll discover: those designs you took for granted might be precisely the obstacles blocking users; those details you thought insignificant might be precisely the experience users care about most.

## Data-Driven: Making Product Iteration Evidence-Based

The last productization practice is: let data speak.

I mentioned doing data statistics—using scripts scripts to record startup counts and invocation situations, or using a dedicated skill-usage skill to analyze usage. What's the use of this data?

**Guide product iteration.** You have two Skills, don't know which to prioritize optimizing? Look at data—which is invoked more frequently, optimize that one. You added a new feature to a Skill, don't know if users like it? Look at data—is this feature being used? How many times? Users' actual behavior is more honest than any questionnaire survey.

**Discover usage blind spots.** You think users will use Skills the way you designed, but data may tell you: what they actually use most is some feature you didn't pay attention to at all. This insight lets you re-understand user needs, adjust product direction.

**Establish feedback loop.** Products aren't "done when released," they're a continuously evolving process. Data is the fuel of this process—it tells you where current problems are, what improvement directions are, whether iteration is effective. Product management without data is fumbling in the dark; with data, you can target precisely.

---

To facilitate memory, I've summarized the value of data-driven as follows:

| Value | Description | Implementation |
|-------|-------------|----------------|
| **Guide Product Iteration** | Know which to prioritize optimizing | Compare invocation frequency, identify high-priority features |
| **Discover Usage Blind Spots** | Discover real usage patterns | Analyze differences between actual usage and expected design |
| **Establish Feedback Loop** | Continuous evolution based on evidence | Use data to verify iteration effects, adjust direction |

Of course, data statistics itself is a sensitive topic—how to protect user privacy? How to store data? These issues need careful handling. But the general direction is clear: **the next stage of productization will definitely be data-driven refined operations.**

---

To facilitate memory and review, I've summarized the four dimensions of productization management as follows:

| Management Dimension | Core Purpose | Practice | Your Case |
|---------------------|--------------|----------|-----------|
| **Brand Identity** | Build trust and recognition | banner + contact info | Leishu \| WeChat: AIRay1015 |
| **User Experience** | Improve usage satisfaction | Progress feedback + completion messages | Translation progress display |
| **Version Management** | Standardize iteration and release | Semantic version control | version: 1.0.0 |
| **Naming Conventions** | Facilitate discovery and recognition | Author name + skill name | akira82-file-organizer |

```
┌─────────────────────────────────────────────────┐
│     Skill Productization Management Framework    │
│            (Four Dimensions)                     │
├─────────────────────────────────────────────────┤
│                                                 |
│   ┌──────────────┐      ┌──────────────┐      │
│   │   Brand      │      │   User       │      │
│   │  Identity    │      │  Experience  │      │
│   │              │      │              │      │
│   │  • banner    │      │  • Progress  │      │
│   │  • contact   │      │    feedback  │      │
│   └──────────────┘      └──────────────┘      │
│                                                 |
│   ┌──────────────┐      ┌──────────────┐      │
│   │   Version    │      │   Naming     │      │
│   │  Management  │      │ Conventions  │      │
│   │              │      │              │      │
│   │  • Semantic  │      │  • Author    │      │
│   │    version   │      │    prefix    │      │
│   │  • Git Tags  │      │  • Clear     │      │
│   │              │      │    descrip.  │      │
│   └──────────────┘      └──────────────┘      │
│                                                 |
└─────────────────────────────────────────────────┘
```

## In Closing

Product thinking isn't a one-time transformation, but a continuous state. Every time you develop a new Skill, every time you optimize existing features, every time you handle user feedback, you're practicing product thinking.

There's no end to this road. But I can guarantee: once you start thinking about Skills in a productized way, you'll discover a whole new world. You're no longer "the person who writes prompts," but "a product manager"; you're no longer "making tools," but "designing experiences."

This is the true charm of Claude Skills.

---
