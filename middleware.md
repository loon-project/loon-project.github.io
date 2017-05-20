<h1 align="center">Middleware</h1>


## 1 What Does a Middleware Do?

Middleware is a function runs before controller dividing by route.

<b>It have the same [parameter](controller.md) ability as the controller action.</b>


## 2 Middleware

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
The class decorated by `@Middleware` will automatically wire to application.
In this example, `ELKMiddleware` should run as the first middleware,
just add an option `{order: 0}` to ensure it run first.

Middleware also support `baseUrl` options to apply middleware to a group of routes
```typescript
@Middleware({baseUrl: '/admin'})
class AdminMiddleware implements IMiddleware {}
```
The example shows add a middleware for the admin routes.


## 3 ErrorMiddleware

`@ErrorMiddleware` have the same usage with `@Middleware`, one different is that in the middleware use function,
 you can get the error object by using `@Err` decorator.

```typescript
@ErrorMiddleware()
class ExceptionHandler implements IMiddleware {

  public use(@Err() err: any, @Res() res: Express.Response) {
    console.log(err.message);
    res.sendStatus(400);
  }
}
```
