# Refactor Promises to Async Await

## Running the project

To run the project:

```bash
npm run start
```

## Running Tests

Run tests using the command:

```bash
$ npm run test-watch
```

<hr />

# Notes on Promises and Async Await

- [Promises](#promises)
  - [The Executor Function](#the-executor-function)
  - [The Consuming Function](#the-consuming-function)

## Promises

### The Executor function

```
let promise = new Promise(
    function(resolve, reject) {}
)
```

- `param` of Promise - a function called the executor
  - the executor will run automatically and will eventually produce the result
    - In the example of loading an image, the executor will be the function that loads the image
- `resolve` and `reject` are callbacks provided by JS
  - `resolve(value)` or `reject(error)`
- the executor function should call one of the callbacks (`reject` or `resolve`) --> this will change the state of the Promise
  - `resolve(value)` changes the promise to `{state:'fulfilled', result: value}`
  - `reject(error)` changes the promise to `{state:'rejected', result: error}`

```
let promise = new Promise(function(resolve, reject) {
  // the function is executed automatically when the promise is constructed

  // after 1 second signal that the job is done with the result "done"
  setTimeout(() => resolve("done"), 1000);
});
```

```
let promise = new Promise(function(resolve, reject) {
  // after 1 second signal that the job is finished with an error
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});
```

### The Consuming Function

- consuming functions can be registered using `.then`, `.catch`, `.finally`
  - to register a function, means that it is listening/waiting on the executor function

#### `.then`

- has 2 parameters, both are callback functions

```
promise.then(
  function(result) { /* handle a successful result */ },
  function(error) { /* handle an error */ }
);
```

```
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done!"), 1000);
});

// resolve runs the first function in .then
promise.then(
  result => alert(result), // shows "done!" after 1 second
  error => alert(error) // doesn't run
);
```

- if we're only interested in a successful result, we can leave off the second argument
- if we're only interested in an error, we provide null as the first argument

#### `.catch`

- if we're only interested in an error, we can alternatively use the `.catch`

```
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

// .catch(f) is the same as promise.then(null, f)
promise.catch(alert); // shows "Error: Whoops!" after 1 second
```

#### `.finally`

- the function provided to finally runs regardless of error or success
- finally is a good handler for performing cleanup, e.g. stopping our loading indicators, as they are not needed anymore, no matter what the outcome is.

```
new Promise((resolve, reject) => {
  // do something that takes time, and then call resolve/reject
})
  // runs when the promise is settled, regardless of outcome
  .finally(() => stop loading indicator)
  .then(result => show result, err => show error)
```
