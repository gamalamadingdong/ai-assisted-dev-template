# AI-Assisted Development Template

> **Context-driven development with persistent memory.** A structured instruction system that helps AI assistants (like GitHub Copilot) maintain context across sessions and guide high-quality implementation.

## 🎯 What is this?

An **AI-assisted development framework** that provides structure, patterns, and persistent context to maximize the effectiveness of AI coding assistants.

**What you get:**
- 🧠 **Memory Bank Pattern** - File-based persistent context across AI sessions
- 📋 **Structured Instructions** - Clear guidance for AI assistants on architecture and patterns
- 🎯 **"Plan & Act" Workflow** - Prevent AI drift by planning before implementation
- 📚 **Decision Tracking** - ADRs (Architectural Decision Records) document the "why"
- 🔄 **Context Hygiene** - Maintain long-term project memory without chat bloat
- 🛠️ **Generic Utilities** - Mobile build scripts and development tools

**What you build:**
- Your project's specific context and requirements
- Your architectural patterns and coding standards
- Your business logic and domain models
- Your AI assistant's "personality" and approach

**Philosophy:** AI assistants are stateless and forget context between sessions. This template solves that problem with structured prompt engineering and file-based memory.

## 🧠 Core Concept: The Memory Bank

AI assistants (like GitHub Copilot) are **stateless** - they reset their memory with every new chat session. For long-term projects, this creates a critical problem:

❌ **Without Memory Bank:**
- AI asks the same questions repeatedly
- Forgets architectural decisions
- Loses context on what's been implemented
- Drifts from original vision over time

✅ **With Memory Bank:**
- AI reads project context at session start
- Maintains consistent architectural patterns
- Tracks decisions and implementations
- Stays aligned with project goals

### How It Works

The Memory Bank is a set of **markdown files** that serve as the AI's "external hard drive":

```
memory-bank/
├── projectBrief.md          # Core mission, non-negotiables
├── productContext.md        # User problems, business model
├── activeContext.md         # Current state (updated frequently)
├── systemPatterns.md        # Architecture patterns, code standards
├── techContext.md           # Stack versions, configuration
├── decisionLog.md           # ADRs - "why" we made choices
├── implementationLog.md     # What's built, what worked/failed
│
└── Optional (add as needed):
    ├── businessAnalysis.md  # Market, competition, revenue (for commercial projects)
    └── experimentLog.md     # ML experiments, model training (for data science projects)
```

**Workflow:**
1. **Session Start**: AI reads `activeContext.md` to understand current state
2. **Before Changes**: AI cross-references `systemPatterns.md` for consistency
3. **After Work**: AI updates `activeContext.md` with new state
4. **Milestone Reached**: AI updates `implementationLog.md` with progress

## 📋 Structured Instructions

Located in `.github/instructions/copilot-instructions.md`, this file provides:

- **Role Definition**: What the AI should act as (expert engineer, etc.)
- **Methodology**: "Plan & Act" workflow to prevent drift
- **Code Standards**: Data-first design, YAGNI, type safety
- **Anti-Patterns**: What to never do
- **Project-Specific Context**: Your unique requirements

### The "Plan & Act" Workflow

Prevents "agentic drift" where AI creates working code that diverges from intent:

1. **Analyze Request**: Understand business context and requirements
2. **Read Memory Bank**: Check current state in `activeContext.md`
3. **Formulate Plan**: Output step-by-step implementation plan
4. **Get Confirmation**: Wait for approval before coding
5. **Execute**: Implement according to approved plan
6. **Document**: Update Memory Bank files

## 🚀 Quick Start

### 1. Clone This Template

```bash
# Clone to your new project
git clone <this-repo-url> my-project
cd my-project

# Remove git history (start fresh)
rm -rf .git
git init
git add .
git commit -m "Initial commit from AI-assisted template"
```

### 2. Customize Memory Bank

Update the template files in `memory-bank/` with your project's context:

**Required Updates:**
- `projectBrief.md` - Define your mission and non-negotiables
- `productContext.md` - Describe user problems and business model
- `activeContext.md` - Set initial implementation state
- `systemPatterns.md` - Define your architecture patterns
- `techContext.md` - Specify your tech stack and versions

**Optional (fill as you go):**
- `decisionLog.md` - Will populate as you make architectural decisions
- `implementationLog.md` - Will populate as you build features

### 3. Configure AI Assistant Instructions

