### Problem 1: Basic Producer/Consumer 

> global state: number of seats already reserved, `numSeats`

> can have multiple people reserving at same time

```c
Lock seatLock;
CV seatCV;
int numSeats = MAX;


void reserve(int n) {
  seatLock.lock();
  
  // asking for more than max → have to wait
  while(numSeats < n) {
    seatCV.wait(seatLock);
  }
  
  // reserve seats
  numSeats -= n;
  
  seatLock.unlock()
}

void cancel(int n) {
  seatLock.lock();
  
  // don't need condition variable here b/c we assume they're using calls correctly
  
  // return seats
  numSeats += n;
  
  seatCV.broadcast(); // wakes up all threads in the queue
  seatLock.unlock();
}

```

Fun fact: some high frequency trading firms will go into Java (or other programming language) and modify things to speed it up
  
### Problem 2: Can't Get Along Problem

> global state: who in bathroom

```c

int numDuke = 0;
int numUnc = 0;
CV hasDukeCV;
CV hasUncCV;
Lock bathLock; 

void duke_enter() {
  bathLock.lock();
  
  while(numUnc > 0) {
    hasUncCV.wait(bathLock);
  }
  
  numDuke += 1;
  
  bathLock.unlock();
}

void unc_enter() {
  bathLock.lock();
  
  while(numDuke > 0) {
    hasDukeCV.wait(bathLock);
  }
  
  numUnc += 1;
  
  bathLock.unlock();
}

void duke_leaves() {
  bathLock.lock();
  
  numDuke -= 1;
  
  hasDukeCV = (numDuke == 0);

  hasDukeCV.broadcast(); 
  
  bathLock.unlock();
}

void unc_leaves() {
  bathLock.lock();
  
  numUnc -= 1;
  
  hasUncCV = (numUnc == 0);
  
  hasDukeCV.broadcast(); 
  
  bathLock.unlock();
}
```
  
  
