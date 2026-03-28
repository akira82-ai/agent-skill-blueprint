# Bonus: The Art of Choice—The Boundary Between Skill and Plugin

> "Skill is lightweight capability encapsulation; Plugin is heavyweight functionality packaging"
>
> "Skill is suitable for rapid iteration and personal use; Plugin is suitable for team standardization and long-term maintenance"
>
> "They're not competitive relationships, but solutions at different levels"

## The Birth of a Confusion

In this book, we've been discussing Skills—its essence, design methods, engineering practices. But when I first deeply used Claude Code, I discovered a confusing phenomenon.

When I pressed the slash key (/) to bring up the command list, I saw a long string of callable commands. Some were Skills I wrote myself, some were Plugins I installed. But in that list, they looked the same—each had a name, a description, a clickable option.

I couldn't tell which was a Skill, which was a Plugin.

This confusion sparked my thinking: since they look the same in the user interface, can both be invoked with slash commands, why have two concepts? What's the actual difference between them? Or is this just product design confusion?

After a period of exploration and use, I gradually understood: Skill and Plugin are indeed two different things, but their "blurring" in the user interface is intentional design. They solve problems at different levels, serving different usage scenarios.

This bonus chapter is to share this understanding process with you—from "can't tell apart" to "understand the difference," to "know when to use what."

## Official Definition: Standalone vs Plugins

According to Claude Code official documentation, there are two ways to add custom functionality:

| Method | Skill Name | Best For |
|--------|-----------|----------|
| **Standalone** (`.claude/` directory) | `/hello` | Personal workflows, project-specific customization, rapid experimentation |
| **Plugins** (directories with `.claude-plugin/plugin.json`) | `/plugin-name:hello` | Team sharing, community distribution, version release, cross-project reuse |

**When to use Standalone (what we call Skill):**
- You're customizing Claude Code for a single project
- Configuration is private, doesn't need sharing
- You're experimenting with skills or hooks before packaging
- You want short skill names, like `/hello` or `/deploy`

**When to use Plugins:**
- You want to share functionality with team or community
- You need to use the same skills/agents across multiple projects
- You want extended version control and simple updates
- You distribute through marketplace
- You can accept namespaced skill names, like `/my-plugin:hello` (namespace prevents conflicts between plugins)

The official recommendation is: **first use standalone configuration in `.claude/` for rapid iteration, then convert to plugin when ready to share.**

## Core Positioning: Capability Encapsulation vs Functionality Packaging

Based on official definitions, we can more clearly summarize the difference between the two:

- **Skill (Standalone) = Lightweight capability encapsulation**
- **Plugin = Heavyweight functionality packaging**

Skill is encapsulation of a specific capability—like "translate this text," "organize these files," "write this function." It focuses on single function, striving for simplicity, speed, flexibility. Stored in project's `.claude/` directory, skill names are short (like `/hello`), suitable for personal use and rapid experimentation.

Plugin is packaging of multiple functions—like "complete frontend development workflow," "team collaboration toolkit," "enterprise CI/CD process." It integrates multiple Skills, multiple agents, multiple hooks, multiple MCP configurations, forming a complete solution. Has independent directory structure, skill names have namespace prefix (like `/my-plugin:hello`), suitable for team sharing and community distribution.

## Multi-Dimensional Comparison

Let's compare Skill and Plugin across multiple dimensions.

| Comparison Dimension | Skill (Standalone) | Plugin |
|---------------------|-------------------|--------|
| **Complexity** | Simple, usually only one SKILL.md | Complex, strict directory structure and multiple components |
| **Naming** | Short names like `/hello` | Namespaced like `/my-plugin:hello` |
| **Scale** | Small-scale, single function | Large-scale, multi-function integration |
| **Sharing** | Share individual files or directories | Package as a whole, distribute through marketplace |
| **Version Management** | Usually no formal version management | Semantic Versioning |
| **Maintenance Cost** | Low, rapid iteration | High, needs careful testing |
| **Usage Barrier** | Low, individuals and small teams | High, learning curve |
| **Hot Deploy** | ✅ Supports live change detection | ❌ Needs manual `/reload-plugins` |
| **Directory Location** | Project's `.claude/skills/` | Dedicated Plugin directory |
| **Contents** | SKILL.md + optional scripts/templates/references | skills/agents/commands/hooks/MCP/LSP/settings |

