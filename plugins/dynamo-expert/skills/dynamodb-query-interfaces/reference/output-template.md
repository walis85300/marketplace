# Query Interfaces Output Template

The generated file MUST follow this exact structure:

```markdown
# DynamoDB Query Interfaces

> Generated on: [date]
> Application: [name]
> Language: [target language or Generic]
> Input: [path to table design file]
> Status: DRAFT | REVIEWED | APPROVED

## Entity Models

### [EntityName]

[interface definition with comments]

[Repeat for each entity]

## Key Builders

### [EntityName] Keys

[key builder functions with AP references]

[Repeat for each entity]

## Repository Interfaces

### [EntityName]Repository

[interface with all methods, each referencing its AP ID]

[Repeat for each repository]

## Paginated Result Type

[PaginatedResult<T> definition]

## Transaction Definitions

### [TransactionName]

[transaction structure for each multi-item write]

## Traceability Matrix

| AP ID | Description | Repository Method | Status |
|-------|-------------|-------------------|--------|
| AP-XXX | ... | Repository.method | Covered |

## Implementation Notes

- [Any implementation guidance specific to the chosen language]
- [Recommended SDK usage patterns]
- [Error handling conventions]
- [Retry and idempotency considerations]
```
