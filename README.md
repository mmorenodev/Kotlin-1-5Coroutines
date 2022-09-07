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

