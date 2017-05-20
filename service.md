<h1 align="center">Service</h1>

## 1 What Does a Service Do?

Service is a class decorated with `@Service`, and can be injected into any other classes like `Controller, Middleware`.

## 2 Term

Service uses dependency injection, for the details: [wiki](https://en.wikipedia.org/wiki/Dependency_injection)

## 3 Usage

```typescript

@Service()
class UserService {
  // any logic
}

@RestController('/users')
class UserController {
  @Inject()
  private userService: UserService;

  @Get('')
  public indexAction() {
    this.userService.listUsers();
  }
}
```
The example `UserService` decorated with `@Service`, and then it is used in `UserController`.

