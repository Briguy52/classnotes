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
