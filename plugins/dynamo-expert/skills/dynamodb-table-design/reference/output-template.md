# Table Design Output Template

The generated file MUST follow this exact structure:

```markdown
# DynamoDB Table Design

> Generated on: [date]
> Application: [name]
> Input: [path to access patterns file]
> Status: DRAFT | REVIEWED | APPROVED

## Table Configuration

| Property | Value |
|---|---|
| Table Name | [user decides, or TBD] |
| Partition Key | `pk` (String) |
| Sort Key | `sk` (String) |
| Billing Mode | PAY_PER_REQUEST / PROVISIONED |
| TTL Attribute | `expires` (if applicable) |

## Global Secondary Indexes

| GSI Name | Partition Key | Sort Key | Projection | Purpose |
|----------|--------------|----------|------------|---------|
| GSI1 | `GSI1PK` (S) | `GSI1SK` (S) | ALL | [list of uses] |

## Entity Key Design

### [EntityName]

- **Description**: [from access patterns doc]
- **PK**: `PREFIX#<identifier>`
- **SK**: `PREFIX#<identifier>`
- **GSI1PK**: [if applicable]
- **GSI1SK**: [if applicable]
- **Attributes**: [list all non-key attributes with types]
- **TTL**: [if applicable]

Example item:
```json
{
  "pk": "USER#550e8400-e29b",
  "sk": "USER#550e8400-e29b",
  "id": "550e8400-e29b",
  "email": "jane@example.com",
  "name": "Jane Doe",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

[Repeat for each entity]

## Item Collections

| Collection PK | Entity Types | Purpose |
|---|---|---|
| `USER#<userId>` | User, Order, Balance, Preferences | User profile + related data |
| `PRODUCTS` | Product (catalog entry) | Global product listing |

## Access Pattern Mapping

### Reads

| AP ID | Description | Operation | Table/GSI | PK | SK Condition | SK Value | Params |
|-------|-------------|-----------|-----------|----|--------------|---------|---------|
| AP-001 | Get user by ID | Query | Table | `USER#<userId>` | = | `USER#<userId>` | Limit=1 |
| AP-002 | List user orders | Query | Table | `USER#<userId>` | begins_with | `ORDER#` | ScanIndexForward=false, paginated |

### Writes

| AP ID | Description | Operation | Items | Conditions |
|-------|-------------|-----------|-------|------------|
| AP-010 | Create order | TransactWriteItems | 1. Put ORDER in user collection 2. Put ORDER self-record 3. Update ORDER_COUNT | Item 1,2: attribute_not_exists(pk) |

### Deletes

| AP ID | Description | Operation | Items | Conditions |
|-------|-------------|-----------|-------|------------|
| AP-020 | Cancel order | TransactWriteItems | 1. Delete ORDER from user collection 2. Delete ORDER self-record 3. Update ORDER_COUNT (-1) | Status = PENDING |

## Design Decisions

- [Document WHY you chose certain key structures over alternatives]
- [Document trade-offs made]
- [Document patterns deferred to application-level filtering]

## Validation

- [x/] All [N] access patterns mapped
- [x/] [N] GSIs defined
- [x/] No hot partitions identified
- [x/] No Scan operations required
```
