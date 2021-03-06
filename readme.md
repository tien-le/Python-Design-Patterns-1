# Python Design Pattern

## Classification of Patterns

1. Creational Patterns
- They work on the basis of how object can be created
- They isolate the details of object creation
- Code is independent of type of object to be created
Factory, Abstract Factory, Builder, Prototype, Singleton versus Borg
    
2. Structural Patterns
- They design the structure of object and classes so that they can compose to achieve larger results
- The focus is on simplifying the structure and identifying the relationship between classes and objects
- They focus on class inheritance and composition
MVC(model-view-controller), Facade, Proxy, Decorator, Adaptor
    
3. Behavioral Patterns
- They are concerned with the interaction among objects and responsibility of objects
- Objects should be also be able to interact and still be loosely coupled
Command, Interpretor, State, Chain of responsibility, Strategy, Observer, Memento, Template, Reactive design patterns

## The Factory Pattern
    
    [Client] ---> [(Interface)] ---> [Factory] 

Advatages:
- The first advandage is loose coupling in which object creation can be independent of the class implementation
- The client need not be be aware of the class that creates the object which, in turn, is utilized by the client.
It is only necessary to know the interface, methods, and parameters that need to be passed to create objects of the desired type.
This simplofies implementations for the client.
- Adding another class to the factory to create objects of another type can be easily done without the client changing the code.
At a minimum, the client needs to pass just another parameter.
- The factory can also reuse the existing objects. However, when the client does direct object creation, this always creates a new object.

Variants:
- Simple Factory Pattern: This allows interfaces to create objects without exposing the object creation logic.
- Factory Method Pattern: This allows interface to create objects, but defers the decision to the subclasses to determine the class for object creation.
- Abstract Factory Pattern: An Abstract Factory is an interface to create related objects without specifying/exposing their classes.
The pattern provides objects of another facory, which internally creates other objects.

## The Builder Pattern

    [Director] ---> [Builder] ---> [(Concrete Builder)] ---> [Product] 

Separate the construction of a complex object from  its representation so that the same construction process can create different representations.
Building objects by assembling specifict parts.
    
## The Prototype Pattern

**"Builder Pattern" with .clone() in the product**

Specify the kinds of objects to use a prototypical instance, and create new objects by copying this prototype.
It's often used when creation(initiation) of an object is costly.

Watch out for the difference of copy and deepcopy in python!

## The Singleton and Borg Design Pattern

Singletons are typically used in the cases need to have only one instance that is available across the application to avoid conflicting requests on the same resource.

- Ensuring that one and only one object of the class gets created
- Providing an access point for an object that is global to the program
- Controlling concurrent access to resources that are shared

The Borg Idiom (a.k.a monostate pattern) lets a class have as many instances as one likes, but ensure that they all share the same state.

Object creation vs. object initiation
- **__new__** is the first step of instance creation; it's called before __init__, and is responsible for returing a new instance of your class
- **__init__** dosen't return anything; it's only responsible for initializing the instance after it's been created.

Comparison between Singleton and Borg
- Singleton: 
    - Pros: allocated only once (mem. efficent), policies can be added to the method for accessing singlton pointer
    - Cons: Derivatives of Singletons are not automatically Singleton (need to overriding methods), must always be accessed through a pointer or reference
- Borg:
    - Pros: Derivatives of Borg are also Borg, Access Borg does not have to be through pointers or references.
    - Cons: allocated/deallocated many times, no instantiation policy can exist

Python modules are singletons! But it's generally not a good idea to us them for the Singleton pattern.
- Error-prone: if you forget local and global statements, it will create confusions.
- They pollute the module namespace
- No OOP benefits like associated methods or reuse through inheritance

## The Model View Controller Pattern

