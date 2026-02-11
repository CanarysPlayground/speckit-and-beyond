# Level 5 — Quick Reference Card

## Advanced Commands

| Command | Purpose | Output |
|---------|---------|--------|
| `/speckit.analyze` | Validate cross-artifact consistency | Analysis report with issues and recommendations |
| `/speckit.checklist` | Create custom quality validation checklists | Checklist template in `.speckit/checklist.md` |

## Complete Command Reference

| Command | Required? | When | Time Cost | Value |
|---------|-----------|------|-----------|-------|
| `/speckit.constitution` | ✅ Yes | Project start | 5-10 min | Foundation for all decisions |
| `/speckit.specify` | ✅ Yes | Every feature | 5-15 min | Requirements clarity |
| `/speckit.clarify` | ⚠️ Optional | Complex features | 3-7 min | Edge case coverage |
| `/speckit.plan` | ✅ Yes | Every feature | 5-10 min | Technical design |
| `/speckit.tasks` | ✅ Yes | Every feature | 2-5 min | Actionable breakdown |
| `/speckit.analyze` | ⚠️ Optional | Complex/critical | 3-5 min | Consistency validation |
| `/speckit.checklist` | ⚠️ Optional | High-quality bar | 5 min | Quality enforcement |
| `/speckit.implement` | ✅ Yes | Every feature | 10-30 min | Code generation |

## Workflow Decision Tree

```
START: New Feature

├─ Is this the first feature?
│  ├─ YES → /speckit.constitution (once per project)
│  └─ NO → Continue
│
├─ Define requirements → /speckit.specify
│
├─ Complex domain? Many edge cases?
│  ├─ YES → /speckit.clarify
│  └─ NO → Skip clarify
│
├─ Mission-critical? Multi-feature dependencies?
│  ├─ YES → /speckit.analyze
│  └─ NO → Skip analyze
│
├─ Create technical plan → /speckit.plan
│
├─ Break down tasks → /speckit.tasks
│
├─ Strict quality requirements? Compliance?
│  ├─ YES → /speckit.checklist (validate spec against checklist)
│  └─ NO → Skip checklist
│
└─ Generate code → /speckit.implement

END: Feature complete, test, commit, deploy
```

## Analysis Report Interpretation

When you run `/speckit.analyze`, you get:

### Consistency Issues

| Issue Type | Example | Resolution |
|------------|---------|------------|
| **Data Model Conflict** | Spec references field not in existing model | Update model or remove from spec |
| **Performance Mismatch** | Spec says < 100ms, constitution says <50ms | Align with constitution or update constitution |
| **Dependency Undefined** | Spec assumes Feature X exists but doesn't | Add dependency note or implement Feature X first |
| **Naming Inconsistency** | Spec calls it "Recipe", code calls it "Dish" | Standardize terminology |

### Coverage Gaps

| Gap Type | Example | Fix |
|----------|---------|-----|
| **Missing Error Handling** | No spec for database failures | Add error handling section |
| **Undefined Edge Case** | What if search query is empty? | Add empty query behavior |
| **Unclear Semantics** | "Search pasta tomato" = both or either? | Clarify AND vs OR logic |
| **No Performance Target** | "Fast search" without metric | Add measurable target |

## Checklist Creation Patterns

### Quality Dimensions to Check

```markdown
## Specification Completeness
- [ ] User stories exist and follow format
- [ ] Functional requirements are testable
- [ ] Data model is fully specified
- [ ] Error handling covers failure modes

## Constitution Alignment
- [ ] Privacy requirements met
- [ ] Performance targets specified
- [ ] Quality standards defined
- [ ] Technical constraints honored

## Implementation Readiness
- [ ] Architecture is clear
- [ ] Dependencies documented
- [ ] Testing strategy defined
- [ ] Success criteria measurable
```

### Custom Checklist Example (Security-Focused)

```markdown
# Security Specification Checklist

## Input Validation
- [ ] All user inputs validated (type, length, format)
- [ ] SQL injection prevention (parameterized queries)
- [ ] File upload validation (size, type, malware scan)
- [ ] No eval() or exec() on user input

## Authentication & Authorization
- [ ] Authentication mechanism specified
- [ ] Role-based access control defined
- [ ] Session management specified
- [ ] Password policy documented

## Data Protection
- [ ] Sensitive data encrypted at rest
- [ ] Secure transmission (HTTPS)
- [ ] PII handling compliant (GDPR, CCPA)
- [ ] Data retention policy specified
```

## Iterative Feature Development

### Adding Feature to Existing Codebase

