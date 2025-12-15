# How to Use This AI-Assisted Development Template

## 🎯 Quick Start (5 minutes)

### 1. Clone & Initialize

```bash
# Clone this template
git clone <this-repo-url> my-new-project
cd my-new-project

# Remove git history and start fresh
rm -rf .git
git init
git add .
git commit -m "Initial commit from AI-assisted template"
```

### 2. Customize Memory Bank (Required!)

Open each file in `memory-bank/` and replace placeholders with your project details:

#### `projectBrief.md` ⭐ CRITICAL
- Define your core mission
- List non-negotiable requirements
- Specify success criteria

#### `productContext.md` ⭐ CRITICAL  
- Describe user problems you're solving
- Define your business model
- Identify target users

#### `activeContext.md` ⭐ UPDATE FREQUENTLY
- Set initial implementation state
- Track current progress
- **Update at end of every session!**

#### `systemPatterns.md`
- Define your architectural patterns
- Document coding standards
- Add anti-patterns (what NOT to do)

#### `techContext.md`
- Specify your tech stack and versions
- Document environment variables
- List deployment configuration

#### `decisionLog.md` (Fills over time)
- Add ADRs as you make architectural decisions
- Use provided template

#### `implementationLog.md` (Fills over time)
- Track feature implementation history
- Note what worked/failed

### 3. Customize Copilot Instructions

Edit `.github/instructions/copilot-instructions.md`:

```markdown
## Critical Technical Components
backend: Node.js + Express  # <-- YOUR STACK
database: PostgreSQL        # <-- YOUR DATABASE
frontend: React + Vite      # <-- YOUR FRONTEND
# ... etc
```

Replace ALL placeholder sections marked with:
> **IMPORTANT**: Update this section...

### 4. Start AI-Assisted Development!

Open VS Code with GitHub Copilot and say:

```
"Read memory-bank/activeContext.md and help me set up [your project]. 
Let's start by reviewing the project brief and creating an implementation plan."
```

## 🧠 Memory Bank Workflow

### Every Session START:
```
AI reads: memory-bank/activeContext.md
You say: "What's the current state? What should we work on next?"
```

### During Development:
```
AI cross-references: memory-bank/systemPatterns.md
Before implementing: AI proposes plan → you approve
```

### Every Session END:
```
AI updates: memory-bank/activeContext.md
- What was completed
- What's next
- Any blockers
```

### When Milestone Reached:
```
AI updates: memory-bank/implementationLog.md
- Document what was built
- Note what worked/failed
```

## 📋 Example Workflows

### Adding a New Feature

1. **Start Session**:
   ```
   "Read activeContext.md. I want to add user authentication. 
   Based on systemPatterns.md, what's the best approach?"
   ```

2. **AI Proposes Plan** (waits for approval)

3. **You Approve**: "Looks good, proceed"

4. **AI Implements** (according to plan)

5. **Update Context**:
   ```
   "Update activeContext.md with what we just completed"
   ```

### Making an Architectural Decision

1. **Discuss Options**:
   ```
   "Should we use PostgreSQL or MongoDB for this project? 
   Consider the requirements in projectBrief.md"
   ```

2. **AI Presents Trade-offs**

3. **You Decide**: "Let's go with PostgreSQL"

4. **Document Decision**:
   ```
   "Add an ADR to decisionLog.md documenting why we chose PostgreSQL"
   ```

5. **Update Patterns**:
   ```
   "Update systemPatterns.md with our database patterns"
   ```

### Context Gets Bloated

If chat history is getting too long:

1. **Save State**:
   ```
   "Update activeContext.md with our current state and next steps"
   ```

2. **Start Fresh Chat**

3. **Restore Context**:
   ```
   "Read memory-bank files and summarize where we are"
   ```

## 🛠️ Optional: Mobile Build Scripts

If you're building iOS/Android apps:

```bash
# Increment iOS build number (required for App Store)
npm run increment:ios

# Increment Android versionCode (required for Play Store)
npm run increment:android
```

These scripts automatically update build numbers in your Xcode/Android Studio projects.

## ✅ Checklist: Is Your Template Ready?

- [ ] Cloned template and removed git history
- [ ] Updated `projectBrief.md` with your mission
- [ ] Updated `productContext.md` with user problems
- [ ] Set initial state in `activeContext.md`
- [ ] Defined patterns in `systemPatterns.md`
- [ ] Specified tech stack in `techContext.md`
- [ ] Customized `copilot-instructions.md` placeholders
- [ ] Tested AI assistant reads Memory Bank correctly

## 🎓 Tips for Success

### 1. Be Specific in Memory Bank
❌ Bad: "Build a web app"  
✅ Good: "Build a B2B SaaS project management tool for design agencies with real-time collaboration"

### 2. Update activeContext.md Religiously
This is your source of truth. Update it EVERY session.

### 3. Use ADRs for Big Decisions
Document the "why" not just the "what". Future you will thank you.

### 4. Start New Chats When Needed
Don't let context get bloated. Memory Bank maintains continuity.

### 5. Challenge AI Complexity
If AI suggests over-engineering, say: "That seems too complex. What's a simpler approach?"

## 🤝 Working with a Team

### Solo Developer
- Keep Memory Bank personal and informal
- Update as you think through problems

### Small Team (2-5)
- Treat Memory Bank as shared team context
- Require everyone to read activeContext.md daily
- Review decisionLog.md together

### Larger Team
- Assign Memory Bank maintenance to tech lead
- Create team-specific sections in systemPatterns.md
- Use implementationLog.md for sprint retrospectives

## 📚 Additional Resources

- [Memory Bank Pattern Explanation](./README.md#core-concept-the-memory-bank)
- [Plan & Act Workflow](./README.md#the-plan--act-workflow)
- [Best Practices](./README.md#best-practices)

---

**Ready to build?** Update those Memory Bank files and start your first AI-assisted session! 🚀
