# Exercise 04 - Implement load testing and secure practices

## Task 1 - Implement load testing

Below is a sample load testing plan. Learners should have a plan with a similar overall feel, although some specific details may vary.

- Define what services and the scope of your testing
  - We will be simulating an average day when a user hits our website during normal business hours 8 AM - 5 PM.  We will be testing all endpoints to ensure all features are tested.  
- Environment Requirements
  - Because we are testing user workloads, we will need the following resources at the same level as production.
    - App Service
- Identify the load characteristics and scenario
  - Our current average daily load is 1000 users/hour.  We will mimic the average daily load by spinning up 30 concurrent threads, hitting all endpoints. Because there is a consistent daily load, we will mimic this with gradual load increasing linearly.
- Identify the test failure criteria
  - Our test will fail if we are unable to support 30 concurrent users, as we already know this is the current demand.
  - We will also consider this a failed test if we find performance below the expected threshold of 600ms per call on average or if more than 10% of calls fail, as this may impact customer satisfaction.
- Identify how you will be monitoring your application
  - We will be monitoring our applications with Application Insights to detect any errors and monitor performance.
- Identify potential bottlenecks or limitations in advance
  - Although we do not cache data, the fact that the application is using an in-memory database should mitigate any data transfer bottlenecks.
- Identify any assumptions or risks in your plan
  - We are assuming these are only casual end users and not other 3rd parties who could be trying to obtain information about our traffic.
  - We are using consumption-based model for Azure App Services.
  - The application does not make any database calls.

Reinforce the importance of using existing metrics, usage patterns, and data to inform the load testing plan on learners' own applications.

## Task 2 - Run a load test from GitHub Actions

- A sample JMeter script is in [the solutions folder](./Solution/Exercise-04/Task-2/LoadTestScript.jmx). This script covers steps 1 and 2.
  - Before creating or running the script, create an environment variable called `webapp` on the Dev Box. Its value should be the URL for your site, in the format `{yourprefix}-dev.azurewebsites.net` with no `https://` or trailing slashes. The following command line operation provides an example of how to create the environment variable:

    `setx webapp {yourprefix}-dev.azurewebsites.net`

  - The JMeter test plan includes the following components. Create them in the following order:
    - Thread Group to perform testing (right-click on the Test Plan and select Add -> Threads to add the Thread Group). The thread group should include 30 threads and 30 loops.
      - HTTP Request Defaults (right-click on the Thread Group and choose Add -> Config Element). Set the protocol to https and the server name or IP to your deployed website. The included script sets the server name to a variable called `${site}`, which uses the environment variable on the PC called `webapp`.
      - Recording Controller (Add -> Logic Controller)
    - HTTP(S) Test Script recorder (Add -> Non-Test Elements)
      - On the test script recorder, the default port is 8888.
    - View Results in Table (Add -> Listener). This allows you to view test results in a table and get a quick idea of how the website is performing during a test.
    - View Results Tree (Add -> Listener). This allows you to view individual test results and diagnose potential issues such as 400 Bad Request.
  - The following steps allow you to perform a test recording.
    - Start the recorder by selecting the Start button on the HTTP(S) Test Script Recorder element. The first time you do this, JMeter will generate a temporary root certificate in `C:\apache-jmeter-5.6.2\bin\` (or wherever you installed JMeter). Stop the recording by selecting the Stop button.
    - Although you can use any browser for testing, the Firefox browser is recommended for this because you can set a manual proxy and do not need to create a Windows-wide proxy like you would for Chrome or Edge. You can set a proxy in Firefox by navigating to the options menu and then choosing Settings. Then, search for "proxy" in the settings search bar and select the Settings button for network settings. When running the test script, use a manual proxy configured to localhost on port 8888, and use it for HTTP as well as HTTPS. After setting the proxy, search for "certificate" in the settings search bar and then select the View Certificates button. From there, navigate to Authorities and choose the Import button. Inside `C:\apache-jmeter-5.6.2\bin\` (or wherever you installed JMeter), select the `ApacheJMeterTemporaryRootCA.crt` certificate and import it into Firefox.
    - After enabling the proxy and importing the certificate, you should be able to return to JMeter and restart the test recording.
    - During the recording, you should capture activities like viewing the page, adding a message, deleting a message, and analyzing messages.
  - After stopping a test recording, you will have a series of test actions. You will need to capture the request verification token from the first call in order to ensure that you have a proper CSRF token for all subsequent calls.
    - Right-click on the first recorded message in the Recording Controller element and select Add -> Post Processors -> CSS Selector Extractor.
    - In the CSS Selector Extractor, set the following attributes:
      - Name of created variable = `token`
      - CSS Selector expression = `input[name=__RequestVerificationToken]`
      - Attribute = `value`
    - For each subsequent call, replace the Value of the "__RequestVerificationToken" parameter with `${token}`. You will only see this parameter on POST requests, not GET requests.
- The following instructions cover steps 3-7.
  - Create Azure Load Testing resource
    - Search for the Azure Load Testing resource on the top search bar and select the resource.
    - On the top left hand corner select "Create"
    - Select the Resource Group that was created by the sample app or create your own.  The resources must be in the same location.
    - Enter a unique name for the resource.
    - Ensure the location matches the location that your application is deployed to.
    - Select "Review + create"
    - Review your configurations and then select "Create"
  - Create Load Test
    - Go into your new resource.  You may see a warning on the top depending on your current access describing the access levels required.  If so, add yourself as one of the roles such as "Load Testing Owner" or "Load Testing Contributor" as you will not be able to proceed otherwise.
    - Select "Create"
    - Fill out the name of the test and a description of the test.
    - Go to the "Test Plan" tab and select the JMeter script you have locally and then select "Upload"
    - Under the "Parameters" tab, enter "webapp" for the name of the environment variable, followed by the URL for your sample app in the "Value" section **without** `https://`.
    - Under the "Monitoring" tab, select "Add/Modify" and select the App Service for the sample app followed by "Apply" in order to monitor the app during testing.
    - Select "Review + create"
    - Review the settings and select "Create"
  - Run the load test
    - In your Azure Load Test service, select "Tests" from the left hand side
    - Select the test that you created from the list
    - At the top of the screen select "Run"
    - A summary page should appear.  Add that this run is the baseline in the description.  Select "Run"
  - Set Pass/Fail Criteria
    - Go back to your load test and select "Configure" followed by "Test"
    - Go to the "Test Criteria" Tab.
    - Enter in your test criteria such as failure rate under x %
