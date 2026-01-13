# Prompts

I want to write some prompts for creating and/or reviewing some documents. Take ADRs as an example. As a use case the user may give the prompt an already written basic ADR and the LLM will refine and critize it or the user just gives an idea description/a problem statement and the LLM creates the initial version of the ADR.

Short answer: **Yes, you *can* do it with one prompt**, and modern models (like GPT-5.1 or o-series) are absolutely capable of reliably switching between *creation* and *review/refinement* modes — **if you structure the meta-prompt carefully**.

But:
**You will get *more predictable* and *more controllable* results with two prompts**, especially in enterprise settings or when you want repeatable output quality (like ADRs).

Below is the detailed reasoning and recommended patterns.

---

# ✅ Should You Use One Prompt or Separate Prompts?

## **Option A — One universal prompt**

Modern LLMs can absolutely follow a single instruction like:

> “If input contains a draft ADR → refine it.
> If input contains an idea/problem → create a full ADR.”

This *works*, and the model can detect context reliably.
**But reliability varies by:**

* user input quality
* how incomplete the starting material is
* whether you need consistent structure (ADR templates)
* whether you need strict compliance (e.g., mandatory sections)

### **When is one prompt OK?**

* You don’t need strict formatting guarantees
* The audience is internal
* The user is skilled (knows what to provide)
* You want fewer moving pieces

### **Risk with one prompt**

The model may:

* hallucinate missing context without asking questions
* refine something when you actually wanted a rewrite
* create a new ADR when the user expected critique only
* skip the clarification step because it “thinks” it knows enough

LLMs are extremely good at pattern inference — sometimes *too good*.

---

## **Option B — Separate prompts**

This is usually the better architectural choice if you want *reliable*, *repeatable*, *auditable* output.

You create:

### **1) “Create ADR” prompt**

Given context/problem → produce ADR with required structure.
Always asks clarifying questions if information is missing.

### **2) “Refine ADR” prompt**

Given an existing ADR → improve clarity, fix structure, identify gaps.

### **3) (Optional) “Review ADR” prompt**

Provide point-by-point critique, score quality, propose improvements.

### Why separate prompts?

Because **intent is unambiguous**, so the model:

* won’t misinterpret incomplete drafts as notes → create wrong ADR
* won’t rewrite something when only critique was expected
* won’t assume missing info
* won’t skip the completeness/consistency check step

This gives much more predictable outputs in real consulting work.

---

# 🧠 Why this matters with current LLMs

Modern models are:

* extremely capable
* extremely contextual
* but sometimes too eager to “helpfully” fill gaps

When prompts serve different *modes* of operation (create / refine / critique), separating them creates **hard rails**.

Especially for ADRs you want:

* stable template structure
* correct use of “Decision vs Context”
* consistency across documents
* high signal/low noise

---

# 🧩 Recommended approach (best of both worlds)

You *can* use one prompt, but make it **mode-explicit and mode-switchable**.

### Pattern:

```
# Role
You are an ADR assistant.

# Modes (choose exactly one based on user input)
1. CREATE – If the user provides a problem or idea.
2. REFINE – If the user provides a draft ADR.
3. REVIEW – If the user explicitly asks for critique.

# Rules
- If information is missing → ask clarifying questions BEFORE writing.
- Follow the ADR template exactly.
- Be explicit about which mode you chose.
```

This gives:

* reliability of separated prompts
* convenience of a single prompt

But you must enforce the mode selection logic.

---

# 🏁 Recommendation for your Thoughtworks-style consulting work

Given your usage patterns (ADR standards, architecture reviews, tech radars, client docs):

### **Use separate prompts for CREATE and REFINE/REVIEW.**

* You get cleaner version control
* Testability is higher
* CI/CD pipelines can run both prompts independently
* You avoid conflating critique with rewriting
* ADR changes become more auditable

But also…

### **Create a “unified mode prompt” as a convenience tool for casual everyday work.**

That’s the pattern I see many senior consultants adopt:

* 2–3 specialized prompts for predictable output
* 1 unified prompt for day-to-day ad-hoc use

