# Site Reliability Engineering

## Principles

> It’s impossible to manage a service correctly, let alone well, without understanding which behaviors really matter for that service and how to measure and evaluate those behaviors. To this end, we would like to define and deliver a given level of service to our users, whether they use an internal API or a public product.

### Have realistic targets
Google doesn't try to build 100% reliable services. 
> Extreme reliability comes at a cost: maximizing stability limits how fast new features can be developed and how quickly products can be delivered to users, and dramatically increases their cost, which in turn reduces the numbers of features a team can afford to offer. Further, users typically don’t notice the difference between high reliability and extreme reliability in a service, because the user experience is dominated by less reliable components like the cellular network or the device they are working with.

Instead service reliability is measured in "number of nines". I.e 99.999% is referred to as five nines and allows for less than 5.26 minutes of downitme per year. 

This number should be viewed as both a minimum and maximum availability target. Exceeding this target would waste opportunities to add features or clean up technical debt. 

Therefore managing risk is costly. It can reduce innovation and add unnecessary operational costs. Furthermore, the cost does not increase linearly as reliability increases. 

## Work with product development
There is an inherent tension between product development goals and SRE goals. 

Product development performance is measured on speed of delivery, while SRE performance is based on the reliability of the service. 

This creates an incentive to push back against high rates of change. 

These tensions are clear in the way engineering practices are approached when it comes to:
* software fault tolerance
* testing
* push frequency
* canary duration and size

Decisions on these practices should be reached based on **data** and **business needs**, rather than politics, fear or hope. 

Both parties need to agree on an objective metric that can guide the negotiations in a meaningful and reproducible way. 

## Be prepared

The effectiveness of the emergency response is based on how quickly the system can be brought back to health - i.e. mean time to repair or MTTR. Reliability then can be calculated as a ration between mean time to failure and mean time to repair. 

In order to increase reliability, it's best to have a playbook to guide the on-call engineer with the debugging process since human intervention adds latency.

> While no playbook, no matter how comprehensive it may be, is a substitute for smart engineers able to think on the fly, clear and thorough troubleshooting steps and tips are valuable when responding to a high-stakes or time-sensitive page. Thus, Google SRE relies on on-call playbooks, in addition to exercises such as the “Wheel of Misfortune,”2 to prepare engineers to react to on-call events.

As the majority of outages are due to changes to the live system, outages can be avoided or reduced in duration by:
* Implementing progressive rollouts
* Quickly and accurately detecting problems
* Rolling back changes safely when problems arise

These practices combined with capacity planing and regular load testing will ensure higher reliability. 

## Be accountable

> We use intuition, experience, and an understanding of what users want to define service level indicators (SLIs), objectives (SLOs), and agreements (SLAs). These measurements describe basic properties of metrics that matter, what values we want those metrics to have, and how we’ll react if we can’t provide the expected service.

* SLI (service level *indicator*) - a carefully defined quantitative measure of some aspect of the level of service that is provided. (e.g request latency, error rate, system throughput, availability etc)

  It's important to standardize how your SLIs are generated. For example:
    * aggregation intervals and regions
    * how frequently measurements are made
    * which requests were included
    * how the data was acquired
    * data-access latency

* SLO (service level *objective*) -  a target value or range of values for a service level that is measured by an SLI. (e.g. the average search request latency for a service should be less than 100 milliseconds.) 

> Choosing and publishing SLOs to users sets expectations about how a service will perform. Without an explicit SLO, users often develop their own beliefs about desired performance, which may be unrelated to the beliefs held by the people designing and operating the service. This dynamic can lead to both over-reliance on the service, when users incorrectly believe that a service will be more available than it actually is ( ... as well as... ) under-reliance, when prospective users believe a system is flakier and less reliable than it actually is.

* SLA (service level *agreement*) - an explicit or implicit contract with your users that includes consequences of meeting (or missing) the SLOs they contain. The consequences are most easily recognized when they are financial—a rebate or a penalty—but they can take other forms. **An easy way to tell the difference between an SLO and an SLA is to ask “what happens if the SLOs aren’t met?”: if there is no explicit consequence, then you are almost certainly looking at an SLO.**

> SRE doesn’t typically get involved in constructing SLAs, because SLAs are closely tied to business and product decisions. SRE does, however, get involved in helping to avoid triggering the consequences of missed SLOs. They can also help to define the SLIs: there obviously needs to be an objective way to measure the SLOs in the agreement, or disagreements will arise.

## Be selective

### Features
Software simplicity is a prerequisite to reliability. A complex system is difficult to debug and reason about. It slows innovation and makes it difficult to discern it's purpose and how to best support it. Sometimes systems are solving a difficult problem and become complex naturally - this is referred to as 'essential complexity'. What we want to avoid is unnecessary complexity introduced by accident, aka 'accidental complexity'. 

> Unlike just about everything else in life, “boring” is actually a positive attribute when it comes to software! We don’t want our programs to be spontaneous and interesting; we want them to stick to the script and predictably accomplish their business goals. In the words of Google engineer Robert Muth, “Unlike a detective story, the lack of excitement, suspense, and puzzles is actually a desirable property of source code.” Surprises in production are the nemeses of SRE.

* Modularity: looser coupling enables developers to make changes and improvements indepedently from the rest of the system

* Minimal APIs: the smaller the interface that is provided, the easier it will be to understand, support and maintain an API; this should apply both to the way it is written but also how changes to it are rolled out

> As a system grows more complex, the separation of responsibility between APIs and between binaries becomes increasingly important. This is a direct analogy to object-oriented class design: just as it is understood that it is poor practice to write a “grab bag” class that contains unrelated functions, it is also poor practice to create and put into production a “util” or “misc” binary. A well-designed distributed system consists of collaborators, each of which has a clear and well-scoped purpose.

### Metrics
> You shouldn’t use every metric you can track in your monitoring system as an SLI; an understanding of what your users want from the system will inform the judicious selection of a few indicators. Choosing too many indicators makes it hard to pay the right level of attention to the indicators that matter, while choosing too few may leave significant behaviors of your system unexamined. 

Different systems have specific requirements:

* User-facing systems: availability, latency, throughput
* Storage systems: latency, availability, durability
* Big data systems: throughput and end-to-end latency

> All systems should care about correctness: was the right answer returned, the right data retrieved, the right analysis done? Correctness is important to track as an indicator of system health, even though it’s often a property of the data in the system rather than the infrastructure per se, and so usually not an SRE responsibility to meet.

### Automation - software to act on software

> “If we are engineering processes and solutions that are not automatable, we continue having to staff humans to maintain the system. If we have to staff humans to do the work, we are feeding the machines with the blood, sweat, and tears of human beings. Think The Matrix with less special effects and more pissed off System Administrators.” by Joseph Bironas, a Google SRE

Choose what to automate carefully. Automation is a *force multiplier*, not a panacea. The benefits of automation are clear:

* Consistency
* Faster repair and action

Automation allows you to create a platform. 

> In general, we have chosen to create platforms where we could, or to position ourselves so that we could create platforms over time. We view this platform-based approach as necessary for manageability and scalability.

Platforms can be extended, applied to more systems, measured and monitored accurately. Additionally once a task has been automated it can be performed by anyone, including the service itself! 

Additionally, because platforms *centralise mistakes*, you only have to fix them in a single place. This reduces MTTR, frees developer time for innovation, and even reduces the overall costs of the system (given its sufficiently large).

Common path automation has followed at Google:
1. Operator-triggered manual action (no automation)
2. Operator-written, system-specific automation
3. Externally maintained generic automation
4. Internally maintained, system-specific automation
5. Autonomous systems that need no human intervention









