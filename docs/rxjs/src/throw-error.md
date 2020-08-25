---
kind: FunctionDeclaration
name: throwError
module: src
---

# throwError

```ts
function throwError(
  errorOrErrorFactory: any,
  scheduler?: SchedulerLike
): Observable<never>;
```

[Link to repo](https://github.com/ReactiveX/rxjs/blob/master/src/internal/observable/throwError.ts#L122-L134)

## Parameters

| Name                | Type            | Description |
| ------------------- | --------------- | ----------- |
| errorOrErrorFactory | `any`           |             |
| scheduler           | `SchedulerLike` |             |

## Overloads

### description

Creates an observable that will create an error instance and push it to the consumer as an error
immediately upon subscription.

<span class="informal">Just errors and does nothing else</span>

![](throw.png)

This creation function is useful for creating an observable that will create an error and error every
time it is subscribed to. Generally, inside of most operators when you might want to return an errored
observable, this is unnecessary. In most cases, such as in the inner return of {@link concatMap},
{@link mergeMap}, {@link defer}, and many others, you can simply throw the error, and RxJS will pick
that up and notify the consumer of the error.

## Example

Create a simple observable that will create a new error with a timestamp and log it
and the message every time you subscribe to it.

```ts
import { throwError } from "rxjs";

let errorCount = 0;

const errorWithTimestamp$ = throwError(() => {
  const error: any = new Error(`This is error number ${++errorCount}`);
  error.timestamp = Date.now();
  return error;
});

errorWithTimesptamp$.subscribe({
  error: (err) => console.log(err.timestamp, err.message),
});

errorWithTimesptamp$.subscribe({
  error: (err) => console.log(err.timestamp, err.message),
});

// Logs the timestamp and a new error message each subscription;
```

## Unnecessary usage

Using `throwError` inside of an operator or creation function
with a callback, is usually not necessary:

```ts
import { throwError, timer, of } from "rxjs";
import { concatMap } from "rxjs/operators";

const delays$ = of(1000, 2000, Infinity, 3000);

delays$
  .pipe(
    concatMap((ms) => {
      if (ms < 10000) {
        return timer(ms);
      } else {
        // This is probably overkill.
        return throwError(() => new Error(`Invalid time ${ms}`));
      }
    })
  )
  .subscribe({
    next: console.log,
    error: console.error,
  });
```

You can just throw the error instead:

```ts
import { throwError, timer, of } from "rxjs";
import { concatMap } from "rxjs/operators";

const delays$ = of(1000, 2000, Infinity, 3000);

delays$
  .pipe(
    concatMap((ms) => {
      if (ms < 10000) {
        return timer(ms);
      } else {
        // Cleaner and easier to read for most folks.
        throw new Error(`Invalid time ${ms}`);
      }
    })
  )
  .subscribe({
    next: console.log,
    error: console.error,
  });
```

```ts
function throwError(errorFactory: () => any): Observable<never>;
```

[Link to repo](https://github.com/ReactiveX/rxjs/blob/master/src/internal/observable/throwError.ts#L100-L100)

### Parameters

| Name         | Type        | Description                                                            |
| ------------ | ----------- | ---------------------------------------------------------------------- |
| errorFactory | `() => any` | A factory function that will create the error instance that is pushed. |

### description

Returns an observable that will error with the specified error immediately upon subscription.

```ts
function throwError(error: any): Observable<never>;
```

[Link to repo](https://github.com/ReactiveX/rxjs/blob/master/src/internal/observable/throwError.ts#L110-L110)

### Parameters

| Name  | Type  | Description                |
| ----- | ----- | -------------------------- |
| error | `any` | The error instance to emit |

### deprecated

Removed in v8. Instead, pass a factory function to `throwError(() => new Error('test'))`. This is
because it will create the error at the moment it should be created and capture a more appropriate stack trace. If
for some reason you need to create the error ahead of time, you can still do that: `const err = new Error('test'); throwError(() => err);`.

### description

Notifies the consumer of an error using a given scheduler by scheduling it at delay `0` upon subscription.

```ts
function throwError(
  errorOrErrorFactory: any,
  scheduler: SchedulerLike
): Observable<never>;
```

[Link to repo](https://github.com/ReactiveX/rxjs/blob/master/src/internal/observable/throwError.ts#L120-L120)

### Parameters

| Name                | Type            | Description                                           |
| ------------------- | --------------- | ----------------------------------------------------- |
| errorOrErrorFactory | `any`           | An error instance or error factory                    |
| scheduler           | `SchedulerLike` | A scheduler to use to schedule the error notification |

### deprecated

Use `throwError` in combination with {@link observeOn}:
`throwError(() => new Error('test')).pipe(observeOn(scheduler));`