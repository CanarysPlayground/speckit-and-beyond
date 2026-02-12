# Experiment 1: Setup & Constitution — Building the Foundation

> **Risk level:** 🟢 Zero — This experiment only sets up your environment and defines project principles.

## Learning Objectives

By the end of this level, you will be able to:

1. Install and configure Spec Kit CLI successfully
2. Initialize a Spec-Driven Development project structure
3. Understand the `.speckit/` directory and its purpose
4. Create a project constitution with governing principles
5. Define development guidelines that shape all future work
6. Understand the role of principles in automated development
7. Navigate the Spec Kit workflow and slash commands
8. Verify your development environment is ready
9. Understand the difference between greenfield and brownfield projects
10. Set up VS Code with proper Spec Kit integration

---

## Prerequisites

- [ ] VS Code installed with GitHub Copilot extension enabled
- [ ] Python 3.11+ installed (`python --version`)
- [ ] uv package manager installed (`uv --version`)
- [ ] Git installed (`git --version`)
- [ ] Valid GitHub Copilot subscription
- [ ] Internet connection (for AI model access)

---

## Workshop Structure

This experiment contains **9 exercises**, building progressively. Estimated time: **20 minutes**.

| Exercise | Topic | Time |
|----------|-------|------|
| 1 | Install Spec Kit CLI | 2 min |
| 2 | Initialize Your First Project | 3 min |
| 3 | Download Workshop Support Files | 3 min |
| 4 | Explore the Project Structure | 2 min |
| 5 | Verify Spec Kit Integration | 2 min |
| 6 | Create Your Constitution | 4 min |
| 7 | Understanding Principles | 2 min |
| 8 | Testing the Workflow | 1 min |
| 9 | Environment Validation | 1 min |

---

## The Story: Recipe Manager Application

Throughout this workshop, we'll build a **Recipe Manager** — a practical application that helps users organize, search, and share their favorite recipes.

### User Story
*"As a home cook, I want to store my family recipes digitally so I can access them from any device, search by ingredients, generate shopping lists, and share them with friends."*

### Why This Application?
- **Familiar domain** — Everyone understands recipes
- **Rich features** — Multiple user workflows to explore
- **Clear requirements** — Easy to specify without technical jargon
- **Iterative potential** — Start simple, add complexity progressively
- **Real-world value** — Actually useful application

---

## Exercise 1: Install Spec Kit CLI

### Goal
Get Spec Kit CLI installed and ready to use on your system.

### Steps

**1.1** Open PowerShell (Windows) or Terminal (macOS/Linux)

**1.2** Verify uv is installed:

```powershell
uv --version
```

> 💡 **If uv is not installed:**
> ```powershell
> # Windows (PowerShell)
> irm https://astral.sh/uv/install.ps1 | iex
> 
> # macOS/Linux
> curl -LsSf https://astral.sh/uv/install.sh | sh
> ```

**1.3** Install Spec Kit CLI:

