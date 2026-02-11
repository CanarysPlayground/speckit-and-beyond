# Level 1 — Quick Reference Card

## Installation Commands

| Command | Purpose |
|---------|---------|
| `uv tool install specify-cli --from git+https://github.com/github/spec-kit.git` | Install Spec Kit CLI |
| `specify --version` | Verify installation |
| `specify check` | Validate environment (Git, Python, AI agents) |
| `specify init <project-name> --ai copilot` | Initialize new Spec Kit project |
| `specify init . --ai copilot` | Initialize in current directory |
| `specify init --here --force --ai copilot` | Force initialize in non-empty directory |

## Project Structure

```
recipe-manager/
├── agents/
│   ├── speckit.constitution.agent.md
│   ├── speckit.specify.agent.md
│   ├── speckit.plan.agent.md
│   ├── speckit.tasks.agent.md
│   ├── speckit.implement.agent.md
│   └── ...                          ← Other agent definitions
├── specify/
│   ├── memory/
│   │   ├── constitution.md          ← Project principles
│   │   └── features/                ← Feature specifications
│   └── scripts/
│       └── powershell/              ← Automation scripts
│           ├── common.ps1
│           ├── create-new-feature.ps1
│           └── ...
├── templates/
│   ├── constitution-template.md
│   ├── spec-template.md
│   ├── plan-template.md
│   └── tasks-template.md
├── .vscode/
│   └── settings.json                ← Agent configuration
├── .github/
└── README.md
```

## Spec Kit Slash Commands

| Command | Purpose | When to Use |
|---------|---------|-------------|
| `/speckit.constitution` | Create/update project principles | Once at project start |
| `/speckit.specify` | Define feature specifications | Start of each new feature |
| `/speckit.clarify` | Ask clarifying questions | After specify, before plan |
| `/speckit.plan` | Create technical implementation plan | After specifications are clear |
| `/speckit.tasks` | Break plan into actionable tasks | After plan is approved |
| `/speckit.implement` | Execute task list, generate code | Final implementation step |
| `/speckit.analyze` | Check consistency and coverage | Before implementation |
| `/speckit.checklist` | Generate quality validation checklists | Anytime for validation |

## Spec-Driven Development Workflow

```
Constitution → Specify → Clarify → Plan → Tasks → Implement
      ↑______________(iterate anytime)_________________↓
```

## Constitution Best Practices

### What to Include

- **Core Principles** — Values that guide decisions (UX, performance, privacy, etc.)
- **Development Standards** — Code quality, testing, documentation requirements
- **Technical Constraints** — Platform limitations, required technologies
- **User Focus** — Target audience, accessibility needs
- **Quality Metrics** — Performance benchmarks, coverage targets

### Example Principle Structure

```markdown
## Principle: [Name]
**Priority:** High/Medium/Low
**Rationale:** Why this matters
**Implications:** How it affects development
**Examples:** Concrete scenarios
```

## Key Concepts

### Spec-Driven vs. Traditional Development

| Aspect | Traditional | Spec-Driven |
|--------|-------------|-------------|
| Starting point | Write code | Write specifications |
| Requirements | Emerge during coding | Defined upfront, AI-executable |
| AI role | Code suggestions | Full workflow orchestration |
| Iteration | Refactor code | Update specs, regenerate |
| Documentation | After coding (often outdated) | Specifications are living docs |

### Constitution as AI Constraints

The constitution acts as:
- **Decision framework** — Guides AI when choices arise
- **Quality gates** — Prevents non-compliant solutions
- **Team alignment** — Single source of truth for priorities
- **Scope control** — Prevents feature creep

## Git Integration

```powershell
# Initial commit after setup
git add .
git commit -m "feat: add project constitution and Spec Kit setup"

# Create feature branch (before /speckit.specify)
git checkout -b 001-core-recipe-storage

# Commit after each major step
git commit -m "feat: add specification for core recipe storage"
git commit -m "feat: add implementation plan for core recipe storage"
git commit -m "feat: implement core recipe storage"
```

## Troubleshooting

| Problem | Solution |
|---------|----------|
| `specify: command not found` | Add uv tools to PATH or reinstall: `uv tool install specify-cli --force ...` |
| Slash commands not appearing in Copilot | Restart VS Code, ensure `.vscode/settings.json` and `agents/*.agent.md` exist |
| Permission errors on Windows | Run PowerShell as Administrator or adjust execution policy: `Set-ExecutionPolicy RemoteSigned -Scope CurrentUser` |
| Scripts not executing | Check if `specify/scripts/powershell/*.ps1` files are accessible |
| Copilot ignores constitution | Explicitly mention: "Based on specify/memory/constitution.md, ..." in prompts |

## Environment Variables

| Variable | Purpose | Example |
|----------|---------|---------|
| `SPECIFY_FEATURE` | Override feature detection (non-Git repos) | `SPECIFY_FEATURE=001-core-storage` |
| `GH_TOKEN` or `GITHUB_TOKEN` | GitHub API authentication (corporate networks) | `GH_TOKEN=ghp_xxxxx` |

## Supported AI Agents

| Agent | CLI Flag | Status |
|-------|----------|--------|
| GitHub Copilot | `--ai copilot` | ✅ Fully supported |
| Claude Code | `--ai claude` | ✅ Fully supported |
| Cursor | `--ai cursor-agent` | ✅ Fully supported |
| Windsurf | `--ai windsurf` | ✅ Fully supported |
| Gemini CLI | `--ai gemini` | ✅ Fully supported |
| Amazon Q Developer | `--ai q` | ⚠️ No custom arguments for slash commands |
| Qwen Code | `--ai qwen` | ✅ Fully supported |
| OpenCode | `--ai opencode` | ✅ Fully supported |

## Quick Wins

1. **Fast prototype:** Use `/speckit.constitution` with 3 principles, then jump to `/speckit.plan`
2. **Team alignment:** Share constitution.md first, get buy-in before coding
3. **Learn the tool:** Start with a tiny feature (single page, one function)
4. **Debug mode:** Use `--debug` flag if scripts fail: `specify init --debug`
5. **Version control:** Commit after each slash command to track evolution

## Resources

- **Official Docs:** https://github.github.io/spec-kit/
- **GitHub Repo:** https://github.com/github/spec-kit
- **Spec-Driven Guide:** https://github.com/github/spec-kit/blob/main/spec-driven.md
- **Video Tutorial:** https://www.youtube.com/watch?v=a9eR1xsfvHg

## Next Level

[Level 2: Specify & Clarify →](../level-2/README.md)
