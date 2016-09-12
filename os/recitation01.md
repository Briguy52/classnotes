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
  
