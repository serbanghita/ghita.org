---
layout: post
title: "Claude Code - Plan, Critique, Execute"
description: "A custom workflow using Claude Code slash commands to write plans, get AI critiques, iterate, and execute - keeping you in control of code changes."
date: 2026-02-03 10:00:00 +0000
categories: [ai-workflows, developer-tools]
tags: [claude-code, llm, planning, software-development]
---

> How I Use Claude Code for Planning Changes in My Projects

<!--more-->

When I work on an existing codebase, I often need to make changes that require some thinking beforehand. Adding a feature, refactoring a module, or fixing a complex bug - these are not things I want an AI to just start coding without a plan.

So I built a simple workflow using Claude Code slash commands that lets me write a plan, get feedback on it, iterate until I'm happy, and only then execute it.

The repository is at [https://github.com/serbanghita/claude-code-plan-critique](https://github.com/serbanghita/claude-code-plan-critique).

---

## The Problem

Claude Code is good at executing tasks, but sometimes it just starts writing code before fully understanding what needs to happen.
On larger changes, this leads to:

- Incomplete implementations
- Missed edge cases
- Changes that don't fit the existing architecture and system design
- Wasted time fixing and replanning AI-generated code

I wanted to **stay in control**. Write my own plan, get it reviewed, refine it, and only then let the AI execute it.

Real-world examples:
1. A game engine that uses specific libraries written by me and an ECS paradigm for working with objects.
2. A game map editor that simulates TiledEditor but with my own JSON conventions and only a couple of tools (select, draw, erase, patrol).
3. A whiteboard that renders everything in webgl primitives but the underlying state management is handled in an ECS fashion.
4. A website that has to follow a specific format of pages, which require a very basic client-side and server-side implementation.

---

## The Workflow

```
                +------------------+
                |  /plan-create    |
                +--------+---------+
                         |
                         v
                +------------------+
                |  Edit plan.md    |<-----------+
                +--------+---------+            |
                         |                      |
                         v                      |
                +------------------+            |
                |  /plan-critique  |        Iteration
                +--------+---------+            |
                         |                      |
                         v                      |
                +------------------+    No      |
                | Satisfied?       +-----------+
                +--------+---------+
                         | Yes
                         v
                +------------------+
                |  /plan-execute   |
                +--------+---------+
                         |
                         v
                +------------------+
                |  /plan-archive   |
                +------------------+
```

1. `/plan-create` - Creates a folder with a plan.md template
2. Edit plan.md - Write your plan in plain markdown
3. `/plan-critique` - Claude reviews your plan against the codebase
4. Iterate - Read critique, update plan, run critique again
5. `/plan-execute` - Claude implements the plan step by step
6. `/plan-archive` - Archive the completed plan

---

## Why This Works for Me

The key is the iteration loop between writing and critique. I'm not asking Claude to write the plan for me - **I write it myself**. But I am asking Claude to review it against:

- The actual codebase (does this file exist? is this the right pattern?)
- My project standards in `CLAUDE.md`. This is also mandatory in order to save tokens for future planning.
- Dependencies and sequencing (should step 2 come before step 1?)
- Scope (am I trying to do too much in one plan?). Super important! I don't want to work with a big context and have to constantly compensate for LLM aberrations.

This catches problems **before** any code is written. The critique might tell me that a function I planned to modify doesn't exist, or that my approach conflicts with how the rest of the codebase works. Critique also points out which lines from the plan it is referring to, making my life easier when comparing the Plan and Critique side by side. You need a big monitor ... hint: LG DualUp.

After a few iterations, when the critique comes back clean, I know the plan is solid and execution will go smoothly.
Most of the time, it has the right result. It matters a lot how much effort I've put into the original plan.
I also felt that "planning" in a single-dimensional file starts to be limiting ... I wonder what improvements can be done in this area.

---

## How This Differs from Claude Code's Built-in Plan Mode

Claude Code has a built-in Plan Mode (press Shift+Tab twice or start with `--permission-mode plan`). It puts Claude in a read-only state where it analyzes your codebase and proposes a plan for you.

The key difference is who writes the plan:

| Aspect              | Built-in Plan Mode              | This workflow                               |
|---------------------|---------------------------------|---------------------------------------------|
| Who writes the plan | Claude proposes it              | You write it                                |
| Iteration           | Single pass - approve or reject | Multiple critique rounds                    |
| Persistence         | Lives in conversation only      | Saved in plan.md files                      |
| Control             | Claude leads, you approve       | You lead, Claude reviews                    |
| Best for            | Exploring options when unsure   | Executing a change you already have in mind |

Plan Mode is useful when you want Claude to figure out how to do something. My workflow is for when I already know what I want - I just want validation that my plan makes sense before execution.

Another practical difference: the critique step checks if your plan can be split into smaller independent plans. Smaller plans mean smaller context windows, which leads to better execution. Plan Mode does not do this.

---

## Example: What a Plan Looks Like

Here is a simple plan I might write:

```markdown
# Add rate limiting to API endpoints

Implement rate limiting for the public API to prevent abuse.

## Configuration

- Add rate limit settings to config.yaml
- Default: 100 requests per minute per IP
- Configurable per endpoint

## Middleware

- Create RateLimiter middleware in src/middleware/
- Use sliding window algorithm
- Store counters in Redis (already used for sessions)

## Apply to routes

- Add middleware to all /api/public/* routes
- Return 429 Too Many Requests when limit exceeded
- Include Retry-After header in response
```

After running `/plan-critique`, Claude might respond with:

- "Line 12: src/middleware/ directory does not exist - the project uses src/lib/ for utilities"
- "Redis is not used for sessions in this project - check src/config/database.ts, sessions use PostgreSQL"
- "Consider adding the rate limit configuration to the existing src/config/api.ts instead of creating a new file"

I update my plan based on this feedback, run critique (`/plan-critique`) again, and repeat until the issues are resolved.

---

## Setting Up Your Project Standards

The critique step relies on having a CLAUDE.md file in your project root. This file tells Claude about your project conventions. It does not need to be complicated. Here is an example:

```markdown
# Project Standards

## Architecture
- Express.js backend with TypeScript
- PostgreSQL database with Prisma ORM
- All new code goes in src/

## Conventions
- Use snake_case for database columns
- Use camelCase for TypeScript variables
- Error responses follow RFC 7807 format

## Testing
- Jest for unit tests
- Test files live next to source files (foo.ts, foo.test.ts)
```

When Claude critiques your plan, it checks against these standards. If your plan proposes putting code in the wrong place or using the wrong naming convention, the critique will catch it.

Hint: If you're lazy, just tell Claude Code to generate a `CLAUDE.md` file for you and then you can adjust it.

---

## Keeping Context Small

One thing the critique step does is recommend splitting large plans. If you try to do too much in one plan, Claude might say:

"This plan contains three independent changes: adding rate limiting, refactoring the database layer, and updating the auth flow. Consider splitting into separate plans."

This matters because smaller plans mean:

- Less context for Claude to hold during execution
- More focused critique iterations
- Easier to resume if something fails
- Independent tasks can proceed without blocking each other

---

## Getting Started

Copy the commands to your project:

```bash
git clone https://github.com/serbanghita/claude-code-plan-critique.git && \
mkdir -p .claude/commands && \
cp -r claude-code-plan-critique/.claude/commands/* .claude/commands/ && \
rm -rf claude-code-plan-critique
```

Restart Claude Code if it is already running, then run `/plan-create` to start your first plan.

---

If you work with Claude Code on existing projects and want more control over how changes are planned and executed, give this a try.

