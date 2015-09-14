# Spring Cloud Services Demo Application

### Prerequisites

You must have a Cloud Foundry organization on
[Pivotal Web Services (PWS)](https://run.pivotal.io) with access to the Spring
Cloud Services BETA services to use this tutorial. Talk to a Pivotal employee
at the Pivotal booth if you're interested in getting access to Spring
Cloud Services.

## PWS Setup

Fortune Teller is a simple application that can be used to demonstrate the
features of Spring Cloud Services (SCS). In this tutorial, we will show how
easy it is to provision and use SCS services on Pivotal CF.

You should have been invited to a PWS organization and space with access to the
Spring Cloud Services BETA.

1. Point `cf` client to your Spring Cloud Services enabled space on PWS.

  From a terminal run the following:

  ```console
  cf api api.run.pivotal.io
  cf login
  cf target -o YOUR_SCS_ORG -s YOUR_SCS_SPACE
  ```

  Replace `YOUR_SCS_ORG` and `YOUR_SCS_SPACE` with the name of the PWS
  organization and space provided to you by a Pivotal employee.

1. Create the services Fortune Teller will need to run

  Fortune Teller requires a database, a Spring Config Server, a service
  breaker dashboard, and a service registry. Thanks to PWS and Spring Cloud
  Services, these services are simple to provision.

  From the command-line, run the following to provision the services required
  by Fortune Teller.

  ```console
  cf create-service elephantsql turtle fortune-db
  cf create-service p-config-server standard config-service
  cf create-service p-service-registry standard service-registry
  cf create-service p-circuit-breaker-dashboard standard circuit-breaker
  ```

  It will take a moment for PWS to provision all of these services.

## Setup the Config Service

[Spring Cloud Config](http://cloud.spring.io/spring-cloud-config/) allows you
to store configuration properties outside of your application.

The Config Service on PWS uses Spring Cloud Config to fetch configuration
properties from a Git or Subversion repository. For this tutorial, we will
point the `config-service` instance to a configuration repository on GitHub.

1. Point the PWS config-service to configuration on GitHub

  Open the [PWS Console](https://console.run.pivotal.io/organizations) in your
  browser. Use the **ORG** drop down on the left to select the org you are using
  for Spring Cloud Services and then select your space below that. Under
  **Services**, you should see the `config-service` service. Click on the
  **Manage** link.

  When the Config Service dashboard has opened, select **Git** and in the
  **Git URI** input box, enter the URL of the Sample Spring Cloud Config GitHub
  repository, https://github.com/springone2gx2015/sample-spring-cloud-config.
  Then click on the **Submit** button to update the Config Server.

1. (Optional) Fork the sample configuration repo

    [Open the sample configuration repository on GitHub (https://github.com/springone2gx2015/sample-spring-cloud-config)](https://github.com/springone2gx2015/sample-spring-cloud-config).
    Now, click on the "Fork" button near the upper right corner of the browser.
    Use the URL of the forked repostory in the Config Service dashboard. This
    will allow you to change your Spring Cloud Config properties.

## Deploy Fortune Teller applications

1. Clone the Fortune Teller application

  Download the Fortune Teller demo application from GitHub.

  ```console
  git clone https://github.com/spring-cloud-samples/fortune-teller
  cd fortune-teller
  ```

1. Build the Fortune Teller service and UI

  We will be deploying two applications to PWS: the Fortune Teller Fortune
  Service, and the Fortune Teller UI.

  To build these applications, run the following from the root of the
  Fortune Teller project cloned from GitHub:

  ```console
  cd fortune-teller-fortune-service
  mvn package
  cd ../fortune-teller-ui
  mvn package
  ```

  This will build Spring Boot `.jar` files that can be deployed to PWS.

1. Deploy the Fortune Teller Apps to PWS

  Once you have built the Fortune Teller Fortune Service and UI, you can Deploy
  them to PWS by running the following:

  ```console
  cd ../fortune-teller-ui
  cf push
  cd ../fortune-teller-fortune-service
  cf push
  ```

  It will take a few minutes for the Fortune Teller UI and Service to deploy.

  Once your applications have deployed, let's test them to make sure they are
  running correctly. When you pushed Fortune Teller to Cloud Foundry, you were
  dynamically given a unique host name. To see what your hostname is, run from
  the terminal:

  ```console
  cf apps
  ```

To the right of `fortune-ui` and the **urls** heading, you should see something
like `fortunes-ui-chimneyless-kine.cfapps.io`. Copy and paste your URL into
your browser. You should see "Fortune Teller!" in large text and a future below
it. If the future reads "Your future is unclear." it means the future-service
is not available. This may be because the future-service hasn't started or is
still initializing. Once fortune-service is running, you will have the
necessary clairvoyance to know what your future holds.

##(Optional) Deploying Fortune Teller applications with STS

1. Open “Boot Dashboard” view via Window -> Show View -> Others… find “Boot Dashboard”. Alternatively, locate and click “Boot Dashboard” button on the main toolbar to open “Boot Dashboard” view. ![Boot Dashboard Button](/images/boot-dash-button.png) 
2. The “Boot Dashboard” view opens up. It should have Fortune Teller applications listed under “Local” run target. ![Boot Dashboard View](/images/boot-dash-view-local.png)
3. Click on the ‘+’ button in the “Boot Dashboard” view toolbar to Create a new Cloud Foundry connection pointing to the Organization and Space where Fortune Teller application services are about to be deployed. ![Boot Dashboard Add CF Target](/images/boot-dash-add-target.png)
4. Wizard page pops up prompting to fill in Cloud Foundry credentials and select a space. Ones info is filled in “Finish” button becomes enabled at the bottom. Click it when ready. ![Boot Dashboard Add CF Target Wizard](/images/boot-dash-add-target-wizard.png)
5. Cloud Foundry target should appear under the “Local” it should have Cloud Foundry Organization, Space, URL and possibly some apps under it. ![Boot Dashboard CF Target](/images/boot-dash-cf-target.png)
6. Open (double-click) manifest.yml file under fortune-teller-fortune-service project in Package Explorer. YML editor for the file is opened. Comment out the line starting with “path” as IDE builds and packages services for deployment on CF by itself without help from Maven. ![Boot Dashboard Fix deployment manifest](/images/boot-dash-fix-manifest.png)
7. Perform the step above on manifest.yml file under fortune-teller-ui project.
8. Multi-select fortune-teller-ui and fortune-teller-fortune-service under “Local” in the “Boot Dashboard” view. ![Boot Dashboard select local apps](/images/boot-dash-select-fortune-teller-apps.png)
9. Drag And Drop selected fortune-teller-ui and fortune-teller-fortune-service under “Local” onto the created Cloud Foundry target from step 5 above. Both applications should start being deployed on Cloud Foundry. ![Boot Dashboard starting CF apps](/images/boot-dash-starting-fortune-teller.png)
10. Wait until both apps are running on the Cloud Foundry. ![Boot Dashboard running CF apps](/images/boot-dash-running-fortune-teller.png) 
11. Double-click on the fortune-ui app under Cloud Foundry run target in the “Boot Dashboard” view. Browser should be opened (either Eclipse’s internal or external browser) showing the Fortune Teller application UI running. ![Boot Dashboard running CF apps](/images/fortune-teller-ui.png)


## Troubleshooting

The Spring Cloud Service dashboards provide a lot of valuable information for
troubleshooting. To use the SCS dashboards, open the
[PWS Console](https://console.run.pivotal.io/organizations) in your browser.
Use the **ORG** drop down on the left to select the org you are using for
Spring Cloud Services and then select your space below that. Under
**Services**, you should see all of the services used by Fortune Teller.

We have already looked at the Config Server. Let's look at the service registry.
Click on the **Manage** link below `service-registry` then click on the
**Service Registry Dashboard** link.

You should see two registered apps: FORTUNES and UI. These are the two apps we
deployed to Cloud Foundry. If one of these apps is missing, it's likely either
not configured properly or not running.

## (Optional) Dynamically update Config Server configured

If you have not cloned the [sample configuration repository on GitHub (https://github.com/springone2gx2015/sample-spring-cloud-config)](https://github.com/springone2gx2015/sample-spring-cloud-config), do so now and **point your `config-service` to the cloned
repository** using the PWS console and Config Service dashboard. (See the
_Setup the Config Service_ section for more details on how to do this.)

When the `fortune-service` application isn't running or isn't functioning, the
UI will display the fortune "Your future is unclear." To change this, use your
favorite Git client to clone your forked `sample-spring-cloud-config`
repository onto your workstation and edit the `application.yml` file. You
should see the `fortune.fallbackFortune` property commented out. Uncomment this
property and change it to your own fallback fortune. Commit the change, and
push it up to GitHub.

Now stop the `fortune-service` application by running the following from a
terminal:

```console
cf stop fortune-service
```

Now restart the UI application by running:

```console
cf restart fortune-ui
```

Refresh the Fortune Teller UI in your browser and you should see your
customized fallback message.

Start the `fortune-service` application back up to start seeing regular
fortunes again.

```
cf start fortune-service
```
