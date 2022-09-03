# Minicore Documentation

## Overview

> Minicore is a JVM base cross-platform, high-performance, open-source framework for building modern, cloud-enabled, Internet-connected apps.

> It is heavily in inspired by the Aspnet Core and Express. Curently Minicore framework is in the development phase ,Basic work flow is ready for creating api :)

#### Currently implemented Features

- Pipeline Creation for for request
- Routing
- Global exception hanlding
- Filters
  - Global level Filter configuration
  - Controller level Filter configuration
  - Action level Filter configuration
- Content negociation using Formatters
  - Input Formatters
  - Output Formatters

> Currently I am looking for contributors for the project and need your help to make this project successfull

> You can find the Github Project Requirement gathering board Link [here](https://github.com/users/priyanhsu10/projects/2)

> [GitHub Repositoy](https://github.com/priyanhsu10/minicore).

## Get started

> comminig soon ..
> ![The San Juan Mountains are beautiful!](/assets/Arcflow.png "Archeture diamgram")

### Program Class


```
public class Program {
    public static void main(String[] args) {
       WebHostBuilder.build(args)
               .useStartup(AppStartup.class)
               .run();
    }
}
```


> comminig soon ..

### AppStartup Class

```
public class AppStartup implements IStartup {

    //Register your service with IOC Container
   
    @Override
    public void configureServices(IServiceCollection service) {
       
        //configuring mvc options
        MvcConfigurer.configureMvc(service,options->{

            // Adding your custom  Global  filters
            options.addGlobalFilter(TestGlobalActionFilter.class);

            //Adding your Custom Exception Filters
            options.addExceptionFilter(TestExceptionFilter.class);
          
            // Adding your custom Global Result filters
            // Global filter will execute for every action before writing the response
            options.addResultFilter(TestResultFilter.class);

        });
        
        //Resigter your dependencies with the IOC Container like below
        service.addSingleton(ITestService.class, TestService.class);


    }
//build your pipeline
    @Override
    public void configure(IApplicationBuilder app) {
        
        app.use(TransactionIdMiddleware.class);-->//Custom Middleware added in the pipeline 
       
       //route endPoint selector
        app.useRouting(); 
       
        //add custom middlewares
        app.use(CustomMiddleware.class);

        ...  --> Other custom midlewares
       
        app.useEndpoints();//Endpoint executor
    }

}
```
> comminig soon ..

### Setting up Pipeline



> In startup class pipeline can be cofnigre in configure method ..
```
public class AppStartup implements IStartup {

   ...
//build your pipeline
    @Override
    public void configure(IApplicationBuilder app) {


      //create your own pipeline :D
        app.use(TransactionIdMiddleware.class);
      
        app.useRouting(); //route EndPoint selector
      

        //add custom middlewares
        app.use(CustomMiddleware.class);

         ...  --> Other custom midlewares


        app.useEndpoints();//Endpoint executor (This should be the last midleware in the pipeline because after this next middleware wont call)
    }

}

```
### Controllers
> You can create the controller by extending the controllerBase class


```

@Route(path = "/hello") --> @Route Annoation setut the base route for Controller
@ActionFilter(filterClass = TestActionFilter2.class) ---> Controller Level Action filter . this is exectuted On every action execution in the controller
public class HelloController  extends ControllerBase {
    private ITestService testService;

    public HelloController(ITestService testService) { ---> Inject Dependency using Contructor Injection 
        this.testService = testService;
    }

    @Get       //-->  Using @Get attribute can specify Get Method 
    @ActionFilter(filterClass = TestActionFilter.class) --> //apply custom filter for specific action
    @ResultFilter(filterClass = CustomResultFilter.class)  --> //custom Result Filter Which Before Result Execution. at this poit you can change the respose.

    public List<Model> get() {
        //accesing transaiton id which is added by transactionIdMiddlware
        System.out.println(httpContext.getData().get("trazId"));
        return this.testService.getlist();

    }

    @Get(path = "/:id")   --> //we can specify route templete like this  eg "<route>/:{parameter from route}" this case route will be hello/:id
    public Model get(int id) {
        return this.testService.getById(id);
    }

    @Get(path = "/filter") //value bind from query
    public List<Model> get(@FromQuery String name) {
        return this.testService.getlist().stream().filter(x->x.getName().equals(name)).collect(Collectors.toList());
    }
    @Post
    //Direct Model binding use @FromBody annotation
    public Model post(@FromBody Model model){
        return testService.create(model);
    }
    @Post(path = "/m2")
    //Custom ModelBiding
    public Model post( Model2 model){
        return model.getModel();
    }
    @Put(path = "/update")
    public String put(String name){
        return "this is post "+name;
    }
    @Delete(path = "/delete")
    public String delete(String name){
        return "this is post "+name;
    }

}
```

> comminig soon ..

### Actions

> comminig soon ..

## Turotials

> comminig soon ..

## Fundametals

> comminig soon ..

### App Startup

> comminig soon ..

### Depenndecy Injection

> Micore proved the the following depencency type to be register 
1. Singleton
2. RequestScope
3. Transient

> IServiceCollection  interface is provide the functionality to resole the and register the types to IOC container ..
```

public class AppStartup implements IStartup {

    //Register your service with IOC Container
   
    @Override
    public void configureServices(IServiceCollection service) {
       
        //Resigter your dependencies with the IOC Container like below
        service.addSingleton(ITestService.class, TestService.class);


    }

.....

Use the dependencies 
public class HelloController  extends ControllerBase {
    private ITestService testService;

    public HelloController(ITestService testService) { ---> Inject Dependency using Contructor Injection 
        this.testService = testService;
    }

```

### Middleware

> comminig soon ..
![The San Juan Mountains are beautiful!](/assets/midleware.png "Archeture diamgram")

### Routing

> Http Route Annotations
1. @Route
2. @Get
3. @Put
4. @Post
5. @Delete

#### Model Binding

> Model binding can be perform in using following annotations

1. @FromBody
2. @FromForm
3. @FromRoute
4. @FromQuery

> custom Model biding Model

```
@Post(path = "/m2")
    //Custom ModelBiding
    public Model post( Model2 model){
        return model.getModel();
    }
....

//Custom ModelBiding
public class Model2 {
   
    //value bind from header
    @FromHeader(Key = "Authorization")
    private String token;

   //value bind from Query
    @FromQurey
    private String name;

    //value bind from Route
    @FromRoute
    private String id;

    //value bind from Body
    @FromBody
    private Model model;

    ....
    setter getter

}

```

#### Route Contraints

> comminig soon ..

### Filters

> comminig soon ..

### Formatters

> comminig soon ..

#### Input Formatters

> comminig soon ..

#### Output Formatters

> comminig soon ..

### App Startup

> comminig soon ..
