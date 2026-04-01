# VibeCoding101
A complete, comprehensive guide to Vibe Coding and Claude Code

by Ariel Aliski
2026
Version 1.0.0

# Vibe Coding with Claude Code: A Practical Field Guide

*From zero to productive, as fast as possible.*

---

Welcome. This document is not a summary. It's not a cheat sheet. It's a full learning resource — the kind you study, dog-ear, and return to when things get weird. If you're a developer who's heard the phrase "Vibe Coding" and wants to actually understand what it means and how to do it well with Claude Code, this is the document you've been waiting for.

Here's what makes this guide different: it explains the *why* behind every feature, not just the *how*. Because once you understand why a CLAUDE.md file exists, or why subagents matter, you can improvise. You can debug your own prompts. You can design your own workflows. That's the goal.

A few things before we begin:

- **Claude Code is not just a coding assistant.** It's a framework for orchestrating AI agents. Most people use 10% of it. This guide is the other 90%.
- **The features build on each other.** Read in order the first time, then treat it as a reference.
- **The ecosystem moves fast.** This guide reflects Claude Code as of early 2026, including Agent Skills, Agent Teams, and the renamed Claude Agent SDK. Official docs live at `docs.claude.com` — verify specifics there.

Let's go.

---

# CHAPTER 0 — THE VIBE CODING MINDSET

## What Is Vibe Coding?

Vibe Coding is a term popularized in 2025 to describe a new relationship between developers and code — one where you *direct* an AI agent rather than write code yourself line by line. The "vibe" part isn't about being casual or imprecise. It's about operating at a higher level of abstraction: describing *what* you want, guiding *how* you want it, and reviewing rather than writing.

Think of the difference between a director and a cinematographer. A director says: "I want this scene to feel cold. Isolated. She walks in and immediately feels watched." The cinematographer handles the lenses, the lighting, the angles. The director doesn't stop caring about the technical details — they understand them deeply enough to steer the conversation. But they're not holding the camera.

That's Vibe Coding. You hold the direction. Claude Code holds the camera.

## The Shift from Writing to Directing

Traditional coding: you open a file, you think through the logic, you type it out, you test it, you fix it.

Vibe Coding with Claude Code: you open a terminal, you describe what needs to happen, Claude reads your codebase, proposes an approach, writes the code, runs the tests, and brings you the results. You review, redirect, and approve.

This isn't magic. It requires you to become very good at a skill you probably haven't practiced deliberately: **communicating intent to a software agent**.

The mental model shift is this:

| Old Model | New Model |
|---|---|
| "How do I write this?" | "What should happen here?" |
| Debugging by reading stack traces | Debugging by describing behavior |
| Learning syntax | Learning to specify outcomes |
| Writing functions | Reviewing and guiding functions |

> **Aha moment:** The best Vibe Coders are not lazy developers — they're developers who've freed up cognitive bandwidth from *implementation* and redirected it toward *architecture, clarity, and judgment*.

## Why Prompting IS the New Programming Skill

In the old world, knowing the syntax of Python or the API of a library was what made you productive. In the Vibe Coding world, what makes you productive is your ability to communicate precisely with an AI agent.

This is a learnable skill. The key dimensions:

**Clarity:** Are you saying what you mean? "Make this faster" is not a prompt. "Profile the `process_orders` function and identify the top 3 performance bottlenecks, then implement the highest-impact fix" is a prompt.

**Context:** Does Claude have what it needs to do the job? A prompt without context forces Claude to guess. A prompt with the right CLAUDE.md, the right files in scope, and a clear description of constraints produces dramatically better results.

**Constraints:** What can't it do? "Refactor this module but don't change the public interface" is a constraint that prevents a whole class of unwanted changes.

**Feedback loops:** Are you reviewing and redirecting quickly? The best Vibe Coders treat Claude like a talented junior engineer — they don't disappear for 20 minutes and come back hoping everything is perfect. They check in, course-correct, and keep momentum.

## Common Beginner Mistakes

**Mistake 1: Treating Claude Code like a search engine.** Asking "how do I do X in Python" is a waste of Claude Code's capabilities. Use it for tasks, not questions.

**Mistake 2: Writing prompts that are too vague.** "Fix my authentication module" produces worse results than "The JWT token validation in `auth/middleware.ts` is not correctly handling expired tokens — fix the expiry check to return a 401 with a specific error message."

**Mistake 3: Not setting up CLAUDE.md.** This is the single biggest leverage point and most beginners skip it. Your CLAUDE.md is Claude's persistent memory about your project. Without it, you're starting from scratch every session.

**Mistake 4: Approving changes without reading them.** Claude Code shows you every file it wants to change before it changes it. Always read. Always. This is non-negotiable.

**Mistake 5: Not using subagents for heavy context tasks.** Asking Claude to do a 10-file refactor, research the best approach, and also fix a bug in the same conversation balloons context and degrades quality. Learn to delegate (Chapter 6).

**Mistake 6: Fighting the agent loop instead of guiding it.** If Claude is going the wrong direction, interrupt early (Ctrl+C) and redirect. Don't let it finish a long wrong path and then ask it to undo everything.

### Try It Now

Before moving on, write one "bad" prompt and one "good" prompt for the same task. For example:

- Bad: *"Improve the user profile page"*
- Good: *"The user profile page at `pages/profile.tsx` has a layout bug where the avatar overlaps the name on mobile viewports under 375px. Fix the CSS to stack them vertically on mobile using our existing Tailwind responsive classes."*

Practice this habit. Your output quality will immediately improve.

---

# CHAPTER 1 — CLAUDE CODE FOUNDATIONS

## What Claude Code Is (and Is Not)

**Claude Code** is an agentic coding tool that runs in your terminal (and IDE) and has direct access to your filesystem, your shell, and your tools. It's not a chatbot you copy-paste from. It actually *reads your files*, *runs your tests*, *commits to git*, and *executes bash commands* — all through natural language.

What it is NOT:
- A standalone IDE (it integrates with your existing environment)
- A magic code generator that works without guidance (garbage in, garbage out)
- A replacement for understanding your codebase (you still need judgment)
- A free tool (it requires a Pro, Max, Teams, or Enterprise plan, or an API key)

What it IS:
- An agentic loop that reads, thinks, acts, and reports back
- A configurable platform you can extend with skills, hooks, subagents, and MCP servers
- A framework for orchestrating multi-agent workflows
- One of the most powerful developer productivity tools currently available

## The Surfaces

Claude Code runs on multiple surfaces. The underlying engine is the same — what differs is the interface:

| Surface | Description | Best For |
|---|---|---|
| **Terminal CLI** | Primary interface, runs in any shell | Power users, automation, full feature access |
| **VS Code Extension** | Native panel inside VS Code/Cursor/Windsurf | Developers who live in VS Code |
| **JetBrains Plugin** | Native integration for IntelliJ, WebStorm, etc. | JetBrains users |
| **Desktop App** | GUI app (macOS/Windows), no terminal required | Non-terminal users, onboarding |
| **GitHub Integration** | Tag `@claude` in PRs/issues | Async code review, CI workflows |

The Terminal CLI has the most features and is what this guide focuses on. Everything works from there.

