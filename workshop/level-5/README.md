# Level 5: Advanced Features — Analysis, Iteration, and Brownfield

> **Risk level:** 🟠 Medium — This level involves modifying existing code and analyzing complex scenarios.

## Learning Objectives

By the end of this level, you will be able to:

1. Use `/speckit.analyze` for cross-artifact consistency checking
2. Create quality checklists with `/speckit.checklist`
3. Add features iteratively to existing codebases
4. Apply Spec-Driven Development to brownfield projects
5. Refine specifications based on implementation learnings
6. Balance greenfield perfection with pragmatic iteration
7. Manage technical debt while adding features
8. Create custom quality gates for your projects
9. Understand when to use optional Spec Kit commands
10. Master the full Spec-Driven Development lifecycle

---

## Prerequisites

- [ ] Completed **Level 4** (Core Recipe Storage is implemented and working)
- [ ] On branch `main` or `001-core-recipe-storage`
- [ ] All tests passing, coverage ≥ 80%
- [ ] GitHub Copilot active in VS Code

---

## Workshop Structure

This level contains **7 exercises**. Estimated time: **30 minutes**.

| Exercise | Topic | Time |
|----------|-------|------|
| 1 | Analyze Specification Consistency | 4 min |
| 2 | Create Quality Checklists | 3 min |
| 3 | Add Second Feature Iteratively | 7 min |
| 4 | Refine Constitution Based on Learnings | 2 min |
| 5 | Brownfield Spec-Driven Development | 3 min |
| 6 | Master the Complete Workflow | 1 min |
| 7 | Create Custom Instructions & Agents (Optional) | 10 min |

---

## Understanding Advanced Commands

### Optional vs. Core Commands

**Core Workflow (Levels 1-4):**
```
Constitution → Specify → Plan → Tasks → Implement
```

**Optional Enhancement Commands:**
- `/speckit.analyze` — Validate specs before implementation
- `/speckit.checklist` — Custom quality gates
- `/speckit.clarify` — Deep dive on ambiguities (covered in Level 2)

Use optional commands when:
- Building mission-critical features (healthcare, finance)
- Large team needs alignment validation
- Complex domain with many edge cases
- Quality bar is exceptionally high

Skip optional commands when:
- Prototype or MVP
- Solo project with flexible scope
- Simple, well-understood features
- Time-constrained development

---

## Exercise 1: Analyze Specification Consistency

### Goal
Use `/speckit.analyze` to validate cross-artifact consistency before adding a new feature.

### Steps

**1.1** Create a second feature branch:

Merge Core Recipe Storage to main (or keep it separate):
```powershell
git checkout main
git merge 001-core-recipe-storage
git push origin main
```

Create new feature branch for Recipe Search:
```powershell
git checkout -b 002-recipe-search
```

**1.2** Create a draft specification for Recipe Search:

Create `.speckit/features/002-recipe-search/spec.md` with basic requirements:
```markdown
# Recipe Search Specification

## Overview
Enable users to search recipes by title, ingredients, or cuisine type.

## User Stories
- As a home cook, I want to search recipes by ingredient so I can use what I have in my fridge
- As a home cook, I want to search by title so I can quickly find a specific recipe

## Functional Requirements
- Full-text search across title and ingredients
- Search results ranked by relevance
- Search performance < 100ms for 10,000 recipes

## Data Model Changes
- Add `search_index` table for full-text search (SQLite FTS5)
- Add `cuisine_type` field to Recipe model (optional, enum)

## Dependencies
- Depends on Core Recipe Storage (001)
- Requires recipes table to exist
```

**1.3** Run analysis command:

```
/speckit.analyze
```

**1.4** Copilot analyzes across all artifacts:

Analysis checks:
- ✅ Spec references existing features correctly (001 Core Storage)
- ⚠️ **Inconsistency:** spec.md mentions `cuisine_type` field but 001's data model doesn't have it
- ⚠️ **Inconsistency:** Performance target is < 100ms, but constitution.md says < 50ms for search
- ✅ Specification completeness: User stories, requirements, acceptance criteria present
- ⚠️ **Gap:** No error handling section for search failures
- ⚠️ **Gap:** No mention of partial matches vs. exact matches