Edit `.github/instructions/copilot-instructions.md`:

```markdown
## Critical Technical Components
backend: [your backend choice]
frontend: [your frontend choice]
mobile: [your mobile choice, if applicable]
[... customize for your stack ...]

## Project Overview
[Replace with your specific project description]
```

### 4. Start Development with AI

Open your project in VS Code with GitHub Copilot and say:

> "Read the Memory Bank files and help me set up [your project type]. Let's start by reviewing the project brief and creating an implementation plan."

The AI will:
- ✅ Read `memory-bank/activeContext.md` to understand current state
- ✅ Review your project requirements
- ✅ Propose an implementation plan
- ✅ Wait for your approval before coding
- ✅ Update Memory Bank files as work progresses

## 📖 Using This Template

### For Any Project Type

This template is **framework-agnostic**. Examples of what you can build:

- **Web Applications**: React, Vue, Angular, Svelte, etc.
- **Mobile Apps**: React Native, Flutter, Capacitor, native iOS/Android
- **Backend Services**: Node.js, Python, Go, Rust, etc.
- **Full-Stack SaaS**: Any combination of frontend + backend
- **CLI Tools**: Command-line applications
- **APIs**: REST, GraphQL, gRPC
- **Desktop Apps**: Electron, Tauri, etc.
- **ML/Data Science**: Model training, data pipelines, research projects
- **Commercial Products**: Includes businessAnalysis.md for market/revenue planning

### Example: Starting a SaaS Project

1. **Update `projectBrief.md`:**
   ```markdown
   ## Core Mission
   Build a B2B project management tool for design agencies...
   
   ## Non-Negotiable Requirements
   - Real-time collaboration
   - Mobile-responsive
   - SOC 2 compliant
   ```

2. **Update `techContext.md`:**
   ```markdown
   ## Core Technology Stack
   - Frontend: React 18 + TypeScript + Vite
   - Backend: Supabase (PostgreSQL + Auth)
   - Hosting: Vercel
   ```

3. **Start AI Session:**
   > "I need to set up the database schema for a project management tool. Based on the Memory Bank, help me design tables for projects, tasks, and team members with proper relationships."

### Example: Starting a Mobile App

1. **Update `projectBrief.md`:**
   ```markdown
   ## Core Mission
   Build a fitness tracking app for personal trainers...
   
   ## Non-Negotiable Requirements
   - iOS and Android support
   - Offline-capable
   - Sync across devices
   ```

2. **Update `techContext.md`:**
   ```markdown
   ## Core Technology Stack
   - Mobile: React Native + TypeScript
   - Backend: Firebase
   - Platform: iOS 15+, Android 10+
   ```

3. **Start AI Session:**
   > "Based on the Memory Bank, help me set up the React Native project structure with offline-first architecture for a fitness tracking app."

## 🛠️ What's Included

### Core Template Files
```
.github/
  instructions/
    copilot-instructions.md     # AI assistant configuration (customize for your project)

memory-bank/
  projectBrief.md               # Template - define your mission
  productContext.md             # Template - define user problems
  activeContext.md              # Template - track current state
  systemPatterns.md             # Template - define patterns
  techContext.md                # Template - define tech stack
  decisionLog.md                # Template - track decisions (ADRs)
  implementationLog.md          # Template - track progress
  
  Optional (add based on project type):
  businessAnalysis.md           # Template - market, competition, revenue
  experimentLog.md              # Template - ML experiments, training runs

scripts/
  increment-ios-build.js        # Utility: Auto-increment iOS build number
  increment-android-build.js    # Utility: Auto-increment Android versionCode
  README.md                     # Utility documentation
```

### Generic Utilities

**Mobile Build Automation** (if building iOS/Android apps):

```bash
# Increment iOS build number (required for App Store submissions)
node scripts/increment-ios-build.js

# Increment Android versionCode (required for Play Store submissions)
node scripts/increment-android-build.js
```

These scripts automatically update build numbers in your Xcode and Android Studio projects.

## 🎯 Design Philosophy

### Why This Approach Works

**Traditional Development:**
```
Developer → Code → Test → Deploy
(All context in developer's head)
```

**AI-Assisted Development Without Memory:**
```
Developer → Prompt AI → Code → Forget Context → Repeat
(Context lost between sessions)
```

