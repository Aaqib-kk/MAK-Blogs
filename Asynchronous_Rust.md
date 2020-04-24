# Asynchronous Rust Programming

As we already know, **Rust** allows us to write fast, safe software programming. But why write asynchronous code?

Asynchronous code allows us to run multiple task on a single thread. Overall, asynchronous code have the potential to be much faster and user fewer resources than a corresponding threaded implementation. However, there is a cost. Threads are naively supported by the operating system, and using them does not require any special programming model, any function can create a thread and calling a function that uses threads is usually as simple as calling any normal function. However, asynchronous function requires special support from the language or libraries. In Rust,  **async fn** creates an asynchronous function which returns a **Future**.

Here when we talk about **async**, we are talking about running code concurrently, or having multiple overlapping (in time) computation run on a single thread.

**Multithreading** is ideal when you have multiple computational intensive tasks also called CPU bound tasks that can be spread across multiple, separated cores.

**Concurrent programming** is better suited for when the task spends more time in waiting, such as for a response from a server. These tasks are called IO bound. 

So **asynchronous programming** lets us run multiple of these IO-bound computations at the same time on a single thread. They can run at the same time because when they are waiting for a response, they are just idle, so we can let the computer keep working on something that is not waiting. When we reach at a point where we need the result of an asynchronous computation, we must **.await** it. In Rust, values that are **awaitable** are known as **futures**.

# **async/.await Primer**

**Async/.await** is Rust’s built-in tool for writing asynchronous function that looks like synchronous code. async transform a block of code into a state machine that implements a trait called **Future**. Whereas calling a blocking function in a synchronous method would block the whole thread, blocked **Futures** will yield control of the thread, allowing other **Futures** to run.

The value returned by **async fn** is a **Future**. For anything to happen, the **Future** needs to be run on an executor.
## Example:
```
use futures::executor::block_on;

async fn hello_world() {
    println!("hello, world!");
}

fn main() {
    let future = hello_world(); // Nothing is printed
    block_on(future); // `future` is run and "hello, world!" is printed
}
```