**Complexity:**

Skill is simple. A Skill usually has only one SKILL.md file, possibly plus some scripts and references. Stored in `.claude/skills/` directory. You can create a Skill in minutes, perfect it in hours.

Plugin is complex. A Plugin has strict directory structure: `.claude-plugin/plugin.json` (manifest), `skills/` (Agent Skills), `agents/` (custom agents), `hooks/` (event handlers), possibly `.mcp.json` (MCP config), `.lsp.json` (LSP config), `settings.json` (default settings). Creating a Plugin usually takes days or weeks.

**Naming:**

Skill uses short names. When invoking, like `/hello`, `/deploy`, `/review`. Names are global, different projects' Skills might have same name (but independent in各自 projects).

Plugin uses namespaced names. When invoking, like `/my-plugin:hello`, `/frontend-toolkit:deploy`. Namespace prefix prevents naming conflicts between different Plugins.

**Scale:**

Skill is suitable for small-scale, single-function scenarios. If you only need to solve a specific problem—like "help me check code style"—Skill is the best choice.

Plugin is suitable for large-scale, multi-function scenarios. If you need to integrate a complete toolchain—like "frontend development from code checking to build and deployment full process"—Plugin is the better choice.

**Sharing:**

Skill can be shared individually. You can send the `.claude/` directory to a friend, or paste the SKILL.md file to others. Users only need to put the file in the correct directory to use it.

Plugin must be packaged as a whole. Distributed through marketplace, users install the entire Plugin. Various parts within Plugin cannot be installed separately.

**Version Management:**

Skill usually has no formal version management. You might use filename or folder name to distinguish versions (like `v1`, `v2`), but this isn't standard practice.

Plugin uses Semantic Versioning. In `plugin.json` define `version` field (like `1.0.0`), supports MAJOR, MINOR, PATCH upgrade rules. This makes Plugin updates more standardized.

**Maintenance Cost:**

Skill's maintenance cost is very low. Because it has single function, simple structure, modifications and updates are convenient. You can iterate rapidly, frequently release new versions.

Plugin's maintenance cost is higher. Because it has complex functionality, many dependencies, each modification needs to consider impact on other parts. You need to test more carefully, manage versions more formally.

**Usage Barrier:**

Skill's usage barrier is very low. Users only need to know the Skill's name and basic usage, can directly invoke it. Suitable for individual developers and small teams.

Plugin's usage barrier is higher. Users need to understand what functions the Plugin contains, how to configure, how to combine use. This usually requires some learning cost. Suitable for medium to large teams and enterprise deployment.

**Hot Deploy Support:**

This is an important difference, often overlooked.

**Skills support hot deploy.** Skills in directories added via `--add-dir` support **live change detection**. You can edit SKILL.md files in a session, modifications are automatically detected and applied, no need to restart Claude Code. This is very convenient for development and debugging—change a bit, test a bit, no need to repeatedly restart.

**Plugins need manual reload.** After modifying a Plugin, you need to manually run `/reload-plugins` command to reload. The system doesn't automatically detect Plugin changes. This design might be for performance considerations—Plugins are usually more complex, frequent change detection might bring overhead.

This means if you're frequently modifying during development, Skills' hot deploy feature will significantly improve your development efficiency. While Plugins are more suitable for "stable release" scenarios—install then很少 modify, after modify manually reload via `/reload-plugins`.

**Directory Location:**

Skill is stored in the project's `.claude/skills/` directory. Each project has its own independent Skills, not interfering with each other.

Plugin is stored in a dedicated Plugin directory, installed through marketplace or local path. After installation, available in all projects (unless disabled by project settings).

**Contents:**

Skill mainly contains SKILL.md (required), optional scripts/, templates/, references/, config.json.

Plugin can contain: skills/ (Agent Skills), agents/ (custom agents), commands/ (Markdown commands), hooks/ (event handlers), .mcp.json (MCP config), .lsp.json (LSP config), settings.json (default settings).

## When to Use Skill?

Skill is the default choice. If you're unsure what to use, use Skill first.

**Scenarios suitable for Skill:**

You need to quickly implement a function. From idea to usable, Skill can be done in hours.