## Single Prompt vs. Multiple Prompts for ADR Creation and Refinement

Based on current research and best practices with modern LLMs (GPT-4, Claude 4, etc.), the answer depends on your priorities, but **separate prompts generally provide better reliability and quality** for document workflows like ADRs.

### Key Research Findings

**Single-task prompts consistently outperform multi-task prompts for accuracy**. Research from Sun Yat-sen University and Carnegie Mellon University confirms that prompts focusing on one task at a time produce higher-quality results. Multi-task prompts risk "cognitive overload," causing models to neglect or misinterpret parts of instructions, resulting in less accurate outputs.[1]

**Prompt chaining excels for complex, multi-stage tasks**. Breaking down tasks into separate prompts allows for iterative refinement and higher-quality outputs. This approach provides greater flexibility and precision, with each step having a specific focus. Studies have shown that prompt chaining can enable smaller models like GPT-3.5 to perform comparably to GPT-4 by utilizing structured, sequential prompts.[2][3][4]

**Modern LLMs can handle conditional logic within a single prompt, but with limitations**. While GPT-4 and Claude 4 are capable of detecting user intent and adjusting behavior accordingly, their performance deteriorates when tasks involve significantly different requirements or varying complexity levels.[5][6][7][1]

### Practical Considerations for ADR Workflows

**For Creation vs. Refinement Tasks**

ADR creation and refinement are fundamentally different operations:

- **Creation** requires generating structure, identifying alternatives, and articulating decisions from scratch
- **Refinement** requires critiquing existing content, identifying gaps, and suggesting improvements

Research on architectural decision records specifically shows that LLMs can generate ADRs but require focused approaches for quality output. Studies using GPT-4 and fine-tuned models demonstrate that while zero-shot prompting can generate relevant ADRs, the quality improves significantly with task-specific approaches.[8][9]

**Single Prompt Approach: When It Works**

A unified prompt could work reasonably well if:

- You explicitly structure conditional logic: "If the user provides an existing ADR, critique and refine it. If the user provides only a problem statement, create a new ADR"[10][5]
- Your use cases have similar complexity levels[1]
- You're using top-tier models (Claude Opus 4, GPT-4.5) with strong instruction-following[11][12]
- Efficiency and token usage are critical concerns[13]

However, even with these conditions, you'll likely experience:
- Reduced accuracy compared to specialized prompts[1]
- Less consistent outputs across different scenarios[1]
- Difficulty troubleshooting when one mode underperforms[14]

**Separate Prompts Approach: Recommended**

Multiple specialized prompts offer clear advantages:

1. **Higher Quality**: Each prompt can be optimized for its specific task, with tailored instructions, examples, and evaluation criteria[3][1]

2. **Better Debugging**: When refinement quality drops, you can improve that prompt without affecting creation[14]

3. **Clearer Context**: Creation prompts can include examples of well-structured ADRs, while refinement prompts can include examples of constructive criticism[3]

4. **Iterative Improvement**: You can test and refine each workflow independently[4]

5. **Scalability**: Adding new modes (e.g., "comparison between two ADRs" or "converting legacy decision docs to ADR format") becomes straightforward[14]

### Implementation Recommendation

**Use a routing layer with specialized prompts**. Modern AI architectures commonly employ dynamic routing that:[15][16][17]

1. Detects the input type (existing ADR vs. problem statement)
2. Routes to the appropriate specialized prompt
3. Maintains consistent output formatting

This approach combines the user experience benefits of a unified interface with the quality advantages of specialized prompts.[16][17]

**Example Architecture:**

```
User Input → Intent Classifier → Route to:
  ├─ Creation Prompt (for new ADRs)
  ├─ Refinement Prompt (for existing ADRs)
  └─ Critique-Only Prompt (for review without changes)
```

The classifier can be:
- A lightweight LLM call that categorizes the input[15]
- A semantic similarity search against example inputs[15]
- Simple rule-based logic (presence of ADR structure elements)[17]

### Hybrid Approach: Best of Both Worlds

