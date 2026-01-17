# Contributing to SOPCOS

Thank you for your interest in contributing to SOPCOS.

SOPCOS is not a feature-driven software project.
It is a **governance-first protocol** where changes are evaluated
through **law, liability, and determinism** before code or convenience.

This document explains **how** to contribute,
**what kind of contributions are welcome**,
and **which boundaries are intentionally strict**.

---

## Core Principle

> **In SOPCOS, protocol law precedes implementation.**  
> If a change cannot be expressed, justified, and reviewed as a SIP,
> it cannot be merged.

---

## What You Can Contribute

We welcome contributions in the following categories:

### 1️⃣ SOPCOS Improvement Proposals (SIPs)

This is the **primary and preferred** contribution path.

You may propose:
- New protocol semantics
- Clarifications or revisions to existing SIPs
- Security, safety, or liability improvements
- New standards-track or informational SIPs

All substantive protocol changes **must** go through the SIP process.

See: `SIPs/README.md`

---

### 2️⃣ Documentation Improvements (Non-Normative)

You may submit PRs for:
- Typo fixes
- Editorial clarity
- Improved explanations or examples
- Diagrams and non-normative references

**Constraint:**  
Documentation changes must not alter normative meaning.

If a documentation change *implicitly changes behavior*,
it must be proposed as a SIP.

---

### 3️⃣ Governance & Process Feedback

We welcome:
- Clarifications to governance text
- Process improvements
- Lifecycle documentation improvements

Governance documents are treated as **constitutional references**.
Substantive changes may require structured review.

---

## What We Do NOT Accept

The following contributions will be closed without review:

- ❌ Feature requests submitted as code
- ❌ Pull requests that bypass the SIP process
- ❌ “Just add this” optimizations
- ❌ Drive-by refactors
- ❌ Performance tweaks without governance context
- ❌ AI autonomy expansions without explicit authority analysis

> SOPCOS is designed to move slowly where mistakes are irreversible.

---

## The SIP-First Workflow

### Step 1 - Start with a SIP

Before writing code or proposing behavior changes:

1. Create a SIP draft in `SIPs/`
2. Clearly state:
   - Motivation
   - Scope
   - Safety and liability implications
   - Backward compatibility
   - Security considerations
3. Mark the SIP status as **DRAFT**

---

### Step 2 - Discussion Before Implementation

- SIPs are discussed **before** implementation
- Consensus focuses on:
  - Determinism
  - Safety monotonicity
  - Authority boundaries
  - Liability clarity

Code without accepted governance rationale will not be merged.

---

### Step 3 - Review & Ratification

Depending on scope:
- Editorial changes may be merged after review
- Protocol changes require formal ratification
- Breaking changes require higher scrutiny

See:
- `governance/GOVERNANCE.md`
- `SIPs/SIP_LIFECYCLE.md`

---

## Pull Request Rules

All PRs must:

- Reference a SIP (if applicable)
- Clearly state whether the change is:
  - Non-breaking
  - Breaking
- Include a short **Risk & Compatibility** note
- Avoid mixing unrelated changes

PRs that mix governance, semantics, and refactors
will be asked to split or will be closed.

---

## Issues & Discussions

You may open issues to:
- Ask clarification questions
- Discuss conceptual concerns
- Identify ambiguity or risks

Issues should **not** be used for:
- Feature requests
- Roadmap demands
- Support questions

SOPCOS is a protocol specification,
not a customer support channel.

---

## Code Contributions (Future Scope)

Reference implementations may exist in the future.
Until explicitly declared:

- No implementation is canonical
- No codebase defines SOPCOS compliance
- “Working code” does not override protocol law

Compliance is defined by **behavior**, not by repository.

---

## Code of Conduct

We expect contributors to:
- Argue from evidence, not authority
- Respect safety-critical constraints
- Avoid hype-driven language
- Treat governance disagreements seriously and professionally

Strong disagreement is acceptable.
Ambiguity is not.

---

## Final Note

> SOPCOS is designed for systems
> where failure has real-world consequences.

If this contribution process feels restrictive,
that is intentional.

Thank you for contributing responsibly.
 