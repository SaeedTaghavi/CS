# Chapter 6 CPU Scheduling

## 6.1 Basic Concepts

The objective of multiprogramming is to have some process running at all times, to maximize CPU utilization.

A process is executed until it must wait, typically for the completion of some I/O request.

### 6.1.1 CPU–I/O Burst Cycle

Process execution = cycle of CPU + I/O wait.

![small](assets/images/6.1.png)

### 6.1.2 CPU Scheduler

**Short-term scheduler** (CPU scheduler): select process in the ready queue.

### 6.1.3 Preemptive Scheduling

CPU-scheduling decisions when a process:

1. (nonpreemptive) Switches from the running state $\to$ the waiting state (e.g. I/O request, `wait()` for child)
2. (preemptive) Switches from the running state $\to$ the ready state (e.g. interrupt)
3. (preemptive) Switches from the waiting state $\to$ the ready state (e.g. completion of I/O)
4. (nonpreemptive) Terminates

### 6.1.4 Dispatcher

!!! note "Dispatcher"
    The module that gives control of the CPU to *the process selected by the short-term scheduler*. It should be fast.

- Switching context
- Switching to user mode
- Jumping to the proper location in the user program to restart that program

!!! note "Dispatch latency"
    The time it takes for the dispatcher to stop one process and start another running.

## 6.2 Scheduling Criteria

- **CPU utilization**.
- **Throughput**.
- **Turnaround time**. Completion time - Start time.
- **Waiting time**. The sum of the periods spent waiting in the ready queue.
- **Response time**.

## 6.3 Scheduling Algorithms

### 6.3-1 First-Come, First-Served Scheduling

Given processes:

| Process | Busrt Time |
| :--: | :--: |
| $P_1$ | 24 |
| $P_2$ | 3  |
| $P_3$ | 3  |

- Consider order $P_1 \to P_2 \to P_3$:

    - Gantt chart:

        ![normal](assets/images/FCFS.png)

    - Average waiting time = $(0 + 24 + 27) / 3 = 17$ ms.

- Consider order: $P_2 \to P_3 \to P_1$:

    - Gantt chart:

        ![normal](assets/images/FCFS-2.png)

    - Average waiting time = $(0 + 3 + 6) / 3 = 9$ ms.

!!! note "Convoy effect"
    All the other processes wait for the one big process to get off the CPU.

### 6.3.2 Shortest-Job-First Scheduling

***Shortest-next-CPU-burst***.

Given processes:

| Process | Burst Time |
| :--: | :--: |
| $P_1$ | 6 |
| $P_2$ | 8 |
| $P_3$ | 7 |
| $P_4$ | 3 |

By SJF scheduling:

- Gantt chart:

    ![normal](assets/images/SJF.png)

- Average waiting time = $(3 + 16 + 9 + 0) / 4 = 7$ ms.

!!! info ""
    SJF is used frequently in long-term (job) scheduling, but it cannot be implemented at the level of short-term CPU scheduling.

!!! note "Exponential average"
    Let $t_n$ be time of $n$th CPU burst, and $\tau_{n + 1}$ be the next CPU burst.

    \begin{align}
    \tau_{n + 1} & = \alpha t_n + (1 - \alpha)\tau_n, \quad 0 \le \alpha \le 1. \\\\
                 & = \alpha t_n + (1 - \alpha)\alpha t_{n - 1} + \cdots + (1 - \alpha)^j \alpha t_{n - j} + \cdots + (1 - \alpha)^{n + 1}\tau_0.
    \end{align}

Given processes:

| Process | Arrival Time | Burst Time |
| :--: | :--: | :--: |
| $P_1$ | 0 | 8 |
| $P_2$ | 1 | 4 |
| $P_3$ | 2 | 9 |
| $P_4$ | 3 | 5 |

By preemptive SJF scheduling:

- Gantt chart:

    ![normal](assets/images/SJF-2.png)

- Average waiting time = $[(10 - 1) + (1 - 1) + (17 - 2) + (5 - 3)] / 4 = 26 / 4 = 6.5$ ms.

### 6.3.3 Priority Scheduling

### 6.3.4 Round-Robin Scheduling
### 6.3.5 Multilevel Queue Scheduling
###
###