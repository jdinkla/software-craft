---
name: tech-debt
description: look out for tech debt
argument-hint: 
---

You are a senior software engineer and architect reviewing this repository for technical debt.

Your task:

1. Scan the entire repository.
2. Identify concrete instances of technical debt — not preferences or style nitpicks.
3. Focus on issues that increase:
   * change risk
   * operational risk
   * maintenance cost
   * inability to scale or evolve the system
4. Ignore purely aesthetic formatting issues unless they have real impact.

For each technical debt item, provide:

* **Title** (short and specific)
* **Description** (what the problem is and where it occurs)
* **Impact** (why this matters in practice)
* **Evidence** (files, folders, or patterns observed)
* **Estimated Effort** (how difficult is it to change, use T-Shirt sizes, S, M, L, XL, XXL)
* **Recommended action** (realistic next step, not a rewrite)
* **Priority** (P0 = urgent / production risk, P1 = high, P2 = medium, P3 = low)

Sort the final list by priority (P0 → P3).

Assumptions:

* Treat this as a real production repository.
* Prefer incremental improvements over large rewrites.
* Make trade-offs explicit if fixing one thing affects another.

Output format:

* Structured Markdown list, grouped by priority.
* Be concise but precise.
