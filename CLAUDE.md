# CLAUDE.md

This file provides guidance for AI assistants (like Claude) working with this repository.

## Project Overview

This is a newly initialized repository. As the project develops, update this section with:
- Project name and description
- Main purpose and functionality
- Target users/audience

## Repository Structure

```
/home/user/claude/
├── CLAUDE.md          # AI assistant guidance (this file)
└── .git/              # Git version control
```

> **Note:** This repository is currently empty. Update this structure diagram as files and directories are added.

## Technology Stack

<!-- Update this section as technologies are added -->
- **Language:** TBD
- **Framework:** TBD
- **Testing:** TBD
- **Build Tool:** TBD

## Development Workflow

### Getting Started

```bash
# Clone the repository
git clone <repository-url>
cd claude

# Install dependencies (update when package manager is chosen)
# npm install / yarn install / pip install -r requirements.txt / etc.
```

### Common Commands

<!-- Add common development commands as they are established -->
| Command | Description |
|---------|-------------|
| TBD | TBD |

### Branch Naming Conventions

- `main` or `master` - Production-ready code
- `develop` - Integration branch for features
- `feature/<name>` - New features
- `bugfix/<name>` - Bug fixes
- `claude/<session-id>` - AI assistant working branches

### Commit Message Format

Follow conventional commits:
```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

## Code Conventions

### General Guidelines

1. **Consistency** - Follow existing patterns in the codebase
2. **Clarity** - Write self-documenting code with clear naming
3. **Simplicity** - Prefer simple solutions over clever ones
4. **Testing** - Write tests for new functionality

### File Organization

<!-- Update as project structure develops -->
- Keep related files together
- Use meaningful directory names
- Follow framework conventions where applicable

## Testing

<!-- Update when testing framework is established -->
```bash
# Run tests
# npm test / pytest / etc.
```

## AI Assistant Guidelines

When working with this repository, AI assistants should:

### Before Making Changes

1. **Read first** - Always read files before modifying them
2. **Understand context** - Review related files and documentation
3. **Check conventions** - Follow existing patterns and styles
4. **Plan ahead** - Use TodoWrite for complex multi-step tasks

### When Writing Code

1. **Keep it simple** - Don't over-engineer solutions
2. **Stay focused** - Only make requested changes
3. **Avoid breaking changes** - Preserve backward compatibility
4. **Security first** - Never introduce vulnerabilities (OWASP Top 10)

### Git Operations

1. **Commit often** - Make atomic commits with clear messages
2. **Push to correct branch** - Use the designated working branch
3. **Never force push** - Especially not to main/master
4. **Stage specific files** - Avoid `git add -A` or `git add .`

### Communication

1. **Be concise** - Keep responses short and focused
2. **Reference code** - Use `file_path:line_number` format
3. **Explain decisions** - Document non-obvious choices
4. **Ask when unclear** - Don't assume, verify

## Environment Setup

<!-- Update with actual environment requirements -->
- **OS:** Linux/macOS/Windows
- **Required tools:** TBD

## Configuration Files

<!-- List important configuration files as they are added -->
| File | Purpose |
|------|---------|
| `CLAUDE.md` | AI assistant guidance |
| `.gitignore` | Git ignore patterns (to be added) |

## Troubleshooting

<!-- Add common issues and solutions as they arise -->

### Common Issues

1. **Issue:** TBD
   - **Solution:** TBD

## Additional Resources

<!-- Add links to relevant documentation -->
- Project Documentation: TBD
- API Documentation: TBD
- Contributing Guide: TBD

---

*Last updated: 2026-01-31*
*Repository status: Newly initialized*
