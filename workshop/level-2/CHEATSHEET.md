# Level 2 — Quick Reference Card

## Specification Commands

| Command | Purpose | Output |
|---------|---------|--------|
| `/speckit.specify` | Create feature specification | `.speckit/features/XXX-feature-name/spec.md` |
| `/speckit.clarify` | Generate clarifying questions | Updates to `spec.md` with refined details |

## Git Workflow for Features

```powershell
# Create feature branch (format: XXX-feature-name)
git checkout -b 001-core-recipe-storage

# Check current branch
git branch

# Commit specification
git add .speckit/features/001-core-recipe-storage/spec.md
git commit -m "feat: add specification for core recipe storage"

# Push for team review (optional)
git push -u origin 001-core-recipe-storage
```

## Specification Structure

```markdown
# Feature Name Specification

## Overview
Brief description (2-3 sentences)

## User Stories
- As a [user type], I want to [action] so that [benefit]

## Functional Requirements
- Requirement 1: Detailed description
- Requirement 2: ...

## Data Model
### Entity Name
- field_name: type (constraints, required/optional)

## Validation Rules
- Field: Constraint description

## Error Handling
- Scenario → Error message / behavior

## Non-Functional Requirements
- Performance: Metrics (< Xms)
- Security: Requirements
- Privacy: Constraints
- Accessibility: Standards

## Out of Scope
- Feature X (future work)
- Feature Y (separate feature)

## Acceptance Criteria
✅ Testable success metric 1
✅ Testable success metric 2
```

## Spec-Driven Writing Principles

### Focus on "What" Not "How"

| ❌ Implementation Detail | ✅ Outcome-Focused |
|-------------------------|-------------------|
| "Use SQLite database" | "Store recipes persistently locally" |
| "Create a FastAPI endpoint" | "Provide recipe creation capability" |
| "Use React components" | "Display recipe list to user" |
| "Implement JWT auth" | "Ensure only authorized users access recipes" |

### Be Specific, Not Vague

| ❌ Vague | ✅ Specific |
|---------|------------|
| "Fast performance" | "Recipe retrieval in < 50ms" |
| "User-friendly" | "Create recipe in < 30 seconds, max 3 clicks" |
| "Robust error handling" | "Display validation errors within 200ms with actionable message" |
| "Many recipes" | "Support up to 10,000 recipes without performance degradation" |

## Clarification Question Categories

When you run `/speckit.clarify`, expect questions in these categories:

1. **Edge Cases** — What happens when...?
2. **Data Constraints** — Max/min values, formats, validation
3. **User Workflows** — Alternate paths, cancellation, undo
4. **Error Scenarios** — Network failure, invalid input, conflicts
5. **Scope Boundaries** — What's included vs. future features
6. **Non-Functional** — Performance targets, accessibility, security
7. **Assumptions** — Are these true for your use case?

## Common Clarification Questions for Recipe Manager

| Topic | Example Questions |
|-------|-------------------|
| **Data** | Free-text ingredients or structured (quantity/unit/name)? |
| **Duplicates** | Allow duplicate recipe titles? Duplicate detection? |
| **Ordering** | Can users reorder instruction steps or ingredients? |
| **Scaling** | Auto-scale ingredient quantities when servings change? |
| **Media** | Support photos? Videos? External links? |
| **Import** | Import from other formats (JSON, PDF, websites)? |
| **Sharing** | Email export? Print formatting? Social media? |
| **Categories** | Tags, cuisine types, dietary restrictions? |
| **Search** | Full-text search? Filters? Autocomplete? |
| **Versions** | Track recipe edits? Revert to previous versions? |

## Constitution Validation Checklist

Before finalizing spec, verify alignment:

- [ ] **Privacy:** Does spec require cloud services if constitution says local-only?
- [ ] **Performance:** Are targets consistent with constitution benchmarks?
- [ ] **Code Quality:** Does spec include testing requirements (coverage, etc.)?
- [ ] **Accessibility:** Are WCAG or other standards referenced if required?
- [ ] **Cross-Platform:** Does spec assume platform-specific features?
- [ ] **Extensibility:** Does design allow future plugins/extensions?
- [ ] **User Focus:** Are target users' needs prioritized?
- [ ] **Constraints:** Are all constitution constraints honored?

## Specification Quality Checklist

Use this before moving to Level 3:

- [ ] All user stories use "As a [role], I want [action] so that [benefit]"
- [ ] Functional requirements are complete (no "TBD")
- [ ] Data model defines all fields with types, constraints, required/optional
- [ ] Validation rules are specific with examples
- [ ] Error messages are documented
- [ ] Non-functional requirements have measurable targets
- [ ] Acceptance criteria are testable (not subjective)
- [ ] "Out of Scope" section exists and is clear
- [ ] No technology names (SQL, React, FastAPI, etc.) in spec
- [ ] No implementation terms (class, function, API endpoint)
- [ ] Specification is committed to Git

## Example User Story Templates

```
USER STORY:
As a [user role],
I want to [perform action]
So that [achieve benefit]

ACCEPTANCE CRITERIA:
- Given [context], when [action], then [expected outcome]
- Given [context], when [action], then [expected outcome]
```

**Example for Recipe Manager:**
```
USER STORY:
As a home cook,
I want to edit a recipe's ingredients
So that I can improve the recipe based on my experience

ACCEPTANCE CRITERIA:
- Given I'm viewing a recipe, when I click "Edit", then all fields become editable
- Given I've edited ingredients, when I click "Save", then changes persist and are visible immediately
- Given I've edited ingredients, when I click "Cancel", then changes are discarded and original recipe is shown
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `/speckit.specify` fails | Ensure you're on a feature branch (`git checkout -b XXX-name`) |
| Spec contains tech details | Refactor to focus on outcomes. Move tech choices to Level 3 (planning) |
| Clarify has no questions | Spec might be too vague. Add more details and run clarify again |
| Spec conflicts with constitution | Update spec to align, or update constitution if priorities changed |
| Can't commit spec.md | Check file path: should be `.speckit/features/XXX-name/spec.md` |

## Pro Tips

1. **Start with user stories** — They anchor everything else
2. **Include examples** — Sample recipe JSON, error messages, validation scenarios
3. **Define "done"** — Acceptance criteria must be measurable
4. **Iterate on clarify** — Run `/speckit.clarify` multiple times as spec evolves
5. **Review out loud** — Reading spec aloud catches ambiguities
6. **Validate early** — Check constitution alignment before finalizing
7. **Version control** — Commit after each major refinement
8. **Get human review** — AI-generated specs benefit from domain experts

## Resources

- **Example Specs:** https://github.com/github/spec-kit/tree/main/examples
- **Spec-Driven Guide:** https://github.com/github/spec-kit/blob/main/spec-driven.md
- **User Story Best Practices:** https://www.atlassian.com/agile/project-management/user-stories

## Next Level

[Level 3: Plan & Tasks →](../level-3/README.md)
