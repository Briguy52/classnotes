## Deadlocks

### Definitions

#### Resource

> thing needed by a thread to do its job (ex locks, disk space, memory, CPU, etc.)

> threads must **wait**

#### Deadlock

> circular waiting for resources

#### Example

* CS 310 and CS 330 are full
* You are in 310 and want to switch out 
* Someone else in 330 and want to switch out

#### Deadlock and starvation

> Deadlock --> Starvation

> Starvation = one process waits forever

#### Common thread work pattern

Phase 1: acquire resources
 
Phase 2: release resources

(Still prone to deadlocks)

#### Dining Philosophers

Everyone picks up right chopstick, have to then wait for left chopstick.

### Conditions for Deadlock

#### 1. Limited resources

Not enough for all threads simultaneously

#### 2. Hold and wait pattern

Hold one resource while waiting for another (ex. hold right chopstick until left becomes available)

#### 3. No pre-emption

Cannot force threads to give up resources

#### 4. Circular chain of requests

Arrows

* Thread → resource it's waiting for
* Resource → thread that's holding it

ex. People build **wait-for** graphs with students/universities

## What do we do about it?

1. Ignore 
2. Detect & Fix
3. Prevent

### Detect-and-fix

#### First part: detect 

> Easy, just scan the wait-for graph

#### Second part: fix 

1. Kill first thread, take back locks by force (unsafe...can expose inconsistent state)
2. Undo actions of 1 or more thread, retry (often used in databases, not always possible to undo actions like launching missiles lol)



