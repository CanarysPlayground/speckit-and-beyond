# Experiment 3: Plan & Tasks — From Specification to Action

> **Risk level:** 🟡 Low — This level creates plans and tasks but doesn't generate code yet.

## Learning Objectives

By the end of this level, you will be able to:

1. Transform specifications into technical implementation plans
2. Use `/speckit.plan` to define architecture and tech stack
3. Make informed technology choices based on constitution constraints
4. Document implementation approach with justification
5. Use `/speckit.tasks` to break plans into actionable items
6. Prioritize tasks for efficient implementation
7. Estimate task complexity and dependencies
8. Review and refine task lists before implementation
9. Understand the separation between "what" (spec) and "how" (plan)
10. Prepare for the implementation phase

---

## Prerequisites

- [ ] Completed **Level 2** (spec.md exists and is finalized)
- [ ] On feature branch `001-core-recipe-storage`
- [ ] Specification validated against constitution
- [ ] GitHub Copilot active in VS Code

---

## Workshop Structure

This experiment contains **6 exercises**. Estimated time: **25 minutes**.

| Exercise | Topic | Time |
|----------|-------|------|
| 1 | Evaluate Technology Options | 5 min |
| 2 | Create Implementation Plan | 8 min |
| 3 | Review Architecture Decisions | 4 min |
| 4 | Generate Task Breakdown | 5 min |
| 5 | Prioritize and Sequence Tasks | 2 min |
| 6 | Finalize Plan and Tasks | 1 min |

---

## The Transition: Specification → Plan

### Where We Are

You have `spec.md` that describes **WHAT** to build:
- User stories and requirements
- Data model and validation
- Non-functional requirements
- Acceptance criteria

### Where We're Going

You'll create `plan.md` that describes **HOW** to build:
- Technology choices (language, framework, storage)
- Architecture (layers, modules, patterns)
- Data persistence strategy
- Testing approach
- Development workflow

### Why Separate Spec and Plan?

1. **Multiple implementations** — Same spec, different tech stacks (Python vs. Node, SQLite vs. PostgreSQL)
2. **Technology evolution** — Rewrite implementation without changing requirements
3. **Team alignment** — Spec is for product, plan is for engineering
4. **AI flexibility** — Copilot can suggest alternatives at planning phase

---

## Exercise 1: Evaluate Technology Options

### Goal
Explore technology choices that align with your constitution and specification.

### Steps

**1.1** Review constitution constraints:

Open `.speckit/constitution.md` and note:
- Cross-platform requirement (Windows, macOS, Linux)
- Local storage requirement (no cloud dependencies)
- Code quality standards (80% test coverage, type hints)
- Offline-first requirement

**1.2** Ask Copilot for tech stack recommendations:

```
Based on constitution.md and spec.md for Core Recipe Storage, suggest 2-3 technology stacks that would work well. Consider:
- Programming language
- Storage mechanism (database or file-based)
- Testing framework
- Build/package tooling

Explain pros/cons of each option.
```

**1.3** Copilot might suggest options like:

**Option 1: Python + SQLite + pytest**
- **Pros:** Cross-platform, SQLite built-in, pytest widely used, strong typing with mypy
- **Cons:** Python packaging can be complex, performance slower than compiled languages
- **Fit:** Excellent — matches constitution (cross-platform, local)

**Option 2: Python + JSON files + pytest**
- **Pros:** Zero dependencies, human-readable storage, easy backup
- **Cons:** No query optimization, file locking challenges, doesn't scale to 10,000+ recipes
- **Fit:** Good for MVP, but performance concerns

**Option 3: TypeScript + SQLite + Jest**
- **Pros:** Type safety, large ecosystem, fast execution
- **Cons:** More setup complexity, Node.js dependency
- **Fit:** Good if team knows JavaScript ecosystem

**1.4** Choose based on your context:

For this workshop, we'll choose **Python + SQLite + pytest**:
- Matches constitution (cross-platform, local, type hints)
- SQLite handles 10,000+ recipes easily (meets performance requirement)
- pytest aligns with 80% coverage requirement
- Minimal dependencies (offline-first)

**1.5** Document your decision rationale:

In Copilot Chat, note:
```
We will use Python + SQLite + pytest because:
1. Python is cross-platform and has built-in SQLite support
2. SQLite provides performant local storage (< 50ms queries)
3. pytest enables high test coverage with minimal setup
4. Type hints via mypy improve code quality
5. Zero cloud dependencies (offline-first)
```

