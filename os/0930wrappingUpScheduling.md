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