For ADR workflows specifically, consider this pattern used successfully in architectural decision documentation:[18][19]

1. **Initial Detection**: Use a brief classifier prompt to determine task type
2. **Specialized Execution**: Route to creation or refinement prompt
3. **Human-in-the-Loop**: Present output for architect review[19]
4. **Iterative Refinement**: Allow architects to provide feedback, which routes through refinement prompt[20]

This mirrors the "ART: Ask, Refine, and Trust" methodology, where smaller specialized models make decisions about when and how to refine outputs.[20]

### Cost-Benefit Analysis

**Separate Prompts:**
- **Pros**: 30-50% better accuracy on complex tasks, easier maintenance, better reliability[1]
- **Cons**: Multiple API calls, higher latency, slightly higher token usage

**Single Prompt:**
- **Pros**: Single API call, lower latency, reduced token usage (30-82% in some scenarios)[13]
- **Cons**: Lower accuracy, harder to debug, performance degradation with complexity

For ADR workflows where quality and reliability are paramount, the accuracy benefits of separate prompts outweigh the efficiency gains of a unified prompt.[3][1]

### Current LLM Capabilities (Late 2025)

Models like Claude Opus 4 and GPT-4.5 show impressive multi-tasking abilities, with Claude 4 scoring 62-70% on complex coding benchmarks and demonstrating strong extended reasoning capabilities. However, even these advanced models benefit from focused, single-task prompts when precision is critical.[12][21][11][1]

### Conclusion

**For ADR creation and refinement, use separate prompts**. The reliability, quality, and maintainability benefits significantly outweigh the minor efficiency costs. Modern LLMs can technically handle both tasks in one prompt, but research consistently shows that specialized prompts deliver superior results for workflows requiring different cognitive modes.[4][3][1]

If you need a unified user interface, implement a simple routing layer that detects intent and dispatches to the appropriate specialized prompt. This gives you both the user experience of a single entry point and the quality advantages of task-specific optimization.[16][17][15]

