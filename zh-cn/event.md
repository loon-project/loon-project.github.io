<h1 align="center">事件</h1>

## 1 什么是事件

事件是 Nodejs 的核心概念，特别建议在使用这个api之前先阅读[Nodejs 文档](https://nodejs.org/api/events.html#events_events)。


## 2 用法

```typescript

// 定义一个事件发布者

@Service()
class CustomEventEmitter extends EventEmitter {}

// 定义一个事件订阅者

@Subscriber()
class CustomEventSubscriber {

  private property = "a property";

  // 用 `On` 注册了 `newEvent` 事件的回调
  @On('newEvent', CustomEventEmitter)
  public onEventEmit() {
    this.property === "a property" // => true
  }
}

const emitter = DependencyRegistry.get(CustomEventEmitter);

emitter.emit('newEvent');
```
当事件发布者发布一个消息时，事件订阅者马上会触发注册的方法。
<b>重要: 在订阅者方法中的`this`是订阅者本身</b>


