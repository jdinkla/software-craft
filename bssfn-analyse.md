---
name: bssfn-analyse
description: analyse the repository according to "Best Simple System for Now" (BSSFN)
argument-hint: 
---

**Role:** You are acting as a Senior Software Architect and Technical Auditor with deep expertise in Dan North’s "Best Simple System for Now" (BSSFN) methodology and the "Cupid" properties of software.

**Context:**

Your task is to perform a "BSSFN Audit." The core philosophy of BSSFN is to build the simplest thing that solves the current problem perfectly, while ensuring the system is easy to reason about, easy to change, and easy to replace when it no longer fits. BSSFN isn't just about "simple" code; it's about **reducing the cost of change** and **avoiding speculative generality.**

**Analysis Dimensions:**
Please evaluate the code against the following BSSFN criteria:

1. **Speculative Generality (YAGNI):** Identify any "just-in-case" code. Look for abstractions, interfaces with only one implementation, generic wrappers, or configuration options that serve no current requirement.
2. **Cognitive Load & Locality:** Is the logic "local"? Can a developer understand a component without traversing a deep tree of dependencies? Look for "shallow" modules vs. "deep" complex ones.
3. **Replaceability over Maintainability:** Is the code decoupled enough to be deleted and rewritten in a few hours? Or is it so tightly integrated that changing it requires a "surgical strike" across the repo?
4. **Standard vs. Custom:** Does the system use standard language features and well-known libraries, or has it reinvented wheels (custom frameworks, bespoke ORMs, complex DSLs) that increase the learning curve?
5. **Surface Area:** Is the public API of the modules minimal? Does it expose internal state or provide more "knobs" than necessary for the current use case?

**Further tips**

* **Look for "The Hook":** When reviewing the output, look specifically for what Dan North calls "The Hook." This is the part of the code that tries to be a "Platform" instead of a "Solution." If the AI identifies code that feels like a "framework," that’s your primary BSSFN violation.
* **Focus on Deletion:** Most code reviews focus on what to *add*. BSSFN is about what to *remove*. This prompt explicitly asks the AI to look for things that shouldn't be there.
* **Cost of Change:** It forces the AI to explain the *economic* impact of the code (cognitive load/maintenance), which is the heart of Dan North’s argument.
* **Replaceability:** It shifts the focus from "is this code clean?" to "is this code replaceable?"—a key distinction in the BSSFN philosophy.

**Output Format:**

Please provide a report structured as follows:

* **Executive Summary:** A high-level verdict on how well the BSSFN methodology is being followed (Scale: 1-10).
* **The "Gold" (BSSFN Wins):** List examples where the code is refreshingly simple and fit for purpose.
* **The "Weight" (Violations):** List specific instances of over-engineering, "clever" code, or premature abstractions. For each, explain the *future cost* of this complexity.
* **The "Refactor Path":** Provide 3-5 concrete recommendations to simplify the system to its "Best Simple" state.
