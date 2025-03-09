# Async IO in Python
Concurrency does not always imply parallel execution on multiple cores. Instead, it suggests that tasks can overlap in their execution, allowing the program to make progress on different tasks while others are waiting for I/O or other events.

Core concepts of `asyncio`: event loops, coroutines, and tasks.

## Event Loop
It is the central orchestrator of asynchronous operations, effectively distributing execution among different tasks. It continuously monitors events, such as I/O operations, timer expirations, and network events. When an event occurs, the event loop wakes up the corresponding task, allowing it to resume execution. This continuous monitoring and scheduling of tasks is what enables `asyncio` to achieve concurrency within a single thread.

`asyncio` offers different event loop implementations based on the platform. The `SeletorEventLoop`, based on the `selectors` module, is the default event loop on most platforms. It utilizes an efficient I/O multiplexing mechanism to monitor multiple I/O events simultaneously.  `asyncio.run(main())` gets the event loop, runs the tasks until they are marked as complete, and then closes the event loop.

## Coroutines
They are specialized generator functions designed for asynchronous programming, defined using `async def` syntax and can be paused and resumed at specific points, marked by `await`. When a coroutine encounters an `await` expression, it temporarily suspends its execution and yields control back to the event loop, which allows other coroutines to run while the awaiting coroutine awaits for an I/O operation or another asynchronous task to complete.

## Tasks
They provide a higher-level abstraction for managing coroutines in `asyncio`. A task is essentially a wrapped coroutine that is scheduled to run by the event loop. When you create a task using `asyncio.create_task()`, the event loop takes charge of its execution, ensuring that it runs concurrently with other tasks. Tasks offer additional functionalities, such as cancellation and the ability to retrieve the result of the coroutine.

**Parallelism** consists of performing multiple operations at the same time. **Multiprocessing** is a means to effect parallelism, and it entails spreading tasks over a computer's CPU cores. **Concurrency** suggests that multiple tasks have the ability to run in an overlapping manner. **Threading** is a concurrent execution model whereby multiple threads take turns executing tasks. One process can contain multiple threads. Threading is better for IO-bound tasks.

Concurrency encompasses both multiprocessing and threading.
> Multiprocessing is a form of parallelism, with parallelism being a specific type of concurrency.

The concept of async IO does not mean threading or multiprocessing. It is not built on top of either of these. In fact, async IO  is a single-threaded, single-process design: it uses **cooperative multitasking**. It simply gives a feeling of concurrency despite using a single thread in a single process.

Async IO is a style of concurrent programming, but it is not parallelism. But what does it mean for something to be **asynchronous**? Async routines are able to "pause" while waiting on their ultimate result and let other routines run in the meantime.

Cooperative multitasking means that a program's event loop communicates with multiple tasks to let each take turns running at the optimal time. Async IO takes long waiting periods in which functions would otherwise be blocking and allows other functions to run during that downtime.


## Rules of Async IO
1. The syntax `async def ` introduces either a **native coroutine** or an **asynchronous generator**. The expressions `async with` and `async for` are also valid.
2. The keyword `await` passes function control back to the event loop, by suspending the execution of the surrounding coroutine. If Python encounters an `await f()` in the scope of `g()` , `await` tells the event loop : "Suspend execution of `g()` until  whatever I'm waiting on--the result of `f()` is returned. In the meantime, go let something else run."

```py
async def g():
    # Pause here and come back to g() when f() is ready
    r = await f()
    return r
```


### Rules of using `async/await`

1. A function that you introduce with `async def` is a coroutine. It may use `await`, `return`, or `yield`, but all of these are optional. Declaring `async def noop(): pass` is valid.
	1. Using `await` and/or `return` creates a coroutine function. To call a coroutine function, you must `await` it to get its results.
	2. It is less common to use `yield` in `async def` block
	3. Anything defined with `async def` may not use `yield from`, which will raise a syntax error.
2. It is a syntax error to use `await` outside of an `async def` coroutine. You can only use `await` in the body of coroutines.

```py
async def f(x):
    y = await z(x)  # OK - `await` and `return` allowed in coroutines
    return y

async def g(x):
    yield x  # OK - this is an async generator

async def m(x):
    yield from gen(x)  # No - SyntaxError

def m(x):
    y = await z(x)  # Still no - SyntaxError (no `async def` here)
    return y

```
