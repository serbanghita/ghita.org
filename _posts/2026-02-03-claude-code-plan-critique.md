---
layout: post
title: "Claude Code - Plan, Critique, Execute, Archive"
description: "A custom workflow using Claude Code slash commands to write plans, get AI critiques, iterate, and execute - keeping you in control of code changes."
date: 2026-02-03 10:00:00 +0000
categories: [ai-workflows, developer-tools]
tags: [claude-code, llm, planning, software-development]
---

> How I Use Claude Code for Planning Changes in My Projects

If you think about it, every change in your software—be it a **new feature**, **refactor**, **bug fix**, or **complex docs**—requires a planning phase.  

I want to catch potential problems early—not deal with them later when the codebase has grown. By iterating on my plan before executing it, I minimize the mental gymnastics down the road.

So I built a simple workflow using Claude Code slash commands that lets me write a plan, get feedback on it, iterate until I'm happy, and only then execute it.

The repository is at [serbanghita/claude-code-plan-critique](https://github.com/serbanghita/claude-code-plan-critique).

---

## The Problem

Claude Code is good at executing tasks, but sometimes it just starts writing code before fully understanding what needs to happen.
This is not Claude's fault, this is a software engineer planning mistake.  
Claude cannot infer every edge case, incomplete details, A/B decisions, changes that don't fit architecture.

I wanted to **stay in control** of my project while **letting go** of the coding aspect per-se.

---

## The Workflow

Building software is an iterative phase. Call it Waterfall[0], Agile, Lean or whatever the army of consultants and coaches
stamped into your engineer lizard brain as the right process of developing software is, they all have on thing in common: **iterations**

The iteration comes from the loop of _You_: "Updating the Plan" and then _Claude Code_: "Criticizing the Plan".

```
  ┌─────────────┐
  │ Create Plan │
  └──────┬──────┘
         │
         ▼
  ┌─────────────────────┐
  │ Update Plan         │◄──────┐
  │ (N iterations)      │       │
  └──────────┬──────────┘       │
             │                  │
             ▼                  │
  ┌───────────────────────┐     │
  │ Claude: Critique Plan │     │
  │ (N iterations)        │     │
  └──────────┬────────────┘     │
             │                  │
             ▼                  │
        ┌──────────┐            │
        │Satisfied?│            │
        └────┬─────┘            │
             │                  │
       ┌─────┴─────┐            │
       │           │            │
      Yes          No───────────┘
       │
       ▼
  ┌─────────┐
  │ Execute │
  └────┬────┘
       │
       ▼
  ┌─────────┐
  │ Archive │
  └─────────┘
```

So after [installing the commands]([https://github.com/serbanghita/claude-code-plan-critique?tab=readme-ov-file#install]) in your project's `.claude` folder,
in your Claude Code console do the following:

1. `/plan-create` - Creates a folder with a `.planning/[plan name]/plan.md` template
2. Edit `.planning/[plan name]/plan.md` - Write your plan in plain markdown.
3. `/plan-critique` - Claude reviews your plan against the codebase.
4. Iterate - Read critique, update plan, run critique again.
5. `/plan-execute` - Claude implements the plan step by step.
6. `/plan-archive` - Archive the completed plan.

---

## Why This Works

The key, as I said before, is the iteration loop between writing (you) and critique (Claude).  
I'm not asking Claude to write the plan for me - **I write it myself**. 

But I am asking Claude to review it against:

- The actual codebase (does this file exist? is this the right pattern?)
- My project standards in `CLAUDE.md`. This is also mandatory in order to save tokens for future planning.
- Dependencies and sequencing (should step 2 come before step 1?)
- Scope (am I trying to do too much in one plan?). Super important! I don't want to work with a big context and have to constantly compensate for LLM aberrations.

This catches problems **before** any code is written. The critique might tell me that a function I planned to modify doesn't exist, or that my approach conflicts with how the rest of the codebase works. Critique also points out which lines from the plan it is referring to, making my life easier when comparing the Plan and Critique side by side. You need a big monitor ... hint: LG DualUp.

After a few iterations, when the critique comes back clean, I know the plan is solid and execution will go smoothly.
Most of the time, it has the right result. It matters a lot how much effort I've put into the original plan.
I also felt that "planning" in a single-dimensional file starts to be limiting ... I wonder what improvements can be done in this area.

## Why Archiving?

Plans are saved in `.planning/archive/[plan name]` so you don't lose them.
Later, when you want to revisit or re-refactor an older change, you can reference the original plan: _"Hey Claude, remember `.planning/archive/JIRA-1234/plan.md`? Now I want something else..."_

This saves backtracking and helps Claude set an immediate context.

Hint: If your archived plan folder contains old spec files (PDFs, docs), these come in handy when re-establishing context.

---

## Difference from Claude Code's Built-in Plan Mode

Claude Code has a built-in Plan Mode[1] (press Shift+Tab twice or start with `--permission-mode plan`). 
It puts Claude in a read-only state where it analyzes your codebase and proposes a plan for you.

The key difference is who writes the plan:

| Aspect              | Built-in Plan Mode              | This workflow                               |
|---------------------|---------------------------------|---------------------------------------------|
| Who writes the plan | Claude proposes it              | You write it                                |
| Iteration           | Single pass - approve or reject | Multiple critique rounds                    |
| Persistence         | Lives in conversation only      | Saved in `plan.md` files                    |
| Control             | Claude leads, you approve       | You lead, Claude reviews                    |
| Best for            | Exploring options when unsure   | Executing a change you already have in mind |


The critique step also checks if your plan can be **split** into smaller independent plans.  
Smaller plans mean smaller context windows, which leads to better execution. Plan Mode does not do this.

---

## Setting Up Your Project Standards

The critique step relies on having a CLAUDE.md file in your project root. This file tells Claude about your project conventions. It does not need to be complicated.
When Claude critiques your plan, it checks against these standards. If your plan proposes putting code in the wrong place or using the wrong naming convention, the critique will catch it.

Hint: If you're lazy, just tell Claude Code to generate a `CLAUDE.md` file for you, and then you can adjust it.

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


[0] Yes, even Waterfall is designed as an iterative process, read the [original paper](https://www.praxisframework.org/files/royce1970.pdf) by Winston Royce.  
[1] Claude's Plan Mode - [https://code.claude.com/docs/en/common-workflows#use-plan-mode-for-safe-code-analysis](https://code.claude.com/docs/en/common-workflows#use-plan-mode-for-safe-code-analysis)