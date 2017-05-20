<h1 align="center">Filter</h1>


## 1 What Does a Filter Do?

Filter is a function that is run "before", "after" a controller action.

<b>It have the same [parameter](controller.md) ability as the controller action.</b>


## 2 Usage


"before" filters may halt the request cycle.
A common "before" filter is one which requires that a user is logged in for an action to run.

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
The `AuthenticateFilter` checks if the user is logged in or not.
If user haven't logged in, then response with forbidden.

The `AuthenticatedResourceController` decorated with `AuthenticateFilter`,
that means every route/action inside this controller will run after filter.
If you want some actions can run for anonymous user, you need specify the exception case for filter

```typescript
// Filter definition

@RestController()
@BeforeFilter(AuthenticateFilter, {only: ['indexAction']})
// Or @BeforeFilter(AuthenticateFilter, {except: ['indexAction']})
class AuthenticatedResourceController {}
```
As an example, the only options in the filter means only `indexAction` will run after filter in this controller,
while the except options means except `indexAction`, others all run after filter.

In a filter, it is important to call `next()` to continue the call chains and request to the next `Filter` or `Controller`.


## 3 Data
You can pass data between different `Controller, Filter, Middleware` use decorator `@Data`.

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
In this example, `FindUserFilter` save user information with decorator `@Data`, then controller can use data.user directly.