## How the Underlying Engine Works

Claude Code runs an **agentic loop**. Here's what happens when you give it a task:

1. **You provide a prompt** (a task, a question, a direction)
2. **Claude reads context** — your CLAUDE.md, relevant files, conversation history
3. **Claude thinks** — reasons through an approach, sometimes using subagents for specific research
4. **Claude proposes actions** — file reads, file writes, bash commands
5. **You approve** (or Claude proceeds in automatic mode)
6. **Claude executes** — actually performs the action
7. **Claude reports** — shows you what happened and asks if you want to continue
8. **Repeat** until the task is complete

At every step where Claude wants to modify a file or run a shell command, it will ask for your permission (unless you've configured it otherwise). This is by design — it keeps you in control.

## Installation (Step-by-Step)

**Requirements:**
- macOS, Windows (with Git for Windows or WSL), or Linux
- An internet connection
- A Claude Pro, Max, Teams, Enterprise, or Console account

**Option 1: macOS with Homebrew (recommended)**
```bash
brew install claude-code
```

**Option 2: macOS/Linux native installer**
```bash
# Check the official docs at docs.claude.com for the current install command
# As of early 2026, native installation is the recommended path
```

**Option 3: Windows with WinGet**
```powershell
winget install Anthropic.ClaudeCode
```

**Option 4: npm (deprecated, but still works)**
```bash
npm install -g @anthropic-ai/claude-code
```

> **Warning:** npm installation is deprecated. Use native installers for auto-updates and better system integration.

**After installation:**
```bash
# Navigate to your project
cd your-project

# Start Claude Code — follow browser prompts to authenticate
claude

# Check that everything is working
claude doctor
```

## Your First Real Task: Exploring a Codebase and Making a Fix

Open a terminal in any existing project directory. Run:

```bash
claude
```

Once inside, try this progression:

```
> Give me a high-level overview of this codebase's architecture
```

Claude will read your files and give you a tour. Then:

```
> Where is the user authentication logic?
```

Then try a real task:

```
> Find any TODO comments in the codebase and show me the top 3 that look most important to address
```

Then attempt an actual fix:

```
> The login form doesn't show an error message when the email is invalid. Find where validation happens and add a user-friendly error message.
```

Watch Claude: it reads files, finds the relevant code, writes the fix, and shows you what it changed before committing anything.

### Try It Now

Open a real project (or a public GitHub repo you've cloned). Ask Claude to explain the architecture to you. Then ask it to find a bug or a TODO and fix it. Observe the full loop: read → propose → confirm → execute.

---

# CHAPTER 2 — CLAUDE.md: GIVING CLAUDE PERSISTENT MEMORY

## The Problem Without CLAUDE.md

Every time you start a new Claude Code session, Claude starts fresh. It doesn't remember that you use Tailwind 4, not Tailwind 3. It doesn't know that you never commit directly to `main`. It doesn't know that your team uses Zod for validation everywhere. So you either re-explain all of this in every session, or you accept inconsistent output.

**CLAUDE.md solves this.** It's a markdown file Claude reads automatically at the start of every session. It's your project's "constitution" — the rules, context, and conventions that Claude should always know.

## What to Put in CLAUDE.md

Think of CLAUDE.md as the onboarding document you'd write for a new senior engineer joining your team. It should include:

**Architecture overview:** What does this system do? How is it structured? What are the main components?

**Tech stack:** Libraries, frameworks, versions, and any non-obvious choices. Don't assume Claude knows you're using Prisma over TypeORM — tell it.

**Coding standards:** Naming conventions, file structure patterns, how you handle errors, async/await patterns vs callbacks.

**Important constraints:** "Never modify the public API surface without discussing first." "All database queries go through the service layer, not directly from controllers."

**Common commands:** How to run tests, start the dev server, run migrations. This lets Claude execute things correctly without guessing.

**Review checklist:** What you care about in code review — security, performance, test coverage, accessibility.

**What NOT to do:** Sometimes explicitly listing anti-patterns is more useful than describing the right approach.

## The Three CLAUDE.md Locations

Claude Code reads CLAUDE.md files from multiple locations, in priority order:

| Location | Scope | Use For |
|---|---|---|
| `~/.claude/CLAUDE.md` | Global (all projects) | Personal preferences, universal standards |
| `./CLAUDE.md` | Project root | Project architecture, team conventions |
| `./src/CLAUDE.md` | Subdirectory | Module-specific rules (e.g., frontend conventions) |

Nested CLAUDE.md files are additive — subdirectory files add to the root file, they don't replace it. This is powerful for monorepos: the root CLAUDE.md describes the whole system, while `packages/api/CLAUDE.md` adds API-specific rules.

## How Claude Auto-Builds Memory

Claude Code can also create and update CLAUDE.md automatically. When you tell Claude something that seems like persistent project knowledge — "by the way, we never use `any` in TypeScript" — Claude may suggest adding it to CLAUDE.md. You can also explicitly say:

```
> Add this to CLAUDE.md: always use Zod for input validation, never write manual validation logic
```

Over time, your CLAUDE.md becomes genuinely useful institutional memory.

## Battle-Tested CLAUDE.md Starter Template

```markdown
# Project: [Project Name]

## Overview
[2-3 sentences: what this project does and who uses it]

## Architecture
- **Frontend:** [e.g., Next.js 14, TypeScript, Tailwind CSS v4]
- **Backend:** [e.g., Node.js with Express, deployed on Railway]
- **Database:** [e.g., PostgreSQL with Prisma ORM]
- **Auth:** [e.g., Clerk for authentication]
- **State management:** [e.g., Zustand for client state, React Query for server state]

## File Structure
```
src/
  components/      # Reusable UI components (no business logic)
  features/        # Feature modules (components + logic together)
  services/        # External service integrations
  lib/             # Utilities and helpers
  types/           # Shared TypeScript types
```

## Coding Standards
- TypeScript everywhere — never use `any`, use `unknown` if needed
- All components use arrow function syntax
- CSS: Tailwind utility classes only, no custom CSS unless unavoidable
- Error handling: all errors should bubble up with context (wrap with cause)
- API responses: always use the `ApiResponse<T>` wrapper type from `types/api.ts`

## Common Commands
```bash
npm run dev          # Start development server
npm run test         # Run all tests
npm run test:watch   # Run tests in watch mode
npm run db:migrate   # Run pending migrations
npm run db:seed      # Seed the database
npm run lint         # Run ESLint
npm run type-check   # Run TypeScript type checker
```

## Git Conventions
- Never commit directly to `main` — all changes via PR
- Commit messages: `type(scope): description` (conventional commits)
- Branch names: `feature/description`, `fix/description`, `chore/description`

## Key Constraints
- Never expose database IDs in API responses — always use UUIDs
- All user input must go through Zod validation before processing
- Database queries only in service files, never in route handlers
- Environment variables must be in `.env.local` and typed in `src/lib/env.ts`

## Code Review Checklist
When reviewing or producing code, check:
1. Are all inputs validated?
2. Are errors handled and logged?
3. Is there a test for the new behavior?
4. Are there any security implications (injection, exposure, IDOR)?
5. Does the code follow the existing pattern in similar files?

## Anti-Patterns (Never Do These)
- Don't use `console.log` for debugging — use the logger service
- Don't write raw SQL — use Prisma
- Don't put business logic in React components — use hooks or services
- Don't use `setTimeout` for coordination — use proper async patterns
```

### Try It Now

Create a CLAUDE.md in a project you're working on right now. Start with just three sections: tech stack, common commands, and one key constraint. Open Claude Code, watch it read the file, and then give it a task. Notice how much context it already has.

---

# CHAPTER 3 — SLASH COMMANDS: YOUR REPEATABLE WORKFLOW ENGINE

## What Slash Commands Are

Slash commands are saved, named prompts you can invoke with `/command-name` inside a Claude Code session. Think of them as macros or shortcuts for workflows you run repeatedly.

Without slash commands, you either retype long prompts every session or paste them from a notes file. With slash commands, you type `/review-pr` and Claude knows exactly what to do.

As of late 2025, **slash commands have been merged into the Skills system** (Chapter 4). A file at `.claude/commands/review.md` and a skill at `.claude/skills/review/SKILL.md` both create `/review` and work identically. The legacy `.claude/commands/` format still works — your existing commands won't break.

## Creating Your First Slash Command

Commands are Markdown files. The filename becomes the command name.

```bash
# Create project-level command
mkdir -p .claude/commands
touch .claude/commands/review.md

# Or create a personal command available in all projects
mkdir -p ~/.claude/commands
touch ~/.claude/commands/review.md
```

Inside the file, write the prompt Claude should run:

```markdown
Review the changes in this diff carefully:

!`git diff HEAD~1`

Check for:
1. **Security issues** — any injection risks, exposed secrets, auth bypass
2. **Performance problems** — N+1 queries, missing indexes, expensive loops
3. **Logic errors** — edge cases not handled, off-by-one errors, null dereferences
4. **Code quality** — naming clarity, function complexity, dead code
5. **Test coverage** — is the new behavior tested?

Provide feedback organized by severity: Critical, Major, Minor.
```

Now in Claude Code:
```
> /review
```

Claude runs `git diff HEAD~1`, reads the output, and applies your review checklist.

## The Anatomy of a Well-Written Slash Command

**1. A clear goal statement:** What should happen? Be specific.

**2. Context injection with `!` commands:** Use `!`backtick`command`backtick to run a shell command and inject its output into the prompt. This is how you make commands dynamic.

```markdown
## Current branch state
!`git status`
!`git log --oneline -10`
```

**3. Structured output format:** Tell Claude how to organize its response so you can scan it quickly.

**4. Tool restrictions (optional):** Add YAML frontmatter to control which tools are available:

```markdown
---
allowed-tools: Read, Grep, Glob, Bash(git diff:*)
description: Review changes since last commit
---

Review the following changes...
```

## Using `$ARGUMENTS` for Parameterized Commands

Commands can accept arguments:

```markdown
# File: .claude/commands/fix-issue.md
Fix GitHub issue #$ARGUMENTS following our coding standards from CLAUDE.md.

Start by searching the codebase for related code, then implement the fix.
Write a test that would have caught this issue.
```

Usage:
```
> /fix-issue 142
```

## Sharing Commands With Your Team

Since commands live in `.claude/commands/` inside your project, they're committed to git automatically. Your whole team gets the same commands. This is one of the highest-leverage things you can standardize across a team.

## 5 Essential Slash Command Examples

**1. `/review` — PR review checklist**
```markdown
---
allowed-tools: Read, Grep, Glob, Bash(git diff:*, git log:*)
description: Full code review of recent changes
---
## Changes to review
!`git diff main...HEAD`

Review for: security, performance, logic errors, code quality, test coverage.
Format findings as: [CRITICAL] / [MAJOR] / [MINOR] with file:line references.
```

**2. `/standup` — Generate daily standup**
```markdown
---
description: Generate standup from git activity
---
## Recent work
!`git log --since="yesterday" --author="$(git config user.name)" --oneline`
!`git diff --stat HEAD~5`

Based on this git activity, write a concise standup update in this format:
- Yesterday: [what was done]
- Today: [natural next steps]
- Blockers: [anything to flag]
Keep it under 5 bullet points total.
```

**3. `/docs` — Generate/update documentation**
```markdown
---
description: Update documentation for modified files
---
## Recently changed files
!`git diff --name-only HEAD~1`

For each changed file, check if there's corresponding documentation in `/docs`.
If documentation exists and is outdated, update it.
If no documentation exists for a significant change, create it.
Use the existing documentation style as a guide.
```

**4. `/security` — Security audit**
```markdown
---
allowed-tools: Read, Grep, Glob
description: Security scan of codebase
---
Perform a security audit of this codebase. Check for:
1. Hardcoded secrets, API keys, or credentials in code
2. SQL injection vulnerabilities
3. Missing input validation on user-supplied data
4. Insecure direct object references in API endpoints
5. Missing authentication checks on sensitive routes
6. Outdated dependencies with known CVEs (check package.json)

Report findings with severity and file:line references.
```

**5. `/new-feature $ARGUMENTS` — Feature scaffolding**
```markdown
---
description: Scaffold a new feature following project conventions
---
Create a new feature called "$ARGUMENTS" following the patterns in `src/features/`.

Steps:
1. Look at an existing feature (e.g., `src/features/user/`) to understand the structure
2. Create the new feature directory with the same structure
3. Implement a basic skeleton (component, hook, service) with TODO comments
4. Add the feature to the routing if applicable
5. Create a test file skeleton

Follow all standards from CLAUDE.md.
```

### Try It Now

Create a `/review` command in your current project. Run it. Then create a second command for something you do repeatedly — starting a feature, running a migration, generating types from your API schema. The best commands come from tasks you already do, just automated.

---

# CHAPTER 4 — SKILLS: GIVING CLAUDE DOMAIN EXPERTISE

## Skills vs. Slash Commands

Slash commands are *user-triggered* — you type `/command` to activate them. Skills are more intelligent: Claude can *automatically* load them when it detects a relevant task, based on the skill's description. Think of it as the difference between a keyboard shortcut (commands) and a specialist consultant who shows up when the conversation calls for it (skills).

This matters because skills allow you to package deep domain expertise — "here's how to review React components for accessibility," "here's how to generate documentation in our style" — and have Claude apply it seamlessly without you having to remember to invoke it.

> **Mental model:** Skills are like hiring specialist contractors. When you need a plumber, you don't describe what plumbing is — you just call the plumber. Skills let you package expertise and have it show up when needed.

## The SKILL.md File Format

Every skill is a directory containing at minimum a `SKILL.md` file:

```
.claude/skills/
  code-reviewer/
    SKILL.md          ← Required: instructions + frontmatter
    checklist.md      ← Optional: supporting files
    examples/         ← Optional: reference examples
```

The SKILL.md file has two parts: YAML frontmatter (between `---` markers) and markdown body:

```markdown
---
name: code-reviewer
description: >
  Performs thorough code review with focus on security, performance, and 
  maintainability. Auto-activates when reviewing PRs or when user asks for 
  code review. Invoked manually with /code-reviewer.
allowed-tools: Read, Grep, Glob, Bash(git diff:*, git log:*)
---

# Code Review Skill

When performing a code review, follow this process:

## Step 1: Understand the Context
Read the CLAUDE.md to understand project standards.
Run `git log --oneline -5` to understand recent history.

## Step 2: Analyze the Changes
...
```

## YAML Frontmatter Fields

| Field | Description |
|---|---|
| `name` | The slash command name (`/name`) |
| `description` | What this skill does — Claude uses this to auto-activate it |
| `allowed-tools` | Restrict which tools Claude can use during this skill |
| `context: fork` | Run this skill in an isolated subagent (see Chapter 6) |
| `disable-model-invocation` | Run as a pure template with no AI generation |

## Pre-Built Anthropic Skills

Anthropic ships four official skills that handle complex file operations. These are production-grade and save you enormous effort:

| Skill | What It Does |
|---|---|
| `pptx` | Creates and edits PowerPoint presentations |
| `xlsx` | Creates and edits Excel spreadsheets |
| `docx` | Creates and edits Word documents |
| `pdf` | Reads, extracts, and manipulates PDFs |

These are available in Claude.ai's computer use environment and via the Claude API's Skills feature. They're excellent examples of what well-built skills look like.

## Progressive Disclosure: Why Claude Doesn't Load Everything

Here's something important to understand: **Claude doesn't load all skill content at the start of every session.** It only loads the *descriptions* — enough to know what skills are available. The full skill content loads only when the skill is invoked.

This is called **progressive disclosure**, and it's critical for performance. If you have 10 skills and each has 2,000 words of instructions, you don't want all 20,000 words stuffed into every session. Instead:

1. Session starts → skill descriptions loaded (small)
2. You invoke `/code-reviewer` → full code-reviewer content loads
3. Skill executes → context cleared when done

This architecture lets you build rich, detailed skill libraries without paying the context cost upfront.

## Build a Custom Skill: `code-reviewer`

Let's build a real skill from scratch:

```bash
mkdir -p .claude/skills/code-reviewer
```

Create `.claude/skills/code-reviewer/SKILL.md`:

```markdown
---
name: code-reviewer
description: >
  Comprehensive code review with security, performance, and quality checks.
  Auto-activates when user asks to review code, review a PR, or check recent changes.
  Use /code-reviewer to invoke manually.
allowed-tools: Read, Grep, Glob, Bash(git diff:*, git log:*, git status:*)
---

# Code Review Playbook

## 1. Gather Context
First, understand what changed:
```bash
git log --oneline -10
git diff --stat HEAD~1
```

Read the project CLAUDE.md for relevant standards.

## 2. Read the Changes in Detail
```bash
git diff HEAD~1
```

## 3. Apply the Review Checklist

### Security (🔴 Critical to check)
- [ ] No secrets, API keys, or credentials in code
- [ ] All user inputs validated and sanitized
- [ ] No SQL injection vulnerabilities
- [ ] Authentication/authorization checks present where needed
- [ ] No exposed internal IDs in API responses

### Performance (🟡 Important)
- [ ] No N+1 database query patterns
- [ ] No expensive operations in loops
- [ ] Pagination on list endpoints
- [ ] Appropriate caching where relevant

### Code Quality (🟢 Standard)
- [ ] Functions have a single responsibility
- [ ] Variable names are descriptive
- [ ] No dead code or commented-out blocks
- [ ] Error cases are handled explicitly

### Tests (🔵 Coverage)
- [ ] New behavior has test coverage
- [ ] Edge cases are tested
- [ ] Tests are readable and well-named

## 4. Format Your Response
Organize feedback as:
- **[CRITICAL]** — Must fix before merging
- **[MAJOR]** — Should fix, meaningful improvement
- **[MINOR]** — Nice to fix, low impact

Always include: file name, line number, what the issue is, and a suggested fix.

End with a summary: "Overall assessment: [Approve / Approve with minor changes / Request changes]"
```

## Build a Custom Skill: `api-documentation`

```markdown
---
name: api-documentation
description: >
  Generates and updates API documentation in our standard format.
  Auto-activates when new API endpoints are created or user asks to document an endpoint.
  Also invoked with /api-documentation.
allowed-tools: Read, Grep, Glob
---

# API Documentation Skill

When documenting an API endpoint, produce documentation in this exact format:

## Documentation Template

```markdown
### POST /api/[resource]

**Description:** [What this endpoint does in one sentence]

**Authentication:** Required / Not required

**Request Body:**
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| field | string | Yes | Description |

**Response (200 OK):**
```json
{
  "id": "uuid",
  "field": "value"
}
```

**Error Responses:**
| Status | Condition |
|--------|-----------|
| 400 | Invalid input |
| 401 | Not authenticated |
| 404 | Resource not found |

**Example Request:**
```bash
curl -X POST https://api.example.com/resource \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"field": "value"}'
```
```

## Process
1. Read the endpoint implementation
2. Identify all input parameters, headers, and body fields
3. Read the response type definitions
4. Check for authentication middleware
5. Note all error conditions
6. Generate documentation using the template above
7. If a `/docs/api/` directory exists, save the documentation there
```

### Try It Now

Build the `code-reviewer` skill above in a project. Invoke it with `/code-reviewer` after making a small change. Then try asking "can you review my recent changes?" without using the slash command — notice if Claude auto-activates the skill based on context.

---

# CHAPTER 5 — MCP (MODEL CONTEXT PROTOCOL): CONNECTING CLAUDE TO THE WORLD

## What MCP Is

**Model Context Protocol (MCP)** is an open standard that lets Claude connect to external tools and data sources. The "USB-C for AI" analogy is genuinely good here: before USB-C, every device had its own connector. MCP is the universal connector for AI agents.

Without MCP, Claude knows only what's in your codebase and what you tell it. With MCP, Claude can:
- Read and create GitHub issues and PRs
- Query your Postgres or Supabase database
- Search your Notion or Google Drive
- Post to Slack or send emails
- Interact with your browser (via Playwright MCP)
- Call any API you configure

## Transport Types

MCP servers come in two flavors:

**stdio (local/standard input-output):** The MCP server runs as a local process on your machine. Claude starts it as a subprocess and communicates over stdin/stdout. Fast, no network, great for tools like filesystem access, git, local databases.

**HTTP/SSE (remote):** The MCP server runs remotely and Claude communicates with it over HTTP with Server-Sent Events. Used for cloud services like GitHub, Google Drive, Slack.

## Popular MCP Servers

| Server | What It Provides |
|---|---|
| `@modelcontextprotocol/server-github` | GitHub API: issues, PRs, code search |
| `@modelcontextprotocol/server-postgres` | Direct Postgres queries |
| `@modelcontextprotocol/server-google-drive` | Read/search Google Drive |
| `@playwright/mcp` | Browser automation and testing |
| `@modelcontextprotocol/server-slack` | Read and post Slack messages |
| `@modelcontextprotocol/server-filesystem` | Extended filesystem operations |

## Configuring MCP in Claude Code

**Via CLI (easiest):**
```bash
# Add GitHub MCP
claude mcp add github-mcp npx @modelcontextprotocol/server-github

# Add Postgres MCP with environment variable
claude mcp add postgres-mcp npx @modelcontextprotocol/server-postgres \
  --env DATABASE_URL=$DATABASE_URL

# Add a remote MCP server
claude mcp add my-api-mcp --transport http https://my-mcp-server.com/sse
```

**Via `.mcp.json` in your project (for team sharing):**
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-postgres", "${DATABASE_URL}"]
    }
  }
}
```

> **Security note:** MCP servers can read and write your codebase. Only install MCP servers you trust. There have been documented supply-chain attacks on MCP packages. Stick to official Anthropic servers or well-vetted community ones.

## Using MCP in Practice

Once connected, MCP tools appear as slash commands:
```
> /mcp__github__create-issue
> /mcp__postgres__query
```

Or just ask naturally:
```
> Find all open GitHub issues tagged "bug" and summarize them
> Query the users table and show me users who signed up in the last week
```

## Build a Simple Custom MCP Server

Here's a minimal MCP server in TypeScript that exposes a custom tool:

```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new Server(
  { name: "my-custom-mcp", version: "1.0.0" },
  { capabilities: { tools: {} } }
);

