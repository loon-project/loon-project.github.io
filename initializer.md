<h1 align="center">Initializer</h1>


## 1 What Does a Initializer Do?

An initializer is a class runs before application boot up,
it's the recommended way to write plugin and hook to framework.


## 2 Usage

```typescript
@Initialize()
class ServiceInitializer implements IInitializer {

    @Inject()
    private application: ApplicationLoader;

    public init() {
      // hook to framework
    }

}
```

A class decorated Initialize and implements interface `IInitializer` considered as a initializer.
You can get application settings by inject `ApplicationLoader`.

