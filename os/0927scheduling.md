# Scheduling

### Performance Metrics

#### 1. Minimize response time

> Elapsed time to do a job (what users care about!)

Try to maximize the idle time (incoming jobs can then finish ASAP)

#### 2. Maximize throughput

> Jobs per second (what admins care about)

Try to keep parts as busy as possible

Minimize wasted overhead (eg. context switching b/w threads)

#### 3. Fairness

> Share CPU among threads equally

How to schedule 2 jobs (100 seconds each)?

1. Job 1 then Job 2 (average reponse time = 150)
2. Alternate b/w 1 and 2 (average response time = 200)

Think of OS as gov't. Fairness is critical but comes ata cost. 

Finding a balance is hard- have to consider trade-offs. 
