
## What I did, and why:
I first chose to implement lock and batching, however I didnt see a really noticable improvements in performance time. After testing with 20 runs of part 1 and the lock and batch impolementation I saw an improvement of roughly 5% or so.
I then attempted to find a better solution and chose the fine grained approach, creating multiple locks for threads to utilize based on block size. This posed a problem though, I found that almost all my blocks were 32 bytes causing only 1 lock class to be used. My solution to this was to use a modulo opterator to disperse locks into each created class. By using this implementation with larger files of 5MB or so I was able to improve the processing time by 12-16% in my experiments using run sets of 20 runs each. I chose this method because one of the 1st problems that Im sure most people saw was that there was a bottleneck when using 1 lock for all threads to utilize. By increasing the amount of locks to be used among the threads this reduces the lock contention by partitioning the memory heap into regions using that regions specific lock.

## What I learned:
At the time of implementing the fine grained approach it made sense that by reducing the lock contention that I would see an improvement in performance, particularly in larger files due to less threads needing to wait to acquire the lock among the X amount of locks created. However, after running some tests I saw that there is a trade off with the overhead that comes with implementing and distributing the threads to each lock such as memory fragmentation, and lock management. This lead me to look into finding some kind of sweet spot to optimize how many locks to use for any given file. Through some research I found that it only makes sense to implement as many locks as there are CPU cores since you can only truely run as many threads simultaneously as there are CPU cores. Overall, while this solution did increase performance, it wasnt as much as I'd hoped it would and I feel like there is a better solution or way to implement a fine grained approach that would yield better results.



## The Data:
### File Size: 4.7MB, 20 runs each.

Unoptimized Average: 305ms
Optimized Average: 272ms

Unoptimized Median: 291ms
Optimized Median: 257ms


## Sample Data:

![Unoptimized Data Sample](./report2original.png "Unoptimized Data Sample")


![Optimized Data Sample](./report2optimize.png "Optimized Data Sample")

