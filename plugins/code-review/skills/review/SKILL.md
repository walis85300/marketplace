---
name: review
description: Reviews code for bugs, security vulnerabilities, performance issues, and style. Use when the user asks for a code review or wants feedback on their changes.
---

When reviewing code, analyze the following aspects:

## Bug Detection
- Look for off-by-one errors, null/undefined access, race conditions
- Check error handling paths — are errors caught and handled properly?
- Verify edge cases: empty inputs, boundary values, unexpected types

## Security
- Check for injection vulnerabilities (SQL, XSS, command injection)
- Look for hardcoded secrets, credentials, or API keys
- Verify input validation and sanitization at system boundaries
- Check for insecure dependencies or patterns

## Performance
- Identify unnecessary re-renders, redundant computations, or N+1 queries
- Look for missing indexes, unbounded loops, or memory leaks
- Check for large bundle sizes or unnecessary imports

## Readability
- Flag overly complex functions (high cyclomatic complexity)
- Suggest clearer naming when variable/function names are ambiguous
- Identify dead code or unused imports

Be concise. For each finding, provide:
1. **What**: the specific issue
2. **Where**: file and line reference
3. **Why**: why it matters
4. **Fix**: a concrete suggestion
