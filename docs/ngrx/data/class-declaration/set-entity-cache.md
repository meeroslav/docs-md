---
kind: ClassDeclaration
name: SetEntityCache
module: data
---

# SetEntityCache

## description

Create entity cache action for replacing the entire entity cache.
Dangerous because brute force but useful as when re-hydrating an EntityCache
from local browser storage when the application launches.

```ts
class SetEntityCache implements Action {
  readonly payload: { cache: EntityCache; tag?: string };
  readonly type = EntityCacheAction.SET_ENTITY_CACHE;
}
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/actions/entity-cache-action.ts#L108-L115)

## Parameters

| Name  | Type | Description                                                      |
| ----- | ---- | ---------------------------------------------------------------- |
| cache | ``   | New state of the entity cache                                    |
| [tag] | ``   | Optional tag to identify the operation from the app perspective. |

## Properties

| Name    | Type                                    | Description |
| ------- | --------------------------------------- | ----------- |
| payload | `{ cache: EntityCache; tag?: string; }` |             |
| type    | `EntityCacheAction.SET_ENTITY_CACHE`    |             |
