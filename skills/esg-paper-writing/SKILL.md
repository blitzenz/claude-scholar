---
name: esg-paper-writing
description: Write publication-ready ESG, sustainability, finance, and management papers for top business journals (Journal of Finance, Management Science, Strategic Management Journal, Journal of Business Ethics, Business Strategy and the Environment, Review of Financial Studies, Academy of Management Journal). Use when drafting empirical papers using LLMs/NLP for ESG text analysis, writing literature reviews, finding related work, verifying citations, or preparing submissions. Includes APA/AMA citation workflows, journal-specific checklists, and empirical writing conventions.
version: 1.0.0
tags: [Academic Writing, ESG, Sustainability, Finance, Management, Business, LLM, NLP, Empirical Research, Citations, Journal of Finance, Management Science]
---

# ESG & Business Paper Writing for Top Journals

Expert-level guidance for writing publication-ready empirical papers targeting **Journal of Finance (JF), Review of Financial Studies (RFS), Journal of Financial Economics (JFE), Management Science (MS), Strategic Management Journal (SMJ), Academy of Management Journal (AMJ), Journal of Business Ethics (JBE), and Business Strategy and the Environment (BSE)**.

## Core Philosophy: Collaborative Writing

**Paper writing is collaborative, but Claude should be proactive in delivering drafts.**

The typical workflow starts with a research dataset, LLM-processed ESG text results, and empirical findings. Claude's role is to:

1. **Understand the research design** by exploring data, regression results, and existing documentation
2. **Deliver a complete first draft** when confident about the contribution
3. **Search literature** using web search and APIs to find relevant citations from business/finance/ESG databases
4. **Refine through feedback cycles** when the researcher provides input
5. **Ask for clarification** only when genuinely uncertain about key decisions

**Key Principle**: Be proactive. Produce something concrete they can react to, then iterate based on response.

---

## ⚠️ CRITICAL: Never Hallucinate Citations

**This is the most important rule in academic writing with AI assistance.**

AI-generated citations have a ~40% error rate. Hallucinated references are a serious form of academic misconduct.

**NEVER generate citations from memory. ALWAYS verify programmatically.**

| Action | ✅ Correct | ❌ Wrong |
|--------|-----------|----------|
| Adding a citation | Search API → verify → fetch → format | Write citation from memory |
| Uncertain about a paper | Mark as `[CITATION NEEDED]` | Guess the reference |
| Can't find exact paper | Note: "placeholder - verify" | Invent similar-sounding paper |

### Citation APIs for Business/ESG Research

```python
# CrossRef API - DOI lookup for all journals
import requests
doi = "10.1111/jofi.12234"
r = requests.get(f"https://api.crossref.org/works/{doi}")
metadata = r.json()["message"]

# Semantic Scholar API - paper search
r = requests.get("https://api.semanticscholar.org/graph/v1/paper/search",
                 params={"query": "ESG greenwashing LLM", "fields": "title,authors,year,venue,externalIds"})

# Web of Science / SSRN searches via WebSearch
# Search: site:ssrn.com "ESG disclosure" "language model"
```

---

## Paper Structure for Top Business Journals

### Standard Empirical Paper Structure

```
1. Introduction (3-4 pages)
   - Hook: Why does this matter economically/societally?
   - Research question (one clear sentence)
   - What you do (brief methodology)
   - Main findings (3-4 key results)
   - Contribution to literature (3 streams)
   - Road map

2. Literature Review & Hypothesis Development (4-6 pages)
   - Theoretical framework
   - ESG/sustainability literature stream
   - LLM/NLP methodology literature stream
   - Relevant finance/management literature stream
   - Hypotheses (clearly numbered H1, H2, ...)

3. Data & Methodology (4-6 pages)
   - Data sources (Bloomberg ESG, Refinitiv, WRDS, etc.)
   - Sample construction and selection criteria
   - LLM/NLP methodology description
   - Variable definitions (main variables, controls)
   - Descriptive statistics table
   - Empirical model specification

4. Results (6-8 pages)
   - Main regression results
   - Robustness checks
   - Endogeneity concerns and solutions (IV, DiD, PSM)
   - Cross-sectional heterogeneity

5. Additional Analyses / Extensions (2-3 pages)
   - Mechanism tests
   - Heterogeneity analysis
   - Economic magnitude discussion

6. Conclusion (1-2 pages)
   - Summary of findings
   - Implications (policy, practice, theory)
   - Limitations and future research

References
Appendix (variable definitions, additional tables, LLM prompts)
```

