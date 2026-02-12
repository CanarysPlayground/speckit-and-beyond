# Experiment 2: Specify & Clarify — Defining What to Build

> **Risk level:** 🟢 Zero — This level focuses on specification writing without generating code.

## Learning Objectives

By the end of this level, you will be able to:

1. Write effective specifications focused on outcomes, not implementation
2. Use `/speckit.specify` to create rich feature specifications
3. Identify underspecified areas in requirements
4. Use `/speckit.clarify` to explore edge cases and ambiguities
5. Refine specifications through AI-guided clarification questions
6. Distinguish between "what" (specification) and "how" (implementation)
7. Structure specifications for AI-readability
8. Capture user stories, acceptance criteria, and constraints
9. Understand feature branching in Spec Kit workflow
10. Document non-functional requirements (performance, security, UX)

---

## Prerequisites

- [ ] Completed **Level 1** (constitution exists in `.speckit/constitution.md`)
- [ ] Project initialized with `specify init`
- [ ] GitHub Copilot active in VS Code
- [ ] `/speckit.*` commands visible in Copilot Chat

---

## Workshop Structure

This experiment contains **7 exercises**. Estimated time: **25 minutes**.

| Exercise | Topic | Time |
|----------|-------|------|
| 1 | Understanding Feature Scope | 3 min |
| 2 | Create First Feature Branch | 2 min |
| 3 | Write Core Recipe Storage Spec | 6 min |
| 4 | Clarify Edge Cases | 5 min |
| 5 | Refine Specification | 4 min |
| 6 | Validate Against Constitution | 3 min |
| 7 | Review Final Specification | 2 min |

---

## The Feature: Core Recipe Storage

For Recipe Manager, we'll start with the **most fundamental feature**: storing and retrieving recipes.

### User Stories

**Primary:**
> "As a home cook, I want to save my recipes with title, ingredients, instructions, and photos so I can access them later."

**Secondary:**
> "As a home cook, I want to edit existing recipes when I improve them so my collection stays current."

> "As a home cook, I want to delete recipes I no longer use so my collection stays organized."

### Why Start Here?

- **Foundation** — All other features depend on recipe storage
- **Core value** — Without this, the app does nothing
- **Well-defined** — Clear inputs, outputs, and operations (CRUD)
- **Testable** — Easy to validate success

---

## Exercise 1: Understanding Feature Scope

### Goal
Define boundaries for the Core Recipe Storage feature.

### Steps

**1.1** Before writing the spec, brainstorm with Copilot:

```
For a Recipe Manager app, what would "Core Recipe Storage" include? 
List the essential capabilities without implementation details.
```

Expected response:
- Create new recipes
- View recipe details
- Edit existing recipes
- Delete recipes
- Basic recipe fields (title, ingredients, instructions)
- Recipe ID/unique identification

**1.2** Ask about what to exclude from this feature:

```
What recipe-related capabilities should NOT be in "Core Recipe Storage" 
but instead be separate features?
```

Expected response:
- Search (separate feature: "Recipe Search")
- Categories/tags (separate feature: "Recipe Organization")
- Sharing (separate feature: "Recipe Sharing")
- Shopping lists (separate feature: "Shopping List Generation")
- Photos (could be separate: "Recipe Photos")

**1.3** Define non-functional requirements:

```
Based on constitution.md, what performance, privacy, and quality requirements 
apply to recipe storage?
```

Expected response (based on your Experiment 1 constitution):
- **Privacy:** Local storage only, no cloud sync
- **Performance:** Recipe loads in < 50ms
- **Data integrity:** No data loss on app crash
- **Validation:** Required fields enforced
- **Testing:** 80%+ code coverage

**1.4** Document your scope decision:

In **Copilot Chat**, summarize:
```
For Core Recipe Storage, we will include:
- CRUD operations (Create, Read, Update, Delete)
- Essential fields: title, ingredients, instructions, prep time, cook time, servings
- Local file-based storage (SQLite or JSON)
- Data validation and error handling

We will EXCLUDE (for future features):
- Search functionality
- Categories and tags
- Photo attachments
- Sharing mechanisms
```

### What You Learned
- Scope definition prevents feature creep
- Separating concerns creates cleaner, more maintainable features
- Non-functional requirements shape implementation
- AI helps identify scope boundaries through conversation

### ✅ Checkpoint
You have a clear mental model of what Core Recipe Storage includes and excludes.

