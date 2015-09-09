# Spring Cloud Services Demo Application

### Prequequisits

You must have a Cloud Foundry organzation on [Pivotal Web Services (PWS)](https://run.pivotal.io) with access to the Spring Cloud Services BETA services
to use this tutorial. Talk to a Pivotal employee at the Pivotal booth if you're
interested in getting access to Spring Cloud Services.

You will also need to have a [GitHub](https://github.com) account.

## PWS Setup

Fortune Teller is a simple application that can be used to demonstrate the
features of Spring Cloud Services (SCS). In this tutorial, we will show how
easy it is to provision SCS services on Pivotal CF.

1. Point `cf` client to your Spring Cloud Services enabled space on PWS.

  From a terminal run the following:

  ```
  $ cf api api.run.pivotal.io
  $ cf target -o NAME_OF_ORG_NOT_YET_CREATED -s NAME_OF_SPACE_CREATED_FOR_ATTENDEE
  ```

1. Create the services Fortune Teller will need to run

  Fortune Teller requires a MySQL database, a Spring Config Server, a service
  breaker dashboard, and a service registry. Thanks to PWS and Spring Cloud
  Services, these services are simple to provision.

  From the command-line, run the following to provision the services required
  by Fortune Teller.

  ```
  $ cf create-service elephantsql turtle fortune-db
  $ cf create-service p-config-server standard config-service
  $ cf create-service p-service-registry standard service-registry
  $ cf create-service p-circuit-breaker-dashboard standard circuit-breaker
  ```

  It will take a moment for PWS to provision all of these services.

## Setup the Config Service

[Spring Cloud Config](http://cloud.spring.io/spring-cloud-config/) allows you
to store configuration properties outside of your application. When you
change the Spring Cloud Config managed properties, your application will
update automatically without the need for a restart.

The Config Service on PWS uses Spring Cloud to fetch configuration properties
from a Git or Subversion repository. For this tutorial, we will point
`config-service` to a configuration repository on GitHub.

1. Fork Matt Stine's demo config-repo

  [Open Matt Stine's configuration repository on GitHub.](https://github.com/mstine/config-repo).
  Now, click on the "Fork" button near the upper right corner of the browser.
  Take note of the URL of the forked repository. You will need it in the next
  step.

2. Point the PWS config-service to your cloned config-repo

  Open the [PWS Console](https://console.run.pivotal.io/organizations) in your
  browser. Select the NAME_OF_ORG_NOT_YET_CREATED on the left and then
  select your space. Under **Services**, you should see the `config-service`
  service. Click on the **Manage** link.

  When the Config Service dashboard has opened, select **Git** and in the
  **Git URI** input box, enter the URL of the forked GitHub repository from
  step 1.

## Deploy Fortune Teller applications

1. Clone the Fortune Teller application

  Download the Fortune Teller demo application from GitHub.

  ```
  $ git clone https://github.com/spring-cloud-samples/fortune-teller
  $ cd fortune-teller
  ```

1. Build the Fortune Teller service and UI

  We will be deploying two applications to PWS: the Fortune Teller Fortune
  Service, and the Fortune Teller UI.

  To build these applications, run the following from the root of the
  Fortune Teller project cloned from GitHub:

  ```
  $ cd fortune-teller-fortune-service
  $ mvn package
  $ cd ../fortune-teller-ui
  $ mvn package
  ```

  This will build Spring Boot `.jar` files that can be deployed to PWS.

1. Deploy the Fortune Teller Apps to PWS

  Once you have built the Fortune Teller Fortune Service and UI, you can Deploy
  them to PWS by running the following:

  ```
  cf push -f manifest-apps.yml
  ```

  Let's see if the apps deployed correctly.
