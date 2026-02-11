# Level 3 — Quick Reference Card

## Planning Commands

| Command | Purpose | Output |
|---------|---------|--------|
| `/speckit.plan` | Create technical implementation plan | `.speckit/features/XXX-name/plan.md` |
| `/speckit.tasks` | Break plan into actionable tasks | `.speckit/features/XXX-name/tasks.md` |

## Spec → Plan → Tasks Flow

```
spec.md (WHAT to build)
    ↓
plan.md (HOW to build: tech stack, architecture)
    ↓
tasks.md (EXECUTION: ordered, actionable steps)
```

## Plan.md Structure

```markdown
# [Feature Name] — Implementation Plan

## Technology Stack
- Language: [Python 3.11+]
- Storage: [SQLite]
- Testing: [pytest]
- Additional tools: [mypy, ruff]

## Architecture
### Layers
1. **Models** — Data classes with validation
2. **Repository** — Database operations (CRUD)
3. **Service** — Business logic orchestration
4. **CLI/UI** — User interaction

### Design Patterns
- Repository pattern for data access
- Dependency injection for testability

## Database Schema
```sql
CREATE TABLE recipes (
    id TEXT PRIMARY KEY,
    title TEXT NOT NULL,
    ...
);
```

## File Structure
```
app/
├── models.py
├── repository.py
├── service.py
...
```

## Implementation Steps
1. Step 1 description
2. Step 2 description
...

## Testing Strategy
- Unit tests for [X]
- Integration tests for [Y]
- Target coverage: 80%+

## Performance Considerations
- Indexing strategy
- Query optimization
- Connection pooling

## Dependencies
```
pytest>=7.4.0
...
```

## Deployment
- How to run
- How to build/package
```

## Tasks.md Structure

```markdown
# [Feature Name] — Implementation Tasks

## Phase 1: [Phase Name] (X tasks)
- [ ] **[P0]** Task 1.1: Description with file/module name
- [ ] **[P1]** Task 1.2: Description with success criteria

## Phase 2: [Phase Name] (Y tasks)
...

## Total: NN tasks across MM phases
```

## Task Priority Levels

| Priority | Meaning | When to Use |
|----------|---------|-------------|
| **P0** | Must-have, blocking | Core functionality, dependencies for other tasks |
| **P1** | Important, not blocking | Feature enhancements, non-critical tests |
| **P2** | Nice-to-have | Documentation, optimizations, future prep |

## Architecture Patterns for Recipe Manager

### Layered Architecture

```
┌─────────────────────────────────────┐
│   CLI / UI Layer                    │  ← User interaction
├─────────────────────────────────────┤
│   Service Layer                     │  ← Business logic
├─────────────────────────────────────┤
│   Repository Layer                  │  ← Data access
├─────────────────────────────────────┤
│   Models Layer                      │  ← Domain entities
└─────────────────────────────────────┘
```

**Benefits:**
- Testable (mock each layer independently)
- Maintainable (change UI without touching database)
- Scalable (add API layer without changing service)

### Repository Pattern

```python
# Repository handles ALL database interactions
class RecipeRepository:
    def create(self, recipe: Recipe) -> Recipe: ...
    def get_by_id(self, id: str) -> Recipe | None: ...
    def update(self, recipe: Recipe) -> Recipe: ...
    def delete(self, id: str) -> bool: ...
    def list_all(self) -> list[Recipe]: ...
```

**Benefits:**
- Swap SQLite for PostgreSQL without changing service
- Mock repository for testing service layer
- Centralize query optimization and error handling

## Technology Decision Framework

### Questions to Ask

1. **Constitution alignment:**
   - Does this tech meet privacy requirements? (local vs. cloud)
   - Does it work cross-platform? (Windows/macOS/Linux)
   - Does it support code quality standards? (type hints, testing)

2. **Specification alignment:**
   - Can it meet performance requirements? (< 50ms queries)
   - Does it scale to data volumes? (10,000+ recipes)
   - Can it handle validation rules? (field constraints)

3. **Team factors:**
   - Do we have expertise in this tech?
   - Is the learning curve acceptable?
   - Is there good tooling/ecosystem support?

4. **Future-proofing:**
   - Can we extend features easily?
   - Is the tech actively maintained?
   - Can we migrate away if needed?

## Common Technology Stacks for Recipe Manager

| Stack | Pros | Cons | Best For |
|-------|------|------|----------|
| **Python + SQLite + pytest** | Fast setup, cross-platform, great tooling | Slower than compiled languages | General apps, data processing |
| **TypeScript + SQLite + Jest** | Type safety, modern tooling, fast | Node.js dependency, more complex setup | JavaScript teams, web integration |
| **Rust + SQLite + cargo test** | Blazing fast, memory safe | Steep learning curve, longer dev time | Performance-critical apps |
| **Go + SQLite + testing** | Fast, simple deployment | Verbose, less libraries | CLI tools, servers |

