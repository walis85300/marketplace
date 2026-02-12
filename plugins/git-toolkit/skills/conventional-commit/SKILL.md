---
name: conventional-commit
description: Generates conventional commit messages based on staged changes. Use when the user wants to commit with a well-formatted message following the Conventional Commits specification.
---

Analyze the current staged changes (`git diff --cached`) and generate a commit message following the Conventional Commits specification:

## Format
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Types
- **feat**: A new feature
- **fix**: A bug fix
- **docs**: Documentation only changes
- **style**: Formatting, missing semi-colons, etc (no code change)
- **refactor**: Code change that neither fixes a bug nor adds a feature
- **perf**: A code change that improves performance
- **test**: Adding or correcting tests
- **build**: Changes to build system or external dependencies
- **ci**: Changes to CI configuration
- **chore**: Other changes that don't modify src or test files

## Rules
- Keep the description under 72 characters
- Use imperative mood ("add" not "added" or "adds")
- Don't capitalize the first letter of the description
- No period at the end of the description
- Add a body if the "why" isn't obvious from the description
- Reference issue numbers in the footer when applicable

Present the suggested message and ask for confirmation before committing.
