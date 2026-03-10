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

**Workflow order follows Sibyl's literature-first principle**: read the field before forming opinions, so ideas are grounded in real gaps rather than imagined ones.

```
Phase I — Ideation
  1. Literature Survey & Zotero Import
     → 2. Research Question & Gap Crystallization
     → 3. Multi-Perspective Idea Stress-Test

Phase II — Design & Validation
     → 4. Research Design & Pre-Analysis Plan
     → 5. Pilot Study (GO / NO-GO gate)

Phase III — Execution
     → 6. Data Assembly & LLM Processing
     → 7. Empirical Analysis & Robustness

Phase IV — Writing & Publication
     → 8. Result Interpretation (multi-angle)
     → 9. Paper Outline & Visual Plan
     → 10. Paper Writing (section by section)
     → 11. Quality Gate & Revision
     → 12. Submission → Review → Acceptance
```

### Stage Details

---

#### Phase I — Ideation

**Stage 1: Literature Survey & Zotero Import**
> *Entry: a rough topic area or research interest. Exit: documented field landscape + 2-3 identified gaps.*
- Search Google Scholar, SSRN, OpenAlex for ABS 3★+ papers on the topic; cast wide first, then refine
- Extract DOIs → batch import to Zotero via MCP → auto-create collections: `Core Papers / Methods / Applications / To-Read`
- Attach Open Access PDFs via Unpaywall; log paywalled papers for user to retrieve via school library (Chrome)
- Full-text analysis of 10-20 core papers: extract RQ, theory, method, data, key findings, limitations
- **Gap Analysis**: synthesize into 2-3 concrete, actionable research opportunities with justification
- Output: `context/literature-review.md`, `context/gap-analysis.md`, `references.bib`

**Stage 2: Research Question & Gap Crystallization**
> *Entry: gap-analysis.md. Exit: one confirmed RQ that user has approved and Claude has stress-tested.*
- Use 5W1H framework to convert the most promising gap into a precise research question:
  - **What** phenomenon / outcome? **Why** does it matter (theory + practice)? **Who** are the stakeholders?
  - **When** (time window)? **Where** (country / industry / firm type)? **How** (preliminary method)?
- Check: Does this RQ address a gap in Stage 1, rather than replicating existing work?
- Claude stress-tests the RQ against the literature: alternative explanations pre-empted? identification strategy plausible?
- User confirms direction before proceeding
- Output: `plan/research-question.md`

**Stage 3: Multi-Perspective Idea Stress-Test** *(from Sibyl)*
> *Entry: confirmed RQ. Exit: refined proposal with hypotheses + documented reservations.*
- Evaluate the idea from 4 adversarial perspectives:
  - **Innovator**: What is genuinely novel? Cross-domain analogies? New measurement approaches?
  - **Skeptic**: Endogeneity threats? Data feasibility? Publication risk? Alternative explanations?
  - **Pragmatist**: Can we actually get the data? Is the timeline realistic? Is the sample large enough?
  - **Theorist**: What is the causal mechanism? Which theory anchors this? Are the hypotheses falsifiable?
- Synthesize into a refined proposal with a crisp contribution statement (1 paragraph)
- Lock in 2-3 testable hypotheses before moving to design
- Output: `idea/stress-test.md`, `idea/proposal.md`, `idea/hypotheses.md`

---

#### Phase II — Design & Validation

