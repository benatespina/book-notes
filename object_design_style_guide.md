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

## 3 Creaing other objects
### 3.1 Require the minimum amount of data needed to behave consistently
* Constructor should be used to protect a domain invariants.

### 3.2 Require data that is meaningful
* If you make sure that every object has the minimum required data provided to it at construction time, and that this data is correct and meaningful, you will only encounter complete and valid objects in your application.

### 3.3 Don’t use custom exception classes for invalid argument exceptions
* An invalid argument means that the client is using the object in an invalid way.
* For RuntimeExceptions on the other hand, it often makes sense to use custom exception classes because you may be able to recover from them, or to convert them into user-friendly error messages.

### 3.4 Test for specific invalid argument exceptions by analyzing the exception’s message
* To distinguish between InvalidArgumentException in a unit test try to expect a concrete message.

### 3.5 Extract new objects to prevent domain invariants from being verified in multiple places
* Wrapping values inside new objects called value objects is useful to avoid repeated validation logic.
* By introducing more objects to represent domain concepts, you’re effectively extending the type system. Your language’s compiler or runtime will be able to support you much better, because it can do type-checking for you and make sure that only the right types end up being used when passing method arguments and returning values.

### 3.6 Extract new objects to represent composite values
* Extract new objects validates data that object wraps.
* An object usually exposes additional, meaningful behaviors that make use of its data.
* An object can keep values together that belong together.
* An object helps you keep implementation details away from its clients.

### 3.7 Use assertions to validate constructor arguments
* Also called precondition checks.
* User assertion libraries for that.
* Don't write unit test if it would be possible for the lan-guage runtime to catch this case, otherwise yes.
* Don't collect exceptions. If you need form validations use DTOs for that.

### 3.8 Don’t inject dependencies; optionally pass them as method arguments
* Other objects shouldn’t get any dependencies injected, only values, value objects, or lists of them.
* The need for passing around services as method arguments could be a hint that the behavior should be implemented as a service instead.

### 3.9 Use named constructors
* These are public static methods that return an instance. They could be considered object factories.

#### 3.9.1 Create from primitive-type values
* For instance: `fromString` or `fromInt`.
* Provide private, constructor method, so that clients won’t be able to bypass the named constructor you offer to them, which would possibly leave the object in an invalid or incomplete state.

#### 3.9.2 Don’t immediately add toString(), toInt(), etc.
* Make sure you only do this once there is a proven need for it.

#### 3.9.3 Introduce a domain-specific concept
* Use domain experts terminology to define the named constructors.

#### 3.9.4 Optionally use the private constructor to enforce constraints
* Using a private constructor helps to ensure that what- ever construction method is chosen, the object will end up in a complete and consistent state.

### 3.10 Don’t use property fillers
* Passing a map as constructor arguments opens object's internals, so always make sure that the construction of an object happens in a way that’s fully controlled by the object itself.
* Exceptions: DTO.

### 3.11 Don’t put anything more into an object than it needs
* Don’t require more data than is strictly needed to implement the object’s behavior.
* For instance events. If you don’t know which data will be important for yet-to-be-implemented event listeners, don’t add anything.

### 3.12 Don’t test constructors
* Only test a constructor for ways in which it should fail.
* Only pass in data as constructor arguments when you need it to implement real
behavior on the object.
* Only add getters to expose internal data when this data is needed by some
other client than the test itself.

### 3.13 The exception to the rule: Data transfer objects
* A DTO can be created using a regular constructor.
* Its properties can be set one by one.
* All of its properties are exposed.
* Its properties contain only primitive-type values.
* Properties can optionally contain other DTOs, or simple arrays of DTOs.

#### 3.13.1 Use public properties
* Since a DTO doesn’t protect its state and exposes all of its properties, there is no need for getters and setters.

#### 3.13.2 Don’t throw exceptions, collect validation errors
* If you want to allow users to correct all their mistakes in one go, before resubmitting the form, you should validate the command’s data before passing the object to the ser- vice that’s going to handle it.
* Form and validation libraries may offer you more convenient and reusable tools for validation.

#### 3.13.3 Use property fillers when needed
* DTO doesn’t protect its internals anyway so does not exist any problem.
* User for example: `fromFormData`.
