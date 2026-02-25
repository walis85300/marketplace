# Common DynamoDB Single-Table Design Patterns

Reference these patterns when designing keys and item collections.

## Pattern: Entity + Catalog Dual Write

When you need both "get by ID" and "list all":
```
PK=ENTITY#<id>  SK=ENTITY#<id>    (self-lookup by ID)
PK=ENTITIES     SK=ENTITY#<id>    (catalog for listing)
```
Trade-off: 2 writes per create, but enables both patterns without a GSI.

## Pattern: Counters as Separate Items

Atomic counters stored alongside parent:
```
PK=USER#<id>  SK=ORDER_COUNT  { count: 42 }
PK=USER#<id>  SK=BALANCE      { amount: 150.00 }
```

## Pattern: Hierarchical PK for Deep Nesting

When children of children need their own query scope:
```
PK=USER#<id>#CART#<cartId>  SK=ITEM#<itemId>
```

## Pattern: Inverted Index via GSI

Query an entity from the reverse direction:
```
Table:  PK=ORDER#<id>         SK=ORDER#<id>         (get order by ID)
GSI1:   GSI1PK=PRODUCT#<id>   GSI1SK=ORDER#<date>#<id>  (orders containing product X)
```

## Pattern: TTL-Based Expiration

Temporary items with automatic cleanup:
```
PK=SESSION#<token>  SK=SESSION#<token>  { expires: 1700000000, userId: "..." }
```

## Pattern: Sparse GSI for Status Filtering

Only items with a certain status appear in the index:
```
// Only items where GSI1PK is set appear in GSI1
// Set GSI1PK only on ACTIVE orders:
GSI1PK=ACTIVE_ORDERS  GSI1SK=ORDER#<date>#<id>
// When order is completed, remove GSI1PK attribute -> disappears from index
```

## Pattern: Sharded Static Partition Key

When a static PK like `PRODUCTS` gets too hot (>1000 WCU or >3000 RCU):
```
// Shard across N partitions
PK=PRODUCTS#0  SK=PRODUCT#<id>
PK=PRODUCTS#1  SK=PRODUCT#<id>
...
PK=PRODUCTS#9  SK=PRODUCT#<id>

// Write: hash(productId) % N -> determines shard
// Read: query all N shards in parallel, merge results
```
Trade-off: More complex read logic, but distributes load evenly.

## Pattern: Version History

Keep current + historical versions of an entity:
```
PK=DOCUMENT#<id>  SK=v0                        (current version pointer)
PK=DOCUMENT#<id>  SK=v#000001                   (version 1)
PK=DOCUMENT#<id>  SK=v#000002                   (version 2, latest)
```
- `v0` item stores `currentVersion: 2` and the latest data
- Historical versions are append-only
- Query with `begins_with(v#)` and `ScanIndexForward=false` for recent versions

## Pattern: Adjacency List (Graph Relationships)

For N:N relationships without deep nesting:
```
PK=USER#alice  SK=FOLLOWS#USER#bob     (alice follows bob)
PK=USER#bob    SK=FOLLOWER#USER#alice  (bob is followed by alice)
```
- Creating a follow = 2 writes (transaction)
- "Who does alice follow?" = Query PK=USER#alice, SK begins_with(FOLLOWS#)
- "Who follows bob?" = Query PK=USER#bob, SK begins_with(FOLLOWER#)
