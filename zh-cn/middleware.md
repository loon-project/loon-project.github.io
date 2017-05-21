<h1 align="center">中间件</h1>


## 1 什么是中间件

中间件就是一个应用在路由或者应用级别的一个方法。

<b>它有和controller action 相同的处理 [参数](zh-cn/controller.md) 的能力。</b>


## 2 中间件

```typescript
@Middleware({order: 0})
class ELKMiddleware implements IMiddleware {

  public use(@Next() next: Express.NextFunction) {
    // start tracking
    next()
    // end tracking
    // send to elk
  }
}
```
注解了 `@Middleware` 并且实现了 `IMiddleware` 的类自动会识别并加载为 `Middleware`。
在这个例子中, `ELKMiddleware` 应该是第一个运行的中间件，
只要在参数中添加 `{order: 0}` 就能保证是第一个运行的。

中间件也支持 `baseUrl` 来使它应用在该路由上。
```typescript
@Middleware({baseUrl: '/admin'})
class AdminMiddleware implements IMiddleware {}
```
这个例子显示了给 `/admin` 路由添加中间件。


## 3 错误中间件

`@ErrorMiddleware` 用法和 `@Middleware` 基本一样, 有一个不同点是你能通过 `@Err` 拿到错误 Object。

```typescript
@ErrorMiddleware()
class ExceptionHandler implements IMiddleware {

  public use(@Err() err: any, @Res() res: Express.Response) {
    console.log(err.message);
    res.sendStatus(400);
  }
}
```
