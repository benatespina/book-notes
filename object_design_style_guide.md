# Object Design Style Guide

By Matthias Noback.

## 1 Programming with objects: A primer
* Objects can be instantiated based on a given class.
* A class defines properties, constants, and methods.
* Private properties and methods are accessible to instances of the same class. Public properties and methods are accessible to any client of an object.
* An object is immutable if all of its properties can’t be modified, and if all
objects contained in those properties are immutable themselves.
* Dependencies can be created on the fly, fetched from a known location, or
injected as constructor arguments (which is called dependency injection).
* Using inheritance you can override the implementation of certain methods of a parent class. An interface can declare methods but leave their implementations entirely to a class that implements the interface.
* Polymorphism means that code can use another object’s methods as defined by
its type (usually an interface), but that the runtime behavior can be different
depending on the specific instance that is provided by the client.
* When an object assigns other objects to its properties, it’s called composition.
* Unit tests specify and verify the behaviors of an object.
* While testing, you may replace an object’s actual dependencies with stand-ins
known as test doubles (such as stubs and mocks).
* Dynamic arrays can be used to define lists or maps without specifying types for its keys and values.

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

## 3 Creating other objects
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

## 4 Manipulating objects
### 4.1 Entities: Identifiable objects that track changes and record events
* An entity may change over time, but it should always be the same object that under- goes the change.
* An entity needs to be identifiable.
* Entities are mutable objects.
* The methods that change the entity’s state should have a void return type and their names should be in the imperative form.
* These methods have to protect the entity against ending up in an invalid state.
* The entity shouldn’t expose all its internals to test what’s going on inside. Instead, an entity should keep a change log and expose that, so other objects
can find out what has changed about it, and why.

### 4.2 Value objects: Replaceable, anonymous, and immutable values
* Value objects are not identifiables.
* We don’t care about the exact instance we’re working with, since we don’t need to track the changes that happen to a value object.
* A value object is any immutable object that wraps primitive-type values.

### 4.3 Data transfer objects: Simple objects with fewer design rules
* Avoid getters and setters in favour public final properties to avoid unit tests.

### 4.4 Prefer immutable objects
* Design every object that is not an entity to be immutable.

### 4.5 A modifier on an immutable object should return a modified copy
* Immutable objects can have methods that could be considered modifiers, but they don’t modify the state of the object, returns a copy of the object.
* Use the constructor of the object, to create the desired copy, like the plus() method in the next listing.
* Useful for immutable objects with mul- tiple properties, is to create an actual copy of the object using the clone operator, and then make the desired change to it.

### 4.6 On a mutable object, modifier methods should be command methods
* They have a name in the imperative form.
* They’re allowed to make a change to the object’s internal data structures.
* They don’t return anything.

### 4.7 On an immutable object, modifier methods should have declarative names
* Good name for modifier methods on immutable objects, you can fill in the following template: "I want this ..., but ...".
    * I want this position, but n steps to the left: `toTheLeft`
* You may often end up using the word "with," or using so called participle adjectives in the past tense.

### 4.8 Compare whole objects
* Use inmutable objects facilitates unit testing without exposing getters.

### 4.9 When comparing immutable objects, assert equality, not sameness
* When comparing integers, we don’t compare their memory locations, only their value.
* Use the same approach to compare immutable objects: assertEquals.
* If you need to compare two objects outside tests, implement equals method.

### 4.10 Calling a modifier method should always result in a valid object
* In order to reuse the validation logic, don't use clone, but always to go through the constructor.

### 4.11 A modifier method should verify that the requested state change is valid
* Make sure that every one of your methods prevents against making invalid state transitions.

### 4.12 Use internally recorded events to verify changes on mutable objects
* To test for changes in a mutable object is to record events inside the object that can later be inspected.

### 4.13 Don't implement fluent interfaces on mutable objects
* For immutable objects, having a fluent interface is not a problem.

## 5 Using objects
### 5.1 A template for implementing methods
```php
[scope] function methodName(type name, ...): void|[return-type] {
    [preconditions checks]
    [failure scenarios]
    [happy path]
    [postcondition checks]
    [return void|specific-return-type]
}
```

#### 5.1.1 Precondition checks
* Any number of checks, and throw exceptions when anything looks off.
* We can use assertion functions.
* Some of these precondition checks may only be needed because the type system of the language lacks some features.
* To avoid checks, use "replace primitive with object" refactor.

#### 5.1.2 Failure scenarios
* If something goes wrong in the method after the precondition checks, you should throw a different kind of exception.
* The type of the exception should indicate that an error condition occurred that could only be detected at runtime.

#### 5.1.3 Happy path
* It is where nothing is wrong, and the method is just performing its task.

### 5.1.4 Postcondition checks
* They can be added to a method to verify that the method did what it was supposed to do.
* In practice, most methods don’t need postcondition checks.
* Useful technique if you are dealing with legacy code, with lots of implicit type casting and no assertions.
* You could get rid of a method’s postcondition checks, just like you can remove pre- condition checks, by promoting primitive-type values to proper objects and returning those from your method.
* Another option is to wrap the method with postcondition checks in a new method that performs these checks.

#### 5.1.5 Return value
* As soon as you know what you will return, return it right away instead of keeping the value around, skipping a few more if clauses, and then returning it.

