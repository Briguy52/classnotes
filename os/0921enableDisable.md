#### How do threads get switched out? (2 ways)
1. interrupts (hardware external events)
2. yields, I/O request (internal events)

Easy to prevent internal events, use disable/enable to prevent external events

#### Why is it ok for lock code to disable interrupts?

It is trusted code- can disable interrupts. (User code is untrusted code)

Lock code is 'part of the government'

#### Do we need to disable interrupts in unlock?

```
unlock () {
  disable interrupts
  value = FREE
  enable interrupts
}
```

It's possible that we set the value to FREE and get stuck

#### Why enable-disable in lock loop body?

Don't want thread that owns the lock to be interrupted! (else other threads will never be able to acquire the lock)

#### Disabling interrupts

ok for uni-processor, breaks on multi-processor

#### No effect on other cores 
> So other threads can still be executing at the same time

If want to acquire lock on my core, don't want to mess with others. They're also probably working on different things than what I'm working on.

Could use atomic load-store to make a lock. BUT inefficient and lots of busy-waiting.

#### Atomic read-modify-write instruction
Made by hardware people

All this happens atomically
* Read value from memory to register
* Write new value to memory
* Many variations on this

Don't worry about the implementation

#### Too much milk solution 2

Hard part- checking if note was there and leaving a note (without being interrupted). Couldn't make this atomic before!

#### One version - test&set

```
test&set (addr) {
  tmp = addr // read
  addr = 1 // write
  return (tmp) // returns old value
}
```

#### Lock implementation with test&set

```
lock(addr) {

  while(test&set(addr)) {
    // why does this work?
    // if lock is held (1 before) then read 1, overwrite as 1, return 1 (so spin!)
    // if lock was free (0 before) then read 0, overwrite as 1, return 0 (so good!)
  }
}

unlock(addr) {
}

```
But we still haven't gotten rid of busy-waiting (wasteful!)

#### Lock implementation #3 (no busy-waiting)

Have queue associated with each lock now 

##### Lock

if not free
* add thread to queue of threads waiting for lock
* switch to next ready thread // don't add to ready queue

##### Unlock

if anyone on queue of threads waiting for lock
* take waiting thread off queue
* put on ready queue
* value - BUSY // why exiting unlock() with value set to BUSY?

#### What is swap context?
> It 'does' the switching from one thread to another

#### Ready queue vs lock queue

Lock queue cannot be scheduled (blocked)
Ready queue can be scheduled

RQ = {T1 T2}
LQ = {T3}
T0 is running and owns the lock

T0 calls unlock()
* disables interrupts
* set value to FREE
* T3 calls lock (???)
* checks if anyone waiting for lock - T3 moves to end of ready queue
* T1 is now running
* T1 calls lock(), disables interrupts
* T1 goes to lock queue (why is this fair? T3 already called lock)
* T1 now blocked
* Switch to T2 (next ready thread)
* T2 tries to acquire lock, sees that lock is busy
* Switch to T3, running T3
* T3 resumes inside of lock() and skips to `enable interrupts` and exits

This is called a **hand-off lock** (hand off lock to next thread in line)

#### Definitely will be an exam problem on describing state of queues!!

Who might get the lock if it weren't handed off directly?

(Could be anyone)

#### What kind of ordering of lock acquisition does the hand-off lock provide?

> FIFO (first to ask are first to get)

#### In lock()

(if not free)
`add thread to queue of threads waiting for lock` = pushing to end of thread, run swapcontext

#### Why a separate queue for the lock?

> Separate queues make this a lot more easy to reason with

