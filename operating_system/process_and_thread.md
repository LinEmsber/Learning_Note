# Process v.s. Thread


## Process
Each process provides the resources needed to execute a program. A process has a virtual address space, executable code, open handles to system objects, a security context, a unique process identifier, environment variables, a priority class, minimum and maximum working set sizes, and at least one thread of execution. Each process is started with a single thread, often called the primary thread, but can create additional threads from any of its threads.

## Thread
A thread is an entity within a process that can be scheduled for execution. All threads of a process share its virtual address space and system resources. In addition, each thread maintains exception handlers, a scheduling priority, thread local storage, a unique thread identifier, and a set of structures the system will use to save the thread context until it is scheduled. The thread context includes the thread's set of machine registers, the kernel stack, a thread environment block, and a user stack in the address space of the thread's process. Threads can also have their own security context, which can be used for impersonating clients.

## Difference
Both processes and threads are independent sequences of execution. The typical difference is that threads (of the same process) run in a shared memory space, while processes run in separate memory spaces. Threads are an operating environment feature, rather than a CPU feature.

## Compare Table
![Leetcode Process vs Thread](https://discuss.leetcode.com/assets/uploads/files/1496197055087-screen-shot-2017-05-30-at-7.17.02-pm.png)


## Reference:
 - Operating System Concepts 9th, Ch.3 and Ch.4
 - [What is the difference between a process and a thread?](https://stackoverflow.com/questions/200469/what-is-the-difference-between-a-process-and-a-thread)
 - [Leetcode Process Vs Thread](https://discuss.leetcode.com/topic/90877/process-vs-thread)