---

## Journal-Specific Guidelines

### Journal of Finance / RFS / JFE (Finance Journals)
- **Style**: Extremely rigorous empirical standards; every result must be robust
- **Key concerns**: Endogeneity, sample selection bias, economic significance
- **Tables**: Must include N, t-stats (in parentheses), R², fixed effects indicators
- **Tone**: Formal, precise, understated — never oversell
- **Length**: ~50 pages including tables

### Management Science (MS)
- **Style**: Methods-rigorous; welcomes interdisciplinary work
- **Key concerns**: Identification strategy, generalizability
- **LLM papers**: Must validate LLM outputs against human coders (Cohen's kappa ≥ 0.7)
- **Tone**: Clear and accessible to broad management audience

### Strategic Management Journal (SMJ) / Academy of Management Journal (AMJ)
- **Style**: Theory-driven; strong literature grounding required
- **Key concerns**: Theoretical contribution, practical implications
- **APA style**: Use APA 7th edition citations
- **Tone**: Connects to broader management theory

### Journal of Business Ethics (JBE)
- **Style**: Interdisciplinary; welcomes normative and empirical work
- **Key concerns**: Ethical framing, stakeholder theory
- **Length**: Flexible, often 8,000-12,000 words

### Business Strategy and the Environment (BSE)
- **Style**: Applied sustainability focus
- **Key concerns**: Environmental measurement validity, policy relevance
- **Impact factor**: ~10; accepts LLM-based ESG measurement studies

---

## LLM Methodology Section Template

When the paper uses LLMs for ESG analysis, include this in Methodology:

```
3.2 LLM-Based ESG Analysis

We employ [model name, e.g., GPT-4, Claude 3.5] to [task description, e.g., classify ESG
disclosures, extract sustainability metrics, detect greenwashing].

**Prompt Design**: We construct structured prompts that [describe approach]. The full
prompts are provided in Appendix [X].

**Validation**: To validate our LLM-based measure, we manually coded a random sample
of [N] documents and compared against LLM outputs. The agreement rate is [X]% with
Cohen's kappa of [X], indicating [substantial/near-perfect] agreement (Landis & Koch, 1977).

**Robustness to Model Choice**: We replicate our main results using [alternative model]
in Table [X] and find qualitatively identical results.
```

---

## Variable Definition Table Template

Standard format for business papers:

| Variable | Definition | Source |
|----------|------------|--------|
| ESG_Score | Overall ESG score from [provider] | Bloomberg/Refinitiv/MSCI |
| LLM_ESG | LLM-generated ESG disclosure quality score (0-100) | Authors' calculation |
| Greenwash | Binary indicator: 1 if LLM detects greenwashing patterns | Authors' calculation |
| Size | Natural log of total assets | Compustat/CSMAR |
| Leverage | Total debt / total assets | Compustat/CSMAR |
| ROA | Net income / total assets | Compustat/CSMAR |
| Tobin_Q | (Market cap + total debt) / total assets | Compustat/CSMAR |
| Inst_Own | Institutional ownership percentage | Thomson Reuters |

---

## Regression Table Template

Standard format for top finance/management journals:

```
Table [X]: [Dependent Variable] and [Main Independent Variable]

                    (1)         (2)         (3)
                  OLS         FE          FE+Controls
---------------------------------------------------
LLM_ESG        0.023***    0.019**     0.017**
               (3.45)      (2.87)      (2.63)

[Control vars]    No          No          Yes
Firm FE           No         Yes          Yes
Year FE           No         Yes          Yes
---------------------------------------------------
N               12,450      12,450      11,823
R²              0.124       0.387       0.412
---------------------------------------------------
Notes: t-statistics in parentheses. Standard errors
clustered at the firm level. *p<0.1, **p<0.05, ***p<0.01
```

---

## Literature Search Strategy for ESG Research

### Key Search Databases
1. **Web of Science** (via institutional access) — best for citation counts, journal ranking
2. **SSRN** (ssrn.com) — latest working papers in finance/economics
3. **Google Scholar** — broad coverage
4. **EBSCOhost / ProQuest** — business database access

### ESG Research Keywords
```
Primary: "ESG disclosure" OR "sustainability reporting" OR "CSR" OR "greenwashing"
LLM methods: "large language model" OR "NLP" OR "text analysis" OR "natural language processing"
Finance: "asset pricing" OR "firm value" OR "stock returns" OR "cost of capital"
Combined: ("ESG" OR "sustainability") AND ("language model" OR "NLP" OR "text mining")
```

### Target Paper Venues (Three-Star and Above)
- **FT50 journals**: Includes AMJ, MS, SMJ, JFE, JF, RFS
- **ABS 4* / 4**: JBE (3), BSE (3), Journal of Accounting Research (4)
- **UTD24 journals**: Includes MS, SMJ, AMJ

---

## Citation Format

### APA 7th Edition (for management journals)
```
Author, A. A., & Author, B. B. (Year). Title of article. Journal Name, Volume(Issue), pages. https://doi.org/xxxxx
```

### Chicago Author-Date (for economics/finance journals)
```
Author, First, and Second Author. Year. "Title of Article." Journal Name Volume (Issue): pages.
```

### Fetch BibTeX via CrossRef
```python
import requests

def get_bibtex(doi):
    url = f"https://doi.org/{doi}"
    r = requests.get(url, headers={"Accept": "application/x-bibtex"})
    return r.text

# Example
bibtex = get_bibtex("10.1111/jofi.12234")
```

---

## Quality Checklist Before Submission

### Empirical Standards
- [ ] Main results survive at least 3 robustness checks
- [ ] Endogeneity addressed (IV, DiD, PSM, or thorough discussion)
- [ ] Standard errors clustered appropriately (firm, industry, or year)
- [ ] Economic magnitude discussed (not just statistical significance)
- [ ] Sample construction clearly documented with attrition table

### LLM Methodology Standards
- [ ] Prompts fully disclosed in appendix
- [ ] Human validation conducted (Cohen's kappa reported)
- [ ] Results robust to alternative LLM (at minimum GPT-4 vs Claude)
- [ ] Prompt sensitivity analysis conducted
- [ ] Computational costs and reproducibility addressed

### Writing Standards
- [ ] Introduction states contribution in first 2 paragraphs
- [ ] Each hypothesis derived from theory/mechanism
- [ ] Tables self-contained (notes explain all abbreviations)
- [ ] Abstract ≤ 150 words with results stated numerically
- [ ] No jargon unexplained on first use

### Journal Fit
- [ ] Confirms target journal scope (check journal website)
- [ ] Cites recent papers from target journal (last 2-3 years)
- [ ] Matches journal's methodological norms

---

## Contribution Framing for ESG + LLM Papers

When writing the introduction, frame contributions across three dimensions:

**1. Substantive ESG Contribution**
> "We contribute to the ESG disclosure literature by [specific finding about ESG phenomenon]..."

**2. Methodological Contribution**
> "We introduce a novel LLM-based measure of [ESG construct] that [advantages over existing measures: scale, accuracy, coverage, timeliness]..."

**3. Broader Implications**
> "Our findings have implications for [regulators / investors / boards / policymakers] seeking to [practical application]..."

---

## Common Reviewer Concerns in ESG + LLM Papers

| Concern | Suggested Response |
|---------|-------------------|
| "LLM measure not validated" | Add human coding validation with kappa statistics |
| "Endogeneity in ESG-performance relation" | Use DiD around regulatory changes, IV with industry ESG norms |
| "Sample selection bias" | Document all exclusion criteria, show attrition table |
| "LLM hallucination in outputs" | Show output consistency across temperature settings and model versions |
| "ESG data provider divergence" | Replicate with multiple ESG data providers |
| "Omitted variable bias" | Include industry × year FE, show coefficient stability (Oster 2019) |
