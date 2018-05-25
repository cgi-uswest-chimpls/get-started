HOW TO CREATE AND BIND A DATABASE IN PIVOTAL CLOUD FOUNDRY

The standard Pivotal Cloud Foundry service marketplace contains third-party service offerings for MySQL, PostgreSQL, and MongoDB.  Other databases such as Oracle are available via adding their respective brokers to a PCF organization.  Add-on tiles are beyond the scope of this guide.

All marketplace service bindings use the cf create-service, or cf cs, command.  For detailed documentation on this command, consult the Cloud Foundry help using cf cs -h.

The standard form of the create-service command is:

cf create-service <SERVICE> <PLAN> <SERVICE_INSTANCE_NAME>

To delete a service, use the delete-service command.  Pivotal Cloud Foundry will not allow the deletion of services with active application bindings; the services must first be unbound from all apps using the cf unbind-service command.

MySQL

The MySQL marketplace service is cleardb.  The cleardb service offerings include spark, boost, amp, and shock.  Of these, spark is the trial service, suitable for a demo application such as version 1 of the CW Portal.

The following command creates a cleardb instance on the spark (free) plan and makes it available in our spaces under the name person-mysql:

cf cs cleardb spark person-mysql

Service instance initialization takes a few minutes.  To check the status of the new service, access the Services tab in your space within the Pivotal Web Services online console (console.run.pivotal.io), or run the cf services command in the CLI.  The web console tends to update faster than cf services.
