<h1 align="center">Application</h1>


## 1 What Does a Application Do?

Application help to wire other components and third party express packages.


## 2 ApplicationSettings


```typescript
@ApplicationSettings({rootDir: `${__dirname}/../`})
class Application extends ApplicationLoader {
}
```

`@ApplicationSettings` accept argument with interface `SettingOptions`:
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


## 3 OnInit

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

Application have an `$onInit` hook which can be used to connect express packages with framework.
You can add `ejs` view support like above example.









