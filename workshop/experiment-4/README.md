# Experiment 4: Implement & Validate — Code Generation and Testing

> **Risk level:** 🟡 Medium — This level generates code and modifies your project files.

## Learning Objectives

By the end of this level, you will be able to:

1. Use `/speckit.implement` to execute automated implementation
2. Review AI-generated code for quality and correctness
3. Validate generated code against specifications
4. Debug and fix implementation issues
5. Run automated tests and achieve coverage targets
6. Validate performance against benchmarks
7. Verify constitution alignment in actual code
8. Iterate on implementation with AI assistance
9. Document code for future maintainability
10. Prepare for feature completion and merge

---

## Prerequisites

- [ ] Completed **Level 3** (spec.md, plan.md, tasks.md exist and are finalized)
- [ ] On feature branch `001-core-recipe-storage`
- [ ] Clean Git working tree (all previous work committed)
- [ ] GitHub Copilot active in VS Code

---

## Workshop Structure

This level contains **8 exercises**. Estimated time: **30 minutes**.

| Exercise | Topic | Time |
|----------|-------|------|
| 1 | Pre-Implementation Checks | 2 min |
| 2 | Execute Implementation | 8 min |
| 3 | Review Generated Code | 5 min |
| 4 | Run Tests and Check Coverage | 4 min |
| 5 | Fix Issues and Iterate | 5 min |
| 6 | Validate Against Specification | 3 min |
| 7 | Performance Validation | 2 min |
| 8 | Finalize and Document | 1 min |

---

## The Implementation Phase

### What Happens During `/speckit.implement`

When you invoke `/speckit.implement`, Copilot will:
1. Read `spec.md`, `plan.md`, and `tasks.md`
2. Execute each task in `tasks.md` sequentially
3. Generate code files according to `plan.md` structure
4. Create test files with fixtures and test cases
5. Implement validation logic from `spec.md`
6. Add error handling as specified
7. Create configuration and setup files
8. Generate documentation comments

### What You Control

- **Review approval:** For each file generated, you can review before accepting
- **Iterative fixes:** If something is wrong, you can ask Copilot to fix it
- **Manual edits:** You can edit generated code directly
- **Re-generation:** If needed, delete files and re-run `/speckit.implement`

---

## Exercise 1: Pre-Implementation Checks

### Goal
Ensure environment is ready for code generation.

### Steps

**1.1** Verify you're on the correct branch:

```powershell
git branch
```

Should show `* 001-core-recipe-storage`.

**1.2** Check working tree is clean:

```powershell
git status
```

Should show "nothing to commit, working tree clean" or only `.speckit/` changes.

**1.3** Review specification artifacts:

```powershell
ls .speckit/features/001-core-recipe-storage/
```

Should see:
- `spec.md` (requirements)
- `plan.md` (technical design)
- `tasks.md` (actionable steps)

**1.4** Check Python environment:

```powershell
python --version  # Ensure 3.11+
```

If using a virtual environment:
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1  # Windows
# or: source venv/bin/activate  # macOS/Linux
```

**1.5** Ask Copilot for a pre-implementation summary:

```
Based on spec.md, plan.md, and tasks.md, summarize what will be implemented when I run /speckit.implement
```

Expected response:
- File structure (models.py, repository.py, etc.)
- Key classes and methods
- Test coverage approach
- Dependencies to install

**1.6** Create a backup (optional but recommended):

```powershell
git tag backup-before-implementation
```

If anything goes wrong, you can restore with:
```powershell
git reset --hard backup-before-implementation
```

### What You Learned
- Pre-flight checks prevent environment issues mid-implementation
- Git tags provide safety nets for experimentation
- Summarizing expectations helps validate output

### ✅ Checkpoint
Environment is ready, specification artifacts are complete, backup created.

---

## Exercise 2: Execute Implementation

### Goal
Run `/speckit.implement` and generate the Recipe Manager codebase.

### Steps

**2.1** Invoke the implement command:

```
/speckit.implement
```

**2.2** Copilot will begin executing tasks:

You'll see progress like:
```
Executing Task 1.1: Set up project structure...
✅ Created recipe_manager/ directory
✅ Created tests/ directory
✅ Created __init__.py files

