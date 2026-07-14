# Contributing to NovaCart Platform

Thank you for your interest in contributing to the NovaCart Platform project.

NovaCart Platform is an enterprise-grade cloud-native e-commerce platform built as a real-world software engineering project. This repository follows professional development practices based on Agile, Scrum, GitHub Flow, and DevOps principles.

Please read the following guidelines before contributing.

---

# Development Workflow

All work must be tracked in Jira before implementation begins.

The standard workflow is:

1. Select a Jira work item.
2. Move the work item to **In Progress**.
3. Create a dedicated Git branch.
4. Implement the required changes.
5. Test the implementation.
6. Commit the changes using the project's commit message convention.
7. Push the branch to GitHub.
8. Open a Pull Request.
9. Complete the code review.
10. Merge into the target branch.
11. Move the Jira work item to **Done**.

---

# Branch Naming Convention

Each Git branch should reference the Jira work item.

Examples:

```text
docs/NCP-4-project-documentation
feature/NCP-5-configure-monorepo
feature/NCP-7-nextjs-frontend
feature/NCP-9-nestjs-backend
feature/NCP-10-postgresql
feature/NCP-11-dynamodb-local
fix/NCP-20-health-endpoint
chore/NCP-30-update-dependencies
```

Recommended prefixes:

| Prefix | Purpose |
|---------|---------|
| feature | New functionality |
| fix | Bug fixes |
| docs | Documentation |
| refactor | Code restructuring |
| test | Tests |
| chore | Maintenance tasks |
| ci | CI/CD changes |
| build | Build system changes |

---

# Commit Message Convention

NovaCart Platform follows the Conventional Commits specification.

Format:

```text
<type>(<scope>): <description>
```

Examples:

```text
docs(project): add initial project documentation

feat(frontend): initialize Next.js application

feat(backend): create health endpoint

fix(database): correct PostgreSQL connection

test(api): add health endpoint tests

chore(deps): update development dependencies

ci(github): add GitHub Actions workflow
```

Common commit types:

- feat
- fix
- docs
- test
- refactor
- build
- ci
- chore

Commit messages should:

- Be written in English.
- Use the imperative mood.
- Clearly describe the change.
- Be concise.

---

# Pull Request Guidelines

Each Pull Request should contain only one logical change.

Before submitting a Pull Request, ensure that:

- The Jira work item has been completed.
- Acceptance Criteria are satisfied.
- Documentation has been updated.
- Code formatting is correct.
- Linting passes.
- Unit tests pass.
- No unnecessary files are included.

Example Pull Request title:

```text
NCP-4: Add initial project documentation
```

Example Pull Request description:

```markdown
## Summary

Adds the initial documentation required for the NovaCart Platform.

## Jira

NCP-4

## Changes

- Updated README.md
- Added CONTRIBUTING.md
- Added CODE_OF_CONDUCT.md
- Added SECURITY.md
- Added CHANGELOG.md

## Testing

- Markdown validation completed.
- Internal links verified.
```

---

# Code Style

The project follows these coding standards:

- TypeScript best practices
- ESLint
- Prettier
- Consistent folder structure
- Meaningful variable names
- Small, reusable components
- Clean Architecture principles where applicable

---

# Documentation

Documentation is considered part of the implementation.

Whenever a feature is introduced:

- Update README if necessary.
- Update architecture documentation if affected.
- Document configuration changes.
- Document environment variables.

---

# Testing

Every new feature should include appropriate testing.

Testing may include:

- Unit Tests
- Integration Tests
- API Tests
- End-to-End Tests

Code should not be merged if automated tests fail.

---

# Security

Never commit:

- Passwords
- API Keys
- AWS Credentials
- Secrets
- Certificates
- Private Keys
- Environment files containing sensitive information

Use environment variables instead.

Refer to:

```text
SECURITY.md
```

for responsible vulnerability reporting.

---

# Reporting Bugs

Bugs should be tracked using Jira.

A bug report should include:

- Description
- Steps to reproduce
- Expected behavior
- Actual behavior
- Environment
- Logs (if available)

---

# Code Review Checklist

Before requesting review, verify that:

- Code builds successfully.
- Tests pass.
- Linting passes.
- Documentation is updated.
- Acceptance Criteria are completed.
- No debug code remains.
- No sensitive information is committed.

---

# Development Principles

Contributors should aim to:

- Write clean and maintainable code.
- Keep Pull Requests small.
- Prefer readability over cleverness.
- Avoid duplicated code.
- Follow SOLID principles where applicable.
- Maintain consistent project structure.

---

# Questions

If you have questions regarding development practices, architecture, or project standards, please open a discussion or contact the project maintainers.

---

Thank you for contributing to NovaCart Platform.

Together we build reliable, scalable, and production-ready software.