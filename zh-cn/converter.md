<h1 align="center">转化器</h1>

你可以通过 `DependencyRegistry` 拿到转化器的实例。

```typescript
const converter = DependencyRegistry.get(ConverterService);

const userObj = converter.convert(User, Object);

const user = converter.convert(userObj, User);
```