Executing Task 1.2: Create requirements.txt...
✅ Generated requirements.txt with dependencies

Executing Task 2.1: Create Recipe model...
✅ Generated models.py with Recipe dataclass
...
```

**2.3** Review files as they're generated:

Copilot will create files one by one. For each file:
- **Review the code** — Check if it matches your expectations
- **Approve or reject** — Accept the file or ask for changes
- **Ask questions** — If something is unclear, ask Copilot to explain

**2.4** Common files generated:

| File | Purpose | Key Content |
|------|---------|-------------|
| `requirements.txt` | Dependencies | pytest, mypy, ruff versions |
| `models.py` | Data models | Recipe dataclass with validation |
| `exceptions.py` | Custom errors | RecipeNotFoundError, ValidationError, StorageError |
| `database.py` | DB connection | SQLite connection manager |
| `repository.py` | Data access | RecipeRepository with CRUD |
| `service.py` | Business logic | RecipeService orchestration |
| `cli.py` | User interface | Command-line menu |
| `tests/conftest.py` | Test fixtures | Sample recipes, in-memory DB |
| `tests/test_models.py` | Model tests | Validation test cases |
| `tests/test_repository.py` | Repo tests | CRUD integration tests |
| `tests/test_service.py` | Service tests | Business logic tests |

**2.5** If Copilot asks for clarification:

Example: "Should ingredients be stored as JSON array or comma-separated string?"
- **Refer to plan.md:** "According to plan.md, ingredients are JSON arrays"
- **Make a decision:** If not specified, choose and update plan.md

**2.6** Monitor token usage:

Copilot may warn about token limits for large implementations. If so:
- **Implement in batches:** Run `/speckit.implement` for phases 1-3, then 4-6
- **Simplify UI:** Start with basic CLI, add features later

**2.7** Installation of dependencies:

After file generation, install dependencies:

```powershell
pip install -r requirements.txt
```

Verify installation:
```powershell
pytest --version
mypy --version
ruff --version
```

### What You Learned
- `/speckit.implement` automates the entire coding process
- Generated code follows plan.md architecture
- Human review is still critical (AI can make mistakes)
- Dependencies are specified, but you must install them

### ✅ Checkpoint
All files from tasks.md are generated, dependencies installed.

---

## Exercise 3: Review Generated Code

### Goal
Manually inspect generated code for quality, correctness, and adherence to standards.

### Steps

**3.1** Review `models.py` (data validation):

Open `recipe_manager/models.py` and check:
- ✅ Recipe dataclass exists with all spec.md fields
- ✅ Field types match specification (title: str, prep_time: int | None)
- ✅ Validation logic for title length (1-200 chars)
- ✅ Validation for ingredients (at least 1)
- ✅ Validation for instructions (at least 1)
- ✅ UUID generation for id field
- ✅ Timestamps (created_at, updated_at)

**3.2** Review `repository.py` (database operations):

Check:
- ✅ `create_recipe(recipe: Recipe) -> Recipe` method exists
- ✅ `get_recipe(id: str) -> Recipe | None` method exists
- ✅ `update_recipe(recipe: Recipe) -> Recipe` method exists
- ✅ `delete_recipe(id: str) -> bool` method exists
- ✅ `list_recipes() -> list[Recipe]` method exists
- ✅ Error handling for database errors
- ✅ SQL injection protection (parameterized queries)
- ✅ Transaction handling (commit/rollback)

**3.3** Review `service.py` (business logic):

Check:
- ✅ Service class depends on repository (dependency injection)
- ✅ Validation orchestration (calls Recipe validation)
- ✅ Error propagation (raises custom exceptions)
- ✅ Business rules enforced (title uniqueness check if required)

**3.4** Review `tests/test_models.py` (validation tests):

Check:
- ✅ Test valid recipe creation
- ✅ Test title validation (too short, too long, empty)
- ✅ Test ingredients validation (empty list)
- ✅ Test instructions validation (empty list)
- ✅ Test optional fields (prep_time, cook_time)

**3.5** Check code quality standards:

Run linter:
```powershell
ruff check recipe_manager/ tests/
```

Fix any issues:
```powershell
ruff check --fix recipe_manager/ tests/
```

Run type checker:
```powershell
mypy recipe_manager/
```

Fix type errors if any (or ask Copilot):
```
Fix mypy errors in repository.py related to Optional return types
```

**3.6** Look for common AI-generated code issues:

| Issue | Where to Check | Fix |
|-------|----------------|-----|
| Hardcoded values | Database path, magic numbers | Move to config.py |
| Missing error messages | Exception raises | Add descriptive messages |
| No logging | Critical operations | Add logging statements |
| Incomplete docstrings | All public methods | Add type hints + descriptions |
| No cleanup | Database connections | Add context managers or close() |

**3.7** Ask Copilot to explain complex sections:

```
Explain the transaction handling in repository.py's create_recipe method
```

### Code Review Checklist

- [ ] All spec.md requirements are implemented
- [ ] Architecture follows plan.md structure (layers are separate)
- [ ] Naming conventions are consistent and clear
- [ ] Error handling covers all failure modes from spec
- [ ] No hardcoded values (use config or constants)
- [ ] Type hints exist on all functions
- [ ] Docstrings explain purpose and usage
- [ ] No security vulnerabilities (SQL injection, etc.)
- [ ] Code passes linters (ruff) and type checkers (mypy)

### What You Learned
- Generated code needs human review for quality
- Architecture violations are easier to spot with plan.md reference
- Linters and type checkers catch issues AI might miss
- Understanding generated code is critical (don't just run it)

### ✅ Checkpoint
Code is reviewed, linters/type checkers pass, no obvious issues.

---

## Exercise 4: Run Tests and Check Coverage

### Goal
Execute test suite and verify it meets constitution requirements.

### Steps

**4.1** Run all tests:

```powershell
pytest tests/ -v
```

Output should show:
```
tests/test_models.py::test_valid_recipe_creation PASSED
tests/test_models.py::test_title_validation PASSED
tests/test_repository.py::test_create_recipe PASSED
tests/test_repository.py::test_get_recipe PASSED
...
===================== 17 passed in 0.45s =====================
```

**4.2** Check test coverage:

```powershell
pytest tests/ --cov=recipe_manager --cov-report=term-missing
```

Expected output:
```
---------- coverage: platform win32, python 3.11.8 -----------
Name                             Stmts   Miss  Cover   Missing
--------------------------------------------------------------
recipe_manager/__init__.py           0      0   100%
recipe_manager/models.py            45      2    96%   78-79
recipe_manager/repository.py        87      5    94%   145-148, 190
recipe_manager/service.py           52      3    94%   89-91
recipe_manager/cli.py               34     34     0%   1-120
--------------------------------------------------------------
TOTAL                              218     44    80%
```

**4.3** Address coverage gaps:

If coverage is below 80% target:

```
Add tests to cover the missing lines in repository.py (lines 145-148)
```

Copilot will generate additional test cases.

**4.4** Run tests for specific modules:

```powershell
# Test only models
pytest tests/test_models.py -v

