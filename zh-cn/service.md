<h1 align="center">服务</h1>

## 1 什么是服务

服务就是被注解了 `@Service` 的类, 这样的类可以被注入到其他的服务或者控制器等等。

## 2 名次

服务使用了依赖注入的技术，细节你可以参考: [wiki](https://en.wikipedia.org/wiki/Dependency_injection)

## 3 用法

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
在例子中, `UserService` 注解了 `@Service`, 然后被注入在了 `UserController`。

