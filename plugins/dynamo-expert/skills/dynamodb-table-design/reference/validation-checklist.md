# Table Design Validation Checklist

Run through this checklist with the user before finalizing the design.

## Coverage Check

- [ ] Every read pattern (AP-0XX) maps to exactly one DynamoDB operation
- [ ] Every write pattern specifies all items written
- [ ] Every multi-entity read is achievable in a single Query call
- [ ] No access pattern requires a Scan

## Key Design Check

- [ ] Every PK+SK combination is unique per item
- [ ] Sort keys enable the required sort orders
- [ ] Composite sort keys use consistent padding
- [ ] No single PK will become a hot partition (>1000 WCU or >3000 RCU)

## GSI Check

- [ ] Total GSIs <= 20
- [ ] Each GSI is justified by at least one access pattern
- [ ] GSI key attributes are populated on all items that need to appear in the index
- [ ] Projection type is explicitly chosen

## Capacity Check

- [ ] No item exceeds 400KB
- [ ] No item collection exceeds 10GB (for tables with LSIs; no limit without LSIs)
- [ ] Transactions involve <= 100 items per call
- [ ] BatchGetItem requests use <= 100 keys per call

## TTL Check

- [ ] Items that expire have a TTL attribute (Unix epoch in seconds)
- [ ] TTL attribute name is consistent across all expiring entities

## Key Convention Check

- [ ] `#` separator used consistently between key segments
- [ ] Entity prefixes are UPPERCASE (e.g., `USER#`, `ORDER#`)
- [ ] No reserved words used as attribute names without escaping
- [ ] Generic key names (`pk`, `sk`) used for table keys (not domain-specific names)
