# Exercise 04 - Implement load testing and secure practices

## Task 1 - Implement load testing

Below is a sample load testing plan. Learners should have a plan with a similar overall feel, although some specific details may vary.

- Define what services and the scope of your testing
  - We will be simulating an average day when a user hits our website during normal business hours 8 AM - 5 PM.  We will be testing all endpoints to ensure all features are tested.  
- Environment Requirements
  - Because we are testing user workloads, we will need the following resources at the same level as production.
    - App Service
- Identify the load characteristics and scenario
  - Our current average daily load is 1000 users/hour.  We will mimic the average daily load, hitting all endpoints. Because there is a consistent daily load, we will mimic this with gradual load increasing linearly.
- Identify the test failure criteria
  - Our test will fail if we are unable to support 1000 users, as we already know this is the current demand.
  - We will also consider this a failed test if we find performance below expected thresholds, as this may impact customer satisfaction.
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
- The following instructions cover steps 3-7. (TODO: confirm that these are the correct steps!)
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
    - Under the "Parameters" tab, enter "webapp" for the name of the environment variable, followed by the URL for your sample app in the "Value" section.
    - Under the "Monitoring" tab, select "Add/Modify" and select the resources from the sample app followed by "Apply" so they are monitored.
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
  - There is a sample action which will run a load test.  The default action will either create or run the existing load test.  However, the "TestName" parameter inside the config.yaml file will be automatically lower cased by the GitHub Action.  Since the UI allows upper or lowercase letters, this can cause a duplicate test to be created.
  - Learners can use the baseline from their first run in challenge 3 to determine what their pass/fail criteria is.

## Task 3 - Identifying bottlenecks, stress tests, and resilience testing

- The following high-level notes cover step 1.
  - For a primer on identifying bottlenecks using Azure Load Testing, [this article](https://docs.microsoft.com/en-us/azure/load-testing/tutorial-identify-bottlenecks-azure-portal) covers the process.
  - Because there is no database associated with this application, identifying areas of slowdown will require code analysis.
  - Learners should notice that there are two operations which consistently lag behind everything else in terms of performance: initial loading of messages and posting a new message. In both cases, the root cause is a thread sleep loop. After removing these "speed loops," learners should notice a drastic performance improvement when performing those operations.
- The following high-level notes cover steps 2-4. There is also [a sample JMeter script](./Solution/Exercise-04/Task-3/WTH_Load_Test-ALD.jmx) in the solution directory.
  - TODO: notes!
- The following high-level notes cover steps 5-8.
  - Chaos Studio does not offer a GitHub Action at this time, so learners will not be able to integrate this into their CI/CD pipelines.
  - Azure Chaos studio only supports a limited number of services, such as VMs, AKS, App Services, Cosmos DB, and networking.  Because our sample app only contains App Services, we will use App Service faults.
  - TODO: update the following notes!
  - First you will need to enable geo-redundancy in your Cosmos DB.  Go to your Cosmos DB instance and select "Enable Geo Redundancy" on the top bar.  Keep note of the paired region your Cosmos DB is paired with.
  - Create Azure Chaos Studio/Experiment
    - Register Chaos Studio Provider
      - Go to your subscription
      - On the left-hand side, select "Resource provider"
      - In the list of providers, search for "Microsoft.Chaos"
      - Click on the provider and select "Register"
    - Go to Azure Chaos Studio
    - Under "Targets" select "Enable Targets" then "Enable service-direct targets"
    - Select the Cosmos DB service for your load test.
    - On the left-hand side, select "Experiments"
    - Select "Add an experiment"
    - Fill in your subscription, resource group, location, and name for this experiment.  Keep track of the experiment name as a managed user will be created for you.
    - Go to the experiment designer on the next tab.  Change the name of the step or branch if you wish.
    - Select "Add Action" follows by "Add Fault"
    - Select "Cosmos DB Failover" as the Fault.  You can choose the duration but 2-3 mins should be enough.  Finally, enter the region that your instance is paired with in the "readRegion" section.
    - Save your experiment.
  - Update Cosmos DB Permissions
    - In Cosmos DB select "Access Control"
    - Select "Add" followed by "Add Role Assignment"
    - Select the CosmosDBOperator role, then click "Select members".
    - Search the name of the experiment from the earlier step and then click "Select".
    - Review and assign the permissions which will grant the role to the experiment.
  - Run load test + experiment
    - Run the load test first, then while the load test is running kick off the chaos experiment.  You should notice that the application does not fail, but there will be a large spike in response time when you kick off the chaos experiment.  You should only see one spike and the app should return to normal behavior afterwards.
