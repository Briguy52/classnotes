# Wrapping Up Scheduling

### Rate-monotonic scheduling

#### Each task has
* periodicity (ex. must run every 10 ms)
* a worst case execution time (ex. 5 ms)
* a static priority (ie does not change over time)

#### Basic idea
* assign task priorities based on periodicity (shorter = better)

#### Thoughts
* Rate monotonic - priorities are static (don't change)
* Earliest deadline first - priorities are dynamic (can change as you complete more of one task)

periodicity is from the beginning of time (not since last time something is run)

### Course Administration
* will cover concurrency
* everything up to today
* best way to study is to do project 1

### Summary
* concurrent programs help simplify tasks decomp
* cooperating threads must sync (to protect shared state, to control interleave, etc.)
* implement the abstraction of many CPUs on one CPU
* deadlock (want to make sure at least one thread can make progress)
* CPU scheduling (tradeoff b/w efficient and fair)

### Practice problem
Barber shop and customers

```
// vars
int roomCapacity
int sofaCapacity
int totalCustomers
int[] seat 
sofaQueue
standingQueue
seatOpenCV

Stylist(int id) {
  acquire seat lock
  
  while(seat[id] == TRUE) {
    wait(lock, seatOpenCV)
  }
  seat[id] = FALSE // seat no longer occupied for this stylist
  totalCustomers-- // done with this customer
  
  NewCustomer = dequeue(sofaQueue)
  
  if totalCustomers > 2 {
    seatOpenCV.broadcast() // move customers up in line
  }
 
  release seat lock
}

Customer() {
  acquire lock
  while (totalCapacity >= 9 || all seats == TRUE) {
     wait(lock, cv) // don't come in if over 9 customers
  }
  
  // promote from standing to sofa
  while (sofaQueue.size < 4) {
    sofaQueue.push(standingQueue.pop)
  }
  
  release lock
}

```

### Landon Method

#### what vars?
* lock

#### what shared/global state?
* numInRoom 
* numOnSofa
* inChair[3]
* stylistCV[3] 
* sofaCV
* standCV
* chairFullCV] // wait until hair cut

```
Stylist(int id) {
  lock acquire
  
  // stylists stuck in room forever lol
  while(1) {
      // no one in chair- wait
      while(!inChair[id]) {
        stylistCV[id].wait
      }
      // do have customer in chair
      // optional: lock release
      cut hair
      // optional: lock acquire
      // wake up customer after done cutting hair
      chairCV.signal
  }

  lock release
}

// think of this as a linear story (what does a single customer do step by step?)

Customer() {
  lock acquire
  
  // check room
  while (numInRoom >= 9) {
    standingCV.wait
  }
  
  // entered room
  numInRoom++
  
  // check sofa
  while(numInSofa >= 4) {
    sofaCV.wait
  }
  
  // got on sofa
  numOnSofa++

// check chairs
  while(inChair[0] && inChair[1] && inChair[2]) {
    chairFullCV.wait
  }
  
  // look for an open chair
  stylist = findOpenChair(inChair)
  inChair[stylist]=1 
  numSofa --
  sofaCV.signal // room on sofa now
  stylistCV[stylist].signal 
  chairCV[stylist].wait
  chairIsFull.signal
  numInRoom---
  standCV.signal
  
  lock release
}