- The following high-level notes cover steps 8-10. In addition, [a GitHub Actions workflow YAML file](./Solution/Exercise-04/Task-2/LoadTest.yml) and [an Azure Load Test configuration file](./Solution/Exercise-04/Task-2/LoadTestConfig.yaml) are available in the solutions folder.
  - The file `LoadTest.yml` should go into `.github/workflows`. The files `LoadTestConfig.yaml` and `LoadTestScript.jmx` should go into the root of the repository. The file `load_test_report.md` should go into a new folder called `.github/ISSUE_TEMPLATE`. Make sure to update `load_test_report.md` to set the assignee to your username.
  - This is a sample action that will run a load test.  The default action will either create or run the existing load test. However, GitHub Actions will automatically lower-case the value of the "TestName" parameter inside `LoadTestConfig.yaml`. Because the UI allows upper or lowercase letters, this can lead to duplicate tests existing in the Azure Load Test workspace.
  - Learners can use the baseline from their first run in challenge 3 to determine pass/fail criteria.

## Task 3 - Identifying bottlenecks, stress tests, and resilience testing

- The following high-level notes cover step 1.
  - For a primer on identifying bottlenecks using Azure Load Testing, [this article](https://docs.microsoft.com/en-us/azure/load-testing/tutorial-identify-bottlenecks-azure-portal) covers the process.
  - Because there is no database associated with this application, identifying areas of slowdown will require code analysis.
  - Learners should notice that there are two operations which consistently lag behind everything else in terms of performance: initial loading of messages and posting a new message. In both cases, the root cause is a thread sleep loop. After removing these "speed loops," learners should notice a drastic performance improvement when performing those operations.
- The following high-level notes cover steps 2-4. There is also [a sample JMeter script](./Solution/Exercise-04/Task-3/StressTestScript.jmx) in the solution directory.
- The following high-level notes cover steps 5-8.
  - Chaos Studio does not offer a GitHub Action at this time, so learners will not be able to integrate this into their CI/CD pipelines.
  - Azure Chaos studio only supports a limited number of services, such as VMs, AKS, App Services, Cosmos DB, and networking.  Because our sample app only contains App Services, we will use App Service faults.
  - Prior to creating a chaos experiment, learners will want to scale out the App Service Plan to multiple instances, such as 3.
  - Create Azure Chaos Studio/Experiment
    - Register Chaos Studio Provider
      - Go to your subscription
      - On the left-hand side, select "Resource provider"
      - In the list of providers, search for "Microsoft.Chaos"
      - Click on the provider and select "Register"
    - Go to Azure Chaos Studio
    - Navigate to the **Targets** menu on the left-hand side and then select the App Service you will test. From there, select "Enable Targets" then "Enable service-direct targets" to complete enablement.
    - On the left-hand side menu for Chaos Studio, select the "Experiments" option.
    - Select "+ Create" and then choose "New experiment" from the dropdown.
    - Fill in your subscription, resource group, location, and name for this experiment.  Keep track of the experiment name as a managed user will be created for you.
    - Go to the experiment designer on the next tab.  Change the name of the step or branch if you wish.
    - Select "Add Action" and then "Add Fault" to create a new fault.
    - Select "Stop App Service" as the Fault.  You can choose the duration but the minimum duration is 5 minutes and that should be enough. From the target resources tab, choose the App Service you wish to test and then select "Add" to complete the addition.
    - Save your experiment by selecting "Review + create" and then choosing the "Create" option.
  - Update App Service Permissions
    - In the appropriate App Service, select "Access control (IAM)" from the left-hand menu.
    - Select "Add" followed by "Add Role Assignment"
    - Select the Contributor role, then select the "Members" tab. Choose the "+ Select members" link.
    - Search the name of the experiment from the earlier step and then click "Select".
    - Review and assign the permissions which will grant the role to the experiment.
  - Run load test + experiment
    - Run the load test first, then while the load test is running kick off the chaos experiment.  You should notice that the application returns a 403 error and the web app is stopped.
