# Getting Started with GitHub Spec Kit

> **Learn Spec-Driven Development**: Build high-quality software faster by defining intent before implementation.

## Description

This workshop teaches you how to **accelerate your development process** by combining three powerful AI-enhanced tools: **GitHub Spec Kit** for structured development, **instruction files** for domain-specific AI assistance, and **custom skills** for methodology coaching. Together, these tools transform how you build software - from vague ideas to production-ready code.

**What makes this workshop different:**

Instead of just learning Spec Kit commands, you'll discover how to create an **AI-powered development environment** where:
- **Spec Kit** structures your workflow (constitution → specify → plan → tasks → implement)
- **Instruction files** teach GitHub Copilot your domain (recipe measurements, dietary restrictions, validation rules)
- **Custom skills** (@SpecKitCoach) provide on-demand methodology coaching

**The result?** AI generates better code because it understands both your project principles AND your domain context. You move faster because specialized agents coach you through best practices without leaving your editor.

### Key Characteristics

- **AI-enhanced workflow** — GitHub Copilot becomes domain-aware through instruction files
- **Structured methodology** — Spec Kit guides you from principles to implementation  
- **On-demand coaching** — custom skills provide methodology help without context switching
- **Works with multiple AI tools** — GitHub Copilot, Claude Code, Cursor, Windsurf, and more
- **Technology independent** — Use any tech stack, language, or framework
- **Reusable patterns** — Take instruction files, and custom skills to your own projects

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

> 💡 **Workshop Enhancement:** This workshop includes AI-powered support files (recipe domain instructions and SpecKitCoach skill) that will be downloaded in Experiment 1. These files significantly improve your learning experience by providing domain-specific AI assistance and methodology coaching.

---

## Workshop Structure

| Experiment | Topic | Duration | Focus |
|------------|-------|----------|-------|
| **1** | Setup & Constitution | 20 min | Install Spec Kit, initialize project, download workshop files, create constitution |
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

### 🟢 Experiment 1: Setup & Constitution

Learn how to:
- Initialize a Spec Kit project
- Create a constitution with project principles
- Understand the Spec-Driven Development workflow
- Set up your development environment

### 🟢 Experiment 2: Specify & Clarify 

Learn how to:
- Write effective specifications focused on "what" not "how"
- Use `/speckit.clarify` to explore underspecified areas
- Refine requirements through AI-guided questions
- Create user-centric feature descriptions

### 🟡 Experiment 3: Plan & Tasks 

Learn how to:
- Create technical implementation plans with your chosen tech stack
- Use `/speckit.plan` to define architecture
- Break down plans into actionable tasks with `/speckit.tasks`
- Review and prioritize task lists

### 🟡 Experiment 4: Implement & Validate 

Learn how to:
- Execute implementation with `/speckit.implement`
- Validate generated code against specifications
- Debug and fix implementation issues
- Test the generated application

### 🟠 Experiment 5: Advanced Features (Advanced)

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
│  1. Constitution → 2. Specify → 3. Plan → 4. Tasks      │
│                                      ↓                   │
│                              5. Implement              │
│                                     ↓                   │
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

1. **Follow in sequence** — Each experiment builds on previous knowledge
2. **Complete all exercises** — Hands-on practice is essential
3. **Experiment freely** — Spec Kit is forgiving; you can always regenerate
4. **Read checkpoints** — Verify understanding before moving forward
5. **Use cheat sheets** — Quick reference cards in each experiment
6. **Ask questions** — Use GitHub Copilot Chat or @SpecKitCoach for help anytime

### Time Management

- **Strict mode:** Follow time estimates, skip "Extra Challenge" sections
- **Deep dive mode:** Complete all exercises including challenges
- **Self-paced:** Take breaks between experiments, revisit concepts

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

## Ready to Start?

🚀 **Begin with [Experiment 1: Setup & Constitution](workshop/experiment-1/README.md)**

Master the fundamentals of Spec-Driven Development and set up your Recipe Manager project!