// Define a tool
server.setRequestHandler("tools/list", async () => ({
  tools: [{
    name: "get_deployment_status",
    description: "Get the current deployment status from our internal API",
    inputSchema: {
      type: "object",
      properties: {
        environment: { type: "string", enum: ["staging", "production"] }
      },
      required: ["environment"]
    }
  }]
}));

// Handle tool calls
server.setRequestHandler("tools/call", async (request) => {
  if (request.params.name === "get_deployment_status") {
    const env = request.params.arguments.environment;
    // Call your internal API
    const status = await fetchDeploymentStatus(env);
    return {
      content: [{ type: "text", text: JSON.stringify(status) }]
    };
  }
});

const transport = new StdioServerTransport();
await server.connect(transport);
```

## Decision Matrix: MCP vs. Skills vs. Slash Commands

| Need | Use |
|---|---|
| Connect to external service (GitHub, DB, Slack) | MCP |
| Encode a workflow Claude follows step-by-step | Skill |
| Quick repeatable prompt you trigger manually | Slash command |
| Something that should auto-activate | Skill (with good description) |
| Real-time data Claude can query | MCP |
| Reusable instructions for your team | Skill or slash command in git |

### Try It Now

Add the Playwright MCP to Claude Code:
```bash
claude mcp add playwright npx @playwright/mcp@latest
```

Then ask Claude to open a browser and test one of your pages. If you don't want to install it, just add the GitHub MCP and ask Claude to list your open issues.

---

# CHAPTER 6 — SUBAGENTS: PARALLEL EXECUTION AND CONTEXT ISOLATION

## The Context Pollution Problem

Here's a problem you'll run into: you ask Claude to do something that requires a lot of exploration — reading 20 files, researching an approach, gathering information. By the time Claude is done gathering, your main conversation context is stuffed with noise. The next request has to wade through all of that.

**Subagents solve this.** A subagent is an isolated Claude instance that does work in its own context window and returns only the result to your main conversation. The junk stays in the subagent's context. You get the clean answer.

> **Analogy:** Think of it like hiring a research assistant. You don't want them reading their research notes aloud to you for an hour. You want them to go away, research, and come back with a one-page brief.

## Subagent Types

Claude Code has four built-in subagent types:

| Type | Description | Best For |
|---|---|---|
| `claude-code` | Full Claude Code instance with all tools | Multi-step coding tasks |
| `explorer` | Read-only: Read, Grep, Glob (no writes) | Safe codebase exploration, research |
| `task-planner` | Planning-focused, extended thinking | Architecture decisions, complex plans |
| `code-writer` | Write-focused, optimized for implementation | Heavy implementation work |

## Defining a Custom Subagent

Subagents are configured as YAML files in `.claude/agents/`:

```yaml
# .claude/agents/security-auditor.yaml
name: security-auditor
description: >
  Performs security audits of code changes. Auto-activates when changes 
  are made to authentication, authorization, or data handling code.
  Invoked with /security-auditor.