**1.5** Review analysis report:

Copilot generates:
```markdown
# Specification Analysis Report — Recipe Search

## Consistency Issues
1. **Data Model Conflict:** spec.md references `cuisine_type` field not in 001/models.py
   - Resolution: Either add to Core Storage or remove from Search spec
2. **Performance Mismatch:** Spec says < 100ms, constitution says < 50ms for search
   - Resolution: Align with constitution (< 100ms is for search, not retrieval)

## Coverage Gaps
1. **Error Handling:** No specification for search failures (database errors, invalid queries)
2. **Search Semantics:** Unclear if "pasta tomato" searches for recipes with BOTH or EITHER
3. **Empty Results:** No specification for UI when search returns 0 results

## Recommendations
1. Add error handling section to spec.md
2. Clarify search query semantics (AND vs. OR)
3. Update data model in 001 or remove cuisine_type from 002
4. Add empty result UX specification
```

**1.6** Resolve issues before planning:

```
Update spec.md for Recipe Search:
1. Remove cuisine_type (future feature: "Recipe Categories")
2. Clarify: "pasta tomato" searches for recipes containing both ingredients (AND logic)
3. Add error handling: malformed queries return empty results + log error
4. Add empty results UX: "No recipes found. Try different keywords."
```

**1.7** Re-run analysis:

```
/speckit.analyze
```

Should now show: "✅ No consistency issues found. Specification is ready for planning."

### What You Learned
- `/speckit.analyze` catches inconsistencies before implementation
- Cross-feature dependencies need validation
- Early analysis saves hours of rework
- Good specs answer "what if" questions upfront

### ✅ Checkpoint
Specification analysis passes with no critical issues.

---

## Exercise 2: Create Quality Checklists

### Goal
Use `/speckit.checklist` to create custom validation checklists for specifications.

### Steps

**2.1** Invoke the checklist command:

```
/speckit.checklist
```

**2.2** Describe the checklist purpose:

```
Create a quality checklist for Recipe Manager feature specifications to validate:
1. All user stories follow "As a [role], I want [action] so that [benefit]" format
2. Performance requirements have measurable targets with units
3. Error handling covers database failures, validation failures, and edge cases
4. Data model changes are backward-compatible with existing features
5. Privacy requirements align with constitution (no cloud, local-only)
6. Accessibility considerations for UI features (WCAG AA)
7. Test coverage plan includes unit, integration, and performance tests
8. Dependencies on other features are documented
```

**2.3** Copilot generates a checklist:

Example output saved to `.speckit/checklist.md`:
```markdown
# Recipe Manager — Specification Quality Checklist

## User Stories ✅ / ❌
- [ ] All user stories use "As a [role], I want [action] so that [benefit]" format
- [ ] Each user story has at least one acceptance criterion
- [ ] User roles are defined (home cook, food blogger, family)
- [ ] Benefits are clear and measurable

## Performance Requirements ✅ / ❌
- [ ] All performance targets have units (ms, seconds, MB)
- [ ] Performance targets reference benchmark (e.g., "with 10,000 recipes")
- [ ] Performance targets align with constitution.md
- [ ] Performance testing approach is specified

## Error Handling ✅ / ❌
- [ ] Database errors are handled (locked, permission denied)
- [ ] Validation errors have user-friendly messages
- [ ] Edge cases documented (empty input, boundary values)
- [ ] Error recovery or fallback behavior specified

## Data Model Changes ✅ / ❌
- [ ] New fields don't break existing features
- [ ] Migration strategy documented (if applicable)
- [ ] Foreign keys and constraints specified
- [ ] Indexes for performance identified

## Privacy & Security ✅ / ❌
- [ ] No cloud dependencies (aligns with constitution)
- [ ] User data stays local
- [ ] Input validation prevents injection attacks
- [ ] Sensitive data handling specified

## Accessibility ✅ / ❌
- [ ] WCAG 2.1 AA compliance for UI features
- [ ] Keyboard navigation specified
- [ ] Screen reader compatibility considered
- [ ] Color contrast meets standards

## Testing Strategy ✅ / ❌
- [ ] Unit tests planned for business logic
- [ ] Integration tests for database operations
- [ ] Performance tests with realistic data
- [ ] Coverage target ≥ 80% specified

## Dependencies ✅ / ❌
- [ ] Dependent features listed (e.g., 001 Core Storage)
- [ ] API contracts between features documented
- [ ] Version compatibility specified (if applicable)
```

