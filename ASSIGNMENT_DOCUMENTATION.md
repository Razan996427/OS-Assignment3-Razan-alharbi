# Assignment 3 - Complete Documentation

**Student Name**: [Razan mohammed alharbi]  
**Student ID**: [445052071]  
**Date Submitted**: [Submission Date]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `445052071_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [May 4, 2026 7:00PM]
**What I implemented**: 
I began by examining the original code to find shared resources like the execution log and counters.
**Challenges encountered**: 
It was challenging to determine where racial circumstances would arise.
**How I solved it**: 
I examined thread behavior and found important passages chapter3,5.
**Testing approach**: 
To see inconsistent results, run the application several times.
**Time spent**: 1h

---

### Entry 2 - [May 4, 2026 8:00PM]
**What I implemented**: 
ReentrantLock was added to safeguard the execution log and shared counters.

**Challenges encountered**: 
ensuring that locks are consistently released correctly.

**How I solved it**: 
Try-finally blocks were used for every lock operation.
**Testing approach**: 
Multiple runs of testing were conducted to confirm steady values.
**Time spent**: 2h

---

### Entry 3 - [May 4, 2026 11:00PM]
**What I implemented**: 
Semaphore was used to regulate CPU access.

**Challenges encountered**: 
appropriately handling interrupted exceptions.
**How I solved it**: 
Try-catch blocks were wrapped around acquire().
**Testing approach**: 
confirmed that only one process is active at a time.
**Time spent**: 
1h
---

### Entry 4 - [May 5, 2026 12AM]
**What I implemented**: 
enhanced progress display and recording.
**Challenges encountered**: 
preventing problems with concurrent modification.
**How I solved it**: 
used a different lock to protect the execution log.
**Testing approach**: 
After execution, log consistency was verified.
**Time spent**: 2h

---

### Entry 5 - [May 5, 2026 1AM]
**What I implemented**: 
Debugging and final testing.
**Challenges encountered**: 
making sure there are no deadlocks.
**How I solved it**: 
used appropriate lock handling and release in the final blocks.
**Testing approach**: 
ran the program in several circumstances.
**Time spent**: 
2h

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:The variable contextSwitchCount has one race condition. Updates could be lost if many threads attempt to increment it at the same time. Incorrect counting could result, for instance, if two threads receive the same data and both report back an incremented value.

The executionLog, an ArrayList, has another race problem. Concurrent writes to ArrayList could alter its internal structure or result in errors like ConcurrentModificationException since it is not thread-safe.

[Your answer here - 4-6 sentences with code examples]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:To ensure that only one thread can access a crucial portion at a time, ReentrantLock is utilized for mutual exclusion. I used it in my code to safeguard the execution log and shared counters.

Access to a restricted set of resources is managed by a semaphore. I simulated CPU access by using a semaphore with a single permit, which only permits one process to run at a time.

[Your answer here - explain your implementation choices]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:When two or more threads wait endlessly for resources owned by one another, this is known as deadlock.

I utilized try-finally blocks to make sure locks are always released in order to avoid deadlock. Additionally, I stayed away from nested locks and maintained straightforward, reliable locking.

[Your answer here - reference try-finally blocks, lock ordering, etc.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:I secured all three counters with a single coarse-grained lock. This lowers complexity and simplifies the design.

However, since the counters are independent, employing distinct locks (fine-grained) would enhance concurrency. Multiple threads can update various counts at the same time thanks to fine-grained locking.

Fine-grained locking is more complicated but provides superior performance, whereas coarse-grained locking is easier but less effective.

[Your answer here - explain coarse-grained vs fine-grained locking, independence of counters, concurrency implications. Show understanding of when to use each approach. 5-8 sentences expected.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, totalWaitingTime

**Why they need protection**: 
Multiple threads update them concurrently.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
// Paste your implementation here
counterLock.lock();
try {
    contextSwitchCount++;
   } finally {
    counterLock.unlock();
  }

**Justification**: 
Ensures atomic updates and prevents race conditions.
---

### Critical Section #2: Execution Log

**What resource**: executionLog (ArrayList)

**Why it needs protection**: Not thread-safe

**Synchronization mechanism used**: 
ReentrantLock

**Code snippet**:
```java
// Paste your implementation here
logLock.lock();
 try {
    executionLog.add(message);
     } finally {
    logLock.unlock();
  }

**Justification**: 
Prevents data corruption and exceptions.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: Limit CPU access to one process

**Number of permits and why**: 1 → simulate single CPU

**Where implemented**: Inside run() and runToCompletion()

**Code snippet**:
```java
// Paste your implementation here
SharedResources.cpuSemaphore.acquire();
try {
    // execution
} finally {
    SharedResources.cpuSemaphore.release();
}

**Effect on program behavior**: 

Ensures sequential execution and prevents conflicts.

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```