```powershell
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

**1.4** Verify installation:

```powershell
specify --version
```

You should see: `specify-cli, version 0.0.x` (exact version may vary)

**1.5** Check system requirements:

```powershell
specify check
```

This command validates:
- ✅ Git is installed
- ✅ Python version is adequate
- ✅ AI agents (copilot, claude, cursor, etc.) are available
- ✅ Required tools are in PATH

### What You Learned
- Spec Kit is distributed as a Python CLI tool via uv
- `specify check` helps diagnose environment issues before starting
- The tool integrates with multiple AI coding agents

### ✅ Checkpoint
You can run `specify --version` and `specify check` successfully.

---

## Exercise 2: Initialize Your First Project

### Goal
Create a new Spec-Driven Development project structure for Recipe Manager.

### Steps

**2.1** Navigate to where you want to create your project:

```powershell
cd $HOME\Documents\Projects
# Or any directory you prefer
```

**2.2** Initialize the Recipe Manager project:

```powershell
specify init recipe-manager --ai copilot
```

This command:
- Creates a `recipe-manager/` directory
- Initializes a Git repository
- Downloads Spec Kit templates
- Configures slash commands for GitHub Copilot
- Sets up the `.speckit/` directory structure

**2.3** Navigate into the project:

```powershell
cd recipe-manager
```

**2.4** Open the project in VS Code:

```powershell
code .
```

### Understanding the `--ai` Flag

Spec Kit supports multiple AI coding agents:
- `--ai copilot` → GitHub Copilot (VS Code)
- `--ai claude` → Claude Code
- `--ai cursor-agent` → Cursor
- `--ai windsurf` → Windsurf
- `--ai gemini` → Google Gemini

For this workshop, we use **GitHub Copilot** since it's widely accessible.

### What You Learned
- `specify init` scaffolds a complete project structure
- The `--ai` flag configures integration with your preferred AI agent
- Spec Kit automatically initializes Git for version control

### ✅ Checkpoint
You have a `recipe-manager/` directory open in VS Code with `agents/`, `specify/`, and `templates/` folders visible.

---

## Exercise 3: Download Workshop Support Files

### Goal
Add AI enhancements that make this workshop easier and more effective.

### Context

This workshop includes two powerful AI enhancements that will help you throughout:

1. **Recipe Domain Instructions** (`.github/instructions/recipe-domain.instructions.md`)
   - Teaches GitHub Copilot about recipe terminology, measurements, and validation
   - Improves AI code generation accuracy for the Recipe Manager
   - Provides context on dietary restrictions, conversions, and best practices

2. **SpecKitCoach skill** (`.github/skills/speckit-coach/SKILL.md`)
   - Your personal AI guide for Spec-Driven Development
   - Invoke with `@SpecKitCoach` anytime for help
   - Answers methodology questions, validates your understanding, provides encouragement

### Steps

**3.1** Create the `.github` directory structure **in your recipe-manager project**:

```powershell
# IMPORTANT: Run this from your recipe-manager project root (not the workshop folder)
cd recipe-manager  # Navigate to your project if not already there
mkdir .github\instructions
mkdir .github\skills\speckit-coach
```

> ⚠️ **Critical:** These files must be in your `recipe-manager` project directory, NOT the workshop repository. The instruction file only affects the project where it's located.

**3.2** Download the recipe domain instruction file:

**Option A: Using PowerShell (Windows)**
```powershell
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/CanarysPlayground/speckit-and-beyond/main/.github/instructions/recipe-domain.instructions.md" -OutFile ".github\instructions\recipe-domain.instructions.md"
```

**Option B: Using curl (macOS/Linux)**
```bash
curl -o .github/instructions/recipe-domain.instructions.md https://raw.githubusercontent.com/CanarysPlayground/speckit-and-beyond/main/.github/instructions/recipe-domain.instructions.md
```

**Option C: Manual Download**
1. Visit: https://github.com/CanarysPlayground/speckit-and-beyond/blob/main/.github/instructions/recipe-domain.instructions.md
2. Click "Raw" button
3. Save as `.github/instructions/recipe-domain.instructions.md`

**3.3** Download the SpecKitCoach skill:

**Option A: Using PowerShell (Windows)**
```powershell
Invoke-WebRequest -Uri "https://raw.githubusercontent.com/CanarysPlayground/speckit-and-beyond/main/.github/skills/speckit-coach/SKILL.md" -OutFile ".github\skills\speckit-coach\SKILL.md"
```

**Option B: Using curl (macOS/Linux)**
```bash
curl -o .github/skills/speckit-coach/SKILL.md https://raw.githubusercontent.com/CanarysPlayground/speckit-and-beyond/main/.github/skills/speckit-coach/SKILL.md
```

**Option C: Manual Download**
1. Visit: https://github.com/CanarysPlayground/speckit-and-beyond/blob/main/.github/skills/speckit-coach/SKILL.md
2. Click "Raw" button
3. Save as `.github/skills/speckit-coach/SKILL.md`

**3.4** Verify the files are in place **in your recipe-manager project**:

```powershell
# From recipe-manager project root
Test-Path .github\instructions\recipe-domain.instructions.md
Test-Path .github\skills\speckit-coach\SKILL.md
# Both should return: True
```

> 💡 **Tip:** Use `pwd` to confirm you're in the recipe-manager directory, not the workshop directory.

**3.5** Restart VS Code to load the new files:

```powershell
# Close VS Code completely, then reopen your recipe-manager project
code .
```

**3.6** Test that the instruction file is working:

Open Copilot Chat and ask:
```
What measurement conversions should I use for recipes? Show the conversion for 1 cup to ml.
```

**Expected response:** Copilot should mention "1 cup = 240 ml" (from recipe-domain.instructions.md)

**If you get a generic answer:**
- Verify the file is in `recipe-manager/.github/instructions/` (not in workshop folder)
- Restart VS Code again
- Ensure you're asking from within the recipe-manager project

### Why These Files Matter

**Recipe Domain Instructions:**
- ✅ AI generates better variable names (`prep_time_minutes` vs `pt`)
- ✅ Automatic measurement conversions (cups to ml)
- ✅ Proper validation logic for dietary restrictions
- ✅ Consistent terminology across all generated code

**SpecKitCoach skill:**
- ✅ Get help without leaving VS Code
- ✅ Spec-Driven Development best practices on demand
- ✅ Validates your checkpoints ("Am I doing this right?")
- ✅ More helpful than generic Copilot for methodology questions

### How to Use

**Recipe Domain Instructions (Automatic):**
- Works automatically once installed in your project's `.github/instructions/` folder
- No invocation needed - Copilot always uses this context when working in that project
- Must restart VS Code after adding the file
- Only affects the project where it's located

**SpecKitCoach Skill (Explicit):**
- Type `@SpecKitCoach` followed by your question in Copilot Chat
- Only active when explicitly invoked
- Provides targeted methodology coaching

**Examples:**
```
@SpecKitCoach What's the difference between /speckit.specify and /speckit.plan?
@SpecKitCoach Is my constitution detailed enough?
@SpecKitCoach How do I handle edge cases in specifications?
```

**Key Difference:**

| Feature | Instruction File | SpecKitCoach Skill |
|---------|-----------------|--------------------|
| **Activation** | Automatic | Explicit (`@SpecKitCoach`) |
| **When Active** | Always (in that project) | Only when invoked |
| **Purpose** | Domain knowledge | Methodology coaching |
| **Scope** | All Copilot interactions in project | Specific Q&A sessions |
| **Location** | Project's `.github/instructions/` | Project's `.github/skills/` |

### What You Learned
- Workshop support files enhance AI assistance in different ways
- **Instruction files** work automatically - teach Copilot domain-specific knowledge
- **Skills** require explicit invocation - provide interactive coaching
- Restart VS Code to load these files

### ✅ Checkpoint
Both files exist in your **recipe-manager project** (`.github/instructions/recipe-domain.instructions.md` and `.github/skills/speckit-coach/SKILL.md`), and Copilot correctly answers the measurement conversion test question.

---

## Exercise 4: Explore the Project Structure

### Goal
Understand what Spec Kit creates and why each component matters.

### Steps

**3.1** In VS Code Explorer, examine the project structure:

```
recipe-manager/
├── agents/
│   ├── speckit.analyze.agent.md
│   ├── speckit.checklist.agent.md
│   ├── speckit.clarify.agent.md
│   ├── speckit.constitution.agent.md
│   ├── speckit.implement.agent.md
│   ├── speckit.plan.agent.md
│   ├── speckit.specify.agent.md
│   ├── speckit.tasks.agent.md
│   └── speckit.taskstostories.agent.md
├── prompts/
├── specify/
│   ├── memory/
│   │   └── constitution.md       ← Project principles (empty for now)
│   └── scripts/
│       └── powershell/           ← PowerShell automation scripts
│           ├── check-prerequisites.ps1
│           ├── common.ps1
│           ├── create-new-feature.ps1
│           ├── setup-plan.ps1
│           └── update-agent-context.ps1
├── templates/
│   ├── agent-file-template.md
│   ├── checklist-template.md
│   ├── constitution-template.md
│   ├── plan-template.md
│   ├── spec-template.md
│   └── tasks-template.md
├── .github/
├── .vscode/
│   └── settings.json             ← VS Code configuration for agents
├── .gitignore
└── README.md
```

**3.2** Open `.vscode/settings.json` and examine the agent configuration:

This file tells GitHub Copilot:
- Agent prompt file recommendations (the `/speckit.*` commands)
- How to recognize and execute each agent
- Configuration for chat participant features

**3.3** Explore the `agents/` directory:

You'll see multiple `.agent.md` files:
- `speckit.constitution.agent.md` — Creates project principles
- `speckit.specify.agent.md` — Defines feature specifications  
- `speckit.plan.agent.md` — Generates technical plans
- `speckit.tasks.agent.md` — Breaks down implementation tasks
- And more...

These agent files define what each `/speckit.*` command does.

**3.4** Check the `templates/` directory:

You'll find templates that agents use:
- `constitution-template.md` — Structure for project principles
- `spec-template.md` — Feature specification format
- `plan-template.md` — Implementation plan structure
- `tasks-template.md` — Task breakdown format

**3.5** View the empty `constitution.md`:

```powershell
cat specify/memory/constitution.md
```

It's empty because we haven't defined our principles yet — that's next!

### What You Learned
- `agents/` contains the agent definitions for each `/speckit.*` command
- `specify/memory/` stores project constitution and feature specifications
- `templates/` provides structure for all Spec Kit artifacts
- `.vscode/settings.json` configures agent integration with GitHub Copilot
- All files are Markdown-based (human + AI readable)

### ✅ Checkpoint
You understand the purpose of each directory in the Spec Kit structure.

---

## Exercise 5: Verify Spec Kit Integration

### Goal
Confirm that GitHub Copilot recognizes Spec Kit slash commands.

### Steps

**4.1** Open GitHub Copilot Chat in VS Code:
- Press `Ctrl+Alt+I` (Windows) or `Cmd+Opt+I` (macOS)
- Or click the Copilot Chat icon in the sidebar

**4.2** Type `/` (forward slash) in the chat input

You should see Spec Kit commands appear in the autocomplete dropdown:
- `/speckit.constitution`
- `/speckit.specify`
- `/speckit.plan`
- `/speckit.tasks`
- `/speckit.implement`
- `/speckit.clarify`
- `/speckit.analyze`
- `/speckit.checklist`

**4.3** If commands don't appear, restart VS Code:
- Close VS Code completely
- Reopen the `recipe-manager` project
- Try step 4.2 again

**4.4** Test the SpecKitCoach skill:

> **Note:** The invocation method for custom skills may vary by VS Code/Copilot version. Try these methods:

**Method 1: Direct invocation (most common)**
```
@SpecKitCoach What is Spec-Driven Development?
```

**Method 2: If @ doesn't work, reference the file:**
```
Based on .github/skills/speckit-coach/SKILL.md, what is Spec-Driven Development?
```

SpecKitCoach should provide a detailed, methodology-focused explanation. If neither works, the skill is still useful as reference documentation.

**4.5** Ask Copilot about Spec Kit:

```
What is Spec-Driven Development and how do the /speckit commands work?
```

Copilot should explain the workflow based on the instructions file.

### Troubleshooting

**Problem:** Slash commands don't appear  
**Fix:**
1. Ensure `.vscode/settings.json` exists with agent configurations
2. Check that `agents/*.agent.md` files are present
3. Restart VS Code
4. Check GitHub Copilot extension is active (bottom status bar)

**Problem:** Copilot gives generic answers, not Spec Kit specific  
**Fix:**
1. The agent files might not be loaded
2. Mention the agent explicitly: *"Based on agents/speckit.constitution.agent.md, explain the constitution command"*
3. Check that `.vscode/settings.json` has correct agent paths

### What You Learned
- Spec Kit extends AI agents through instruction files and scripts
- Slash commands are custom integrations recognized by Copilot
- Instructions are project-specific, not global

### ✅ Checkpoint
You can see `/speckit.*` commands in the Copilot Chat autocomplete.

---

## Exercise 6: Create Your Constitution

### Goal
Generate project-governing principles that will guide all development decisions.

### Context

A **constitution** in Spec-Driven Development is similar to a project charter or guiding document. It defines:
- **Core values** — What matters most (performance, security, UX, etc.)
- **Development standards** — Code quality, testing, documentation requirements
- **Constraints** — Technical limitations, compliance requirements
- **User focus** — Who you're building for and why

The constitution is **referenced by AI** when generating specifications, plans, and code. Think of it as the "personality" of your project.

### Steps

**5.1** In Copilot Chat, invoke the constitution command:

```
/speckit.constitution
```

**5.2** When prompted, provide guidance for the Recipe Manager principles:

```
Create a constitution for a Recipe Manager application with these priorities:

1. User Experience: Simple, intuitive interface accessible to non-technical users
2. Data Privacy: User recipes are stored locally, no cloud dependencies required
3. Search Performance: Fast recipe search across 1000+ recipes (< 100ms)
4. Code Quality: Well-tested Python code with 80%+ coverage, type hints throughout
5. Cross-Platform: Works on Windows, macOS, Linux
6. Extensibility: Plugin architecture for future features (meal planning, nutrition tracking)
7. Accessibility: WCAG 2.1 AA compliance for web interface
8. Offline-First: Full functionality without internet connection

Target users: Home cooks, food bloggers, families organizing meal planning
```

**5.3** Copilot will generate a constitution and write it to `.speckit/constitution.md`

**5.4** Review the generated constitution:

Open `.speckit/constitution.md` and read through:
- Core Principles section
- Development Guidelines
- Technical Constraints
- Quality Standards
- User Experience Goals

**5.5** Refine if needed:

If something doesn't align with your vision, ask Copilot:

```
Update the constitution to emphasize [specific aspect]
```

Or directly edit `.speckit/constitution.md` in VS Code.

### Example Constitution (Partial)

```markdown
# Recipe Manager — Project Constitution

## Core Principles

### 1. User-Centric Simplicity
Every feature must be immediately understandable to a non-technical home cook. 
Avoid jargon, minimize clicks, provide helpful defaults.

### 2. Privacy by Design
User data stays on their device. No telemetry, no cloud sync unless explicitly opted in.
Recipes are personal — respect that privacy.

### 3. Performance Over Features
A fast, reliable app with 10 features beats a slow app with 100 features.
Search must feel instant (< 100ms). Startup time < 2 seconds.

[... more sections ...]
```

### What You Learned
- Constitution is the foundation of Spec-Driven Development
- It's a living document (can be updated as project evolves)
- AI uses it to make aligned decisions during generation
- Clear priorities prevent scope creep and misalignment

### ✅ Checkpoint
You have a comprehensive `constitution.md` that reflects Recipe Manager's priorities.

---

## Exercise 7: Understanding Principles

### Goal
Understand how principles influence specifications and generated code.

### Steps

**6.1** Ask Copilot to explain how principles will be used:

```
How will the principles in constitution.md affect the code and specifications you generate for Recipe Manager?
```

**6.2** Test principle influence with a hypothetical scenario:

```
If I ask you to add a feature that uploads recipes to a cloud service for social sharing, how would you respond based on our constitution?
```

Expected response: Copilot should note the **conflict** with "Data Privacy: User recipes are stored locally" and suggest alternatives (local sharing via export, optional cloud sync with explicit consent).

**6.3** Ask Copilot to identify missing principles:

```
Review constitution.md and suggest any important principles we might have missed for a Recipe Manager application.
```

Copilot might suggest:
- Internationalization (for measurements, cuisines)
- Data backup and export strategies
- Image handling and storage policies
- Collaboration workflows (family recipe books)

**6.4** Update the constitution if good suggestions emerge:

```
Add a principle about internationalization: support for imperial and metric measurements, multiple languages for cuisine types.
```

### Why This Matters

Without a constitution:
- AI makes arbitrary decisions (cloud vs local? SQL vs NoSQL? React vs Vue?)
- Team alignment suffers (different developers, different approaches)
- Refactoring is frequent (changing core assumptions late is expensive)

With a constitution:
- **Consistency** — All decisions trace back to stated principles
- **Efficiency** — AI doesn't need to ask about priorities every time
- **Quality** — Constraints prevent low-quality shortcuts
- **Team alignment** — New developers understand project values immediately

### What You Learned
- Constitution is not just documentation — it's an **AI constraint system**
- Good principles prevent misalignment between intent and implementation
- You can (and should) iterate on the constitution as you learn

### ✅ Checkpoint
You understand how the constitution guides AI decision-making throughout the workflow.

---

## Exercise 8: Testing the Workflow

### Goal
Do a dry run of the Spec-Driven workflow without actually building anything yet.

### Steps

**7.1** Ask Copilot to walk through the workflow:

```
Without executing anything yet, explain the Spec-Driven Development workflow for Recipe Manager. What would we do after creating the constitution?
```

Expected response:
1. ✅ Constitution (done)
2. Next: `/speckit.specify` to define the first feature
3. Optional: `/speckit.clarify` to explore edge cases
4. Then: `/speckit.plan` to choose tech stack and architecture
5. Then: `/speckit.tasks` to break plan into actionable items
6. Finally: `/speckit.implement` to generate code

**7.2** Ask about branching and features:

```
How does Spec Kit organize multiple features? Can we build feature-by-feature?
```

Expected response: Each feature gets its own directory in `.speckit/features/`. Git branches correlate with features (e.g., `001-core-recipe-storage`).

**7.3** Preview what a feature spec might look like:

```
Show me an example of what a specification for "Core Recipe Storage" might include, without creating the file yet.
```

This gives you a mental model before Experiment 2.

**7.4** Understand the difference between commands:

| Command | Purpose | When to Use |
|---------|---------|-------------|
| `/speckit.constitution` | Define project principles | Once at project start, occasionally update |
| `/speckit.specify` | Describe WHAT to build | Start of each new feature |
| `/speckit.clarify` | Ask questions about spec | After specify, before planning |
| `/speckit.plan` | Define HOW to build (tech stack) | After clear specifications |
| `/speckit.tasks` | Break plan into TODO list | After plan is solid |
| `/speckit.implement` | Generate code from tasks | Final step of feature |

### What You Learned
- Spec-Driven Development is a multi-stage pipeline
- Each stage has a specific purpose and output
- You can (should) iterate within stages before proceeding
- Features are independent units of work

### ✅ Checkpoint
You can explain the Spec Kit workflow from constitution to implementation.

---

## Exercise 9: Environment Validation

### Goal
Final checks before proceeding to Experiment 2.

### Steps

**8.1** Run environment validation:

```powershell
specify check
```

Ensure all checks pass:
- ✅ Git installed
- ✅ Python 3.11+
- ✅ AI agent available (copilot)

**8.2** Verify Git repository:

```powershell
git status
```

You should see:
- A clean working tree
- On branch `main` (or `master`)
- Initial commit already created by `specify init`

**8.3** Check file structure:

```powershell
ls -Recurse .speckit | Select-Object FullName
```

Confirm:
- `.speckit/constitution.md` exists and has content
- `.speckit/scripts/` contains `.ps1` and `.sh` files
- `.speckit/features/` exists (empty for now)

**8.4** Test GitHub Copilot responsiveness:

In Copilot Chat:
```
Summarize the constitution.md in 3 bullet points
```

Copilot should quickly respond with key principles from your constitution.

**8.5** Commit your constitution:

```powershell
git add .
git commit -m "feat: add project constitution and Spec Kit setup"
```

This creates a save point before starting feature development.

### What You Learned
- Environment validation prevents issues mid-workflow
- Git commits create checkpoints for experimentation
- Constitution is version-controlled like code

### ✅ Checkpoint
All environment checks pass, constitution is committed, ready for Experiment 2.

---

## Experiment 1 Wrap-Up

### What You Accomplished

✅ Installed Spec Kit CLI and dependencies  
✅ Initialized a Spec-Driven Development project structure  
✅ Created a comprehensive project constitution  
✅ Understood how principles guide AI decision-making  
✅ Learned the Spec Kit workflow pipeline  
✅ Verified your development environment  

### Key Takeaways

1. **Constitution is foundational** — It shapes every subsequent decision
2. **Spec Kit extends AI** — Slash commands are project-specific workflows
3. **Iterative by design** — You can update constitution, specs, plans anytime
4. **Tool-agnostic** — Works with Copilot, Claude, Cursor, and more
5. **Git-integrated** — Version control is built into the workflow

### Time Investment vs. Benefit

**20 minutes spent on Experiment 1 prevents:**
- Hours of misaligned implementation
- Multiple refactoring cycles
- Team debates about priorities
- Scope creep and feature bloat

### Common Questions

**Q: Can I change the constitution after starting development?**  
A: Yes! Constitution is a living document. Update it and regenerate affected specs.

**Q: Do I need a constitution for a 2-hour prototype?**  
A: Even simple projects benefit. A 5-minute constitution saves 30 minutes of rework.

**Q: What if I disagree with the AI-generated constitution?**  
A: Edit `constitution.md` directly or ask Copilot to revise specific sections.

**Q: Can multiple people collaborate on one constitution?**  
A: Yes! Use Git to merge constitution changes like any code file.

---

## Next Steps

🚀 **Proceed to [Experiment 2: Specify & Clarify](../experiment-2/README.md)**

Learn how to transform user stories into rich, AI-ready specifications and explore underspecified areas with clarifying questions.

---

## Quick Reference

See [CHEATSHEET.md](CHEATSHEET.md) for a compact reference of all Experiment 1 concepts, commands, and best practices.



