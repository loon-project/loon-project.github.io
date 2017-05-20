<h1 align="center">Controller</h1>


## 1 What Does a Controller Do?

Controller is the C in MVC. The controller register route and handler for a request,
the controller is responsible for registering a route, making sense of the request and
producing the appropriate output.

In loon framework, any class decorated by `@RestController` or `@Controller` all considered as a controller.


## 2 Route

A controller can register the route for the application, with meaningful group way.
It accept a string parameter as the group routes prefix.

```typescript
@RestController('/admin')
class AdminController {}
```
As an example, the `AdminController` register a route with prefix `/admin` to the application.


## 3 Action

Action register the path with the help of decorators `@Get, @Post, @Put, @Path, @Delete`. <br>
As a function, action also take the responsibility to handle the incoming request.

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
As an example, the `listUsersAction` returns a user list with ids.

## 4 Parameter

Parameter helps to access data sent to `Action`,
there are multiple decorators `@PathParam, @QueryParam, @BodyParam, @HeaderParam, @CookieParam` to achieve this.

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
Above example shows how to use `@PathParam`, `@QueryParam`, and `@BodyParam`,
the `@HeaderParam, @CookieParam` is the same usage. <br>
Controller can convert simple type `string, boolean, number` automatically by using `ConverterService` inside it:

```typescript
//...
  public userAction(@QueryParam('flag') flag: boolean) {
    flag === true or false
  }

//...

```

For convert to a model, we will cover it in the [Model](model.md) chapter.

## 5 Response
Framework use express inside, so you can leverage the Express.Response object to send response to the user.

```typescript
@RestController('/admin')
class AdminController {

  @Get('/users')
  public usersAction(@Res() res: Express.Response) {
    res.send([{id: 1, id: 2}]);
  }
}
```
Use decorator `@Res` to get the Express.Response object, for the details:
[Express.Response](https://expressjs.com/en/4x/api.html#res)





