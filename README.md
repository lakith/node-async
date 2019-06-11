# Asynchronous Node

```sh
console.log("Event Start up.");

setTimeOut(()=>{
	console.log("Wait 2 Seconds")
},2000);

setTimeOut(()=>{
	console.log("Wait 0 Seconds")
},0);

console.log("Event End.");
```
**Output of this code snippet.**

> Event Start up.<br/>
Event End. <br/>
Wait 0 Seconds. <br/>
Wait 2 Seconds.

**Lets understand the code behavior.**

In order to understand the behavior we should consider about call stack callback queue and event loop. 

> **Call Stack**
A stack is also a data structure that stores items. It is a [LIFO](http://www.i-programmer.info/babbages-bag/263-stacks.html) data structure in that the last item to be stored (pushed) on the stack is the next or "first" item to be removed (popped) from the stack 
There are various places for stacks and their counterparts [queues](https://en.wikipedia.org/wiki/Queue_(abstract_data_type)). Basically, they are important because they store procedures so that they can be executed in an organized fashion.
Basically all the node.js synchronous calls will be run through this stack. For an example log outputs will be directory run on this stack.

  > **Node API**
  > When an asynchronous code comes up node API takes it and it will handles the call. For an example in above example setTimeOut() calls will be execute by the Node API. Then it will be queued in the callback queue.

> **Callback Queue**
> After executing calls with node API those will be queued in the callback queue.after executing the main thread , these queued calls will entered to the call stack.

> **Event Loop**
> Before we dive into what the Node.js event loop is and how it works, we need to identify the various parts of the whole.
> 1.  **[Chrome V8 Engine](https://developers.google.com/v8/)**: Google's open source high-performance JavaScript engine, written in C++.
> 1.  **[The C  `libuv`  library](http://docs.libuv.org/en/v1.x/)**: libuv is a multi-platform support library with a focus on asynchronous I/O. It was primarily developed for use by Node.js.
> 1.  **[The Operating System](https://en.wikipedia.org/wiki/Operating_system)**: system software that manages computer hardware and software resources and provides common services for computer programs. All computer programs, excluding firmware, require an operating system to function.
> 
> The Chrome V8 engine is responsible for executing JavaScript code. It can actually be embedded into a C++ application which is what Node.js is at its core. The V8 engine takes in JavaScript as a string, executes it and then prints the result to [STDOUT](http://stackoverflow.com/questions/3385201/confused-about-stdin-stdout-and-stderr). However, there is an issue with this because JavaScript is synchronous. So how does Node.js allow V8 to execute synchronous JavaScript code while performing asynchronous operations?
> The following diagram illustrates the communication between these various pieces of Node's event-driven I/O.
>![Node.js non-blocking event loop](https://i.ibb.co/Ld9JQT9/Capture1.png%22%20alt=%22Capture1)

The diagram can be broken down into these steps : 
1.  V8 runs JavaScript that requires an asynchronous task to be performed.
2.  The  `libuv`  library submits a request to the OS to perform the task.
3.  The task is placed onto a queue of tasks that will complete sometime in the future.
4.  The event loop constantly checks to see if any tasks in the queue are complete.
5.  Once the event loop finds a completed task it returns it to continue executing the corresponding JavaScript callback.

By viewing this diagram and understanding these steps, it is easy to see that the Node.js event loop is exactly that: it is a continuously looping process that is checking to see if other processes have completed.


**Code Execution steps explained :**

First  main method will be initiated on the call stack. Main method starts the code executing. Then the log method will be initiated on the call stack. Since its not a asynchronous code, it will execute and print the result "Event Start up." to the console. 
Then, setTimeOut() method will be executed, since its an asynchronous call, it will pass to the node.js api and it will be execute and other processes will continue to run. Then the the other set time out method starts to execute by the node.js api since its time out is 0, results will be queued on the call back queue and the other code base will continue to execute. Then the last line of  the code, which is the log method calls and it will print the result to the console. Then the log method and main method will be removed from the call stack. after removing the main method, event loop will add the asynchronous code results which were queued in the callback queue. Since 0 time out method is already queued in the callback queue, it will be entered to the call stack and it will executes.

![Code Executing steps in brief](https://i.ibb.co/Fn7gNHT/Untitled-Diagram-2.png" alt="Untitled-Diagram-2)
