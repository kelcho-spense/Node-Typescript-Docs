# 12. Worker Threads in Node.JS (Multithreading)

## Overview
**Node.js** runs the javascript code using only a single thread, which means only one statement can be executed at a time. 
However, Nodejs itself is multithreaded and supplies hidden threads in js using the **libuv library**, which handles operations like network requests or reading a file from a disk. Nodejs introduced the **worker threads module**, which allows us to create `multiple tasks in parallel` and execute those tasks. These worker threads divide CPU-bound tasks to many workers for optimization and do not block the main thread.

## Introduction
 
* Threads are like a small process: they have their instruction pointer and can conduct one Javascript task at a time. Threads reside within a process's memory and they don't have their memory. The execution of threads is similar to that of processes.
* Javascript is single-threaded by default which means only one thread is available to perform all the operations. This was the main drawback of the javascript which led to the implementation of server-side Asynchronous I/O, which would reduce the race among the thread in a multithreading environment.
* Lets us now see how javascript has evolved. Javascript has introduced a new concept to prevent the limitation we are facing, that is, Web Workers. Web Workers (threads in js) are mainly used to perform CPU-intensive Javascript tasks. They can perform long-running tasks without affecting or blocking the main execution thread.
* Using a Constructor, we can create a worker object, which will run a js file. This js file includes code to run the worker thread; these workers run in some other global context that is different from the current window. However, Node.js built-in asynchronous I/O operations are more efficient than Workers can be. Web workers are not of much help with I/O-intensive work.

## Need for Parallel Processing

Imagine if we do CPU-intensive tasks synchronously, such as a loop with a large number of iterations, fetching data from an API or external resource, and handling file system I/O. This will create a large block of synchronous code that will block the rest of the code. Meanwhile, if another request is sent,
it will be waiting for a much larger period before the response is sent back to the user.

![event_loop.png](event_loop.png)

Hence, there is a need for parallel computation where the incoming requests don't have to wait. But due to the single-threaded nature of Node.js, the main loop can take up only one task at a time. Hence worker threads are required to provide the parallel computation. The event loop takes on the synchronous part of all the processes,
and the time taking asynchronous part is assigned to a worker thread provided by the **libuv.**
The threads then parallel execute the asynchronous task in the background and respond to the event loop once the process is completed.

## Worker Threads in Node JS

When a Node.js process is executed, it runs:

* **One Process:** It is the Node.js global object that can be accessed from anywhere and has details about what’s being executed at a time.
* **One Thread:** Javascript being single-threaded, has only one main thread for the execution of tasks at a given time.
* **One Event Loop:** This plays an important role in the asynchronous nature and non-blocking I/O of Node.js, by offloading tasks to the system kernel whenever possible through promises, callbacks and async/ await.
* **One JS Engine Instance:** This is a computer program that executes js code.
* **One Node.js Instance:** This is a computer program that executes Node.js code.

In simple words, Node runs on **one main thread**, and there can only be a single task executing at a time in the event loop. However,
there is a downside: if we have CPU-intensive code, like reading a file from the disk or any complex calculations in a large dataset,
then this can block the main thread and hence another process from being executed.

A function is known as **blocking** if the main event loop must wait until it has finished executing the following command. 
A **_Non-blocking function_** will allow the main event loop to continue executing further code and alerts the main loop once it has finished by calling a _callback_.

It’s important to differentiate between _CPU operations_ and _I/O (input/output) operations_, 
as only I/O operations are run _concurrently because they are executed asynchronously_.
So the main _goal of Worker threads in js is to enhance the performance of CPU-intensive operations_, _**and not I/O operations**_.

![threads.png](threads.png)

### Hence worker threads in js have the following properties:

1. One process
2. Multiple threads
3. One event loop per thread
4. One JS Engine Instance per thread
5. One Node.js Instance per thread

## How do Worker Threads Work?

Worker threads in js were submitted as an experimental feature in Node.js version v10, but they became stable in version v12 of Node.js.
Worker threads provide us with APIs to deal with CPU-intensive tasks without blocking the main thread; hence they maintain the responsive application. 
Unlike clusters or child_process, worker threads share memory among themselves by transferring ArrayBuffer instances.

The worker thread gives CPU-intensive tasks to another thread while keeping the main thread available to new user requests.
When we create a new worker thread, it is associated with a new event loop, so the main thread's event loop can take any further requests.

Worker threads in js don't have any synchronization mechanism like any multi-threaded programming language. 
Chrome's V8 engine is used to run the Node process. Each worker thread is isolated from other threads. 
The V8 engine allows us to create isolated V8 runtimes.V8 isolates the instances with their own Javascript heaps and micro-task queues.
V8 isolates exist in fully distinct states. Objects from one isolation may not be used in another.
The embedder can build several isolates and use them in different threads at the same time. At any given time, only one thread can enter an isolate.

![workerthreads.png](workerthreads.png)