### What You Learned
- Architecture decisions should explicitly reference constitution constraints
- Multiple valid options often exist — choose based on team/context
- AI can compare trade-offs, but humans make final decisions
- Documenting "why" prevents future second-guessing

### ✅ Checkpoint
You've chosen a technology stack with justified rationale.

---

## Exercise 2: Create Implementation Plan

### Goal
Use `/speckit.plan` to generate a comprehensive technical plan.

### Steps

**2.1** Invoke the planning command:

```
/speckit.plan
```

**2.2** Provide your implementation guidance:

```
Create an implementation plan for Core Recipe Storage using:

TECHNOLOGY STACK:
- Language: Python 3.11+
- Storage: SQLite (single database file)
- Testing: pytest with pytest-cov for coverage
- Type checking: mypy for static type validation
- Linting: ruff for fast Python linting

ARCHITECTURE:
- Repository pattern: Separate data access from business logic
- Layers:
  1. Models (data classes with validation)
  2. Repository (SQLite CRUD operations)
  3. Service (business logic, validation orchestration)
  4. CLI interface (basic text-based UI for v1)

DATABASE SCHEMA:
- Single table: recipes
- Columns: id (TEXT PRIMARY KEY), title (TEXT NOT NULL), ingredients (TEXT NOT NULL), 
  instructions (TEXT NOT NULL), prep_time (INTEGER), cook_time (INTEGER), 
  servings (INTEGER), notes (TEXT), source (TEXT), created_at (REAL), updated_at (REAL)
- Ingredients and instructions stored as JSON strings (array of strings)
- Timestamps as Unix epoch floats

ERROR HANDLING:
- Custom exception hierarchy (RecipeNotFoundError, ValidationError, StorageError)
- All repository methods return Result type or raise typed exceptions
- User-facing errors include actionable messages

TESTING STRATEGY:
- Unit tests for models (validation logic)
- Integration tests for repository (with in-memory SQLite)
- Service layer tests with repository mocks
- Target: 80%+ coverage
- Fixtures for sample recipes

DEVELOPMENT WORKFLOW:
1. Create data models with validation
2. Implement repository with CRUD operations
3. Create service layer with business logic
4. Add CLI interface (basic menu)
5. Write tests for each layer
6. Integration testing with real SQLite file
7. Performance validation (< 50ms queries)

FILE STRUCTURE:
```
recipe_manager/
├── models.py         # Recipe data class with validation
├── repository.py     # SQLite CRUD operations
├── service.py        # Business logic layer
├── cli.py            # Command-line interface
├── exceptions.py     # Custom exception classes
├── database.py       # Database connection management
└── tests/
    ├── test_models.py
    ├── test_repository.py
    ├── test_service.py
    └── conftest.py   # pytest fixtures
```

DEPENDENCIES (requirements.txt):
```
pytest>=7.4.0
pytest-cov>=4.1.0
mypy>=1.5.0
ruff>=0.1.0
```

Ensure this plan aligns with constitution.md principles (local storage, performance, code quality, cross-platform).
```

**2.3** Copilot generates `plan.md` in `.speckit/features/001-core-recipe-storage/`

**2.4** Review the generated plan:

Open `.speckit/features/001-core-recipe-storage/plan.md` and verify:
- **Technology Stack** section with versions
- **Architecture** with layer descriptions
- **Data Model** with SQL schema
- **Implementation Steps** (numbered sequence)
- **Testing Strategy** with coverage targets
- **File Structure** showing all modules
- **Dependencies** with versions
- **Performance Considerations** (indexing, connection pooling)
- **Deployment** instructions (even if simple)

### What You Learned
- `/speckit.plan` transforms specifications into actionable technical plans
- Plans include technology stack, architecture, and implementation sequence
- Good plans justify decisions with reference to constitution/spec
- Plans are detailed enough for AI (or junior developer) to implement

### ✅ Checkpoint
You have a comprehensive `plan.md` in `.speckit/features/001-core-recipe-storage/`.

---

## Exercise 3: Review Architecture Decisions

### Goal
Validate that architectural choices align with requirements and constraints.

### Steps

**3.1** Check layering against separation of concerns:

Ask Copilot:
```
Review the architecture layers in plan.md. Does the separation between Models, Repository, Service, and CLI provide proper separation of concerns?
```

Expected validation:
- ✅ Models handle data structure and validation (single responsibility)
- ✅ Repository handles database operations (persistence)
- ✅ Service coordinates business logic (orchestration)
- ✅ CLI handles user interaction (presentation)