model: claude-sonnet-4-6
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash(git diff:*, git log:*)
system_prompt: |
  You are a security-focused code reviewer. Your job is to find security 
  vulnerabilities in code changes. You are thorough, precise, and you 
  never miss an injection vulnerability or authentication bypass.
  
  You report findings in this format:
  [CRITICAL] File:line — Description of vulnerability — Suggested fix
  [HIGH] File:line — Description — Suggested fix
  [MEDIUM] File:line — Description — Suggested fix
  
  Always end with a count: "X critical, Y high, Z medium findings."
```

## Spawning Subagents for Parallel Work

Here's where it gets powerful. You can run multiple subagents simultaneously:

```
> I need a thorough analysis of our checkout flow. 
> Run two parallel investigations:
> 1. A security audit of all payment-related code
> 2. A performance analysis of the database queries in checkout
> Report findings from both when complete.
```

Claude will spawn two subagents, run them concurrently, and synthesize their results. Each subagent runs in isolation — they don't interfere with each other or pollute your main context.

## Async Agents: Fire and Forget with Ctrl+B

You can also background a running agent with **Ctrl+B**. This is great for long tasks:

1. Start a task: "Refactor the entire authentication module to use our new auth service"
2. While Claude is working, press **Ctrl+B** to push it to background
3. Start a new task in the foreground
4. Claude continues working on the background task
5. It notifies you when done (you can set up desktop notifications with hooks — Chapter 7)

## Tool Permissions: Restricting What Subagents Can Do

A crucial pattern: give subagents only the tools they need, nothing more.

```yaml
# A read-only research agent — cannot modify anything
allowed-tools:
  - Read
  - Grep
  - Glob
  - WebSearch    # Only if needed

