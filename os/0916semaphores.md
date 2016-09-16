# Semaphores 9/16

### Producer /  consumer with semaphores

> same before/after constraints

> semaphore assignments: 

* mutex (1) - lock
* fullBuffers (0) - init empty (0 full sodas)
* emptyBuffers (MaxSodas) - all empty 

``` 
consumer() {

  fullBuffers.down // block if empty (block before acquiring the lock so that producer can restock)

  mutex.down // lock
  
  take soda out // increase num full, decrease num empty
  
  mutex.up // release
  
  emptyBuffers.up // signal
  
}

producer() {

  emptyBuffers.down

  mutex.down // lock
  
  put soda in
  
  mutex.up // release
  
  fullBuffers.up
  
}

```

#### why have mutex?

don't want 2 consumers, 2 producers, or one of each modifying critical section (`take soda out`) at the same time

fullBuffer/emptyBuffer is there for waiting

#### does order of up/down calls matter?

no, not for correctness

### monitors vs. semaphores

* monitors - *separate* mutual exclusion and ordering
* semaphores - provide both with same mechanism
* semapahores are more 'elegant', but trickier to program
* monitors are more flexible (conditions in while loop, sempahores have it predefined- lock when 0)
* monitor condition can be read easily in code

#### when to use semaphores?

> shared integer maps naturally to problem (ex. counting stuff)

## ok now how do we implement this stuff? 

### thread facts

1. threads share physical memory
2. threads share one or more cpus (hardware)

#### play analogy

* process is a play performance
* program is the play's script
* one cpu is a one person show

#### what is a non-running thread?

* thread = sequence of executing instructions
* non-running thread = paused execution

#### must save thread's private steate
* to re-run, need to reload private state
* want thread to start where it left off 
* thread shouldn't even 'know' that it was paused

#### what state is private to each thread?
* has own PC (location in script)
* has own stack, SP
* has own registers

### thread control block (TCB)

#### what needs to access threads' private data
* the cpu
* the line is stored in the pc
* tcb lives in memory (generally the heap- call `new`)

running threads > stored in cpu

non-running rhead > stored in memory

#### threads can either be...

* running
* ready
* blocked

### switching threads

1. thread returns control to OS (ex. via the `yield` call)
2. OS chooses next thread to run
3. OS saves state of current thread *to* its TCB
4. OS loads context of next thread *from* its TCB
5. Run the next thread

project one: 3-5 in `swapcontext`

### 1. Thread returns control to OS

#### How does the thread system get control?
* voluntary internal events
* involuntary external events (hardware interrupts)

### 2. Choosing the next thread
* no threads ready, wait
* when threads ready, choose one

### 3. saving state of current thread
* what needs to be saved? (registers, PC, SP)
* what makes this tricky? (need registers to save state but you're trying to save all the registers)

#### storing the PC is hard
> don't just store current PC, store where you want to resume (may be a couple lines down)

### 4. OS loads the next thread 

#### Where is the next thread's state?
> thread control block (in mem)

#### How to load the registers?
> with load calls 

### 5. OS runs the next thread

#### How to resume thread's execution?
> jump to the saved PC

#### On whose stack are these steps running? (ie Who jumps to the saved PC?)
> the thread that called `yield` (or was interrupted or called lock/wait)

fun fact: can't go from ready to blocked (have to be running to be blocked)