**3.2** Validate database schema against data model in spec:

```
Compare the database schema in plan.md to the data model in spec.md. 
Are all required fields present? Are constraints enforced?
```

Check:
- ✅ All spec fields (title, ingredients, instructions, etc.) are columns
- ✅ NOT NULL constraints for required fields
- ✅ Appropriate data types (TEXT, INTEGER, REAL)
- ✅ Primary key defined (id as UUID TEXT)

**3.3** Review testing strategy against constitution:

```
Does the testing strategy in plan.md achieve the 80% coverage requirement in constitution.md?
```

Verify:
- ✅ Unit tests for each layer
- ✅ Integration tests for end-to-end workflows
- ✅ Coverage tool specified (pytest-cov)
- ✅ Target explicitly stated (80%+)

**3.4** Check performance approach:

```
How does plan.md address the < 50ms retrieval requirement from spec.md?
```

Look for:
- ✅ SQLite indexing strategy (index on id)
- ✅ Connection reuse (not re-opening DB for each query)
- ✅ Query optimization (SELECT only needed columns)
- ✅ Performance testing in plan

**3.5** Verify offline-first and cross-platform:

```
Does plan.md maintain offline-first (no cloud) and cross-platform (Windows/macOS/Linux)?
```

Confirm:
- ✅ No network dependencies (SQLite is local)
- ✅ Python standard library only (no platform-specific packages)
- ✅ File paths use pathlib (cross-platform)

**3.6** If issues found, refine the plan:

```
Update plan.md to add database indexes on the id column for faster retrieval.
```

Or manually edit `plan.md` in VS Code.

### Architecture Best Practices

**Good Architecture Indicators:**
- ✅ Layers have clear responsibilities (no overlap)
- ✅ Dependencies point inward (UI → Service → Repository → Models)
- ✅ Data models are independent of storage (can swap SQLite for PostgreSQL)
- ✅ Testing is easy (layers can be tested in isolation)
- ✅ Complexity is managed (repository handles SQL, service handles business rules)

**Red Flags:**
- ❌ "God class" — One class/module does everything
- ❌ Circular dependencies (Service imports Repository, Repository imports Service)
- ❌ Direct UI-to-database coupling (CLI writes SQL queries)
- ❌ No error handling strategy
- ❌ Hard-coded values (database path, magic numbers)

### What You Learned
- Architecture review catches design flaws early
- Plans must align with constitution, spec, and technical realities
- Layering enables testing, maintainability, and future changes
- Performance is addressed proactively, not as an afterthought

### ✅ Checkpoint
Your `plan.md` is architecturally sound and aligned with all requirements.

---

## Exercise 4: Generate Task Breakdown

### Goal
Use `/speckit.tasks` to convert the plan into a prioritized, actionable task list.

### Steps

**4.1** Invoke the tasks command:

```
/speckit.tasks
```

**4.2** Copilot analyzes `plan.md` and generates `tasks.md`

**4.3** Review the generated task list:

Open `.speckit/features/001-core-recipe-storage/tasks.md`

Example task structure:
```markdown
# Core Recipe Storage — Implementation Tasks

## Phase 1: Foundation (3 tasks)
- [ ] Task 1.1: Set up project structure (directories, __init__.py files)
- [ ] Task 1.2: Create requirements.txt with dependencies (pytest, mypy, ruff)
- [ ] Task 1.3: Configure pytest (pytest.ini) and mypy (mypy.ini)

## Phase 2: Data Models (2 tasks)
- [ ] Task 2.1: Create Recipe model in models.py with dataclass and validation
- [ ] Task 2.2: Write unit tests for Recipe model validation in tests/test_models.py

## Phase 3: Database Layer (4 tasks)
- [ ] Task 3.1: Create database connection manager in database.py
- [ ] Task 3.2: Implement RecipeRepository with CRUD operations in repository.py
- [ ] Task 3.3: Write integration tests for repository in tests/test_repository.py
- [ ] Task 3.4: Add database indexes for performance

## Phase 4: Service Layer (3 tasks)
- [ ] Task 4.1: Create RecipeService with business logic in service.py
- [ ] Task 4.2: Implement custom exceptions in exceptions.py
- [ ] Task 4.3: Write service layer tests in tests/test_service.py

## Phase 5: User Interface (2 tasks)
- [ ] Task 5.1: Create CLI menu interface in cli.py
- [ ] Task 5.2: Add input validation and error display in CLI

## Phase 6: Validation (3 tasks)
- [ ] Task 6.1: Run pytest with coverage, ensure 80%+ coverage
- [ ] Task 6.2: Run mypy for type checking, fix all errors
- [ ] Task 6.3: Performance test: verify recipe retrieval < 50ms with 1000 recipes

## Total: 17 tasks across 6 phases
```

