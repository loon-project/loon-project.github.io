<h1 align="center">应用</h1>


## 1 什么是应用？

应用就是用来串起各个组件和第三方 `express` 插件。


## 2 应用设定


```typescript
@ApplicationSettings({rootDir: `${__dirname}/../`})
class Application extends ApplicationLoader {
}
```

`@ApplicationSettings` 接受一个实现接口 `SettingOptions` 的参数:
```typescript
interface SettingOptions {
    // Required
    rootDir: string;

    srcDir?: string;

    publicDir?: string;

    logDir?: string;

    configDir?: string;

    dbDir?: string;

    env?: string;

    port?: string|number;
}
```


## 3 启动钩子方法

```typescript
@ApplicationSettings({rootDir: `${__dirname}/../`})
export class Application extends ApplicationLoader {

    // this.server is the express application instance

    public $onInit() {
        this.server.set('view engine', 'ejs');
        this.server.set('views', `${this.rootDir}/view`);
        this.server.use(require('serve-static')(this.publicDir));
    }
}
```

应用有个 `$onInit` 的钩子方法，在这个方法里面可以很方便的使用 `express` 的各种包。 <br>
这个例子中显示了如何添加 `ejs` 的支持。









