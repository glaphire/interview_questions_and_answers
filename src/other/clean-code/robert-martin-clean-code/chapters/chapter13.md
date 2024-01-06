## Chapter 13. Concurrency

*Disclaimer: this chapter overview is simplified due to skipping Java-specific recommendations

1. **Why Concurrency?**
   - Concurrency is a decoupling strategy. It helps us decouple _what_ gets done from _when _it
     gets done. In single-threaded applications what and when are so strongly coupled that the
     state of the entire application can often be determined by looking at the stack backtrace.

2. **Myths and Misconceptions**
    - ___Concurrency always improves performance.___
      Concurrency can _sometimes_ improve performance, but only when there is a lot of wait
      time that can be shared between multiple threads or multiple processors. 
      Neither situation is trivial.
    - ___Design does not change when writing concurrent programs.___
      In fact, the design of a concurrent algorithm can be remarkably different from the
      design of a single-threaded system. The decoupling of _what_ from _when_ usually has a
      huge effect on the structure of the system.
   
3. **Realistic view on concurrency issues**
    - ___Concurrency incurs some overhead,___
      both in performance as well as writing additional code.
    - ___Correct concurrency is complex,___ even for simple problems.
    - ___Concurrency bugs aren’t usually repeatable,___ so they are often ignored as one-offs
      instead of the true defects they are.
    - ___Concurrency often requires a fundamental change in design strategy.___

4. **Concurrency Defense Principles**
    - ___Single Responsibility Principle___
      - Concurrency-related code has its own life cycle of development, change, and tuning.
      - Concurrency-related code has its own challenges, which are different from and often
        more difficult than non-concurrency-related code.
      - The number of ways in which miswritten concurrency-based code can fail makes it
        challenging enough without the added burden of surrounding application code.
      - **Recommendation**: Keep your concurrency-related code separate from other code.
    - ___Corollary: Limit the Scope of Data___
      - **Recommendation**: Take data encapsulation to heart; severely limit the access of any
        data that may be shared.
    - ___Corollary: Use Copies of Data___
      - A good way to avoid shared data is to avoid sharing the data in the first place. In some
      situations it is possible to copy objects and treat them as read-only.
    - ___Corollary: Threads Should Be as Independent as Possible___
      - **Recommendation**: Attempt to partition data into independent subsets than can be
        operated on by independent threads, possibly in different processors.
    - ___Know Your Library___
      - **Recommendation**: (rewritten for PHP): 
        be familiar will all classes and concepts that PHP, extensions and third-party libraries are providing.
    - ___Know Your Execution Models___
      - <table>
            <tr>
                <th>Model</th>
                <th>Explanation</th>
            </tr>
            <tr>
                <td>Bound Resources</td>
                <td>Resources of a fixed size or number used in a concurrent environment. Examples include database connections and fixed-size read/write buffers.</td>
            </tr>
            <tr>
                <td>Mutual Exclusion</td>
                <td>Only one thread can access shared data or a shared resource at a time.</td>
            </tr>
            <tr>
                <td>Starvation</td>
                <td>One thread or a group of threads is prohibited 
                    from proceeding for an excessively long time or forever.
                    For example, always letting fast-running threads through first could starve out 
                    longer running threads if there is no end to the fast-running threads.</td>
            </tr>
            <tr>
                <td>Deadlock</td>
                <td>Two or more threads waiting for each other to finish. Each thread has a resource 
                    that the other thread requires and neither can finish until 
                    it gets the other resource.</td>
            </tr>
            <tr>
                <td>Livelock</td>
                <td>Threads in lockstep, each trying to do work but finding another“ 
                    in the way.” Due to resonance, threads continue trying to make progress
                    but are unable to for an excessively long time — or forever.</td>
            </tr>
        </table>
        - Given these definitions, we can now discuss the various execution models used in concurrent programming.
      **Recommendation**: Learn these basic algorithms and understand their solutions.
    
- **Producer-Consumer**
    - One or more producer threads create some work and place it in a buffer or queue. 
      One or more consumer threads acquire that work from the queue and complete it. 
      The queue between the producers and consumers is a ___bound resource___.
      This means producers must wait for free space in the queue before writing
      and consumers must wait until there is something in the queue to consume.
      Coordination between the producers and consumers via the queue involves producers
      and consumers signaling each other. The producers write to the queue and signal
      that the queue is no longer empty. Consumers read from the queue and signal 
      that the queue is no longer full. Both potentially wait to be notified
      when they can continue.

- **Readers-Writers**
    - When you have a shared resource that primarily serves as a source of information for readers, 
      but which is occasionally updated by writers, throughput is an issue. 
      Emphasizing throughput can cause starvation and the accumulation of stale information. 
      Allowing updates can impact throughput. Coordinating readers, 
      so they do not read something a writer is updating and vice versa is a tough balancing act. 
      Writers tend to block many readers for a long period of time, thus causing throughput issues.
    - The challenge is to balance the needs of both readers and writers to satisfy correct operation,
      provide reasonable throughput and avoiding ___starvation___. A simple strategy makes writers wait 
      until there are no readers before allowing the writer to perform an update.
      If there are continuous readers, however, the writers will be starved. On the other hand,
      if there are frequent writers and they are given priority, throughput will suffer. 
      Finding that balance and avoiding concurrent update issues is what the problem addresses.

- **Dining Philosophers**
    - Imagine a number of philosophers sitting around a circular table. A fork is placed to the left of each philosopher.
      There is a big bowl of spaghetti in the center of the table.
      The philosophers spend their time thinking unless they get hungry.
      Once hungry, they pick up the forks on either side of them and eat.
      A philosopher cannot eat unless he is holding two forks.
      If the philosopher to his right or left is already using one of the forks he needs,
      he must wait until that philosopher finishes eating and puts the forks back down.
      Once a philosopher eats, he puts both his forks back down on the table and waits until he is hungry again.
    - Replace philosophers with threads and forks with resources and this problem is similar 
      to many enterprise applications in which processes compete for resources. Unless carefully designed,
      systems that compete in this way can experience ___deadlock, livelock,___ throughput, and efficiency degradation.

5. **Beware Dependencies Between Synchronized Methods**
   - Client-Based Locking — Have the client lock the server before calling the first method and make sure 
     the lock’s extent includes code calling the last method.
   - Server-Based Locking — Within the server create a method that locks the server, calls
     all the methods, and then unlocks. Have the client call the new method.
   - Adapted Server — create an intermediary that performs the locking. This is an example of server-based locking,
     where the original server cannot be changed.

6. **Writing Correct Shut-Down Code Is Hard**
    - Writing a system that is meant to stay live and run forever is different from writing something that works
      for a while and then shuts down gracefully.
    - Graceful shutdown can be hard to get correct. Common problems involve deadlock, 
      with threads waiting for a signal to continue that never comes.
      For example, imagine a system with a parent thread that spawns several child threads
      and then waits for them all to finish before it releases its resources and shuts down. What if
      one of the spawned threads is deadlocked? The parent will wait forever, and the system will never shut down.
    - **Recommendation**: Think about shut-down early and get it working early. It’s going to take longer than you expect.
      Review existing algorithms because this is probably harder than you think.

7. **Testing Threaded Code**
   - Write tests that have the potential to expose problems and then
     run them frequently, with different programatic configurations and system configurations
     and load. If tests ever fail, track down the failure. Don’t ignore a failure just because the
     tests pass on a subsequent run.
   - Do not ignore system failures as one-offs.
   - Do not try to chase down non-threading bugs and threading bugs at the same time. 
     Make sure your code works outside of threads.