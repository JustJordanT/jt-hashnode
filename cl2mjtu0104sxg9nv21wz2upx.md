## Day 4 - Pulumi Stacks


# Pulumi stacks

## Why do we want to manage a Stack?

A new project can have as many stacks as you need. By default, Pulumi creates a stack for you when you start a new project using the `pulumi new` command.

Pulumi program is deployed to a _stack_. A stack is an isolated, independently [configurable](https://www.pulumi.com/docs/intro/concepts/config/) instance of a Pulumi program. Stacks are commonly used for different phases of development (such as `development`, `staging`, and `production`) or feature branches (such as `feature-x-dev`); we could even use stacks from a different region standpoint (as in `centralus`, `easteu`. 

Managing stacks can help us manage different environments within our cloud infrastructures, and easier deployments since everything for a given stack (environments) has been configured via `stack files`.  ie `Pulumi.dev.yaml`; these are also containers for the state of our infrastructure.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651364388972/7qhgP4m0M.png align="left")

## How can we manage a Stack?

##### Create a new stack.

We can create a new stack in pulumi with the following. 

`pulumi stack init staging.`

This will create the stack with the name `staging`, and set this to the active stack. Based on the pulumi documentation the project that the stack is associated with is determined by finding the nearest `Pulumi.yaml` file.

> **Side Note:** The `pulumi stack init stackName` CLI command will not create the stack configuration file, you will need to configure this manually by creating the file and adding the need `yaml` required configuration for your stack, or you can us the pulumi CLI to make this for you by using the `config set` command. 
> 
> `pulumi config set azure-native:location CentralUS` 
> 
>We can think of these as Key:Values pairs `CentralUS` is just a value to `location` we can use this to add any type of variables that we will need in our stacks.

#### Listing stacks in a project.

We can list the current stacks that we have in our project by using the `pulumi stack ls`.

```bash
pulumi stack ls
NAME      LAST UPDATE  RESOURCE COUNT  URL
dev       n/a          n/a             https://app.pulumi.com/JustJordanT/azure-csharp/dev
staging*  n/a          n/a             https://app.pulumi.com/JustJordanT/azure-csharp/staging
```

Viewing the output we see that we have two stacks available to us; The `*`  shows the current that is selected.

- The `NAME` shows the name of the stacks.
- `LAST UPDATE` shows the last time the stack was updated.
- `RESOURCE COUNT` show how many resources we currently have in each stack.


#### Selecting a stack.

Using `pulumi stack select` we can change the active stack.

```bash
pulumi stack select
 
Please choose a stack, or create a new one:  [Use arrows to move, enter to select, type to filter]
> dev
  staging
  <create a new stack>
```

> **Side Note:** The pulumi operations `config`, `preview`, `update` and `destroy` operate on the active stack.

#### Generate an update plan.
We can use the `pulumi preview --save-plan=planName.json` to save the output to a file; this will use the current `Configuration values` that we have in place for this stack.

![[Pasted image 20220424211507.png]]

#### Update a stack.
Update our selected stack, by running `pulumi up`. If you saved a plan from a preview you can pass that in to constrain the update to only doing what was planned with `pulumi up --plan=plan.json`. The operation uses the latest [configuration values](https://www.pulumi.com/docs/intro/concepts/config/#configuration-keys)) for the active stack.

#### Viewing Stack Resources.
Running the `pulumi stack`, will show us the current, stack resources and stats. a good way to see the correct resources, and outputs.

```bash
pulumi stack
Current stack is dev:
    Last updated 1 week ago (2022-03-02 06:26:09 -0800 CST)
    Pulumi version v3.0.0

Current stack resources (3):
    TYPE                                             NAME
    pulumi:pulumi:Stack                              webserver-dev
    aws:ec2/securityGroup:SecurityGroup              web-secgrp
    aws:ec2/instance:Instance                        web-server-www

Current stack outputs (2):
    OUTPUT                                           VALUE
    publicDns                                        ec2-18-218-85-197.us-east.com
    publicIp                                         20.215.88.199
```

### Stack tags.
We can use tags just like we do on our cloud providers, we are able to separate environments based on the tags from `prod`, `dev`, `staging`, and so on. 

> **Side Note:** We are only able to use `tags` if we are managing our state with the pulumi backend. i.e `pulumi cloud`

```bash
 pulumi stack tag ls
NAME                VALUE
gitHub:owner        60DaysOf
gitHub:repo         60-days-of-pulumi
pulumi:description  A minimal Azure Native C# Pulumi program
pulumi:project      azure-csharp
pulumi:runtime      dotnet
vcs:kind            github.com
vcs:owner           60DaysOf
vcs:repo            60-days-of-pulumi
```

### Stack outputs.
Outputs can be used by calling these out in our code; here is an example of this below.

```typescript
import * as pulumi from "@pulumi/pulumi";
import * as docker from "@pulumi/docker";

const stackName = pulumi.getStack()

// Find the latest Ubuntu precise image.
const ubuntuRemoteImage = new docker.RemoteImage("ubuntuRemoteImage", {name: "ubuntu:precise"});

// Create a network

const network = new docker.Network("network", {name: `network-${stackName}`})

// Outputs

export let imageName = ubuntuRemoteImage.name
export let imageId = ubuntuRemoteImage.id
export let networkName = network.name
```

We can see here at the end of the typescript file we can some outputs that we have created. 

Using the following `pulumi cli` commands, we can pull these `output` values.

```bash
pulumi stack output imageName
ubuntu:precise
```

### Stack References.

With `stack references`, we can access `outputs` from one stack to another. This can allow us to `output` certain information that will need in another stack, or even a different pulumi project together.

To reference values from a stack, we create a stack reference from within our source code so that we can call this by name.

```csharp
var dockerCsharp = new StackReference("JustJordant/docker-csharp/dev");
var csharpContainerImageName = dockerCsharp.GetOutput("imageName");
```

From here, we would be able to pass this `output` throughout our project and use it as needed.

### Importing and exporting stacks
A stack can be Imported and Exported; when we do this, we are handling the state of our stacks. We can export our stack before changing our state to have an excellent baseline to return to; then, we can import our stack back to a good state.

Example:

```bash 
pulumi stack export --file stack-bak.json

pulumi stack import --file stack-bak.json
```

### Destroy and Deleting Stacks.
 Before we delete a `stack`, we need to make sure that we have no resources in it. Then, we can delete a `stack`. This is best practice!

`pulumi destroy`

`pulumi stack`

If we want to force the deletion, we can, but this will leave those non-deleted resources in an unmanaged state by pulumi.

`pulumi stack rm --force.`

## What does managing a Stack do for us?
Managing `Stacks` enables us to have our different environments and via stacks `prod`, `dev`, `staging`; this way, we can architect this is in various ways to build our stacks/environment that will be more manageable from the infrastructure side of the house.

### Resources.

- Be sure to look at the [Stacks | Pulumi](https://www.pulumi.com/docs/intro/concepts/stack/) documentation to find even more details about this feature of pulumi. 
- [Pulumi Configuration](https://www.pulumi.com/docs/intro/concepts/config/)

Happy coding, My Friends!