# Test only repository
pytest tests/test_repository.py -v
```

**4.5** Check for test failures:

If tests fail:
1. **Read the error message** — Understand what's failing
2. **Check the code** — Is the implementation wrong?
3. **Check the test** — Is the test assumption wrong?
4. **Ask Copilot:**
   ```
   Fix the failing test in test_repository.py::test_delete_recipe. 
   Error message: "AssertionError: Recipe was not deleted"
   ```

**4.6** Validate test quality:

Review test files for:
- ✅ Clear test names (test_create_recipe_with_valid_data)
- ✅ Arrange-Act-Assert pattern
- ✅ One assertion per test (or related assertions)
- ✅ Edge cases covered (empty inputs, boundary values)
- ✅ Error scenarios tested (ValidationError raised when expected)

**4.7** Generate coverage report:

```powershell
pytest tests/ --cov=recipe_manager --cov-report=html
```

Open `htmlcov/index.html` in browser to see detailed line-by-line coverage.

### What You Learned
- Automated tests validate implementation correctness
- Coverage metrics show untested code paths
- Test failures guide you to bugs
- Good tests have clear names and structure

### ✅ Checkpoint
All tests pass, coverage is ≥ 80% (constitution requirement).

---

## Exercise 5: Fix Issues and Iterate

### Goal
Debug any remaining issues and refine implementation.

### Steps

**5.1** Manually test the CLI:

```powershell
python recipe_manager/cli.py
```

Interact with the menu:
- Create a recipe
- View the recipe
- Edit the recipe
- Delete the recipe
- List all recipes

**5.2** Look for user experience issues:

Common problems:
- Confusing error messages
- Missing input validation feedback
- Unclear menu options
- No way to exit gracefully

**5.3** Test edge cases manually:

Try to break the application:
- Create recipe with empty title
- Create recipe with 1000-character title
- Create recipe with no ingredients
- Delete non-existent recipe
- Update recipe that doesn't exist

**5.4** If you find bugs, describe them to Copilot:

```
When I create a recipe with an empty title, the app crashes instead of showing a validation error. 
Fix models.py to handle this gracefully.
```

**5.5** Iterate on code quality:

```
Add helpful docstrings to all public methods in repository.py
```

```
Improve error messages in cli.py to be more user-friendly
```

**5.6** Refactor if needed:

If you notice code smells (duplication, long methods, etc.):

```
Refactor the create_recipe method in cli.py to extract input collection into a separate function
```

**5.7** Re-run tests after fixes:

```powershell
pytest tests/ --cov=recipe_manager
```

Ensure fixes don't break existing tests.

**5.8** Commit incremental changes:

```powershell
git add recipe_manager/ tests/
git commit -m "fix: improve validation error handling in Recipe model"
```

### Debugging Tips

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Test passes but manual test fails | Test uses mock data, real data is different | Update test fixtures |
| SQLite locked error | Database not closed properly | Add connection cleanup (context manager) |
| Type checker errors | Missing type hints or wrong types | Add/fix type annotations |
| Validation doesn't work | Validation logic not called | Ensure service calls model validation |
| Poor performance | Missing indexes, inefficient queries | Add indexes, use EXPLAIN QUERY PLAN |

### What You Learned
- Manual testing catches issues automated tests miss
- Edge case testing reveals validation gaps
- Iterative refinement improves code quality
- Small commits make debugging easier

### ✅ Checkpoint
Application works correctly in manual testing, all bugs fixed.

---

## Exercise 6: Validate Against Specification

### Goal
Systematically verify implementation meets ALL requirements from spec.md.

### Steps

**6.1** Open `spec.md` and create a validation checklist:

For each requirement, test manually:

| Requirement (from spec.md) | Test Action | Result |
|----------------------------|-------------|--------|
| Create recipe with required fields | Add recipe with title, ingredients, instructions | ✅ / ❌ |
| Title 1-200 characters | Try 0 chars, 1 char, 200 chars, 201 chars | ✅ / ❌ |
| At least 1 ingredient | Try 0 ingredients, 1 ingredient | ✅ / ❌ |
| At least 1 instruction | Try 0 instructions, 1 instruction | ✅ / ❌ |
| Prep/cook time positive or zero | Try -1, 0, 999, 1000 | ✅ / ❌ |
| Recipe retrieval < 50ms | Time with 100 recipes | ✅ / ❌ |
| Update any field except ID | Change title, try to change ID | ✅ / ❌ |
| Delete with confirmation | Delete recipe, check it's gone | ✅ / ❌ |
| Data persists after restart | Add recipe, restart app, check recipe exists | ✅ / ❌ |

**6.2** Test each acceptance criterion from spec.md:

Example criteria:
- ✅ User can create recipe in < 30 seconds
- ✅ Recipe retrievable by ID in < 50ms
- ✅ Validation errors shown with helpful messages
- ✅ All operations work offline
- ✅ Tests achieve 80%+ coverage

**6.3** Ask Copilot to validate:

```
Review the implementation in recipe_manager/ against all requirements in spec.md. 
List any missing or incomplete requirements.
```

**6.4** If requirements are missing:

```
Implement the missing "duplicate recipe" feature from spec.md section 3.4
```

**6.5** Test non-functional requirements:

- **Performance:** Run with 1000 recipes, measure query time
- **Privacy:** Verify no network calls (check with network monitoring tool)
- **Cross-platform:** Test on Windows and macOS (or in VM)
- **Offline:** Disconnect internet, verify app works

**6.6** Document validation results:

Create a validation report:
```markdown
# Core Recipe Storage — Validation Report

