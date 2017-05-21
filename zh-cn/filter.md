<h1 align="center">过滤器</h1>


## 1 什么是过滤器

过滤器就是一个在controller action 之前或者之后运行的方法。

<b>它有和controller action 相同的处理 [参数](zh-cn/controller.md) 的能力。</b>


## 2 用法

前置过滤情可能会停止 request cycle。 <br>
一个常用的前置过滤器是某些 action 需要用户登陆后才能访问.

```typescript
@Filter()
class AuthenticateFilter implements IMiddleware {

  public use(@Res() res: Express.Response, @Next() next: Express.NextFunction) {
    if (// use is logged in) {
      next();
    } else {
      res.sendStatus(403);
    }
  }
}

@RestController()
@BeforeFilter(AuthenticateFilter)
class AuthenticatedResourceController {}
```
`AuthenticateFilter` 前置过滤器检查是否用户已经登陆，
如果用户还没有登陆， 就直接返回 403 状态码。

使用时 `AuthenticatedResourceController` 加上一个 `@BeforeFilter(AuthenticateFilter)` 的注解,
这就意味着所有这个 Controller 里面的 路由都会使用前置过滤器 `AuthenticateFilter`。
如果这个 Controller 需要几个能让匿名用户访问的路由, 可以在使用注解时加上参数来达到部分使用过滤器的目的。

```typescript
// Filter definition

@RestController()
@BeforeFilter(AuthenticateFilter, {only: ['indexAction']})
// Or @BeforeFilter(AuthenticateFilter, {except: ['indexAction']})
class AuthenticatedResourceController {}
```
使用 `only` 参数意味着只有 `indexAction` 方法才会使用过滤器 `AuthenticateFilter` 过滤。
使用 `except` 参数意味着 除了 `indexAction` 方法以外都会使用过滤器 `AuthenticateFilter` 过滤。

在过滤器中, 必须要调用 `next()` 方法才会讲请求传递到下一个 `Filter` 或者 `Controller`， 不然应用就会卡在这里。


## 3 数据
你可以通过注解 `@Data` 在 `控制器, 过滤器, 中间件` 中传递数据。

```typescript
@Filter()
class FindUserFilter implements IMiddleware {

  public use(@Data() data: any, @Next() next: Express.NextFunction) {
    data.user = # findById();
    next();
  }
}

@RestController()
@BeforeFilter(FindUserFilter)
class PostController {

  public(@Data() data: any, @Res() res: Express.Response) {
    typeof data.user !== 'undefined' # => true
  }
}
```
在例子中， `FindUserFilter` 在注解中过滤器保存了用户的信息，然后在控制其中就能够直接提取用户信息了。

