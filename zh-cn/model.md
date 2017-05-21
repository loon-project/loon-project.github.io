<h1 align="center">模型</h1>


## 1 什么是模型

模型就是一个逻辑对象，其中保存了对象的各种属性。


## 2 简单模型

一个简单的模型就是一个类。

```typescript
class User {
  firstName: string;
  lastName: string;
}
```


## 3 能够转化的模型

模型通过添加 `@Property` 注解增加了转化的能力。

```typescript

class User {

  @Property()
  firstName: string;

  @Property()
  lastName: string;
}

@RestController("/users")
class UserController {

  @Post("")
  public createAction(@BodyParam('user') user: User) {
    user instanceof User # => true
  }
}
```
控制器中的参数 `user` 制定了类是 `User`, 当带有 http body 的请求到来时:
```
{
  user: {
    firstName: "Jack",
    lastName: "Hai"
  }
}
```
user 这个变量会自动的转化成 `User` model 的实例。

## 4 属性

### 4.1 别名
属性可以定义转化的别名。

```typescript
class User {

  @Property('first_name') // => 等同于 @Property({name: 'first_name'})
  firstName: string;
}

const obj = {
  first_name: "Jack"
};

```
这样就能转化一个与类的属性名不同的Object到模型了。

### 4.2 数组
可以定义一个数组属性。

```typescript
class User {

  @Property({baseType: number})
  public friendIds: number[];

}
```
定于属性的类型是数组的时候，需要声明 `baseType`。

### 4.3 自定义转化器
属性也支持传一个实现了 `IConverter` 借口的类作为自定义转化器。

```typescript
@Service()
export class MomentConverter implements IConverter {

    public deserialize(data: any, klassProperty: string, objectProperty: string): any {
        const value = data[objectProperty];

        if (typeof value === 'undefined') {
            return;
        }

        const m = Moment.utc(value);

        if (m.isValid()) {
            return m;
        }

        return;
    }
}

class User {

  @Property({converter: MomentConverter})
  public registerDate: Moment.Moment;
}
```
这个例子显示了如何写一个转化 `string/Date` 对象到 `moment` utc 对象。