## Functional Requirements: ✅ All Passed
- Create recipe: ✅
- Read recipe: ✅
- Update recipe: ✅
- Delete recipe: ✅
- List recipes: ✅

## Non-Functional Requirements: ✅ All Passed
- Performance (< 50ms): ✅ Measured 12ms average
- Test coverage: ✅ 82%
- Type checking: ✅ No mypy errors
- Offline-first: ✅ No network dependencies
- Cross-platform: ✅ Tested on Windows 11

## Edge Cases: ✅ All Handled
- Empty inputs: ✅ Validation errors shown
- Boundary values: ✅ 1 char and 200 chars accepted
- Non-existent recipe: ✅ Not found error
- Data persistence: ✅ Survives restart

## Known Limitations:
- CLI only (no GUI yet) — planned for future feature
```

### What You Learned
- Systematic validation prevents missing requirements
- Acceptance criteria are testable success metrics
- Non-functional requirements need explicit testing
- Documentation of validation builds confidence

### ✅ Checkpoint
All spec.md requirements are implemented and validated.

---

## Exercise 7: Performance Validation

### Goal
Ensure implementation meets performance targets from spec.md.

### Steps

**7.1** Create performance test script:

Ask Copilot:
```
Create a performance test script that:
1. Loads 1000 recipes into the database
2. Measures average retrieval time for get_recipe_by_id
3. Reports if it meets the < 50ms requirement
```

Copilot will generate `tests/test_performance.py`.

**7.2** Run performance test:

```powershell
pytest tests/test_performance.py -v -s
```

Output might show:
```
tests/test_performance.py::test_retrieval_performance 
Average retrieval time: 8.34ms (target: < 50ms)
✅ PASSED
```

**7.3** If performance target is missed:

```
Retrieval time is 75ms (target < 50ms). How can I optimize?
```

Copilot might suggest:
- Add index on `id` column (should already exist as PRIMARY KEY)
- Use connection pooling (reuse connections)
- Profile with cProfile to find bottlenecks

**7.4** Benchmark against specification:

| Spec Requirement | Measured | Status |
|------------------|----------|--------|
| Recipe retrieval < 50ms | 8ms | ✅ Pass (6x faster) |
| App startup < 2s | 0.3s | ✅ Pass |
| No data loss on crash | Manual test | ✅ Pass (SQLite transactions) |

**7.5** Document performance characteristics:

Add to README:
```markdown
## Performance

