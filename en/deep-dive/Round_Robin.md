# Round Robin

## What is Round-Robin?

- In computer science and information technology, it is one of the `scheduling algorithms`.
- This algorithm is used in multi-tasking environments to `fairly allocate CPU time` and `ensure processes are executed fairly`.
- It is often used in real-time systems or conversational systems where `response time is important`.

## How Round-Robin Works

- The round robin algorithm assigns a `fixed time slice (quantum)` to each operation, and when this time is up, the operation moves to the next one.
- Each operation is executed `in order`, and is only executed for the `given time slice`.
- If the operation finishes before the time slice ends, it moves to the next operation; otherwise, it goes to the end of the waiting queue and waits for its next turn.

## Advantages and Disadvantages of Round-Robin

- The round robin algorithm `provides equal execution opportunities for all operations` and, by ensuring short response times, `prevents infinite loops`.
- However, if the execution times of all operations are not equal or if the priorities of the operations differ, `fair scheduling may not be guaranteed`.