[Description #1]

MVC is an UI pattern, intended to separate internal representations of data from the ways it is presented to/accepted from the user.

    WEB REQUEST ---> [ROUTES] ---> [CONTROLLER] <---> [MODEL] <---> [DATABASE]
                                        |
                                        v
                                    [VIEW] ---> PAGE

Flask: Micro web framework for python / Uses Jinja template engine/ Used by Pinterest and LinkedIn/ Django is a more feature-heavy alternative

[Description #2]

MVC is a compound pattern. In most software implementation, patterns don't work in isolation.
The model presents the data and business logic (how information is sorted and queried), view is nothing but the representation (how it is presented) of the data, and controller is the glue between the two.

The MVC pattern is used in the following cases:
- When there is a need to change the presentation without changes in the business logic.
- Multiple controllers can be used to work with multiple views to change the representation on the user interface.
- Once again, the model can be changed without changes in the view as they can work independently of each other.
    
## The Facade Pattern

Provide a unified interface to a set of interfaces in a subsystem. A facade defines a higher-level interface that makes the subsystem easier to use.
Use the components without knowing the details of them. Used when combining complex systems into one simple interface.

    CLIENT ---> [FACADE] ---> [ComplicatedClassA]
                    |
                    --------> [ComplicatedClassB]
                    |
                    --------> [ComplicatedClassC] ...
            
- Reduce dependencies of outside code on the inner workings of library.
- Wrap a poorly designed collection of APIs with a single well designed API.

## The Proxy Pattern

A proxy provides a surrogate or place holder to provide access to an object.

    CLIENT                   [PROXY]
        |                       |
        [] ---- request() ----> [] ---------------> [REAL SUBJECT]
        []                      |                       |
        [] <------------------- [] <---------------------
    
The Proxy pattern is used in the following senarios:
- It adds security to the existing real object.
- It provides a local interface for remote objects on different servers.
- It provides a light handle for a higher memory-consuming object.
        
## The Decorator Pattern

Attach additional responsibilities to an object dynamically.
Decorators provide a flexible alternative to subclassing for extending functionality.

    [Window] ------ [Window with Horizontal SB] -------- [Window with Vertical and Horizontal SB]
                |                                   |                       |
                --- [Window with Vertical SB] -------                       |
                |                                                           |
                --- [Window with Border] ---------------------------------------- [Window with Vertical and Horizontal SB and Border]
    
    [LCD] ----- [Window]        --- [Border]
            |                   |
            --- [Decorator] ------- [Vertical SB]
                                |
                                --- [Horizontal SB]
                                
- Sometimes, subclassing is not the best solution
- Decorator design pattern is not Python decorator/function wrapper !!! (OOB design pattern vs function decorator)
    
## The Adapter Pattern

Convert the interface of a class into another interface clients expect.
Adapter lets classes work together that couldn;t otherwise because of incompatible interfaces.

    [Ratchet / interface#1 ] ---> [Adapter / interface#1, interface#2] ---> [Socket / interface#2]

- Make old component usable in a new system
- Make an 'off-the-shelf' solution usable in a system that is not fully compatible.
    
## The Command Pattern
    
[Description #1]
Decouple the object that invokes the operation from the one that knows how to perform it.

[Client] ---> [Command / execute()] ...> [CommandInterface / execute()] --- [Invoker]
        |               |
        |               v
        --------> [Receiver]

- The Client creates a Command object that can ve executed at a later date
- This object knows about a receiver object that manages its own internal state when the command is executed on it
- One or more Invoker objects execute the command at the correct time

[Description #2]
The Command pattern is a behavioral design pattern in which an object is used to encapsulate all the infomation needed to perform an action or trigger an event at a later time.
This information includes the following:
- The method name
- An object that owns the method
- Values for method parameters

Example: installation wizard ---> choose different setting (store command information) ---> click "Finish" Button (execute command)

The Command pattern works with the following terms - Command, Receiver, Invoker and Client:
- A Command object knows about the Receiver objects and invokes a method of the Receiver object
- Values for parameter of the reveiver method are stored in the Command object
- The invoker knows how to execute a command
- The client creates a Command object and sets its receiver

The main intentions of the Command pattern are as follows:

    [Client] ---> [Receiver / action()]
        .                   ^
        .                   |
        ........> [ConcreteCommand / execute()] ---> [Command / execute()] --- [Invoker]

- Encapsulating a request as an object
- Allowing the parameterization of clients with different requests
- Allowing to save the requests in a queue
- Providing an object-oriented callback

Advantages
- It decouples the classes that invoke the operation from the object that knows hot to execute the operation
- It allows you to create a sequence of commands by providing a queue system
- Extensions to add a new command is easy and can be done without changing the existing code
- You can also define a rollback system with the Command pattern, for example, in the Wizard example, we could write a rollback method

Disavantages
- There are a high number of classes and objects working together to achieve a goal.
Application developers need to be careful developing these classes correctly.
- Every individual command is a ConcreteCommand class that increases the volume of classes for implementation and maintenance.
    
## The Interpretor Pattern

(Language interpretor for specific domain. Grammar or DSL)

Define a grammatical representation for a language and an interpretor to interpret the grammar.

    [Client] ---> [ExpressionBase] ------------------
        |               ^   ^                       |
        |               |   |                       |
        v               |   ---------- [NonterminalExpression]
    [Context]   [TerminalExpression]

- Client: build the tree of expressions, the interpret method of the top item in the tree is then called
- Context(Optional): used to store any information that needs to be available to all expression objects
- ExpressionBase: base class defining the Interpret method
- TerminalExpression: can be interpreted in a single object
- NonterminalExpression: aggregates containing one or more further expressions, each of which may be terminal or non-terminal

## The State Pattern

[Description #1]
Allow an object to alter its behavior when its internal state changes.

    [Context / Request()] <state>---> [State / Handle()] <---------------
        .                               ^                               |
        .                               |                               |
        .                               |                               |
        .                       [ConcreteStateA / Handle()]    [ConcreteStateB / Handle()]
        v
    [state.Handler()]

- Define a Context class to present a single interface to the outside world.
- Define a State abstract base class.
- Represent the different states of the state machine as derived classes of the State base class.
- Maintain a pointer to the current state in the context class
- To change the state of the state machine, change the current state pointer

[Description #2]
The state design is a behavioral design pattern, which is also sometimes referred to as an objects for states pattern.
In this pattern, an object can encapsulate multiple behaviors based on its internal state.
A State pattern is also considered as a way for an object to change its behavior at runtime

- State: This is considered to be an interface that encapsulates the object's behavior.
This behavior is associated with the state of the object.
- ConcreteState: This is a subclass that implements the State interface.
ConcreteState implements the actual behavior associated with object's particular state.
- Context: This defines the interface of interest to clients.
Context also maintains an instance of the ConcreteState subclass that internally defines the implementation of the object's particular state.

Advantages:
- This removes the dependency on the if/else or switch/case conditional logic.
- It's benefitial for implementing polymorphic behavior.
- State pattern imporves cohesion since ConcreteState code location.
- It is easy to add states to support additional behavior.

Disavantages:
- Class Explosion: class for each state.
- With the introduction of every new behavior, the Context class needs to be updated to deal with each behavior.

## Chain of Responsibility

Avoids coupling the sender of a request to the receiver by giving more than one object a chance to handle the request.

                              ..........................................................//
                              .                                                         \\
    [Client] ---<Request>---> . ---> [Processing element] ---> [Processing element] --->//
                              .                                                         \\
                              ..........................................................//

- Number and/or type of handler objects unknown at the point the client sends the request
- Multiple and dynamically configuable receivers, each only needs to maintain a reference to its immediate successor
- Immediate response not required

## The Observer Pattern

Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and update automatically.

    [Subject    ] ----<views>---> [Observer     ]    
    [+attach    ] <---<model>---- [+update()    ]
    [+setState()]                   ^
    [+getState()]                   |
        .               ---------------------
        .               |                   |
        .           [ViewOne    ]       [ViewTwo    ]
        .           [+update()  ]       [+update()  ]
        .                                   .
        .                                   .       
    for each view in views                  .
        v.update()                      model.getState()

- The subject is the "keeper" of the data model/business logic.
- Delegate all "view" functionality to decoupled and distinct Observer objects, which register themselves with the Subject.
- When the Subject changes, it broadcasts to all registered Observers.

- Observer is unidirectional, v.s. Controller in MVC pattern is bidirectional
- It encapsulates the core component of the Subject

The Observer pattern is used in the following multiple scenarios:
- Implementation of the Event service in distributed systems
- A framework for a news agency
- The stock market also represents a great case for the Observer pattern    

The loose coupling architecture ensures following features:
- It reduces the risk that a change made within one element might create an unanticipated impact on the other elements
- It simplifies testing, maintenance, and troubleshooting problems
- The system can be easily broken down into definable elements

Advantages:
- It supports the principle of loose coupling between objects that interact with each other
- It allows sending data to other objects effectively without any change in the Subject or Observer classes
- Observers can be add/removed at aany point in time

Disavantages:
- The Observer interface has ti be implemented by ConcreteObserver, which involves inheritance.
There is no option for composition, as the Observer interface can be instantiated.
- If not correctly implemented, the Observer can add complexity and lead to inadvertent performance issues.
- In software application, notifications can, at times, be undependable and result in race conditions or inconsistency.
    
## The Strategy Pattern

Define a family of algorithms, encapsulate each one, and make them interchangeable.
Strategy lets the algorithm vary independently from the clients that use it.

    [Clent*] ---> [Abstraction* <<interface>>] <--- [ImplementationOne]
                            ^
                            |
                            ----------------------- [ImplementationOne]

- Open-closed principle: software entities (classes, modules, functions, and so on) should be open for extension, but closed for modification
- Program to an interface, not an implementation

## The Memento Pattern

Capture and externalize an object's internal state so that the object can be returned to this state later.
(It supports undoable & rollback operations)

    [Originator / state ] ---> [Memento / state ] <---<> [Caretaker]
    [+setMemento()      ]      [+getState()     ]
    [+createMemento()   ]      [+setState()     ]
    .       .    
    .       ............. state = m->getState()
    ..................... return new Memento(state)

- Originator: the object that knows how to save itself
- Caretaker: the object that knows why and when the Originator needs to save/restore itself
- Memento: the state box written and read by the Originator, and shepherded by the Caretaker.

Recall: "undo" in Command pattern
- Each Command has its own undo method
- Memento uses chain states in Memento object 
 
## The Template Pattern

[Description #1]

Define the skeleton of an algorithm, deferring some steps tp client subclasses.
Let subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

    [Implementation #1]
    
    [FrameworkClass     ]       ---[ApplicationClassOne ]
    [+templateMethod()  ]       |  [+stepTwo()          ]
    [+stepOne()         ] <-----|
    [+stepTwo()         ]       |
    [+stepThree()       ]       ---[ApplicationClassTwo ]
                                   [+stepTwo()          ]                                   

    [Implementation #2]
    
    [Client] ---> [SortAlgorithm    ] <-------- [SortAscending  ]
                  [+sort()          ]       |   [+#compare()    ]
                  [+#compare()      ]       |
                  [+returnArray()   ]       --- [SortDescending ]
                  [+processArray()  ]           [+#compare()    ]
                  .
                  .
                  ... processArray()
                      compare()
                      returnArray()
    
- Strategy is like Template Method except in its granularity - 
Template Method uses inheritance to vary part of an algorithm.
Strategy uses delegation to vary the entire algorithm.
- Abstract Factory Method is a specialization of Template Method, where the abstract factory acts like the template,
and its concrete subclasses fill in the details of how to return the required objects.
    
[Description #2]

An abstract class is used to define the steps of the algorithm.
These steps are also known as primitive operations in the context of the Template Method pattern.
These steps are defined with abstract methods, and the Template Method defines the algorithm.
The ConcreteClass (that subclasses the abstract class) implements subclass-specific steps of the algorithm.

The Template Method pattern is used in the following cases:
- When multiple algorithms or classes implement similar or identical logic
- The implementation of algorithms in subclasses helps reduce code duplication
- Multiple algorithms can be defined by letting the subclasses implement the behavior through overriding

Main intentions of the Template Method pattern:
- Defining a skeleton of an algorithm with primitive operations
- Redefining certain operations of the subclass without chaning the algorithm's structure
- Achieving code reuse abd avoiding duplicate effort
- Leveraging common interfaces or implementations

- The Template Method pattern - hooks
A hook is a method that is declared in the abstract class.
It is generally given a default implementation.
The idea behind hooks is to give a subclass the ability to hook into the algorithm whenever needed.
It's not imperative for the subclass to use hooks and it can easily ignore this.

Advantages
- There is no code duplication
- Code reuse happens with the Template Method pattern as it uses inheritance and not composition. Only a few methods need to be overridden.
- Flexibility lets subclasses decide how to implement steps in an algorithm.

Disavantages
- Debugging and understanding the sequence of flow in the Template Method pattern can be confusing at times. 
- Maintenance of the template framework can be a problem as changes at any level.

## The Reactive Programming Pattern

(This is not an ordinary pattern from Gang of Four.)

    ---> [event #1] ---> [event #2] ---> [X] ---> [event #3] ---> [event #4] --- || ---> time
            .                            .                                       .
            .                            .                                       .
            .                            .                                       .
            ...... indicates an event    ... indicates an error                  ... indicates that
                   with some value,                                                  stream has completed
                   e.g. coordinates of a click

What is Reactive Programming:
- Extends the observer pattern
- Composing asynchronous and event-based programs by using observable sequences
- Treat streams of asynchronous events with the same sort of simple, composable operations that you use for collections of data items licke arrays
- It frees your form tangled webs of callbacks, and thereby makes your code more readable and less prone to bugs
- Abstract away concerns about things like synchronization, thread-safety, concurrent data structures, and non-blocking I/O

What is RxPY:
- Reactive Extensions for Python (RxPY) is a set of libraries for reactive programming
- Asynchronous data streams are represented with Observables.
Multiple strams from diverse sources (for example, stock quote, Tweets, computer event) are supported
- The Observable type implements static query operatiors which allow you to easily filter, map, reduce, compose and perform time-based operations on multiple events
- Concurrency in data/event strams can be parametrized using Schedulers

> pip install rx

Guthub [Link](http://github.com/ReactiveX/RxPY) to RxPY

## Spaghetti Code (AntiPattern)

Program flow is twisted and tangled like spaghetti.

Symptoms:
- Code with very little structure
- Hard to understand, even to the original developer
- Very process oriented
- Small number of objects with very large methods and multiple responsibilities
- Minimal relationship between objects
- Pattern of object use is very predictable

Consequences:
- Difficult to extend or reuse
- Software quickly reach to a point of diminishing returns;
the effort involved in maintaining an existing code base is greater than the cost of developing a new solution
- Flow of execution is determined by object implementation, not the client
- Benefits of OO are lost

Typical Causes:
- Inexperience with or lack of understanding of OOP
- Working in isolation
- Ineffective code review
- No design before implementation
- In fact, these are typical causes of most anipatterns!

How to prevent it?
- Python meaningful indentation helps
- Avoid large multi-responsibility classes or methods
- Ask someone unfamiliar with the code to read it and see if they can easily understand its intention
- Design before implementation

## Blob (AntiPattern)

One class monopolizes the processing, and other classes primarily encapsulate data, aka a God object.

    [Example Blob Archetecture]
    
    [Main Controller Class  ] <--- [Images]
    [+Data_List_Provider    ] <--- [Records]
    [+State                 ] <--- [Data1]
    [+Mode                  ] <--- [Figure1]
    [+User                  ] <--- [Users]
    [+Group                 ] <--- [ErrorSet]
    [...                    ] <--- [Table1]
    [-----------------------]      ...
    [+Start()               ]
    [+Stop()                ]
    [+Initialize()          ]
    [+Set_Mode()            ]
    [...                    ]
    
Syptoms
- Single class with a large number of attributes and/or operations
- These attributes and operations tend to be quite disparate - not clear why they should belong to one class
- The single contoller class has many associated simple, data-object classes
- All source code in one monolithic file

Consequences:
- Limits OO benefits, for example, makes it hard to modify the system without affecting the functionality of other encapsulated objects
- Typical too complex for testing
- The Blob class may be expensive to load into memory

How to refactor a Blob?
- System represents a library, with items that can be catalogued, and checked in and out by people
- The Library Main Control is a Blob

1. Identify related methods on the Blob
2. Try to find natual homes for these in the other classes
3. Rmove far-coupled links, for example, Main Control doesn't need to access Item directly.
Item belongs to Catalog.
4. Remove transient associations, for example, Check_Out_Item can be move to a transient class.

** Acceptable exception **: wrapping legacy systems - there is no software partitioning required, just a final layer of code to make the legacy system more accessible.

## Functional Decomposition (AntiPattern)

Discrete functions for data manipulation.
Good for procedural progamming but doesn't translate to class hierarchy.

Symptoms:
- All class attributes are private and used only inside the class
- Classes with a single action such as a function
- No leveraging of object-oriented principles such as inheritance and polymophism

Consequences:
- Class models make little sense as object
- Hard to document how the system works
- No OO benefit of reuse

Tips for Refactoring Away from Functional Decomposition
- If the class has a single method, try to better model it as part of an existing class
- Attempt to combine several classes into a new class that satisfies a design objective.
E.g. rather than have classes to manage device access, to filter information to an from the devices and to control the device, combine them into a single device controller object.
- If the class does not contain state information of any kind, consider rewriting it as a funcion
- Examine the design and find similar subsystems, these are reuse candidates

** Acceptable exception **: when OO solution is not requred. This can be extended to solutions that are purely functional in nature but wrapped to provide an object-oriented interface to the implementation code.

## Copy and Paste (AntiPattern)

Degenerate form of software reuse which is hard to maintain.
Overusing the notion that it's easier to modify existing software than program from scratch.
It's totaly against the idea of DRY (Donot Repeat Yourself).

Symptoms and Consequences:
- Several similar segments of code interspersed throughout the software project
- Lines of code increase without adding to overall productivity
- Same bug reoccurs throughout software
- Difficult to locate and fix all instances of a particular mistake
- Reusable assets are not converted into an easily reusable and documented form

Causes:
- In the short-term it's faster to copy and paste than to design reusable components, so this antipattern occurs when short term payoff is valued over long term investmentor maintainablility
- Poor understanding of inheritance, composition, and other reuse strategies
- Existing code is hard to understance, so when asked to create something similar, copy and paste may seem like the lowest risk option
- Spaghetti code

** Acceptable exception **: urgent hot fix, patch or feature deadline, with the intention of refactoring almost immediately.

# Reference:

1. **LearningPython Design Patterns - Second Edition**
by Chetan Giridhar, Packt Publishing, Feb. 2016

2. **Python Design Patterns**
by Tong Qiu, Packt Publishing, 2017
