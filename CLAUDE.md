# Claude Scholar Configuration

## Project Overview

**Claude Scholar** - Personal Claude Code configuration system for academic research and software development

**Mission**: Cover the complete academic research lifecycle (from ideation to publication) and software development workflows, with plugin development and project management capabilities.

---

## User Background

### Academic Background
- **Degree**: Business School PhD candidate — ESG (Environmental, Social, Governance) Research
- **Approach**: Using Large Language Models (LLMs) as primary research tools, NOT developing LLMs
- **Target Venues (Journal Quality Filter)**:
  - **ABS/AJG 4* / 4**: Journal of Finance, Review of Financial Studies, Journal of Financial Economics, Management Science, Strategic Management Journal, Academy of Management Journal, American Economic Review, Quarterly Journal of Economics, Journal of Political Economy, Administrative Science Quarterly, Journal of Consumer Research, MIS Quarterly
  - **ABS/AJG 3**: Journal of Business Ethics, Business Strategy and the Environment, Journal of Accounting and Economics, Journal of Management Studies, Journal of International Business Studies, Ecological Economics, Corporate Social Responsibility and Environmental Management
  - **FT50** (all FT50 journals are ABS 3* or above, safe to use as equivalent filter): includes AMJ, MS, SMJ, JF, RFS, ASQ, etc.
  - **UTD24** (also broadly equivalent to ABS 3+): includes MS, SMJ, AMJ, JFE, etc.
  - **High-quality SSRN working papers**: Accept if from ABS 4*/4 journal authors, top-20 university affiliations (Harvard, MIT, Stanford, Chicago, Wharton, LSE, etc.), or already cited 50+ times
  - **Interdisciplinary**: Nature, Science, PNAS, Nature Sustainability, Nature Climate Change
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

### Research Lifecycle (12 Stages)

