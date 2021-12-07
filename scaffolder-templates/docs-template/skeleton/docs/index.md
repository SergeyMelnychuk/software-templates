**System**: One or more units of software that allow us to provide capabilities to customers or support that provision. It may be a *monolith*, be made up of *microservices* or *nanoservices* or even a mix of the last two.

**Service Oriented Architecture (SOA)**: An architectural style in which a *System* is divided into multiple components, called services, each of which represents self-contained functionality that corresponds to a real world business process.

**Monolith**: An architectural style in which the *System* must be deployed as a whole, usually due to a Shared Database architecture. Monolith is not a pejorative term. Big-Ball-Of-Mud is the pejorative term for a decayed form of this architectural style.

**Microservice**: Microservices are an opinionated type of *SOA*, catalogued by [Fowler and Lewis](https://martinfowler.com/articles/microservices.html). It is an architectural style that partitions a *System* into independently deployable services modelled around a *Business Capability*. The service is the technical authority for that *Business Capability*, and all data and business rules live within the service. A service uses lightweight communication protocols to communicate with other services. A service is a logical boundary, not a physical one. A microservice may consist of multiple endpoints, all of which are implemented by separate *Applications*, yet form part of the deployment boundary.  These *Applications* may be independently scaled, but are deployed together. For example a Microservice might consist of an HTTP web *Application*, a SQS worker *Application*, and database tables, which have different scaling requirements, but should be deployed together. 

**Nanoservice**: Is an architectural style that partitions a *System* into independently deployable services modelled around functions. A function is usually at the *Task*, not *Process* or *Activity* level in a process model. FAAS platforms, such as AWS Lambda, are a common host for nanoservices. [Nanoservices](https://arnon.me/wp-content/uploads/2010/10/Nanoservices.pdf?372273&372273) are considered an anti-pattern at scale, due to the management and cognitive overhead of each service becoming unmanageable with large numbers of services. At scale, it is better to create *Microservices* that contain multiple FAAS endpoints treating them as Applications, over managing each one as an independently deployable service boundary with its own database etc.. 

**Application (App)**: A single executable, that runs on a single 'machine'. An application is not a *Microservice* although a *Microservice* consists of one or more Applications. An Application is not a *Nanoservice* though an *Nanoservice* has one app. An Application is a unit of deployment for scalability, but if part of a *Microservice* is not the deployment boundary. Applications may follow the [12-factor app](https://12factor.net/) guidelines. 

**Business Capability**: Something that a business does to offer value to a customer, or to support offering value to a customer. A capability implements one or more business Processes through the application of people, knowledge, and systems. A capability is usually a noun like 'Catering'. A capability is how we deliver a *Process*.

**Process**: What the *Business Capability* delivers on, usually a verb. So 'Catering' may deliver the 'Make Lunch' process. Some *Business Capabilities* deliver more than one Process. So Catering might also deliver the 'Make Breakfast' process. Processes are stable, and so are the boundaries that we use for partitioning by capability in *Microservices*. If the process  is too large, we may partition at an Activity level, which is a step in the Process. Tasks are a level below, and whilst they may be targets for *Applications* or *Nanoservices*, they are not good targets for *Microservices* as they are less stable. Processes can be discovered by techniques such Value Stream Mapping or Event Storming. Alignment of backlogs and OKRs to a process forms a "Products not Projects" strategy.

## Other SOA Architectural Style Taxonomies ##
There are other varieties of *SOA* than *Microservices*. Web Services, for example, was a popular variety at the turn of the millennium based around services exposed via SOAP endpoints. They impose different constraints on a service, and in return offer differing properties created by adhering to those constraints.

Two may be worth mentioning, so as to illuminate alternatives to *Microservices* as described by Fowler and Lewis.

### Udi Dahan - Distributed Systems ###

**Service**: A service that is the authority for a *Business Capability*. A service can consist of one or more  *Business Components*. Maps broadly to a *Microservice*.

**Business Component**: A Business Component implements one of the *Activities* within the *Process* that the *Service* automates. *Services* are not expected to have more than 2-4 Business Components -- although this seems a little arbitrary, process modelling also tends to recommend less than 5 or so activities in a *Process*. This is a logical concept, focused on the breakdown of the *Process* into its *Activities*, over a unit of deployment or scaling. 

**Autonomous Component**: A *Business Component* may consist of one or more Autonomous Components. Autonomous Components are the units of deployment within a *Service*. There may be many Autonomous Components within a Business Component. They might be a unit of scaling i.e. an *Application*, but can also be a shared module, usually shared via a package manager. 

Module based Autonomous Components may be shared between *Services*. In some cases this is as simple as shared packages around given protocols or infrastructure. But this model also allows for shared business logic etc. to be replicated across services via Autonomous Components. Validation is the canonical example here. 

Other *Microservice* architectural styles don't tend to consider shared components and so are agnostic on the concept of sharing modules between Services; however, *Self-Contained Systems* forbid shared business logic in shared modules.

## Stephen Tilkov - Self-Contained Systems  ##

**Self-Contained System (SCS)**: A *Service* that is full-stack (UI and backend), managed by a single team and communicates with other services asynchronously. An [SCS](https://scs-architecture.org/) is an opinionated *Microservice* to some extent, in that it doubles down on alignment with *Business Capability* in terms of sizing, and asynchronous communication and [does not allow Nanoservice creep](https://scs-architecture.org/vs-ms.html ). It's focus on a web front-end can make its description somewhat archaic in an API First world. It's strictness on shared *Autonomous Components* and asynchronous communication styles may remove some options.

## Obsolete ##

**Feature**: A unit of deployment at Allianz, that broadly corresponds to an *Application*. Usually hosted in an application container such as IIS or AWS Lambda over being self-hosted. We should prefer the term *Application* and recognise that they are not a logical deployment boundary, even if they are a physical unit of deployment.

**Vertical**: A collection of features that need to coordinate backlogs and deployments. Whilst Allianz originally used the term to mean something close to a *Microservice*, or *Self-Contained System*, the term has drifted to become an organisational unit and not a logical architectural unit. We would prefer not to use this term for architectural concepts as a result.
