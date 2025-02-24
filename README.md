# Multi_Threaded_Concurrency_Examples
This project demonstrates advanced multithreaded programming concepts in C, focusing on thread synchronization, resource management, and deadlock prevention. It contains two separate programs that solve different concurrency problems.

The full project is available at `https://github.com/lili-carbonn/OS_Simulation_Project/`

If you would like to view the full code, you should request access. This is due to concerns of plagiarism.

## Authors

- Leala Carbonneau
- Jake DePietro

## Problem 1: Shoe World Simulation (`shoeElection.c`)

### Overview

This program simulates different types of shoes (Running, Dress, and Crossover) going on and off a stage in a round-robin fashion. The simulation follows these rules:

- Only shoes of the currently selected type can go on stage
- Maximum of 6 shoes can be on stage at once
- Once all 6 spots are filled, the shoes wait for each other to finish, then leave together
- After all shoes leave, the next shoe type is selected
- The process repeats in a round-robin style

### Implementation Details

- Uses POSIX threads to simulate each shoe as an independent entity
- Employs mutexes and condition variables for synchronization
- Implements a wait queue for shoes waiting to go on stage
- Uses a state machine to track each shoe's current status:
  1. Listening off stage (initial state)
  2. Ready to go on stage
  3. Yapping on stage (active on stage)
  4. Ready to go off stage (finished talking)

### Known Issues

There is a defect that sometimes causes shoes to continuously spin loop and get stuck in the makeShoe main loop. This can cause the program to halt early.

## Problem 2: Package Processing System (`FedOops.c`)

### Overview

This program simulates a package processing system with different colored teams (Red, Blue, Yellow, Green) of workers processing packages through various stations (Weighing, Barcoding, Measuring, Jostling). Each package has a set of instructions that specify which stations it needs to visit in what order.

### Implementation Details

- Uses POSIX threads to simulate 60 workers (15 per team)
- Employs semaphores to manage access to shared resources (stations)
- Implements a deadlock prevention strategy:
  - Workers try to acquire all necessary station semaphores before processing
  - If any acquisition fails, they release all acquired semaphores and retry
  - Once all semaphores are acquired, the worker processes the package through each station
- Each team has a counter that represents the most current team member on the workfloor
- Workers acquire their respective team's semaphore if it's their turn

### Deadlock Prevention

To avoid deadlock, the worker tries to acquire a semaphore for all necessary stations. If any of these fail, the worker releases all semaphores and retries. Once all semaphores are acquired, the worker goes to each station and after completing the task, atomically switches to the next station and releases the lock of the previous station.

## Compilation and Execution

### Prerequisites

- GCC compiler
- POSIX threads library
- POSIX semaphores library

### Compiling

```bash
# Compile Problem 1
gcc -o shoeElection shoeElection.c -lpthread

# Compile Problem 2
gcc -o fedoops FedOops.c -lpthread
```

### Running

Both programs require a `seed.txt` file in the same directory for random number generation.

```bash
# Run Problem 1
./shoeElection

# Run Problem 2
./fedoops
```

## Project Structure

- `shoeElection.c` - Source code for Problem 1
- `FedOops.c` - Source code for Problem 2
- `seed.txt` - Seed file for random number generation
- `problem1_explanation.txt` - Additional explanation for Problem 1
- `problem2_explanation.txt` - Additional explanation for Problem 2
- `output1.txt` - Sample output from Problem 1
- `output2.txt` - Sample output from Problem 2
- `README.md` - This file

## Credits

This project was created by Leala Carbonneau as a operating systems homework assignment.
