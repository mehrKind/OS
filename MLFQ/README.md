MLFQ (Multi-Level Feedback Queue) CPU Scheduler
📋 Overview

This implementation features a Two-Level Multi-Level Feedback Queue with the following characteristics:

    Level 1 (High Priority Queue): Priority-based scheduling (lower priority number = higher priority)

    Level 2 (Low Priority Queue): Round Robin scheduling with a time quantum

    Dispatcher Latency: Context switch overhead time

    Priority Aging: Processes in the low priority queue gradually increase in priority to prevent starvation

⚙️ System Parameters
text

Dispatcher Latency (DL): 2 units
Round Robin Quantum (Q): 6 units

🏗️ Queue Structure
text

┌─────────────────────┐
│   HIGH PRIORITY     │ ← Priority Queue
│   (Level 1)         │   (Lower number = Higher priority)
├─────────────────────┤
│   LOW PRIORITY      │ ← Round Robin Queue
│   (Level 2)         │   (Quantum = 6 units)
└─────────────────────┘

📊 Process Input Format

Each process is defined with the following comma-separated values:
text

ProcessName: ArrivalTime, Priority, CPU_Burst1, IO_Burst1, CPU_Burst2, IO_Burst2, ...

Example Process:
text

p1:0,1,5,7,3

    p1 - Process name

    0 - Arrival time

    1 - Priority (1 is high priority)

    5 - First CPU burst

    7 - First I/O burst

    3 - Second CPU burst

🔄 Scheduling Rules
1. Priority Queue (Level 1)

    Lower priority number = Higher actual priority

    Preemptive priority scheduling

    New processes start in this queue

    Processes demoted to Level 2 after exceeding CPU usage

2. Round Robin Queue (Level 2)

    Fixed time quantum = 6 units

    Non-preemptive within quantum

    Processes complete current CPU burst or quantum

    Implements priority aging to prevent starvation

3. Priority Aging

    Processes in Level 2 gain +1 priority every 20 time units

    Aging prevents indefinite starvation in lower queue

4. Dispatcher Operations

    Context switch time = 2 units (applied when switching between different processes)

    No dispatcher latency when resuming same process after I/O

5. I/O Operations

    When process issues I/O request:

        It leaves CPU immediately

        Goes to I/O queue

        After I/O completion, returns to Priority Queue (Level 1)

📈 Algorithm Flowchart
text

Process Arrives
    ↓
Enter Priority Queue (Level 1)
    ↓
Execute based on priority
    ↓
CPU burst complete? ──Yes──→ Execute I/O ──→ Return to Level 1
    ↓ No
Quantum expired? ──Yes──→ Demote to Level 2
    ↓ No
Continue execution
    ↓
Preempted by higher priority? ──Yes──→ Go back to Level 1 queue
    ↓ No
Process completes ──→ Terminate

🎯 Example Execution

Given the input:
text

2       # Dispatcher Latency
6       # Quantum
p1:0,1,5,7,3
p2:14,3,11
p3:12,0,4,2,8
p4:20,2,8,5,5
p5:26,4,9

Initial Process States:
Process	Arrival	Priority	CPU Bursts	I/O Bursts
p1	0	1	5, 3	7
p2	14	3	11	-
p3	12	0	4, 8	2
p4	20	2	8, 5	5
p5	26	4	9	-
📊 Performance Metrics Calculated

The simulator tracks:

    Turnaround Time: Completion time - Arrival time

    Waiting Time: Total time spent waiting in ready queues

    Response Time: First time getting CPU - Arrival time

    CPU Utilization: Percentage of time CPU was busy

    Throughput: Number of processes completed per time unit

🚀 How to Compile and Run
Compilation:
bash

gcc -o mlfq mlfq_scheduler.c

Execution:
bash

./mlfq
# or with input file
./mlfq < input.txt

Input File Format:
text

2
6
p1:0,1,5,7,3
p2:14,3,11
p3:12,0,4,2,8
p4:20,2,8,5,5
p5:26,4,9

📝 Output Format

The program provides detailed:

    Gantt Chart showing process execution timeline

    Process-by-process statistics (Turnaround, Waiting, Response times)

    System averages for performance metrics

    Queue transitions showing process movement between levels

🎯 Key Features Implemented

    True MLFQ Behavior: Processes can move between queues

    Aging Mechanism: Prevents starvation in lower queue

    Dispatcher Latency: Realistic context switch overhead

    I/O Handling: Proper I/O queue management

    Preemption: Higher priority processes can preempt lower ones

    Statistics Tracking: Comprehensive performance metrics

📚 Theoretical Background

MLFQ attempts to address several scheduling goals:

    Minimize Response Time: For interactive processes (Level 1)

    Maximize Throughput: For CPU-bound processes (Level 2)

    Prevent Starvation: Through priority aging

    Adapt to Process Behavior: CPU-bound processes sink to lower queue

🔍 Troubleshooting Common Issues

    Process starvation in Level 2? - Check aging mechanism (+1 priority every 20 units)

    High dispatcher overhead? - Reduce dispatcher latency parameter

    Poor response time? - Adjust quantum size or priority thresholds

    Infinite loop? - Verify I/O completion and queue transitions

🤝 Contributing

To extend this implementation:

    Add more queue levels

    Implement different aging policies

    Add visualization of queue states

    Implement varying time quanta per level

    Add more sophisticated I/O scheduling

📄 License

This educational implementation is open for academic use and modification.

Note: This MLFQ implementation balances interactive response time with batch job throughput, making it suitable for general-purpose operating systems
