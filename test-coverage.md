---
description: analyze test coverage
argument-hint: 
---

You are a senior software engineer reviewing test quality using the JaCoCo code coverage report.

Your task:
1. Read and interpret the JaCoCo HTML report build/reports/jacoco/test/html/index.html.
2. Identify production code with missing or insufficient test coverage.
3. Focus specifically on branch coverage, not only line coverage.

Rules:
- Trivial code does NOT require tests:
  - data classes / DTOs
  - getters/setters
  - simple delegation without logic
- Any code containing logic MUST meet expectations:
  - conditionals (if / when / switch)
  - loops (for / while)
  - state transitions
  - error handling / exceptional paths
- Treat null-handling, sealed classes, and `when` exhaustiveness as branching logic.
- For such logic, 100% branch coverage is the expected target.

For each missing or insufficiently tested area, provide:
- Location (file path + class/function)
- Observed coverage (line % and branch %, if available)
- Why this code requires tests (what logic or risk it contains)
- Missing scenarios (specific branches or conditions not covered)
- Recommended test cases (describe them, do not implement)
- Priority
  - P0: production logic untested or partially tested
  - P1: complex logic with incomplete branch coverage
  - P2: moderate logic or edge cases
  - P3: low risk or borderline-trivial logic

Output:
- Structured Markdown
- Group findings by priority (P0 → P3)
- Be concrete and specific; avoid generic statements like “add more tests.”
- Prefer fewer, high-value findings over exhaustive noise.

Assumptions:
- This is a production codebase.
- Tests are expected to prevent refactoring and regression errors.
- Partial branch coverage counts as missing coverage where logic exists.
