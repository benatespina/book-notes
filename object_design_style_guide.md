# Object Design Style Guide

By Matthias Noback.

## 1 Programming with objects: A primer
(TODO)

## 2 Creating services
### 2.1 Two types of objects
* Service objects that either perform a task or return a piece of information.
* Objects that will hold some data, and optionally expose some behavior for
manipulating or retrieving that data.

### 2.2 Inject dependencies and configuration values as constructor arguments
* It will make the service ready for use immediately after instantiation. No further setup will be required, and you can’t accidentally forget to provide a dependency.
* Sometimes a service needs some configuration values.
    * Instead of injecting the whole configuration object, make sure you only inject the values that the service actually needs.

#### 2.2.1 Keeping together configuration values that belong together
* Some of these values will always be used together, and injecting them separately would break their natural cohesion.
    * To keep these values together, you can introduce a dedicated configuration object.

### 2.3 Inject what you need, not where you can get it from
* A service locator is itself a service, from which you can retrieve other services.
* Inject service locator adds extra function calls in the code, obscuring what really does.
* Furthermore, because services aren’t injected as dependencies, the service needs to know how to retrieve them.
* This service has access to many other services that can be retrieved from the service locator.
* So declare the actual dependencies that it needs as constructor arguments, and expect them to be injected.

### 2.4 All constructor arguments should be required
* Make dependencies optionals unnecessarily complicates the code inside the service.
* Whether constructor arguments are used to inject dependencies or to provide configuration values, constructor arguments should always be required and not have default values.

### 2.5 Only use constructor injection
* Use non-constructor injection unnecessarily complicates the code inside the service.
* It shouldn’t be possible to create an object in an incomplete state.
* Services should be immutable, that is, impossible to change after they have
been fully instantiated.

### 2.6 There’s no such thing as an optional dependency
* If you need to be something optional, prefer to pass "Null" or "Dummy" implementation doing nothing.

### 2.7 Make all dependencies explicit
#### 2.7.1 Turn static dependencies into object dependencies
* Change static deps for constructor dependencies.

#### 2.7.2 Turn complicated functions into object dependencies
* Functions part of the standard library of the language could be hidden deps so, try to promote to object dependency via constructor. 
* Functions that could easily be written inline definitely don’t need to be wrapped.

#### 2.7.3 Make system calls explicit
* The current time isn’t something that this service could derive from either the provided method arguments, or from any of its dependencies, so it’s a hidden dependency.
    * Create Clock service with default implementation instantiating DateTime object.
* Sometimes we can pass DateTime as a function param removing constructor dependency.

### 2.8 Task-relevant data should be passed as method arguments instead of constructor arguments
* The word “current” is a useful signal that this information is contextual infor- mation that needs to be passed as method arguments: “the current time,” “the currently logged in user ID,” “the current web request,” etc.

### 2.9 Don’t allow the behavior of a service to change after it has been instantiated
* If you don’t allow a service to be modified after instantiation, and you also don’t allow it to have optional dependencies, the resulting service will behave predictably over time, and won’t suddenly start to follow different execution paths based on who called a method on it.

### 2.10 Do nothing inside a constructor, only assign properties
* The real work will be done inside one of the object’s methods.
* Inside a constructor, you may sometimes be tempted to do more than just assign properties, to make the object truly ready for use.
* If you have some logic probably you should push out the objecto moving to a factory.

### 2.11 Throw an exception when an argument is invalid
* Only after the argument has been validated should it be assigned, as follows.
* By throwing an exception inside the constructor, you can prevent the object from being constructed based on invalid arguments.

### 2.12 Define services as an immutable object graph with only a few entry points
* All the services in an application form one large object graph.
* The entry points will be the controllers.
* No service will need the service locator to retrieve services.
* We could use a service container as a service locator to retrieve a controller from. All the other service instantiation logic that’s needed to produce the controller objects can stay behind the scenes, in private methods.
