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
CV uncCV;
CV dukeCV;
Lock bathLock; 

void duke_enter() {
  bathLock.lock();
  
  while(numUnc > 0) {
    dukeCV.wait(bathLock);
  }
  
  numDuke += 1;
  
  bathLock.unlock();
}

void unc_enter() {
  bathLock.lock();
  
  while(numDuke > 0) {
    uncCV.wait(bathLock);
  }
  
  numUnc += 1;
  
  bathLock.unlock();
}

void duke_leaves() {
  bathLock.lock();
  
  numDuke -= 1;
  
  if (numDuke == 0) {
    uncCV.broadcast();
  }

  bathLock.unlock();
}

void unc_leaves() {
  bathLock.lock();
  
  numUnc -= 1;
  
  if (numUnc == 0) {
    dukeCV.broadcast();
  }
  
  bathLock.unlock();
}
```

Note: Condition variables are *not booleans*! (Don't set them true/false, instead, the condition is whatever is in the if/while)  
  
#### Would starvation be a problem?

1. UNC enter A
2. Duke enter x[wait]
3. UNC enter B
4. UNC leave A
5. UNC enter C
6. ...

One solution- set a limit to how many UNC people can enter (e.g. after 5, wait for all to leave and then Duke can enter)

