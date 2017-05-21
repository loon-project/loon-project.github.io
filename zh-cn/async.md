<h1 align="center">异步</h1>

框架支持通过 `async await` 来实现异步编程，这也是最推荐的方式。

```typescript
class HomeService {

  public async findJob() {
    const result = await somethingAsync();
  }
}
```
