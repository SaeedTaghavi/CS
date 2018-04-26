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

### 6.3.2 Shortest-Job-First Scheduling

### 6.3.3 Priority Scheduling

### 6.3.4 Round-Robin Scheduling
### 6.3.5 Multilevel Queue Scheduling
###
###