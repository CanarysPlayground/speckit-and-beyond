# Experiment 4 — Quick Reference Card

## Implementation Commands

| Command | Purpose | When to Use |
|---------|---------|-------------|
| `/speckit.implement` | Execute all tasks, generate code | After plan.md and tasks.md are finalized |

## Implementation Workflow

```
1. Pre-check: Git clean, Python ready, specs finalized
    ↓
2. Run /speckit.implement
    ↓
3. Review generated code (architecture, quality)
    ↓
4. Install dependencies: pip install -r requirements.txt
    ↓
5. Run tests: pytest tests/ --cov=recipe_manager
    ↓
6. Fix issues, iterate as needed
    ↓
7. Validate against spec.md (all requirements)
    ↓
8. Performance validation (benchmarks)
    ↓
9. Document and finalize
    ↓
10. Commit and push for review
```

## Test Commands

```powershell
# Run all tests
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=recipe_manager --cov-report=term-missing

# Run specific test file
pytest tests/test_models.py -v

# Run specific test
pytest tests/test_models.py::test_valid_recipe_creation -v

# Generate HTML coverage report
pytest tests/ --cov=recipe_manager --cov-report=html
# Opens htmlcov/index.html

# Run with verbose output (print statements)
pytest tests/ -v -s

# Run tests in parallel (faster)
pytest tests/ -n auto
```

## Code Quality Commands

```powershell
# Lint Python code
ruff check recipe_manager/ tests/

# Auto-fix linting issues
ruff check --fix recipe_manager/ tests/

# Format code consistently
ruff format recipe_manager/ tests/

# Type checking
mypy recipe_manager/

# Type checking with detailed errors
mypy --show-error-codes recipe_manager/

# Check for unused imports
ruff check --select F401 recipe_manager/
```

## Code Review Checklist

### Architecture
- [ ] Layers are separate (Models, Repository, Service, UI)
- [ ] No circular dependencies
- [ ] Dependency injection used (testability)
- [ ] Business logic in Service, not UI or Repository

### Code Quality
- [ ] No hardcoded values (use config/constants)
- [ ] Type hints on all functions
- [ ] Docstrings on public methods
- [ ] Descriptive variable/function names
- [ ] Functions are small (< 50 lines)
- [ ] Error messages are user-friendly

### Security
- [ ] SQL queries use parameterized statements (no injection)
- [ ] No sensitive data in logs
- [ ] Input validation on all user inputs
- [ ] File paths validated (no directory traversal)

### Testing
- [ ] Unit tests for business logic
- [ ] Integration tests for database operations
- [ ] Edge cases covered (empty, boundary, invalid input)
- [ ] Error scenarios tested
- [ ] Coverage ≥ 80%

### Performance
- [ ] Database indexes on frequently queried columns
- [ ] Connections reused (not opened/closed repeatedly)
- [ ] Queries optimized (SELECT specific columns)
- [ ] No N+1 query problems

## Common Issues and Fixes

| Issue | Symptom | Fix |
|-------|---------|-----|
| **Import errors** | ModuleNotFoundError | Install dependencies: `pip install -r requirements.txt` |
| **SQLite locked** | Database is locked error | Add connection cleanup, use context manager |
| **Type errors** | mypy complains about types | Add type hints: `def func(x: int) -> str:` |
| **Tests fail** | AssertionError | Check test assumptions vs. implementation |
| **Low coverage** | Coverage < 80% | Add tests for uncovered lines (see --cov-report) |
| **Slow queries** | Performance < target | Add indexes, profile with EXPLAIN QUERY PLAN |
| **Validation not working** | Invalid data accepted | Ensure service calls model validation |
| **Hardcoded paths** | Works locally, fails elsewhere | Use pathlib, environment variables |

## Testing Best Practices

### Test Structure (Arrange-Act-Assert)

```python
def test_create_recipe_with_valid_data():
    # Arrange: Set up test data
    recipe = Recipe(
        title="Pasta",
        ingredients=["pasta", "sauce"],
        instructions=["boil pasta", "add sauce"]
    )
    repository = RecipeRepository(":memory:")
    
    # Act: Perform action
    created = repository.create(recipe)
    
    # Assert: Verify outcome
    assert created.id is not None
    assert created.title == "Pasta"
```

### Test Naming

| ❌ Bad | ✅ Good |
|-------|--------|
| `test1` | `test_create_recipe_with_valid_data` |
| `test_recipe` | `test_recipe_validation_fails_for_empty_title` |
| `test_error` | `test_repository_raises_not_found_error_for_invalid_id` |

### Test Coverage Targets

| Component | Target Coverage | Rationale |
|-----------|----------------|-----------|
| **Models** | 95-100% | Pure logic, easy to test |
| **Repository** | 85-95% | Integration tests, some error paths hard to trigger |
| **Service** | 85-95% | Business logic, mock repository |
| **CLI** | 50-70% | UI harder to test, manual testing supplements |

## Performance Validation

### Timing Code

```python
import time

start = time.perf_counter()
recipe = repository.get(recipe_id)
elapsed = (time.perf_counter() - start) * 1000  # milliseconds
print(f"Retrieval time: {elapsed:.2f}ms")
assert elapsed < 50, f"Too slow: {elapsed}ms"
```

### Profiling

