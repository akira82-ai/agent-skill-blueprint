# Part 3: Beyond Conversation—The CLI Evolution Path of Skills

> "Parameterization transforms Skills from conversation tools to CLI tools, greatly increasing flexibility"
>
> "Solidify fixed logic with code, pass variable input through parameters—the essence of parameterization is separating the variable from the invariant"
>
> "Not all Skills need parameterization; only those needing flexible input should be parameterized"

## From Conversation to Command: The Paradigm Shift Brought by Parameterization

The first two chapters discussed the essence of Skills and product thinking. But whether you treat Skills as tools or products, if you only invoke them with natural language, you're only leveraging half their capabilities.

Parameterization is a key step in Skill evolution. It upgrades Skills from "conversational interaction" to "command-based interaction," from "flexible but imprecise" to "precise and reusable." This transformation seems simple but has far-reaching implications.

## The Bias in Popular Perception

Most people's understanding of Skills stays at the "conversational interaction" level—you use natural language to tell Claude what to do, it understands and executes. In this mode, a Skill is like a packaged prompt, obtaining information through conversation each time it's invoked.

But this cognition overlooks a possibility: **Skills can receive input through parameters, just like CLI tools.**

CLI (Command Line Interface) tools are the working method most familiar to programmers. You don't say "help me copy this file to that place," you input `cp source.txt target.txt`. The advantages of this approach are precision, efficiency, and scriptability. Parameterization gives Skills these characteristics too.

## The Essence of Parameter Parsing

First, a misconception needs to be clarified: parameter parsing is not universal or automatic. Each Skill's parameter format, quantity, names, and meanings are custom-defined, requiring developers to clearly specify them in SKILL.md.

This sounds like a burden, but is actually freedom. You can design the most suitable parameter format according to the Skill's purpose. If you make a file organization tool, you can design `--source` and `--target` parameters; if you make a translation tool, you can design `--from`, `--to`, and `--text` parameters. Entirely up to you.

## Two Parameter Formats

Claude Code supports two parameter formats; you can choose according to preference.

| Format | Syntax | Example | Suitable Scenario | Advantage |
|--------|--------|---------|-------------------|-----------|
| **Linux Style** | `/skill-name --param value` | `/organize --source ~/Downloads --target ~/Documents` | Multi-parameter scenarios | Parameters clear and explicit, familiar to programmers |
| **Colon Style** | `/skill-name: param value` | `/organize: ~/Downloads ~/Documents` | Few-parameter scenarios | More concise, faster typing |

**The first is Linux style:**

```bash
/skill-name --param value
```

Example:
```bash
/organize --source ~/Downloads --target ~/Documents
```

This format is very natural for users familiar with Linux command lines, parameter names are clear and explicit, suitable for scenarios with multiple parameters.

**The second is colon style:**

```bash
/skill-name: param value
```

Example:
```bash
/organize: ~/Downloads ~/Documents
```

This format is more concise, suitable for scenarios with fewer parameters or fixed parameter order. You can choose based on the Skill's complexity and target user group.

## Implicit vs Explicit Invocation

Parameterization brings an important question: How should this Skill be invoked?

| Skill Type | Invocation Method | Reason | Example |
|------------|-------------------|--------|---------|
| **Parameterized Skill** | Explicit invocation (slash command) | Difficult to match parameters with context | `/organize --source ~/Downloads --target ~/Documents` |
| **Non-parameterized Skill** | Implicit invocation (auto-trigger) | Information can be inferred from context | Code detection Skill auto-triggers |

**Parameterized Skills are suitable for explicit invocation.** That is, users need to explicitly specify Skill name and parameters through slash commands. Why? Because implicit invocation (automatic triggering through conversation) struggles to correctly match parameters with context. When a user says in conversation "organize my files," the system can hardly know whether `~/Downloads` should correspond to `--source` or `--target`.

**Non-parameterized Skills are suitable for implicit invocation.** If a Skill doesn't need parameters, or all information can be inferred from conversation context, then it can be designed to auto-trigger. For example, a Skill specifically for detecting code bugs can be automatically invoked when a user submits a code snippet.

This distinction is important. When designing Skills, you first need to ask yourself: Does this Skill need parameters? If yes, you should expect users to invoke it through explicit slash commands.

## The Core Value of Parameterization: Separating Variable from Invariant

What is the essence of parameterization? It is **separating fixed logic from variable input**.

```
┌─────────────────────────────────────────────────┐
│  Essence of Parameterization: Separate Variable  │
│              from Invariant                      │
├─────────────────────────────────────────────────┤
│                                                 │
│   ┌─────────────────────────────────────┐       │
│   │    Fixed Logic (Business Logic)      │       │
│   │                                     │       │
│   │   1. Scan source directory, see what│       │
│   │      files are there                │       │
│   │   2. Scan target directory, see what│       │
│   │      subdirectories exist           │       │
│   │   3. Match files based on filename  │       │
│   │      and directory name             │       │
│   │   4. Move files                     │       │
│   │                                     │       │
│   │   This logic doesn't change with    │       │
│   │   different usage scenarios         │       │
│   └─────────────────────────────────────┘       │
│                                                 │
│   ┌─────────────────────────────────────┐       │
│   │      Variable Input (Parameters)     │       │
│   │                                     │       │
│   │   • Source directory: different each│       │
│   │     time                            │       │
│   │   • Target directory: different each│       │
│   │     time                            │       │
│   │                                     │       │
│   │   Passed through parameters, logic  │       │
│   │   remains unchanged                 │       │
│   └─────────────────────────────────────┘       │
│                                                 │
└─────────────────────────────────────────────────┘
```