# A documentation agent — can write, but only .md files
allowed-tools:
  - Read
  - Grep
  - Glob
  - Write(*.md)
  - Bash(echo:*, cat:*)
```

This is safety through least-privilege. A documentation agent that can't touch `.ts` files can't accidentally break your code.

## Skills + Subagents: The `context: fork` Pattern

Add `context: fork` to a skill's frontmatter to run the skill in an isolated subagent:

```markdown
---
name: research-codebase
description: Explore the codebase to understand a specific area
context: fork
subagent_type: explorer
---

Perform a thorough exploration of the authentication system.
Read all relevant files and return a concise summary covering:
1. How authentication flows work end-to-end
2. Which files are most relevant to modify for auth changes
3. Any patterns or anti-patterns you observe
```

When you invoke `/research-codebase`, Claude spawns an isolated explorer subagent, it does all the reading, and returns just the summary to your main conversation.

### Try It Now

In a project with substantial code, try this prompt:
```
> Use a subagent to explore the database layer and return a summary of all the tables, their relationships, and the most common query patterns. Don't modify anything, just explore and report.
```

Watch the context isolation in action — your main context stays clean while the subagent does the heavy lifting.

---

# CHAPTER 7 — HOOKS: AUTOMATING ACTIONS AROUND CLAUDE'S BEHAVIOR

## What Hooks Are

Hooks are scripts that run automatically at specific points in Claude Code's lifecycle — before or after certain events. They're deterministic automation, not AI intelligence. Think of them like Git hooks: you write a script, you attach it to an event, it runs every time.

> **Analogy:** Hooks are the automatic door-closer at a hotel. You don't tell the door to close every time. The hook does it for you.

## The Events System

Claude Code fires hooks at these events:

| Event | When It Fires |
|---|---|
| `PreToolUse` | Before Claude uses any tool |
| `PostToolUse` | After Claude uses a tool (write, bash, etc.) |
| `PreSendMessage` | Before a message is sent to the API |
| `PostSendMessage` | After a response is received |
| `SessionStart` | When a Claude Code session begins |
| `SessionEnd` | When a session ends |
| `TaskCreated` | When a new task/subagent is spawned |

Each event can also have a **matcher** — a pattern that limits when the hook fires. `PostToolUse` with matcher `Write(*.py)` only fires when Claude writes a Python file.

## Hook Configuration

Hooks are configured in `settings.json` (in `~/.claude/settings.json` for global, `.claude/settings.json` for project):

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write(*.py)",
        "hooks": [
          {
            "type": "command",
            "command": "python -m black \"$CLAUDE_TOOL_INPUT_PATH\""
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "/path/to/check-dangerous-command.sh"
          }
        ]
      }
    ]
  }
}
```

