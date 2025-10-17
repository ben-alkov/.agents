---
description: this command analyzes codebase architecture, patterns, and design quality
argument-hint: [directory] [--focus=architecture|dependencies|scalability|security|testing]
allowed-tools: Read, Grep, Glob
---

# /architecture-reviews

Analyze codebase architecture to assess maintainability, scalability, and design
quality.

## Scope

Arguments: `[directory] [--focus=AREA]`

- No arguments: Review entire repository
- Directory: Scope review to specific path
- Focus areas: architecture, dependencies, scalability, security, testing

## Analysis Process

1. **Map the structure:** Identify entry points, core modules, external
   boundaries (config, persistence, API, UI)

2. **Identify patterns:** Note architectural style (layered, hexagonal,
   microservices, etc.) and consistency

3. **Trace dependencies:** Map module relationships, check for circular
   dependencies, assess coupling

4. **Assess key risks:**
   - Security: Authentication boundaries, input validation, data protection
   - Scalability: Stateless design, caching, resource management
   - Coupling: Tight dependencies, unclear boundaries, god objects

5. **Evaluate maintainability:**
   - Test architecture and coverage
   - Documentation quality
   - Extension points and flexibility
   - Error handling consistency

For focused reviews (`--focus`), prioritize that dimension but flag critical
issues in other areas.

## Output Format

Deliver findings as:

1. **Executive summary:** 3-5 sentence overview of architectural health

2. **Key findings:** Top 3-5 issues with specific file/line references
   - What: Concrete problem description
   - Why: Impact on maintainability/scalability/security
   - How: Specific remediation approach

3. **Strengths:** 2-3 well-designed aspects worth preserving

4. **Recommendations:** Prioritized improvements
   - Critical: Security risks, data corruption potential, blocking issues
   - High: Significant coupling, missing tests, unclear boundaries
   - Low: Style consistency, documentation gaps, minor refactoring

Use code snippets to illustrate problems. Explain rationale for each
recommendation.