**Results**: All runs produced consistent values.
═══ Synchronization Statistics ═══
Total Context Switches: 16
Total Completed Processes: 10
Total Waiting Time: 251794ms
Average Waiting Time: 25179ms

═══ Process Summary Table ═══
Process    Priority     Burst Time   Waiting Time
────────────────────────────────────────────────
P1         2            7286         31127       
P2         2            3785         4020        
P3         5            5256         34407       
P4         3            2595         11815       
P5         3            2565         14415       
P6         3            2859         16979       
P7         4            5636         35665       
P8         2            3261         23847       
P9         4            8620         38212       
P10        5            4898         41307       

═══ Execution Log Summary ═══
Total log entries: 32
(Show that running multiple times produces consistent, correct results)

**Why synchronization is necessary**: Without synchronization, race conditions could occur when multiple threads update shared variables such as contextSwitchCount, completedProcessCount, and totalWaitingTime. This could lead to incorrect values such as missing increments or inconsistent totals. Additionally, concurrent access to executionLog (ArrayList) could cause data corruption or runtime exceptions
**Conclusion**: The use of locks and semaphores ensures that all shared resources are accessed safely, resulting in consistent and correct outputs across multiple executions.

---

### Test 2: Exception Testing
**What I tested**: 
Checking for ConcurrentModificationException and other concurrency-related issues.

**Testing procRan the program multiple times under normal conditions.
Observed execution log behavior while multiple threads were writing to it.
Verified that all log entries were added without errors.edure**: 

**Results**: No ConcurrentModificationException occurred.

**What this proves**: 
This proves that the executionLog is properly synchronized using ReentrantLock, ensuring safe concurrent modifications. It confirms that thread-safe access to shared collections has been correctly implemented.
---

### Test 3: Correctness Verification
**What I tested**: Verifying that the final values of shared variables are correct and match expected logical outcomes.

**Expected values**:All processes should complete execution.
completedProcessCount should equal the total number of processes.
Waiting time values should be reasonable and non-negative.
Context switches should reflect the number of process executions. 
**Actual values**: All processes completed successfully.
completedProcessCount matched the total number of processes.
Waiting time values were consistent and logical.
Context switch count was correctly incremented.

**Analysis**: 
The results match the expected values, indicating that synchronization mechanisms are working correctly. There were no missing updates or incorrect calculations, which confirms that race conditions were successfully prevented.
---

### Test 4: Different Scenarios
**Scenario tested**: 
Running the program with different time quantum values and varying number of processes (automatically generated using different student ID seeds).
**Purpose**: To ensure that the synchronization logic works correctly under different scheduling conditions and workloads.

**Results**: The program behaved correctly in all scenarios. Processes executed in an orderly manner, and no synchronization issues or inconsistencies were observed.

**What I learned**:  I learned that proper synchronization ensures program stability regardless of input variations. Using locks and semaphores makes the system robust and reliable under different execution conditions.

---

## Part 5: Reflection and Learning

### What I learned about synchronization:
This task taught me that synchronization is crucial for preventing race situations and guaranteeing accurate outcomes when many threads access shared resources. I was aware of how ReentrantLock offers mutual exclusion to safeguard important areas. Additionally, I discovered how Semaphore can regulate access to scarce resources, including limiting the number of processes that can use the CPU at once.

I also discovered that certain data structures, such as ArrayList, need explicit synchronization and are not thread-safe. I discovered how crucial it is to use try-finally blocks to ensure that locks are always released, which helps avoid deadlocks. I also comprehended how performance is impacted by the distinction between fine-grained and coarse-grained locking.

Overall, this assignment helped me better grasp how to write multithreaded systems that are secure and dependable.

[6-8 sentences about key concepts, challenges, insights]

---

### Real-world applications:


Give TWO examples where synchronization is critical:

**Example 1**: banking systems, where the same account balance may be updated concurrently by several transactions. Deposits and withdrawals are performed accurately and without data corruption thanks to synchronization.

**Example 2**: 
CPU scheduling in operating systems, in which several tasks vie for the CPU. Processes are carried out fairly and under control thanks to synchronization.
---

### How I would explain synchronization to others:
Organizing access to a shared resource such that only one person uses it at a time when needed is similar to synchronization. Consider an office with just one printer, for instance. The output will be disorganized or tainted if several people attempt to use it simultaneously without cooperation.

Synchronization in programming makes ensuring that threads don't interfere with one another when they try to access shared data. Semaphores and locks are examples of tools that function as rules to restrict who can use a resource and when.

While semaphores may offer restricted entry based on the quantity of permits, locks guarantee exclusive access. We can prevent mistakes and guarantee that programs operate as intended by properly utilizing synchronization.

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/Razan996427/OS-Assignment3-Razan-alharbi.git

**Number of commits**: 

**Commit messages**: 
1. 
2. 
3. 
4. 

---

## Summary

**Total time spent on assignment**: 

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