## Common Hook Recipes

**Auto-format Python files after writes:**
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write(*.py)",
      "hooks": [{"type": "command", "command": "python -m black \"$CLAUDE_TOOL_INPUT_PATH\""}]
    }]
  }
}
```

**Run linter after TypeScript writes:**
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write(*.ts,*.tsx)",
      "hooks": [{"type": "command", "command": "npx eslint \"$CLAUDE_TOOL_INPUT_PATH\" --fix --quiet"}]
    }]
  }
}
```

**Desktop notification when task completes:**
```json
{
  "hooks": {
    "SessionEnd": [{
      "hooks": [{
        "type": "command",
        "command": "osascript -e 'display notification \"Claude finished!\" with title \"Claude Code\"'"
      }]
    }]
  }
}
```

**Block dangerous bash commands:**

Create `~/.claude/hooks/check-bash.sh`:
```bash
#!/bin/bash
# Called with the bash command as an argument
CMD="$1"

# Block force pushes
if echo "$CMD" | grep -q "git push.*--force\|git push.*-f"; then
  echo "BLOCKED: Force push detected. Use --force-with-lease or confirm manually." >&2
  exit 1
fi

# Block dropping tables
if echo "$CMD" | grep -qi "DROP TABLE\|DROP DATABASE"; then
  echo "BLOCKED: Destructive SQL detected. Run this manually." >&2
  exit 1
fi

# Allow everything else
exit 0
```

```json
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "hooks": [{
        "type": "command",
        "command": "~/.claude/hooks/check-bash.sh \"$CLAUDE_TOOL_INPUT_COMMAND\""
      }]
    }]
  }
}
```

**Run tests after a test file is written:**
```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": "Write(*.test.ts,*.spec.ts)",
      "hooks": [{"type": "command", "command": "npm test -- --testPathPattern=\"$CLAUDE_TOOL_INPUT_PATH\" --passWithNoTests"}]
    }]
  }
}
```

## Interactive Hook Configuration

You can also configure hooks interactively:
```
> /hooks
```

Claude will guide you through setting up hooks for your project.

### Try It Now

Add one hook to your global settings: a desktop notification when sessions end (macOS uses `osascript`, Linux uses `notify-send`). Background a task with Ctrl+B, let it run, and see the notification fire. Then add the auto-format hook for your primary language.

---

# CHAPTER 8 — PLUGINS: PACKAGING AND SHARING YOUR SETUP

## What Plugins Are

A **plugin** is a bundle that packages multiple Claude Code customizations — slash commands, subagents, hooks, skills, and metadata — into a single installable unit. Think of plugins as npm packages, but for your Claude Code configuration.

Without plugins, sharing your Claude Code setup with a teammate means: "Here, copy these files from `.claude/skills/`, `.claude/agents/`, and also update your `settings.json` with these hooks." With plugins, it's:

```bash
claude plugin install team-workflows
```

## Plugin Structure

A plugin is a directory with this structure:

```
my-plugin/
  plugin.json          ← Plugin manifest
  skills/              ← Skills to install
    code-reviewer/
      SKILL.md
  agents/              ← Subagent configs
    security-auditor.yaml
  hooks/               ← Hook scripts
    check-bash.sh
  settings-patch.json  ← Settings additions (merged, not replaced)
  README.md
```

The `plugin.json` manifest:
```json
{
  "name": "team-workflows",
  "version": "1.2.0",
  "description": "Standard engineering workflows for Acme Corp",
  "author": "engineering@acme.com",
  "skills": ["skills/code-reviewer"],
  "agents": ["agents/security-auditor.yaml"],
  "hooks": ["hooks/check-bash.sh"],
  "settingsPatch": "settings-patch.json"
}
```

## Installing Plugins

```bash
# Install from GitHub
claude plugin install github:myorg/team-claude-plugins

# Install from local directory
claude plugin install ./path/to/my-plugin

# List installed plugins
claude plugin list

# Disable a plugin without uninstalling
claude plugin disable team-workflows

# Uninstall
claude plugin uninstall team-workflows
```

## Building a Plugin for Your Team

1. Create the directory structure above
2. Build and test each component individually
3. Write a clear README with what each skill/command does
4. Commit to a shared git repo (private or public)
5. Share the install command with your team

The key insight: plugins let you standardize an engineering team's AI workflow the same way you standardize linters, formatters, or IDE settings. The team gets consistent behavior, consistent review checklists, consistent automation — all from one install.

## Community Plugins

The community at `github.com/ccplugins/awesome-claude-code-plugins` maintains a curated list of plugins. Some notables:

- **session-summary:** Logs a summary of each session to a local file
- **git-guardian:** Hooks that prevent common git mistakes
- **security-kit:** A bundle of security-focused review skills

### Try It Now

Take the code-reviewer skill you built in Chapter 4 and the `/review` command from Chapter 3 and package them together as a plugin. Write a `plugin.json` and a README. Then install it fresh from the directory and verify it works.

---

# CHAPTER 9 — THE CLAUDE AGENT SDK: BUILDING YOUR OWN AGENTS

## What the Claude Agent SDK Is For

The **Claude Agent SDK** (formerly "Claude Code SDK") lets you use Claude Code's agentic loop as a library in your own programs. Instead of running `claude` interactively in a terminal, you write code that starts Claude programmatically, gives it tasks, and processes results.

