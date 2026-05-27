# Exercise 17 — Infrastructure as Code & CI/CD Pipelines

**Duration**: 5 minutes  
**Copilot Feature**: DevOps Custom Agent + Prompt Files  
**Goal**: Use the DevOps agent to generate Docker, Terraform/Bicep, and GitHub Actions pipelines for the ITMS application.

---

> ---
> 🟡 **OPTIONAL EXERCISE**
>
> This exercise is **not required** for the mandatory track. It is the infrastructure and deployment capstone — ideal if you have extra time after Exercise 15 or want to explore IaC and CI/CD automation with Copilot after the workshop.
>
> **Best after**: Exercise 15 &nbsp;|&nbsp; This is the final exercise in the workshop.
> ---


## Background

The final phase of SDLC is deployment infrastructure. The **DevOps Agent** (`devops.agent.md`) you created in Exercise 02's setup is a specialized agent that reads the TSD's deployment architecture and generates production-ready IaC and CI/CD configurations.

---

## Step 1 — Switch to the DevOps Agent

1. In Copilot Chat, click the agent selector
2. Select **DevOps & IaC Agent**

---

## Step 2 — Create Docker Configuration

Send this prompt:

```
Read doc/tsd.md and src/. Create infra/docker/Dockerfile, infra/docker/docker-compose.yml, and .dockerignore. Requirements: multi-stage minimal runtime, non-root user, production artifacts only, correct port, /api/v1/health HEALTHCHECK, api/db/redis services, migrations mounted, .env variables, health-checked depends_on, and successful image build.
```

---

## Step 3 — Create Infrastructure as Code (Azure)

Send this prompt:

```
Create Azure Bicep under infra/bicep matching doc/tsd.md: main, modules for Container App, PostgreSQL Flexible Server, Key Vault, ACR, monitoring, dev/prod parameters, and README. Use managed identity for Key Vault/ACR, private DB access, tags environment/project=ITMS/owner, and no secrets in env vars.
```

> **If preferring Terraform**: Replace "Bicep" with "Terraform" in the prompt above.

---

## Step 4 — Create CI/CD Pipelines

Send this prompt:

```
Create .github/workflows/ci.yml, cd-staging.yml, and cd-production.yml. CI on PR: setup/cache, install, lint, unit coverage >=80%, Docker build, Trivy CRITICAL gate, Semgrep HIGH gate, PR comment. Staging on main: build/push ACR, migrate, deploy with OIDC, smoke + integration tests, Teams notify. Production on v* tag: approval, versioned image, migrations, blue-green/gradual traffic, health polling, rollback, GitHub Release. Use GitHub OIDC and environment secrets.
```

---

## Step 5 — Create the infra/README.md

Send:

```
Create infra/README.md covering architecture, prerequisites, first-time Azure/GitHub setup, day-to-day CI/CD, rollback, Key Vault/env variable reference, monitoring/alerts, and cost estimate for 500 concurrent users.
```

---

## Verify

Open the created files and check:

- [ ] Dockerfile uses multi-stage build and non-root user
- [ ] `docker-compose.yml` starts both app and database
- [ ] Bicep/Terraform templates are parameterized (no hardcoded values)
- [ ] CI workflow has test + security scan gates
- [ ] CD staging runs integration tests before marking deploy as success
- [ ] CD production requires manual approval
- [ ] No secrets hardcoded in any YAML, Dockerfile, or IaC file

---

## Workshop Complete! 🎉

You have completed the full E2E SDLC using GitHub Copilot:

| Phase | Output |
|-------|--------|
| Planning | `doc/brd.md`, `doc/tsd.md`, `doc/frd.md` |
| Project Management | GitHub Issues via MCP |
| Development | `src/` — REST APIs with auth, task management, dependencies, and reporting |
| Database | `db/migrations/`, `db/procedures/`, stored procedures with PL/SQL |
| Testing | `tests/` — unit + integration tests from FRD acceptance criteria |
| Security | OWASP review report, vulnerability fixes |
| Infrastructure | `infra/` — Docker, Bicep/Terraform, GitHub Actions CI/CD |

---

## What to Explore Next

| Topic | How |
|-------|-----|
| Claude or GPT-4o as base model | Change model in Copilot Chat settings |
| Custom skill for API docs | Create `.github/skills/api-docs/SKILL.md` |
| Automated PR review agent | Create `.github/agents/code-review.agent.md` |
| Prompt for performance profiling | Add `.github/prompts/performance-review.prompt.md` |
| Pre-commit security hook | Create `.github/hooks/pre-commit.json` |

---

**Return to**: [Workshop Overview (README.md)](../README.md)
