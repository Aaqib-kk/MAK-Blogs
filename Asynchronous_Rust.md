# Asynchronous Rust Programming

As we already know, **Rust** allows us to write fast, safe software programming. But why write asynchronous code?

*** Asynchronous code allows us to run multiple task on a single thread. Overall, asynchronous code have the potential to be much faster and user fewer resources than a corresponding threaded implementation. However, there is a cost. Threads are naively supported by the operating system, and using them does not require any special programming model, any function can create a thread and calling a function that uses threads is usually as simple as calling any normal function. However, asynchronous function requires special support from the language or libraries. In Rust,  **async fn** creates an asynchronous function which returns a **Future**.
