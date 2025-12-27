**Tags:**
#programming #programming-note #database 
**Docs:**
___

There are several ways that a transaction can be interfered bt other transactions that runs simultaneously with it. This interference will cause something we called "read phenomenon"

## Read Phenomena
- **Dirty Read**
  A transaction reads data written by other concurrent uncommitted transaction
- **Non-Repeatable Read**
  A transaction reads the same row twice and sees different value because it has been modified by other committed transaction
- **Phantom Read**
  A transaction re-executes a query to find rows that satisfy a condition and sees a different set of rows, due to changes by other committed transaction
- **Serialization Anomaly**
  The result of a group of concurrent committed transactions is impossible to achieve if we try to run them sequentially in any order without overlapping

___

## 4 Standard Isolation Level
To deal with those phenomenon above, The American National Standards Institute (ANSI) defined 4 standard isolation level:
1. **Read Uncommitted**
   Can see data written by uncommitted transaction
2. **Read Committed**
   Only see data written by committed transaction.
   Prevent Dirty Read.
3. **Repeatable Read**
   Same read query always returns same result.
   Prevent Non-Repeatable Read and Phantom Read.
4. **Serializable**
   Can achieve same result if execute transactions serially in some order instead of concurrently
