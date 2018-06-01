# Pushing a Microservice to Cloud Foundry

Having [built a microservice](./how-to-build-a-microservice.md) your next goal is to deploy your microservice into the shared Cloud Foundry space.

The Cloud Foundry CLI command to send your code to the cloud is `cf push`.  In a production environment we would have our CI automatically push to the development space.  For the purposes of the CW Portal demo, however, we will push manually.

[Creating a Manifest](./pushing-a-microservice.md#creating-a-manifest)

[Pushing to Cloud Foundry](./pushing-a-microservice.md#pushing-to-cloud-foundry)

[Viewing Logs](./pushing-a-microservice.md#viewing-logs)

## Creating a Manifest

It is possible to retype your `cf push` parameters each time you push, but to standardize the push process it is advisable to create a manifest file.

The following is a valid manifest YAML file.  Brief explanations of the parameters follow.

```
--- 
applications: 
  - 
    buildpack: java_buildpack
    instances: 1
    memory: 750M
    name: person
    path: C:\Users\jerry.hagen\Documents\GitHub\person\build\libs\person-0.0.1-SNAPSHOT.jar
    services: 
      - cw-portal-config-server-encrypted
      - cw-portal-service-registry
      - person-mysql
    timeout: 180
```

* The **buildpack** is a standard Cloud Foundry repository which contains detection, dependency and release information to the app.  If a buildpack parameter is not provided, Cloud Foundry will attempt to detect the proper buildpack based on the release file (for example, it will build with the Java buildpack if it detects a jar).  However it is best to specify the buildpack to save time during the push.
* The **instances** parameter defines the initial scaling for the app.  In a production environment this parameter will be at least 2 and typically much higher; the CW Portal demo app is not likely to support many simultaneous users and so 1 instance per microservice is sufficient.  Horizontal scaling (increasing or decreasing the number of app instances) can be accomplished while the app is running through the use of the `cf scale` command.
* The **memory** parameter defines the maximum allowable memory each instance is allowed to use.  Scaling the memory is called vertical scaling; unlike horizontal scaling this cannot be accomplished while the app is running, so in a production environment it is important to get this value right from the outset.
* The **name** parameter is the application name.  Your app will be available to inspect through the CLI or through the Pivotal console at https://run.pivotal.io under this name.
* The **path** parameter is the path to your application file on your local filesystem.  This is the file that will be uploaded to Cloud Foundry.
* The **services** parameter defines the services that are bound to this app.  This parameter should always contain `cw-portal-config-server-encrypted` and `cw-portal-service-registry`.  It may also contain a database service that you have defined.  Apps may be bound as services to other apps as well.
* The **timeout** parameter defines how many seconds to allow each instance to start up before throwing a fatal error.  The Eureka service registry in particular takes some time to bind on startup, so increasing the timeout beyond the default of 60 seconds is required.

## Pushing to Cloud Foundry

After defining a valid manifest file, you may push your app to Cloud Foundry using the parameters defined in your manifest file.  For example, the following version of `cf push` uses the `-f` flag to push using a local copy of the manifest file defined above.

`cf push -f <local path to person app>\person\person-manifest.yml`

Your app will be moved to Cloud Foundry, staged for deployment automatically, and started up.  If you have defined your manifest properly and have no other issues which prevent proper startup, the app will start after a couple minutes.

## Viewing Logs

Frequently application misconfigurations prevent proper startup.  In a production implementation, a log drain would be bound as a service for efficient log viewing.  For single app log viewing, the default is for application logs to be viewed via the CLI, or via the Pivotal console.  Check the documentation on `cf logs` for more information.