Expanded from 7 to 12 stages, incorporating multi-perspective debate, pilot validation, visual planning, and quality gate mechanisms (inspired by Sibyl Research System's dual-loop architecture).

```
1. Topic Scoping
   → 2. Literature Search & Zotero Import
   → 3. Multi-Perspective Idea Debate
   → 4. Research Design & Planning
   → 5. Pilot Validation (small sample)
   → 6. Full Data Collection & LLM Processing
   → 7. Empirical Analysis & Robustness
   → 8. Result Assessment (multi-angle)
   → 9. Visual Planning & Paper Outline
   → 10. Paper Writing (parallel sections)
   → 11. Quality Gate & Self-Review
   → 12. Submission / Rebuttal / Post-Acceptance
```

### Stage Details

**Stage 1: Topic Scoping**
- 5W1H framework: What phenomenon? Why important? Who are stakeholders? When (time scope)? Where (geography/industry)? How (preliminary method)?
- User provides a rough direction; Claude structures it into a researchable question
- Output: `plan/topic-scoping.md`

**Stage 2: Literature Search & Zotero Import**
- WebSearch across Google Scholar, SSRN, OpenAlex for ABS 3★+ papers
- Extract DOIs → batch import to Zotero via API → auto-create themed collections (Core Papers / Methods / Applications / To-Read)
- Attach Open Access PDFs via Unpaywall; record paywalled papers for user to download via school access
- Full-text analysis for core papers; extract: research question, method, data, findings, limitations
- Gap analysis: identify 2-3 concrete research opportunities
- Output: `context/literature-review.md`, `context/gap-analysis.md`, `references.bib`

**Stage 3: Multi-Perspective Idea Debate** *(from Sibyl)*
- Evaluate the proposed idea from 4 perspectives (adapted from Sibyl's 6-agent debate for business research):
  - **Innovator**: What's novel about this? Cross-domain analogies? New measurement approaches?
  - **Skeptic**: What could go wrong? Endogeneity concerns? Data limitations? Identification challenges?
  - **Pragmatist**: Is this feasible with available data/methods? Timeline realistic? Sample size sufficient?
  - **Theorist**: What's the theoretical mechanism? Which theory anchors this? Are hypotheses well-grounded?
- Synthesize perspectives into a refined proposal with clear contribution statement
- Output: `idea/perspectives/`, `idea/proposal.md`, `idea/hypotheses.md`

**Stage 4: Research Design & Planning**
- Define variables: dependent, independent, controls (with sources: WRDS, CSMAR, Bloomberg, etc.)
- Specify empirical model (OLS, panel FE, DiD, IV, PSM)
- Plan endogeneity strategy
- If using LLM for text analysis: design prompt, plan human validation (Cohen's kappa target ≥ 0.7)
- Plan all tables and figures upfront (regression tables, descriptive stats, robustness checks, mechanism tests)
- Output: `plan/methodology.md`, `plan/variable-definitions.md`, `plan/visual-plan.md`

**Stage 5: Pilot Validation** *(from Sibyl)*
- Before full-scale analysis, test on a small sample (~100-500 firms, 1-2 years):
  - Does the LLM prompt produce sensible ESG scores/classifications?
  - Does the main regression show the expected sign?
  - Are there obvious data quality issues?
- GO/NO-GO decision: if pilot shows promise → proceed; if not → revise approach or pivot
- Output: `exp/pilot-results.md`

**Stage 6: Full Data Collection & LLM Processing**
- Run LLM pipeline on full dataset (batch processing with error handling)
- Record all prompts in appendix-ready format
- Validate LLM outputs: inter-coder reliability, prompt sensitivity analysis, model robustness (GPT-4 vs Claude)
- Output: processed dataset, `data/llm-validation.md`

**Stage 7: Empirical Analysis & Robustness**
- Main regression with clustered standard errors
- At least 3 robustness checks: alternative variable definitions, sub-samples, alternative estimators
- Endogeneity tests: IV regression / DiD / PSM / Oster (2019) coefficient stability
- Mechanism tests (mediation analysis)
- Cross-sectional heterogeneity analysis
- Economic magnitude discussion
- Output: `exp/results/`, regression tables, figures

**Stage 8: Result Assessment (Multi-Angle)** *(from Sibyl)*
- Assess results from multiple perspectives before writing:
  - **Optimist**: What are the strengths? What extensions are possible?
  - **Skeptic**: Are there statistical concerns? Missing robustness checks? Alternative explanations?
  - **Strategist**: Which results go in the main paper vs appendix? What's the narrative?
- PROCEED (write paper) or PIVOT (try alternative approach from Stage 3 alternatives)
- Output: `idea/result-assessment.md`

**Stage 9: Visual Planning & Paper Outline** *(from Sibyl)*
- Design paper outline with **explicit Figure & Table Plan** before writing any section:
  - Table 1: Descriptive statistics
  - Table 2: Main regression results
  - Table 3-5: Robustness checks
  - Table 6: Mechanism tests
  - Figure 1: Research framework diagram
  - Figure 2: Key trend visualization
- Each visual element has: description, data source, generation method, target section
- Output: `writing/outline.md` (with visual plan), `writing/figures/`

**Stage 10: Paper Writing (Parallel Sections)**
- Write sections referencing the outline and visual plan:
  - Introduction (hook → RQ → what we do → findings → contributions)
  - Literature Review & Hypotheses (theory → 3 streams → hypotheses)
  - Data & Methodology (data sources → sample → LLM method → variables → model)
  - Results (main → robustness → endogeneity → mechanisms → heterogeneity)
  - Conclusion (summary → implications → limitations → future)
- Each section cross-references planned figures/tables
- Output: `writing/sections/`, `writing/paper.md`

**Stage 11: Quality Gate & Self-Review** *(from Sibyl)*
- Score the paper on 6 dimensions (1-10 each):
  1. **Contribution clarity**: Is the "so what" compelling?
  2. **Empirical rigor**: Endogeneity addressed? Robustness sufficient?
  3. **LLM methodology**: Validated against human coders? Prompt disclosed?
  4. **Writing quality**: Clear, concise, logical flow?
  5. **Journal fit**: Matches target journal norms and recent publications?
  6. **Citation completeness**: All claims supported? Key papers cited?
- If average < 7/10 → identify weakest dimension → iterate on that section
- If average ≥ 7/10 → ready for submission
- Anti-AI check: remove AI writing patterns (e.g., "it's important to note", "delve into")
- Output: `writing/quality-review.md`, `writing/improvement-plan.md`

**Stage 12: Submission / Rebuttal / Post-Acceptance**
- Format paper for target journal (LaTeX or Word template)
- Generate cover letter
- After reviews: systematic rebuttal writing (point-by-point response with evidence)
- After acceptance: presentation slides, poster, promotion content
- Output: final submission package

### Zotero Integration

- **API Key**: configured (Library ID: 19903524)
- **Capabilities**: DOI-based import, collection management, PDF attachment (Open Access via Unpaywall), full-text reading, BibTeX export
- **Paywalled papers**: User logs into school account via Chrome → Claude operates browser to search and download

### File Structure Convention

```
{project}/
├── plan/
│   ├── topic-scoping.md
│   ├── methodology.md
│   ├── variable-definitions.md
│   └── visual-plan.md
├── context/
│   ├── literature-review.md
│   └── gap-analysis.md
├── idea/
│   ├── perspectives/          # Multi-perspective debate outputs
│   ├── proposal.md            # Refined research proposal
│   ├── hypotheses.md          # Testable hypotheses
│   └── result-assessment.md   # Post-analysis assessment
├── data/
│   └── llm-validation.md      # LLM output validation report
├── exp/
│   ├── pilot-results.md
│   └── results/               # Full regression outputs
├── writing/
│   ├── outline.md             # Paper outline WITH visual plan
│   ├── sections/              # Individual section drafts
│   ├── figures/               # Figure generation scripts & outputs
│   ├── paper.md               # Integrated draft
│   ├── quality-review.md      # Quality gate scoring
│   └── improvement-plan.md    # Iteration improvements
└── references.bib
```

---

## Skills Directory

### 🔬 Research Ideation & Literature

- **research-ideation**: Research startup (5W1H, literature review, gap analysis, research question formulation, Zotero integration)
- **literature-review** *(K-Dense)*: Systematic literature review across PubMed, Google Scholar, Semantic Scholar; PRISMA flow diagrams; citation verification
- **research-lookup** *(K-Dense)*: Deep research synthesis on any topic with citations
- **openalex-database** *(K-Dense)*: Query 240M+ scholarly works; bibliometric analysis; citation tracking; identify influential papers
- **hypothesis-generation** *(K-Dense)*: Formulate testable hypotheses from observations; experimental design
- **scientific-brainstorming** *(K-Dense)*: Creative ideation, interdisciplinary connections, assumption reversal
- **scientific-critical-thinking** *(K-Dense)*: Methodology critique, bias detection, evidence quality grading (GRADE, Cochrane ROB)
- **daily-paper-generator**: Daily paper generator for ESG/finance/management research tracking

### 📝 Paper Writing & Publication

- **esg-paper-writing**: ESG/business/finance paper writing (JF, MS, SMJ, JBE, BSE; APA/AMA/Chicago; regression tables; LLM validation templates)
- **scientific-writing** *(K-Dense)*: IMRAD structure, section-specific writing strategies, reporting guidelines (STROBE, PRISMA)
- **writing-anti-ai**: Remove AI writing patterns, bilingual (Chinese/English)
- **paper-self-review**: Paper self-review (6-item quality checklist)
- **peer-review** *(K-Dense)*: Peer review writing, manuscript evaluation, reviewer feedback
- **review-response**: Systematic rebuttal writing
- **post-acceptance**: Post-acceptance processing (presentation, poster, promotion)
- **doc-coauthoring**: Document co-authoring workflow
- **citation-management** *(K-Dense)*: Citation formatting, reference management across APA/Chicago/AMA styles
- **citation-verification**: Citation verification (multi-layer: format→API→info→content)
- **research-grants** *(K-Dense)*: Grant writing (NSFC, NSF, NIH styles); budget preparation; broader impacts
- **latex-conference-template-organizer**: LaTeX template organization

### 📊 Data & Empirical Analysis

- **results-analysis**: Empirical analysis (OLS, panel FE, DiD, event study, IV, PSM; regression tables with clustered SE; robustness checks; LLM output validation)
- **statistical-analysis** *(K-Dense)*: Hypothesis testing, assumption checking, effect sizes, power analysis, APA-format reporting, Bayesian methods
- **exploratory-data-analysis** *(K-Dense)*: Auto-detect 200+ file formats; data quality assessment; preprocessing recommendations
- **market-research-reports** *(K-Dense)*: 50+ page consulting-quality reports; Porter's Five Forces, PESTLE, SWOT, BCG; market sizing
- **edgartools** *(K-Dense)*: SEC filings analysis (10-K, 10-Q, 8-K); ESG disclosure extraction from regulatory filings
- **fred-economic-data** *(K-Dense)*: 800,000+ economic time series; macroeconomic context for ESG research

### 🎨 Visualization & Presentation

- **scientific-visualization** *(K-Dense)*: Publication-quality figures; colorblind-friendly palettes; vector formats
- **scientific-schematics** *(K-Dense)*: AI-generated research diagrams and flowcharts
- **matplotlib** *(K-Dense)*: Python matplotlib for publication-quality charts
- **scientific-slides** *(K-Dense)*: Academic presentation slides
- **markdown-mermaid-writing** *(K-Dense)*: Text-based diagrams (flowcharts, mindmaps, Gantt charts)
- **generate-image** *(K-Dense)*: AI image generation for research illustrations

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
- **hook-development**: Hook development and event handling
- **mcp-integration**: MCP server integration

### 🧪 Tools & Utilities

- **planning-with-files**: Planning and progress tracking with Markdown files
- **uv-package-manager**: uv package manager usage
- **pdf** *(K-Dense)*: PDF processing (extract, merge, split, OCR)
- **docx** *(K-Dense)*: Word document processing
- **pptx** *(K-Dense)*: PowerPoint processing
- **xlsx** *(K-Dense)*: Excel/spreadsheet processing

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
