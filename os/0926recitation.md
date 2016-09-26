# OS recitiation monday 9/26

## Three rules about mutex and CV

### 1. A mutex keps track of the threads waiting to acquire it in a queue

> Each mutex has *its own* queue

> Mutex is just a lock

### 2. A condition variable keeps a queue of threads waiting on that variable

### 3. The queues in 1) and 2) are separate

> Things get broken if use same queue 

## Deadlockia example problem

> cringeworthy

1. Limited resource? Yes, roads.
2. Hold and wait? Yes, if at intersection.
3. Pre-Emption? Yes, cannot remove people from roads
4. Circular chain of requests? No, because only going one direction (won't want to acquire N/S lock if you're going W/E)

So no deadlocks!!

## Control Flow Graph and locks

idk check slides

## Semaphores vs. Mutex - what's the difference?

### Looks the same... right?

```
mutex.acquire()
.
.
.
mutex.release()
```


```
semaphore.down()
.
.
.
semaphore.up()
```
NOT THE SAME!!!

### Mutexes are *coupled*

`mutex.acquire()` called --> HAVE TO call `mutex.release()`

likewise, cannot call `mutex.release()` without having called `mutex.acquire()` first

Creates a symmetry