```powershell
# Merge previous feature to main
git checkout main
git merge 001-core-storage
git push origin main

# Create new feature branch
git checkout -b 002-new-feature

# Follow workflow
/speckit.specify    # New feature spec
/speckit.plan       # Extends existing architecture
/speckit.tasks      # Tasks to modify existing code
/speckit.implement  # Updates existing files + adds new ones

# Validate backward compatibility
pytest tests/       # All old tests still pass
# Manual test core features still work

# Commit new feature
git add .
git commit -m "feat: add [feature name]"
git push -u origin 002-new-feature
```

### Backward Compatibility Checklist

When adding features iteratively:

- [ ] Existing tests still pass
- [ ] Existing data models remain unchanged (or have migration)
- [ ] New code doesn't break old APIs
- [ ] Performance of existing features not degraded
- [ ] Configuration is backward compatible (defaults for new settings)
- [ ] Database schema changes are additive (no DROP COLUMN)

## Brownfield Spec-Driven Development

### Steps for Legacy Codebases

1. **Reverse-Engineer Current State**
   ```
   Analyze existing codebase and generate specification for current behavior
   ```

2. **Create Selective Constitution**
   ```
   Define principles for NEW features only:
   - Don't break existing functionality
   - Improve test coverage incrementally (Boy Scout Rule)
   - Document as you go
   - Refactor only when touching code
   ```

3. **Specify New Feature with Constraints**
   ```
   /speckit.specify
   
   New feature: [Name]
   
   CONSTRAINTS (Brownfield):
   - Cannot modify existing [module] (in use by other systems)
   - Must use existing [database/API] (no breaking changes)
   - Performance impact: < 5% degradation to existing features
   ```

4. **Plan Around Legacy**
   ```
   /speckit.plan
   
   Strategy:
   - Extend via new modules (minimize changes to legacy code)
   - Adapter pattern to integrate with legacy interfaces
   - Feature flags to enable gradually
   ```

5. **Incremental Implementation**
   ```
   /speckit.implement
   
   - Create new modules first
   - Touch legacy code last (minimal changes)
   - Add tests to legacy code as you modify it
   - Document legacy behavior you discover
   ```

### Boy Scout Rule for Brownfield

"Leave the code better than you found it"

| When You Touch | What to Do |
|----------------|-----------|
| **Legacy function** | Add docstring, type hints |
| **Untested code** | Add unit tests (even 1-2) |
| **Hardcoded value** | Extract to config |
| **Long function** | Extract one helper function |
| **Cryptic name** | Rename variable/function |
| **Missing error handling** | Add try/catch with logging |

Don't refactor unrelated code (scope creep), only what you touch.

## Constitution Evolution Triggers

### When to Update Constitution

| Trigger | Example | Action |
|---------|---------|--------|
| **Implementation reveals conflict** | Performance target too strict/loose | Adjust target based on data |
| **New pattern emerges** | All features need backward compatibility | Add as principle |
| **Team grows** | More developers, need standards | Add code review guidelines |
| **Requirements change** | Business pivots to enterprise | Update privacy, compliance principles |
| **Technology shift** | Moving from local to cloud | Update architecture constraints |

### Constitution Update Process

```markdown
1. Identify issue (conflict, gap, outdated constraint)
2. Discuss with team (if applicable)
3. Draft update to constitution.md
4. Validate against existing features (do they still align?)
5. Commit with explanation:
   git commit -m "docs: update constitution - [reason]"
6. Communicate to team
7. Apply to future features
```

## Workflow Variations

### MVP/Prototype (Speed over quality)
```
Constitution (minimal) → Specify → Plan → Tasks → Implement
```
Skip: clarify, analyze, checklist  
Time: 1-2 hours per feature

### Standard Feature (Balanced)
```
Constitution → Specify → Clarify → Plan → Tasks → Implement
```
Skip: analyze, checklist (unless needed)  
Time: 3-6 hours per feature

### Mission-Critical (Quality over speed)
```
Constitution → Specify → Clarify → Analyze → Plan → Tasks → Checklist → Implement
```
Skip: Nothing  
Time: 1-2 days per feature

### Brownfield Addition
```
Reverse-engineer spec → Selective constitution → Specify (with constraints) → 
Plan (around legacy) → Tasks → Implement (incrementally)
```
Time: Varies (2x-3x greenfield due to legacy complexity)

## Common Mistakes and Fixes

| Mistake | Problem | Fix |
|---------|---------|-----|
| **Over-planning** | Spec too detailed, takes longer than coding | Focus on outcomes, not implementation detail |
| **Under-planning** | Vague spec leads to rework | Use `/speckit.clarify` to explore ambiguities |
| **Skipping validation** | Issues found during QA | Run `/speckit.analyze` before implementation |
| **Ignoring constitution** | Features deviate from principles | Reference constitution in spec, validate alignment |
| **No iteration** | Build everything at once | Break into features, implement incrementally |
| **Abandoning specs** | Specs outdated after implementation | Update specs when code changes |

