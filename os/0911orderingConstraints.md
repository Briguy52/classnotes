# Ordering Constraints

### Intro 

> Fun fact: in Java, use `synchronized (this)` as a lock

**Problem:** You can't go into an inf loop while you're holding the lock (no other threads can break you out)

**Key idea:** hold the lock while reading/writing from shared state

example: 
```
unlock 
while (head == NULL) {} // PROBLEM! Other threads could mess with head
lock
```

### Ideal Solution

* Let dequeueing thread 'sleep' (and wake up when Q is non-empty)
* Problem: what to do with the lock? 

#### Release the lock before sleep? 

Note: answer all 'what could go wrong?' questions by 1) release lock 2) look at other thread 3) go back to first one

### Two Types of Sync

1. Mutual exclusion (one thread doing something at a time with **locks**)
2. Ordering (condition variables let threads sleep inside a critical section)

#### Locks and condition variables

internal atomic actions: release lock, go on waitlist, sleep (we'll implement later)

#### Condition variable operations

```
wait (lock) {
  // next 3 lines must be done atomically
  release lock // lock held on entry
  put thread on wait queue
  go to sleep
  // after wakeup
  acquire lock // lock held on exit
}

signal () { 
  wakeup one waiter (if any) // atomic, lock usually held
} 

broadcast () {
  wakeup all waiters (if any) // atomic, lock usually held 
}
```

#### CVs and invariant 

> recall: invariants must be true when locks are released (e.g. room must be clean when open the door)

User programs

* make sure 'your room is clean' before wait (b/c wait releases the lock!)
* lock may have changed hands
* state may have changed b/w wait entry and exit (whole purpose of waiting)

**Note:** always wrap `wait` inside of a `while` loop

### Tips for using monitors

1. List shared date needed for problem
2. Figure out the locks (1 lock / group of shared date)
3. Bracket code that uses shared data with lock/unlock
4. Think about before/after conditions

### Producer / Consumer Problem

ex. Soda producers and soda drinkers: how to avoid direct handoff? Answer: use vending machines as buffers!

1. What are variables/shared state? - Soda machine buffer, number of sodas in machine (≤ maxSodas)
2. Locks? - 1 to protect all shared state (sodaLock)
3. Mutual exclusion - Only one thread can use machine at a time (can't take out when putting in)
4. Ordering Constraints - Consumer must wait if machine is empty (CV hasSoda), Producer must wait if machine is full (CV hasRoom)

```
consumer() {
  sodaLock.acquire()
  
  while (machine.empty) {
    hasSodaCV.wait(sodaLock) 
  }
  
  take out soda
  
  hasRoomCV.signal()
  
  sodaLock.release()
}

producer() {
  sodaLock.acquire()
  
  while(machine.isFull) {
    hasRoomCV.wait(sodaLock)
  }
  
  fill machine
  
  hasSodaCV.signal()
  
  sodaLock.release()
}
```

#### Producer- inf loop ok? Why/why not?

> it's okay... wait has an **unlock** inside it!

Review: what's inside `wait`? 

```
wait (lock) {
  // next 3 lines must be done atomically
  release lock // lock held on entry
  put thread on wait queue
  go to sleep
  // after wakeup
  acquire lock // lock held on exit
}
```

#### Producer - sleep ok? Why/why not?

> no, shouldn't hold lock while doing slow stuff 

#### Can I always use broadcast instead of signal?

> yes

#### Why would signal be preferred?

> won't wake up everyone (some can still sleep)
