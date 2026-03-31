# my-copilot

> A practical collection of GitHub Copilot prompt patterns for performing systematic, high-quality code reviews in VS Code

[![VS Code](https://img.shields.io/badge/VS%20Code-Required-blue.svg)](https://code.visualstudio.com/)
[![GitHub Copilot](https://img.shields.io/badge/GitHub%20Copilot-Required-purple.svg)](https://github.com/features/copilot)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 📋 Overview

Stop doing manual, inconsistent code reviews. **my-copilot** provides production-ready prompt patterns that transform GitHub Copilot into your personal code review expert. Get systematic, repeatable reviews that catch issues in security, performance, architecture, design compliance, and safety-critical failure modes.

**What you get:**
- 🎯 **4 specialized prompts** - Design compliance, comprehensive checklists, quality reviews, and DRBFM analysis
- 📝 **Real example outputs** - See what professional reviews look like before you run them
- 🚦 **Prioritized findings** - Critical/Important/Suggestion levels help you focus on what matters
- ✅ **Copy-paste ready** - Just install and run, no configuration needed

**Perfect for:**
- Safety-critical and automotive projects requiring formal reviews
- Teams using Rhapsody design models with strict traceability requirements
- Projects needing MISRA-C compliance verification
- Anyone who wants consistent, high-quality code reviews

## 🔧 Prerequisites

**Required:**
- [VS Code](https://code.visualstudio.com/) with [GitHub Copilot](https://github.com/features/copilot) extension
- Git repository with committed changes

**Optional (prompt-specific):**
- **Against Design Review**: [sbsx2plantuml](https://github-ix.int.automotive-wan.com/uidn8804/sbsx2plantuml) converter, Rhapsody design files
- **DRBFM Analysis**: [drbfm-analyst](agents/drbfm-analyst.agent.md) agent installed

## 🚀 Quick Start

### Installation

**For Prompts:**
1. Copy the desired prompt file from [prompts/](prompts/) directory
2. Paste into your workspace at `.github/prompts/`
3. Open Copilot Chat in VS Code
4. Type `/` and select your installed prompt

**For Agents:**
1. Copy the agent file from [agents/](agents/) directory (e.g., [drbfm-analyst.agent.md](agents/drbfm-analyst.agent.md))
2. Paste into your workspace at `.github/agents/`
3. Select the agent from the chat mode dropdown (agent/ask/edit)

### First Run Example - Quality Review

```bash
# 1. Make some code changes
git add .
git commit -m "Add new feature"

# 2. Install the prompt
cp prompts/perform-quality-code-review.prompt.md $YOUR_WORKSPACE/.github/prompts/

# 3. Open VS Code Copilot Chat
# Type: /perform-quality-code-review

# 4. Get prioritized findings with 🔴 Critical, 🟡 Important, 🟢 Suggestion levels
```

### First Run Example - Design Review

```bash
# 1. Convert Rhapsody design to PlantUML
sbsx2plantuml your-design.sbsx -o .github/designs/puml/
# Will create automation workflow to download directly from JAZZ server if more users require to convert it frequently

# 2. Make code changes and commit
git add .
git commit -m "Implement feature from design"

# 3. Install the prompt
cp prompts/perform-code-review-agaisnt-design.prompt.md $YOUR_WORKSPACE/.github/prompts/

# 4. Run review in Copilot Chat
# Type: /perform-code-review-agaisnt-design

# 5. Review findings comparing your code against design diagrams
```

## 📚 Available Prompts

| Prompt | Best For | Prerequisites | Review Focus | Blocking Level |
|--------|----------|---------------|--------------|----------------|
| [Against Design](prompts/perform-code-review-agaisnt-design.prompt.md) | Rhapsody-based projects with strict traceability | `.puml` design files in `.github/designs/puml/` | Design compliance, architecture matching | 🔴 Critical mismatches |
| [Checklist](prompts/perform-code-review-checklist.prompt.md) | Formal reviews, automotive, MISRA-C compliance | None | 38-point checklist: traceability, security, MISRA-C | 🟡 Important |
| [Quality Review](prompts/perform-quality-code-review.prompt.md) | General projects, teaching best practices | None | Security, testing, performance, Clean Code | 🔴🟡🟢 Mixed priorities |
| [DRBFM Analysis](prompts/perform-drbfm-analysis.prompt.md) | Safety-critical systems, failure mode analysis | [drbfm-analyst](agents/drbfm-analyst.agent.md) agent | Failure modes, root causes, preventive measures | 🔴 Critical risks |

## 📖 Detailed Prompt Guides

### 1. Code Review Against Design
**File:** [perform-code-review-agaisnt-design.prompt.md](prompts/perform-code-review-agaisnt-design.prompt.md)

**Purpose:** Compare code implementation against Rhapsody design diagrams (exported as PlantUML `.puml` files).

**Key Features:**
- Identifies changed files from latest commit
- References design diagrams from `.github/designs/puml/`
- Enforces strict adherence to design specifications
- Flags deviations as critical issues
- Provides evidence of compliance when code matches design

**Priority Levels:**
- 🔴 **CRITICAL**: Traceability issues, design mismatches (block merge)
- 🟡 **IMPORTANT**: Architecture deviations (requires discussion)

**When to Use:**
- Your project uses Rhapsody design models
- Design files are exported to PlantUML format
- Strict design-to-code traceability is required
- You need to verify implementation matches architecture diagrams

**Required Setup:**
- Design diagrams in `.github/designs/puml/` directory
- Use [sbsx2plantuml](https://github-ix.int.automotive-wan.com/uidn8804/sbsx2plantuml) to convert Rhapsody `.sbsx` to `.puml` files

---

### 2. Checklist-Based Review
**File:** [perform-code-review-checklist.prompt.md](prompts/perform-code-review-checklist.prompt.md)

**Purpose:** Perform comprehensive review against a 38-point quality checklist covering traceability, testing, security, performance, and MISRA-C compliance.

**Key Features:**
- Reviews all changed files against comprehensive checklist
- Marks each item as: `[yes]`, `[no]`, `[partial]`, `[n.a]`, or `[ ]`
- Generates formal report: `SUZ_MY24_SW_{TICKET}_Code_Review_Checklist.md`
- Includes detailed comments for non-compliant items

**Checklist Categories:**
- Bilateral traceability (SW Units ↔ Design, SW Tests ↔ Units)
- Documentation standards and markers
- Error handling and thread safety
- Security and compliance to project standards
- Resource leaks and performance
- MISRA-C 2004 compliance (rules not covered by Klocwork)

**When to Use:**
- Formal reviews required by development process
- Safety-critical or automotive projects
- MISRA-C compliance verification needed
- Comprehensive quality gate before merge

---

### 3. Quality Code Review
**File:** [perform-quality-code-review.prompt.md](prompts/perform-quality-code-review.prompt.md)

**Purpose:** Generic, best-practices code review applicable to any project with prioritized findings.

**Key Features:**
- Three-tier priority system (Critical, Important, Suggestion)
- Comprehensive coverage: security, testing, performance, architecture
- Code quality standards (Clean Code, SOLID principles)
- Structured comment format with examples
- Language-agnostic guidelines

**Review Areas:**
- **Clean Code:** Naming, SRP, DRY, function size, nesting depth
- **Security:** SQL injection, input validation, secrets exposure, authorization
- **Testing:** Coverage, test structure, independence, edge cases
- **Performance:** N+1 queries, algorithmic complexity, resource management
- **Architecture:** Separation of concerns, dependency direction, design patterns
- **Documentation:** API docs, complex logic explanation, README updates

**Priority Levels:**
- 🔴 **CRITICAL**: Security, correctness, breaking changes, data loss (block merge)
- 🟡 **IMPORTANT**: Code quality, test coverage, performance, architecture (requires discussion)
- 🟢 **SUGGESTION**: Readability, optimization, best practices (non-blocking)

**When to Use:**
- General-purpose code reviews
- Projects without specific design artifacts
- Cross-language codebases
- Teaching good coding practices to teams

---

### 4. DRBFM Analysis
**File:** [perform-drbfm-analysis.prompt.md](prompts/perform-drbfm-analysis.prompt.md)

**Purpose:** Perform Design Review Based on Failure Mode (DRBFM) analysis to systematically identify potential failure modes and assess impacts of code changes in safety-critical systems using standardized terminology and structured format.

**Key Features:**
- Analyzes code changes using Gerrit Change IDs or diffs
- Identifies "Changes," "Impacts," and "Failure Modes" (DRBFM core pillars)
- Generates structured failure mode documentation
- Refines user-provided failure modes into technically precise format
- Suitable for Excel documentation (concise, professional)
- Requires [drbfm-analyst](agents/drbfm-analyst.agent.md) agent

**DRBFM Structure:**
- **Behavior Before/After Change**: Clear description of system behavior changes
- **Concerns**: Specific failure conditions and their occurrence
- **System Failures**: Technical impact of each concern
- **Root Causes**: Race conditions, overflows, configuration failures, etc.
- **End User Impact**: Real-world consequences
- **Preventive Measures**: System, software, evaluation, and manufacturing measures

**When to Use:**
- Safety-critical or automotive projects
- Analyzing changes that could introduce failure modes
- When formal DRBFM documentation is required
- Evaluating edge cases and fault conditions
- Preparing technical risk assessments

**Required Setup:**
- Install [drbfm-analyst](agents/drbfm-analyst.agent.md) agent in `.github/agents/`
- Git repository with committed changes or Gerrit integration

---

## 🤖 Available Agents

| Agent | Best For | Prerequisites | Analysis Focus |
|-------|----------|---------------|----------------|
| [DRBFM Analyst](agents/drbfm-analyst.agent.md) | Safety-critical failure mode analysis | [perform-drbfm-analysis](prompts/perform-drbfm-analysis.prompt.md) prompt | Failure modes, impacts, preventive measures |
| [Potential Cause Code Analysis](agents/potential-cause-code-analysis.agent.md) | Investigating bugs, system behavior, log analysis | Git repo or log files | Root cause, timeline, evidence, fix recommendations |

## 📖 Detailed Agent Guides

### 1. DRBFM Analyst
**File:** [drbfm-analyst.agent.md](agents/drbfm-analyst.agent.md)

Supporting agent for the [DRBFM Analysis prompt](prompts/perform-drbfm-analysis.prompt.md). See [DRBFM Analysis](#4-drbfm-analysis) above for full details.

---

### 2. Potential Cause Code Analysis
**File:** [potential-cause-code-analysis.agent.md](agents/potential-cause-code-analysis.agent.md)

**Purpose:** Root-cause analysis specialist for any software issue. Investigates bugs and unexpected behavior by correlating log files, tracing message flows across components, and pinpointing failure points — regardless of technology stack or protocol.

**Key Features:**
- Builds chronological timelines correlating events across multiple log sources
- Traces correlation IDs, session IDs, and request IDs across services
- Identifies the exact component, function, and line responsible for a failure
- References code paths alongside log evidence for stronger diagnosis
- Produces minimal, targeted fix recommendations (`[CODE]`, `[CONFIG]`, or `[PROCESS]`)
- Provides reproduction steps with pre-conditions, actions, expected vs. observed results

**Output Sections:**
- **Timeline**: Chronological table of events anchored to identifiers
- **Root Cause**: Single precise statement — what failed, where, and why
- **Evidence**: Log lines and code references supporting the root cause
- **Fix Recommendation**: Smallest change that prevents recurrence
- **Reproduction Steps**: How to reproduce the issue reliably

**Priority Levels:**
- 🔴 **CRITICAL**: Data corruption, security breach, system crash, silent failure
- 🟡 **IMPORTANT**: Intermittent failures, incorrect behavior, performance degradation
- 🟢 **SUGGESTION**: Hardening improvements, observability, defensive checks

**When to Use:**
- Investigating a reported bug or production incident
- Debugging unexpected system behavior or missing responses
- Tracing a request across microservices or distributed components
- Analyzing log files when a correlation/session ID is available
- Preparing a root-cause report after an outage

**How to Use:**
1. Copy the agent to your workspace: `cp agents/potential-cause-code-analysis.agent.md $YOUR_WORKSPACE/.github/agents/`
2. Select **potential-cause-code-analysis** from the Copilot Chat agent dropdown
3. Provide a problem description, ticket ID, correlation/session ID, or log file path

---

## 📂 Repository Structure

> **Note:** This repository contains two distinct sets of prompt files with different purposes:
> - **`prompts/`** — Distributable prompts intended to be copied into your own projects.
> - **`.github/prompts/`** — Internal maintenance prompts used to maintain *this* repository (e.g. reviewing new prompt additions for structural compliance). These are **not** meant for distribution and should not be copied to other projects.

### Prompt Files
Located in [prompts/](prompts/):
- [perform-code-review-agaisnt-design.prompt.md](prompts/perform-code-review-agaisnt-design.prompt.md) - Compare implementation against Rhapsody design diagrams
- [perform-code-review-checklist.prompt.md](prompts/perform-code-review-checklist.prompt.md) - Comprehensive 38-point quality checklist review
- [perform-quality-code-review.prompt.md](prompts/perform-quality-code-review.prompt.md) - Generic best practices review with prioritized findings
- [perform-drbfm-analysis.prompt.md](prompts/perform-drbfm-analysis.prompt.md) - Design Review Based on Failure Mode (DRBFM) analysis for safety-critical changes

### Agent Files
Located in [agents/](agents/):
- [drbfm-analyst.agent.md](agents/drbfm-analyst.agent.md) - Specialized agent for DRBFM analysis
- [potential-cause-code-analysis.agent.md](agents/potential-cause-code-analysis.agent.md) - Root-cause analysis agent for investigating software issues via log correlation and message tracing

### Example Outputs
Located in [example/prompts/](example/prompts/):
- [example-output-perform-code-review-against-design.md](example/prompts/example-output-perform-code-review-against-design.md) - Real-world design review output
- [example-output-perform-code-review-checklist.md](example/prompts/example-output-perform-code-review-checklist.md) - Checklist review template
- [example-output-perform-quality-code-review.md](example/prompts/example-output-perform-quality-code-review.md) - Quality review narrative example
- [example-output-perform-drbfm-analysis.md](example/prompts/example-output-perform-drbfm-analysis.md) - DRBFM analysis example

Located in [example/agents/](example/agents/):
- [example-output-potential-cause-code-analysis.agent.md](example/agents/example-output-potential-cause-code-analysis.agent.md) - analyze potential cause in code level against available logs

---

## 🎯 Example Outputs

See real-world examples of what each prompt produces:

| Prompt / Agent | Example Output | Description |
|----------------|----------------|-------------|
| Against Design | [View Example](example/prompts/example-output-perform-code-review-against-design.md) | Shows design compliance checking with critical/important findings |
| Checklist | [View Template](example/prompts/example-output-perform-code-review-checklist.md) | Complete 38-point checklist with yes/no/partial/n.a. markings |
| Quality Review | [View Example](example/prompts/example-output-perform-quality-code-review.md) | Prioritized findings with code examples and recommendations |
| DRBFM Analysis | [View Example](example/prompts/example-output-perform-drbfm-analysis.md) | Failure mode analysis with concerns, root causes, and preventive measures |
| Potential Cause Code Analysis | [View Example](example/agents/example-output-potential-cause-code-analysis.agent.md) | Root-cause investigation with timeline, evidence, and fix recommendations |

---

## 💡 Tips & Best Practices

**For Best Results:**
- ✅ Commit your changes before running reviews (prompts analyze git diffs)
- ✅ Start with Quality Review for general projects, then add specialized prompts
- ✅ Use Against Design review only if you have `.puml` design files
- ✅ Run Checklist review before formal code review meetings
- ✅ Combine multiple prompts for comprehensive analysis

**Common Workflows:**
1. **Daily development**: Quality Review → Fix Critical/Important issues
2. **Formal review**: Checklist → Generate report → Review meeting
3. **Design-driven**: Against Design → Fix mismatches → Quality Review
4. **Safety-critical**: DRBFM Analysis → Document failure modes → Preventive measures

---

## 🤝 Contributing

Contributions are welcome! Please feel free to:
- Report issues or suggest improvements
- Share your custom prompt patterns
- Add example outputs from your projects
- Improve documentation

---

## 📄 License

MIT License - feel free to use and modify for your projects.