Use the Agent SDK when you want to:
- Build internal tools with a chat interface (but backed by Claude's full agent capabilities)
- Run batch processing — e.g., "for each of these 50 files, do X"
- Build autonomous agents that run on a schedule or as part of CI/CD
- Prototype new agent workflows before building production infrastructure

Don't use the Agent SDK when Claude Code's interactive mode or GitHub Actions integration would suffice. The SDK is for when you need *programmatic* control.

## The Agentic Loop

Understanding what the SDK does internally:

```
You provide: task + tools + permissions
Claude receives: task + context
Claude loop:
  1. Think: plan the next action
  2. Act: call a tool (read file, run bash, search web)
  3. Observe: receive tool result
  4. Repeat until task complete
  5. Return: final response to you
```

The SDK gives you hooks into every step of this loop.

## A Simple Agent with the SDK

```typescript
import { ClaudeCode } from "@anthropic-ai/claude-code";

const agent = new ClaudeCode({
  // Optional: configure tools, permissions, context
  allowedTools: ["Read", "Grep", "Glob"],
  cwd: "/path/to/project"
});

// Run a task and stream results
const result = await agent.run("Find all TODO comments in the codebase and list them with file paths", {
  onMessage: (msg) => {
    // Stream output to user
    if (msg.type === "text") process.stdout.write(msg.content);
  }
});

console.log("Task complete:", result.summary);
```

## Batch Processing Pattern

One of the highest-leverage Agent SDK patterns: running Claude on many items in parallel.

```typescript
import { ClaudeCode } from "@anthropic-ai/claude-code";
import { readdir } from "fs/promises";

const files = await readdir("./src/components");

// Process 5 files at a time
const CONCURRENCY = 5;
const results = [];

for (let i = 0; i < files.length; i += CONCURRENCY) {
  const batch = files.slice(i, i + CONCURRENCY);
  const batchResults = await Promise.all(
    batch.map(async (file) => {
      const agent = new ClaudeCode({
        allowedTools: ["Read"],
        cwd: process.cwd()
      });
      return agent.run(
        `Review src/components/${file} for accessibility issues. Return a JSON object with: { file, issues: [{line, description, severity}] }`,
        { outputFormat: "json" }
      );
    })
  );
  results.push(...batchResults);
}

// Write results
await writeFile("accessibility-report.json", JSON.stringify(results, null, 2));
```

## Orchestrating Multi-Agent Workflows

```typescript
// Lead agent coordinates several specialist agents
const leadAgent = new ClaudeCode({ allowedTools: ["Read", "Task"] });

await leadAgent.run(`
  Implement the new user notification system described in feature-spec.md.
  
  Coordinate the following parallel work:
  1. Spawn a Task agent to research how other notification systems are implemented 
     in this codebase (read-only exploration)
  2. Spawn a Task agent to design the database schema changes needed
  3. Once both return results, implement the feature using their findings
  4. Spawn a Task agent to write comprehensive tests for what you implement
  
  Do not proceed to implementation until the research is complete.
`);
```

## Cost Tracking

Always track costs in production:

```typescript
const agent = new ClaudeCode({
  onUsage: (usage) => {
    console.log(`Input tokens: ${usage.inputTokens}`);
    console.log(`Output tokens: ${usage.outputTokens}`);
    // Log to your cost tracking system
  }
});
```

### Try It Now

Write a small SDK script that takes a directory path and generates a one-sentence description of each file in it. Run it on your project's `src/components/` or equivalent. This gives you a feel for the programmatic loop.

---

# CHAPTER 10 — ADVANCED VIBE CODING PATTERNS

## The Lead Agent + Subagents Pattern

For complex, multi-file tasks, the most reliable pattern is:

1. **Lead agent** receives the task and plans
2. **Research subagent** explores the codebase (read-only)
3. **Implementation** is done by the lead agent with the research output
4. **Test subagent** writes tests independently
5. **Review** brings it together

This mirrors how a well-run engineering team actually works. The lead engineer doesn't read every file themselves — they delegate research, synthesize findings, and implement with full context.

```
> Implement a rate limiting middleware for our Express API.

> Process:
> 1. First, use a read-only subagent to explore: how existing middleware is structured in this codebase, what libraries are already installed (check package.json), and any existing auth/security patterns.
> 2. Report back what you found.
> 3. Then implement the rate limiter following those patterns.
> 4. Then write tests.
> Get my approval before making any file changes.
```

## Prompting for Large, Multi-File Tasks

The biggest challenge with large tasks: Claude can drift, forget constraints, or take a shortcut partway through.

**Break it into phases with explicit checkpoints:**

```
> Phase 1: Understand the scope. Read all files related to the payment flow and tell me which ones need to change. Don't modify anything yet.

[Claude responds with a file list and plan]

> That looks right. Phase 2: Implement the changes in src/services/payment.ts only. Show me the diff before committing.

[Claude shows diff, you approve]

> Phase 3: Update the tests in src/services/__tests__/payment.test.ts to cover the new behavior.
```

This is slower than saying "refactor the whole payment system" but produces dramatically better results for complex changes.

## Using CLAUDE.md + Skills + MCP Together

When these three work in concert, the result is a highly capable, context-aware agent:

- **CLAUDE.md** gives Claude project knowledge (conventions, architecture, constraints)
- **Skills** give Claude procedural knowledge (how to review, how to document, how to deploy)
- **MCP** gives Claude real-time data access (GitHub issues, database state, deployment status)

Example full workflow:

```
> /deploy-staging 
```

Behind this single command, the skill does:
1. Reads CLAUDE.md to know branch conventions and deployment checklist
2. Uses GitHub MCP to check for open blocking issues
3. Runs tests via bash
4. Creates a deployment PR via GitHub MCP
5. Posts a status update to Slack via Slack MCP

Everything automated. Everything informed by project context.

## Full Feature Lifecycle Workflow

Here's a complete "spec to PR" workflow:

```
> New feature: users need to be able to export their data as CSV.

> Step 1: Read the existing user data model and tell me what fields we have.
```

*[Claude reads models, reports]*

```
> Good. Step 2: Write a spec for this feature — what the API endpoint looks like, the CSV format, and what error cases we handle. Save it to docs/specs/data-export.md
```

*[Claude writes spec, saves to file]*

```
> Step 3: Implement the feature. Follow the spec exactly. Use our existing CSV utility if there is one, otherwise add the `csv-stringify` package.
```

*[Claude implements, shows diffs]*

```
> Step 4: Write tests. Cover the happy path, empty data case, and large dataset case.
```

*[Claude writes tests]*

```
> Step 5: Run the tests and fix any failures.
```

*[Claude runs tests, fixes issues]*

```
> Step 6: Create a PR with a description that references the spec.
```

*[Claude uses GitHub MCP to create PR]*

This entire workflow, from idea to PR, can run in 15-30 minutes with light supervision.

## Handling Errors and Agent Drift

**Agent drift** is when Claude gradually deviates from your original intent — solving sub-problems in ways that don't fit the larger goal.

Signs you're experiencing drift:
- Claude is making changes you didn't ask for
- The implementation approach diverges from what was agreed
- Error handling is getting complicated in unexpected ways

What to do:
1. **Interrupt early** (Ctrl+C). Don't let it finish if it's going wrong.
2. **Re-anchor to the spec.** Paste the original requirement again.
3. **Use checkpoints.** "Show me what you've done so far before continuing."
4. **Use /rewind** to restore a previous checkpoint if available.

```
> Stop. Let's reset. The goal is X. You started solving Y. Let's go back to the last clean state and try a different approach.
```

## Measuring and Reducing Token Costs

Agentic workflows use a lot of tokens. Here's how to keep costs reasonable:

**Use `/compact` to compress history.** Claude Code automatically compresses old conversations, but you can trigger it manually when context is bloated.

**Scope your tasks.** "Fix this specific bug in this specific file" uses far fewer tokens than "audit the entire codebase."

**Use read-only subagents for research.** They can gather a lot of context and return a compressed summary.

**Don't include large files unnecessarily.** When using `@file` references, be specific. Don't include a 2,000 line file when Claude only needs a 50-line function.

**Use the `explorer` subagent type for read-heavy tasks.** It's optimized for this and won't load write tools unnecessarily.

### Try It Now

Take a feature you've been planning to build. Write a prompt that breaks it into phases with explicit checkpoints. Run Phase 1 only — just exploration and planning. Evaluate the plan before proceeding. This discipline alone will double your quality.

---

# CHAPTER 11 — QUICK REFERENCE & CHEAT SHEETS

## Key Commands

| Command | Description |
|---|---|
| `claude` | Start an interactive session |
| `claude -p "task"` | Run a single task (non-interactive) |
| `claude --continue` | Resume the last session |
| `claude doctor` | Check installation health |
| `claude mcp list` | List connected MCP servers |
| `claude mcp add <name> <command>` | Add an MCP server |
| `claude plugin list` | List installed plugins |
| `claude plugin install <source>` | Install a plugin |

## Keyboard Shortcuts (Interactive Mode)

| Shortcut | Action |
|---|---|
| `Ctrl+C` | Interrupt/cancel current operation |
| `Ctrl+B` | Background current task (async) |
| `Ctrl+R` | Search prompt history |
| `Escape` | Cancel current input |
| `Esc Esc` | Restore from last checkpoint |
| `/compact` | Compress conversation history |
| `/rewind` | Restore a session checkpoint |
| `/clear` | Clear conversation history |
| `/help` | Show all available commands |
| `/bug` | Report a bug to Anthropic |

## CLAUDE.md Template (Copy-Paste Ready)

```markdown
# Project: [Name]

## Overview
[What this project does and who uses it — 2 sentences]

## Tech Stack
- **Frontend:** [e.g., Next.js 14, TypeScript, Tailwind v4]
- **Backend:** [e.g., Node.js, Express, deployed on Railway]
- **Database:** [e.g., PostgreSQL with Prisma]
- **Auth:** [e.g., Clerk]
- **Key libraries:** [list important ones]

## File Structure
[Brief description of where things live]

## Commands
```bash
npm run dev       # Start dev server
npm run test      # Run tests
npm run build     # Production build
npm run lint      # Run linter
```

## Coding Standards
- [Language/framework preference]
- [Naming conventions]
- [Error handling approach]
- [Test requirements]

## Key Constraints
- [Things Claude must never do]
- [Required patterns to follow]
- [Review checklist items]

## Anti-Patterns (Don't Do These)
- [Common mistakes to avoid]
```

## SKILL.md Template (Copy-Paste Ready)

```markdown
---
name: skill-name
description: >
  [What this skill does in 1-2 sentences. Be specific — Claude uses this 
  description to decide when to auto-activate the skill. Include trigger words 
  like "when reviewing", "when creating", "when debugging".]
allowed-tools: Read, Grep, Glob
# Optional:
# context: fork    ← run in isolated subagent
# subagent_type: explorer
---

# [Skill Name]

## When to Use This Skill
[Conditions that indicate this skill should be applied]

## Process
[Step-by-step instructions Claude should follow]

## Output Format
[How Claude should structure its response]

## Examples
[Optional: sample inputs and expected outputs]
```

## Slash Command Template (Copy-Paste Ready)

```markdown
---
allowed-tools: Read, Grep, Glob, Bash(git diff:*, git log:*)
description: [What this command does — shown in /help]
---

# [Command Name]

## Context
!`git status`

## Task
[The actual prompt/instruction Claude should follow]

## Output Format
[How to structure the response]
```

## MCP Config Template (`.mcp.json`)

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "postgres": {
      "command": "npx",
      "args": [
        "@modelcontextprotocol/server-postgres",
        "${DATABASE_URL}"
      ]
    }
  }
}
```

## The Master Decision Matrix

| You Need To... | Use This |
|---|---|
| Give Claude persistent project knowledge | **CLAUDE.md** |
| Encode a repeatable workflow Claude follows automatically | **Skill** |
| Create a quick named shortcut you trigger manually | **Slash Command** (`.claude/commands/`) |
| Connect Claude to GitHub, Slack, a database, or an API | **MCP Server** |
| Run heavy research without polluting main context | **Subagent** (explorer type) |
| Run parallel work | **Subagents** (multiple) |
| Run a long task without blocking | **Ctrl+B** (async background) |
| Auto-format/lint after file changes | **Hook** (PostToolUse) |
| Block dangerous commands | **Hook** (PreToolUse) |
| Package your whole setup for your team | **Plugin** |
| Build a custom agent or batch processing tool | **Claude Agent SDK** |
| Orchestrate a complex multi-step feature | **Lead agent + subagents** pattern |

## Top 10 Prompting Tips for Vibe Coding

1. **Specify the file or function** you want Claude to work on. Vague scope = vague output.

2. **State constraints explicitly.** "Don't change the public API." "Don't add new dependencies." "Keep the test suite green."

3. **Use phases with checkpoints** for multi-file tasks. "Do X first, show me the result, then we'll proceed to Y."

4. **Inject current state with `!` commands** in slash commands: `!``git diff HEAD~1`` `, `!``npm test``.