## Task Breakdown Best Practices

### Atomic Tasks

**Good (Atomic):**
- ✅ "Create Recipe dataclass in models.py with title, ingredients, instructions fields"
- ✅ "Implement create_recipe() method in RecipeRepository with error handling"
- ✅ "Write unit tests for Recipe validation in test_models.py, achieve 90%+ coverage"

**Bad (Too Large):**
- ❌ "Build the entire database layer"
- ❌ "Finish backend"
- ❌ "Make tests work"

### Task Sizing

| Task Size | Time | Risk | When to Use |
|-----------|------|------|-------------|
| **Micro** | 5-15 min | Very low | Simple additions (single function) |
| **Small** | 15-30 min | Low | Typical feature task |
| **Medium** | 30-60 min | Medium | Complex logic, multiple files |
| **Large** | 1-2 hours | High | Should be split into smaller tasks |

## Task Dependencies Visualization

```
Phase 1: Foundation
    Task 1.1: Project setup
    Task 1.2: Dependencies
    Task 1.3: Config
         ↓
Phase 2: Data Models
    Task 2.1: Recipe model  ──→  Task 2.2: Model tests
         ↓
Phase 3: Database
    Task 3.1: DB connection ──→  Task 3.2: Repository ──→  Task 3.3: Repo tests
         ↓
Phase 4: Service
    Task 4.1: Service logic  ──→  Task 4.2: Service tests
         ↓
Phase 5: UI
    Task 5.1: CLI interface
         ↓
Phase 6: Validation
    Task 6.1: Coverage check
    Task 6.2: Type check
    Task 6.3: Performance test
```

## Planning Anti-Patterns

| Anti-Pattern | Problem | Solution |
|--------------|---------|----------|
| **"God class" plan** | One module does everything | Separate concerns into layers |
| **Premature optimization** | Planning for scale not needed | Follow YAGNI (You Aren't Gonna Need It) |
| **Technology bikeshedding** | Endless debate on tools | Set decision deadline, use ADRs |
| **Waterfall phasing** | Can't test until end | Plan for incremental testing |
| **Missing error handling** | Only happy path planned | Plan for failures upfront |

## Traceability Matrix Template

| Spec Requirement | Plan Section | Tasks | Acceptance Test |
|------------------|--------------|-------|-----------------|
| Create recipe | Repository.create() | 3.2, 4.1, 5.1 | test_create_recipe_success |
| Validate title | Recipe model | 2.1 | test_validate_title_length |
| < 50ms retrieval | DB indexing | 3.4, 6.3 | test_performance_retrieval |
| 80% coverage | Testing strategy | 6.1 | pytest --cov report |

## Git Workflow for Planning

```powershell
# Commit plan after generation
git add .speckit/features/001-name/plan.md
git commit -m "feat: add implementation plan for [feature]"

# Commit tasks after refinement
git add .speckit/features/001-name/tasks.md
git commit -m "feat: add task breakdown for [feature]"

# Push for team review before implementation
git push -u origin 001-name
```

## Performance Planning Checklist

For Recipe Manager with < 50ms retrieval requirement:

- [ ] Database indexes on frequently queried columns (id, title)
- [ ] Connection pooling or reuse (don't open/close DB per query)
- [ ] Query optimization (SELECT specific columns, not SELECT *)
- [ ] Benchmark with realistic data (1000+ recipes)
- [ ] Measure with timing decorators or profilers
- [ ] Plan for performance regression tests

## Testing Strategy Components

### Unit Tests
- **What:** Individual functions/methods in isolation
- **Mock:** External dependencies (database, file system)
- **Coverage:** Aim for 90%+ on business logic

### Integration Tests
- **What:** Multiple components working together
- **Real:** Use in-memory SQLite, not mocks
- **Coverage:** Critical paths (create → save → retrieve)

### End-to-End Tests
- **What:** Full user workflows
- **Real:** Actual database file, CLI interactions
- **Coverage:** Happy path + error scenarios

### Performance Tests
- **What:** Measure speed, memory, scalability
- **Real:** Load 1000+ recipes, measure query time
- **Target:** Meet specification benchmarks (< 50ms)

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Plan includes vague "implement feature X" | Break into concrete file/function tasks |
| No test tasks | Add test tasks for each implementation task |
| Missing validation phase | Add final phase: coverage, types, performance |
| Tasks depend on single developer | Parallelize where possible |
| No error handling planned | Add exception classes, error propagation strategy |
| Performance afterthought | Include performance targets in plan, test in tasks |

## Resources

- **Architecture Patterns:** https://martinfowler.com/eaaCatalog/
- **Repository Pattern:** https://www.cosmicpython.com/book/chapter_02_repository.html
- **Task Estimation:** https://www.atlassian.com/agile/project-management/estimation

## Next Level

[Level 4: Implement & Validate →](../level-4/README.md)