**4.4** Check task characteristics:

Each task should be:
- ✅ **Actionable** — Clear deliverable (file, function, test)
- ✅ **Atomic** — Can be completed in 10-30 minutes
- ✅ **Testable** — You'll know when it's done
- ✅ **Ordered** — Dependencies are clear (earlier phases first)

**4.5** Review task phasing:

Phases should follow logical order:
1. **Foundation** — Setup before coding
2. **Data Models** — Core domain logic
3. **Database Layer** — Persistence
4. **Service Layer** — Business logic
5. **User Interface** — Presentation
6. **Validation** — Quality gates

**4.6** Commit the task list:

```powershell
git add .speckit/features/001-core-recipe-storage/tasks.md
git commit -m "feat: add implementation tasks for core recipe storage"
```

### What You Learned
- `/speckit.tasks` breaks plans into bite-sized work items
- Task lists are phased for logical development flow
- Each task is independently verifiable
- Task granularity affects implementation speed (too big = risky, too small = overhead)

### ✅ Checkpoint
You have a detailed, phased task list ready for implementation.

---

## Exercise 5: Prioritize and Sequence Tasks

### Goal
Review task order and identify any dependencies or optimizations.

### Steps

**5.1** Visualize task dependencies:

Ask Copilot:
```
Create a dependency diagram for the tasks in tasks.md. 
Which tasks must be completed before others?
```

Example dependency flow:
```
Task 1.1 (setup) → Everything else
Task 2.1 (models) → Task 2.2 (tests), Task 3.2 (repository)
Task 3.1 (database) → Task 3.2 (repository)
Task 3.2 (repository) → Task 4.1 (service)
Task 4.2 (exceptions) → Task 4.1 (service)
Task 4.1 (service) → Task 5.1 (CLI)
All implementation tasks → Phase 6 (validation)
```

**5.2** Identify parallelization opportunities:

```
Which tasks in tasks.md can be worked on in parallel by multiple developers?
```

Possible pairs:
- Task 2.1 (models) and Task 4.2 (exceptions) — No dependency
- Task 3.3 (repo tests) while another developer works on Task 4.1 (service)
- Task 5.1 (CLI) and Task 5.2 (CLI validation) can overlap

**5.3** Check for missing tasks:

```
Review tasks.md against plan.md. Are any implementation steps from the plan missing as tasks?
```

Copilot might find:
- Documentation task (README for module)
- Database migration/initialization script
- Sample data fixtures for testing
- Performance benchmarking script

**5.4** Add missing tasks if needed:

```
Add a task: "Create sample_data.py with 100 example recipes for performance testing"
```

Or edit `tasks.md` manually.

**5.5** Review task estimates:

Ask Copilot:
```
Estimate effort for each task in tasks.md. Flag any tasks that seem too large (> 1 hour).
```

If a task is too large, split it:
- Task 3.2 (Implement RecipeRepository) might be:
  - Task 3.2a: Implement create and read operations
  - Task 3.2b: Implement update and delete operations

**5.6** Finalize task priorities:

Update `tasks.md` with priority flags:
```markdown
## Phase 2: Data Models
- [ ] **[P0]** Task 2.1: Create Recipe model (blocking all downstream work)
- [ ] **[P1]** Task 2.2: Write unit tests for Recipe model
```

Priority levels:
- **P0** — Must-have, blocks other work
- **P1** — Important, but not blocking
- **P2** — Nice-to-have, can defer

### Task Management Best Practices

**Clear Tasks:**
- Use imperative verbs ("Create", "Implement", "Write")
- Include file/module name ("in models.py", "in test_repository.py")
- Specify success criteria ("ensure 80%+ coverage")

**Avoid Task Pitfalls:**
- ❌ "Work on database stuff" (too vague)
- ❌ "Finish entire repository layer" (too large)
- ❌ "Make it work" (not actionable)
- ✅ "Implement create_recipe() method in RecipeRepository with error handling"

### What You Learned
- Task dependencies determine implementation order
- Some tasks can be parallelized for team efficiency
- Task granularity affects risk (smaller = lower risk)
- Priorities help when time is constrained

### ✅ Checkpoint
Your task list is ordered, estimated, and ready for execution.

---

