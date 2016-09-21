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
test&set (X) {
  tmp = X // read
  X = 1 // write
  return (tmp) // returns old value
}
```

#### Lock implementation with test&set




