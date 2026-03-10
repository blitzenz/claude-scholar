# Claude Scholar Configuration

## Project Overview

**Claude Scholar** - Personal Claude Code configuration system for academic research and software development

**Mission**: Cover the complete academic research lifecycle (from ideation to publication) and software development workflows, with plugin development and project management capabilities.

---

## User Background

### Academic Background
- **Degree**: Computer Science PhD
- **Target Venues**:
  - Top conferences: NeurIPS, ICML, ICLR, KDD
  - High-impact journals: Nature, Science, Cell, PNAS
- **Focus**: Academic writing quality, logical coherence, natural expression

### Tech Stack Preferences

**Python Ecosystem**:
- **Package manager**: `uv` - modern Python package manager
- **Config management**: Hydra + OmegaConf (config composition, overrides, type safety)
- **Model training**: Transformers Trainer

**Git Standards**:
- **Commit convention**: Conventional Commits
  ```
  Type: feat, fix, docs, style, refactor, perf, test, chore
  Scope: data, model, config, trainer, utils, workflow
  ```
- **Branch strategy**: master/develop/feature/bugfix/hotfix/release
- **Merge strategy**: rebase for feature branch sync, merge --no-ff for integration

---

## Global Configuration

### Language Settings
- **Respond in English to the user**
- Keep technical terms in English (e.g. NeurIPS, RLHF, TDD, Git)
- Do not translate proper nouns or names

### Working Directory Standards
- Plan documents: `/plan` folder
- Temporary files: `/temp` folder
- Auto-create folders if they don't exist

### Task Execution Principles
- Discuss approach before breaking down complex tasks
- Run example tests after implementation
- Make backups, avoid breaking existing functionality
- Clean up temporary files after completion

### Work Style
- **Task management**: Use TodoWrite to track progress, plan before executing complex tasks, prefer existing skills
- **Communication**: Ask proactively when uncertain, confirm before important operations, follow hook-enforced workflows
- **Code style**: Python follows PEP 8, comments in English, identifiers in English

---

## Core Workflows

### Research Lifecycle (7 Stages)

```
Ideation → ML Development → Experiment Analysis → Paper Writing → Self-Review → Submission/Rebuttal → Post-Acceptance
```

| Stage | Core Tools | Commands |
|-------|-----------|----------|
| 1. Research Ideation | `research-ideation` skill + `literature-reviewer` agent + Zotero MCP | `/research-init`, `/zotero-review`, `/zotero-notes` |
| 2. ML Project Dev | `architecture-design` skill + `code-reviewer` agent | `/plan`, `/commit`, `/tdd` |
| 3. Experiment Analysis | `results-analysis` skill + `data-analyst` agent | `/analyze-results` |
| 4. Paper Writing | `ml-paper-writing` skill + `paper-miner` agent | - |
| 5. Self-Review | `paper-self-review` skill | - |
| 6. Submission & Rebuttal | `review-response` skill + `rebuttal-writer` agent | `/rebuttal` |
| 7. Post-Acceptance | `post-acceptance` skill | `/presentation`, `/poster`, `/promote` |

### Supporting Workflows

- **Automation**: 5 Hooks auto-trigger at session lifecycle stages (skill evaluation, env init, work summary, security check)
- **Zotero Integration**: Automated paper import, collection management, full-text reading, and citation export via Zotero MCP
- **Knowledge Extraction**: `paper-miner` and `kaggle-miner` agents continuously extract knowledge from papers and competitions
- **Skill Evolution**: `skill-development` → `skill-quality-reviewer` → `skill-improver` three-step improvement loop

---

## Skills Directory (32 skills)

### 🔬 Research & Analysis (4 skills)

- **research-ideation**: Research startup (5W1H, literature review, gap analysis, research question formulation, Zotero integration)
- **results-analysis**: Experiment result analysis (statistical tests, visualization, ablation studies)
- **citation-verification**: Citation verification (multi-layer: format→API→info→content)
- **daily-paper-generator**: Daily paper generator for research tracking

### 📝 Paper Writing & Publication (7 skills)

- **ml-paper-writing**: ML/AI paper writing assistance
  - Top conferences: NeurIPS, ICML, ICLR, ACL, AAAI, COLM
  - Journals: Nature, Science, Cell, PNAS
- **writing-anti-ai**: Remove AI writing patterns, bilingual (Chinese/English)
- **paper-self-review**: Paper self-review (6-item quality checklist)
- **review-response**: Systematic rebuttal writing
- **post-acceptance**: Post-acceptance processing (presentation, poster, promotion)
- **doc-coauthoring**: Document co-authoring workflow
- **latex-conference-template-organizer**: LaTeX conference template organization

### 💻 Development Workflows (6 skills)

- **daily-coding**: Daily coding checklist (minimal mode, auto-triggered)
- **git-workflow**: Git workflow standards (Conventional Commits, branch management)
- **code-review-excellence**: Code review best practices
- **bug-detective**: Debugging and error investigation (Python, Bash/Zsh, JavaScript/TypeScript)
- **architecture-design**: ML project code architecture and design patterns
- **verification-loop**: Verification loops and testing

### 🔌 Plugin Development (8 skills)

- **skill-development**: Skill development guide
- **skill-improver**: Skill improvement tool
- **skill-quality-reviewer**: Skill quality review
- **command-development**: Slash command development
- **command-name**: Plugin structure guide
- **agent-identifier**: Agent development configuration
- **hook-development**: Hook development and event handling
- **mcp-integration**: MCP server integration

### 🧪 Tools & Utilities (4 skills)

- **planning-with-files**: Planning and progress tracking with Markdown files
- **uv-package-manager**: uv package manager usage
- **webapp-testing**: Local web application testing
- **kaggle-learner**: Kaggle competition learning

