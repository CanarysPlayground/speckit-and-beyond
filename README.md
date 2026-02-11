# Getting Started with GitHub Spec Kit

> **Learn Spec-Driven Development**: Build high-quality software faster by defining intent before implementation.

## Description

**GitHub Spec Kit** is an open-source toolkit that revolutionizes software development by emphasizing **specifications over vibe coding**. Instead of jumping straight into code, Spec Kit guides you through a structured process: define project principles, create rich specifications, generate implementation plans, break down tasks, and execute with AI assistance. This workshop teaches you how to master Spec-Driven Development through hands-on exercises building a real application.

### Key Characteristics

- **Intent-driven development** — Define "what" and "why" before "how"
- **Works with multiple AI agents** — GitHub Copilot, Claude Code, Cursor, Windsurf, and more
- **Multi-step refinement** — Structured process from principles to implementation
- **Technology independent** — Use any tech stack, language, or framework
- **Editor integration** — Built-in slash commands for seamless workflow

---

## Workshop Overview

**Total Duration:** 2 hours  
**Format:** Hands-on exercises with progressive difficulty  
**Application:** Recipe Manager (a practical, feature-rich app everyone can understand)

### What You'll Build

Throughout this workshop, you'll build a **Recipe Manager Application** that allows users to:
- Store and organize their favorite recipes
- Search recipes by ingredients, cuisine, or dietary restrictions
- Create shopping lists from recipe ingredients
- Share recipes with friends and family
- Rate and review recipes

This real-world application demonstrates how Spec Kit transforms high-level requirements into working software through a structured, AI-assisted process.

---

## Requirements