**2.4** Run the checklist on Recipe Search spec:

```
Apply the quality checklist to .speckit/features/002-recipe-search/spec.md and report which items pass or fail.
```

Copilot reviews and reports:
```
Recipe Search Specification — Checklist Results

✅ PASSED: 12 / 18 items
❌ FAILED: 6 / 18 items

FAILURES:
- ❌ Accessibility: No WCAG compliance specified (UI feature)
- ❌ Testing Strategy: No integration test plan mentioned
- ❌ Data Model: No index strategy for search performance
- ❌ Error Handling: Missing specification for malformed queries
- ❌ Performance: Benchmark not specified (how many recipes?)
- ❌ Dependencies: Version of Core Storage not documented

RECOMMENDATIONS:
1. Add "Performance tested with 10,000 recipes" to spec
2. Add search index creation in data model section
3. Add accessibility note: "Search UI follows WCAG 2.1 AA"
4. Add test plan: integration tests for FTS5 queries
5. Add error handling: "Invalid search syntax returns empty results"
6. Add dependency note: "Requires Core Storage v0.1.0+"
```

**2.5** Update spec based on checklist failures:

Fix the 6 issues, then re-run:
```
Re-run quality checklist on updated spec.md
```

Should show: "✅ PASSED: 18 / 18 items"

### What You Learned
- Checklists enforce consistent quality standards
- Custom checklists capture project-specific requirements
- Checklists catch details humans often forget
- Automated validation scales better than manual reviews

### ✅ Checkpoint
Custom quality checklist created and validates specifications successfully.

---

## Exercise 3: Add Second Feature Iteratively

### Goal
Build Recipe Search feature on top of existing Core Storage using Spec-Driven Development.

### Steps

**3.1** Ensure recipe_search spec.md is finalized:

After Exercise 1 and 2, you should have a solid spec.

**3.2** Create implementation plan:

```
/speckit.plan

Create implementation plan for Recipe Search using:
- SQLite FTS5 (full-text search extension)
- Extend RecipeRepository with search methods
- Add search_index table with triggers to keep in sync
- Minimal CLI additions (search command)
- Reuse existing models and repository architecture
```

**3.3** Generate tasks:

```
/speckit.tasks
```

Expected tasks (simplified):
```markdown
## Phase 1: Database Schema (2 tasks)
- [ ] Add FTS5 virtual table for search index
- [ ] Add triggers to sync recipes table with search index

## Phase 2: Repository Extension (2 tasks)
- [ ] Implement search_recipes(query: str) method in RecipeRepository
- [ ] Add tests for search functionality

## Phase 3: CLI Integration (2 tasks)
- [ ] Add "Search recipes" menu option in cli.py
- [ ] Display search results with ranking

## Phase 4: Validation (1 task)
- [ ] Performance test: < 100ms search with 10,000 recipes
```

**3.4** Implement with `/speckit.implement`:

```
/speckit.implement
```

Copilot will:
- Modify existing `repository.py` to add `search_recipes()` method
- Create migration script to add FTS5 table
- Update `cli.py` to add search menu option
- Generate tests in `tests/test_repository.py`

**3.5** Test iterative addition:

```powershell
# Run all tests (existing + new)
pytest tests/ --cov=recipe_manager

# Manually test search
python recipe_manager/cli.py
# Select "Search recipes"
# Enter query: "pasta tomato"
```

**3.6** Validate backward compatibility:

Ensure Core Storage still works:
- Create recipe (should work as before)
- View recipe (should work)
- Update recipe (should work)
- Delete recipe (should work)

**3.7** Commit the feature:

```powershell
git add .
git commit -m "feat: add recipe search functionality

- Implement FTS5 full-text search
- Extend RecipeRepository with search_recipes()
- Add search UI to CLI
- Maintain backward compatibility with Core Storage
- Performance: 15ms average (10,000 recipes)

Closes #002-recipe-search"
```

### Iterative Development Benefits

Compared to building both features together:
- ✅ **Lower risk** — Core Storage is stable before adding Search
- ✅ **Faster feedback** — Users can give feedback on Storage before Search exists
- ✅ **Better testing** — Test Search independently of Storage
- ✅ **Easier debugging** — Know if bug is in new feature or old
- ✅ **Incremental value** — Deliver Storage immediately, Search later

### What You Learned
- Spec-Driven Development works for iterative additions
- New features extend existing code without breaking it
- Backward compatibility is validated explicitly
- Small, incremental features are less risky than big-bang releases

### ✅ Checkpoint
Recipe Search feature added successfully on top of Core Storage.

---

## Exercise 4: Refine Constitution Based on Learnings

### Goal
Update the constitution to reflect lessons learned during implementation.

### Steps

**4.1** Review what you learned building 2 features:

Ask yourself:
- Were all constitution principles helpful?
- Are there new principles we should add?
- Did any constraints turn out to be wrong?

**4.2** Ask Copilot for constitution suggestions:

```
Based on implementing Core Recipe Storage and Recipe Search, suggest improvements to constitution.md. 
Consider:
1. Principles that were unclear or conflicting
2. Technical constraints that were too strict or too loose
3. Quality standards that need adjustment
4. New principles discovered during development
```

**4.3** Copilot might suggest:

```markdown
## Suggested Constitution Updates

### New Principles
1. **Iterative Feature Development:** Build features incrementally, validate each before next
   - Rationale: Reduces risk, enables faster feedback
   - Implication: Feature specs should minimize dependencies

2. **Backward Compatibility:** New features must not break existing functionality
   - Rationale: Users rely on stable core features
   - Implication: Database migrations, API versioning required

### Refinement of Existing Principles
1. **Performance:** Original target "< 50ms" was for retrieval, but search is different
   - Refinement: "Retrieval < 50ms, Search < 100ms, Bulk operations < 500ms"
   - Rationale: Different operations have different complexity

2. **Code Quality:** 80% coverage was achievable but CLI was hard to test
   - Refinement: "80% coverage for business logic, 60% for UI, 95% for data models"
   - Rationale: Different layers have different testability

### Clarifications
1. **Data Privacy:** "Local storage" was clear, but is export okay?
   - Clarification: "Local storage with user-initiated export (JSON, CSV) allowed"
```

**4.4** Update constitution.md:

```
Update constitution.md with the suggested refinements from Copilot's analysis
```

**4.5** Commit constitution update:

```powershell
git add .speckit/constitution.md
git commit -m "docs: refine constitution based on implementation learnings

- Add iterative development principle
- Add backward compatibility principle
- Clarify performance targets by operation type
- Adjust code coverage targets by layer
- Clarify data privacy allows user-initiated export

This reflects lessons from implementing 002 features."
```

### Constitution as Living Document

The constitution is not set in stone:
- ✅ **Evolve** as you learn about your project
- ✅ **Refine** constraints based on real implementation challenges
- ✅ **Add** principles when patterns emerge
- ⚠️ **Don't change** core values (privacy, performance, quality) without team discussion

### What You Learned
- Specifications and plans are not the only artifacts that evolve
- Constitution reflects learnings from implementation
- Refining principles improves future features
- Living documents beat frozen documentation

### ✅ Checkpoint
Constitution updated with implementation-informed improvements.

---

## Exercise 5: Brownfield Spec-Driven Development

### Goal
Apply Spec-Driven Development to an existing project (not built with Spec Kit).

### Context

**Brownfield projects** are existing codebases where you want to add features using Spec-Driven Development.

**Challenges:**
- No existing specifications
- Unknown architecture
- Unclear original intent
- Technical debt

