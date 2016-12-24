Web Services
===

Web based applications that interact with other web applications for the purpose of exchanging data.

## What are Web Services?

A **web service** is any piece of software that makes itself available over the internet and uses a standardized `XML` messaging system.

*Example:* A client sends an `XML` message as `request` and expects an `XML` `response`.

The `request` and `response` being `XML` allows for communication between any platform that can create these requests and responses. It doesn't matter if the server is written in Perl and the client written in Swift.

To summarize, a complete web service is, therefore, any service that:

* Is available over the Internet or private (intranet) networks
* Uses a standardized `XML` messaging system
* Is not tied to any one operating system or programming language
* Is self-describing via a common `XML` grammar
* Is discoverable via a simple find mechanism

### Components of Web Services

The basic web services platform is `XML` + `HTTP`. All standard web services work using the following components

* `SOAP` (**S**imple **O**bject **A**ccess **P**rotocol)
* `UDDI` (**U**niversal **D**escription, **D**iscovery and **I**ntegration)
* `WSDL` (**W**eb **S**ervices **D**escription **L**anguage)

### How Does a Web Service Work?

A web service takes the help of:

* `XML` to tag the data
* `SOAP` to transfer a message
* `WSDL` to describe the availability of service

## Web Services - Architecture

There are two ways to view the web service architecture:

1. The first is to examine the individual roles of each web service actor.
2. The second is to examine the emerging web service protocol stack.

### Web Service Roles

There are 3 major roles within the web service architecture

#### Service Provider
The provider of the web service. The provider implemetns the service and makes it available on the Internet.

#### Service Requestor
The requestor is any consumer of the web service

#### Service Registry
A logically centralized directory of services. The registry provides a central place where developers can publish new services or find existing ones.

***
### Web Service Protocol Stack

#### Service Transport
This layer is responsible for transporting messages between applications. This layer includes:

* HTTP
* SMTP
* FTP
* BEEP

#### XML Messaging
This layer is responsible for encoding messages in a common `XML` format so that messages can be understood at either end. This layer includes

* XML-RPC
* SOAP

#### Service Description
This layer is responsible for describing the public interface to a specific web service. This is handled via the Web Service Description Language (**WSDL**)

#### Service Discovery
This layer is responsible for centralizing services into a common registry and providing easy publish/find functionality. This is currently handled via **UDDI**.

## Web Services - Components

### SOAP

SOAP is an XML-based protocol for exchanging information between computers.

### WSDL

WSDL is an XML-based language for *describing* web services and how to access them.

### UDDI 
UDDI is an XML-based standard for *describing*, *publishing*, and *finding* web services.