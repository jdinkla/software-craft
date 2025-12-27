# Prompts and Skills

A collection of specialized prompt templates for AI agents to assist in high-level software engineering, architecture, and maintenance tasks.

## Overview

This repository contains structured Markdown prompts designed to turn LLMs into expert-level engineering assistants. These templates follow professional standards for Architecture Decision Records (ADRs), Domain-Driven Design (DDD), technical debt analysis, and more.

## Available Prompts

### Architecture & Decisions
- **[adr-create.md](adr-create.md)**: Create a new ADR from scratch based on notes or problem descriptions.
- **[adr-refine.md](adr-refine.md)**: Improve and polish an existing ADR.
- **[adr-review.md](adr-review.md)**: Review an ADR for clarity, trade-offs, and completeness.
- **[arch-review-kotlin.md](arch-review-kotlin.md)**: Specialized architecture review for Kotlin-based projects.

### Domain Analysis
- **[ddd-analyse.md](ddd-analyse.md)**: Extract a domain model (Entities, Value Objects, Aggregates) from a legacy system.
- **[bssfn-analyse.md](bssfn-analyse.md)**: Analyze business functions within the repository.

### Maintenance & Quality
- **[tech-debt.md](tech-debt.md)**: Scan a repository to identify and prioritize technical debt.
- **[test-coverage.md](test-coverage.md)**: Analyze test coverage and identify critical gaps.
- **[test-naming.md](test-naming.md)**: Review and improve test naming conventions.

### Meta / Quality Control
- **[check-agents-md.md](check-agents-md.md)**: Verify if the recommendations in `AGENTS.md` are being followed.

## Usage

These prompts can be used by copying the content into your AI chat interface or by using them as system prompts for automated agents. Most prompts expect a specific role and provide a clear structure for the output.
