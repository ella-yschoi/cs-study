# Blocking vs. Non-blocking

## What are Blocking and Non-blocking?

- In programming and computer systems, these are two main approaches related to **how operations are executed**.
- The key difference between blocking and non-blocking is **the persistence of the operation** and **concurrent processing**.

## Blocking

- **A situation where, while one operation is being executed, other operations must wait until that operation is finished**.
- This means the system's resources and control are **exclusively held** by the blocking operation until it completes.
- As a result, other operations must wait for the blocking operation to finish, which can affect overall system performance.

## Non-blocking

- While one operation is being executed, **other operations can proceed concurrently**, and there is **no mutual waiting** between operations.
- In the non-blocking approach, **the persistence of operations is reduced**, so **overall system performance improves**, and even if one operation fails, other operations can continue.