**Spec-Driven approach for brownfield:**
1. Reverse-engineer a specification from code
2. Create a selective constitution (for new features, not entire codebase)
3. Define new features using Spec Kit
4. Implement with awareness of legacy constraints

### Steps

**5.1** Simulate a brownfield scenario:

Imagine you inherited a codebase with:
- A working Recipe Manager (your implemented code)
- No documentation
- Some technical debt (hardcoded paths, missing tests in CLI)

**5.2** Reverse-engineer a specification:

```
Analyze the existing recipe_manager/ codebase and generate a specification that describes its current behavior. Include:
1. What features exist today
2. Data models (reverse from database schema)
3. API (reverse from service layer)
4. Known limitations
```

Copilot generates a reverse-engineered spec:
```markdown
# Recipe Manager — Reverse-Engineered Specification

## Current Features
- Create, Read, Update, Delete recipes
- Full-text search by title and ingredients
- Local SQLite storage

## Data Model
- Recipe: id (UUID), title, ingredients (JSON array), instructions (JSON array), 
  prep_time, cook_time, servings, created_at, updated_at
- Search index: FTS5 virtual table

## Known Limitations
- CLI only (no web UI)
- No user authentication
- No recipe photos
- Hardcoded database path
- CLI not unit tested
```

**5.3** Create a brownfield constitution:

```
Create a constitution for future features in this brownfield Recipe Manager. Focus on:
1. Do not break existing functionality
2. Improve test coverage for new code (80%+)
3. Refactor legacy code only when touching it (Boy Scout Rule)
4. Add documentation as you go
```

**5.4** Define a new feature for brownfield:

```
/speckit.specify

Add "Recipe Photos" feature:
- Users can attach 1-5 photos to a recipe
- Photos stored locally as image files
- Database stores file paths
- Photo viewing in CLI (show file path, allow opening in default viewer)

CONSTRAINTS (Brownfield):
- Must not modify existing Recipe model (add new PhotoRepository)
- Must not break existing create/update/delete operations
- Performance: Photo operations should not slow down core CRUD
```

**5.5** Plan with legacy constraints:

```
/speckit.plan

Implementation plan for Recipe Photos:
- Create NEW module: photo_manager.py (don't modify existing files unnecessarily)
- Add photos table to database (migration script)
- Add RecipePhotoService (depends on RecipeRepository)
- Update CLI only where needed (new "Manage Photos" menu item)
- Strategy: Minimal changes to existing code, extend via new modules
```

**5.6** Understand incremental refactoring:

As you add photos, you **can** refactor nearby code:
- If you touch `repository.py`, add docstrings and tests
- If you modify `cli.py`, extract functions for testability
- Don't refactor unrelated code (scope creep risk)

### Brownfield Best Practices

