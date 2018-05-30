# How to Build a Cloud Foundry Microservice Project for the CW Portal

In principle, CW Portal microservices can be built on any platform provided they expose appropriate endpoints.  This guide will walk through setting up a microservice on two platforms - Java with Spring Boot, and .NET.

## How to Build a Java Microservice with Spring Boot

This guide assumes you have [Spring Tool Suite](https://spring.io/tools) installed locally as your IDE.  It also assumes you have integrated Gradle.  How to integrate Gradle into your STS instance is the topic of another guide.

For this project all microservice apps will be built using Gradle.  It is also possible to build with Maven.

[Create a Spring Boot Project](./how-to-build-a-microservice.md#create-a-spring-boot-project)

[Import a Spring Boot Project into STS](./how-to-build-a-microservice.md#import-a-spring-boot-project-into-sts)

[How to Sync your Gradle Project with STS](./how-to-build-a-microservice.md#how-to-sync-your-gradle-project-with-sts)

[Build the Project](./how-to-build-a-microservice.md#build-the-project)

[Viewing Unit Test Results](./how-to-build-a-microservice.md#viewing-unit-test-results)

[Adding Bootstrap Configuration](./how-to-build-a-microservice.md#adding-bootstrap-configuration)

### Create a Spring Boot Project

Go to the [Spring Initializr](https://start.spring.io) site.  This helpful site will create a new project with options to include a number of common dependencies.

Find the "Switch to the full version" link and click it to open the full suite of options.

On the first line, "Generate a...", select Gradle Project in the first dropdown.  Verify that Java and the latest version of Spring Boot are selected in the other dropdowns.

For Group, enter `com.cgi.uswest.chimpls`.

For Artifact, enter the name of your microservice, which should be a one-word description of what the microservice does.  This should autopopulate your Name and Package Name as well.

The options you select will vary based on what you need enabled in your microservice, but all CW Portal Spring microservices will require at least the following:

* Core - Spring Security
* SQL - JPA, H2 (for local unit tests only), and a JDBC driver for your database; or NoSQL as needed
* Pivotal Cloud Foundry - Config Client (PCF) and Service Registry (PCF)

Once you have selected all options you need, click Generate Project.  Your project zip file will automatically be downloaded.

### Import a Spring Boot Project into STS

Extract your project zip file into the folder from which you intend to push it to GitHub later.

Within STS, open File...Import.  For the import wizard, select Gradle...Existing Gradle Project.  Click Finish to begin the import.

### How to Sync your Gradle Project with STS

When you import your project, you may find you have a buildable project which still is marked by STS as containing errors.  This is because Gradle does not automatically update the Eclipse classpath with its dependency jar files.

To sync your project and eliminate the phantom "errors" you can right-click the project in Package Explorer and navigate to Gradle...Refresh Gradle Project.

### Build the Project

The project may be built from within STS through the following steps.

Open the Gradle Tasks view.  Find your project in the folder list and navigate to build/build.  Right-click and select Run Gradle Tasks.

If you have a JRE installed which is not a JDK, the build may fail due to using the JRE as its Java Home and failing to find tools.jar.  To set the JRE manually, you can navigate to the build task, right-click and select Open Gradle Run Configurations, and select the correct Java Home (a full JDK, not a JRE) via the Java Home tab.  Unfortunately I have not yet found how to set the Java Home globally in your STS installation so this guide requires that you set Java Home on a project-by-project basis.

### Viewing Unit Test Results

The Spring Boot project comes with one test class pre-packaged.  If you have not specified any configuration yet, the `contextLoads` unit test will fail.  To view the test results, follow the file link in your `BUILD FAILED` message.  The default location is:  `\<project folder\build\reports\tests\test\index.html.`

The `contextLoads` test attempts to load all application dependencies in a runtime environment, so if configuration is missing this test will often catch it.

### Adding Bootstrap Configuration

Go to your project's src/main/resources folder.  Delete `application.properties`.  Create a new file and name it `bootstrap.yml`.  [YAML](https://en.wikipedia.org/wiki/YAML) is a human-readable standard used for configuration files in the cloud ecosystem.  Precise syntax and indenting are important in YAML, and configuration issues are often resolved by fixing an incorrectly indented line or adding proper spacings.

Add the following configuration to the new file:

```
---

spring:
  application:
    name: <YOUR MICROSERVICE NAME>
  cloud:
    config:
      uri: ${vcap.services.cw-portal-config-server-encrypted.credentials.uri:http://localhost:8888}
      
  security:
    user:
      name: <A USER NAME>
      password: <A PASSWORD>  

  jpa:
    hibernate:
      ddl-auto: update
```

This introduces the following configuration elements:

* An application name
* The URI of the configuration server service.  Per the [12 factors](https://12factor.net/) we [store configuration in the environment](https://12factor.net/config).  Thus we can change configuration without changing code, and all that is required to pick up new configuration files is an application restart.  If no cloud service uri is found in the environment (such as in local automated unit testing) then a local instance is created at `localhost:8888`.
* Basic authentication.  Simply hardcoding a username and password is a wholly unacceptable practice for a production application but will serve well to give our demo application the minimum of security while we build out features.
* Database initialization for Hibernate, useful for when we later [bind a database service](./database-binding.md).

At this point, when you execute the Gradle `build` task, it should succeed including the `contextLoads` unit test.  There may be stack traces involving the Eureka service.  Registering your service as a Eureka client is the topic of another guide.

## How to Build a .NET Microservice 

under construction...