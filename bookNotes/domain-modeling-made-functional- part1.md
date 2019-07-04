# Chapter 1 - Introduction to DDD

## Understanding the Domain thru Business Events
Focus on business events rather than data structures

When starting a project, start with event storming. Get everyone involved in a room (business people, devs, domain experts),
and write out on the wall, or with sticky notes all events that take place.
These will make up the events in your system eventually, 
but what is important about this practice: there is no talk of tech. Its just events written on paper or written on the wall.
Its this DOMAIN MODEL, that makes up every teams understanding, including the devs.

Next, we talk about input, or COMMANDS, so:
COMMANDS: input, verbs, present tense (CREATE_CHARGE, SEND_SHIPMENT_TO_CUSTOMER_ABC)
EVENTS: past tense (CHARGE_CREATED, SHIPMENT_SENT)
PROCESS / WORFLOW: a string of commands and events


## Partitioning the Domain into SubDomains
A domain is an area of coherent knowledge

## Creating a solution using bounded contexts
Some advice on creating bounded contexts:
- Listen to the domain experts. If they all share the same language and focus on the same issues, they are probably working in the same subdomain (which maps to a bounded context).
- Don’t forget the “bounded” part of a bounded context. Watch out for scope creep when setting boundaries. In a complex project with changing requirements, you need to be ruthless about preserving the “bounded” part of the bounded context. A boundary that is too big or too vague is no boundary at all. As the saying goes, “Good fences make good neighbors.”
- Design for friction-free business workflows. If a workflow interacts with multiple bounded contexts and is often blocked or delayed by them, consider refactoring the contexts to make the workflow smoother, even if the design becomes “uglier.” That is, always focus on business and customer value rather than any kind of “pure” design.

###### Context Map
Create a context map of your bounded contexts, and the events that go between them
its common to create multiple maps when the system gets complex

which domains should we build first?
- Core Domains - Generally, some domains are more important than others. These are the core domains—the ones that provide a business advantage, the ones that bring in the money.
- Supportive Domains - Other domains may be required but are not core. These are called supportive domains
- Generic Domains - and if they are not unique to the business they are called generic domains.


Focus instead on those bounded contexts that add the most value, and then expand from there.

## Creating a Ubiquitous Language
Things in our design must represent real things in the domain expert’s mental model

###### Use Domain Language
That is, if the domain expert calls something an “order,” then we should have something called an Order in the code that corresponds to it and that behaves the same way.

###### Avoid Technical Language
And conversely, we should not have things in our design that do not represent something in the domain expert’s model. That means no terms like OrderFactory, OrderManager, OrderHelper, and so forth. A domain expert wouldn’t know what you meant by these words. Of course, some technical terms will have to occur in the codebase, but you should avoid exposing them as part of the design.

The set of concepts and vocabulary that is shared between everyone on the team is called the Ubiquitous Language—the “everywhere language.” This is the language that defines the shared mental model for the business domain. And, as its name implies, this language should used everywhere in the project, not just in the requirements but in the design and, most importantly, in the source code

## Summary
- Focus on events and processes rather than data.
- Partition the problem domain into smaller subdomains.
- Create a model of each subdomain in the solution.
- Develop an “everywhere language” that can be shared between everyone involved in the project.

# Chapter 2 - Understanding the Domain

## Interview with a domain expert
One nice thing about the commands/events approach is that rather than needing all-day meetings, we can have a series of short interviews, each focusing on only one workflow, so a domain expert is more likely to be able to make time for this.

In the first part of the interview, we want to stay at a high level and focus only on the inputs and outputs of the workflow. This will help us avoid getting swamped with details that are not (yet) relevant to the design.

The best way to learn about a domain is to pretend you’re an anthropologist and avoid having any preconceived notions. 

## Fighting the impluse to do database driven design
In domain-driven design we let the domain drive the design, not a database schema.

It’s better to work from the domain and to model it without respect to any particular storage implementation. After all, in a real-world, paper-based system, there is no database. The concept of a “database” is certainly not part of the ubiquitous language. The users do not care about how data is persisted.

In DDD terminology this is called `persistence ignorance`. It is an important principle because it forces you to focus on modeling the domain accurately, without worrying about the representation of the data in a database.

## Fighting the impulse to do class driven design

If you’re an experienced object-oriented developer, then the idea of not being biased to a particular database model will be familiar, Indeed, object-oriented techniques such as dependency injection encourage you to separate the database implementation from the business logic.

But you, too, may end up introducing bias into the design if you think in terms of objects rather than the domain.

In the preliminary design above we have separated orders and quotes, but we have introduced an artificial base class, OrderBase, that doesn’t exist in the real world. This is a distortion of the domain. Try asking the domain expert what an OrderBase is!

The lesson here is that we should keep our minds open during requirements gathering and not impose our own technical ideas on the domain.

## Documenting the Domain
let’s just create a simple text-based language that we can use to capture the domain model:

For workflows, we’ll document the inputs and outputs and then just use some simple pseudocode for the business logic.

For data structures, we’ll use AND to mean that both parts are required, such as in Name AND Address. And we’ll use OR to mean that either part is required, such as in Email OR PhoneNumber.

Using this mini-language, then, we can document the Place Order workflow like this:
```
Bounded context: Order-Taking
​ 	
​ 	Workflow: "Place order"
​ 	   triggered by:
​ 	      "Order form received" event (when Quote is not checked)
​ 	   primary input:
​ 	      An order form
​ 	   other input:
​ 	      Product catalog
​ 	   output events:
​ 	      "Order Placed" event
​ 	   side-effects:
​ 	      An acknowledgment is sent to the customer,
​ 	      along with the placed order

```

And we can document the data structures associated with the workflow like this:
```
bounded context: Order-Taking
​ 	
​ 	data Order =
​ 	    CustomerInfo
​ 	    AND ShippingAddress
​ 	    AND BillingAddress
​ 	    AND list of OrderLines
​ 	    AND AmountToBill
​ 	
​ 	data OrderLine =
​ 	    Product
​ 	    AND Quantity
​ 	    AND Price
​ 	
​ 	data CustomerInfo = ???   // don't know yet
​ 	data BillingAddress = ??? // don't know yet
```

Note that we have not attempted to create a class hierarchy or database tables or anything else. We have just tried to capture the domain in a slightly structured way.

The advantage of this kind of text-based design is that it’s not scary to non-programmers, which means it can be shown to the domain expert and worked on together.


# Chapter 3 - A Functional Architecture

## Intro
We’ll use the terminology from Simon Brown’s “C4” approach:
- SYSTEM CONTEXT - The “system context” is the top level, representing the entire system.

- CONTAINERS - The system context comprises a number of “containers,” which are deployable units such as a website, a web service, a database, and so on.

- COMPONENTS - Each container in turn comprises a number of “components,” which are the major structural building blocks in the code.

- CLASSES / MODULES - Finally, each component comprises a number of “classes” (or in a functional architecture, “modules”) that contain a set of low-level methods or functions.

One of the goals of a good architecture is to define the various boundaries between containers, components, and modules, such that when new requirements arise, as they will, the “cost of change” is minimized.

## Bounded Contexts as Autonomous Software Components
It’s tricky to create a truly decoupled microservice architecture: if you switch one of the microservices off and anything else breaks, you don’t really have a microservice architecture, you just have a distributed monolith!

Reach for the monolith first, and break it apart as needed


