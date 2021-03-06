---
kind: InterfaceDeclaration
name: OnIdentifyEffects
module: effects
---

# OnIdentifyEffects

## description

Interface to set an identifier for effect instances.

By default, each Effects class is registered
once regardless of how many times the Effect class
is loaded. By implementing this interface, you define
a unique identifier to register an Effects class instance
multiple times.

### Set an identifier for an Effects class

```ts
class EffectWithIdentifier implements OnIdentifyEffects {
constructor(private effectIdentifier: string) {}

ngrxOnIdentifyEffects() {
return this.effectIdentifier;
}

```

```ts
interface OnIdentifyEffects {}
```

[Link to repo](https://github.com/ngrx/platform/blob/master/modules/effects/src/lifecycle_hooks.ts#L29-L35)