- **Valid GitHub Copilot subscription** (Individual, Business, or Enterprise)
- **VS Code** installed with GitHub Copilot extension
- **Python 3.11+** ([Download](https://www.python.org/downloads/))
- **uv** package manager ([Install](https://docs.astral.sh/uv/))
- **Git** version control ([Download](https://git-scm.com/downloads))
- **Basic understanding** of software development concepts

---

## Workshop Structure

| Level | Topic | Duration | Focus |
|-------|-------|----------|-------|
| **1** | Setup & Constitution | 20 min | Install Spec Kit, initialize project, create governing principles |
| **2** | Specify & Clarify | 25 min | Define requirements, ask clarifying questions, refine specifications |
| **3** | Plan & Tasks | 25 min | Create technical plans, break down into actionable tasks |
| **4** | Implement & Validate | 30 min | Execute implementation, validate against specs |
| **5** | Advanced Features | 20 min | Analyze quality, add features iteratively, best practices |

---

## Installation

### Step 1: Install Spec Kit

**Option 1: Persistent Installation (Recommended)**

```powershell
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

**Option 2: One-time Usage**

```powershell
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

### Step 2: Verify Installation

```powershell
specify --help
specify check
```

### Step 3: Ensure GitHub Copilot is Active

Open VS Code and verify:
- GitHub Copilot extension is installed and enabled
- You're signed in with a valid subscription
- Copilot Chat is accessible (Ctrl+Alt+I or Cmd+Opt+I)

---

## Learning Path

### 🟢 Level 1: Setup & Constitution (Beginner)
**Risk level:** Zero Risk — Setting up environment and defining principles

Learn how to:
- Initialize a Spec Kit project
- Create a constitution with project principles
- Understand the Spec-Driven Development workflow
- Set up your development environment

### 🟢 Level 2: Specify & Clarify (Beginner)
**Risk level:** Zero Risk — Read-only specification work

Learn how to:
- Write effective specifications focused on "what" not "how"
- Use `/speckit.clarify` to explore underspecified areas
- Refine requirements through AI-guided questions
- Create user-centric feature descriptions

### 🟡 Level 3: Plan & Tasks (Intermediate)
**Risk level:** Low Risk — Planning and task breakdown

Learn how to:
- Create technical implementation plans with your chosen tech stack
- Use `/speckit.plan` to define architecture
- Break down plans into actionable tasks with `/speckit.tasks`
- Review and prioritize task lists

### 🟡 Level 4: Implement & Validate (Intermediate)
**Risk level:** Medium Risk — Actual code generation and validation

Learn how to:
- Execute implementation with `/speckit.implement`
- Validate generated code against specifications
- Debug and fix implementation issues
- Test the generated application

### 🟠 Level 5: Advanced Features (Advanced)
**Risk level:** Medium Risk — Quality analysis and iteration

Learn how to:
- Use `/speckit.analyze` for consistency checks
- Create quality checklists with `/speckit.checklist`
- Add features iteratively to existing codebase
- Apply Spec-Driven Development to brownfield projects

---

## Key Concepts

### Spec-Driven Development Process

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  1. Constitution → 2. Specify → 3. Plan → 4. Tasks    │
│                                     ↓                    │
│                              5. Implement               │
│                                     ↓                    │
│                      6. Validate & Iterate              │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### Why Spec-Driven Development?

**Traditional Approach:**
- Start coding immediately
- Requirements emerge during implementation
- High risk of rework and misalignment
- Specifications become outdated documentation

**Spec-Driven Approach:**
- Define intent and principles first
- Create executable specifications
- Generate implementation from specs
- Specifications drive the code

### The Power of Specifications

- **Clarity:** Forces you to think through requirements before coding
- **Alignment:** Ensures team and stakeholders agree on goals
- **Validation:** Provides a baseline to verify implementation
- **Iteration:** Makes changes easier by updating specs first
- **Documentation:** Living specs that evolve with code

---

## Workshop Guidelines

### How to Use This Workshop

1. **Follow in sequence** — Each level builds on previous knowledge
2. **Complete all exercises** — Hands-on practice is essential
3. **Experiment freely** — Spec Kit is forgiving; you can always regenerate
4. **Read checkpoints** — Verify understanding before moving forward
5. **Use cheat sheets** — Quick reference cards in each level
6. **Ask questions** — Use GitHub Copilot Chat for help anytime

### Time Management

- **Strict mode:** Follow time estimates, skip "Extra Challenge" sections
- **Deep dive mode:** Complete all exercises including challenges
- **Self-paced:** Take breaks between levels, revisit concepts

### Best Practices

- 📝 **Write clear specifications** — Be specific about "what" and "why"
- 🎯 **Focus on outcomes** — Describe desired behavior, not implementation
- 🔄 **Iterate** — Refine specs based on clarification questions
- ✅ **Validate early** — Check each step before proceeding
- 🧪 **Test thoroughly** — Verify implementation matches specifications

---

## Troubleshooting

### Common Issues

**Issue:** `specify: command not found`  
**Solution:** Ensure uv tool installation path is in your PATH:
```powershell
# Check if specify is installed
uv tool list

# If missing, reinstall
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

**Issue:** GitHub Copilot not responding to `/speckit.*` commands  
**Solution:**
1. Ensure you've run `specify init` in your project
2. Restart VS Code
3. Verify `.speckit/` directory exists in your project root

**Issue:** Python version error  
**Solution:** Install Python 3.11 or higher:
```powershell
python --version  # Check current version
# Download from python.org if needed
```

**Issue:** uv not installed  
**Solution:** Install uv package manager:
```powershell
# Windows (PowerShell)
irm https://astral.sh/uv/install.ps1 | iex

# Verify installation
uv --version
```

---

## Additional Resources

- **Official Documentation:** [https://github.github.io/spec-kit/](https://github.github.io/spec-kit/)
- **GitHub Repository:** [https://github.com/github/spec-kit](https://github.com/github/spec-kit)
- **Spec-Driven Development Guide:** [spec-driven.md](https://github.com/github/spec-kit/blob/main/spec-driven.md)
- **Video Overview:** [Watch on YouTube](https://www.youtube.com/watch?v=a9eR1xsfvHg)

---

## Contributing

Found an issue or have suggestions for improving this workshop? Please open an issue or submit a pull request!

---

## License

This workshop is licensed under MIT License. See [LICENSE](LICENSE) for details.

---

## Ready to Start?

🚀 **Begin with [Level 1: Setup & Constitution](workshop/level-1/README.md)**

Master the fundamentals of Spec-Driven Development and set up your Recipe Manager project!