5. **Ask for a plan before implementation.** "Before writing any code, describe your approach. I'll approve before you start."

6. **Use negative constraints.** "Do NOT use the global state — use local component state instead."

7. **Reference existing patterns.** "Look at how we implemented the user invite feature and follow the same pattern."

8. **Be specific about output format.** "Return a JSON array, not prose." "Format findings as a table with severity, file, and description columns."

9. **Interrupt and redirect early.** If Claude is going the wrong direction, Ctrl+C and correct immediately. Don't let it finish a wrong implementation.

10. **Build your CLAUDE.md incrementally.** After every session where you found yourself correcting Claude, add that correction to CLAUDE.md. Over time it becomes genuinely powerful institutional memory.

---

## Closing Thoughts

Vibe Coding is not about typing less. It's about thinking at a higher level — spending your cognitive effort on architecture, clarity, and judgment rather than syntax and boilerplate. The developers who get the most out of Claude Code aren't the ones who type the shortest prompts. They're the ones who invest in their setup: a strong CLAUDE.md, a library of well-crafted skills, hooks that automate the boring stuff, and a reliable workflow for moving from idea to PR.

The features in this guide compound. Start with CLAUDE.md. Add slash commands for your most repetitive tasks. Build a skill or two for your most common workflows. Add an MCP server for your most-used external tool. Each addition makes the next interaction faster and better.

The ceiling is genuinely high. Claude Code can orchestrate multi-agent workflows, run tests, interact with GitHub, search the web, query your database, and deliver working code — all from a single well-crafted prompt. Get there one chapter at a time.

---

*Official documentation: `docs.claude.com` | Community resources: `github.com/hesreallyhim/awesome-claude-code`*
