# AbortController

You can use AbortController to:

- abort one or more Web requests;
- abort a request with a timeout.

You can create a new AbortController object using the AbortController() constructor, passing a AbortSignal object to fetch to
abort a request:

```javascript
let abortController;

function download() {
  abortController = new AbortController();
  const { signal } = abortController;

  fetch("download", {
    signal,
  })
    .then(() => {})
    .catch((err) => {
      console.error(err);
    });
}

function onCancel() {
  if (abortController) {
    abortController.abort("");
  }
}
```

Cancel a request with timeout

```javascript
/**
 * fetch with timeout
 * @param {number} timeout
 */
async function getAnswer(timeout) {
  try {
    const res = fetch("api", {
      // other options
      signal: AbortSignal.timeout(timeout),
    });
    // do something
  } catch (err) {
    if (err.name === "TimeoutError") {
      // handle timeout error
    } else {
      // other
    }
  }
}
```

or both (not recommended, check [caniuse AbortSignal.any](https://caniuse.com/?search=AbortSignal.any))

```javascript
function download() {
  const abortController = new AbortController();

  fetch("download", {
    signal: AbortSignal.any([
      abortController.signal,
      AbortSignal.timeout(5000),
    ]),
  })
    .then(() => {})
    .catch((err) => {
      console.error(err);
    });
}
```
