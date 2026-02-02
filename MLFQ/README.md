# 🖥️ Multi-Level Feedback Queue (MLFQ) CPU Scheduler

## 📋 Overview

This project implements a **Two-Level Multi-Level Feedback Queue (MLFQ)** CPU Scheduler with:

- **Level 1 (High Priority Queue):** Preemptive Priority Scheduling  
- **Level 2 (Low Priority Queue):** Round Robin Scheduling  
- **Dispatcher Latency:** Context switching overhead  
- **Priority Aging:** Prevents starvation in lower queue  

The scheduler simulates realistic OS-level scheduling behavior with full statistics tracking.

---

## ⚙️ System Parameters

| Parameter            | Value  |
|----------------------|--------|
| Dispatcher Latency   | 2 units |
| Round Robin Quantum  | 6 units |

---

## 🏗️ Queue Structure


---

## 📊 Process Input Format

Each process is defined using comma-separated values:
ProcessName:ArrivalTime,Priority,CPU1,IO1,CPU2,IO2,...


### Example

p1:0,1,5,7,3


| Field | Description |
|-------|-------------|
| p1    | Process Name |
| 0     | Arrival Time |
| 1     | Priority |
| 5     | CPU Burst 1 |
| 7     | I/O Burst |
| 3     | CPU Burst 2 |

---

## 🔄 Scheduling Rules

### 1️⃣ Level 1: Priority Queue

- Lower number = Higher priority
- Preemptive scheduling
- All new processes enter here
- Processes are demoted after exceeding CPU usage

---

### 2️⃣ Level 2: Round Robin Queue

- Quantum = 6 units
- Non-preemptive inside quantum
- Process runs until:
  - CPU burst ends, or
  - Quantum expires
- Implements aging

---

### 3️⃣ Priority Aging

- Level 2 processes gain **+1 priority every 20 units**
- Prevents starvation

---

### 4️⃣ Dispatcher Operations

- Context switch overhead = **2 units**
- No latency when resuming after I/O

---

### 5️⃣ I/O Operations

- Process leaves CPU immediately
- Enters I/O queue
- Returns to Level 1 after completion

---

## 📈 Scheduling Flow

Process Arrives
↓
Enter Level 1 Queue
↓
Execute (Priority Based)
↓
CPU Burst Complete? ──Yes──→ I/O → Return to Level 1
↓ No
Quantum Expired? ──Yes──→ Demote to Level 2
↓ No
Continue
↓
Preempted? ──Yes──→ Back to Level 1
↓ No
Finished ──→ Terminate


---

## 🎯 Example Input

2
6
p1:0,1,5,7,3
p2:14,3,11
p3:12,0,4,2,8
p4:20,2,8,5,5
p5:26,4,9


---

## 📊 Initial Process Table

| Process | Arrival | Priority | CPU Bursts | I/O Bursts |
|---------|---------|----------|------------|------------|
| p1 | 0 | 1 | 5, 3 | 7 |
| p2 | 14 | 3 | 11 | - |
| p3 | 12 | 0 | 4, 8 | 2 |
| p4 | 20 | 2 | 8, 5 | 5 |
| p5 | 26 | 4 | 9 | - |

---

## 📊 Performance Metrics

The simulator calculates:

- **Turnaround Time** = Completion − Arrival
- **Waiting Time** = Time in Ready Queues
- **Response Time** = First CPU − Arrival
- **CPU Utilization**
- **Throughput**

---

## Input File Format
DispatcherLatency
Quantum
Process1
Process2
...

## 🔍 Troubleshooting
| Problem       | Solution                        |
| ------------- | ------------------------------- |
| Starvation    | Check aging (+1 every 20 units) |
| High overhead | Reduce dispatcher latency       |
| Poor response | Tune quantum                    |
| Infinite loop | Check I/O transitions           |


---

If you’d like, I can also help you:

✅ Customize it for your university format  
✅ Add diagrams/images  
✅ Convert it to PDF/LaTeX  
✅ Write documentation for your report  

Just tell me 👍
