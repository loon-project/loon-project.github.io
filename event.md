<h1 align="center">Event</h1>

## 1 What Does an Event Do?

Event is the core concept of Nodejs, highly recommended to read [Nodejs documentation](https://nodejs.org/api/events.html#events_events)
first, and then use this api.

## 2 Usage

```typescript

// define an event emitter

@Service()
class CustomEventEmitter extends EventEmitter {}

// define an event subscriber

@Subscriber()
class CustomEventSubscriber {

  private property = "a property";

  @On('newEvent', CustomEventEmitter)
  public onEventEmit() {
    // `this` is the subscriber not event emitter

    this.property === "a property" // => true
  }
}

const emitter = DependencyRegistry.get(CustomEventEmitter);

emitter.emit('newEvent');
```
When emitter emit an event, the subscriber listener will trigger automatically.
<b>IMPORTANT: the `this` in the listener is the subscriber itself.</b>


