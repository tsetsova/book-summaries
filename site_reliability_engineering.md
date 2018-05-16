# Site Reliability Engineering

## Principles

### Have realistic targets
Google doesn't try to build 100% reliable services. 
> Extreme reliability comes at a cost: maximizing stability limits how fast new features can be developed and how quickly products can be delivered to users, and dramatically increases their cost, which in turn reduces the numbers of features a team can afford to offer. Further, users typically don’t notice the difference between high reliability and extreme reliability in a service, because the user experience is dominated by less reliable components like the cellular network or the device they are working with.

Instead service reliability is measured in "number of nines". I.e 99.999% is referred to as five nines and allows for less than 5.26 minutes of downitme per year. 

This number should be viewed as both a minimum and maximum availability target. Exceeding this target would waste opportunities to add features or clean up technical debt. 

Therefore managing risk is costly. It can reduce innovation and add unnecessary operational costs. Furthermore, the cost does not increase linearly as reliability increases. 

# Work with product development
There is a inherent tension between product development goals and SRE goals. 

Product development performance is measured on speed of delivery, while SRE performance is based on the reliability of the service, which creates an incentive to push back against high rates of change. 

These tensions are clear in the way engineering practices are approached when it comes to
* software fault tolerance
* testing
* push frequency
* canary duration and size

Decisions on these practices should be reached based on data and business needs, rather than politics, fear or hope. 

Both parties need to agree on an objective metric that can guide the negotiations in a meaningful and reproducible way. 

### Measure

In order to identify the risk tolerance of your consumer services consider the following:

* target level of availability
* types of failures
* cost
* other service metrics

Once appropriate metrics have been identified and agreed upon, consider using an **error budgets**. The main benefit of an error budget is that it provides both product development and SREs with a common incentive to find a balance between innovation and reliability.

For example:
A service needs to have 99% availability. Therefore the error budget allows for 1% downtime.

While the system meets this objective, product development can take more risks. However, as the budget is being drained, product development themselves will add more tests or slow down their velocity to ensure they don't exceed the budget. If the budget is ever exceeded, releases are temporarily halted while additional resources are invested in resilience and performance. 

In this way the error budget helos manage risk with cost.

