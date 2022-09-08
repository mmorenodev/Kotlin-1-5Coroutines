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

## Job Interface
Job is an interface, coroutine builders such as 'launch' return a Job to us for us to use. 
'launch' returns a Job
- Can use this to 'join' the coroutine
- Can use it to check if the coroutine has finished

## Join
Similar to joining a thread in that the CALLING CODE BLOCKS UNTIL the coroutine has finished.

## Joining Coroutines

<img width="506" alt="imagen" src="https://user-images.githubusercontent.com/66931789/189231935-8554d76e-1a42-480a-b75f-8ab6b6aa1c02.png">

By calling the join method, which suspends THIS COROUTINE until the coroutine it's joined to has finished its work. 

## Basic Cancellation of Coroutines
One of the issues with join is that we might wait forever, and we can certainly end up waiting for too long. 

What happens if a coroutine runs too long?
- Can cancel

Coroutine may have open resources, and coroutines **may throw exceptions DURING CANCELLATION**

I can't just stop a thread, I can't just terminate a thread. To stop a thread we have to get that thread to cooperate. The same thing is true with coroutines, if I want to cancel a coroutine, cancellation IS COOPERATIVE. This means that the coroutine itself must get INVOLVED WITH THE CANCELLATION. 

**If you don't check for cancellation then will not be cancelled.**

If I write a suspend function that is doing some long-running task, then that suspend function should occasionally check to see if it's been cancelled, and if it's been cancelled. If it has been cancelled, then end itself appropriately. 

**ALL BUILT-IN SUSPENDING FUNCTIONS COOPERATE.** For example, if you're in the middle of a delay function and you cancel the coroutine, delay will check to see if it's been cancelled, and if it has, the coroutine itself will become cancelled. 