- Recipe retrieval: ~8ms average (1000 recipes)
- Recipe creation: ~12ms average
- Full recipe list: ~25ms (1000 recipes)
- Startup time: ~300ms
- Memory footprint: ~15MB (1000 recipes loaded)
```

### What You Learned
- Performance testing validates non-functional requirements
- Benchmarks provide confidence for production use
- Optimization should be data-driven (measure first)
- Documenting performance sets user expectations

### ✅ Checkpoint
Performance meets all targets from spec.md and constitution.md.

---

## Exercise 8: Finalize and Document

### Goal
Prepare implementation for code review and merge.

### Steps

**8.1** Generate project README:

Ask Copilot:
```
Create a comprehensive README.md for the recipe_manager project including:
- Project description
- Installation instructions
- Usage examples
- Running tests
- Architecture overview
```

**8.2** Add inline documentation:

Ensure all modules have docstrings:
```python
"""
Recipe Manager — Core Storage Module

This module provides CRUD operations for recipe management using SQLite.
"""
```

**8.3** Create a CHANGELOG entry:

```markdown
# Changelog

## [0.1.0] - 2026-02-11

### Added
- Core recipe storage with CRUD operations
- SQLite database backend
- CLI interface for recipe management
- Comprehensive test suite (82% coverage)
- Type checking with mypy
- Performance validation (< 50ms retrieval)
```

**8.4** Final code cleanup:

```powershell
# Remove unused imports
ruff check --fix recipe_manager/