---

## Exercise 2: Create First Feature Branch

### Goal
Set up Git workflow for isolated feature development.

### Steps

**2.1** Check current Git status:

```powershell
git status
git branch
```

You should be on `main` with a clean working tree.

**2.2** Create a feature branch:

```powershell
git checkout -b 001-core-recipe-storage
```

Naming convention: `XXX-feature-name` where XXX is a sequential number (001, 002, etc.)

**2.3** Verify branch:

```powershell
git branch
```

You should see:
```
  main
* 001-core-recipe-storage
```

The `*` indicates your active branch.

**2.4** Create feature directory:

Spec Kit will auto-create this, but understanding the structure helps:

```powershell
mkdir -p .speckit/features/001-core-recipe-storage
```

**2.5** Ask Copilot about feature detection:

```
How does Spec Kit determine which feature I'm working on?
```

Expected response: Spec Kit uses your Git branch name. If you're on `001-core-recipe-storage`, it knows to work in `.speckit/features/001-core-recipe-storage/`.

### What You Learned
- Feature branches isolate work-in-progress from main
- Branch names map to feature directories in `.speckit/features/`
- Git integration is automatic (Spec Kit reads current branch)
- Sequential numbering helps track feature order

### ✅ Checkpoint
You're on branch `001-core-recipe-storage` and ready for specification.

---

## Exercise 3: Write Core Recipe Storage Spec

### Goal
Use `/speckit.specify` to generate a rich, AI-ready specification.

### Steps

**3.1** Invoke the specify command in Copilot Chat:

```
/speckit.specify
```

**3.2** Provide your specification content when prompted:

```
Create a specification for Core Recipe Storage:

USER STORIES:
1. As a home cook, I want to save recipes with title, ingredients, instructions, prep time, cook time, and servings so I can access them later
2. As a home cook, I want to edit existing recipes to improve them over time
3. As a home cook, I want to delete recipes I no longer need to keep my collection organized
4. As a home cook, I want to view all my saved recipes in a list

FUNCTIONAL REQUIREMENTS:
- Create recipe: User provides title (required), ingredients (required, list), instructions (required, steps), prep time (optional, minutes), cook time (optional, minutes), servings (optional, number)
- Each recipe must have a unique identifier (UUID)
- Read recipe: Display full recipe details by ID
- Update recipe: Modify any field except UUID
- Delete recipe: Remove recipe permanently with confirmation
- List recipes: Show all recipes with title, prep time, cook time (summary view)

DATA VALIDATION:
- Title: 1-200 characters, cannot be only whitespace
- Ingredients: At least 1 ingredient, each ingredient 1-500 characters
- Instructions: At least 1 step, each step 1-2000 characters
- Prep time, cook time: Positive integers or zero, max 999 minutes
- Servings: Positive integer, max 100

ERROR HANDLING:
- Validation failures return clear error messages
- Database errors (file locked, permission denied) are caught and logged
- Duplicate titles are allowed (same recipe name, different versions)

NON-FUNCTIONAL REQUIREMENTS:
Based on constitution.md:
- Storage must be local (SQLite database or JSON file)
- Recipe retrieval must complete in < 50ms
- All operations must be synchronous (no async complexity for v1)
- Data must persist across app restarts
- No data loss on unexpected shutdown (flush on write)

CONSTRAINTS:
- No external dependencies (cloud services, APIs)
- Must work offline 100% of the time
- Cross-platform: Windows, macOS, Linux
```

**3.3** Copilot will generate a `spec.md` file in `.speckit/features/001-core-recipe-storage/`

**3.4** Review the generated specification:

Open `.speckit/features/001-core-recipe-storage/spec.md` and examine:

- **Overview** — High-level description
- **User Stories** — Who/what/why format
- **Functional Requirements** — Detailed capabilities
- **Data Model** — Recipe structure
- **Validation Rules** — Input constraints
- **Error Handling** — Failure scenarios
- **Non-Functional Requirements** — Performance, privacy, etc.
- **Out of Scope** — What this feature does NOT include
- **Acceptance Criteria** — How to verify success

**3.5** Check alignment with constitution:

Ask Copilot:
```
Review the spec.md for Core Recipe Storage and confirm it aligns with principles in constitution.md
```

Copilot should validate:
- Local storage (matches "Data Privacy" principle)
- Performance target (matches "Search Performance" principle)
- Error handling (matches "Code Quality" principle)

