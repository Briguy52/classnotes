# 9/14

### Prioritizing waiting writers

* always safe to broadcast- sometimes safe to signal

* broadcast to all readers, signal to other writer

### When to use broadcast?

* more than one thread might need to run (if writer leaves, waiting readers should be woken)
* signal could wake up wrong thread

### Reader/writer interface vs. standard locks

* R/W looks like locks (*Start = lock, *Finish = unlock)

Pros/Cons
* Have lock = only thread vs. have reader not necessarily only one
* reader doing stuff
* other thread calls reader *Start
* no active/waiting writers then **also** can mess with database
* if writer, have to wait for all readers to clear out


```
check310waitlist() {
  readerStart
  sqlQuery
  readerFinish
}

eurom310() {
  writerStart
  sqlQuery
  writerFinished
}
```

`sqlQuery` is shared state

### One more type of synchronization primitive: Semaphores

monitors = lock + CV
'condition' in condition variable is the argument of the while() loop it's housed in

> Semaphores, 2 operations: up/down

``` 
// aka "V" (verhogen)
up() {
  // begin atomic
  value++
  // end atomic
}

// aka "P" (proberen)
down() {
  do {
    // begin atomic
    if (value > 0) {
      value-- 
      break
    }
    // end atomic
  }  while (1)
}
```

Note: value (above) can never be negative b/c only decremented when positive

Sempahores can do three things

* set init value
* up
* down

You cannot get its value!

#### Semaphore mutual exclusion (behave like a lock)

``` 
init s = N
s.down(); // any other threads call down() --> have to wait ('spin')
// critical section
s.up();
```
Set initial value to N for N threads

#### Semaphore ordering constraints (behave like monitor)

> thread A waits for thread B before proceeding

```
// thread A
s.down();

// thread B
s.up();
```
Set initial value of semaphore to 0

A is guaranteed to wait for B to finish (doesn't matter if A or B first)

Like a CV in which condition is `sem.value == 0`

Can think of it as a 'prefab' condition variable with an internal condition 
