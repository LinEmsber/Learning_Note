# Deadlock

## Introduction
In concurrent computing, a deadlock occurs when two competing actions wait for the other to
finish, and thus neither ever does. Deadlock is a common problem in multiprocessing systems,
parallel computing, and distributed systems, where software and hardware locks are used to
handle shared resources and implement process synchronization.


In an operating system, a deadlock occurs when a process or thread enters a waiting state because
a requested system resource is held by another waiting process, which in turn is waiting for
another resource held by another waiting process. If a process is unable to change its state
indefinitely because the resources requested by it are being used by another waiting process,
then the system is said to be in a deadlock.


In a transactional database, a deadlock occurs when two processes, each within its own transaction,
updates two rows of data (that is, records) but in the opposite order. For example, process A
updates row 1 then row 2 at the same time that process B updates row 2 then row 1. Process A can't
finish updating row 2 until process B is finished, but process B cannot finish updating row 1
until process A is finished. No matter how much time is allowed to pass, this situation will never
resolve itself. Because of this, a database management system will typically kill the transaction
of the process that has done the least amount of work.


## Livelock

A livelock is similar to a deadlock, except that the states of the processes involved in the livelock
constantly change with regard to one another, none progressing. Livelock is a special case of resource
starvation; the general definition only states that a specific process is not progressing.

A real-world example of livelock occurs when two people meet in a narrow corridor, and each tries to
be polite by moving aside to let the other pass, but they end up swaying from side to side without
making any progress because they both repeatedly move the same way at the same time.

Livelock is a risk with some algorithms that detect and recover from deadlock. If more than one process
takes action, the deadlock detection algorithm can be repeatedly triggered. This can be avoided by
ensuring that only one process (chosen randomly or by priority) takes action.


## Reference
 - [wiki Deadlock](https://en.wikipedia.org/wiki/Deadlock)
 - [whats the difference between deadlock and livelock](http://stackoverflow.com/questions/6155951/whats-the-difference-between-deadlock-and-livelock)
