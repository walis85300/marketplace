---
description: Generate API documentation for endpoints or functions
disable-model-invocation: true
---

Generate API documentation for: $ARGUMENTS

If $ARGUMENTS is a file path, document all exported functions/endpoints in that file.
If $ARGUMENTS is a description, find the relevant code and document it.

For each endpoint/function, document:
- **Signature**: function name, parameters with types, return type
- **Description**: what it does in one sentence
- **Parameters**: table with name, type, required/optional, description
- **Returns**: what the function returns and when
- **Example**: a minimal usage example
- **Errors**: what errors can be thrown and when

Use the project's existing documentation style if one exists. Otherwise, use JSDoc-style for JS/TS or docstring-style for Python.
