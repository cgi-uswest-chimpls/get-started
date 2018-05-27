So you have decided to contribute to the CGI US West Chicago/Minneapolis Metro CW Portal project.  Thank you!  Here are a few things you need to know in order to get started.

## Cloud Foundry Setup

Pivotal Cloud Foundry is lightweight.  It is possible to host your own local CF instance, but that is beyond the scope of this project.  In order to interact with our public space you merely need the following:

* An account on Pivotal Web Services (run.pivotal.io).  Contact Jerry at jerry.hagen@cgi.com to get an invite to our PWS organization.
* An instance of the Cloud Foundry CLI (Command Line Interface).  Go to https://github.com/cloudfoundry/cli and follow the instructions to download and install.  Assuming you have a standard CGI Windows laptop, you will be able to run CF commands from a command prompt as long as your PATH variable points to the CLI.

To log into our shared CW Portal development space from a command prompt, use the following command:  

`cf login -a https://api.run.pivotal.io -u <YOUR PWS USER NAME> -o cgi-uswest-chimpls -s development`
  
Of course you will then be asked for a password; it is your password you used when you signed up at PWS.

It's also highly encouraged to create your own trial org on Pivotal Web Services, for your own personal learning purposes.  Trial orgs are valid for one year or until you upgrade to a paid plan (which you only need to do if you plan on using above 2GB of memory at a time, i.e. multiple microservices or many instances).  You can log into your own personal org/space by substituting your org/space names for the -o and -s parameters in the login command.

## Individual Topics

[How to Create and Bind a Database in Cloud Foundry](./database-binding.md)