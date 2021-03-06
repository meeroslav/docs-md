---
kind: ClassDeclaration
name: EntityDispatcherBase
module: data
---

# EntityDispatcherBase

## description

Dispatches EntityCollection actions to their reducers and effects (default implementation).
All save commands rely on an Ngrx Effect such as `EntityEffects.persist$`.

```ts
class EntityDispatcherBase<T> implements EntityDispatcher<T> {
  guard: EntityActionGuard<T>;
  toUpdate: (entity: Partial<T>) => Update<T>;

  createEntityAction<P = any>(
    entityOp: EntityOp,
    data?: P,
    options?: EntityActionOptions
  ): EntityAction<P>;
  createAndDispatch<P = any>(
    op: EntityOp,
    data?: P,
    options?: EntityActionOptions
  ): EntityAction<P>;
  dispatch(action: Action): Action;
  add(entity: T, options?: EntityActionOptions): Observable<T>;
  cancel(
    correlationId: any,
    reason?: string,
    options?: EntityActionOptions
  ): void;
  delete(
    arg: number | string | T,
    options?: EntityActionOptions
  ): Observable<number | string>;
  delete(entity: T, options?: EntityActionOptions): Observable<number | string>;
  delete(
    key: number | string,
    options?: EntityActionOptions
  ): Observable<number | string>;
  getAll(options?: EntityActionOptions): Observable<T[]>;
  getByKey(key: any, options?: EntityActionOptions): Observable<T>;
  getWithQuery(
    queryParams: QueryParams | string,
    options?: EntityActionOptions
  ): Observable<T[]>;
  load(options?: EntityActionOptions): Observable<T[]>;
  update(entity: Partial<T>, options?: EntityActionOptions): Observable<T>;
  upsert(entity: T, options?: EntityActionOptions): Observable<T>;
  addAllToCache(entities: T[], options?: EntityActionOptions): void;
  addOneToCache(entity: T, options?: EntityActionOptions): void;
  addManyToCache(entities: T[], options?: EntityActionOptions): void;
  clearCache(options?: EntityActionOptions): void;
  removeOneFromCache(
    arg: (number | string) | T,
    options?: EntityActionOptions
  ): void;
  removeOneFromCache(entity: T, options?: EntityActionOptions): void;
  removeOneFromCache(key: number | string, options?: EntityActionOptions): void;
  removeManyFromCache(
    args: (number | string)[] | T[],
    options?: EntityActionOptions
  ): void;
  removeManyFromCache(entities: T[], options?: EntityActionOptions): void;
  removeManyFromCache(
    keys: (number | string)[],
    options?: EntityActionOptions
  ): void;
  updateOneInCache(entity: Partial<T>, options?: EntityActionOptions): void;
  updateManyInCache(
    entities: Partial<T>[],
    options?: EntityActionOptions
  ): void;
  upsertOneInCache(entity: Partial<T>, options?: EntityActionOptions): void;
  upsertManyInCache(
    entities: Partial<T>[],
    options?: EntityActionOptions
  ): void;
  setFilter(pattern: any): void;
  setLoaded(isLoaded: boolean): void;
  setLoading(isLoading: boolean): void;
}
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L34-L628)

## Properties

| Name                                                      | Type                          | Description                                                       |
| --------------------------------------------------------- | ----------------------------- | ----------------------------------------------------------------- |
| guard                                                     | `EntityActionGuard<T>`        | Utility class with methods to validate EntityAction payloads.     |
| toUpdate                                                  | `(entity: Partial<T>) => any` | Convert an entity (or partial entity) into the `Update<T>` object |
| `update...` and `upsert...` methods take `Update<T>` args |

## Methods

### createEntityAction

#### description (#createEntityAction-description)

Create an {EntityAction} for this entity type.

```ts
createEntityAction<P = any>(  entityOp: EntityOp,  data?: P,  options?: EntityActionOptions ): EntityAction<P>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L84-L95)

#### Parameters (#createEntityAction-parameters)

| Name      | Type                  | Description                     |
| --------- | --------------------- | ------------------------------- |
| entityOp  | `EntityOp`            | {EntityOp} the entity operation |
| [data]    | ``                    | the action data                 |
| [options] | ``                    | additional options              |
| data      | `P`                   |                                 |
| options   | `EntityActionOptions` |                                 |

#### returns (#createEntityAction-returns)

the EntityAction

### createAndDispatch

#### description (#createAndDispatch-description)

Create an {EntityAction} for this entity type and
dispatch it immediately to the store.

