---
id: es
title: Event Sourcing
---

## Events
Describes interesting things that have already happened
- use past tense names, e.i: AccountWasCharged, PricingLevelChanged, PostWasPublished, PostWasCategorised, PostWasUncategorised  

## What is event sourcing
Event sourcing is a specific procedure for storing data. event sourcing does not persist the current state of a record, but instead stores the individual changes as a series of deltas that led to the current state over time.

### How does it work?
It exactly how bank manages an account. The bank does not save the current balance. Instead, it records the deposits and withdrawals that occur over time. The current balance can then be calculated from this data: 
if the account was first opened with a deposit of 500 EUR, then another 200 EUR were added, and then 300 EUR were debited, the following calculation takes place:
```json
500 (deposit) AccountWasCreated
+ 200 (deposit) AmountWasDeposited
- 300 (payment) AmountWasWithdrawed
= 400 (balance) 
```

How we can get the balance? it could be done by replaying the events.

As a special feature of event sourcing, it is not only possible to determine the current state, but also any state from the past. To do this, it is only necessary to stop the replay at the desired time in the past and not to play back the events completely.
 It is also possible to determine the historical development of the state, which provides an easy way for time series analysis and other evaluations of historical data.

### Optimizing performance
replaying events over the time will become tedious work and more time consuming.

Since events are always only added at the end of the existing list and existing events are never changed, a replay calculated once will always produce the very same result for a certain point in time. If you try to follow the analogy with account management, this is obvious: the account balance at a given point in time is always the same, regardless of whether there were any deposits or withdrawals afterwards.

You can take advantage of this situation by saving the currently calculated state as a so-called `snapshot`. The entire history does not always have to be played back all along the way. Usually it is sufficient to start from the last snapshot and only look at the events that have been saved since then. As a snapshot only supplements history, and does not replace it, the older events are still available if they are required for an evaluation.

## Event sourcing with GDPR

As many already know GDPR came into effect since end of May 2018. As per GDPR customer has right to erasure of right to be forgotten. In my event sourcing application I am having customer sensitive data. 

For whole immutability purpose I do not want to delete or update events. I am familiar with one of the approach which is encrypting data for each customer with it's own key. But this approach has couple of disadvantages

1. I cannot perform search in events (I am using Mongodb to store events)

2. Maintaining key

I wanted to know if anyone else has tried any other approach?

## Symfony messenger as event bus
### Event bus
Event bus separate actions from reactions. Event bus could have zero or more subscribers.


https://symfony.com/doc/current/messenger/multiple_buses.html

##
