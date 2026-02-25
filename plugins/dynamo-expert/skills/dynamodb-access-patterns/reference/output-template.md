# Access Patterns Output Template

The generated file MUST follow this exact structure:

```markdown
# DynamoDB Access Patterns

> Generated on: [date]
> Application: [name]
> Status: DRAFT | REVIEWED | APPROVED

## Domain Overview

[1-3 sentence description of the application]

## Entities

### [EntityName]

- **Description**: [what it is]
- **Attributes**:
  - `attributeName` (type) - description
  - `attributeName` (type, optional) - description
- **Relationships**: [how it relates to other entities]
- **Cardinality**: [expected volume]
- **Lifecycle**: [create/update/delete/expire behavior]

[Repeat for each entity]

## Access Patterns

### Reads

| ID | Entity | Description | Input Parameters | Result | Sort | Paginated | Frequency | Consistency | Multi-Entity |
|----|--------|-------------|------------------|--------|------|-----------|-----------|-------------|--------------|
| AP-001 | User | Get user by userId | userId | Single | - | No | Hot | Eventual | No |
| AP-002 | Order | List orders for a user | userId | Collection | createdAt DESC | Yes | Warm | Eventual | No |
| AP-003 | User, Order | Get user profile with recent orders | userId | Collection | orders by createdAt DESC | No | Hot | Eventual | Yes |

### Writes

| ID | Entity | Description | Input Parameters | Items Written | Transaction | Conditions | Frequency |
|----|--------|-------------|------------------|---------------|-------------|------------|-----------|
| AP-010 | Order | Create a new order | userId, items, total | Order + OrderCount update | Yes | User must exist | Warm |
| AP-011 | Order | Update order status | orderId, newStatus | 1 (Order) | No | Order must exist | Warm |

### Deletes

| ID | Entity | Description | Input Parameters | Items Deleted | Transaction | Conditions | Frequency |
|----|--------|-------------|------------------|---------------|-------------|------------|-----------|
| AP-020 | Order | Cancel an order | orderId | Order + OrderCount update | Yes | Status must be PENDING | Cold |

## Item Collection Candidates

These access patterns benefit from grouping entities into the same item collection
(same partition key) so they can be fetched in a single DynamoDB Query:

| Group | Entities | Access Patterns | Rationale |
|-------|----------|-----------------|-----------|
| User Profile | User, Order, Balance | AP-003, AP-005 | Profile page needs user + recent orders + balance |

## Notes and Decisions

- [Any assumptions, constraints, or decisions made during this process]
- [Known future access patterns that aren't needed yet]
- [Patterns explicitly excluded (e.g., analytics done in a separate system)]
```