### Understanding Specification Structure

Good specifications are:
- **Outcome-focused** — What users can achieve, not how code works
- **Complete** — All requirements, edge cases, constraints documented
- **Unambiguous** — Clear enough for AI (or human) to implement correctly
- **Testable** — Acceptance criteria are measurable
- **Prioritized** — Must-have vs. nice-to-have is clear

### What You Learned
- `/speckit.specify` transforms user stories into structured specifications
- Specifications document both functional and non-functional requirements
- Good specs reference the constitution for alignment
- Edge cases and validation rules are critical (not afterthoughts)

### ✅ Checkpoint
You have a comprehensive `spec.md` in `.speckit/features/001-core-recipe-storage/`.

---

## Exercise 4: Clarify Edge Cases

### Goal
Use `/speckit.clarify` to explore underspecified areas through AI-generated questions.

### Steps

**4.1** Invoke the clarify command:

```
/speckit.clarify
```

**4.2** Copilot will analyze your spec and ask clarifying questions like:

Example questions Copilot might generate:
1. **Recipe duplication:** Should users be able to duplicate an existing recipe as a starting point for a new one?
2. **Measurement units:** Should ingredients support structured units (e.g., "2 cups flour") or free-form text?
3. **Instruction ordering:** Can users reorder instruction steps, or are they fixed once created?
4. **Servings scaling:** Should the app auto-scale ingredient quantities when servings are changed?
5. **Draft state:** Can recipes be saved as "drafts" (incomplete) or must all required fields be filled?
6. **Deletion safety:** Should deleted recipes go to a "trash" first, or be permanently removed immediately?
7. **Import/export:** Should this feature include recipe export (JSON, PDF) or is that a separate feature?
8. **Multi-language:** Should the app support recipes in multiple languages?

**4.3** Answer each question thoughtfully:

For example:
```
1. Recipe duplication: YES - add "Duplicate" action that copies recipe with " (Copy)" appended to title
2. Measurement units: Free-form text for v1 (structured units in future feature)
3. Instruction ordering: YES - users can reorder steps via drag-drop or up/down buttons
4. Servings scaling: NO - separate feature ("Recipe Scaling")
5. Draft state: NO - all required fields (title, ingredients, instructions) must be present to save
6. Deletion safety: Permanent deletion with confirmation dialog (no trash for v1)
7. Import/export: Separate feature ("Recipe Import/Export")
8. Multi-language: YES - support Unicode, but UI in English only for v1
```

**4.4** Copilot will integrate your answers into the specification

**4.5** Review updated `spec.md`:

Check that clarifications are added as new subsections:
- **Recipe Duplication** section under Features
- **Instruction Ordering** under CRUD Operations
- **Data Model** might include `ingredients` as text field
- **Out of Scope** explicitly lists servings scaling and import/export

### Why Clarification Matters

Without clarification:
- AI makes assumptions (often wrong)
- Implementation surprises lead to rework
- Edge cases become production bugs
- User expectations aren't met

With clarification:
- **Shared understanding** — Team and AI aligned
- **Better estimates** — Complexity is visible
- **Fewer bugs** — Edge cases handled upfront
- **Clear scope** — "Out of scope" prevents mission creep

### What You Learned
- `/speckit.clarify` acts as a requirements analyst
- Clarification questions reveal assumptions you didn't know you had
- Answering questions improves specification quality
- Some questions lead to new features (scope expansion awareness)

### ✅ Checkpoint
Your `spec.md` has been updated with clarification answers and is more comprehensive.

---

## Exercise 5: Refine Specification

### Goal
Manually review and improve the specification based on your domain knowledge.

### Steps

**5.1** Open `.speckit/features/001-core-recipe-storage/spec.md` in VS Code

**5.2** Review the **Data Model** section:

Example data model:
```markdown
## Data Model

### Recipe
- `id`: UUID (auto-generated, immutable)
- `title`: string (1-200 chars, required)
- `ingredients`: string (list, at least 1, required)
- `instructions`: string (list, at least 1, required)
- `prep_time`: integer (minutes, optional)
- `cook_time`: integer (minutes, optional)
- `servings`: integer (optional)
- `created_at`: timestamp (auto-generated)
- `updated_at`: timestamp (auto-updated)
```

**5.3** Check for missing considerations:

As a human reviewer, ask yourself:
- Are there any fields users will definitely want? (e.g., notes, difficulty level)
- Are there any constraints missing? (e.g., ingredients max count)
- Are timestamps in UTC or local time?
- What happens if two users edit the same recipe simultaneously? (Not applicable for v1, but worth noting)

**5.4** Make manual edits if needed:

For example, add optional fields:
```markdown
- `notes`: string (optional, free-form text for tips)
- `source`: string (optional, where recipe came from - cookbook, website, etc.)
```

**5.5** Update **Acceptance Criteria**:

Ensure measurable criteria exist:
```markdown
## Acceptance Criteria

✅ User can create a recipe with title, ingredients, and instructions in < 30 seconds
✅ Recipe is retrievable by ID in < 50ms
✅ User can edit any recipe field and changes persist
✅ User can delete a recipe with 2 clicks (delete button + confirmation)
✅ Validation errors are shown immediately with helpful messages
✅ All recipes are listed with title, prep time, cook time visible
✅ App survives unexpected shutdown without data loss (last 5 seconds)
✅ Unit tests achieve 80%+ coverage for storage module
```

**5.6** Commit your refined specification:

```powershell
git add .speckit/features/001-core-recipe-storage/spec.md
git commit -m "feat: add core recipe storage specification"
```

### Specification Best Practices

**DO:**
- ✅ Use precise language ("at most 200 characters" not "short title")
- ✅ Include examples (sample recipe JSON)
- ✅ Reference constitution principles explicitly
- ✅ Separate "must have" from "nice to have"
- ✅ Define error messages and edge cases

**DON'T:**
- ❌ Specify implementation details (SQL vs NoSQL, React vs Vue)
- ❌ Use vague terms ("fast", "user-friendly" without metrics)
- ❌ Skip validation rules ("title is required" without length/format)
- ❌ Assume obvious behavior (make it explicit)
- ❌ Mix multiple features (keep scope tight)

### What You Learned
- AI-generated specs need human review
- Manual refinement adds domain expertise
- Acceptance criteria are testable success metrics
- Version control tracks specification evolution
- Good specs answer "what" and "why", not "how"

### ✅ Checkpoint
Your `spec.md` is complete, reviewed, refined, and committed.

---

## Exercise 6: Validate Against Constitution

### Goal
Ensure the specification upholds all principles defined in `constitution.md`.

### Steps

**6.1** Ask Copilot to perform validation:

```
Review spec.md for Core Recipe Storage against every principle in constitution.md. 
Flag any conflicts or missing considerations.
```

**6.2** Copilot will check alignment:

Example validation output:
```
✅ User-Centric Simplicity: Spec focuses on CRUD operations with clear workflows
✅ Privacy by Design: Local storage specified, no cloud dependencies
✅ Performance Over Features: 50ms retrieval target documented
✅ Code Quality: 80% test coverage requirement included
✅ Cross-Platform: No platform-specific features mentioned
⚠️ Extensibility: Plugin architecture not addressed in spec
⚠️ Accessibility: No mention of WCAG compliance for UI (if applicable)
✅ Offline-First: 100% offline functionality specified
```

**6.3** Address warnings:

For flagged issues:
```
Update spec.md to note:
- Extensibility: Recipe storage will use a Repository pattern to allow future storage backend plugins
- Accessibility: UI accessibility is out of scope for this spec (handled in UI feature)
```

**6.4** Verify performance requirements:

```
Do the performance targets in spec.md (< 50ms retrieval) align with constitution.md (< 100ms search)?
```

Ensure consistency across documents.

**6.5** Final validation:

```
Are all requirements in spec.md implementable given the constraints in constitution.md (no cloud, cross-platform, etc.)?
```

Copilot should confirm feasibility.

### Constitution as Quality Gate

The constitution acts as a **specification lint tool**:
- Catches requirements that violate principles
- Surfaces missing considerations (e.g., accessibility)
- Ensures consistency across features
- Prevents technical debt from misalignment

### What You Learned
- Constitution validation is a critical review step
- Conflicts should be resolved before planning phase
- Some requirements may need to move to other features
- AI can automate compliance checking

### ✅ Checkpoint
Your specification is validated against the constitution with no unresolved conflicts.

---

## Exercise 7: Review Final Specification

### Goal
Do a final human review to ensure specification quality.

### Steps