Your functional requirements are relatively simple. No complex configuration needed, no multiple tools collaborating.

You're using in personal projects or small teams. No strict standardization needed, can tolerate some differences.

You want frequent iteration. Rapidly respond to user feedback, continuously optimize functionality.

You're exploring an idea. Not sure of final form, need to adjust while doing.

You want short skill names. `/hello` is easier to input and remember than `/my-plugin:hello`.

**Scenarios not suitable for Skill:**

You need to integrate multiple functions, and want them shared and deployed as a whole.

Your team needs strict standardized configuration. Everyone must use exactly the same toolchain.

You need version control and standardized update processes.

You need to distribute your solution through marketplace.

## When to Use Plugin?

Plugin is an advanced choice. Consider Plugin only when you find Skills can't meet your needs.

**Scenarios suitable for Plugin:**

You need to integrate a complete toolchain. Like "frontend development workflow" includes code checking, formatting, testing, building, deployment and other links.

Your team needs standardized configuration. Like all members using the same code standards, same document templates, same workflows.

Your functionality needs complex configuration. Like needing to configure multiple MCPs, set various keys and permissions, handle complex dependency relationships.

You want "one-click install, ready to use." Users don't need to configure Skills one by one, only need to install Plugin to get all functionality.

You're building a distributable solution. Like you want to provide a complete toolset for a certain industry, team, or specific use.

You need to distribute through marketplace. Official marketplace makes Plugin discovery and installation simple.

You need namespaces to avoid conflicts. When your function has same name as others' developed functions, namespace prefix can prevent conflicts.

**Scenarios not suitable for Plugin:**

You just need a simple function. Plugin is overkill for you.

You want frequent modification and iteration. Plugin's maintenance cost will overwhelm you.

Your users have varied technical backgrounds. Plugin's usage barrier might block some users.

## Migration Path: From Skill to Plugin

Official documentation clearly suggests a migration path: **first use standalone configuration in `.claude/` for rapid iteration, then convert to plugin when ready to share.**

This is a very practical suggestion. Many functions don't initially need Plugin's complexity, use Skill to quickly verify feasibility. When function matures, has sharing needs, needs standardization, then convert to Plugin.

The conversion process includes: creating Plugin directory structure, adding `plugin.json` manifest file, moving Skills to `skills/` directory, adding namespace prefix, testing and publishing.

## They're Complementary, Not Competitive

Finally, I want to emphasize one point: Skill and Plugin aren't competitive relationships, but complementary relationships.

**A Plugin can contain multiple Skills.** This is the most common combination. Plugin's `skills/` directory stores multiple Skills, they share Plugin's configuration and namespace.

**You can first use Skill to validate idea, then use Plugin to package mature solution.** This is many developers' actual path: first make a Skill to quickly verify feasibility, if successful, then integrate it into a Plugin, share with team or community as standardized solution.

**You can use both Skills and Plugins simultaneously.** You can install some common Plugins to get basic capabilities, and also create your own Skills in project's `.claude/` directory to meet personalized needs. They won't conflict, but complement each other.

## Selection Recommendations

If your team is considering using Skill or Plugin, my suggestion is:

**For individual developers and small teams:** Start with Skills. They're simple, flexible, enough. When your needs become complex, team size expands, need standardization, then consider migrating to Plugin.

**For medium to large teams:** Can consider directly using Plugin. Standardization's benefits will exceed maintenance costs. But even so, you can use Skills within Plugin to implement specific functions, maintaining modularity and flexibility.

**For open source projects:** Providing a Plugin is a good choice. Users can install with one click, get complete experience. Official marketplace makes discovery and installation simple.

**For product teams:** Plugin is the better choice. You can package a complete workflow as Plugin, deliver to customers as part of the product. This is much more professional than asking customers to configure Skills one by one.

**Naming recommendation:** Use short Skill names for personal projects (`/hello`), use namespaced Plugin names for team and community projects (`/my-plugin:hello`). This maintains personal development convenience while avoiding conflicts during team collaboration.

## In Closing

The difference between Skill and Plugin is essentially the difference between "tool" and "toolbox."

When you need a screwdriver, you only need a Skill. When you need a complete toolbox, you need a Plugin.

Don't be troubled by the choice. Start with Skill, upgrade to Plugin when needed. This is the optimal path in most cases.

---