## Exercise 6: Finalize Plan and Tasks

### Goal
Final review and commit before moving to implementation phase.

### Steps

**6.1** Validate traceability:

Ask Copilot:
```
Trace each requirement in spec.md to implementation tasks in tasks.md. 
Are all requirements covered?
```

Example traceability:
- Spec requirement: "Create recipe" → Tasks 2.1, 3.2, 4.1, 5.1
- Spec requirement: "Validate title length" → Task 2.1 (model validation)
- Spec requirement: "80% test coverage" → Task 6.1 (validation phase)

If gaps exist:
```
Add a task to implement the "duplicate recipe" feature from spec.md section 3.2
```

**6.2** Check alignment with constitution:

```
Review plan.md and tasks.md against constitution.md. 
Any conflicts or missing quality requirements?
```

**6.3** Final sanity checks:

- [ ] `spec.md` exists and is complete
- [ ] `plan.md` exists with architecture and tech stack
- [ ] `tasks.md` exists with phased, actionable tasks
- [ ] All three files reference each other (traceability)
- [ ] No unresolved "TBD" or "TODO" sections
- [ ] Git commit history tracks evolution

**6.4** Create a pre-implementation summary:

Ask Copilot:
```
Summarize the Core Recipe Storage feature in 3 bullet points covering: 
what (spec), how (plan), and execution (tasks).
```

Example summary:
```
WHAT: Core Recipe Storage enables users to create, read, update, and delete recipes with validation and local persistence.

HOW: Python + SQLite with layered architecture (Models → Repository → Service → CLI), pytest for testing, 80%+ coverage.

EXECUTION: 17 tasks across 6 phases, starting with project setup, building from models to database to service to UI, ending with validation.
```

**6.5** Final commits:

```powershell
git add .
git commit -m "feat: finalize implementation plan and task breakdown for core recipe storage"
git push -u origin 001-core-recipe-storage
```

**6.6** Set expectations for Experiment 4:

In Experiment 4, you'll run `/speckit.implement` which will:
- Execute all 17 tasks automatically
- Generate code for models, repository, service, CLI
- Create tests and fixtures
- Validate against acceptance criteria

The better your spec, plan, and tasks, the better `/speckit.implement` will perform.

### What You Learned
- Traceability ensures no requirements are missed
- Pre-implementation validation saves hours of rework
- Spec → Plan → Tasks creates a complete blueprint for AI
- Version control preserves the entire decision history

### ✅ Checkpoint
You have production-ready specifications, plans, and tasks for Core Recipe Storage.

---

## Experiment 3 Wrap-Up

### What You Accomplished

✅ Evaluated technology options based on constitution constraints  
✅ Created a detailed implementation plan with `/speckit.plan`  
✅ Validated architecture decisions against requirements  
✅ Generated actionable task breakdown with `/speckit.tasks`  
✅ Prioritized and sequenced tasks for efficient execution  
✅ Finalized and committed all planning artifacts  

### Key Takeaways

1. **Plans bridge intent and code** — They translate "what" into "how"
2. **Architecture matters** — Layering enables testing and maintainability
3. **Task granularity is critical** — Too large = risky, too small = overhead
4. **Traceability prevents gaps** — Every requirement should map to tasks
5. **AI excels at structured planning** — `/speckit.plan` and `/speckit.tasks` save hours

### Time Investment vs. Benefit

**25 minutes on Experiment 3 prevents:**
- Poor technology choices discovered mid-implementation
- Architectural refactoring (move code between layers)
- Missing requirements (found during QA, not planning)
- Unclear next steps (developers blocked, waiting for decisions)

### Planning Quality Checklist

Before Experiment 4, ensure:

- [ ] Technology stack is justified with pros/cons
- [ ] Architecture has clear layers with separation of concerns
- [ ] Database schema matches data model in specification
- [ ] Testing strategy achieves constitution requirements (coverage, etc.)
- [ ] Performance approach addresses specification targets
- [ ] Tasks are atomic, actionable, and ordered
- [ ] Dependencies between tasks are clear
- [ ] All requirements trace to tasks
- [ ] No conflicts with constitution principles
- [ ] Plan and tasks are committed to Git

---

## Next Steps

🚀 **Proceed to [Experiment 4: Implement & Validate](../experiment-4/README.md)**

Execute your task list, generate working code, and validate against specifications and constitution.

---

## Quick Reference

See [CHEATSHEET.md](CHEATSHEET.md) for a compact reference of all Experiment 3 concepts, commands, and best practices.



