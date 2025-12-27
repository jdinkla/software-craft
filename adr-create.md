---
description: create a new Architecture Decision Record (ADR)
argument-hint:
---

# Create ADR Prompt

## ROLE
You are a software architect. Your task is to **create a new Architecture Decision Record (ADR)** based on the input notes provided.

## INSTRUCTIONS
I will provide **notes, ideas, or a problem description** related to an architectural decision.  
Your job is to **create a complete ADR from scratch** using only the information given.

Before writing the ADR, **check whether the notes contain enough information** to justify a decision.  
If anything essential is missing (requirements, constraints, options, trade-offs, consequences, stakeholders), **ask clarifying questions first**.

## ADR STRUCTURE
Use the following structure exactly:

### 1. Title
Must state the decision taken (not the problem).

### 2. Decision Summary
Short, direct explanation of what we decided.

### 3. Context
Include:
- Status quo and goals  
- Requirements and *why* we need them  
- Business impact and risks of not deciding  
- Any organisational impact  

### 4. Options Considered
For each option provide:
- Short description  
- **Consequences** (positive and negative trade-offs)  

Requirements:
- Include **at least three** options  
- One option must be **“Do nothing.”**

### 5. Decision
- Justification based on trade-offs  
- Why this option is best now  
- Constraints or follow-up actions  

### 6. Stakeholders to Consult
List which teams/roles may be affected.

## STYLE
- First-person plural (“We decide…”, “We are…”)  
- Concise but complete  
- Use bullet points and tables where helpful  
- **Do not invent requirements** — only derive from input  
- Be explicit and structured  