```ts
createAndDispatch<P = any>(  op: EntityOp,  data?: P,  options?: EntityActionOptions ): EntityAction<P>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L105-L113)

#### Parameters (#createAndDispatch-parameters)

| Name      | Type                  | Description                     |
| --------- | --------------------- | ------------------------------- |
| op        | `EntityOp`            | {EntityOp} the entity operation |
| [data]    | ``                    | the action data                 |
| [options] | ``                    | additional options              |
| data      | `P`                   |                                 |
| options   | `EntityActionOptions` |                                 |

#### returns (#createAndDispatch-returns)

the dispatched EntityAction

### dispatch

#### description (#dispatch-description)

Dispatch an Action to the store.

```ts
dispatch(action: Action): Action;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L120-L123)

#### Parameters (#dispatch-parameters)

| Name   | Type  | Description |
| ------ | ----- | ----------- |
| action | `any` | the Action  |

#### returns (#dispatch-returns)

the dispatched Action

### add

#### description (#add-description)

Dispatch action to save a new entity to remote storage.

```ts
add(entity: T, options?: EntityActionOptions): Observable<T>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L134-L155)

#### Parameters (#add-parameters)

| Name    | Type                  | Description                                                                          |
| ------- | --------------------- | ------------------------------------------------------------------------------------ |
| entity  | `T`                   | entity to add, which may omit its key if pessimistic and the server creates the key; |
| options | `EntityActionOptions` |                                                                                      |

#### returns (#add-returns)

A terminating Observable of the entity
after server reports successful save or the save error.

### cancel

#### description (#cancel-description)

Dispatch action to cancel the persistence operation (query or save).
Will cause save observable to error with a PersistenceCancel error.
Caller is responsible for undoing changes in cache from pending optimistic save

```ts
cancel(  correlationId: any,  reason?: string,  options?: EntityActionOptions ): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L164-L173)

#### Parameters (#cancel-parameters)

| Name          | Type                  | Description                                           |
| ------------- | --------------------- | ----------------------------------------------------- |
| correlationId | `any`                 | The correlation id for the corresponding EntityAction |
| [reason]      | ``                    | explains why canceled and by whom.                    |
| reason        | `string`              |                                                       |
| options       | `EntityActionOptions` |                                                       |

### delete

```ts
delete(  arg: number | string | T,  options?: EntityActionOptions ): Observable<number | string>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L193-L213)

#### Parameters (#delete-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| arg     | `string               | number      | T` |  |
| options | `EntityActionOptions` |             |

### delete

#### description (#delete-description)

Dispatch action to delete entity from remote storage by key.

```ts
delete(entity: T, options?: EntityActionOptions): Observable<number | string>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L181-L181)

#### Parameters (#delete-parameters)

| Name    | Type                  | Description                             |
| ------- | --------------------- | --------------------------------------- |
| key     | ``                    | The primary key of the entity to remove |
| entity  | `T`                   |                                         |
| options | `EntityActionOptions` |                                         |

#### returns (#delete-returns)

A terminating Observable of the deleted key
after server reports successful save or the save error.

### delete

#### description (#delete-description)

Dispatch action to delete entity from remote storage by key.

```ts
delete(  key: number | string,  options?: EntityActionOptions ): Observable<number | string>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L189-L192)

#### Parameters (#delete-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| key     | `string               | number`     | The entity to delete |
| options | `EntityActionOptions` |             |

#### returns (#delete-returns)

A terminating Observable of the deleted key
after server reports successful save or the save error.

### getAll

#### description (#getAll-description)

Dispatch action to query remote storage for all entities and
merge the queried entities into the cached collection.

```ts
getAll(options?: EntityActionOptions): Observable<T[]>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L222-L242)

#### returns (#getAll-returns)

A terminating Observable of the queried entities that are in the collection
after server reports success query or the query error.

#### see (#getAll-see)

load()

#### Parameters (#getAll-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| options | `EntityActionOptions` |             |

### getByKey

#### description (#getByKey-description)

Dispatch action to query remote storage for the entity with this primary key.
If the server returns an entity,
merge it into the cached collection.

```ts
getByKey(key: any, options?: EntityActionOptions): Observable<T>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L251-L264)

#### returns (#getByKey-returns)

A terminating Observable of the collection
after server reports successful query or the query error.

#### Parameters (#getByKey-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| key     | `any`                 |             |
| options | `EntityActionOptions` |             |

### getWithQuery

#### description (#getWithQuery-description)

Dispatch action to query remote storage for the entities that satisfy a query expressed
with either a query parameter map or an HTTP URL query string,
and merge the results into the cached collection.

```ts
getWithQuery(  queryParams: QueryParams | string,  options?: EntityActionOptions ): Observable<T[]>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L274-L301)

