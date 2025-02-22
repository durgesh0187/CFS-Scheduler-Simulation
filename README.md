CFS Scheduler Simulation
This project is a simulation of the Completely Fair Scheduler (CFS), a process scheduling algorithm used in the Linux kernel. The simulation demonstrates how CFS manages CPU-bound and I/O-bound tasks by maintaining fairness and prioritizing tasks based on their vruntime and weight.

Table of Contents
Overview
Key Concepts
CFS Parameters
Scheduling Logic
Simulation Details
CPU-Bound Tasks
I/O-Bound Tasks
How It Works
Getting Started
Simulation
Overview
The Completely Fair Scheduler (CFS) is designed to provide fair CPU time to all tasks by using a virtual runtime (vruntime) metric. Tasks with lower vruntime are prioritized, ensuring that higher-priority tasks (those with higher weight) get more CPU time. This simulation implements a simplified version of CFS, focusing on:

CPU-bound tasks: Tasks that primarily use the CPU.
I/O-bound tasks: Tasks that frequently wait for I/O operations.
Key Concepts
CFS Parameters
Weight: A priority value assigned to each task. Higher weight means higher priority.
Formula: weight = NICE_0_LOAD / (priority + 1)
NICE_0_LOAD is a constant (1024 in this simulation).
vruntime: Virtual runtime of a task, calculated as:
vruntime += (executed_time * NICE_0_LOAD) / weight
Higher-priority tasks have slower vruntime growth.
Runqueue: A priority queue (min-heap) sorted by vruntime. The task with the smallest vruntime is scheduled next.
Scheduling Logic
CPU-bound tasks:
Execute for a fixed time slice (1ms in this simulation).
Update vruntime based on executed time and weight.
I/O-bound tasks:
Simulate I/O wait by sleeping for a fixed duration (10ms in this simulation).
Penalize vruntime for the I/O wait time.
Execute for a short time slice (1ms) after I/O completion.
Simulation Details
CPU-Bound Tasks
Tasks that primarily use the CPU.
Each task has:
cpu_burst_time: Total CPU time required.
priority: Determines the task's weight.
Execution:
Run for a fixed time slice (1ms).
Update vruntime and reduce cpu_burst_time.
Requeue if cpu_burst_time > 0.
I/O-Bound Tasks
Tasks that frequently wait for I/O.
Each task has:
cpu_burst_time: Total CPU time required.
priority: Determines the task's weight.
iowait: Simulated I/O wait time (10ms).
Execution:
Sleep for iowait to simulate I/O wait.
Penalize vruntime for the I/O wait time.
Run for a short time slice (1ms) after I/O completion.
Requeue if cpu_burst_time > 0.
How It Works
Initialization:

All tasks are added to the runqueue with initial vruntime values.
Tasks are categorized as CPU-bound or I/O-bound.
Scheduling Loop:

The scheduler picks the task with the smallest vruntime from the runqueue.
If the task is CPU-bound:
Execute for 1ms.
Update vruntime and reduce cpu_burst_time.
Requeue if cpu_burst_time > 0.
If the task is I/O-bound:
Simulate I/O wait by sleeping for 10ms.
Penalize vruntime for the I/O wait time.
Execute for 1ms after I/O completion.
Requeue if cpu_burst_time > 0.
Termination:

The simulation ends when all tasks have completed their cpu_burst_time.
Getting Started
Prerequisites
C++ compiler (e.g., g++).
Basic understanding of process scheduling.
Steps to Run the Simulation
Clone the repository:
git clone <repo-url>
cd cfs-scheduler-cpp
python -m venv /myvenv # create virtual env if not created, use python3 if python not working
source myvenv/bin/activate
cd build # make build dir in root folder if it is not there
cmake ..
make
./cfs-schedular
cd ..
python3 plot.py
Simulation
First simulation is of processes with different vruntime, priority etc but all processes are of IO_BOUND nature whereas second one contains all CPU_BOUND processes.

We can clearly see that IO tasks are waiting for their iowait time and getting penalised. More priority IO task is getting more penalised and scheduling will get delayed whereas in CPU bound tasks, Process 1 got finalised quickly because it has the highest priroirty and got penalised the least because it started with least vruntime and highest priority. So, resulted in more frequent scheduling.

image image

dissecting linux schedulers & implementing our toy cfs_scheduler simulation:- Link