| Principle | Brownfield Adaptation |
|-----------|----------------------|
| **Constitution** | Create for new features only, not entire legacy codebase |
| **Specification** | Reverse-engineer current behavior first |
| **Planning** | Plan around legacy constraints (can't change everything) |
| **Implementation** | Extend via new modules, minimize changes to legacy code |
| **Testing** | Test new code thoroughly, add tests to legacy as you touch it |
| **Refactoring** | Boy Scout Rule: leave code better than you found it (incrementally) |

### What You Learned
- Spec-Driven Development works for brownfield projects
- Reverse-engineer specs from existing code
- Extend legacy code with new modules (minimize changes)
- Incremental improvement beats big-bang rewrites

### ✅ Checkpoint
You understand how to apply Spec Kit to existing codebases.

---

## Exercise 6: Master the Complete Workflow

### Goal
Understand when to use each Spec Kit command and master the full lifecycle.

### Steps

**6.1** Review the complete Spec Kit command set:

| Command | Purpose hint | When to Use | Can Skip? |
|---------|----------|-------------|-----------|
| `/speckit.constitution` | Project principles | Project start, major pivots | ❌ Never |
| `/speckit.specify` | Define feature requirements | Every new feature | ❌ Never |
| `/speckit.clarify` | Explore ambiguities | Complex domains, many edge cases | ✅ For simple features |
| `/speckit.plan` | Technical design | Every feature | ❌ Never |
| `/speckit.tasks` | Break down into steps | Every feature | ❌ Never |
| `/speckit.analyze` | Validate consistency | Mission-critical features, large teams | ✅ For solo/simple projects |
| `/speckit.checklist` | Create quality gates | Projects with strict quality requirements | ✅ For MVPs |
| `/speckit.implement` | Generate code | Every feature | ❌ Never (unless manual coding) |

**6.2** Understand workflow variations:

**Minimal Workflow (MVP, solo, time-pressured):**
```
Constitution → Specify → Plan → Tasks → Implement
```

**Standard Workflow (most projects):**
```
Constitution → Specify → Clarify → Plan → Tasks → Implement
```

**High-Quality Workflow (mission-critical, large teams):**
```
Constitution → Specify → Clarify → Analyze → Plan → Tasks → Checklist → Implement
```

**6.3** Create a workflow guide for your team:

Ask Copilot:
```
Create a decision tree for Recipe Manager team: when to use each Spec Kit command based on feature complexity, risk, and time constraints.
```

Copilot generates:
```markdown
# Recipe Manager — Spec Kit Workflow Guide

## Decision Tree

### For Every Feature:
1. ✅ `/speckit.constitution` (if first feature or principles changed)
2. ✅ `/speckit.specify`
3. ✅ `/speckit.plan`
4. ✅ `/speckit.tasks`
5. ✅ `/speckit.implement`

### Use `/speckit.clarify` IF:
- Feature has 5+ user stories
- Domain is unfamiliar (e.g., nutrition calculations)
- Stakeholders have conflicting requirements
- Edge cases are non-obvious

### Use `/speckit.analyze` IF:
- Feature modifies existing data models
- Feature depends on 2+ other features
- Feature is customer-facing (high visibility)
- Large team (3+ developers) working in parallel

### Use `/speckit.checklist` IF:
- Compliance requirements (GDPR, HIPAA, etc.)
- Quality bar is exceptionally high
- Team is new to Spec-Driven Development (training wheels)
- External audit or review process required

## Time Estimates

| Workflow | Feature Complexity | Time Estimate |
|----------|-------------------|---------------|
| Minimal | Simple (CRUD, UI tweak) | 1-2 hours |
| Standard | Medium (new capability) | 3-6 hours |
| High-Quality | Complex (multi-feature integration) | 1-2 days |
```

**6.4** Commit workflow guide:

```powershell
git add .speckit/workflow-guide.md
git commit -m "docs: add Spec Kit workflow decision guide"
```

**6.5** Reflect on what you learned:

Ask Copilot:
```
Summarize the key benefits of Spec-Driven Development based on this workshop experience
```

**6.6** Celebrate mastery! 🎉

You've completed all 5 levels of the Spec Kit workshop and can now:
- Set up Spec-Driven projects
- Write effective specifications
- Create technical plans and tasks
- Generate implementations with AI
- Validate quality and iterate
- Apply Spec Kit to any project (greenfield or brownfield)

### What You Learned
- Not all commands are needed for every feature
- Workflow adapts to project risk and complexity
- Documentation of workflow helps teams scale
- Mastery comes from understanding when to skip steps

### ✅ Checkpoint
You are a Spec-Driven Development practitioner!

---

## Exercise 7: Create Custom Instructions & Agents (Optional)

> **Note:** This exercise is optional and extends the workshop. It teaches transferable skills for your own projects.

### Goal
Understand how to create instruction files and custom agent skills to enhance AI assistance in your own projects.

### Background: What You've Been Using

Throughout Levels 1-5, two AI enhancements have been helping you:

1. **Instruction File** (`.github/instructions/recipe-domain.instructions.md`)
   - Teaches GitHub Copilot about recipe domain concepts
   - Provides measurement conversions, dietary restrictions, terminology
   - Improves code generation accuracy for Recipe Manager

2. **SpecKitCoach Agent** (`.github/skills/speckit-coach/SKILL.md`)
   - Specialized AI assistant invoked with `@SpecKitCoach`
   - Provides guided help through Spec-Driven Development workflow
   - Offers encouragement, validates understanding, answers methodology questions

### Steps

**7.1** Examine the existing instruction file:

Open and review [.github/instructions/recipe-domain.instructions.md](../../.github/instructions/recipe-domain.instructions.md)

Notice the structure:
- **Domain Overview** — What problem space this covers
- **Core Concepts** — Key terminology and definitions
- **Code Generation Guidelines** — How to write code in this domain
- **Best Practices** — Common patterns and pitfalls
- **Terminology Guide** — Domain vocabulary

**7.2** Understand when instruction files help:

Instruction files are valuable when:
- ✅ Domain has specialized terminology (medical, legal, finance)
- ✅ Coding patterns require domain knowledge (measurement conversions)
- ✅ Validation rules are domain-specific (dietary restrictions)
- ✅ Multiple team members need consistent AI behavior

Skip instruction files when:
- ❌ Domain is generic (CRUD, basic UI)
- ❌ Solo project with no specialized knowledge
- ❌ Short-lived prototype

**7.3** Create a template for your own domain:

Ask Copilot:
```
Based on .github/instructions/recipe-domain.instructions.md, create a template I can use for [YOUR DOMAIN].

My domain is: [e.g., "E-commerce order processing" or "Healthcare patient records" or "Financial portfolio management"]

Include sections for:
1. Domain overview
2. Core concepts and terminology
3. Code generation guidelines
4. Common validation rules
5. Best practices
```

Copilot generates a customized template you can use in future projects.

**7.4** Examine the SpecKitCoach agent skill:

Open and review [.github/skills/speckit-coach/SKILL.md](../../.github/skills/speckit-coach/SKILL.md)

Notice the structure:
- **Your Role** — Agent's purpose and personality
- **Expertise Areas** — Topics the agent knows deeply
- **Interaction Patterns** — How to respond in different scenarios
- **Response Guidelines** — Structure and tone
- **Common Questions** — Frequently asked questions with answers
- **Example Opening** — What to say when invoked

**7.5** Understand when custom agents help:

Custom agent skills are valuable when:
- ✅ Workflow has specific methodology (Spec-Driven Development, TDD, DDD)
- ✅ Team needs guided learning (onboarding, training)
- ✅ Subject matter expertise is specialized (architecture patterns, security)
- ✅ Consistent coaching across team members

Skip custom agents when:
- ❌ Generic coding questions (standard Copilot works fine)
- ❌ No specialized methodology
- ❌ Solo developer with deep expertise

**7.6** Try invoking the SpecKitCoach agent:

In Copilot Chat, type:
```
@SpecKitCoach What's the difference between /speckit.specify and /speckit.plan?
```

Notice the response is:
- Contextualized to Spec-Driven Development
- Encouraging and educational
- Provides actionable guidance
- More specific than generic Copilot

**7.7** Create an agent skill template for your domain:

Ask Copilot:
```
Based on .github/skills/speckit-coach/SKILL.md, create an agent skill template for [YOUR AGENT NAME].

My agent should help with: [e.g., "Database migration best practices" or "React performance optimization" or "API security reviews"]

Include:
1. Role definition
2. Expertise areas (3-5 topics)
3. Interaction patterns (when to guide vs. answer directly)
4. Example responses to common questions
```

Copilot generates a SKILL.md template you can customize.

**7.8** Commit your templates:

```powershell
git add .github/templates/
git commit -m "docs: add instruction file and agent skill templates for future projects"
```

### File Path Conventions

For instruction files:
```
.github/instructions/
  ├── domain-name.instructions.md  (e.g., recipe-domain.instructions.md)
  ├── api-design.instructions.md
  └── security.instructions.md
```

For agent skills:
```
.github/skills/
  ├── agent-name/
  │   └── SKILL.md  (e.g., speckit-coach/SKILL.md)
  ├── database-expert/
  │   └── SKILL.md
  └── security-reviewer/
      └── SKILL.md
```

### What You Learned
- Instruction files teach domain knowledge to GitHub Copilot
- Agent skills create specialized AI assistants for your workflow
- Both are transferable: use in your own projects
- Structure matters: follow conventions for consistency
- Custom AI enhancements improve team productivity

### ✅ Checkpoint
You can create instruction files and agent skills for your own projects!

### 🎁 Takeaway

The templates you created in this exercise are **yours to keep**. Use them for:
- Your next work project (add domain knowledge)
- Open source contributions (create helpful agents)
- Team onboarding (guide new developers)
- Personal projects (specialized AI assistance)

**Pro tip:** Share your instruction files and agent skills with teammates. Consistent AI behavior across the team improves code quality and collaboration.

---

## Level 5 Wrap-Up

### What You Accomplished

✅ Validated specification consistency with `/speckit.analyze`  
✅ Created custom quality checklists with `/speckit.checklist`  
✅ Added a second feature iteratively (Recipe Search)  
✅ Refined constitution based on implementation learnings  
✅ Understood brownfield Spec-Driven Development  
✅ Mastered the complete workflow and command set  
✅ (Optional) Created instruction files and agent skills for future projects  

### Key Takeaways

1. **Analysis prevents costly mistakes** — `/speckit.analyze` catches issues early
2. **Checklists enforce standards** — Custom gates ensure consistent quality
3. **Iteration beats big-bang** — Small, incremental features reduce risk
4. **Constitution evolves** — Principles improve with implementation experience
5. **Brownfield works** — Spec-Driven Development applies to legacy code
6. **Adapt the workflow** — Not every feature needs every command

### Full Workshop Achievement

🏆 **Congratulations!** You've mastered Spec-Driven Development with GitHub Spec Kit.

**You can now:**
- Build high-quality software faster with AI assistance
- Write specifications that generate correct implementations
- Validate quality before and after implementation
- Iterate on features without breaking existing functionality
- Apply Spec-Driven Development to any project
- Teach others this methodology

### Time Investment Summary

| Workshop Level | Time | Value Delivered |
|----------------|------|-----------------|
| Level 1 | 20 min | Project setup, constitution |
| Level 2 | 25 min | Specification and clarification |
| Level 3 | 25 min | Technical plan and tasks |
| Level 4 | 30 min | Working implementation |
| Level 5 | 20 min | Advanced quality and iteration |
| Level 5 (Ex 7) | +10 min (optional) | Custom instructions & agents |
| **Total** | **2 hours (2.5 with Exercise 7)** | **Production-ready Recipe Manager with 2 features** |

**Without Spec-Driven Development:**
- 8-16 hours of manual coding
- Multiple refactoring cycles
- Incomplete documentation
- Lower test coverage
- More bugs

### Next Steps Beyond the Workshop

1. **Apply to real projects** — Use Spec Kit for your next feature
2. **Customize for your team** — Adapt constitution and checklists
3. **Share knowledge** — Teach teammates Spec-Driven Development
4. **Contribute** — Share your learnings with the Spec Kit community
5. **Stay updated** — Follow [Spec Kit repository](https://github.com/github/spec-kit) for updates

### Resources for Continued Learning

- **Spec Kit Documentation:** https://github.github.io/spec-kit/
- **Spec-Driven Development Guide:** https://github.com/github/spec-kit/blob/main/spec-driven.md
- **Community Examples:** https://github.com/github/spec-kit/tree/main/examples
- **GitHub Discussions:** https://github.com/github/spec-kit/discussions

---

## Workshop Complete! 🎉

You've successfully completed the **Getting Started with GitHub Spec Kit** workshop.

**Share your success:**
- Tweet about your experience with #SpecDrivenDevelopment
- Star the [Spec Kit repository](https://github.com/github/spec-kit)
- Share feedback in [GitHub Discussions](https://github.com/github/spec-kit/discussions)

**Join the community:**
- Follow [Spec Kit on GitHub](https://github.com/github/spec-kit)
- Connect with other practitioners
- Contribute improvements to Spec Kit

Thank you for learning with us! 🚀

---

## Quick Reference

See [CHEATSHEET.md](CHEATSHEET.md) for a compact reference of all Level 5 concepts, commands, and best practices.
