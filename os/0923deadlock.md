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

#### 1. Detect 

> Easy, just scan the wait-for graph

#### 2. Fix

1. Kill first thread, take back locks by force (unsafe...can expose inconsistent state)
2. Undo actions of 1 or more thread, retry (often used in databases, not always possible to undo actions like launching missiles lol)

#### Dining Philosophers with Detect & Fix

> If holding R and will wait for L, drop R and try again

> Livelock - keep on dropping and picking up at the same time (happens in real life with databases)

### Deadlock Prevention 

#### Pre-Emption

* Can pre-empt CPU usage (context switch)
* Can pre-empt memory usage
* Not that great for locks

#### Eliminate circular chain of requests

> How can we get rid of these cycles?

In dining philosophers?

> Number chopsticks

> Pick up lower-numbered chopstick first

#### Why does this work? 

> At some point in time

* Thead T holds highest-numbered acquired resource
* T is guaranteed to make progress. Why?
* If T needs a higher numbered resource, it must be free
* If T needs a lower numbered resource, it must already have it

In practice, imposing global orderings has its issues- must change the application. 

### Banker's Algorithm

> Like acquiring all resources first (but more efficient)

Phase 1: State max resources needed

Phase 2: While not done, get some resources (blocking if not safe)

Phase 3: Release all resources

#### Example

Bank has $6k

Customers establish credit limits (max resources)

Borrow money (up to their limit)

Pay back money

#### Nice algorithm but few people use it. Why?

> Very difficult to know in advance what resources you need

> Computationally difficult for large allocation problems

> Many resources aren't 'fungible' (ie money is fungible but locks may not be)

### Course Administration

* Review most recent lectures
* Several puzzles to solve on your own
* Extra office hours begin next week

