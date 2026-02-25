# Language Conventions for Query Interfaces

Ask the user which language they intend to implement in. This affects type syntax but NOT the design.

## Supported Language Conventions

| Language | Types | Interface Keyword | Optional | Collection |
|---|---|---|---|---|
| TypeScript | `string`, `number`, `boolean` | `interface` | `field?: type` | `Array<T>` |
| Python | `str`, `int`, `float`, `bool` | `class` (dataclass/Protocol) | `Optional[type]` | `list[T]` |
| Go | `string`, `int`, `float64`, `bool` | `type ... interface` | `*type` | `[]T` |
| Java/Kotlin | `String`, `int`, `long`, `boolean` | `interface` | `@Nullable` | `List<T>` |
| Ruby | No static types | `class` / module | `nil` | `Array` |
| Rust | `String`, `i32`, `f64`, `bool` | `trait` | `Option<T>` | `Vec<T>` |
| Generic | `string`, `number`, `boolean` | `interface` | `optional` | `list<T>` |

If the user has no preference, use **Generic** pseudocode.

## Method Naming Convention

| DynamoDB Operation | Method Prefix | Example |
|---|---|---|
| Query (single result) | `get` | `getOrderById` |
| Query (collection) | `list` | `listUserOrders` |
| Query (paginated collection) | `list...Paginated` | `listUserOrdersPaginated` |
| BatchGetItem | `batchGet` | `batchGetOrders` |
| PutItem / TransactWriteItems (create) | `create` | `createOrder` |
| UpdateItem / TransactWriteItems (update) | `update` | `updateOrderStatus` |
| DeleteItem / TransactWriteItems (delete) | `delete` | `deleteOrder` |

## Cursor-Based Pagination

The cursor is an opaque string that encodes DynamoDB's `LastEvaluatedKey`. The repository implementation should:

1. Encode `LastEvaluatedKey` to a base64 string when returning a cursor
2. Decode the base64 string back to `LastEvaluatedKey` when receiving a cursor
3. Never expose raw DynamoDB key structure to callers

```
interface PaginatedResult<T> {
  items: list<T>
  nextCursor: optional string   // Opaque token. null/absent = no more pages.
}
```
