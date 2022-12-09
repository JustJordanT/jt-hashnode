# Scaling Web Apps on Azure.

Scaling web apps in the cloud can be a difficult situation dealing with so many layers of frontends and backends;  managing containers, databases, load balancers, and the list goes on. 

Load testing an application is a good idea because it helps to ensure that the application can handle the expected amount of traffic or usage without performance degradation or outages. 

This is important because if an application is not able to handle the expected load, it can lead to a poor user experience, which can result in lost customers or revenue. In addition, load testing can help identify potential bottlenecks or other issues that can be addressed before the application is deployed, which can help to prevent costly outages or other problems.

There are always so many nuances with different Cloud providers managing things from an Azure perspective; compared to AWS, it can be a little different dealing with the same systems theoretically but with a different interface and different means of management.

Moving through this document will take a look at the Azure-specific resources that we will need to account for regarding scaling our infrastructure; However, you can take this information and use it on a cloud provider if they allow you to edit the same parameters. I.e., the number of Instances/ DTUs, and so on.

# Scaling Web Applications.

When scaling a web app, the first thing you will want to take a look at is the application side itself; is the application consume high amounts of CPU, Memory? If so, this is where we will want to spend your time looking at how your application behaves from a metric standpoint. 

## Metrics

Looking at metrics, we can get a good baseline on how frequently a given alert is going off and how this will behave under development and production workloads. We can monitor metrics on the **Application Service Plan**, which a good starting point is: 

*   Memory Consumption
    
*   CPU Consumption
    

There are several different ways to scale an App Service on Azure, depending on the specific needs of the application. One approach is auto-scaling, which allows the user to adjust the number of instances and the size of the instances in the App Service based on the metrics; this can also be done on demand to respond to changes in workloads or performance requirements quickly.

Another approach is autoscaling, which allows the App Service to automatically adjust the number of instances and their size based on predefined rules or metrics. For example, the App Service can automatically increase or decrease the number of instances based on the average CPU usage or the number of incoming requests.

Scaling an App Service on Azure can help to improve the performance and availability of the application. By adjusting the number of instances and their size in response to changes in workloads and performance requirements, the App Service can ensure that the application has sufficient resources to handle incoming requests efficiently and without downtime.

# Load Testing

