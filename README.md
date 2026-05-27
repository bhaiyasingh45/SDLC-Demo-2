# E2E SDLC with GitHub Copilot — Workshop

> **Audience**: Developers, Tech Leads, Architects  
> **Total Duration**: ~41 min mandatory + ~30 min optional (see two-track map below)  
> **Pre-requisites**: VS Code with GitHub Copilot Chat extension, Node/Python/Java runtime, Git CLI, GitHub Copilot access, GitHub MCP, Mermaid extension for VS code.

---

## What You Will Build

An **Intelligent Task Management System (ITMS)** — a realistic business application that covers every phase of the Software Development Life Cycle, all guided by GitHub Copilot.

The system enables teams to create, assign, and track tasks, manage task dependencies, monitor project progress, and identify bottlenecks — all key pain points for software teams working across multiple members and workstreams.

**Core capabilities from [`requirement.md`](requirement.md):**

| Capability | Description |
|---|---|
| **Task Creation** | Create tasks with ID, title, priority, status, assignee, and due date |
| **Task Assignment** | Assign or reassign tasks to team members |
| **Dependency Management** | Link tasks with dependencies; auto-mark blocked tasks |
| **Status Tracking** | Track To Do / In Progress / Blocked / Completed with history |
| **Filtering & Listing** | Query tasks by status, priority, assignee, or due date |
| **Progress Summary** | Dashboard showing total, completed, in-progress, and blocked counts |

> The requirement is already in [`requirement.md`](requirement.md). You do **not** need to write the application yourself — Copilot does the heavy lifting. Your job is to learn how to **direct Copilot effectively** through each SDLC phase.

---

## Prerequisites

Participants should have the following set up before starting the workshop:

- [VS Code](https://code.visualstudio.com/download) with GitHub Copilot Chat extension
- Node/Python/Java runtime/.net SDK (depending on your language choice)
- GitHub Copilot access
- [Git CLI](https://git-scm.com/install/)
- [GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli)
- [GitHub MCP](https://github.com/mcp/github/github-mcp-server)
- [Mermaid extension](https://marketplace.visualstudio.com/items?itemName=MermaidChart.vscode-mermaid-chart) for VS code

## Workshop Map

> **Two-Track Design**: Complete the **Mandatory Track** first (~80 min) — it covers every SDLC phase end-to-end with no gaps. Then pick any **Optional** exercises based on time and interest. Optional exercises are self-contained; no mandatory exercise depends on them.

---

### 🔵 Mandatory Track (~80 minutes)

These exercises form the core SDLC journey. Each builds directly on the previous one and introduces a distinct GitHub Copilot feature.

| # | Exercise | Copilot Feature | Duration |
|---|----------|----------------|----------|
| 01 | [Custom Instructions](workshop/exercise-01-custom-instructions.md) | Workspace Instructions (`.github/copilot-instructions.md`) | 3 min |
| 02 | [Setup & Custom Agents](workshop/exercise-02-setup-agents.md) | Custom Agents + Agent Orchestration (`.agent.md`) | 8 min |
| 03 | [Generate BRD](workshop/exercise-03-brd.md) _(skip if Ex 02 orchestrator ran)_ | BRD Custom Agent | 4 min |
| 04 | [Generate TSD](workshop/exercise-04-tsd.md) _(skip if Ex 02 orchestrator ran)_ | TSD Custom Agent | 4 min |
| 05 | [Generate FRD](workshop/exercise-05-frd.md) _(skip if Ex 02 orchestrator ran)_ | FRD Custom Agent | 4 min |
| 06 | [Plan Mode – Implementation Plan](workshop/exercise-06-plan-mode.md) | Plan Mode | 4 min |
| 07 | [Create Implementation Prompt File](workshop/exercise-07-implementation-prompt.md) | Prompt Files | 3 min |
| 08 | [Build APIs with Local Agent](workshop/exercise-08-api-local-agent.md) | Local (Default) Agent | 5 min |
| 09 | [Design & Scaffold the Task Management UI](workshop/exercise-09-ui-design.md) | Local Agent — UI Scaffolding | 5 min |
| 10 | [Complete ITMS: CLI with /plan & /fleet + Coding Agent](workshop/exercise-10-cli-fleet-and-coding-agent.md) | Copilot CLI (`/plan`, `/fleet`) + Coding Agent via MCP | 30 min |
| 11 | [Unit & Functional Tests](workshop/exercise-11-testing.md) | Local Agent + Prompt File | 5 min |
| 12 | [Security Review](workshop/exercise-12-security.md) | Security Prompt File | 4 min |
| 13 | [Build & Debug](workshop/exercise-13-build-debug.md) | Local Agent + Terminal | 5 min |

> **Why these 13?** They cover every key Copilot feature — Custom Agents → Agent Orchestration → Prompt Files → Plan Mode → Local Agent → UI Scaffolding → CLI (`/plan` & `/fleet`) → Coding Agent → Testing → Security — mirroring a real SDLC from requirements to a running, tested application.

> **Time-saving tip**: Exercise 02 includes an **SDLC Docs Orchestrator** agent that automatically chains BRD → TSD → FRD generation in one run. If you use it, Exercises 03, 04, and 05 are already complete — skip directly to Exercise 06, saving ~12 minutes. The TSD it generates will automatically use the language you chose in Exercise 01.

---

### 🟡 Optional Track (~30 minutes — pick any, in any order)

These exercises are self-contained. No mandatory exercise depends on them. Complete them if time allows, or revisit them after the workshop.

| # | Exercise | Copilot Feature | Duration | Best After |
|---|----------|----------------|----------|------------|
| 14 | [Create GitHub Issues via MCP](workshop/exercise-14-github-issues.md) | GitHub MCP + Prompt File | 5 min | Ex 07 |
| 15 | [Context Map Skill](workshop/exercise-15-context-map.md) | Skills (`SKILL.md`) | 4 min | Ex 08 |
| 16 | [Database & SQL / PL/SQL](workshop/exercise-16-database-sql.md) | Local Agent + Instructions | 5 min | Ex 15 |
| 17 | [IaC & CI/CD](workshop/exercise-17-iac-cicd.md) | Custom Agent + Prompt File | 5 min | Ex 13 |

> **Note on Ex 14**: Requires a GitHub repository and a Personal Access Token with `repo` and `issues` scopes. Skip if GitHub MCP is not pre-configured in your environment.
> **Note on Ex 16**: References the context map from Ex 15. If you skip Ex 15, remove the `#context-map.md` reference from the prompts before sending.

---
> The workshop has been tested with the following AI models on GitHub Copilot: `Claude Sonnet 4.6`,`GPT-5.3-codex`. Results may vary with different models. If you encounter issues, try switching to one of these models in your Copilot settings.

> **Note:** Complete **Mandatory** exercises in order (each artifact feeds the next). **Optional** exercises can be done in any order after their recommended prerequisite, or revisited after the workshop.

---

## Key GitHub Copilot Features Covered

| Feature | Description |
|---------|-------------|
| **Custom Agents** | Scoped AI personas with tool restrictions and domain prompts |
| **Plan Mode** | Pre-execution planning before Copilot takes action |
| **Local Agent** | Default interactive coding agent in VS Code |
| **Copilot CLI** | `/plan` and `/fleet` commands for parallel feature implementation |
| **Coding Agent** | Asynchronous agent that opens PRs from GitHub Issues |
| **Prompt Files** | Reusable, parameterized prompt templates (`.prompt.md`) |
| **Instructions** | Always-on context injected into every Copilot conversation |
| **Skills** | On-demand workflow bundles loaded from `SKILL.md` |
| **GitHub MCP** | Model Context Protocol server for GitHub integration |

---

## Getting Started

1. Open the cloned repository in VS Code
1. Ensure **GitHub Copilot** and **GitHub Copilot Chat** extensions are installed and signed in
1. Open the Copilot Chat panel (`Ctrl+Alt+I`)
1. Start with [Exercise 01](workshop/exercise-01-custom-instructions.md)
1. Configure your GitHub token for MCP access by setting `GITHUB_TOKEN` in your environment
---

> **Instructor Note**: Each exercise has a `> Instructor Guide` section visible only in the markdown source. Exercises are designed so attendees never need to copy code — they copy **prompts** and let Copilot generate the output.
