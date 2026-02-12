---
description: Generate a README.md for the current project
disable-model-invocation: true
---

Analyze the current project and generate a README.md file. Look at:

1. `package.json`, `pyproject.toml`, `Cargo.toml`, or equivalent for project metadata
2. The source code structure to understand what the project does
3. Any existing documentation or comments

Generate a README with these sections:
- **Title and description**: What this project is
- **Quick start**: How to install and run it (exact commands)
- **Usage**: Key features with brief examples
- **Project structure**: Only the top-level directories with one-line descriptions
- **Contributing**: How to set up a dev environment

Keep it concise. Don't add badges, shields, or decorative elements unless asked.
Present the generated README and ask for confirmation before writing the file.