### 5.2 Some rules for exceptions
#### 5.2.1 Use custom exception classes only if needed
* If you want to catch a specific exception type higher up.
* If there are multiple ways to instantiate a single type of exception
* If you want use named constructors for instantiating the exception

#### 5.2.2 Naming invalid argument or logic exception classes
* Exception class names don’t need to have “Exception” in them.
* For instance: `InvalidEmailAddress`, `InvalidTargetPosition`, or `InvalidStateTransition`.

#### 5.2.3 Naming runtime exception classes
* They communicate how the system tried to perform the requested job, but couldn’t finish it successfully.
* For example, `CouldNotFindProduct`, `CouldNotStoreFile`, or `CouldNotConnect`.
#### 5.2.4 Use named constructors to indicate reasons for failure
* Use the name to indicate the ingredients needed to instantiate the exception.
* Use the method name to indicate the reason why something is wrong.

#### 5.2.5 Add detailed messages
* With named constructors will set up the exception’s message.

## 6 Retrieving information
### 6.1 Use query methods for information retrieval
* The have specific return type, and it’s not allowed to produce any side effects.
* It’s better to have safe methods that can be called any time:
    * Via command/query separation principle (CQS).
    * Make your objects immutable.

```php
   final class Counter
   {
       private int count = 0;
       public function incremented(): Counter
       {
           copy = clone this;
           copy.count++;
           
           return copy;
       }
    
       public function currentCount(): int
       {
           return this.count;
       }
   }
```
* A modifier method returns a copy of the whole object, and once you have that copy, you can ask it questions.

### 6.2 Query methods should have single-type return values
* Avoid returning multiple types.
* Alternatives to return nullable types:
    * throw an exception
    * return an object that can represent the null case
    * return a result of the same type. For instance empty array
* You can show the uncertainty in the name of the method, for example `findById` instead of `getById`.

### 6.3 Avoid query methods that expose internal state
* The only type of object that could be designed as a JavaBean is a DTO.
* Instead of expose getters for clients to do somme tasks, expose method returning task result.
* Don't use `getItemCount()` or `countItems()` because these method names sound like commands, telling the object to do something. Use `itemCount()`.
* Make the method smarter, and adapt it to the actual need of its clients.
* Move the call inside the object, letting it make its own decisions.
* Avoid use `get` prefix for getters because method isn't a command method, but that it simply provides a piece of information.

### 6.4 Define specific methods and return types for the queries you want to make
* The “answer” class, should be designed to be as useful as possible for the client that needs it.
* Potentially, this class can be reused at other call sites, but it doesn’t have to be.

### 6.5 Define an abstraction for queries that cross system boundaries
* Abstraction should be a service interface instead of a service class.
* Abstraction should leaving out the implementation details.
* Abstraction will make it possible to run your code in a test scenario.

### 6.6 Use stubs for test doubles with query methods
* An abstraction is a useful extension point. You can easily change the implementation details of how the answer will be found.
* Testing logic will be easier.
* This makes the test deterministic and therefore stable.
* Naming test methods:
    * Test method names describe object behaviors. The best description is an actual sentence.
    * Because they are sentences, test method names will be longer than regular method names. It should still be easy to read them (so use snake case instead).
    * Good rule is to start methods with `it_`, `when_` or `if_`.

* A fake is one kind of test double, which can be characterized as showing "somewhat complicated".
* A stub is a test double that just returns hardcoded values. Use to replace real service.
* Don't make any assertions about the number of calls made to Stubs or the order in which those calls are made. (In query methods).
* Dummies are test doubles that don't return anything meaningful and are only there to be passed as unused arguments.

### 6.7 Query methods should use other query methods, not command methods
* A chain of query calls won't contain a call to a command method.
* Queries are supposed to have no side effects, and calling a command method somewhere in the chain will violate that rule.

## 7 Performing tasks
### 7.1 Use command methods with a name in the imperative form
* Use commands for performing tasks returning void.

### 7.2 Limit the scope of a command method, and use events to perform secondary tasks
Advantages:
* You can add even more effects without modifying the original method.
* The original object will be more decoupled because it doesn’t get dependen-
cies injected that are only needed for effects.
* You can handle the effects in a background process if you want.

### 7.3 Make services immutable from the outside as well as on the inside
* If your services are immutable shouldn’t need to be shared, they can but they don’t have to.

### 7.4 When something goes wrong, throw an exception
* When something goes wrong, don’t return a special value to indicate it; throw an exception instead.

### 7.5 Use queries to collect information and commands to take the next steps
* When a chain of calls starts with a command method, it’s possible that you’ll encounter a call to a query method down the line.

### 7.6 Define abstractions for commands that cross system boundaries
* It’s generally a good idea to start with the abstraction and leave the generalization until you’ve seen about three cases that could be simplified by making the interface and object types involved more generic.

### 7.7 Only verify calls to command methods with a mock
* Don't mock query methods because they do not produce side-effects.
* In a unit test, you shouldn’t verify the number of calls made to them.
* If you decide to call a method twice instead of remembering its result in a variable, the test won’t break.
* When a command method makes a call to another command method, you may want to mock or create a spy of the latter.
    * With mock: there are no regular assertions at the end of this test method, because the mock object itself will verify that our expectations were met. The test framework will ask all mock objects that were created for a single test case to do this.
    * With spy: use assertions combined with spy; a spy will remember all method calls that were made to it, including the arguments used.