**7.1** Read the entire `spec.md` aloud (yes, literally):

Reading forces you to catch:
- Ambiguous wording ("should" vs. "must")
- Missing details (what happens if...?)
- Inconsistent terminology

**7.2** Use the "explain to a 10-year-old" test:

Ask Copilot:
```
Explain the Core Recipe Storage feature in spec.md as if I'm 10 years old
```

If Copilot's explanation is confusing, your spec might be too complex or unclear.

**7.3** Check for implementation leakage:

Search spec.md for these red flags:
- Technology names (SQLite, FastAPI, React) ← Should be in plan, not spec
- Code terms (class, function, endpoint) ← Implementation details
- "We will use X" ← Should be "System will provide Y capability"

**7.4** Verify completeness:

Ask Copilot:
```
What questions do you still have about Core Recipe Storage that spec.md doesn't answer?
```

If significant questions remain, add clarifications.

**7.5** Get a second opinion:

If working in a team:
```powershell
# Push your branch for review
git push -u origin 001-core-recipe-storage
```

Ask a teammate to review `spec.md` before proceeding to planning.

**7.6** Mark specification as ready:

Add a status badge at the top of `spec.md`:
```markdown
# Core Recipe Storage Specification

**Status:** ✅ Ready for Planning  
**Feature:** 001-core-recipe-storage  
**Last Updated:** 2026-02-11
```

**7.7** Final commit:

```powershell
git add .
git commit -m "docs: finalize core recipe storage specification"
```

### Specification Quality Checklist

Before moving to Experiment 3, ensure:

- [ ] All user stories have clear "who/what/why"
- [ ] Functional requirements are complete (no "TBD" sections)
- [ ] Data model defines all fields with types and constraints
- [ ] Validation rules are specific and comprehensive
- [ ] Error handling covers common failure modes
- [ ] Non-functional requirements have measurable targets
- [ ] Acceptance criteria are testable
- [ ] "Out of scope" section prevents misunderstandings
- [ ] Constitution alignment is validated
- [ ] No implementation details leaked into spec
- [ ] Specification is committed to Git

### What You Learned
- Specifications require both AI and human review
- Reading aloud catches ambiguities
- Implementation details should stay out of specs
- Status tracking helps teams coordinate
- Incomplete specs lead to implementation delays

### ✅ Checkpoint
You have a production-ready specification for Core Recipe Storage.

---

## Experiment 2 Wrap-Up

### What You Accomplished

✅ Defined feature scope and boundaries  
✅ Created a feature branch for isolated development  
✅ Generated a comprehensive specification with `/speckit.specify`  
✅ Clarified edge cases and ambiguities with `/speckit.clarify`  
✅ Refined specification with human domain expertise  
✅ Validated specification against constitution  
✅ Reviewed and finalized specification for implementation  

### Key Takeaways

1. **Specifications before implementation** — Prevents costly rework
2. **AI as requirements analyst** — `/speckit.clarify` reveals hidden assumptions
3. **Constitution as quality gate** — Ensures all features align with principles
4. **Iteration is expected** — Refine specs until they're crystal clear
5. **Git tracks evolution** — Every spec change is versioned

### Time Investment vs. Benefit

**25 minutes on Experiment 2 prevents:**
- Building the wrong feature
- Misaligned implementation attempts
- Missing critical edge cases
- Rework after user feedback
- Team miscommunication

### Common Pitfalls

**Pitfall:** Spec includes technology choices ("use PostgreSQL")  
**Fix:** Keep specs tech-agnostic. Save choices for `/speckit.plan` in Experiment 3.

**Pitfall:** Spec is too vague ("user-friendly interface")  
**Fix:** Be measurable ("< 3 clicks to create recipe", "validation errors shown within 200ms").

**Pitfall:** Skipping clarification to save time  
**Fix:** `/speckit.clarify` often reveals dealbreakers. 5 minutes here saves hours later.

**Pitfall:** Spec conflicts with constitution  
**Fix:** Always run validation before moving to planning phase.

---

## Next Steps

🚀 **Proceed to [Experiment 3: Plan & Tasks](../experiment-3/README.md)**

Transform your specification into a technical implementation plan with architecture decisions, tech stack choices, and actionable tasks.

---

## Quick Reference

See [CHEATSHEET.md](CHEATSHEET.md) for a compact reference of all Experiment 2 concepts, commands, and best practices.


