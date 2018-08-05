# Blocking and Non-Blocking I/0

## Introduction

```clike=
ssize_t read(int fd, void * data, size_t count);
ssize_t write(int fd, void * data, size_t count);
```

  * Both of these calls work roughly the same way; they try to read or write the number of bytes requested via the count parameter from or to the open file specified by the descriptor fd. Those bytes are stored in or written from the address pointed to by data. Both calls return the number of bytes which were processed, which is often the same as count.

  * If your application is trying to read 50 bytes of data from a pipe, and only 5 bytes are in that pipe, the kernel will return immediately with those 5 bytes and tell your program what happened via the value of the read() call.

  * By default, read() waits until at least one byte is available to return to the application; this default is called "blocking" mode. Alternatively, individual file descriptors can be switched to "non-blocking" mode, which means that a read() on a slow file will return immediately, even if no bytes are available.

  * This is almost always the behavior programs want; when programs read a line of text from the terminal, for example, they will instruct the kernel to return that line as soon as it’s ready rather than force the user to enter exactly 80 characters before the program sees any of it. Similarly, most programs which read data from a network socket will want to get that data as soon as possible rather than in discretely sized chunks.

```clike=
// read the number of bytes from file descriptor, fd, until it satisfy the specific number, count.
ssize_t readall(int fd, void * data, size_t count)
{
        size_t total = 0;
        ssize_t bytesRead;
        char * dataPtr = data;

        while (count) {
                bytesRead = read(fd, dataPtr, count);
                /* we should check bytesRead for < 0 to return errors
                properly, but this is just sample code! */
                dataPtr += bytesRead;
                count -= bytesRead;
                total += bytesRead;
        }

        return total;
}
```

## References
  * [Blocking and Non-Blocking I/0](http://www.linux-mag.com/id/308/)