## Quality Metrics

### Spec Quality Indicators

| Metric | Good | Needs Work |
|--------|------|------------|
| **Clarity** | No ambiguous terms, measurable criteria | "Fast", "user-friendly", "robust" without metrics |
| **Completeness** | All edge cases documented | "TBD", "TODO", missing sections |
| **Consistency** | Aligns with constitution and other specs | Conflicts in performance, architecture |
| **Testability** | Acceptance criteria are verifiable | Subjective criteria ("looks good") |
| **Implementation-agnostic** | No tech names (SQL, React) | Specifies tools, not outcomes |

### Implementation Quality Indicators

| Metric | Target | Measurement |
|--------|--------|-------------|
| **Test Coverage** | 80%+ total, 95%+ models | `pytest --cov` |
| **Type Safety** | 0 mypy errors | `mypy` |
| **Code Quality** | 0 critical lint errors | `ruff check` |
| **Performance** | Meets spec targets | Custom benchmarks |
| **Spec Alignment** | All requirements implemented | Manual validation checklist |
| **Backward Compatibility** | Old tests pass | `pytest` after new feature |

## Custom AI Enhancements (Exercise 7)

### Instruction Files

**Purpose:** Teach GitHub Copilot domain-specific knowledge

**File Location:**
```
.github/instructions/
  ├── domain-name.instructions.md
  ├── api-design.instructions.md
  └── security.instructions.md
```

**Structure Template:**
```markdown
# [Domain Name] Instructions

## Domain Overview
Brief description of problem space

## Core Concepts
Key terminology and definitions

## Code Generation Guidelines
Patterns and practices for this domain

## Best Practices
Common patterns and pitfalls to avoid

## Terminology Guide
Domain vocabulary
```

**When to Use:**
- ✅ Specialized domain (medical, legal, finance)
- ✅ Domain-specific validation rules
- ✅ Team needs consistent AI behavior
- ❌ Generic CRUD operations
- ❌ Solo short-term projects

### Custom Agent Skills

**Purpose:** Create specialized AI assistants for workflows

**File Location:**
```
.github/skills/
  ├── agent-name/
  │   └── SKILL.md
  ├── database-expert/
  │   └── SKILL.md
  └── security-reviewer/
      └── SKILL.md
```

**Structure Template:**
```markdown
# Agent Name

## Your Role
Agent's purpose and personality

## Expertise Areas
Topics the agent knows deeply (3-5 areas)

## Interaction Patterns
How to respond in different scenarios

## Response Guidelines
Structure, tone, and format

## Common Questions
FAQs with detailed answers

## Example Opening
What to say when invoked
```

**Invocation:**
```
@AgentName [your question or request]
```

**When to Use:**
- ✅ Specialized methodology (TDD, DDD, SDD)
- ✅ Team onboarding and training  
- ✅ Consistent coaching needed
- ❌ Generic coding questions
- ❌ Solo developer with deep expertise

### Examples from Workshop

**Instruction File:** `.github/instructions/recipe-domain.instructions.md`
- Teaches recipe terminology (ingredients, measurements)
- Provides conversion formulas (cups to ml)
- Defines validation rules (dietary restrictions)

**Agent Skill:** `.github/skills/speckit-coach/SKILL.md`
- Guides through Spec-Driven Development workflow
- Answers methodology questions
- Validates understanding at checkpoints

### Creating Your Own

**Quick Start:**
```
# For instruction file:
Based on .github/instructions/recipe-domain.instructions.md, create a template for [YOUR DOMAIN].

# For agent skill:
Based on .github/skills/speckit-coach/SKILL.md, create an agent skill for [YOUR AGENT PURPOSE].
```

**Benefits:**
- Improved AI code generation accuracy
- Consistent behavior across team
- Guided learning for new developers
- Transferable to other projects

## Resources

- **Spec Kit Docs:** https://github.github.io/spec-kit/
- **Spec-Driven Guide:** https://github.com/github/spec-kit/blob/main/spec-driven.md
- **Community Examples:** https://github.com/github/spec-kit/tree/main/examples
- **Discussions:** https://github.com/github/spec-kit/discussions
- **Video Tutorial:** https://www.youtube.com/watch?v=a9eR1xsfvHg

## Workshop Completed! 🎉

You've mastered Spec-Driven Development with GitHub Spec Kit:
- ✅ Setup and constitution
- ✅ Specification and clarification
- ✅ Planning and task breakdown
- ✅ Implementation and validation
- ✅ Advanced analysis and iteration

**Next Steps:**
1. Apply to real projects
2. Share with your team
3. Contribute to Spec Kit community

Thank you for completing this workshop!
