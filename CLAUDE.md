# Claude Scholar Configuration

## Project Overview

**Claude Scholar** - Personal Claude Code configuration system for academic research and software development

**Mission**: Cover the complete academic research lifecycle (from ideation to publication) and software development workflows, with plugin development and project management capabilities.

---

## User Background

### Academic Background
- **Field**: Business School — ESG (Environmental, Social, Governance) Research
- **Approach**: Using Large Language Models (LLMs) as primary research tools, not developing LLMs
- **Target Venues**:
  - Top business/finance/management journals: Journal of Finance, Review of Financial Studies, Journal of Financial Economics, Management Science, Strategic Management Journal, Academy of Management Journal, Journal of Business Ethics, Business Strategy and the Environment, Journal of Accounting and Economics
  - High-impact interdisciplinary journals: Nature, Science, PNAS
  - ESG/sustainability-focused outlets: Journal of Sustainable Finance, Corporate Social Responsibility and Environmental Management, Ecological Economics
- **Focus**: Empirical research using LLMs for text analysis, classification, scoring, and measurement; academic writing quality, logical coherence, natural expression

### Research Specialization
- **ESG Topics**: Corporate sustainability disclosure, ESG scoring, greenwashing detection, climate-related financial risk, social responsibility reporting, board governance, stakeholder engagement, corporate governance
- **LLM Applications in ESG Research**:
  - NLP-based ESG text analysis (annual reports, CSR reports, earnings calls, proxy statements)
  - Automated ESG scoring and classification with LLMs
  - Sentiment and tone analysis of sustainability disclosures
  - Greenwashing detection using language models
  - Cross-country ESG comparison via multilingual LLMs
  - Information extraction from unstructured ESG documents
- **Empirical Methods**: Panel data regression, event studies, difference-in-differences, text-as-data approaches, propensity score matching
- **Data Sources**: Bloomberg ESG, MSCI ESG, Refinitiv/LSEG, WRDS, CSMAR, Wind, corporate sustainability reports, SEC EDGAR, Web of Science

### Tech Stack Preferences

**Python Ecosystem**:
- **Package manager**: `uv` - modern Python package manager
- **Data analysis**: pandas, numpy, statsmodels, linearmodels
- **NLP/LLM**: transformers, openai, anthropic, langchain, sentence-transformers
- **Visualization**: matplotlib, seaborn, plotly

**Git Standards**:
- **Commit convention**: Conventional Commits
  ```
  Type: feat, fix, docs, style, refactor, perf, test, chore
  Scope: data, model, analysis, config, utils, workflow
  ```
- **Branch strategy**: master/develop/feature/bugfix/hotfix/release
- **Merge strategy**: rebase for feature branch sync, merge --no-ff for integration

---

## Global Configuration

