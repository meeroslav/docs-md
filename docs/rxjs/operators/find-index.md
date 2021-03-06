---
kind: FunctionDeclaration
name: findIndex
module: operators
---

# findIndex

## description

Emits only the index of the first value emitted by the source Observable that
meets some condition.

<span class="informal">It's like {@link find}, but emits the index of the
found value, not the value itself.</span>

![](findIndex.png)

`findIndex` searches for the first item in the source Observable that matches
the specified condition embodied by the `predicate`, and returns the
(zero-based) index of the first occurrence in the source. Unlike
{@link first}, the `predicate` is required in `findIndex`, and does not emit
an error if a valid value is not found.

## Example

Emit the index of first click that happens on a DIV element

```ts
import { fromEvent } from "rxjs";
import { findIndex } from "rxjs/operators";

const clicks = fromEvent(document, "click");
const result = clicks.pipe(findIndex((ev) => ev.target.tagName === "DIV"));
result.subscribe((x) => console.log(x));
```

```ts
function findIndex<T>(
  predicate: (value: T, index: number, source: Observable<T>) => boolean,
  thisArg?: any
): OperatorFunction<T, number>;
```

[Link to repo](https://github.com/ReactiveX/rxjs/blob/master/src/internal/operators/findIndex.ts#L44-L47)

## see

{@link filter}
{@link find}
{@link first}
{@link take}

## Parameters

| Name             | Type                                                          | Description                                                     |
| ---------------- | ------------------------------------------------------------- | --------------------------------------------------------------- |
| {function(value: | ``                                                            | T, index: number, source: Observable<T>): boolean} predicate    |
| {any}            | ``                                                            | [thisArg] An optional argument to determine the value of `this` |
| predicate        | `(value: T, index: number, source: Observable<T>) => boolean` |                                                                 |
| thisArg          | `any`                                                         |                                                                 |

## return

{Observable} An Observable of the index of the first item that
matches the condition.

## name

find
