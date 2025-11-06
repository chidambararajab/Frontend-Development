# Async JS: Demystifying the Event Loop, Microtasks & Macrotasks

## JavaScript: A Synchronous, Single-Threaded Language

Have you ever wondered how JavaScript works behind the scenes? At its core, JavaScript is a synchronous, single-threaded language. What does that mean? It means JavaScript can handle only one task at a time using a single call stack.

Think of the call stack as a to-do list where JavaScript adds tasks and completes them one by one, from top to bottom. Its job is to execute all JavaScript code in order, without waiting for anything. As they say: "Time, tide, and JavaScript wait for none." 😄

For example, when you run this simple code:

```jsx
console.log("First");
console.log("Second");
console.log("Third");
```

JavaScript executes them exactly in that order – First, Second, Third – with no delays or interruptions.

## Handling Delays in JavaScript

But wait a minute... what if we need to wait for something? What if we want a program to run after 5 seconds? Can JavaScript do that?

The call stack doesn't have a built-in timer – it executes everything immediately. So how do we handle delays? This is where the superpowers of WebAPIs come in!

Some common WebAPIs include:

1. **Timer** - `setTimeout()`, `setInterval()`
2. **DOM APIs** - `document.getElementById("test")`
3. **fetch()** - for making API calls
4. **Local storage** - for storing data in the browser
5. **Console** - for logging messages
6. **Location** - for working with URLs

All these WebAPIs live in the global `window` object, so you can call them using `window.setTimeout()` or just `setTimeout()`.

## How JavaScript Handles Async Operations

Let's break down how JavaScript manages asynchronous operations step by step:

1. JavaScript starts by executing synchronous code, pushing function calls onto the call stack.
2. When it encounters an asynchronous operation (like `setTimeout`, `fetch`, or an event listener), it sends it to the Web APIs (provided by the browser or Node.js).
3. Once the async operation completes, the callback function is placed in the Callback queue (also called the macrotask queue).
4. The event loop checks if the call stack is empty before moving the next task from the task queue to the call stack.

Let's see this in action with a simple example:

```jsx
console.log("Start");
setTimeout(() => console.log("Executed me after 5 seconds"), 5000);
console.log("End");
```

```jsx
Start
End
Executed me after 5 seconds
```

But why does it work this way? Let's break it down:

1. `console.log("Start")` runs first because it's synchronous code.
2. When JavaScript sees `setTimeout()`, it doesn't wait! It hands off the timer to the WebAPI and continues to the next line. The WebAPI starts the 5-second countdown and registers the callback function for later.
3. `console.log("End")` runs next.
4. After 5 seconds pass, the WebAPI puts our callback function into the macrotask queue.
5. The event loop checks if the call stack is empty, and if it is, it pushes our callback function into the call stack.
6. Finally, `console.log("Executed me after 5 seconds")` is executed.

It's like ordering food at a restaurant – you place your order (async operation) and then do other things while the kitchen (WebAPI) prepares your meal. The waiter (event loop) brings your food (callback) when it's ready and you're not busy with something else.

## Understanding the Event Loop

The event loop is the secret sauce that makes asynchronous JavaScript possible. It continuously monitors both the call stack and various task queues, ensuring everything runs in the right order.

Think of the event loop as a vigilant security guard who constantly checks if the call stack (the VIP area) is empty. When it is, the guard allows the next person from the waiting line (task queue) to enter.

### Why Do We Need the Macrotask Queue?

You might wonder: "Why not push callback functions directly onto the call stack once they're ready?"

Let's understand this with an event listener example:

```jsx
console.log("Start");

document.getElementById("submit-btn").addEventListener("click", function cb() {
  console.log("callback");
});

console.log("End");
```

Here's what happens:

