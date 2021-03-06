# Asynchronous I/O Learning Note

## Introduction

  * In computer science, asynchronous I/O, or "Non-sequential I/O" is a form of input/output processing that permits other processing to continue before the transmission has finished.

  * Input and output (I/O) operations on a computer can be extremely slow compared to the processing of data. An I/O device can incorporate mechanical devices that must physically move, such as a hard drive seeking a track to read or write; this is often orders of magnitude slower than the switching of electric current. For example, during a disk operation that takes ten milliseconds to perform, a processor that is clocked at one gigahertz could have performed ten million instruction-processing cycles.

  * A simple approach to I/O would be to start the access and then wait for it to complete. But such an approach (called synchronous I/O or blocking I/O) would block the progress of a program while the communication is in progress, leaving system resources idle. When a program makes many I/O operations, this means that the processor can spend almost all of its time idle waiting for I/O operations to complete.

  * Alternatively, it is possible to start the communication and then perform processing that does not require that the I/O be completed. This approach is called asynchronous input/output. Any task that depends on the I/O having completed (this includes both using the input values and critical operations that claim to assure that a write operation has been completed) still needs to wait for the I/O operation to complete, and thus is still blocked, but other processing that does not have a dependency on the I/O operation can continue.

  * In general Asynchronous I/O revolves around two functions: The ability to determine that input or output is immediately possible without blocking or to queue I/O operations, and then determine that a pending I/O operation has completed. The first case is arguably preferable since it potentially avoids a redundant copy operation.

## Edge-triggered versus level-triggered AIO mechanisms

  * A level-triggered AIO mechanism provides (when queried) information about which AIO events are still pending. In general, this translates to a set of file descriptors on which reading (or writing) can be performed without blocking.

  * An edge-triggered mechanism on the other hand provides information about which events have changed status (from non-pending to pending) since the last query.

## open() in non-blocking mode
  * It is possible to open a file (or device) in "non-blocking" mode by using the O_NONBLOCK option in the call to open. You can also set non-blocking mode on an already open file using the fcntl call. Both of these options are documented in the GNU libc documentation.

  * Non-blocking mode makes it possible to continuously iterate through the interesting file descriptors and check for available input (or check for readiness for output) simply by attempting a read (or write). This technique is called polling and is problematic primarily because it needlessly consumes CPU time - that is, the program never blocks, even when no input or output is possible on any file descriptor. An event notification mechanism is needed to discover when useful reads/writes are possible.

## AIO on Linux

  * There are several ways to deal with asynchronous events on linux; all of them presently have at least
some minor problems, mainly due to limitations in the kernel.
    * Threading
    * Signals
    * The SIGIO signal
    * select() and poll() (and pselect/ppoll)
    * epoll()
    * POSIX asynchronous I/O (AIO)

### Threading

  * The use of multiple threads is in some ways an ideal solution to the problem of asynchronous I/O, as well as asynchronous event handling in general, since it allows events to be dealt with asynchronously and any needed synchronization can be done explicitly (using mutexes and similar mechanisms).

  * However, for large amounts of concurrent I/O, the use of threads has significant problems for practical application due to the fact that each thread requires a stack (and therefore consumes a certain amount of memory) and the number of threads of in a process may be limited by this and other factors.

  * Threading is presently the only way to deal certain kinds of asynchronous operation (obtaining file locks, for example).

### Signals

  * Signals can be sent between unix processes by using kill() as documented in the libc manual, or between threads using pthread_kill(). There are also the so-called "real-time" signal interfaces. Most importantly, signals can be sent automatically when certain asynchronous events occur.

  * Signal handlers as an asynchronous event notification mechanism work just fine, but because they are truly executed asynchronously there is a limit to what they can usefully do. There are a limited number of C library functions which can be called safely from within a signal handler).

### Asynchronous Notification
  * Although the combination of blocking and nonblocking operations and the select method are sufficient for querying the device most of the time, some situations aren't efficiently managed by the techniques we've seen so far.

  * Let's imagine a process that executes a long computational loop at low priority but needs to process incoming data as soon as possible. If this process is responding to new observations available from some sort of data acquisition peripheral, it would like to know immediately when new data is available. This application could be written to call poll regularly to check for data, but, for many situations, there is a better way. By enabling asynchronous notification, this application can receive a signal whenever data becomes available and need not concern itself with polling.

  * User programs have to execute two steps to enable asynchronous notification from an input file. First, they specify a process as the "owner" of the file. When a process invokes the F_SETOWN command using the fcntl system call, the process ID of the owner process is saved in filp->f_owner for later use. This step is necessary for the kernel to know just whom to notify. In order to actually enable asynchronous notification, the user programs must set the FASYNC flag in the device by means of the F_SETFL fcntl command.

  * After these two calls have been executed, the input file can request delivery of a SIGIO signal whenever new data arrives. The signal is sent to the process (or process group, if the value is negative) stored in filp->f_owner (A pointer to a struct file is commonly named filp).

  * For example, the following lines of code in a user program enable asynchronous notification to the current process for the stdin input file:

```clike=
/* dummy sample; sigaction(  ) is better */
signal( SIGIO, &input_handler );

fcntl( STDIN_FILENO, F_SETOWN, getpid() );

oflags = fcntl( STDIN_FILENO, F_GETFL );

fcntl( STDIN_FILENO, F_SETFL, oflags | FASYNC );
```

## Reference
  * [Asynchronous_I/O](https://en.wikipedia.org/wiki/Asynchronous_I/O)
  * [Asynchronous I/O and event notification on linux](http://davmac.org/davpage/linux/async-io.html)