#### Parameters (#getWithQuery-parameters)

| Name        | Type                  | Description  |
| ----------- | --------------------- | ------------ |
| queryParams | `string               | QueryParams` | the query in a form understood by the server |
| options     | `EntityActionOptions` |              |

#### returns (#getWithQuery-returns)

A terminating Observable of the queried entities
after server reports successful query or the query error.

### load

#### description (#load-description)

Dispatch action to query remote storage for all entities and
completely replace the cached collection with the queried entities.

```ts
load(options?: EntityActionOptions): Observable<T[]>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L310-L317)

#### returns (#load-returns)

A terminating Observable of the entities in the collection
after server reports successful query or the query error.

#### see (#load-see)

getAll

#### Parameters (#load-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| options | `EntityActionOptions` |             |

### update

#### description (#update-description)

Dispatch action to save the updated entity (or partial entity) in remote storage.
The update entity may be partial (but must have its key)
in which case it patches the existing entity.

```ts
update(entity: Partial<T>, options?: EntityActionOptions): Observable<T>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L327-L355)

#### Parameters (#update-parameters)

| Name    | Type                  | Description                                                                  |
| ------- | --------------------- | ---------------------------------------------------------------------------- |
| entity  | `Partial<T>`          | update entity, which might be a partial of T but must at least have its key. |
| options | `EntityActionOptions` |                                                                              |

#### returns (#update-returns)

A terminating Observable of the updated entity
after server reports successful save or the save error.

### upsert

#### description (#upsert-description)

Dispatch action to save a new or existing entity to remote storage.
Only dispatch this action if your server supports upsert.

```ts
upsert(entity: T, options?: EntityActionOptions): Observable<T>;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L365-L386)

#### Parameters (#upsert-parameters)

| Name    | Type                  | Description                                                                          |
| ------- | --------------------- | ------------------------------------------------------------------------------------ |
| entity  | `T`                   | entity to add, which may omit its key if pessimistic and the server creates the key; |
| options | `EntityActionOptions` |                                                                                      |

#### returns (#upsert-returns)

A terminating Observable of the entity
after server reports successful save or the save error.

### addAllToCache

#### description (#addAllToCache-description)

Replace all entities in the cached collection.
Does not save to remote storage.

```ts
addAllToCache(entities: T[], options?: EntityActionOptions): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L401-L403)

#### Parameters (#addAllToCache-parameters)

| Name     | Type                  | Description |
| -------- | --------------------- | ----------- |
| entities | `T[]`                 |             |
| options  | `EntityActionOptions` |             |

### addOneToCache

#### description (#addOneToCache-description)

Add a new entity directly to the cache.
Does not save to remote storage.
Ignored if an entity with the same primary key is already in cache.

```ts
addOneToCache(entity: T, options?: EntityActionOptions): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L410-L412)

#### Parameters (#addOneToCache-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| entity  | `T`                   |             |
| options | `EntityActionOptions` |             |

### addManyToCache

#### description (#addManyToCache-description)

Add multiple new entities directly to the cache.
Does not save to remote storage.
Entities with primary keys already in cache are ignored.

```ts
addManyToCache(entities: T[], options?: EntityActionOptions): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L419-L421)

#### Parameters (#addManyToCache-parameters)

| Name     | Type                  | Description |
| -------- | --------------------- | ----------- |
| entities | `T[]`                 |             |
| options  | `EntityActionOptions` |             |

### clearCache

```ts
clearCache(options?: EntityActionOptions): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L424-L426)

#### Parameters (#clearCache-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| options | `EntityActionOptions` |             |

### removeOneFromCache

```ts
removeOneFromCache(  arg: (number | string) | T,  options?: EntityActionOptions ): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L441-L446)

#### Parameters (#removeOneFromCache-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| arg     | `string               | number      | T` |  |
| options | `EntityActionOptions` |             |

### removeOneFromCache

#### description (#removeOneFromCache-description)

Remove an entity directly from the cache.
Does not delete that entity from remote storage.

```ts
removeOneFromCache(entity: T, options?: EntityActionOptions): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L433-L433)

#### Parameters (#removeOneFromCache-parameters)

| Name    | Type                  | Description          |
| ------- | --------------------- | -------------------- |
| entity  | `T`                   | The entity to remove |
| options | `EntityActionOptions` |                      |

### removeOneFromCache

#### description (#removeOneFromCache-description)

Remove an entity directly from the cache.
Does not delete that entity from remote storage.

```ts
removeOneFromCache(key: number | string, options?: EntityActionOptions): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L440-L440)

