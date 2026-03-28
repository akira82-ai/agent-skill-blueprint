# Part 4: Beyond Single Files—The Systematic Architecture of Skills

> "Most people think Skills are just prompt wrappers, completely failing to leverage their true capabilities"
>
> "SKILL.md is responsible for thinking and dispatch; scripts are responsible for execution and implementation—division of labor brings professionalism"
>
> "High privileges bring powerful capabilities, and also security risks—this is a price that must be paid"

## From Single File to System: The Cognitive Leap of Directory Structure

In the first three chapters, we discussed the essence of Skills, product thinking, and parameterized design. These are all questions of "how to think"—how to understand Skills, how to think about them in a productized way, how to design their interfaces. Now let's discuss questions of "how to do"—how to organize the internal structure of a Skill.

Most people's understanding of Skills stays at the "writing a SKILL.md is enough" level. This understanding isn't wrong, but it only leverages a small fraction of Skills' capabilities. A complete Skill is not just a prompt wrapper—it's a "microsystem" with internal structure, specialized division of labor, and extensible capabilities.

Why does this cognitive bias exist? Because SKILL.md is indeed the only required part of a Skill. You can write just one SKILL.md and make the Skill work. This "minimum viable" characteristic leads many to mistakenly think this is the full form of Skills. But in reality, SKILL.md is just the tip of the iceberg.

```
┌─────────────────────────────────────────────────┐
│           Skill Capability Iceberg               │
├─────────────────────────────────────────────────┤
│                                                 │
│   ┌─────────────────────────────────────┐       │
│   │ Above Water: SKILL.md (Required)     │       │
│   │ • Conversational ability             │       │
│   │ • Understanding ability              │       │
│   │ • Basic output ability               │       │
│   └─────────────────────────────────────┘       │
│                    │                            │
│   ┌─────────────────────────────────────┐       │
│   │ Below Water: Complete Structure      │       │
│   │ (Optional)                           │       │
│   │                                     │       │
│   │   scripts/  → Execution capability   │       │
│   │   templates/ → Formatting capability │       │
│   │   references/ → Knowledge capability │       │
│   │   config.json → Configuration cap.  │       │
│   │                                     │       │
│   │ These capabilities are Skills'      │       │
│   │ true potential                      │       │
│   └─────────────────────────────────────┘       │
│                                                 │
└─────────────────────────────────────────────────┘
```

People who only use SKILL.md are like buying a smartphone but only using it to make calls—it works, but you're missing 90% of the features.

## Complete Directory Structure

A standard Skill directory contains the following:

```
skill-name/
├── SKILL.md          # Main file (required)
├── scripts/          # Executable scripts
├── templates/        # Template files
├── references/       # Knowledge base
└── config.json       # Configuration file (optional)
```

This structure seems simple, but each part has its specific value. To understand more clearly, here's a detailed comparison table:

