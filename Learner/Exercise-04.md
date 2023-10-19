# Exercise 04 - Implement load testing and secure practices

## Task 1 - Implement load testing (15 minutes)

### Introduction

When some people think of load testing, the first thought that comes to mind is pointing a tool at your site, cranking the load to the max, and seeing what happens.  While that might be exciting at the moment, it’s critical to take a step back and develop a load testing strategy that is tailored to the application. This means breaking down the architecture, internal and external dependencies, high availability design, scaling, and the data tier.  Having a plan not only helps you prepare while you are testing the application but it also provides context as to why and how you are testing the application for anyone in the future.  

### Description

The development team at Munson's Pickles and Preserves would like to understand how to perform proper load testing on a website, using the Team Messaging System as an example.

In this task, you will create a load testing strategy to describe your plan and goals. Below are some examples of topics, but you should also think about whether there are other questions or criteria to consider.

- Define the scope of your testing, including which services you will test.
- Identify the load characteristics for this scenario.
- Identify the test failure criteria.
- Identify how you will monitor your application while performing load testing.
- Identify potential bottlenecks or limitations in advance.
- Identify any assumptions or risks in your plan.

Although the Team Messaging System itself does not experience heavy utilization throughout the day, the development team has asked you to consider a similar application with the following usage profile:

- The majority of utilization happens during business hours, which are 8 AM until 5 PM Central time in the United States.
- During normal business hours, the average daily load is 1000 users per hour and users interact with all system endpoints, not just a subset. For a load test, they would like to ensure that 30 concurrent users are able to access the site and ensure that the overall workload should take no more than 600ms per call on average. Also, no more than 10% of calls should fail under this workload.
- There are no external APIs or automated systems which add an appreciable amount of workload to the application.

1. Create a load testing strategy based on the Team Messaging System application.

### Success Criteria

- Present your comprehensive load testing plan, paying special attention to how the load test will interact with any applications or services.
- Explain what potential bottlenecks you might encounter during the test, as well as your expected mitigation strategy for each bottleneck.
- Explain how a service failure or degradation might impact the performance of the application or the load test.

### Learning Resources