**Stage 4: Research Design & Pre-Analysis Plan**
> *Entry: approved hypotheses. Exit: fully specified empirical plan registered before touching data.*
- Define all variables: dependent, independent, controls — with data sources (WRDS, CSMAR, Bloomberg ESG, Refinitiv, etc.)
- Specify empirical model: OLS / panel FE / DiD / IV / PSM; justify choice
- Endogeneity strategy: instrument candidates, parallel-trends test plan, Oster (2019) coefficient stability
- If LLM text analysis: draft prompt, specify human validation protocol (target Cohen's kappa ≥ 0.7, n ≥ 200 hand-labeled)
- Pre-specify all planned robustness checks and mechanism tests (prevents HARKing)
- Output: `plan/methodology.md`, `plan/variable-definitions.md`, `plan/preanalysis-plan.md`

**Stage 5: Pilot Study** *(GO / NO-GO gate)*
> *Entry: pre-analysis plan. Exit: GO decision with evidence, or PIVOT back to Stage 3.*
- Sample: ~100-500 firms, 1-3 years; collect pilot data and run pipeline end-to-end
- Checklist:
  - [ ] LLM prompt produces internally consistent outputs across diverse cases?
  - [ ] Main coefficient has expected sign and is at least marginally significant?
  - [ ] No obvious data quality issues (survivorship bias, unit mismatch, missing >30%)?
  - [ ] Cohen's kappa ≥ 0.7 on hand-labeled validation set?
- **GO**: all boxes checked → proceed to full data collection
- **NO-GO**: fail any box → diagnose root cause → revise prompt / RQ / method → re-pilot
- Output: `exp/pilot-results.md` (include pilot regression table)

---

#### Phase III — Execution

**Stage 6: Data Assembly & LLM Processing**
> *Entry: GO from pilot. Exit: clean, analysis-ready panel dataset.*
- **Structured data**: download financial/governance variables from WRDS, CSMAR, Bloomberg, Refinitiv; merge on firm-year identifiers; handle currency, fiscal-year, and exchange adjustments
- **Text data**: collect CSR reports / earnings calls / proxy statements; run LLM pipeline with batch processing and error handling
- Validate LLM outputs: inter-coder reliability on 10% holdout, prompt sensitivity (paraphrase test), model robustness (GPT-4o vs Claude cross-check)
- Document all prompts in appendix-ready format (exact wording, model, temperature, date)
- Winsorize continuous variables at 1%/99%; construct final panel; export to `data/final_panel.csv`
- Output: `data/final_panel.csv`, `data/data-construction.md`, `data/llm-validation.md`

**Stage 7: Empirical Analysis & Robustness**
> *Entry: final_panel.csv + pre-analysis plan. Exit: complete set of tables and figures.*
- Follow the pre-analysis plan strictly; any deviation must be documented and justified
- **Main results**: OLS / panel FE with two-way fixed effects; standard errors clustered by firm (and year if T > 20)
- **Robustness checks** (minimum 3): alternative variable definitions, sub-sample splits, alternative estimators (Poisson, logit, Fama-MacBeth)
- **Endogeneity**: IV regression, DiD parallel-trends, PSM matched-sample, or Oster δ; report at least one
- **Mechanism tests**: mediation / moderation analysis for the theoretical channel
- **Heterogeneity**: cross-sectional splits by size, industry, country, governance quality
- **Economic magnitude**: translate coefficients into real-world units (% change, dollar value)
- Output: `exp/results/` (all tables as .csv + LaTeX), `exp/figures/`

---

#### Phase IV — Writing & Publication

**Stage 8: Result Interpretation (Multi-Angle)**
> *Entry: completed results tables. Exit: clear narrative strategy + decision to write or pivot.*
- Assess results from 3 perspectives before writing a single word:
  - **Optimist**: Which findings are strongest? What are the theoretical and practical implications?
  - **Skeptic**: Which results are fragile? Missing robustness tests? Remaining identification concerns?
  - **Strategist**: Main paper vs online appendix split? What is the core narrative thread?
- **PROCEED**: results are coherent, main finding survives robustness → write paper
- **PIVOT**: null result or reversed sign → return to Stage 3, select alternative hypothesis
- Output: `idea/result-assessment.md`

**Stage 9: Paper Outline & Visual Plan**
> *Entry: result-assessment.md. Exit: locked outline with every table/figure assigned before writing.*
- Draft full section outline (1 paragraph per section summarizing argument)
- **Mandatory visual plan** — assign each table/figure before writing:
  - Table 1: Summary statistics (full sample + split by key variable)
  - Table 2: Main regression (baseline + FE variants)
  - Table 3: Robustness checks (battery format)
  - Table 4: Endogeneity test (IV / DiD / PSM)
  - Table 5: Mechanism test
  - Table 6: Heterogeneity analysis
  - Figure 1: Research framework / conceptual model
  - Figure 2: Key trend or event-study plot
- Each visual: description, data source, generation method, target section, caption draft
- Output: `writing/outline.md` (with visual plan embedded)

**Stage 10: Paper Writing**
> *Entry: locked outline. Each section is written in sequence, cross-referencing the visual plan.*
- **Introduction**: hook (phenomenon + puzzle) → gap → what we do → main findings → contributions (3 bullets) → roadmap
- **Literature & Hypotheses**: anchor theory → 3 literature streams → hypotheses derived from gaps, not cherry-picked support
- **Data & Methodology**: data sources → sample construction (obs count at each filter step) → LLM method + validation → variable definitions → model specification
- **Results**: main table → robustness → endogeneity → mechanisms → heterogeneity; each sub-section starts with one-sentence finding
- **Conclusion**: summary → theoretical contribution → managerial/policy implication → limitations → future research
- Anti-AI pass after each section: remove "it is important to note", "delve into", "furthermore", "robust", "nuanced", "leverage", "underscore"
- Output: `writing/sections/`, `writing/paper.md`

**Stage 11: Quality Gate & Revision** *(from Sibyl)*
> *Entry: complete draft. Exit: submission-ready manuscript (score ≥ 7/10 on all dimensions).*
- Score on 6 dimensions (1-10):
  1. **Contribution clarity**: Is the "so what" compelling to a non-specialist referee?
  2. **Empirical rigor**: Endogeneity credibly addressed? ≥3 robustness checks passed?
  3. **LLM methodology**: Validation reported? Prompt disclosed in appendix? Sensitivity tested?
  4. **Writing quality**: Clear, concise, no AI patterns, flows like a native speaker?
  5. **Journal fit**: Matches recent publications in target journal? Correct citation style?
  6. **Citation completeness**: Every empirical claim cited? No missing core papers?
- Any dimension < 7 → targeted revision of that section → re-score
- Final anti-AI sweep on full paper
- Output: `writing/quality-review.md`, `writing/paper-final.md`

**Stage 12: Submission → Review → Acceptance**
> *Entry: paper-final.md. Covers the full publication cycle.*
- **Submission**: format for target journal (LaTeX / Word template); write cover letter (contribution + fit + no conflicts)
- **Desk rejection contingency**: if rejected without review, diagnose (fit? contribution framing?) → retarget next journal
- **Revise & Resubmit**: systematic point-by-point rebuttal (Reviewer 1 / Reviewer 2 / AE); each point = acknowledge + respond + cite evidence + show change
- **Post-acceptance**: camera-ready formatting, data availability statement, replication package (`code/` + `data/README`)
- **Dissemination**: conference slides, 1-page policy brief, Twitter/LinkedIn thread, SSRN posting
- Output: submission package, `rebuttal/response-to-reviewers.md`, `dissemination/`

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
