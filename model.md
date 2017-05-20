<h1 align="center">Model</h1>


## 1 What Does a Model Do?

Model represents the business object, store the properties for the business object.


## 2 Simple

A simple model is just a class

```typescript
class User {
  firstName: string;
  lastName: string;
}
```


## 3 Convert

A model can have ability to convert to an object with the help of `@Property` decorator.

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
Controller `@BodyParam` variable user specified type as `User`, when a request comes in with http body:
```
{
  user: {
    firstName: "Jack",
    lastName: "Hai"
  }
}
```
the user variable will automatically convert to `User` model with the right value.

## 4 Property

### 4.1 Alias
Property specify different name for the conversation.

```typescript
class User {

  @Property('first_name') // => equivalent to @Property({name: 'first_name'})
  firstName: string;
}

const obj = {
  first_name: "Jack"
};

```
So you can convert a object with different name to the model.

### 4.2 Array
Property can convert array property.

```typescript
class User {

  @Property({baseType: number})
  public friendIds: number[];

}
```
By specify the baseType of the `Array`, the `friendIds` will fill in the right data.

### 4.3 Custom Converter
Property can also pass in a class which implements `IConverter` interface to convert any data.

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
The above example shows how to convert string/Date object to a `moment` utc object.