Take the file-organizer I developed as an example. The following is an excerpt from file-organizer's SKILL.md, showing how to achieve the separation of "variable and invariant" through parameterization. This code is from the latest version of the GitHub repository `akira82-ai/file-organizer`:

```yaml
## Execution Flow

### 1. Parse Parameters
Check whether the user has provided source and target directory parameters.

### 2. Determine Source Directory (if not specified)

#### 2.1. Read Source Directory History
```bash
python3 scripts/manage_history.py read --type source
```
- Parse JSON output, get history records
- Save to variable `source_history`

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

### 3. Determine Target Directory (if not specified)

#### 3.1. Read Target Directory History
```bash
python3 scripts/manage_history.py read --type target
```
- Parse JSON output, get history records
- Save to variable `target_history`

#### 3.2. Ask User
Use `AskUserQuestion` to ask user:
- If `target_history` is not empty, add history records as options (marked "recently used")
- Provide common directory options (documents folder, desktop, etc.)
- Provide "enter new path" option

### 4. Scan Target Directory
```bash
ls -la "$target"
```
- Check if classification directories already exist
- If yes, record existing classification list for subsequent matching

### 5. Scan Source Files
```bash
ls -1 "$source"
```
- Get all files list
- Skip hidden files (starting with `.`)

### 6. AI Analysis and Generate Classifications

**If target directory has classifications**: Match files to existing classifications

**If target directory has no classifications**: AI analyzes all filenames, dynamically creates classifications
```

In this design:
- **Fixed logic**: The steps of scanning, analyzing, matching, moving are invariant
- **Variable input**: Source and target directories are passed through parameters, can differ each time

This logic doesn't change because of different usage scenarios. But each time, the source and target directories are different—this time might be organizing from download folder to documents folder, next time might be from desktop to project folder.

If the path is hardcoded in SKILL.md, code would need to be modified before each use, which is clearly unreasonable. Parameterization makes the path a variable input: business logic remains unchanged, paths are passed through parameters. This is the value of parameterization.

## Configuration Mode vs Execution Mode

Some Skills need not only parameters but also configuration. What's the difference between these two?

| Comparison Dimension | Parameters | Configuration |
|---------------------|------------|---------------|
| **Change Frequency** | May change with each invocation | Set once, used long-term |
| **Typical Examples** | Source directory, target directory, user input | API keys, local paths, usage preferences |
| **Storage Location** | Passed via command line | config.json file |
| **Purpose** | Provide flexibility | Provide convenience |

**Parameters are values that may change with each invocation.** Like source and target directories for file organization—each use might be different.

**Configuration is relatively stable settings.** Like API keys, default preferences, local paths, etc.—set once and can be used long-term.

Based on this distinction, a Skill can have two running modes:

**Configuration Mode** (triggered on first invocation): Skill requires user to provide configuration information—API key, local paths, usage preferences, etc. This information is saved to config.json, then can be used repeatedly.

**Execution Mode** (after configuration complete): Skill uses saved configuration, combines with user-passed parameters, executes specific tasks.

Not all Skills need configuration mode. Simple one-time tools might not need any configuration, but complex Skills needing to connect to external services often do. When designing, ask yourself: Does this Skill have settings that need persistent storage? If yes, consider implementing configuration mode.

## Implementation Details of Configuration Mode

The specific implementation of configuration mode is simple. When Skill executes, first check if config.json file exists in local directory:

- If yes, read configuration, enter execution mode
- If no, require user to fill in configuration, save then enter execution mode

Here's a design decision to consider: granularity of parameter validation. Should you check if configuration is "correct"?

My suggestion is: **only check if there's a value, don't check if the value is correct.**

Why? Because the definition of "correct" is complex, and may vary by person. If you require user to input an API key, you only need to confirm they input some value; as for whether this key is valid, has permissions—let them discover the problem in actual use. Over-validation instead increases user burden and also increases your maintenance cost.

## When Does Parameterization Make Sense?

The last question is: How to judge whether a Skill needs parameterization?

| Judgment Criterion | Needs Parameterization | Doesn't Need Parameterization |
|--------------------|------------------------|-------------------------------|
| **Input Requirement** | Needs flexible input | Input fixed or obtained through conversation |
| **Configuration Requirement** | Needs pre-configuration | No configuration needed |
| **Tool Type** | General utility tool | One-time task type |
| **Interaction Method** | Pure command type | Pure conversation type |

**Consider parameterization when you need flexible, variable input:**

- ✅ Need user to specify file paths or directories
- ✅ Need user to provide custom text
- ✅ Need to select one from multiple options
- ✅ Need to adjust certain parameters to control behavior

**Keep simple conversational interaction:**

- ❌ One-time tasks
- ❌ Pure conversation type, obtaining information through exchange
- ❌ Simple operations, user doesn't need to specify anything

Parameterization is a powerful tool, but it's not a necessity. Don't parameterize for the sake of parameterization. Remember the principle of product design: **enough is enough, complexity has a cost.**

---