**AI-Assisted Development With Memory Bank:**
```
Developer → Update Memory Bank → Prompt AI → AI Reads Context →
Code with Consistency → Update Memory Bank → Maintain Context
(Persistent context across sessions)
```

### Key Principles

1. **Context is King**: AI needs explicit context to make good decisions
2. **File-Based Memory**: Use files, not chat history, for persistence
3. **Plan Before Act**: Always plan and get approval before implementation
4. **Data-First Design**: Define data structures before writing logic
5. **Document Decisions**: Track the "why" not just the "what"
6. **YAGNI**: Reject complexity, build only what's needed now
7. **Simplicity**: Prefer simple, direct solutions over clever abstraction

## 📚 Best Practices

### Starting a New Feature

```markdown
1. Open new Copilot chat
2. Say: "Read memory-bank/activeContext.md and help me implement [feature]"
3. AI reads context and proposes plan
4. Review and approve plan
5. AI implements according to plan
6. AI updates activeContext.md with new state
```

### Making Architectural Decisions

```markdown
1. Discuss options with AI
2. Document decision in memory-bank/decisionLog.md as ADR
3. Include: Context, Decision, Rationale, Consequences, Alternatives
4. Update systemPatterns.md with new pattern (if applicable)
```

### Context Getting Bloated

```markdown
If chat context becomes too large:
1. Save important decisions to decisionLog.md
2. Update activeContext.md with current state
3. Start fresh chat
4. Say: "Read memory-bank files and continue from where we left off"
```

### Session-to-Session Workflow

```markdown
**End of Session:**
1. Update activeContext.md with:
   - What was completed
   - What's next
   - Any blockers
2. If milestone reached, update implementationLog.md

**Start of Session:**
1. Say: "Read memory-bank/activeContext.md and summarize current state"
2. AI reads and provides context
3. Continue working with full context restored
```

## 🔧 Customization Guide

### For Your Specific Project

1. **Fill Out Memory Bank Templates**: Replace placeholders with your actual project details
2. **Customize copilot-instructions.md**: Add project-specific patterns and anti-patterns
3. **Add Project Files**: Create your actual codebase structure
4. **Update This README**: Replace generic descriptions with your project specifics

### Adapting for Different Team Sizes

**Solo Developer:**
- Keep all Memory Bank files simple and personal
- Focus on tracking your own thought process

**Small Team (2-5):**
- Use Memory Bank as shared team context
- Require all team members to update activeContext.md
- Review decisionLog.md together during standups

**Larger Team:**
- Assign Memory Bank ownership/maintenance
- Create team-specific systemPatterns.md sections
- Use implementationLog.md for sprint retrospectives

## 🎓 Learning More

### Understanding ADRs (Architectural Decision Records)

See `memory-bank/decisionLog.md` for template and examples. Key elements:

- **Context**: What situation led to this decision?
- **Decision**: What did we decide?
- **Rationale**: Why this choice? (most important!)
- **Consequences**: What are the trade-offs?
- **Alternatives**: What else did we consider?

### Memory Bank File Purposes

| File | Purpose | Update Frequency |
|------|---------|------------------|
| `projectBrief.md` | Core mission, non-negotiables | Rarely (only major pivots) |
| `productContext.md` | User problems, business model | Quarterly or when strategy changes |
| `activeContext.md` | Current work, next steps | **Every session** |
| `systemPatterns.md` | Architecture patterns | When new patterns emerge |
| `techContext.md` | Stack, versions, config | When tech changes |
| `decisionLog.md` | ADRs | When architectural decisions made |
| `implementationLog.md` | Feature history | When milestones reached |
| `businessAnalysis.md` (optional) | Market, competition, revenue | Quarterly or when market changes |
| `experimentLog.md` (optional) | ML experiments, training runs | After each experiment/training run |

## 🤝 Contributing

This is a template meant to be forked and customized. If you develop useful patterns:

1. Document them in your own `systemPatterns.md`
2. Share learnings about what works/doesn't work
3. Consider contributing generic utilities back to this repo

## 📝 License

MIT License - Use freely for any project

## 🙏 Credits

Memory Bank pattern inspired by ["Persistent Context Architecture" for AI assistants](https://example.com).

Built to solve the stateless nature of LLMs in long-term software projects.

---

**Ready to start?**

1. Clone this template
2. Customize `memory-bank/` files for your project
3. Update `.github/instructions/copilot-instructions.md`
4. Start your first AI-assisted session!

**Last Updated:** December 15, 2025