### Language Settings
- **Respond in Chinese (Simplified) to the user by default**
- Keep technical terms in English (e.g. ESG, LLM, NLP, API, Git, WRDS, OLS, DiD)
- Do not translate proper nouns, journal names, or method names

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
Ideation → Data Collection & LLM Processing → Empirical Analysis → Paper Writing → Self-Review → Submission/Rebuttal → Post-Acceptance
```

| Stage | Core Tools | Commands |
|-------|-----------|----------|
| 1. Research Ideation | `research-ideation` skill + `literature-reviewer` agent + Zotero MCP | `/research-init`, `/zotero-review`, `/zotero-notes` |
| 2. LLM-Based Data Processing | `architecture-design` skill + `code-reviewer` agent | `/plan`, `/commit`, `/tdd` |
| 3. Empirical Analysis | `results-analysis` skill + `data-analyst` agent | `/analyze-results` |
| 4. Paper Writing | `esg-paper-writing` skill + `paper-miner` agent | - |
| 5. Self-Review | `paper-self-review` skill | - |
| 6. Submission & Rebuttal | `review-response` skill + `rebuttal-writer` agent | `/rebuttal` |
| 7. Post-Acceptance | `post-acceptance` skill | `/presentation`, `/poster`, `/promote` |

### Supporting Workflows

- **Automation**: 5 Hooks auto-trigger at session lifecycle stages (skill evaluation, env init, work summary, security check)
- **Zotero Integration**: Automated paper import, collection management, full-text reading, and citation export via Zotero MCP
- **Knowledge Extraction**: `paper-miner` agent continuously extracts writing knowledge from top business/ESG papers
- **Skill Evolution**: `skill-development` → `skill-quality-reviewer` → `skill-improver` three-step improvement loop

---

## Skills Directory

### 🔬 Research & Analysis

- **research-ideation**: Research startup (5W1H, literature review, gap analysis, research question formulation, Zotero integration)
- **results-analysis**: Empirical analysis results (regression tables, significance tests, robustness checks, visualization)
- **citation-verification**: Citation verification (multi-layer: format→API→info→content)
- **daily-paper-generator**: Daily paper generator for ESG/finance/management research tracking

### 📝 Paper Writing & Publication

- **esg-paper-writing**: ESG/business/finance paper writing assistance
  - Target journals: Journal of Finance, Management Science, Strategic Management Journal, Journal of Business Ethics, Business Strategy and the Environment
  - Empirical business writing conventions, APA/AMA/Chicago style
- **writing-anti-ai**: Remove AI writing patterns, bilingual (Chinese/English)
- **paper-self-review**: Paper self-review (6-item quality checklist)
- **review-response**: Systematic rebuttal writing
- **post-acceptance**: Post-acceptance processing (presentation, poster, promotion)
- **doc-coauthoring**: Document co-authoring workflow
- **latex-conference-template-organizer**: LaTeX template organization

### 💻 Development Workflows

- **daily-coding**: Daily coding checklist (minimal mode, auto-triggered)
- **git-workflow**: Git workflow standards (Conventional Commits, branch management)
- **code-review-excellence**: Code review best practices
- **bug-detective**: Debugging and error investigation (Python, Bash/Zsh, JavaScript/TypeScript)
- **architecture-design**: Research project code architecture and design patterns
- **verification-loop**: Verification loops and testing

### 🔌 Plugin Development

- **skill-development**: Skill development guide
- **skill-improver**: Skill improvement tool
- **skill-quality-reviewer**: Skill quality review
- **command-development**: Slash command development
- **command-name**: Plugin structure guide
- **agent-identifier**: Agent development configuration
- **hook-development**: Hook development and event handling
- **mcp-integration**: MCP server integration

### 🧪 Tools & Utilities

- **planning-with-files**: Planning and progress tracking with Markdown files
- **uv-package-manager**: uv package manager usage
- **webapp-testing**: Local web application testing

### 🎨 Web Design

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
| `/analyze-results` | Analyze empirical results (regression tables, robustness checks, visualization) |
| `/rebuttal` | Generate systematic rebuttal document |
| `/presentation` | Create conference/seminar presentation outline |
| `/poster` | Generate academic poster design |
| `/promote` | Generate promotion content (Twitter/X, LinkedIn, blog) |

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
- `/sc analyze` - Code/data analysis
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

- **literature-reviewer** - Literature search, classification, and trend analysis for ESG/finance/management (Zotero MCP integration: auto-import, full-text reading)
- **data-analyst** - Automated empirical data analysis and visualization (panel data, regression, text-as-data)
- **rebuttal-writer** - Systematic rebuttal writing with tone optimization
- **paper-miner** - Extract writing knowledge from successful ESG/business/finance papers

### Development Workflow Agents

- **architect** - Research project code architecture design
- **build-error-resolver** - Build error fixing
- **bug-analyzer** - Deep code execution flow analysis and root cause investigation
- **code-reviewer** - Code review
- **dev-planner** - Development task planning and breakdown
- **refactor-cleaner** - Code refactoring and cleanup
- **tdd-guide** - TDD workflow guidance

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
| `coding-style.md` | Research project code standards: 200-400 line files, immutable config, type hints |
| `agents.md` | Agent orchestration: auto-invocation timing, parallel execution, multi-perspective analysis |
| `security.md` | Security standards: key management, sensitive file protection, pre-commit security checks |
| `experiment-reproducibility.md` | Analysis reproducibility: random seeds, config recording, environment recording, checkpoint management |

---

## Naming Conventions

### Skill Naming
- Format: kebab-case (lowercase + hyphens)
- Form: prefer gerund form (verb+ing)
- Example: `scientific-writing`, `git-workflow`, `bug-detective`

### Tags Naming
- Format: Title Case
- Abbreviations all caps: ESG, LLM, NLP, APA
- Example: `[Writing, Research, ESG, Academic]`

### Description Standards
- Person: third person
- Content: include purpose and use cases
- Example: "Provides guidance for ESG academic paper writing, covering top business journal submission requirements"

---

## Task Completion Summary

After each task, proactively provide a brief summary (in Chinese):

```
📋 操作回顾
1. [主要操作]
2. [修改的文件]

📊 当前状态
• [Git/文件系统/运行状态]

💡 下一步
1. [具体建议]
```
