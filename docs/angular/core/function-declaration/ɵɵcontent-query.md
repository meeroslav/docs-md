---
kind: FunctionDeclaration
name: ɵɵcontentQuery
module: core
---

# ɵɵcontentQuery

## description

Registers a QueryList, associated with a content query, for later refresh (part of a view
refresh).

```ts
function ɵɵcontentQuery<T>(
  directiveIndex: number,
  predicate: Type<any> | InjectionToken<unknown> | string[],
  descend: boolean,
  read?: any
): void;
```

[Link to repo](https://github.com/timdeschryver/angular/blob/master/packages/core/src/render3/query.ts#L500-L506)

## Parameters

| Name           | Type       | Description                             |
| -------------- | ---------- | --------------------------------------- |
| directiveIndex | `number`   | Current directive index                 |
| predicate      | `Type<any> | InjectionToken<unknown>                 | string[]` | The type for which the query will search |
| descend        | `boolean`  | Whether or not to descend into children |
| read           | `any`      | What to save in the query               |

## returns

QueryList<T>

## codeGenApi
