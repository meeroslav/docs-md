---
kind: FunctionDeclaration
name: timer
module: src
---

# timer

## description

Creates an observable that emits incrementing numbers, starting at `0`, over time.

<span class="informal">Its like {@link index/interval}, but you can specify when
should the emissions start (either by delay or exact date).</span>

![](timer.png)

Will return an observable that emits incrementing numbers over time, starting a `0`,
or just a single number `0` after a specified delay or start time.

If no scheduler is provided, {@link index/asyncScheduler} will be the assumed default.

When the first value of `0` is emitted is determined by a calculation based off of the
`dueTime`:

1. If the `dueTime` is a _valid_ `Date` object, the first emission will be after a delay
   calculated by subtracting the current timestamp (provided by the scheduler), from the numeric
   value of the `Date`, which will be an epoch number in milliseconds.
2. If the `dueTime` is a number, the first emission will be after a delay equal to that number.
3. If the result of either 1 or 2 above is negative, that is the `dueTime` is a `Date` in the
   past, OR a negative number, the result is the first emission will be scheduled after a delay
   of `0`.

When subsequent values are emitted is determined by the `period` argument:

1. If their is no `period` argument, there will be no further emissions.
2. If the `period` argument is `0`, each new emission will be scheduled with a delay of `0`.
3. If the `period` argument is a postive number, each new emission will be scheduled with a
   delay equal to that number.
4. If the `period` argument is negative, there will be no further emissions.

**Known Issues**:

If a `scheduler` is provided that returns a timsstamp other than an epoch from `now()`, and
a `Date` object is passed to the `dueTime` argument, the calculation for when the first emission
should occur will be incorrect. In this case, it would be best to do your own calculations
ahead of time, and pass a `number` in as the `dueTime`.

Once the first value is emitted, if a `period` was provided

## Examples

### Wait 3 seconds and start another observable

You might want to use `timer` to delay subscription to an
observable by a set amount of time. Here we use a timer with
{@link concatMapTo} or {@link concatMap} in order to wait
a few seconds and start a subscription to a source.

```ts
import { timer, of } from "rxjs";
import { concatMapTo } from "rxjs/operators";

// This could be any observable
const source = of(1, 2, 3);

const result = timer(3000).pipe(concatMapTo(source)).subscribe(console.log);
```

### Take all of the values until the start of the next minute

Using the a date as the trigger for the first emission, you can
do things like wait until midnight to fire an event, or in this case,
wait until a new minute starts (chosen so the example wouldn't take
too long to run) in order to stop watching a stream. Leveraging
{@link takeUntil}.

```ts
import { interval, timer } from "rxjs";
import { takeUntil } from "rxjs/operators";

// Build a Date object that marks the
// next minute.
const currentDate = new Date();
const startOfNextMinute = new Date(
  currentDate.getFullYear(),
  currentDate.getMonth(),
  currentDate.getDate(),
  currentDate.getHours(),
  currentDate.getMinutes() + 1
);

// This could be any observable stream
const source = interval(1000);

const result = source.pipe(takeUntil(timer(startOfNextMinute)));

result.subscribe(console.log);
```

### Start an interval that starts right away

Since {@link index/interval} waits for the passed delay before starting,
sometimes that's not ideal. You may want to start an interval immediately.
`timer` works well for this. Here we have both side-by-side so you can
see them in comparison.

```ts
import { timer, interval } from "rxjs";

timer(0, 1000).subscribe((n) => console.log("timer", n));
interval(1000).subscribe((n) => console.log("interval", n));
```

```ts
function timer(
  dueTime: number | Date = 0,
  periodOrScheduler?: number | SchedulerLike,
  scheduler?: SchedulerLike
): Observable<number>;
```

[Link to repo](https://github.com/ReactiveX/rxjs/blob/master/src/internal/observable/timer.ts#L131-L163)

## see

{@link index/interval}
{@link delay}

## Parameters

| Name              | Type            | Description                                     |
| ----------------- | --------------- | ----------------------------------------------- |
| dueTime           | `number         | Date`                                           | The initial delay time specified as a Date object or as an integer denoting |
| periodOrScheduler | `number         | SchedulerLike`                                  | The period of time between emissions of the |
| scheduler         | `SchedulerLike` | The {@link SchedulerLike} to use for scheduling |

## return

An Observable that emits a `0` after the
`dueTime` and ever increasing numbers after each `period` of time
thereafter.
