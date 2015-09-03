## FE
FE will need to create a space for the attendee in the **as yet not created PWS
org** for the attendee.

```
$ cf create-space attendee-name?
$ cf set-space-role attendee_pws_email_address NAME_OF_ORG_NOT_YET_CREATED NAME_OF_SPACE_CREATED_FOR_ATTENDEE SpaceManager
$ cf set-space-role attendee_pws_email_address NAME_OF_ORG_NOT_YET_CREATED NAME_OF_SPACE_CREATED_FOR_ATTENDEE SpaceDeveloper
```

## attendee

1. Point `cf` to your Spring Cloud Services enabled space on PWS.
```
$ cf api api.run.pivotal.io
$ cf target -o NAME_OF_ORG_NOT_YET_CREATED -s NAME_OF_SPACE_CREATED_FOR_ATTENDEE
```

2. Create the services Fortune Teller will need to run

  Fortune Teller requires a MySQL database, a Spring Config Server, a service
  breaker dashboard, and a service registry. Thanks to PWS and Spring Cloud
  Services, these services are simple to provision.

  From the command-line, run the following to provision the services required
  by Fortune Teller.

  ```
  $ cf create-service p-mysql 100mb fortune-db
  $ cf create-service p-config-server standard config-service
  $ cf create-service p-service-registry standard service-registry
  $ cf create-service p-circuit-breaker-dashboard standard circuit-breaker
  ```

  It will take a moment for PWS to provision all of these services.

3. Clone the Fortune Teller application

  Download the Fortune Teller demo application from GitHub.

  ```
  $ git clone https://github.com/spring-cloud-samples/fortune-teller
  $ cd fortune-teller
  ```

4. Build the Fortune Teller service and UI

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

5. Deploy the Fortune Teller Apps to PWS

  Once you have built the Fortune Teller Fortune Service and UI, you can Deploy
  them to PWS by running the following:

  ```
  cf push -f manifest-apps.yml
  ```

  Let's see if the apps deployed correctly. 
