## pattern

**Creational Patterns**:

| Todo | Pattern                                                      | Description                                                  |      |
| ---- | :----------------------------------------------------------- | ------------------------------------------------------------ | ---- |
|      | [abstract_factory](https://github.com/faif/python-patterns/blob/master/patterns/creational/abstract_factory.py) | use a generic function with specific factories               |      |
|      | [borg](https://github.com/faif/python-patterns/blob/master/patterns/creational/borg.py) | a singleton with shared-state among instances                |      |
|      | [builder](https://github.com/faif/python-patterns/blob/master/patterns/creational/builder.py) | instead of using multiple constructors, builder object receives parameters and returns constructed objects |      |
|      | [factory](https://github.com/faif/python-patterns/blob/master/patterns/creational/factory.py) | delegate a specialized function/method to create instances   |      |
|      | [lazy_evaluation](https://github.com/faif/python-patterns/blob/master/patterns/creational/lazy_evaluation.py) | lazily-evaluated property pattern in Python                  |      |
|      | [pool](https://github.com/faif/python-patterns/blob/master/patterns/creational/pool.py) | preinstantiate and maintain a group of instances of the same type |      |
|      | [prototype](https://github.com/faif/python-patterns/blob/master/patterns/creational/prototype.py) | use a factory and clones of a prototype for new instances (if instantiation is expensive) |      |

**Structural Patterns**:

|      | Pattern                                                      | Description                                                  |      |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
|      | [3-tier](https://github.com/faif/python-patterns/blob/master/patterns/structural/3-tier.py) | data<->business logic<->presentation separation (strict relationships) |      |
|      | [adapter](https://github.com/faif/python-patterns/blob/master/patterns/structural/adapter.py) | adapt one interface to another using a white-list            |      |
|      | [bridge](https://github.com/faif/python-patterns/blob/master/patterns/structural/bridge.py) | a client-provider middleman to soften interface changes      |      |
|      | [composite](https://github.com/faif/python-patterns/blob/master/patterns/structural/composite.py) | lets clients treat individual objects and compositions uniformly |      |
|      | [decorator](https://github.com/faif/python-patterns/blob/master/patterns/structural/decorator.py) | wrap functionality with other functionality in order to affect outputs |      |
|      | [facade](https://github.com/faif/python-patterns/blob/master/patterns/structural/facade.py) | use one class as an API to a number of others                |      |
|      | [flyweight](https://github.com/faif/python-patterns/blob/master/patterns/structural/flyweight.py) | transparently reuse existing instances of objects with similar/identical state |      |
|      | [front_controller](https://github.com/faif/python-patterns/blob/master/patterns/structural/front_controller.py) | single handler requests coming to the application            |      |
|      | [mvc](https://github.com/faif/python-patterns/blob/master/patterns/structural/mvc.py) | model<->view<->controller (non-strict relationships)         |      |
|      | [proxy](https://github.com/faif/python-patterns/blob/master/patterns/structural/proxy.py) | an object funnels operations to something else               |      |

**Behavioral Patterns**:

|      | Pattern                                                      | Description                                                  |      |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
|      | [chain_of_responsibility](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/chain_of_responsibility.py) | apply a chain of successive handlers to try and process the data |      |
|      | [catalog](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/catalog.py) | general methods will call different specialized methods based on construction parameter |      |
|      | [chaining_method](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/chaining_method.py) | continue callback next object method                         |      |
|      | [command](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/command.py) | bundle a command and arguments to call later                 |      |
|      | [iterator](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/iterator.py) | traverse a container and access the container's elements     |      |
|      | [iterator](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/iterator_alt.py) (alt. impl.) | traverse a container and access the container's elements     |      |
|      | [mediator](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/mediator.py) | an object that knows how to connect other objects and act as a proxy |      |
|      | [memento](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/memento.py) | generate an opaque token that can be used to go back to a previous state |      |
|      | [observer](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/observer.py) | provide a callback for notification of events/changes to data |      |
|      | [publish_subscribe](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/publish_subscribe.py) | a source syndicates events/data to 0+ registered listeners   |      |
|      | [registry](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/registry.py) | keep track of all subclasses of a given class                |      |
|      | [specification](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/specification.py) | business rules can be recombined by chaining the business rules together using boolean logic |      |
|      | [state](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/state.py) | logic is organized into a discrete number of potential states and the next state that can be transitioned to |      |
|      | [strategy](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/strategy.py) | selectable operations over the same data                     |      |
|      | [template](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/template.py) | an object imposes a structure but takes pluggable components |      |
|      | [visitor](https://github.com/faif/python-patterns/blob/master/patterns/behavioral/visitor.py) | invoke a callback for all items of a collection              |      |

**Design for Testability Patterns**:

|      | Pattern                                                      | Description                        |      |
| ---- | ------------------------------------------------------------ | ---------------------------------- | ---- |
|      | [dependency_injection](https://github.com/faif/python-patterns/blob/master/patterns/dependency_injection.py) | 3 variants of dependency injection |      |

**Fundamental Patterns**:

|      | Pattern                                                      | Description                                                  |      |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
|      | [delegation_pattern](https://github.com/faif/python-patterns/blob/master/patterns/fundamental/delegation_pattern.py) | an object handles a request by delegating to a second object (the delegate) |      |

**Others**:

|      | Pattern                                                      | Description                                                  |      |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
|      | [blackboard](https://github.com/faif/python-patterns/blob/master/patterns/other/blackboard.py) | architectural model, assemble different sub-system knowledge to build a solution, AI approach - non gang of four pattern |      |
|      | [graph_search](https://github.com/faif/python-patterns/blob/master/patterns/other/graph_search.py) | graphing algorithms - non gang of four pattern               |      |
|      | [hsm](https://github.com/faif/python-patterns/blob/master/patterns/other/hsm/hsm.py) | hierarchical state machine - non gang of four pattern        |      |