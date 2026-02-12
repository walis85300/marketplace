---
description: Create a well-named git branch from a description
argument-hint: [description]
disable-model-invocation: true
---

Create a new git branch based on the description: "$ARGUMENTS"

Follow this naming convention:
- `feat/short-description` for features
- `fix/short-description` for bug fixes
- `docs/short-description` for documentation
- `refactor/short-description` for refactoring
- `chore/short-description` for maintenance

Rules:
- Use kebab-case
- Keep it under 50 characters
- Make it descriptive but concise

Create the branch and switch to it.
