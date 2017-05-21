<h1 align="center">启动器</h1>


## 1 什么是启动器?

启动器是一个在应用启动过程中运行的类,
它是一个推荐的用来写插件并注入到启动过程中的方法。


## 2 用法

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

一个实现了 `IInitializer` 接口并注解了 `@Initialize` 的类就是一个启动器。
在启动器中可以注入 `ApplicationLoader` 来拿到该启动器的应用的设定。

