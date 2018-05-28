#How to Build a Cloud Foundry Microservice for the CW Portal

In principle, CW Portal microservices can be built on any platform provided they expose appropriate endpoints.  This guide will walk through setting up a microservice on two platforms - Java with Spring Boot, and .NET.

## How to Build a Java Microservice with Spring Boot

This guide assumes you have [Spring Tool Suite](https://spring.io/tools) installed locally as your IDE.

For this project all microservice apps will be built using Gradle.  It is also possible to build with Maven.

### Create a Spring Boot Project

Go to the [String Initializr](https://start.spring.io) site.  This helpful site will create a new project with options to include a number of common dependencies.

Find the "Switch to the full version" link and click it to open the full suite of options.

On the first line, "Generate a...", select Gradle Project in the first dropdown.  Verify that Java and the latest version of Spring Boot are selected in the other dropdowns.

For Group, enter `com.cgi.uswest.chimpls`.

For Artifact, enter the name of your microservice, which should be a one-word description of what the microservice does.  This should autopopulate your Name and Package Name as well.

The options you select will vary based on what you need enabled in your microservice, but all CW Portal Spring microservices will require at least the following:

* Core - Spring Security
* SQL - JPA and a JDBC driver for your database, or NoSQL as needed
* Pivotal Cloud Foundry - Config Client (PCF) and Service Registry (PCF)

Once you have selected all options you need, click Generate Project.  Your project zip file will automatically be downloaded.

### Import a Spring Boot Project into STS

### Build the Project

## How to Build a .NET Microservice 

under construction...