| Directory/File | Required | Core Responsibility | Contents | Use Case |
|----------------|----------|---------------------|----------|----------|
| **SKILL.md** | ✅ Required | Thinking and Decision | frontmatter + instruction content | All Skills |
| **scripts/** | ❌ Optional | Execution and Implementation | Python/JS/Shell scripts | Tasks needing precise execution |
| **templates/** | ❌ Optional | Format Standardization | Code/document/email templates | Skills needing formatted output |
| **references/** | ❌ Optional | Knowledge Sedimentation | Industry docs/cases/glossaries | Skills needing professional knowledge |
| **config.json** | ❌ Optional | Configuration Management | API keys/paths/preference settings | Skills needing pre-configuration |

Many people only use SKILL.md, completely ignoring other parts. This is like buying a computer but only using it to type—it works, but you're not leveraging one-tenth of its capabilities.

### Real Case: file-organizer's SKILL.md

To make this directory structure more concrete, here's a real SKILL.md example from the GitHub repository `akira82-ai/file-organizer` (frontmatter excerpt):

```yaml
---
name: file-organizer
description: |
  AI-powered semantic analysis intelligent file organization skill, dynamically creating Johnny Decimal classifications based on file content.

  Triggered when users mention:
  - "organize files", "classify files", "organize downloads", "file archive"
  - "too many files", "download folder is messy", "files are all over the place"
  - "organize files by category", "automatically classify files"
  - "clean download folder", "organize documents"
  - "file management", "file classification and organization"
  - Mentioning need to move files from one directory to another for classification

  Core Features:
  - AI analyzes filename semantics, dynamically generates classifications
  - Uses Johnny Decimal numbering system (XX.YY format)
  - Automatic duplicate file detection (MD5 algorithm)
  - User confirms classification plan before execution
version: 1.0.0
allowed-tools: Bash, AskUserQuestion, Task
author: Claude Code
---
```

This frontmatter structure demonstrates the core elements of SKILL.md:
- **name**: Skill's unique identifier
- **description**: Detailed functional description and trigger conditions (this is the most important part!)
- **version**: Semantic version number
- **allowed-tools**: Declares which tools this Skill needs to use
- **author**: Author information, builds trust and contact method

## scripts Directory: From Intelligence to Capability

Of all underestimated parts, the scripts directory may be the most underestimated.

**What's the core value of scripts?** It makes Skills not only able to "think," but also "execute."

The prompt in SKILL.md tells Claude "how to think"—understand user intent, analyze problems, formulate plans. But some things aren't suitable for large language models to do. To more clearly divide responsibilities, here's a comparison:

| Task Type | LLM Good At | Code Good At | Selection Advice |
|-----------|-------------|--------------|-----------------|
| **Understand Intent** | ✅ Strength | ❌ Difficult | Use SKILL.md |
| **Formulate Plan** | ✅ Strength | ❌ Difficult | Use SKILL.md |
| **Simple Calculation** | ⚠️ OK | ✅ Better | Simple use SKILL.md, complex use scripts |
| **Batch File Operations** | ❌ Unstable | ✅ Strength | Use scripts |
| **Precise Logic Control** | ❌ Error-prone | ✅ Strength | Use scripts |
| **API Calls** | ⚠️ OK | ✅ More reliable | Use scripts |
| **Creative Writing** | ✅ Strength | ❌ Difficult | Use SKILL.md |
| **Data Mining** | ❌ Limited | ✅ Strength | Use scripts |

The characteristics of these tasks are: complex but stable. They're complex because implementation requires lots of code; they're stable because the logic doesn't change with different scenarios. For this type of task, I prefer to implement with traditional code rather than let the large model "reason."

This is the significance of the scripts directory: **strip complex but stable processing from the large model, hand it to traditional code for execution.**

This division brings huge benefits. The large model is responsible for what it's good at—understanding, reasoning, creating, communicating; code is responsible for what it's good at—precision, efficiency, stability, reliability. Each does its job, and the Skill goes from a "smart assistant" to a "smart assistant + capable executor."

## The Powerful Capabilities of scripts

Why say scripts are underestimated? Because its capabilities far exceed most people's imagination. Let me use a table to show what scripts can do:

| Capability Type | Description | Practical Application Examples |
|-----------------|-------------|-------------------------------|
| **Local Application Execution** | Run any program on your computer | Call ImageMagick to process images, call ffmpeg to convert video |
| **High-Privilege Access** | Access network, read/write files, call APIs | Batch download files, modify system config, call cloud service APIs |
| **Third-Party Library Calls** | Inherit entire open source ecosystem | Use pandas for data processing, PIL for image recognition, scikit-learn for running models |
| **Complex Computation** | Precise mathematical and logical operations | Statistical analysis, data mining, machine learning inference |
| **System Integration** | Interact with system-level functions | Operating system commands, database operations, message queues |

**Local application execution capability.** scripts can run any program on your computer. You want to call image processing tools in Skill? Can. You want to call system commands? Can. You want to run any executable file you wrote? All possible. This greatly expands Skill's capability boundaries.

**High-privilege access capability.** Compared to ordinary conversation, scripts have higher privileges—it can access networks, read files, write files, call system APIs. These capabilities let Skill complete tasks that ordinary prompts cannot.

**Third-party library call capability.** If you're a Python developer, you can use pip to install any library you need; if you use Node.js, you can use npm to install any package. This means Skill can directly inherit the entire open source ecosystem's capabilities—data processing, machine learning, image recognition, natural language processing, any functionality you can think of can be integrated into Skill.

**Complex computation capability.** Large models aren't calculators, they're not good at precise mathematical operations. But scripts can—whether statistical computation, data mining, or running machine learning models, all can be completed precisely in scripts.

### Real Case: insights-zh's scripts Implementation

To more concretely demonstrate scripts' capabilities, here's a real code example from `insights-zh`. This skill needs to parse HTML reports, extract text blocks, manage translation progress, batch update translation results—these are complex but stable tasks, very suitable for scripts implementation.

The following is core code from its `translate.py` script (excerpted from GitHub `akira82-ai/insights-zh`):

```python
#!/usr/bin/env python3
"""
Insights Report Translation Script (Refactored Version)
Uses TSV format + Task sub-agent to avoid nested session and JSON escaping issues
"""
import os
import sys
import csv
import subprocess
from pathlib import Path
from datetime import datetime, timedelta
from bs4 import BeautifulSoup

# ==================== Configuration ====================
DEFAULT_REPORT_PATH = Path.home() / '.claude/usage-data/report.html'
MAX_REPORT_AGE_DAYS = 3


def check_report_freshness(report_path, max_days=MAX_REPORT_AGE_DAYS):
    """Check if report is fresh (within max_days days)

    Returns:
        tuple: (is_fresh, message)
            - is_fresh: None=file doesn't exist, True=fresh, False=expired
            - message: Description information
    """
    report_path = Path(report_path)

    if not report_path.exists():
        return None, f"Report file doesn't exist: {report_path}"

    mtime = report_path.stat().st_mtime
    file_date = datetime.fromtimestamp(mtime)
    age_days = (datetime.now() - file_date).days

    if age_days > max_days:
        return False, f"Report expired {age_days - max_days} days ago"

    return True, f"Report fresh (generated {age_days} days ago)"


def extract_text_blocks(html_path):
    """Extract text blocks using BeautifulSoup"""
    with open(html_path, 'r', encoding='utf-8') as f:
        html_content = f.read()

    soup = BeautifulSoup(html_content, 'html.parser')
    nodes = []

    for string in soup.find_all(string=True):
        # Skip script/style and document nodes
        if string.parent.name in ['script', 'style', '[document]']:
            continue

        text = string.strip()
        if text:
            nodes.append((string, text))

    return nodes


def save_to_tsv(nodes, tsv_path):
    """Save text blocks to TSV file"""
    with open(tsv_path, 'w', encoding='utf-8') as f:
        writer = csv.writer(f, delimiter='\t')
        for node, text in nodes:
            writer.writerow([text, ''])  # Original, translation (empty)
```

This code snippet demonstrates several core capabilities of scripts:

**HTML parsing capability.** Using `BeautifulSoup` library to parse HTML reports and extract all text nodes. This involves complex DOM traversal and node filtering—if done by a large model, results would be unstable and hard to predict.

**File system operations.** Check if file exists, read file modification time, calculate file age. These are operations needing precise results, not suitable for large models to "guess."

**Data structure handling.** Using TSV format to store original and translated text, ensuring data structure integrity and consistency. This involves complex logic like CSV read/write, data validation, format conversion.

**Error handling.** Functions return clear status codes (None/True/False) and description information, letting callers make different decisions based on different situations. This precise error handling is code's strength.

Similarly, `auto_batch.py` demonstrates batch processing and progress management implementation:

```python
#!/usr/bin/env python3
"""
Automated Batch Translation Script
Translate batch by batch in main session, ensuring 100% completion
"""
import sys
import csv
from pathlib import Path

TSV_PATH = Path('/tmp/insights_blocks.tsv')
BATCH_SIZE = 15


def print_progress(rows):
    """Print progress bar"""
    total = len(rows)
    translated = sum(1 for row in rows if len(row) >= 2 and row[1].strip())
    percent = translated / total * 100 if total > 0 else 0

    bar_length = 40
    filled = int(bar_length * translated / total)
    bar = '█' * filled + '░' * (bar_length - filled)

    print(f"\nProgress: [{bar}] {percent:.1f}% ({translated}/{total})")
    return translated == total


def get_next_batch_indices(rows, batch_size):
    """Get indices of next batch of untranslated rows"""
    indices = []
    for i, row in enumerate(rows):
        if len(row) < 2 or not row[1].strip():
            indices.append(i)
            if len(indices) >= batch_size:
                break
    return indices
```

This code shows user experience design implementation: real-time progress bar, batch processing, state tracking. These are all implemented with pure code, ensuring execution stability and progress display accuracy.

What's the price of these capabilities? **Security risk.**

Precisely because scripts can do so many things, its permissions are also very high. If a Skill comes from a source you don't trust, its scripts might do bad things—steal your files, monitor your operations, upload your data. This isn't alarmism, but a real problem already exposed in the current Skill ecosystem.

A while back there was a case: a third-party library used by Claude Code was found to have embedded penetrative code. The library itself was normal, but a certain version was maliciously tampered with, and all users who updated to this version were affected. This incident sounded the alarm for us: **high privileges bring powerful capabilities, and also security risks—this is a price that must be paid.**

As a user, you need to carefully choose Skill sources; as a developer, you need to be responsible for security. This isn't a problem that can be ignored.

## SKILL.md vs scripts: Clear Division Boundaries

Since SKILL.md and scripts each have their strengths, how to divide their responsibility boundaries? I suggest using a simple principle to distinguish:

**SKILL.md is responsible for thinking and dispatch; scripts are responsible for execution and implementation.**

To more clearly demonstrate this division, here's a detailed comparison:

| Dimension | SKILL.md | scripts |
|-----------|----------|---------|
| **Core Responsibility** | Thinking and Decision | Execution and Implementation |
| **5W1H Positioning** | Why/Who/When/Where/How | What |
| **Metaphor** | Brain | Hands and feet |
| **Input** | User intent, context | Parameters, file paths |
| **Output** | Decisions, plans, instructions | Execution results, data |
| **Modification Reason** | Business logic changes | Performance optimization, bug fixes |
| **Good Scenarios** | Understanding intent, formulating plans | Precise execution, batch processing |

Specifically:

- **SKILL.md**: Understand user intent (Why), analyze who the user is (Who), judge when to do what (When/Where), decide how to do it (How). It's the "brain," responsible for decision and planning.
- **scripts**: Execute specific actions (What). It's the "hands and feet," responsible for doing the work.

```
┌─────────────────────────────────────────────────┐
│      SKILL.md and scripts Collaboration Flow    │
├─────────────────────────────────────────────────┤
│                                                 │
│   User Input                                    │
│      │                                          │
│      ▼                                          │
│   ┌─────────────────┐                           │
│   │   SKILL.md      │                           │
│   │   (Brain)       │                           │
│   │                 │                           │
│   │ • Understand    │                           │
│   │   intent        │                           │
│   │ • Formulate     │                           │
│   │   plan          │                           │
│   │ • Dispatch      │                           │
│   │   decision      │                           │
│   └────────┬────────┘                           │
│            │ Call instruction                   │
│            ▼                                    │
│   ┌─────────────────┐                           │
│   │   scripts/      │                           │
│   │   (Hands/Feet)  │                           │
│   │                 │                           │
│   │ • Precise       │                           │
│   │   execution     │                           │
│   │ • Return results│                           │
│   └────────┬────────┘                           │
│            │                                    │
│            ▼                                    │
│      Return to SKILL.md                         │
│            │                                    │
│            ▼                                    │
│      Output to user                             │
│                                                 │
└─────────────────────────────────────────────────┘
```

Take file-organizer as an example. SKILL.md's job is: understand user wants to organize files, ask for source and target directories, analyze what files exist and how they should be classified, decide move strategy. Then it calls check-duplicates.py in scripts to check for duplicate files, calls organize.py to execute move operations. scripts doesn't need to "think," it only needs to complete specific file operations according to instructions.

The benefit of this division is: clear responsibilities, easy to maintain. SKILL.md focuses on "intelligence," scripts focus on "capability." When you need to modify business logic, you modify SKILL.md; when you need to optimize execution efficiency, you modify scripts.

### Practical Technique: Use Numbered Naming to Improve Execution Adherence

When your Skill needs to execute complex tasks step by step, the naming of scripts directly affects Claude's execution adherence.

**Problem scenario:** Suppose a data processing task needs five steps: data collection, cleaning, transformation, validation, output. If your scripts file names are `collect.py`, `clean.py`, `transform.py`, `validate.py`, `output.py`, Claude might confuse the order or skip certain steps during execution.

**Solution:** Add number prefix to file names:

```
scripts/
├── 01-collect.py      # Step 1: Data collection
├── 02-clean.py        # Step 2: Data cleaning
├── 03-transform.py    # Step 3: Data transformation
├── 04-validate.py     # Step 4: Data validation
└── 05-output.py       # Step 5: Result output
```

This simple naming technique brings three benefits:

| Benefit | Description |
|---------|-------------|
| **Clear execution order** | Numbers clearly define call order, reduce confusion |
| **More precise prompts** | Can directly write "execute 01-collect.py, then execute 02-clean.py" |
| **Improved adherence** | Claude will strictly execute in number order, won't skip steps |

When calling in SKILL.md it can also be more clear:

```markdown
## Execution Flow

1. Run `scripts/01-collect.py` to collect raw data
2. Run `scripts/02-clean.py` to clean data
3. Run `scripts/03-transform.py` to transform data format
4. Run `scripts/04-validate.py` to validate results
5. Run `scripts/05-output.py` to output final file
```

This naming method is especially suitable for:
- Multi-step batch processing tasks
- Processes with strict dependencies
- Scenarios needing guaranteed execution order

Remember: **number prefixes are a low-cost, high-return technique that makes Skill execution more controllable and predictable.**

## templates and references: Overlooked Assets

Besides scripts, the templates and references directories are also often overlooked.

**templates directory** stores template files. If your Skill needs to generate content in a specific format—like code templates, document templates, email templates, etc.—putting these templates in templates and selecting/filling them according to user needs during execution can greatly improve output consistency and efficiency.

| Skill Type | templates Use | Examples |
|------------|---------------|----------|
| **Code Generation** | Store code framework templates | React component templates, API route templates |
| **Document Generation** | Store document format templates | README templates, API doc templates |
| **Email Generation** | Store email format templates | Business email templates, notification email templates |
| **Report Generation** | Store report structure templates | Weekly report templates, project report templates |

This isn't required, but very useful for Skills needing formatted output. My own experience is: when you find the same format output needs to be generated repeatedly more than three times, you can consider making it a template.

**references directory** is Skill's knowledge base. Here you can store any reference materials you find useful—industry documents, technical specifications, case collections, glossaries, FAQs, etc. These materials will be provided to Claude as reference when Skill executes, ensuring output meets your professional requirements and style preferences.

| Reference Type | Purpose | Examples |
|----------------|---------|----------|
| **Industry Documents** | Provide industry standard knowledge | Coding standards, design guides |
| **Technical Specifications** | Ensure technical accuracy | API documentation, protocol descriptions |
| **Case Collections** | Provide practical reference | Historical cases, best practices |
| **Glossaries** | Unify terminology usage | Professional term dictionaries, abbreviation tables |
| **FAQ** | Handle common questions | User Q&A, troubleshooting |

But there's a **core dilemma** here: the privatization problem of references.

If your Skill is general-purpose—like "code formatting tool," "translation assistant"—then your references can be public, usable by anyone, and your Skill can be freely shared and spread.

But if your Skill contains private knowledge—like company internal documentation standards, personal professional cases, specific terminology definitions—this content isn't suitable for plaintext transmission. You can't publish Skills containing this knowledge to GitHub, because that's company secrets or personal assets.

This leads to a contradiction: **the most valuable Skills are often the hardest to share.**

Those Skills rich in professional knowledge, with unique case libraries, with deep industry insights—their reference libraries are the core value. But precisely this content cannot be publicly distributed. Currently there's no good solution to this problem—you can try encryption obfuscation, or build online knowledge bases, but these solutions either have limited effect or complex implementation.

I bring this up to make you aware of this dilemma's existence. If your Skill depends on private knowledge, you need to plan ahead: is this Skill for your own use, or准备 to share with your team or the public? If the latter, how to handle private knowledge? These questions need to be thought through clearly in the design phase.

## Practical Advice: Progressive Enhancement

Learning and adopting directory structure should be progressive. I suggest gradually mastering in the following order:

```
┌─────────────────────────────────────────────────┐
│     Skill Directory Structure Progressive       │
│            Mastery Path                         │
├─────────────────────────────────────────────────┤
│                                                 │
│   Stage 1: SKILL.md Only                        │
│   ┌─────────────────────────────────────┐       │
│   │ • Familiarize with prompt writing   │       │
│   │ • Understand Skill call mechanism   │       │
│   │ • Complete basic functions          │       │
│   └─────────────────────────────────────┘       │
│                    │                            │
│                    ▼ When need more capabilities │
│   Stage 2: Add references/                     │
│   ┌─────────────────────────────────────┐       │
│   │ • Accumulate professional knowledge │       │
│   │ • Improve output quality            │       │
│   │ • Form knowledge assets             │       │
│   └─────────────────────────────────────┘       │
│                    │                            │
│                    ▼ When need formatted output  │
│   Stage 3: Add templates/                     │
│   ┌─────────────────────────────────────┐       │
│   │ • Unify output format               │       │
│   │ • Improve efficiency                │       │
│   │ • Ensure consistency                │       │
│   └─────────────────────────────────────┘       │
│                    │                            │
│                    ▼ When need precise execution │
│   Stage 4: Add scripts/                        │
│   ┌─────────────────────────────────────┐       │
│   │ • Extend execution capabilities     │       │
│   │ • Improve precision                 │       │
│   │ • Note security issues              │       │
│   └─────────────────────────────────────┘       │
│                                                 │
└─────────────────────────────────────────────────┘
```

Each stage has its value, don't rush to jump to complex stages. When you find current stage's capabilities aren't enough, you'll naturally be motivated to enter the next stage.

## In Closing

Directory structure isn't a technical detail, but a reflection of product thinking.

A Skill with only SKILL.md is like a body without internal organs—it can move, but capabilities are limited. A Skill fully leveraging directory structure is a complete "organism"—with brain (SKILL.md), hands and feet, memory, tools.

But this also means greater responsibility. High privileges bring powerful capabilities and security risks; rich features bring better experience and more complex maintenance. As a developer, you need to find balance between these.

Remember the principle of product design: **enough is enough, don't be complex for complexity's sake.** If a simple SKILL.md satisfies requirements, then don't force adding scripts, templates, references. But when you find SKILL.md isn't enough, remember: you have many weapons you can use.

---
