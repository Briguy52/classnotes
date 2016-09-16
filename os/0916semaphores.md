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





