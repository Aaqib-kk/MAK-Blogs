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
Inside an **async fn**, you can use **.await** to wait for the completion of another type that implements the **Future trait**, such as the output of another **async fn**. Unlike **block_on, .await** does not block the current thread, but instead asynchronously waits for the future to complete, allowing other tasks to run if the future is currently unable to make progress.
Lets make an example of learning, singing and dancing. We can sing and dance at a same time but we can not learn and sing at a same time. 
## Example
``` 
use futures::executor::block_on;
use std::thread;
use std::time::Duration;

async fn learn_song() -> String {
thread::sleep(Duration::from_secs(2));
"Love me like you do".to_string()
}

async fn sing_song(song:String) {
thread::sleep(Duration::from_secs(2));
println!("{}", song);
}
async fn dance() {
println!(" Lets dance");
}
async fn learn_and_sing () {
let song = learn_song().await;
sing_song(song).await;
}

async fn audition () {
let f1 = learn_and_sing();
let f2 = dance();
futures::join!(f1, f2);
}

fn main () {
block_on(audition());
} 
```

In this example, learning the song must happen before singing the song, but both learning and singing can happen at the same time as dancing. If we used **block_on(learn_song())** rather than **learn_song().await** in **learn_and_sing**, the thread wouldn't be able to do anything else while **learn_song** was running. This would make it impossible to dance at the same time. By **.await**-ing the **learn_song** future, we allow other tasks to take over the current thread if **learn_song** is blocked. This makes it possible to run multiple futures to completion concurrently on the same thread.

# Conclusion

It's important to remember that traditional threaded applications can be quite effective, and that Rust's small memory footprint and predictability mean that you can get far without ever using **async**. The increased complexity of the **asynchronous programming** model isn't always worth it, and it's important to consider whether your application would be better served by using a simpler threaded model.
