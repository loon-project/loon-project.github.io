<h2 align="center">Release Note</h2>

### v1.1.0 (2017-05-25)
* add event emit and listen support

### v1.0.0 (2017-05-20)
* documentation

### v0.8.0 (2017-04-18)
* add test solution: use bootstrap function
* add PropertyInherited decorator

### v0.7.0 (2017-04-17)
* add type convert in controller parameters
* add ConverterService to convert data
* add Property decorator

### v0.6.0 (2017-03-17)
* add required parameter options
* add @Head, @Options http request


### v0.5.0 (2017-03-14)
* add logger config


### v0.4.0 (2017-03-14)
* @Middleware and @ErrorMiddleware support
* middleware order and base url support


### v0.3.0 (2017-03-13)
* Support @Filter decorator to decorate a class as a Filter
* Support @BeforeFilter, @AfterFilter to filter for a controller, add options: only, except
* Support @Data param type to inject data for the controller and filters


### v0.2.0 (2017-03-12)
* Support @Middleware decorator to inject for controllers
* Support Middleware, Controller parameter injections, now your middleware have the same ability as Controller to inject not only Req, Res, and BodyParam, PathParam and so on.
* Support new @ApplicationSettings and ApplicationLoader for new start up application way
* Support convert class to object and object to class by Converter, and provide template ability


### v0.1.0 (2017-02-17)
* Spring like routes and controller support based on [express](https://github.com/expressjs/expressjs.com)
* Build in support for log based on [winston](https://github.com/winstonjs/winston)
* Build in support for query builder based on [knex](https://github.com/tgriesser/knex)
* Best practice middlewares preinstalled
* Best practice project structure
* Easy server initialization
* Dependency Injection
