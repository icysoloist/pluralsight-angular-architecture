# Angular Architecture

| Section | Description |
| :- |:- |
| [**Organization**](#Organization) | Organizing DRY code, features, and folders. |

## Organization

### Core directory
Core folder should contain singleton services shared throughout app.
- LoggingService, ErrorService, AuthService, etc...
- Components who only receive content projection can live here too.

Preventing the reimport of Core
> @Optional(): marks dependency as optional.  
> @SkipSelf(): tells the DI system that dependency resolution should start from the parent injector.
```typescript
export class EnsureModuleLoadedOnceGuard {
  constructor(targetModule: any) {
    if (targetModule) {
      throw new Error(`${targetModule.constructor.name} already loaded`);
    }
  }
}

export class CoreModule extends EnsureModuleLoadedOnceGuard {
  constructor(@Optional() @SkipSelf() parentModule: CoreModule) {
    super(parentModule);
  }
}   
```

### OnPush change detection
Causes change detection to run when:
1) An input property reference changes
2) An EventEmitter (output property) or DOM event fires
3) Async pipe receives an event
4) Change detection is manually invoked

> In other words, the framework will check the component if its input properties change, it fires an event, or an Observable emits
