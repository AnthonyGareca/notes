# The Twelve-Factor App

[back](../README.md)

## Introduction

web apps are also called software-as-a-service (SAAS). The twelve-factor app is a methodology for building SAAS that:

* Use **declarative** formats for setup automation. (Minimize time and cost for new developers joining the project)
* Have a **clean contract** with the operating system. (offer maximum portability between execution environments)
* Are suitable for **deployment on modern cloud platform**. (obviating the need for servers and systems administration)
* **Minimize divergence** between development and production. (enabling continuous deployment for maximum agility)
* Can **scale up** without significant changes to tooling, architecture, or development practices.

## I. Codebase

One codebase tracked in revision control, many deploys.

A twelve-factor app is tracked in a version control system (Git, Mercurial, Subversion). A copy of the revision tracking database is know as a code repository, code repo, or just repo.

A codebase is any single repo (Subversion), or any set of repos who share a root commit (Git).

There is always a one-to-one correlation between the codebase and the app:

* If there are multiple codebases, it's not an app it's a distributed system. (Each component in a distributed system is an app)
* Multiple apps sharing the same code is a violation of twelve-factor. (The solution is to factor shared code into a libraries which can be include through the dependency manager)

There is a only one codebase per app, but there will be many deploys of the app. A deploy is a running instance of the app. The code base is the same across all deploys, although different versions may be active in each deploy.

## II. Dependencies

Explicitly declare and isolate dependencies.

Most programming languages offer a packaging system for distributing support libraries (NuGet, NPM, Yarn, etc). Libraries installed through a packaging system can be installed system-wide or scoped into the directory containing the app.

A **twelve-factor app never relies on implicit existence of system-wide packages**. It declares all dependencies, completely and exactly, via a *dependency declaration* manifest (Python - Pip). Furthermore, ir uses a *dependency isolation* tool during execution to ensure that no implicit dependencies "leak in" from the surrounding system (Python - Virtualenv).

No matter what the toolchain, dependency declaration and isolation must always be used together, only one or the other is not sufficient to satisfy twelve-factor.

One benefit of explicit dependency declaration is that it simplifies setup for developers new to the app.

Twelve-factor app also do not rely on the implicit existence of any system tools. If the app needs to shell out to a system tool that tool should be vendored into the app.

## III. Config

Store config in the environment.

An app's *config* is everything that il likely to vary between deploys. This includes:

* Resource handles to the database, Memcached, and other backing services.
* Credentials to external services such as Amazon S3 on Twitter.
* Per-deploy values such as their canonical hostname for the deploy.

Apps sometimes store config as constants in the code. This is a violation of twelve-factor which requires **strict separation of config from code**. Config varies substantially across deploys, code does not.

A litmus test for whether an app has all config correctly factored out of the code is whether the codebase could be made open source at any moment, without compromising any credentials.

Note that this definition of "config" does **not** include internal application config. This type of config does not vary between deploys, and so is best done in the code.

Another approach to config is the use of config files which are not checked into revision control. It still has weakness: it's easy to mistakenly check in a config file to the repo; there is a tendency for config files to be scattered about in different places and different formats, making hard to see and manage all the config in one place.

The twelve-factor app stores config in environment variables. Env vars are easy to change between deploys without changing any code; unlike config files.

Another aspect of config management is grouping. Sometimes apps batch config into named groups (environments) named specific deploys. This method does not scale cleanly.

In a twelve-factor app, env vars are granular controls, each fully orthogonal to other env vars. They are never grouped together as "environments", but instead are independently managed for each deploy. This is a model tha scales up smoothly as the app naturally expands into more deploys over its lifetime.

## IV. Backing Services

Tread backing services as attached resources.

A backing service is any service the app consumes over the network as part of its normal operation. Datastores (MySQL, CouchDB), massaging/queueing systems (RabbitMQ, Beanstalkd), SMTP services for outbound emails (Postfix), and caching systems (Memcached).

Backing services like the databases are traditionally managed by the mane systems administrators who deploy the app's runtime. In addition to these locally-managed services, the app may also have services provided and managed by third parties. SMTP (Postmark), metrics-gathering services (New Relic, Loggly), binary asset services (Amazon S3), and even API-accessible customer service (Twitter, Google Maps, Last.fm).

The code for a twelve-factor app makes no distinction between local and third party services. To the app, both are attached resources, accessed via a URL or other locator/credentials stored in the config. A deploy of the twelve-factor app should be able to swap out la local MySQL database with one managed by a third party without any changes to the app's code. Only the resource handle in the config needs to change.

Each distinct backing service is a resource.

Resources can be attached to and detached from deploys at will. All without any code changes.

## V. Build, Release and Run

Strictly separate build and run stages.

A codebase is transformed into a deploy through three stages:

* The **build stage** is a transform which converts a code repo into an executable bundle known as a *build*. Using a version of the code at a commit specified by the deployment process, the build stage fetches vendors dependencies and compiles binaries and assets.
* The **release stage** takes the build produced by the stage and combines it with the deploy's current config. The resulting release contains both the build and the config and is ready for immediate execution in the execution environment.
* The **run stage** (runtime) runs the app in the execution environment, by launching some set of the app's processes against a selected release.

The twelve-factor app uses strict separation between the build, release, and run stages.

Deployment tools typically offer release management tools, most notably the ability to roll back to a previous release.

Every release should always have a unique release ID, such as a timestamp of the release or an incrementing number. Releases are an append-only ledger and a release cannot be mutated once it is created. Any change must create a new release.

Builds are initiated by the app's developers whenever new code is deployed. Runtime execution, by contrast, can happen automatically in cases such as a server reboot, or a crashed process being restarted by the process manager.

## VI. Processes

Execute the app as one or more stateless processes.

The app is executed in the execution environment as one or more *processes*.

In the simplest case, the code is a stand-aline script, the environment is a developer's local laptop with an installed language runtime, and the process is launched via the command line. On the other end of the spectrum, a production deploy of a sophisticated app may use many process types, instantiated into zero or more running processes.

Twelve-factor processes are stateless and share-nothing. Any data that needs to persist must be stored in a stateful backing service, typically a database.

The memory space or filesystem of the process can be used as a brief, single-transaction cache. For example, downloading a large file, operating on it, and storing the result of the operations in the database. The twelve-factor app never assumes that anything cached in memory or on disk will be available on a future request or job - with many processes of each type running, chances are high that a future request will be served by a different process. Even when running only one process, a restart will usually wipe out all local state.

Some web systems rely on "sticky sessions" - that is, catching user session data in memory of the app's process and expecting future request from the same visitor to be routed to the same process. Sticky sessions are a violation of twelve-factor and should never be used or relied upon. Session state data is a good candidate for a datastore that offers time-expiration (Memcached, Redis).

## VII. Port Binding

Export services via port binding.

Web apps are sometimes executed inside a webserver container.

The twelve-factor app is completely self-contained and does not rely on runtime injection of a webserver into the execution environment to create a web-facing service. The web app exports HTTP as a service by binding to a port, and listening to request coming in on that port.

In deployment, a routing layer handles routing request from a public-facing hostname to the port-bound web process.

This is typically implemented by using dependency declaration to add a webserver library to the app. This happens entirely in *user space*, that is, within the app's code. The contract with the execution environment is binding to a port to serve requests.

HTTP is not the only service that can be exported by port binding. Nearly any king of server software can be run via process binding to a port and awaiting incoming request (ejabberd, Redis).

Note also that the port-binding approach means that one app can become the backing service for another app, by providing the URL to the backing app as a resource handle in the config for the consuming app.

## VIII. Concurrency

Scale out the process model.

Any computer program, once run, is represented by one or more processes. Web apps have taken a variety of process-execution forms. The running process(es) are only minimally visible to the developers of the app.

In twelve-factor app, processes are a first class citizen. Processes in the twelve-factor app take string cues from the unix process model for running service daemons. Using this mode, the developer can architect their app to handle diverse workloads by assigning *each type of work* to a *process type*. (HTTP requests may be handled by a web process, long-running background task handled by a worker process)

This does not exclude individual process from handling their own internal multiplexing, via threads inside the runtime VM, or the async/evented model (EventMachine, Twisted, Node.js). But an individual VM can only grow so large (vertical scale), so the applications must also be able to span multiple processes running on multiple physical machines.

The process model truly shines when it comes time to scale out. The share-nothing, horizontally partitionable nature of twelve-factor app processes means that adding more concurrency is a simple and reliable operation. The array of process types and number of processes of each type is known as the *process formation*.

Twelve-factor app processes should never daemonize or write PID files. Instead, rely on the operating system's process manager (systemd, Foreman) to manage output streams, respond to crashed process, and handle user-initiated restarts and shutdowns.

## IX. Disposability

Maximize robustness with fast startup and graceful shutdown.

Twelve-factor app's are disposable, meaning they cab be started or stopped at the moment's notice. This facilitates fast elastic scaling, rapid deployment of code of config changes, and robustness of production deploys.

Processes should strive to **minimize startup time**. Ideally, a process takes a few seconds from the launch command is executed until the process is up and ready to receive request or jobs. Short startup time provides more agility for the release process and scaling up; and it aids robustness, because the process manager can more easily move processes to new physical machine whe warranted.

Processes shut down gracefully when they receive a SIGTERM (signal to terminate a process) signal from the process manager. Implicit in this model is that all jobs are reentrant (if multiple invocations can safely run concurrently on a single processor system), which typically is achieved by wrapping the results in a transaction, or making the operation idempotent (is the property of certain operations can be applied multiple times without changing the result beyond the initial application).

