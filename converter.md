<h1 align="center">Converter</h1>

You can manually get converter instance by using `DependencyRegistry`.

```typescript
const converter = DependencyRegistry.get(ConverterService);

const userObj = converter.convert(User, Object);

const user = converter.convert(userObj, User);
```
