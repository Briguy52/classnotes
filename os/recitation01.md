### Problem 1: Basic Producer/Consumer 

> global state: number of seats n

> can have multiple people reserving at same time

```
void reserve(int n) {
  seatLock.acquire()
  
  // asking for more than max → have to wait
  while(numReserved + n ≥ MAX) {
    hasSpaceCV.wait(seatLock)
  }
  
  reserveSeats(n) 

  seatLock.release()
}

void cancel(int n) {
  seatLock.acquire()
  
  // don't need condition variable here b/c we assume they're using calls correctly
  
  cancelSeats(n)
  
  hasSpaceCV.signal()
  
  seatLock.release()
}

```
  
  
