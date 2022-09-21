\newpage
# Capítulo 07: Synchronization Examples

## 7.1 Classic Problems of Synchronization
* In this section, we are going to use semaphores for synchronization, but that's not always the case in real applications.

### 7.1.1 The Bounded-Buffer Problem
* The **bounded-buffer problem** is commonly
```
/* Structure of the producer process */
while (true){
    ...
    /* produce an item in next produced */
    ...
    wait(empty);
    wait(mutex);
    ...
    /* add next produced to the buffer */
    ...
    signal(mutex);
    signal(full);
}

/* Structure of the consumer process */
while (true){
    wait(full);
    wait(mutex);
    ...
    /* remove an item from buffer to next consumed */
    ...
    signal(mutex);
    signal(empty);
    ...
    /* consume the item in nextconsumed */
    ...
}
```
### 7.1.2 The Readers-Writers Problem
* **Readers**: processes that want to read from a database
* **Writers**: processes that want to write in a database
* If a reader and a writer tries to acces the shared data simultaneously, chaos may ensue.
* **First readers-writers problem**: no reader will be kept wating unless a writer has already obtained permission to use the shared object.
    * no readers are to be kept waiting for other readers to finish simply because a writer is waiting.
* **Second readers-writers problem**: once a writer is ready, that writer perform its write as soon as possible.
    * if a writer access an object, no new readers may start reading.
* The readers-writers problem and its solutions have been generalized to privide **reader-writer locks** on some systems.
    * Acquiring a reader-writer lock requires specifying the mode of the lock: either _read_ or _write_ access.
* Readers-writer locks are most useful in the following situations:
    * In applications where it is easy to identify which processes only read shared data and which precesses only write shared data.
    * In applications that have more readers than writers.
### 7.1.3 The Dining-Philosophers Problem
* 
 
## 7.2 Synchronization within the Kernel
### 7.2.1 Synchronization in Windows
* Windows OS is a multithreaded kernel that provides support for real-time applications and multiple processors.
* For thread synchronization outside the kernel, Windows provides **dispatcher objects**.
    * uses different mechanisms.
    * may be in either a signaled state or a nonsignaled state.
    * **signaled state**:
        * if available, a thread will not block when acquiring an object
    * **unsignaled state**:
        * if unavailable, a thread will block when attempting to acquire an object
* A **critical-section object** is a user-mode mutex that can often be acquired and released without kernel intervention.
    * particularly efficient because the kernel mutex is allocated only when there is contention for the object.
    * in practice, there is very little contention, so the savings are significant.
### 7.2.2 Synchronization in Linux
* Linux provides several different mechanisms for synchronization in the kernel, the simplest one being an atomic integer.
    * _Atomic integer_: all math operations using atomic integers are performed without interruption.
        * particularly efficient in situations where an integer variable, such as a counter, needs to be updated.
* Mutex locks are available in Linux for protecting critical sections within the kernel.
* Spinlocks asn semaphores for locking in the kernel are also provided.
* Both spinlocks and mutex locks are **nonrecursive**.
