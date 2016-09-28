# Scheduling

## Scheduling Goals

1. Minimize average response time (users)
2. Maximize throughput (admins)
3. Fairness (share cpu among threads equitably)

#### How can we schedule these jobs?

Job 1 then Job 2

Alternate between 

### FIFO

First come first served

No pre-emption (run until done)

Run thread until complete, no timer interrupts

#### Pros

Simple (what we do for our locks)

#### Cons

Short jobs stuck behind long ones 

#### Solution

Add pre-emption! (pre-empt CPU from long-running jobs)

### Round Robin

In what way is FCFS fairer than round robin?

> First to start will have better response time (ex. kids lol)

### STCF (shortest time to completion first)

> with and without pre-emptions

sort jobs with bubble sort kinda

> short jobs benefit a lot, long jobs not that impacted

#### Pros

* Optimal average response time

#### Cons

* Prone to starvation
* Can be unfair
* Requires knowledge of the future

### Knowledge of the future

#### Examples

* Caching
* Banker's algo

How do you know how much time jobs take?

* Ask the user
* Distribution and guess

### Grocery Store Scheduling

How do grocery stores schedule?

Kinda like FCFS

Express lanes - make it kind of like STCF

No context switching/pre-emptions (people think unfair)

### Final Example - Disk/CPU

Good principle - start things that can be parallelized early! (ex. cooking)

## Real Time Systems

#### Typical hardware constraints

* Slow, low-power processors
* Small memories
* Little to no persistent storage

#### Typical workload

* Periodic sensing and actuation
* Task periodicity --> deadlines (ex. self-driving car)

### Real time scheduling

Worst-case analysis 

#### How do we meet all of our deadlines? o.O

> Optimal strategy: work on what's due first (earliest deadline first - EDF)

> Sometimes have to drop tasks

### Earliest deadline first (EDF)

### Priority scheduling

* nasa mars rover example 
* high priority tasks stuck behind lower priorities 
* weather should have inherited bus' priority (b/c bus was waiting on weather)

### Lottery scheduling

> tasks are assigned tickets (% of tickets represents share of resource)

ex. A gets 75 tickets, B gets 25 tickets

To schedule A and B, hold a lottery (so choose a random number b/w 0, 99, whoever has the winning ticket runs)

#### Pros

Lotteries can be fast, require little state

Proportional shift

Tickets can be dynamically assigned 


