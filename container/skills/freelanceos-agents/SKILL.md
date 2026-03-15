---
name: freelanceos-agents
description: Trigger parallel Claude Code agent analysis on any GitHub repository via FreelanceOS API. Use when the user asks to analyze, review, or scan a repository.
allowed-tools: Bash(freelanceos-agents:*)
---

# FreelanceOS Agent Runner

Trigger parallel Claude Code agent analysis runs on any GitHub repository.

## When to use

When the user asks you to:
- Analyze or review a GitHub repository
- Run code quality, bug hunting, or architecture review on a repo
- Scan a codebase for issues

## How to trigger

```bash
# Create an agent run via the FreelanceOS API
curl -s -X POST "http://freelanceos-backend:8000/api/agents/webhook/nanoclaw" \
  -H "Content-Type: application/json" \
  -H "X-API-Key: ${FREELANCEOS_API_KEY}" \
  -d '{
    "repo_url": "https://github.com/user/repo",
    "tasks": [
      {"task_type": "bug_investigation"},
      {"task_type": "architecture_review"},
      {"task_type": "code_quality"}
    ],
    "sender": "USER_NAME",
    "chat_jid": "CHAT_JID"
  }'
```

## Available task types

| Type | Description |
|------|-------------|
| `bug_investigation` | Find bugs, race conditions, security vulnerabilities |
| `architecture_review` | Analyze dependencies, patterns, scalability |
| `code_quality` | Duplication, naming, dead code, complexity |
| `backup_check` | Disaster recovery, data persistence |
| `custom` | Provide your own prompt (add `"prompt": "..."` to the task) |

## Check run status

```bash
# Get run details
curl -s "http://freelanceos-backend:8000/api/agents/runs/{run_id}"
```

## Response format

The API returns the run with all tasks and their statuses. Results from completed tasks contain markdown-formatted analysis reports.

## Notes

- Up to 8 agents run in parallel per run
- Each agent gets the full repository mounted read-only
- Results are persisted in PostgreSQL for later review
- Completion notifications are sent back via IPC automatically