# Format code consistently
ruff format recipe_manager/ tests/

# Final type check
mypy recipe_manager/

# Final test run
pytest tests/ --cov=recipe_manager
```

**8.5** Commit final changes:

```powershell
git add .
git commit -m "feat: complete core recipe storage implementation

- Add Recipe model with validation
- Implement SQLite repository with CRUD
- Create service layer for business logic
- Build CLI interface for user interaction
- Achieve 82% test coverage
- Validate performance (8ms avg retrieval)
- Document all modules and usage

Closes #001-core-recipe-storage"
```

**8.6** Push feature branch:

```powershell
git push -u origin 001-core-recipe-storage
```

**8.7** Prepare for merge:

Ask Copilot:
```
Summarize the changes in this feature branch for a pull request description
```

Use the summary to create a pull request (if using GitHub).

**8.8** Celebrate! 🎉

You've successfully built a feature using Spec-Driven Development!

### What You Learned
- Documentation makes code maintainable
- Cleanup ensures code quality before merge
- Git history tells the feature development story
- Summaries help reviewers understand changes

### ✅ Checkpoint
Feature is complete, documented, tested, and ready for merge.

---

## Experiment 4 Wrap-Up

### What You Accomplished

✅ Executed automated implementation with `/speckit.implement`  
✅ Reviewed AI-generated code for quality and correctness  
✅ Ran tests and achieved 80%+ coverage requirement  
✅ Fixed issues through iterative refinement  
✅ Validated implementation against all spec.md requirements  
✅ Verified performance meets targets (< 50ms retrieval)  
✅ Documented code and usage thoroughly  
✅ Prepared feature for merge to main branch  

### Key Takeaways

1. **AI accelerates coding** — `/speckit.implement` generated entire codebase
2. **Human review is critical** — AI can make mistakes, test everything
3. **Specs drive quality** — Clear requirements = correct implementation
4. **Testing validates assumptions** — Passing tests ≠ bug-free, manual testing matters
5. **Performance is measurable** — Benchmarks prevent surprises in production

### Time Investment vs. Benefit

**30 minutes on Experiment 4 delivered:**
- Fully functional Recipe Manager core
- 82% test coverage
- Type-checked, linted code
- Performance-validated implementation
- Comprehensive documentation

**Without Spec-Driven Development, this would take:**
- 4-6 hours of manual coding
- Multiple refactoring cycles
- Hours of debugging
- Incomplete test coverage
- Missing documentation

### Implementation Quality Checklist

- [ ] All code generated from tasks.md
- [ ] Tests pass with ≥ 80% coverage
- [ ] Type checking passes (mypy clean)
- [ ] Linting passes (ruff clean)
- [ ] Manual testing confirms correct behavior
- [ ] All spec.md requirements validated
- [ ] Performance targets met and documented
- [ ] Code is documented (docstrings, README)
- [ ] Feature branch is committed and pushed
- [ ] Ready for code review and merge

---

## Next Steps

🚀 **Proceed to [Experiment 5: Advanced Features](../experiment-5/README.md)**

Learn advanced Spec Kit capabilities: analyze specifications, create quality checklists, add features iteratively, and apply Spec-Driven Development to brownfield projects.

---

## Quick Reference

See [CHEATSHEET.md](CHEATSHEET.md) for a compact reference of all Experiment 4 concepts, commands, and best practices.



