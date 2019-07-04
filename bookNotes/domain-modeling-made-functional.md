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

Create a map of your bounded contexts, and the events that go between them
its common to create multiple maps when the system gets complex

which domains should we build first?
- Core Domains - Generally, some domains are more important than others. These are the core domains—the ones that provide a business advantage, the ones that bring in the money.
- Supportive Domains - Other domains may be required but are not core. These are called supportive domains
- Generic Domains - and if they are not unique to the business they are called generic domains.


Focus instead on those bounded contexts that add the most value, and then expand from there.

