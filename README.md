## Welcome to WTG 👋

We are a community-focused gaming platform that allows users to provision and manage private game servers on demand, directly from our Discord server.

Learn more at [wethegamers.org](https://wethegamers.org) | [Join our Discord](https://discord.gg/wtg)

---

## For Developers

This repository contains shared configurations for all WTG repositories.

### VS Code Copilot Prompt Files

Clone this repo to your workspace root as `.github/` to get VS Code Copilot prompt files:

```bash
# In your WTG workspace root
git clone git@github.com:wethegamers/.github.git .github
```

Then use `/prompt:` in Copilot Chat to access:

| Prompt | Description |
|--------|-------------|
| `/prompt:doc-writer` | Generate technical documentation |
| `/prompt:software-engineer` | Production-ready optimized code |
| `/prompt:code-architect` | Step-by-step coding workflow |
| `/prompt:troubleshooting` | Debug WTG K8s platform issues |
| `/prompt:infrastructure` | Create K8s/ArgoCD resources |
| `/prompt:code-review` | Code review with severity levels |
| `/prompt:security` | Security review checklist |
| `/prompt:adr` | Architecture Decision Records |
| `/prompt:commit-message` | Conventional commit messages |
| `/prompt:pr-description` | Pull request descriptions |
| `/prompt:runbook` | Operational runbooks |
| `/prompt:refactor` | Code refactoring guidance |
| `/prompt:documentation` | Technical documentation |
| `/prompt:prompt-maker` | Create new VS Code prompts |

### Master Copilot Instructions

The `copilot-instructions.md` file provides platform-wide context for GitHub Copilot across all WTG repositories.

### Structure

```
.github/
├── copilot-instructions.md   # Master AI agent instructions
├── PULL_REQUEST_TEMPLATE.md  # Default PR template for all repos
├── ISSUE_TEMPLATE/           # Default issue templates
│   ├── bug_report.md
│   ├── feature_request.md
│   ├── question.md
│   └── config.yml
├── prompts/                  # VS Code Copilot prompt files
│   ├── doc-writer.prompt.md
│   ├── software-engineer.prompt.md
│   └── ...
└── profile/                  # GitHub organization profile
    └── README.md
```

### Organization-Wide Templates

This repo provides **default templates** for all WTG repositories:

- **Issue Templates**: Bug reports, feature requests, questions
- **PR Template**: Standard pull request format

Individual repos can override these by adding their own templates in their `.github/` folder.
