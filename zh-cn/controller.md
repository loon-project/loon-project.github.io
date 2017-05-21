<h1 align="center">控制器</h1>


## 1 什么是控制器?

控制器就是 MVC 中的 C. 控制器在应用中注册了一个路由，并提供了处理这个路由请求的handler.
控制器的责任就是注册一个路由，理解请求，并产生合适回复。

在框架中， 任何被`@RestController`和`@Controller`注解的类都是控制器。


## 2 路由

控制器通过一个简单的方法为应用注册路由。它接受一个 string 参数作为该控制器路由的前缀。

```typescript
@RestController('/admin')
class AdminController {}
```
这个例子中, `AdminController` 注册了一组前缀为 `/admin` 的路由。


## 3 Action

Action 通过注解 `@Get, @Post, @Put, @Path, @Delete` 注册路径。 <br>
Action 作为方法， 也负责处理过来的请求。

```typescript
@RestController('/admin')
class AdminController {

  @Get("/users")
  public listUsersAction(@Res() res: Express.Response) {
    res.send({
      users: [{id: 1}, {id: 2}]
    });
  }
}
```
在例子中， `listUsersAction` action 返回用户的id列表。

## 4 参数

Action 的参数有助于获取发送来的数据,
不同的注解 `@PathParam, @QueryParam, @BodyParam, @HeaderParam, @CookieParam` 可以获取不同的数据.

```typescript
@RestController('/admin')
class AdminController {

  @Get('/users/:id')
  public userAction(@PathParam('id') id: number) {
      # incoming request is /admin/users/1
      # then id inside this action would be 1
  }

  @Get('/users')
  public listUsersAction(@QueryParam('is_active') isActive: boolean) {
      # incoming request is /admin/users?is_active=true
      # then isActive inside this action would be true

      isAction === true # => true
  }

  @Post('/users')
  public createUserAction(@BodyParam('user') user: any) {
      # incoming request is POST /admin/users with json body
      # {
      #   user: {name: "Jack"}
      # }
      # then the user inside this action would be a object

      user.name === "Jack" # => true
  }
}
```
上述方法展示了如何使用 `@PathParam`, `@QueryParam`, and `@BodyParam`,
没有展示的 `@HeaderParam, @CookieParam` 用法也是相同的. <br>
控制器能够转化简单的数据类型 `string, boolean, number`, 通过内部的 `ConverterService` 做到自动转化:

```typescript
//...
  public userAction(@QueryParam('flag') flag: boolean) {
    flag === true or false
  }

//...

```

如果是转化一个模型, 文档会在 [模型](zh-cn/model.md) 这一章里讲述。

## 5 回复
框架内部使用了express, 可以利用 express 生成回复 Express.Response。

```typescript
@RestController('/admin')
class AdminController {

  @Get('/users')
  public usersAction(@Res() res: Express.Response) {
    res.send([{id: 1, id: 2}]);
  }
}
```
使用注解 `@Res` 获取 Express.Response 对象。
Express 详细信息: [Express](https://expressjs.com/en/4x/api.html#res)





