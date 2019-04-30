## Keep things as simple and small as possible to avoid the need to update things.
Updating code is a source of bugs. The best thing to do is avoid updating as much as possible, and test what is being updated. 

A way forward with this is thru DDD. Here are the layers:
- Domain (business logic)
- Infrastructure (connection definitions to outside world, aka repo, emitter)
- IO - outside conection setup

One strategy is to keep io and infra as simple as possible and only test the domain. This is nice because testing outside
services is fragile, and time consuming, and difficult. Testing only business logic is easy. So as a strategy, lets
try to make it so all of our updates happen in the domain, and no updates ever happen to io or infra.

This is not always possible, but if you have logic and complexity in your repo, it is completely impossible. To make
it somewhat possible, we try to never put logic in the repo ever. Even if it seems convenient. The more logic you put in, 
the more likely you will have to come back and update it when the business logic changes.

So instead, make the repo as simple and thin and small as possible. It only ever does one thing. Make it so it never
needs to be updated again, it is what is, a simple low level function that you can compose higher up in the domain.

If we can make our infra a write once, never change situation, then we only need to check if it works once (at the beginning)
and not include it in our CI tests (we still do integration tests).

We then depend on our domain being 100% tested, since that is the part of our code that is changing. 

So in summary:
Anything that changes can change for the worse. Testing infra is fragile and time consuming, so lets avoid testing infra
by never updating it, only ever adding to it. The only way we can never update this code is to make it so simple that
it never needs to change. 
