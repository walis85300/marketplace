---
description: Quick review of staged git changes
disable-model-invocation: true
---

Review my current staged git changes (`git diff --cached`). Focus only on critical issues:
- Bugs that will cause runtime errors
- Security vulnerabilities
- Broken logic

Skip style nits and minor suggestions. Be brief and actionable.
If there are no staged changes, check unstaged changes with `git diff` instead.