- [How to write a test plan for load testing](https://www.flood.io/blog/how-to-write-a-test-plan-for-load-testing)

## Task 2 - Run a load test from GitHub Actions (75 minutes)

### Introduction

In this task, you will create a load testing script in Apache JMeter which should hit each application endpoint. This script will implement the load testing strategy you created in the previous task. Your scripts should help you get a baseline for how the application can handle typical user loads.

Azure Load Testing is based on Apache JMeter - a popular open source load testing tool. This means you can reuse existing JMeter scripts or create new scripts by using the JMeter GUI.

To demonstrate this, you will then create a Load Testing service in Azure and use the load testing scripts you created to run baseline tests.

Baselines help to determine the current efficiency state of your application and its supporting infrastructure. They can provide good insights for improvements and determine if the application is meeting business goals. Baselines can exist for an application in any state of maturity, so no matter when you establish the baseline, you can measure performance against that baseline during continued development. When code or infrastructure changes, you can actively measure the performance impact of that change.

After creating a baseline, you will integrate Azure Load Testing into your CI/CD pipeline as part of a GitHub Action workflow.

### Description

Now that you have demonstrated your load testing plan for the Team Messaging System to the development team at Munson's Pickles and Preserves, it is time to put it into action.

1. Using the JMeter GUI, create a load testing script that targets each of the application endpoints.
2. Execute the load test using the JMeter GUI against your endpoints and use the Azure portal to confirm the App Service is getting the traffic.
3. Create a Load Testing service in Azure.
4. Create a load test in the Load Testing service that accepts the website URL as an environment variable and passes that value to your load testing script.
5. Run your load test multiple times using a typical user load and establish a performance baseline.
6. Review the test results, identify the bottlenecks (CPU, memory, resource limits), and compile a list of insights.
7. Discuss where you may be able to leverage the baseline data you have gathered.
8. Use the Azure Load Testing GitHub Action to execute your load test with a manual trigger.
9. Intentionally set the pass/fail rules in your test configuration to a value that would cause the test to fail during the workflow.
10. On load test failure, automatically create a GitHub Issue.

### Success Criteria

- Verify that you have a load testing script in .jmx format that targets each application endpoint.
- Verify that your load test works in JMeter and you are able to see the load in your sample application.
- Show your load tests running in Azure Load Testing service, as well as your performance baseline.
- Verify that you can run the same load test and change parameters to hit different environments.
- Demonstrate that you can execute your load test on a manual GitHub Actions workflow trigger.
- Demonstrate that, when the load test fails due to it not meeting a performance target, your workflow automatically creates a GitHub Issue.

## Learning Resources

- [Apache JMeter Getting Started](https://jmeter.apache.org/usermanual/get-started.html) - This requires Java 8+ to be installed
- [Apache JMeter Docs](https://jmeter.apache.org/index.html)
- [Create Test Plan from Template](https://jmeter.apache.org/usermanual/get-started.html#template)
- [JMeter best practices](https://jmeter.apache.org/usermanual/best-practices.html)
- [Custom Plugins for Apache JMeter](https://jmeter-plugins.org/)
- [Establish Baselines](https://docs.microsoft.com/en-us/azure/architecture/framework/scalability/performance-test#establish-baselines)
- [Create configurable load tests with secrets and environment variables](https://docs.microsoft.com/en-us/azure/load-testing/how-to-parameterize-load-tests)
- [Load Testing documentation](https://docs.microsoft.com/en-us/azure/load-testing/)
- [Video: JMeter Test Script Recorder](https://www.youtube.com/watch?v=voYj16cETAM)
- [JMeter + POST + anti-forgery token](https://stackoverflow.com/questions/53034969/jmeter-post-anti-forgery-token)

## Task 3 - Identifying bottlenecks, stress tests, and resilience testing (30 minutes)

### Introduction

At this point, you should have load tests running against your environment. Although our load tests are automated now, we need to make sure we review the tests to see if we can identify any bottlenecks or changes in behavior.  From there, we can share our findings and work towards remediating the issues.  If your application is hosted in Azure, you can leverage the synergy Azure Load Testing has with the other Azure services.

In addition to load testing, we may also wish to perform stress testing. Stress testing focuses on overloading the system until it breaks. A stress test determines how stable a system is and its ability to withstand extreme increases in load. It does this by testing the maximum number of requests that a system can handle at a given time before performance is compromised and fails. Find this maximum to understand what kind of load the current environment can adequately support.

We will also want to perform a third type of testing, called resilience testing. The resiliency of an application isn’t solely decided by how quickly it can scale up or out, but also how it handles failures. This means planning for a failure of every application component: a single container, cluster, VM, database, or region.

### Description

The team at Munson's Pickles and Preserves is excited about the possibilities of automated load testing of their applications and they are interested in learning more about the types of testing we can perform in Azure to gauge application quality and resiliency.

1. Review your load test results to see where there are potential bottlenecks in your application.  If needed, perform additional tests with different settings to help identity any bottlenecks.  Once you have identified one or more bottlenecks, see how you can resolve the bottleneck and show the new performance profile after your fixes are in place.
2. Create a new set of test scripts or parameters and run a stress test.
3. Identify the bottlenecks and breaking points of the application under stress.
4. Decide whether you should add stress test remediation steps as GitHub Issues.
5. Use Azure Chaos Studio to design a Chaos experiment. Note that you will need to register the `Microsoft.Chaos` provider for your subscription if you have not already. You can register this in the **Resource providers** menu item under the **Settings** menu for the subscription.
6. After ensuring that you have a recent baseline test, run a load test.
7. During the load test, start the Chaos experiment and note the failures. Note that there is, at present, no GitHub Action for Chaos experiments, so you will need to run this manually, outside of a GitHub Actions workflow.
8. Prioritize the failure points arising from the Chaos experiment and build an action plan to remediate.

### Success Criteria

- Demonstrate how you found any bottlenecks in the Team Messaging System application.
- Verify that your bottleneck has been resolved.
- Show a side-by-side comparison of the load tests results and the stress test results.
- Show, based on these tests, where the bottlenecks and breakpoints are in the application.
- Show the failure points that occurred during your resilience testing, as well as your remediation plan.

### Learning Resources

[Adding Monitoring to Load Testing](https://docs.microsoft.com/en-us/azure/load-testing/how-to-appservice-insights)
[Learn more about stress testing](https://docs.microsoft.com/en-us/azure/architecture/framework/scalability/performance-test#stress-testing)
[What is Azure Chaos Studio](https://docs.microsoft.com/en-us/azure/chaos-studio/chaos-studio-overview)
[Create and run a chaos experiment using Azure Chaos Studio](https://learn.microsoft.com/en-us/azure/chaos-studio/chaos-studio-quickstart-azure-portal)