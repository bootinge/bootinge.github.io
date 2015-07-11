---
layout: default
title: load_testing
---

# Load Testing Tools
## Grinder
### Key Features
- TCP proxy - records network activity into the Grinder test script
- Distributed testing - can scale with the increasing number of agent instances
- Power of Python or Closure combined with any Java API for test script creation or modification
- Flexible parameterization which includes creating test data on-the-fly and the capability to use external data sources like files, databases, etc.
- Post processing and assertion - full access to test results for correlation and content verification
- Support of multiple protocols

## Gatling
### Key Features
- HTTP Recorder
- An expressive self-explanatory DSL for test development
- Scala-based
- Produces higher load by using an asynchronous non-blocking approach
- Full support of HTTP(S) protocols & can also be used for JDBC and JMS load testing
- Multiple input sources for data-driven tests
- Powerful and flexible validation and assertions system
- Comprehensive informative load reports

## Tsung
### Key Features
- Distributed by design
- High performance. Underlying multithreaded-oriented Erlang architecture enables the simulation of thousands of virtual users on mid-end developer machines
- Support of multiple protocols
- A test recorder which supports HTTP and Postgres
- OS monitoring. Both the load generator and application under the test operating system metrics can be collected via several protocols
- Dynamic scenarios and mixed behaviours. The flexible load scenarios definition mechanism allows for any number of load patterns to be combined in a single test
- Post processing and correlation
- External data sources for data driven testing
- Embedded easy-readable load reports which can be collected and visualized during load

## JMeter
### Key Features
- Cross-platform. JMeter can be run on any operating system with Java
- Scalable. When you need to create a higher load than a single machine can create, JMeter can be executed in a distributed mode - meaning one master JMeter machine will control a number of remote hosts.
- Multi-protocol support. The following protocols are all supported ‘out-of-the-box’: HTTP, SMTP, POP3, LDAP, JDBC, FTP, JMS, SOAP, TCP
- Multiple implementations of pre and post processors around sampler. This provides advanced setup, teardown parametrization and correlation capabilities
- Various assertions to define criteria
- Multiple built-in and external listeners to visualize and analyze performance test results
- Integration with major build and continuous integration systems - making JMeter performance tests part of the full software development life cycle

