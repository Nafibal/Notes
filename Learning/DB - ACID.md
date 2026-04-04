**Tags:**
#programming #programming-note #database 
**Docs:**
___
## Atomicity
Either all operations complete successfully or the transaction fails and the db is unchanged.
## Consistency
The DB state must be valid after the transaction. All constraints must be satisfied.
## Isolation
Concurrent transactions must not affect each other.
## Durability
Data written by a successful transaction must be recorded in persistent storage