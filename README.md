# Agent Skills

A collection of skills for AI coding agents.

## What are Agent Skills?

Agent Skills are specialized capabilities that extend the functionality of AI coding agents like Claude Code. They enable agents to perform complex, domain-specific tasks through well-defined prompts and workflows.

Learn more at [agentskills.io](https://agentskills.io/).

## Available Skills

### assess-agent-guidance-files

A thoughtful skill to evaluate a repository’s agent readiness. It scans the repo’s AGENTS.md/CLAUDE.md files, along with any referenced guidance files, and provides an assessment across key areas, with practical recommendations for improvement.

It also creates or updates an `AGENTIC_READINESS_ASSESSMENT.md` artifact in the assessed repository root, using the required rating table and recommendation structure, and produces an overall repository rating for quick evaluation.

**Installation:**

```bash
npx skills add https://github.com/micheleangioni/agent-skills --skill assess-agent-guidance-files
```

### codebase-auditor

A comprehensive skill for auditing codebases, identifying potential issues, security vulnerabilities, and code quality concerns.

**Installation:**

```bash
npx skills add https://github.com/micheleangioni/agent-skills --skill codebase-auditor
```

### create-nextjs-ddd-auth-db

A skill to create a complete Next.js application with DDD structure, API endpoints, frontend pages, database integration, Google auth via better-auth, Jest tests, and ESLint. The last available version will be used for all packages.

**Installation:**

```bash
npx skills add https://github.com/micheleangioni/agent-skills --skill create-nextjs-ddd-auth-db
```

## Removal

To remove an installed skill, use:

```bash
npx skills remove <skill-name>
```

## License

MIT