#### Parameters (#removeOneFromCache-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| key     | `string               | number`     | The primary key of the entity to remove |
| options | `EntityActionOptions` |             |

### removeManyFromCache

```ts
removeManyFromCache(  args: (number | string)[] | T[],  options?: EntityActionOptions ): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L464-L477)

#### Parameters (#removeManyFromCache-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| args    | `(string              | number)[]   | T[]` |  |
| options | `EntityActionOptions` |             |

### removeManyFromCache

#### description (#removeManyFromCache-description)

Remove multiple entities directly from the cache.
Does not delete these entities from remote storage.

```ts
removeManyFromCache(entities: T[], options?: EntityActionOptions): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L453-L453)

#### Parameters (#removeManyFromCache-parameters)

| Name     | Type                  | Description            |
| -------- | --------------------- | ---------------------- |
| entity   | ``                    | The entities to remove |
| entities | `T[]`                 |                        |
| options  | `EntityActionOptions` |                        |

### removeManyFromCache

#### description (#removeManyFromCache-description)

Remove multiple entities directly from the cache.
Does not delete these entities from remote storage.

```ts
removeManyFromCache(  keys: (number | string)[],  options?: EntityActionOptions ): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L460-L463)

#### Parameters (#removeManyFromCache-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| keys    | `(string              | number)[]`  | The primary keys of the entities to remove |
| options | `EntityActionOptions` |             |

### updateOneInCache

#### description (#updateOneInCache-description)

Update a cached entity directly.
Does not update that entity in remote storage.
Ignored if an entity with matching primary key is not in cache.
The update entity may be partial (but must have its key)
in which case it patches the existing entity.

```ts
updateOneInCache(entity: Partial<T>, options?: EntityActionOptions): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L486-L491)

#### Parameters (#updateOneInCache-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| entity  | `Partial<T>`          |             |
| options | `EntityActionOptions` |             |

### updateManyInCache

#### description (#updateManyInCache-description)

Update multiple cached entities directly.
Does not update these entities in remote storage.
Entities whose primary keys are not in cache are ignored.
Update entities may be partial but must at least have their keys.
such partial entities patch their cached counterparts.

```ts
updateManyInCache(  entities: Partial<T>[],  options?: EntityActionOptions ): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L500-L511)

#### Parameters (#updateManyInCache-parameters)

| Name     | Type                  | Description |
| -------- | --------------------- | ----------- |
| entities | `Partial<T>[]`        |             |
| options  | `EntityActionOptions` |             |

### upsertOneInCache

#### description (#upsertOneInCache-description)

Add or update a new entity directly to the cache.
Does not save to remote storage.
Upsert entity might be a partial of T but must at least have its key.
Pass the Update<T> structure as the payload

```ts
upsertOneInCache(entity: Partial<T>, options?: EntityActionOptions): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L519-L521)

#### Parameters (#upsertOneInCache-parameters)

| Name    | Type                  | Description |
| ------- | --------------------- | ----------- |
| entity  | `Partial<T>`          |             |
| options | `EntityActionOptions` |             |

### upsertManyInCache

#### description (#upsertManyInCache-description)

Add or update multiple cached entities directly.
Does not save to remote storage.

```ts
upsertManyInCache(  entities: Partial<T>[],  options?: EntityActionOptions ): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L527-L535)

#### Parameters (#upsertManyInCache-parameters)

| Name     | Type                  | Description |
| -------- | --------------------- | ----------- |
| entities | `Partial<T>[]`        |             |
| options  | `EntityActionOptions` |             |

### setFilter

#### description (#setFilter-description)

Set the pattern that the collection's filter applies
when using the `filteredEntities` selector.

```ts
setFilter(pattern: any): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L541-L543)

#### Parameters (#setFilter-parameters)

| Name    | Type  | Description |
| ------- | ----- | ----------- |
| pattern | `any` |             |

### setLoaded

```ts
setLoaded(isLoaded: boolean): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L546-L548)

#### Parameters (#setLoaded-parameters)

| Name     | Type      | Description |
| -------- | --------- | ----------- |
| isLoaded | `boolean` |             |

### setLoading

```ts
setLoading(isLoading: boolean): void;
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/data/src/dispatchers/entity-dispatcher-base.ts#L551-L553)

#### Parameters (#setLoading-parameters)

| Name      | Type      | Description |
| --------- | --------- | ----------- |
| isLoading | `boolean` |             |
