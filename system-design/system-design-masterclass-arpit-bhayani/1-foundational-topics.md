# Foundational Topics in System Design

## Online-Offline Indicator

### Approaching System Design
- Decide your core and build your system around it.
- Core is use-case specific and it could be database, communication protocol, must have component (CDN, cache)

### Another good approch
- Start with a Day 0 Architecture
- See how each component would behave at scale
- Mitigate and re-architect
- Repeat

### Points to remember
- Understand the _core property_ and _access pattern_
- Affinity towards a tech comes second
- Build an _intuition_ towards building systems

**Storage**: user (int) -> online/offline (boolean)
**Access**: Key Value

### Querying the database
**Pushed based model**: Users push their status periodically. Every user periodically sends "heartbeat" to the service.

```
POST /heartbeat -> the authenticated user will be marked as alive
```
- When is user offline?
When we did not receive "heartbeat" for long enough.

This would mean we would need to store the "time you received last heartbeat" in the database.

Pulse Table:
| user_id | last_hb |
|---------|--------|
| u1 | 1000 |
| u2 | 1050 |
| u3 | 1060 |

- When we receive heartbeat
```sql
UPDATE pulse
SET last_hb = NOW()
WHERE user_id = u1
```
- Get Status API
```
GET /status/<user_id>

if not entry in the db for the user -> offline
if entry and entry.last_hb < Now() - 30 sec -> offline
online
```

### Estimating the scale
- Each DB entry has 2 columns - user_id (int -> 4 bytes) and last_hb (int -> 4 bytes).
- Thus, each entry would have size of 8 bytes.
- Total storage required for 1 billion users -> 8GB

This information can help us select our database.

- One DB instance can definitely hold the data (since it is only 8GB)
- However, handling the load would not be possible for a single DB instance.
- Hence, we partition the data and each node handles a subset of the requests.

We can also optimize storage further by deleting the entries of inactive users.

- Approach 1: Write a cron job that deletes expired entries.
	- not a robust solution
	- we need to handle edge case in the business logic
- Approach 2: Offload this to the database.
	- DB with KV + expiration -> Redis and DynamoDB
	- update entry in Redis/DynamoDB with TTL = 30s

1:04:00