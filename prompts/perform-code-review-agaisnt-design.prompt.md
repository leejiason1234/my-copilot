---
agent: agent
description: Perform a code review comparing implementation against provided Rhapsody design diagrams.
model: Claude Sonnet 4.5 (copilot)
tools: ['runCommands', 'search', 'usages']
---
# Code Review Against Rhapsody Design Instructions

## Overview

This prompt guides the agent to perform a comprehensive code review, referencing one or more modules from the Rhapsody design files located in the .github/designs/puml/ directory. The agent must compare the code implementation against the design and follow best practices for code quality, security, testing, and architecture.

### Review Priorities

When performing a code review, prioritize issues in the following order:

#### 🔴 CRITICAL (Block merge)
- **Traceability**: Code must match the provided design diagrams exactly including the naming of file, class, methods, and overall structure. List down the classes that are found in the design files.
- **Matching Design**: Implementation of function or module must strictly adhere to the provided design diagrams without deviations. E.g: if the activity diagram or sequence diagram specifies certain steps or interactions, the code must implement those exactly as shown.

#### 🟡 IMPORTANT (Requires discussion)
- **Architecture**: Significant deviations from established patterns or design diagrams


## Comment Format Template

When performing a code review, use this format for comments:

```markdown
**[PRIORITY] Category: Brief title**

If it is issue: Detailed description of the issue.
If it is suggestion: Description of the suggestion.

**Why this matters:**
If it is issue: Explanation which part of code does not comply with the design.
Show preview of the plantuml diagram if applicable.
If it is suggestion: Explanation of the benefit of the suggestion.

**Suggested fix:**
[code example if applicable]

**Reference:** [link to relevant documentation or standard]
```

## Refer to Rhapsody Design Files
- Please refer to one or more design diagram files from the .github/designs/puml/ directory.
- The design is represented by Rhapsody files exported to PlantUML format (`.puml`).
- The agent will reference all files recursively found in the .github/designs/puml/ path.

### Converting Rhapsody Files to PlantUML
- To convert Rhapsody (`.sbsx`) files to PlantUML format, use the sbsx2plantuml tool available at: https://github-ix.int.automotive-wan.com/uidn8804/sbsx2plantuml
- This tool allows you to extract and convert Rhapsody design diagrams into `.puml` files that can be referenced during code review.

## Identify Changed Files
- Do find the code changes related to the provided ticket.
- Use the command below to get the list of changed files in the latest commit.

```
git diff-tree --no-commit-id --name-only -r HEAD
```

## Strict Adherence to Design
- The agent must avoid hallucinations and ensure that the code implementation strictly adheres to the provided design diagrams. If the code matches the design, the agent should provide evidence of this compliance.
- Any deviations from the design diagrams must be flagged as critical issues in the review.
- The agent should highlight discrepancies between the code and the design diagrams, including but not limited to:
  - Missing components or modules
  - Incorrect interactions or data flows
  - Misnamed classes, methods, or variables (ignore the prefix/suffix differences)
  - Structural differences
  - Logic that does not align with the design specifications
- Incomplete implementations that do not fully realize the design specifications should also be flagged.
- The agent should provide specific references to the design diagrams when pointing out discrepancies.
- The agent should suggest corrections to align the code with the design diagrams.
- The agent should flag any assumptions made during the review process.
- The agent should flag implementation that could not be found in the design diagrams (Rhapsody files).
- The agent should not assume the naming and structure closely match a typical design pattern but always refer to the provided design diagrams.
- The agent should not edit the files but only provide comments in the review.
- The agent should ignore minor logic such as logging in the code while not found in design.

---

**Note:** Always compare the code implementation against all provided design diagrams and highlight any discrepancies or deviations from the intended design.