Processes should also be robust against sudden death, in the case of a failure in the underlying hardware. Either way, a twelve-factor app is architected to handle unexpected, non-graceful terminations.

## X. Dev/Prod Parity

Keep development, staging, and production as similar as possible.

Historically, there have been substantial gaps between development and production. These gaps manifest in three areas:

* The time gap: A developer may work on code that takes days, weeks, or even months to go into production.
* The personnel gap: Developers write code, ops engineers deploy it.
* The tools gaps: Developers may be using a stack, while the production deploy uses another stack.

Twelve-factor app is designed for continuous deployment by keeping the gap between development and production small.

* Make the time gap small: a developer may write code and have it deployed hours or even just minutes later.
* Make the personnel gap small: developers who wrote code are closely involved in deploying it and watching its behavior in production.
* Make the tools gap small: keep development and production as similar as possible.

Summarizing the above into a table:

||Traditional app|Twelve-factor app|
|-|-|-|
|Time between deploys|Weeks|Hours|
|Code authors vs code deployers|Different people|Same People|
|Dev vs production environments|Divergent|As similar as possible|

Backing services, such as the app's database, queueing system, or cache, is one area where dev/prod parity is important. Many languages offer libraries which simplify access to the backing service, including adapters to different types of services.

Developers sometimes find great appeal in using a lightweight backing service in their local environments, while a more serious and robust backing service will be used in production.

The twelve-factor developers resit the urge to use different backing services between development and production, even when adapters theoretically abstract away any differences in backing services. Differences between backing services mean that tiny incompatibilities crop up, causing code that worked and passed test in development or staging to fail in production. These types of errors create friction that disincentivizes continuous deployment. The cost of this friction and the subsequent dampening of continuous deployment is extremely high when considered in aggregate over the lifetime of an application.

Lightweight local services are less compelling than they once were. Modern backing services (Memcached, PostgreSQL, RabbitMQ) are not difficult to install and run thanks to modern packaging system (Homebrew, apt-get). Alternatively, declarative provisioning tools (Chef, Puppet) combined with light-weight virtual environments (Docker, Vagrant) allow developers to run local environments which closely approximate production environments. The cost of installing and using these systems is low compared to the benefit of dev/prod parity and continuous deployment.

Adapters to different backing service are still useful, because they make porting to new backing service relatively painless. But all deploys of the app should be using the same type and version of each of the backing services.

## XI. Logs

Tread logs as event streams.

Logs provide visibility into the behavior of a running app. In server-based environments they are commonly written to a file on disk (logfile); but this is only an output format.

Logs are the stream of aggregated, time-ordered events collected from the output streams of all running processes and backing services. Logs in their raw form are typically a text format with one event per line. Logs have no fixed beginning or end, but flow continuously as long as the app is operating.

A twelve-factor app never concerns itself with routing or storage of its output stream. It should not attempt to write to or manage logfiles. Instead, each running process writes its event stream, unbuffered, to *stdout* (standard output). During local development, the developer will view this stream in the foreground of their terminal to observe the app's behavior.

In staging or production deploys, each process' stream will be captured by the execution environment, collated together with all other streams from the app, and routed to one or more final destinations for viewing and long-term archival. These archival destinations are not visible to or configurable by the app,and instead are completely managed by the execution environment. (Logplex, Fluentd)

The event stream for an app can be routed to a file, or watched via realtime tail in a terminal. Most significantly, the stream can be sent to a log indexing and analysis system (Splunk), or a general-purpose data warehousing system (Hadoop/Hive). These systems allow for great power and flexibility for introspecting an app's behavior over the time, including:

* Finding specific events in the past.
* Large-scale graphing of trends (requests per minute).
* Activate alerting according to user-defined heuristic (quality of errors per minute exceeds a certain threshold).

## XII. Admin Processes

Run admin/management tasks as one-off processes.

The process formation is the array of processes that are used to do the app's regular business as it runs. Separately, developers will often wish to do one-off administrative or maintenance task for the app, such as:

* Running database migrations.
* Running a console to run arbitrary code or inspect the app's model against the live database.
* Running one-time script committed into the app's repo.

One-off admin process should be run in an identical environment as the regular long-running process of the app. They run against a release, using the same codebase and config as any process run against the release. Admin code must ship with application code to avoid synchronization issues.

The same dependency isolation techniques should be used on all process types.

Twelve-factor strongly favors languages which provide a REPL (Read-Eval-Print-loop) shell out of the box, and which make it easy to run one-off scripts. In a local deploy, developers invoke one-off admin processes by a direct shell command inside the app's checkout directory. In a production deploy, developers can use ssh or other remote command execution mechanism provided by that deploy's execution environment to run such a process.
