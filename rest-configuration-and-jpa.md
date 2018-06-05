The goal of a CW Portal microservice is to provide a set of related endpoints which provide access to, and change data in, an associated database.  In order to achieve this goal, an app will need to be configured to expose REST endpoints and query its database service.

## Adding REST Configuration

In order to configure your application to provide REST endpoints, you need to add a `RESTConfiguration` class, an application controller, and one or more objects.

### Object and Repository

For each table you intend to access from this microservice, you will need a persistence entity object.  This object must have the `@Entity` mapping.

It is recommended for one field in this class to be named id, of type UUID, as follows:

```
	@Id
	@GenericGenerator(name = "uuid2", strategy = "uuid2")
	@GeneratedValue(generator = "uuid2")
	@Column(columnDefinition = "BINARY(16)")
	private UUID id;
```

In addition this object should have one field for each column in the table, and a constructor that maps parameters to fields.

Also create a repository interface.  This interface exposes query methods to the table.  See the [documentation on Spring JPA Repositories](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories).

You may query using method names or custom queries; see [Section 2.4 of the Spring JPA documentation](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#repositories.query-methods.details) for details.

### RestConfiguration

Before proceeding, make sure your `build.gradle` file contains the dependency `compile('org.springframework.boot:spring-boot-starter-data-rest')` and re-sync.

The `RestConfiguration` class is boilerplate.  Here is the code for a person microservice:

```
package com.cgi.uswest.chimpls.person;

import org.springframework.context.annotation.Configuration;
import org.springframework.data.rest.core.config.RepositoryRestConfiguration;
import org.springframework.data.rest.webmvc.config.RepositoryRestConfigurerAdapter;

import com.cgi.uswest.chimpls.person.objects.Person;


@Configuration
public class RestConfiguration extends RepositoryRestConfigurerAdapter {

   @Override
    public void configureRepositoryRestConfiguration(RepositoryRestConfiguration config) {
        config.exposeIdsFor(Person.class);
    }
	
}
```

### Application Controller

Finally, you need to pull query information to expose in an endpoint.  Your application controller can do this.

This class is defined with the `@RestController` annotation and will contain one or more methods annotated with `@RequestMapping` annotations.  These annotations expose the resulting objects via REST endpoints.

