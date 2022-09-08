# Kotlin-1-5Coroutines
Learning about coroutines at Pluralsight
Content:
- Introduction to Coroutines
- Examine builders and 'suspend' functions
- Coordinator of coroutines
- Returning data from coroutines (async/await)
- Understand exceptions and cancellation
- Understand 'structured concurrency'


**Coroutines are Asynchronous not necessarily multi threaded**

## Asynchronous Programming Styles
- Callbacks
- Futures (promises)

## Using Coroutines
- Coroutines are more natural
- Looping constructs are natural
- Exception handling is natural 


launch coroutine builder -> creates a fire and forget coroutine, so it doesn't return any data. 

A coroutines may run on the same thread it was launched from, or it may run on a separate thread, but IT WILL RUN ASYNCHRONOUSLY. 

Delay suspends the coroutine, what it means is that when se suspend the coroutine, the thread on which the coroutine is running is able to be reused for other coroutines. 

A coroutines is not specifically tied to a thread; they'll use threads in the background, maybe, but when coroutines are scheduled, a coroutine is scheduled onto one thread, so the next time it runs, it could be scheduled onto a different thread. So we could have many coroutines running on a limited number of threads. 

We can only create a certain number of threads, and we should be able to create many more coroutines. 

## Defining Suspend Functions and Running Them With Coroutine Builders
Need to understand two core concepts:
- Coroutine builders
- Suspending functions

What is a Coroutine? 
- Provide a mechanism for non-preemptive multitasking
- Coroutines allow functions to yield control
- Control can then return to this same point later
- Provide a context for suspend functions

What is a suspend function?
- A function marked with the 'suspend' MODIFIER (NOT IS A KEYWORD)
- Adding the modifier DOES NOT CHANGE THE BEHAVIOR but changes the compiled code
- Suspend functions offer a promise - never block the calling thread
- When you write a suspend function, that suspend funciton should never block the calling thread.

**Suspend functions can only be run from other suspend functions or within a coroutine.**
**Normal functions, like 'main' are not suspend functions**, it's kind of a problem, because if we can only run suspend functions from inside another suspend function or from within a coroutine, and normal functions like main are not suspend functions, then how do we run suspend functions? 
- NEED TO CREATE A COROUTINE.

**Coroutine builders bridge BETWEEN NON-SUSPENDING AND SUSPENDING WORLD.**
Coroutine builders can run 'suspend' functions and also 'normal' functions. 

There are 'global' builders. 
- However, these should not generally be used
- See 'structured concurrency' later

If you're writing console applications, then you will often use a global builder called 'runBlocking' at the start of the application. 

runBlocking is another coroutine builder that waits until the coroutine finishes.

**BLOCKING THREADS LEADS TO SCALABILITY ISSUES.**

COROUTINES CAN HAVE OTHER COROUTINES AS CHILDREN.

# Jobs, Contexts, Scopes and Structured Concurrency
- Provide a 'context' in which to run suspend functions: builders provide for us context in which we can run suspend functions. 
- Project a 'scope' for suspend functions
- Scope allows for a degree of control: cancellation
- Coroutine provides a 'Job': can be used to wait, cancel, etc.