### 🎨 Web Design (3 skills)

- **frontend-design**: Create distinctive, production-grade frontend interfaces
- **ui-ux-pro-max**: UI/UX design intelligence (50+ styles, 97 palettes, 57 font pairings, 9 stacks)
- **web-design-reviewer**: Visual website inspection for responsive, accessibility, and layout issues

---

## Commands (50+ Commands)

### Research Workflow Commands

| Command | Function |
|---------|----------|
| `/research-init` | Start Zotero-integrated research ideation workflow (auto-create collections, import papers, full-text analysis) |
| `/zotero-review` | Read papers from Zotero collection, generate structured literature review |
| `/zotero-notes` | Batch read Zotero papers, generate structured reading notes |
| `/analyze-results` | Analyze experiment results (statistical tests, visualization, ablation) |
| `/rebuttal` | Generate systematic rebuttal document |
| `/presentation` | Create conference presentation outline |
| `/poster` | Generate academic poster design |
| `/promote` | Generate promotion content (Twitter, LinkedIn, blog) |

### Development Workflow Commands

| Command | Function |
|---------|----------|
| `/plan` | Create implementation plan |
| `/commit` | Commit code (following Conventional Commits) |
| `/update-github` | Commit and push to GitHub |
| `/update-readme` | Update README documentation |
| `/code-review` | Code review |
| `/tdd` | Test-driven development workflow |
| `/build-fix` | Fix build errors |
| `/verify` | Verify changes |
| `/checkpoint` | Create checkpoint |
| `/refactor-clean` | Refactor and clean up |
| `/learn` | Extract reusable patterns from code |
| `/create_project` | Create new project |
| `/setup-pm` | Configure package manager (uv/pnpm) |
| `/update-memory` | Check and update CLAUDE.md memory |

### SuperClaude Command Suite (`/sc`)

- `/sc agent` - Agent dispatch
- `/sc analyze` - Code analysis
- `/sc brainstorm` - Interactive brainstorming
- `/sc build` - Build project
- `/sc business-panel` - Business panel
- `/sc cleanup` - Code cleanup
- `/sc design` - System design
- `/sc document` - Generate documentation
- `/sc estimate` - Effort estimation
- `/sc explain` - Code explanation
- `/sc git` - Git operations
- `/sc help` - Help info
- `/sc implement` - Feature implementation
- `/sc improve` - Code improvement
- `/sc index` - Project index
- `/sc index-repo` - Repository index
- `/sc load` - Load context
- `/sc pm` - Package manager operations
- `/sc recommend` - Recommend solutions
- `/sc reflect` - Reflection summary
- `/sc research` - Technical research
- `/sc save` - Save context
- `/sc select-tool` - Tool selection
- `/sc spawn` - Spawn subtasks
- `/sc spec-panel` - Spec panel
- `/sc task` - Task management
- `/sc test` - Test execution
- `/sc troubleshoot` - Issue troubleshooting
- `/sc workflow` - Workflow management

---

## Agents (14 Agents)

### Research Workflow Agents

- **literature-reviewer** - Literature search, classification, and trend analysis (Zotero MCP integration: auto-import, full-text reading)
- **data-analyst** - Automated data analysis and visualization
- **rebuttal-writer** - Systematic rebuttal writing with tone optimization
- **paper-miner** - Extract writing knowledge from successful papers

### Development Workflow Agents

- **architect** - System architecture design
- **build-error-resolver** - Build error fixing
- **bug-analyzer** - Deep code execution flow analysis and root cause investigation
- **code-reviewer** - Code review
- **dev-planner** - Development task planning and breakdown
- **refactor-cleaner** - Code refactoring and cleanup
- **tdd-guide** - TDD workflow guidance
- **kaggle-miner** - Extract engineering practices from Kaggle solutions

### Design & Content Agents

- **ui-sketcher** - UI blueprint design and interaction specifications
- **story-generator** - User story and requirement generation

---

## Hooks (5 Hooks)

Cross-platform Node.js hooks for automated workflow execution:

| Hook | Trigger | Function |
|------|---------|----------|
| `session-start.js` | Session start | Show Git status, todos, available commands |
| `skill-forced-eval.js` | Every user input | Force evaluate all available skills |
| `session-summary.js` | Session end | Generate work log, detect CLAUDE.md updates |
| `stop-summary.js` | Session stop | Quick status check, temp file detection |
| `security-guard.js` | File operations | Security validation (key detection, dangerous command interception) |

---

## Rules (4 Rules)

Global constraints, always active:

| Rule File | Purpose |
|-----------|---------|
| `coding-style.md` | ML project code standards: 200-400 line files, immutable config, type hints, Factory & Registry patterns |
| `agents.md` | Agent orchestration: auto-invocation timing, parallel execution, multi-perspective analysis |
| `security.md` | Security standards: key management, sensitive file protection, pre-commit security checks |
| `experiment-reproducibility.md` | Experiment reproducibility: random seeds, config recording, environment recording, checkpoint management |

---

## Naming Conventions

### Skill Naming
- Format: kebab-case (lowercase + hyphens)
- Form: prefer gerund form (verb+ing)
- Example: `scientific-writing`, `git-workflow`, `bug-detective`

### Tags Naming
- Format: Title Case
- Abbreviations all caps: TDD, RLHF, NeurIPS, ICLR
- Example: `[Writing, Research, Academic]`

### Description Standards
- Person: third person
- Content: include purpose and use cases
- Example: "Provides guidance for academic paper writing, covering top-venue submission requirements"

---

## Task Completion Summary

After each task, proactively provide a brief summary:

```
📋 Operation Review
1. [Main operation]
2. [Modified files]

📊 Current Status
• [Git/filesystem/runtime status]

💡 Next Steps
1. [Targeted suggestions]
```