1. `console.log("Start")` executes immediately.
2. The event listener gets registered for the submit button, but the callback function isn't executed yet – it's just stored in the browser's memory.
3. `console.log("End")` executes immediately.
4. The call stack is now empty.
5. When the user clicks the button:
    - The browser detects the click event
    - The callback function goes to the macrotask queue, not directly to the call stack
    - The event loop checks if the call stack is empty
    - If it's empty, the event loop moves the callback to the call stack
    - The callback executes, and we see "callback" in the console

This queue system ensures JavaScript doesn't interrupt itself while executing other code. It's like taking a number at a busy office – you wait your turn rather than barging in while someone else is being served.

## The Fetch API and Microtasks

Now, let's see how the `fetch` API works – it's a bit different!

```jsx
console.log("Start");

setTimeout(() => console.log("Executed me after 5 seconds"), 5000);

fetch("https://api.netflix.com").then(() => {
  console.log("Success Response of Netflix API");
});

*// Let's assume we have 1000 lines of code that take 10 seconds to execute*

console.log("End");
```

Expected Output:

```jsx
Start
End
Success Response of Netflix API
Executed me after 5 seconds
```

Wait, why does the fetch response come before the setTimeout, even though the timeout is only 5 seconds and our code takes 10 seconds to run? This brings us to an important distinction: microtasks vs. macrotasks!

Here's what's happening:

1. `console.log("Start")` runs (synchronous code).
2. The `setTimeout` is handed off to the WebAPI, which starts the 5-second timer.
3. The `fetch` request is sent to the WebAPI to make the network call.
4. Our imaginary 1000 lines of code execute for 10 seconds.
5. Meanwhile, both the timer and API call complete, but their callbacks go to different queues:
    - The timer callback goes to the macrotask queue
    - The fetch promise callback goes to the microtask queue
6. `console.log("End")` runs.
7. The call stack is now empty.
8. The event loop first processes all microtasks (our fetch callback) before any macrotask.
9. Finally, it processes the macrotask (our setTimeout callback).

## Microtasks vs. Macrotasks

Asynchronous operations in JavaScript fall into two categories: microtasks and macrotasks.

### Microtasks

Microtasks are high-priority asynchronous operations that execute immediately after the current script completes, before any macrotask. They're like VIP guests who get priority access!

Examples of microtasks:

- Promises (`.then()`, `.catch()`, `.finally()`)
- `MutationObserver` (used to detect DOM changes)
- `queueMicrotask()`

### Macrotasks

Macrotasks execute after all microtasks are complete. They're related to the browser or Node.js environment.

Examples of macrotasks:

- `setTimeout`, `setInterval`
- `setImmediate` (Node.js)
- Event listeners (click, scroll, etc.)

### Execution Order

The JavaScript engine follows this sequence:

1. Execute all synchronous code (what's in the call stack)
2. Process ALL microtasks in the microtask queue
3. Execute ONE macrotask from the macrotask queue
4. Go back to step 2 (process all new microtasks)
5. Continue this cycle

It's like a restaurant that always serves all its VIP customers (microtasks) before serving even one regular customer (macrotask).

## Understanding Task Starvation

What happens if microtasks keep creating more microtasks? The macrotask queue might never get a chance to execute its tasks! This situation is called "task starvation" – where tasks in the Callback Queue are "starved" of execution time.

Imagine being in line at a store, but the VIP line keeps getting new people who are always served before you. You might end up waiting forever!

## Key Takeaways

1. JavaScript runs code synchronously, but async operations are handled using the event loop.
2. WebAPIs handle operations that take time (timers, network requests, events).
3. Microtasks execute before macrotasks, making them high-priority.
4. The event loop constantly checks if the call stack is empty before moving tasks from the queues.
5. Understanding this execution order helps write better asynchronous code.

So next time you're writing asynchronous JavaScript, remember the journey your code takes through the call stack, WebAPIs, task queues, and the event loop. It's this elegant dance that makes JavaScript feel responsive and powerful despite being single-threaded! 🚀

Happy coding! 😎