```powershell
# Profile a script
python -m cProfile -s cumtime recipe_manager/cli.py

# Profile with visual output
pip install snakeviz
python -m cProfile -o profile.stats recipe_manager/cli.py
snakeviz profile.stats
```

### SQLite Query Analysis

```python
import sqlite3

conn = sqlite3.connect("recipes.db")
cursor = conn.cursor()

# Explain query plan
cursor.execute("EXPLAIN QUERY PLAN SELECT * FROM recipes WHERE id = ?", (recipe_id,))
print(cursor.fetchall())

# Check indexes
cursor.execute("SELECT name, sql FROM sqlite_master WHERE type='index'")
print(cursor.fetchall())
```

## Git Workflow for Implementation

```powershell
# Before implementation
git status  # Ensure clean working tree
git tag backup-before-implementation

# During implementation (incremental commits)
git add recipe_manager/models.py tests/test_models.py
git commit -m "feat: add Recipe model with validation"

git add recipe_manager/repository.py tests/test_repository.py
git commit -m "feat: implement RecipeRepository with CRUD operations"

# After implementation (final commit)
git add .
git commit -m "feat: complete core recipe storage implementation

- Add Recipe model with validation
- Implement SQLite repository with CRUD
- Create service layer for business logic
- Build CLI interface
- Achieve 82% test coverage
- Validate performance (8ms avg retrieval)

Closes #001-core-recipe-storage"

# Push for review
git push -u origin 001-core-recipe-storage

# If you need to rollback
git reset --hard backup-before-implementation
```

## Validation Checklist (from spec.md)

Go through each requirement systematically:

```markdown
## Functional Requirements Validation

- [ ] Create recipe with all required fields
  - Test: Add recipe manually via CLI
  - Expected: Recipe saved and retrievable

- [ ] Validate title (1-200 chars)
  - Test: Try 0, 1, 200, 201 characters
  - Expected: 1-200 accepted, 0 and 201 rejected with error

- [ ] Validate ingredients (at least 1)
  - Test: Try 0 ingredients, 1 ingredient
  - Expected: 0 rejected, 1 accepted

- [ ] Update recipe
  - Test: Change title, ingredients, instructions
  - Expected: Changes persist

- [ ] Delete recipe
  - Test: Delete recipe, try to retrieve
  - Expected: Recipe not found

- [ ] List all recipes
  - Test: Add 3 recipes, list all
  - Expected: All 3 appear with correct data

## Non-Functional Requirements Validation

- [ ] Performance (< 50ms retrieval)
  - Test: Load 1000 recipes, measure get_by_id time
  - Expected: Average < 50ms

- [ ] Test coverage (≥ 80%)
  - Test: pytest --cov=recipe_manager
  - Expected: Report shows ≥ 80%

- [ ] Offline-first (no network)
  - Test: Disconnect internet, use app
  - Expected: Full functionality works

- [ ] Cross-platform
  - Test: Run on Windows and macOS/Linux
  - Expected: Works identically

- [ ] Data persistence
  - Test: Add recipe, close app, reopen
  - Expected: Recipe still exists
```

## Documentation Checklist

- [ ] **README.md**: Project description, installation, usage
- [ ] **Module docstrings**: Purpose of each file
- [ ] **Class docstrings**: What the class does
- [ ] **Method docstrings**: Parameters, return types, examples
- [ ] **CHANGELOG.md**: Version history
- [ ] **requirements.txt**: All dependencies with versions
- [ ] **Comments**: Complex logic explained

### Example Docstring

```python
def create_recipe(self, recipe: Recipe) -> Recipe:
    """
    Create a new recipe in the database.
    
    Args:
        recipe: Recipe object with title, ingredients, and instructions.
                ID will be auto-generated if not provided.
    
    Returns:
        Recipe object with ID and timestamps populated.
    
    Raises:
        ValidationError: If recipe validation fails.
        StorageError: If database write fails.
    
    Example:
        >>> recipe = Recipe(title="Pasta", ingredients=["pasta"], instructions=["boil"])
        >>> created = repository.create(recipe)
        >>> print(created.id)
        'a1b2c3d4-...'
    """
    recipe.validate()  # Ensure valid before saving
    # ... rest of implementation
```

## Troubleshooting

| Problem | Diagnostic | Solution |
|---------|------------|----------|
| `/speckit.implement` fails | Check spec.md, plan.md, tasks.md exist | Ensure Experiment 3 completed |
| Code generated but wrong | Review plan.md clarity | Update plan.md, re-run implement |
| Tests fail after generation | Check test assumptions | Fix implementation or test |
| Import errors | Check `pip list` | `pip install -r requirements.txt` |
| Type errors | Run `mypy` | Add type hints, fix return types |
| Linting errors | Run `ruff check` | `ruff check --fix` (auto-fix) |
| Performance issues | Profile with cProfile | Optimize hot paths, add indexes |
| Git conflicts | Check branch | Merge main into feature branch |

## Resources

- **pytest docs:** https://docs.pytest.org/
- **mypy docs:** https://mypy.readthedocs.io/
- **ruff docs:** https://docs.astral.sh/ruff/
- **SQLite performance:** https://www.sqlite.org/optoverview.html
- **Python testing best practices:** https://realpython.com/python-testing/

## Next Level

[Experiment 5: Advanced Features →](../level-5/README.md)