Another thing we can do to get a good baseline would have good load testing in place. This can show us bottlenecks that we have within our application flow from an end-to-end perspective, how load balancers, Application Services, and Databases are all handling requests. Using something like [k6.io](http://k6.io), we can test our limits.

Here is an example of a load test using k6, a popular open-source load testing tool; first, install k6 using the command 

`brew install k6`
 
Next, create a JavaScript file that defines the load test. For example, let's call the file loadtest.js:

  
```js

import http from "k6/http";

import { check, sleep } from "k6";

export let options = {

  vus: 10,

  duration: "30s"

};

export default function() {

  let res = http.get("[https://mywebsite.com/](https://mywebsite.com/)");

  check(res, {

    "status was 200": (r) =&gt; r.status == 200,

    "transaction time OK": (r) =&gt; r.timings.duration &lt; 200

  });

  sleep(1);

}  
```

In this example, the load test is configured to run with 10 virtual users (vus) for 30 seconds. The test sends a GET request to [https://mywebsite.com/](https://mywebsite.com/) and checks that the response has a status code of 200 and a transaction time of less than 200 milliseconds. The test then sleeps for 1 second before repeating.

To run the load test, use the command k6 run loadtest.js. This will execute the load test and print the results to the console. The results will include metrics such as the number of requests per second, the average response time, and the success rate of the requests.

This is just a simple example of a load test using k6. More complex and realistic load tests can be created by adding additional requests, checks, and scenarios.

There are several benefits to using k6 for load testing:

*   k6 is an open-source tool, which means that it is free to use and customize. This makes it an attractive option for teams with limited budgets or for those who want to have full control over their load-testing setup.
    
*   k6 is easy to use and has a simple, intuitive syntax. This makes it easy for developers to write load tests and integrate them into their continuous integration and deployment pipelines.  
    
*   k6 is highly customizable and can be extended with custom scripts and libraries. This allows teams to tailor their load tests to specific needs and requirements.
    
*   k6 is a modern tool built on the Go language and supports the latest web standards and protocols. This allows it to test various web, mobile, and API applications.
    
*   k6 has a rich set of built-in metrics and reporting features, which make it easy to analyze the results of load tests and identify performance bottlenecks or other issues.
    

Overall, using k6 for load testing can help teams to ensure the performance and reliability of their applications under high load and to make informed decisions about how to optimize and improve their applications.

# Scaling Load Balancers.

Load balancers are used in Azure to distribute incoming traffic across multiple compute resources, such as virtual machines, to improve the performance and availability of applications. This is done by dividing incoming requests among the available compute resources, allowing each resource to handle a portion of the workload.

Load balancers on Azure can be used in several different ways, depending on the specific needs of the application. For example, a load balancer can distribute traffic across a group of web servers, ensuring that each server can handle a portion of the incoming requests. This can help to improve the overall performance and scalability of the web application.

Load balancers can also be used to improve the availability of applications by automatically routing traffic away from computing resources that are unavailable or experiencing issues. This can help to prevent downtime and ensure that the application remains available to users even if one or more compute resources fail.

In Azure, an application gateway is a type of load balancer that is used to distribute incoming traffic among multiple web servers or back-end pools. Scaling an application gateway refers to adjusting the number of instances or the size of the instances in the gateway in response to changing traffic patterns or workloads.

There are several different ways to scale an application gateway on Azure. One approach is to manually adjust the number of instances or the size of the instances in the gateway using the Azure portal, Azure CLI, or Azure PowerShell. This can be done on-demand to respond to changes in traffic or workloads quickly.

Another approach to scaling an application gateway on Azure is to use autoscaling. Autoscaling allows the gateway to adjust the number of instances or the size of the instances in response to predefined rules or metrics. For example, the gateway can automatically add or remove instances based on the number of incoming requests or the average response time; in Azure, this is set by minimum and maximum count.

Scaling an application gateway on Azure can help to improve the performance and availability of the web applications it serves. By adjusting the number of instances and their size in response to changes in traffic or workloads, the gateway can ensure that the applications can handle incoming requests efficiently and without downtime.  

# Scaling Databases.

There are several different ways to scale a database on Azure, depending on the specific needs of the application. One approach is to use manual scaling, which allows the user to manually adjust the number of DTUs allocated to the database using the Azure portal, Azure CLI, or Azure PowerShell. We can also look at read replicas from a load perspective. Scaling Databases can be done on demand to respond to changes in performance or workload requirements quickly.

## Read Replicas

One way to handle the load on bata bases can be to utilize read replicas. A read replica is a type of database that is a duplicate of another database. The replica offload read operations from the main database, allowing the primary database to focus on handling write operations and improving overall performance.

Read replicas are often used when a high volume of read operations is causing performance issues for the primary database. By creating a replica of the database and routing read operations to the replica, the primary database can handle the write operations more efficiently. This can improve the overall performance of the database system and reduce the response time for read operations.

Read replicas can also be used for disaster recovery and data redundancy. If the primary database becomes unavailable for any reason, the read replica can be promoted to become the new primary database, ensuring that the data is still accessible. Additionally, having multiple replicas of the database can help to protect against data loss in the event of a failure or disaster.

## Database transaction units (DTUs)

DTUs (database transaction units) measure the performance and capacity of a SQL database. Scaling DTUs refers to the process of increasing or decreasing the number of DTUs allocated to a database to meet the changing performance and workload requirements of the application.

There are two main ways to scale DTUs in Azure SQL databases: manual scaling and autoscaling. Manual scaling allows users to manually adjust the number of DTUs allocated to a database using the Azure portal, Azure CLI, or Azure PowerShell. This can be done on demand to respond to changes in performance or workload requirements quickly.

Autoscaling, on the other hand, allows the DTUs allocated to a database to be automatically adjusted based on predefined rules or metrics. For example, the database can automatically increase or decrease the number of DTUs based on the average CPU usage or the number of incoming requests.

Scaling DTUs in an Azure SQL database can help to improve the performance and availability of the application. By adjusting the number of DTUs allocated to the database in response to changes in performance or workload requirements, the database can ensure sufficient capacity to handle incoming requests efficiently and without downtime.

Happy coding, my friends!  👨🏽‍🚒

## Resources:

https://permanentiteration.com/automatically-scale-up-an-azure-app-service-plan/
https://techcommunity.microsoft.com/t5/apps-on-azure-blog/azure-app-service-automatic-scaling/ba-p/2983300
https://aws.amazon.com/rds/features/read-replicas/
https://learn.microsoft.com/en-us/azure/mysql/single-server/concepts-read-replicas
https://learn.microsoft.com/en-us/azure/architecture/framework/scalability/design-scale
https://k6.io/docs/
[Replication to Azure SQL Database](https://learn.microsoft.com/en-us/azure/azure-sql/database/replication-to-sql-database?view=azuresql)