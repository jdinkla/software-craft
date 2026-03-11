---
description: Analyze project docs against the documentation system and propose reorganization
allowed-tools: Read, Glob, Grep, Bash(find:*), Bash(wc:*), Bash(ls:*)
model: opus
---

# Documentation QA: Documentation System Analysis

Analyze the current project's documentation structure and classify it.

## The Framework

Documentation falls into exactly 4 types, arranged on two axes:

|              | Practical (doing)    | Theoretical (understanding) |
|--------------|---------------------|-----------------------------|
| **Learning** | **Tutorials**       | **Explanation**             |
| **Working**  | **How-to Guides**   | **Reference**               |

### 1. Tutorials (learning-oriented)
- Guide a beginner through a series of steps to complete a meaningful project
- The author is fully responsible for directing the learner
- Focus on learning by doing, not on explaining concepts
- Must be repeatable and produce concrete, visible results
- Pitfalls: too much explanation, too many options/alternatives, steps that don't produce visible output

### 2. How-to Guides (goal-oriented)
- Address a specific real-world problem ("How do I...?")
- Assume the reader has foundational knowledge
- Sequential steps leading to a practical result
- Titles should start with "How to..."
- Pitfalls: starting too early, over-explaining concepts, being too rigid/specific

### 3. Reference (information-oriented)
- Technical description of the machinery: APIs, classes, functions, config options
- Structure mirrors the codebase, not user workflows
- Austere, factual, consistent formatting
- Pitfalls: mixing in how-to instructions, becoming outdated vs. code, narrative tone

### 4. Explanation (understanding-oriented)
- Clarifies and illuminates concepts, provides context and background
- Discusses "why", design decisions, alternatives, trade-offs
- Discursive, can explore multiple viewpoints
- Pitfalls: scattered as fragments rather than standalone docs, unclear naming ("Background", "Other notes"), drifting into instructions

### The Collapse Problem
Without deliberate effort, these 4 types gravitationally collapse into each other. Mixed documents that try to be tutorial + reference + explanation serve none of those purposes well.

## Your Task

### Phase 1: Discovery

Find all documentation in the project. Look broadly:
- `docs/`, `doc/`, `documentation/` directories
- `specs/`, `spec/`, `specifications/` directories
- `guides/`, `tutorials/`, `howto/` directories
- `README.md`, `README.rst`, `CONTRIBUTING.md`, `ARCHITECTURE.md`, `CHANGELOG.md`
- `*.md`, `*.rst`, `*.txt`, `*.adoc` files in the project root
- Wiki or ADR (Architecture Decision Records) directories
- Inline doc directories that might exist inside `src/` or other code dirs

Use Glob and Grep to find these. Be thorough but efficient.

### Phase 2: Analysis

For each document or document group found:

1. **Read** the file (or at least the first ~100 lines for long files) to understand its content
2. **Classify** it into one or more of the 4  types, or flag it as "mixed" or "unclassifiable"
3. **Assess quality** within its type:
   - Does a tutorial actually guide step-by-step?
   - Does a how-to guide solve a specific problem?
   - Does reference material accurately mirror the code?
   - Does explanation provide genuine understanding?
4. **Note problems**: mixed types, missing types, orphaned docs, outdated content signals

### Phase 3: Report

Produce a structured report with these sections:

#### A. Inventory
A table listing each doc file, its classification (Tutorial / How-to / Reference / Explanation / Mixed / Other), and a one-line summary.

#### B. Coverage Matrix
Show which of the 4 types are present, underrepresented, or missing entirely. Use a simple grid:

```
              Practical    Theoretical
Learning      [status]     [status]
Working       [status]     [status]
```

Where status is one of: well-covered, partial, missing, mixed-in

#### C. Key Findings
- Documents that conflate multiple types (the collapse problem)
- Types that are entirely missing
- Documents that would benefit from splitting
- Naming/organization issues

#### D. Reorganization Plan (if needed)
Only propose reorganization if the current structure has real problems. Don't reorganize for its own sake.

If reorganization is warranted:
1. **Proposed directory structure** with the 4 types as organizing principle
2. **Migration table**: current file -> proposed location + any splitting needed
3. **New documents to create** for gaps in coverage
4. **Priority order** for the work (what matters most for users)

Be pragmatic. A small project with a good README and a few focused docs may not need a full  structure. The framework is a lens for analysis, not a mandate.

### Output Format

Use clear markdown. Be direct. Lead with findings, not methodology. If the docs are fine, say so and explain why. If they need work, be specific about what and why.
