## Day 10 - Pulumi Putting it all together.

# Day 10 - Pulumi Putting it all together.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656019397437/6H6ek5uJX.png align="left")

On our last day of 10DaysOfPulumi, let’s just review what we have gone over thus far. 

## Pulumi Projects
We can think of pulumi Projects as a kind of repository for our pulumi source code, at a high level; we can see how this encapsulates all other things in the pulumi-projects In our YAML file we can specify a binary to be executed from here.

We can also call the runtime from here. dotnet, go, nodejs; we can add settings in our stack files needed for our stacks. from `dev`, `prod`, `stagging`  ie `Pulumi.dev.yml` , `Pulumi.prod.yml`

## Pulumi Configuration
Different stacks for a project can need differing values. For example, you may want to use a different size for your instances on your cloud infrastructure or a different number of servers for your Kubernetes cluster between your staging to production stacks.
Pulumi enabled us to use a configuration system for managing this in a meaningful way. Instead of hard-coding the differences, you can store and retrieve configuration values using a combination of the CLI and the programmatically.
The key-value pairs for any given stack are stored in your project’s stack settings file, which is named as follows `Pulumi.stackName.yaml`. You may want to check it in and version it with your project source code i.e. GitHub, GitLab.

## Pulumi Stacks
A new project can have as many stacks as you need. By default, Pulumi creates a stack for you when you start a new project using the pulumi new command.

Pulumi program is deployed to a stack. A stack is an isolated, independently configurable instance of a Pulumi program. Stacks are commonly used for different phases of development (such as development, staging, and production) or feature branches (such as feature-x-dev); we could even use stacks from a different region standpoint as in `centralus`, `easteu`.

Managing stacks can help us manage different environments within our cloud infrastructures, and easier deployments since everything for a given stack (environments) has been configured via stack files. ie `Pulumi.dev.yaml`; these are also containers for the state of our infrastructure.

## Pulumi State
We can think of the state as where all of our configurations get stored. Each one of our stacks has its state. Pulumi can store this state in a backend that we specify. 

Pulumi state doesn't add your cloud credentials; these are only kept locally on your system. Pulumi supports two types of backends for storing your state.

Service: a managed cloud portal that is online or a self-hosted one.
Self Managed: a managed object store; this could be AWS, Azure, or GCP.

## Pulumi Inputs and Outputs
All resources args in Pulumi can accept Inputs; Inputs are values of type Input<T>, a kind that permits either a raw value of a given type like string, integer, boolean, list, map, and so on.

All resource properties on the instance object itself are outputs. Outputs are values of `type Output<T>.` 
If we can look at the Pulumi documentation for AWS S3) buckets; we can see that we have the following list of Outputs.

we can see that we are setting Outputs to both Id and Name; When creating an instance of this class we will now have access to Id and Name that we could in turn now pass into another class that we may need to storage account Id for.

## Pulumi Component Recourses
When we group our resources, this is what we can call a Component Resource. Components also instantiate a set of related resources in their constructor, aggregate them as children, and create a ledger.

Some examples of Component Resources are: 

Azure KeyVault with all the required settings. 
Aws VPN would have all the best practices.
A Kubernetes cluster that can create `EKS`, `AKS`, and `GKE` clusters, depending on the target.

## Pulumi Logging
Pulumi has a collection of functions available to us such as. 

Info
Debug
Warning
Error

These are displayed, with all other Pulumi output, in the Pulumi CLI. Events are logged and kept for historical purposes in case you want to audit or diagnose a past event.

We can also use certain arguments when running pulumi up, The --logtostderrflag can be added to send this verbose logging directly to stderr for easier access. Pulumi also has verbosity levels that we can utilize as well; these are levels 1-9.

## Pulumi Testing
Since we are using general-purpose programming languages, we can use native testing frameworks in our language of choice, i.e., NUnit for .NET or Jest for the JS ecosystem.
We are also able to take advantage of multiple styles of automated testing.
Unit tests: testing of individual parts of our code.
Property tests: run resource-level assertions while infrastructure is being deployed.
Integration Tests: deploy ephemeral infrastructure and run external tests against it.
Resources
Pulumi
Pulumi Docs
Pulumi Registry
Pulumi Slack
Learn Pulumi

## As always happy coding my friends 😊
