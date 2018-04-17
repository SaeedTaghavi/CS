# Chapter 5 Process Synchronization

## 5.1 Background

Recall [producer–consumer problem](../Chap03/#producerconsumer-problem). We modify it as follows:

```c
while (true) {
    /* produce an item in next_produced */

    while (counter == BUFFER_SIZE)
        ; /* do nothing */

    buffer[in] = next_produced;
    in = (in + 1) % BUFFER_SIZE;
    counter++;
}
```

```c
while (true) {
    while (counter == 0)
        ; /* do nothing */
        
        next_consumed = buffer[out];
        out = (out + 1) % BUFFER_SIZE;
        counter--;

        /* consume the item in next_consumed */
}
```

Suppose `counter == 5` initially. After executing `counter++` in producer or `counter--` in consumer. The value of `counter` may be $4$, $5$, or $6$!

\begin{array}{lllll}
T_0: producer & \text{execute} & register_1 = \text{counter} & \\{register_1 = 5\\} \\\\
T_1: producer & \text{execute} & register_1 = register_1 + 1 & \\{register_1 = 6\\} \\\\
T_2: consumer & \text{execute} & register_2 = \text{counter} & \\{register_2 = 5\\} \\\\
T_3: consumer & \text{execute} & register_2 = register_2 - 1 & \\{register_2 = 4\\} \\\\
T_4: producer & \text{execute} & \text{counter} = register_1 & \\{counter = 6\\} \\\\
T_5: consumer & \text{execute} & \text{counter} = register_2 & \\{counter = 4\\}
\end{array}

!!! note "Race condition"
    Several processes access and manipulate the same data concurrently and the outcome of the execution depends on the *particular order* in which the access takes place.

We need to ensure that only one process at a time can be manipulating the variable `counter`.

## 5.2 The Critical-Section Problem

!!! note "Critical section"
    In which the process may be changing common variables, updating a table, writing a file, and so on.

```c
do {
    /* entry section */
        // critical section
    /* exit section */
        // remainder section
} while (true);
```

A solution to the critical-section problem must satisfy:

1. **Mutual exclusion**
2. **Progree**. Cannot be postponed indefinitely.
3. **Bounded waiting**

Two general approches are used to handle critical sections:

1. **Preemptive kernels**
2. **Nonpreemptive kernels**

Why anyone favor a preemptive kernel over a nonpreemptive one?

- Responsive.
- More suitable for real-time programming.

## 5.3 Peterson's Solution

Peterson's solution is restricted to two processes that alternate execution between their critical sections and remainder sections. The processes are named $P_i$ and $P_j$.

Shared data items:

```c
int turn;
boolean flag[2];
```

```c
do {
    flag[i] = true;
    turn = j;
    while (flag[j] && turn == j);
        /* critical section */

    flag[i] = false;
        /* remainder section */
} while (true);
```

!!! info ""
    If both processes try to enter at the same time, `turn` will be set to both $i$ and $j$ at roughly the same time. *Only one of these assignments will last.*

*Proof*

1. Mutual exclusion is preserved.
2. The progress requirement is satisfied.
3. The bounded-waiting requirement is met.

## 5.4 Synchronization Hardware

!!! info "Atomic"
    Modern computer allow us either to test and modify the content of a word or to swap the contents of two words **atomically**—that is, as one uninterruptible.

Although following algorithms satisfy the mutual-exclusion requirement, they don't satisfy the bounded-waiting requirement.

```c
boolean test_and_set(boolean *target) {
    boolean rv = *target;
    *target = true;

    return rv;
}
```

```c
do {
    while (test_and_set(&lock))
        ; /* do nothing */

        /* critical section */
    
    lock = false;

        /* remainder section */
} while (true);
```

```c
int compare_and_swap(int *value, int expected, int new_value) {
    int temp = *value;

    if (*value == expected)
        *value = new_value;
    
    return temp;
}
```

```c
do {
    while (compare_and_swap(&lock, 0, 1) != 0)
        ; /* do nothing */
    
        /* critical section */

    lock = 0;

        /* remainder section */
} while (true);
```

Following algorithms satisfies all the critical-section requirements.

```c
boolean waiting[n];
boolean lock;
```

```c
do {
    waiting[i] = true;
    key = true;
    while (waiting[i] && key)
        key = test_and_set(&lock);
    waiting[i] = false;

        /* critical section */

    j = (i + 1) % n;
    while ((j != 1) && !waiting[j])
        j = (j + 1) % n;
    
    if (j == i)
        lock = false;
    else
        waiting[j] = false;

        /* remainder section */
} while (true);
```