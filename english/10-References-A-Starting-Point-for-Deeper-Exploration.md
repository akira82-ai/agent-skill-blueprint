# Appendix: References—A Starting Point for Deeper Exploration

## A. Reference Resources

| Resource Type | Link | Description |
|---------------|------|-------------|
| **Official Documentation** | [docs.anthropic.com/claude-code](https://docs.anthropic.com/claude-code) | Complete Claude Code documentation |
| **Skills Guide** | [docs.anthropic.com/claude-code/skills](https://docs.anthropic.com/claude-code/skills) | Skills development guide |
| **Plugins Guide** | [docs.anthropic.com/claude-code/plugins](https://docs.anthropic.com/claude-code/plugins) | Plugins development guide |
| **clawhub** | [clawhub.dev](https://clawhub.dev) | Official skill marketplace |
| **awesome-claude-skills** | [github.com/ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) | Curated list, 48.4k+ stars |
| **claude-code-skills** | [github.com/daymade/claude-code-skills](https://github.com/daymade/claude-code-skills) | Professional marketplace, 43 production-grade skills |

## B. Example Skill Repositories

| Skill | Author | Description |
|-------|--------|-------------|
| **file-organizer** | Leishu (akira82) | Intelligent file organizer, typical practice of parameterized Skills |
| **AutoSkill** | Leishu (akira82) | Task orchestration and Skill management, demonstrates possibility of Skills as planners |
| **idea-to-post** | Leishu (akira82) | Idea to content creation, practice of multi-round dialogue and AskUserQuestion |
| **translation-skill** | Community contribution | Translation tool, excellent case of progress feedback and user experience |

## C. Development Tools

| Tool | Purpose | How to Get |
|------|---------|------------|
| **Claude Code CLI** | Claude Code command-line tool | [docs.anthropic.com/claude-code/quickstart](https://docs.anthropic.com/claude-code/quickstart) |
| **skillgun** | Skill development, testing, packaging | `npm install -g skillgun` |
| **firecrawl** | Web scraping, search | `npm install -g firecrawl-cli` |
| **Git** | Version control | [git-scm.com](https://git-scm.com/) |

## D. Glossary

| Term | Definition |
|------|------------|
| **Skill** | Reusable workflow instruction package, prompt-based meta-tool architecture |
| **Plugin** | Shareable functionality packaging, containing multiple Skills, agents, hooks, etc. |
| **Standalone** | Independent configuration method, stored in project's `.claude/` directory |
| **Parameterization** | Skill's ability to accept command-line parameters, like `/skill --param value` |
| **Implicit invocation** | Automatically trigger Skills through semantic matching |
| **Explicit invocation** | Explicitly invoke Skills through slash commands |
| **AskUserQuestion** | Claude Code's dynamic interaction capability |
| **Task mechanism** | Claude Code's task orchestration mechanism |
| **Semantic Versioning** | Version management specification of MAJOR.MINOR.PATCH |
| **Product thinking** | Thinking of Skills as products rather than tools |
| **Hot deploy** | Capability to take effect without restart after modification |
| **Live Change Detection** | Real-time detection of file changes and automatic application |

## E. Common Questions

**Q: What's the difference between Skill and Plugin?**

A: Skill is lightweight capability encapsulation, suitable for personal use and rapid iteration; Plugin is heavyweight functionality packaging, suitable for team sharing and standardized deployment. See the bonus chapter for detailed comparison.

**Q: How to choose parameterization format?**

A: Linux style (`--param value`) suits multi-parameter scenarios, colon style (`: param value`) suits fewer-parameter scenarios. Choose based on Skill's complexity and target user group.

**Q: When do I need Plugin?**

A: Consider Plugin when you need to integrate multiple functions, team standardized configuration, distribute through marketplace. For personal projects and rapid experimentation, Skill is enough.

**Q: What is AutoSkill?**

A: AutoSkill is a "meta-skill" that can understand user intent, decompose tasks, match Skills, generate Tasks. It demonstrates the transformation from executor to planner.

**Q: How does hot deploy work?**

A: Skills in directories added via `--add-dir` support live change detection, automatically take effect after modification. Plugins need to manually run `/reload-plugins` command.

---

**[End of Book]**

---

**Copyright Notice**: This content is original, welcome to reprint and share, please cite source.

**Author**: Leishu
**WeChat**: AIRay1015
**Publication Date**: March 28, 2026
**Version**: v1.0.0

