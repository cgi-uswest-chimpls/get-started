# How to Create and Bind a Database in Cloud Foundry

The standard Pivotal Cloud Foundry service marketplace contains third-party service offerings for MySQL, PostgreSQL, and MongoDB.  Other databases such as Oracle are available via adding their respective brokers to a PCF organization.  Add-on tiles are beyond the scope of this guide.

The following process will create and bind a MySQL database service to an application.  Binding a PostgreSQL or MongoDB service follows a similar process, as do other non-database marketplace services.

[Creating a Database Service](./database-binding.md#creating-a-database-service)

[Deleting a Service](./database-binding.md#deleting-a-service)

[Binding and Unbinding Services via the CLI](./database-binding.md#binding-and-unbinding-services-via-the-cli)

[Binding Services via the Application Manifest](./database-binding.md#binding-services-via-the-application-manifest)

[Service Keys](./database-binding.md#service-keys)

## Creating a Database Service

All marketplace service bindings use the `cf create-service`, or `cf cs`, command.  For detailed documentation on this command, consult the Cloud Foundry help using `cf cs -h`.

The standard form of the `create-service` command is:

`cf create-service <SERVICE> <PLAN> <SERVICE_INSTANCE_NAME>`

The MySQL marketplace service is cleardb.  The cleardb service offerings include spark, boost, amp, and shock.  Of these, spark is the trial service, suitable for a demo application such as version 0 of the CW Portal.

The following command creates a cleardb instance on the spark (free) plan and makes it available in our spaces under the name person-mysql:

`cf cs cleardb spark person-mysql`

Service instance provisioning takes a few minutes.  To check the status of the new service, access the Services tab in your space within the Pivotal Web Services online console (console.run.pivotal.io), or run the `cf services` command in the CLI.  The web console tends to update faster than the CLI.

## Deleting a Service

To delete a service, use the `delete-service` command.  Pivotal Cloud Foundry will not allow the deletion of services with active application bindings; the services must first be unbound from all apps using the `cf unbind-service` command.

## Binding and Unbinding Services via the CLI

There are multiple ways to bind services to an active application.  The service binding sets fields related to service in the `VCAP_SERVICES` environment variable within the application, exposing the service to the application.

The `cf bind-service` command accomplishes a manual binding.  The standard form of this command is:

`cf bind-service <APP_NAME> <SERVICE_INSTANCE_NAME>`

Its corollary is `cf unbind-service`:

`cf unbind-service <APP_NAME> <SERVICE_INSTANCE_NAME>`

## Binding Services via the Application Manifest

Including service bindings in the application manifest allows for automatic binding each time the application is pushed to Cloud Foundry.  When iterating rapidly, this feature is useful to avoid repeated manual binding.

To bind services via the manifest, include the following property in your manifest yml file:

```
    services: 
      - <service 1 name>
      - <service 2 name>
      - ...
```

When you push an application using the manifest, the services will be auto-bound at the beginning of the push, before your application container is created.

## Service Keys

From time to time it is useful to connect entities outside Cloud Foundry to a Cloud Foundry service.  For example, one may wish to populate a cleardb MySQL instance via MySQL Workbench on a local PC.

In order to create a service key, use the `create-service-key` command.  The example below will create a service key named `person-mysql-key` for a service named `person-mysql`.

`cf create-service-key person-mysql person-mysql-key`

To list service keys for a given service instance, use the `cf service-keys` command.

To get credentials for a service key, use `cf service-key`.  For example, to show credentials for the `person-mysql-key` created above, use the following command:

`cf service-key person-mysql person-mysql-key`

This will return parameters for service access, including a username/password that can be used to authenticate from an external client such as MySQL Workbench.