[1](https://yourway.substack.com/p/single-task-vs-multi-task-prompting)
[2](https://www.reddit.com/r/PromptEngineering/comments/1eolncx/has_prompt_chaining_been_proven_to_work_better/)
[3](https://www.airops.com/blog/prompt-chaining-vs-chain-of-thought)
[4](https://hiringnet.com/prompting-for-multi-step-processes-stepwise-vs-prompt-chaining)
[5](https://www.linkedin.com/pulse/programming-words-developers-view-prompt-engineering-mark-gerow-37tgc)
[6](https://aclanthology.org/2025.findings-acl.995.pdf)
[7](https://docsbot.ai/prompts/support/user-intent-recognition)
[8](https://papers.ssrn.com/sol3/Delivery.cfm/80518f54-ee25-4a4f-9bb1-c519ee13c46a-MECA.pdf?abstractid=5217607&mirid=1)
[9](https://arxiv.org/html/2403.01709v1)
[10](https://www.reddit.com/r/LLMDevs/comments/1mtvktq/what_is_the_best_way_to_include_conditional/)
[11](https://felloai.com/2025/05/we-tested-claude-4-gpt-4-5-gemini-2-5-pro-grok-3-whats-the-best-ai-to-use-in-may-2025/)
[12](https://www.datastudios.org/post/claude-4-in-2025-performance-safety-benchmarks-ecosystem-news-and-real-world-impact-for-enterpr)
[13](https://arxiv.org/html/2406.10786v3)
[14](https://docs.retellai.com/build/single-multi-prompt/prompt-overview)
[15](https://github.com/aws-samples/sample-multi-llm-dynamic-prompt-routing)
[16](https://www.emergentmind.com/topics/llm-based-prompt-routing)
[17](https://aws.amazon.com/blogs/machine-learning/multi-llm-routing-strategies-for-generative-ai-applications-on-aws/)
[18](https://www.linkedin.com/pulse/agentic-ai-living-architecture-enhancing-adrs-llms-decision-khan-5gpgc)
[19](https://www.salesforce.com/blog/architectural-decisions-human-led-ai-powered-approach/)
[20](https://aclanthology.org/2024.naacl-long.327.pdf)
[21](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices)
[22](https://www.promptingguide.ai/introduction/examples)
[23](https://intuitionlabs.ai/pdfs/nlp-for-prescribing-information-an-evidence-based-review.pdf)
[24](https://www.reddit.com/r/ClaudeAI/comments/1ljv2kz/tips_for_developing_large_projects_with_claude/)
[25](https://www.tavus.io/post/llm-prompt)
[26](https://www.youtube.com/watch?v=Q2HxSfS6ADo)
[27](https://www.claude.com/blog/best-practices-for-prompt-engineering)
[28](https://www.prompthub.us/blog/everything-you-need-to-know-about-claude-4-5)
[29](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-the-openai-api)
[30](https://garrettlanders.com/prompt-engineering-guide-2025/)
[31](https://towardsdatascience.com/become-a-better-data-scientist-with-these-prompt-engineering-hacks/)
[32](https://news.ycombinator.com/item?id=44779428)
[33](https://geniusee.com/single-blog/prompt-engineering-best-practices)
[34](https://www.anthropic.com/news/claude-4)
[35](https://developersvoice.com/blog/ai/agent-driven-tech-specs/)
[36](https://www.supercharge.io/us/blog/ai-prompt-engineering-best-practices)
[37](https://github.com/macromania/adr-agent)
[38](https://simonwillison.net/2025/May/25/claude-4-system-prompt/)
[39](https://www.reddit.com/r/LocalLLaMA/comments/1fsj4ww/has_prompt_chaining_been_proven_to_work_better/)
[40](https://www.cometapi.com/is-claude-better-than-chatgpt-for-coding-in-2025/)
[41](https://www.promptingguide.ai/techniques/prompt_chaining)
[42](https://p0stman.com/guides/ai-model-selection-guide-claude-gpt4-gemini-2025.html)
[43](https://yourgpt.ai/blog/general/prompt-chaining-vs-chain-of-thoughts)
[44](https://arxiv.org/html/2506.06069v1)
[45](https://aclanthology.org/2023.findings-acl.27.pdf)
[46](https://skimai.com/10-best-prompting-techniques-for-llms-in-2025/)
[47](https://latitude-blog.ghost.io/blog/guide-to-multi-model-prompt-design-best-practices/)
[48](https://www.cs.cmu.edu/~sherryw/assets/pubs/2025-underspec.pdf)
[49](https://openaccess.thecvf.com/content/ICCV2025/papers/Liu_Unified_Open-World_Segmentation_with_Multi-Modal_Prompts_ICCV_2025_paper.pdf)
[50](https://futureagi.com/blogs/ai-prompting-llm-2025)
[51](https://docs.kore.ai/xo/automation/dynamic-routing/)
[52](https://www.sciencedirect.com/science/article/pii/S2667305325000249)
[53](https://latitude-blog.ghost.io/blog/template-syntax-basics-for-llm-prompts/)
[54](https://www.linkedin.com/posts/how-to-ai-guide_claude-4s-system-prompt-just-leaked-this-activity-7333099684892143617-tRu0)
[55](https://aclanthology.org/2024.lrec-main.863.pdf)
[56](https://www.reddit.com/r/ClaudeAI/comments/1evwv58/archive_of_injections_and_system_prompts_and/)
[57](https://clickup.com/p/ai/prompts/handling-project-documentation)
[58](https://techinfotech.tech.blog/2025/06/09/best-practices-to-build-llm-tools-in-2025/)
[59](https://www.anainesurrutia.com/post/using-ai-hub-document-outputs-in-prompts)
[60](https://www.designveloper.com/blog/advanced-rag/)
[61](https://docs.aws.amazon.com/prescriptive-guidance/latest/agentic-ai-patterns/llm-workflows.html)
[62](https://promptdrive.ai/how-prompt-review-systems-improve-team-ai-workflows/)
[63](https://arxiv.org/pdf/2509.08646.pdf)
[64](https://www.aidocmaker.com/blog/how-to-optimize-prompts-for-ai-document-generation)
