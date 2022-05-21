## Day 3 - Pulumi Configuration.

# Day 3 Working With Configuration Inside of Pulumi


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651250771053/TRk9xEL6N.png align="left")

## Why configuration?
Different stacks for a project can need differing values. For example, you may want to use a different size for your instances on your cloud infrastructure or a different number of servers for your Kubernetes cluster between your staging to production stacks.

Pulumi enabled us to use a configuration system for managing this in a meaningful way. Instead of hard-coding the differences, you can store and retrieve configuration values using a combination of the [CLI](https://www.pulumi.com/docs/reference/cli/) and the [programmatically](https://www.pulumi.com/docs/intro/concepts/config/#code).

The key-value pairs for any given stack are stored in [your project’s stack settings file](https://www.pulumi.com/docs/intro/concepts/project/#stack-settings-file), which is named as follows `Pulumi.stackName.yaml`. You may want to check it in and version it with your project source code i.e. `GitHub, GitLab.`

## How to use configurations?
We can use configurations via `Stack files`, we can use the following commands when manipulating our configs.

```bash
Available Commands:
  cp          Copy config to another stack
  get         Get a single configuration value
  refresh     Update the local configuration based on the most recent deployment of the stack
  rm          Remove configuration value
  rm-all      Remove multiple configuration values
  set         Set configuration value
  set-all     Set multiple configuration values
```

#### Setting config values

Using the Pulumi config command, we are able to set Key: Value pairs, that we can pull into our code for our infrastructure.

```bash
pulumi config set -h
---------------------
Configuration values can be accessed when a stack is being deployed and used to configure the behavior.
If a value is not present on the command line, pulumi will prompt for the value. Multi-line values
may be set by piping a file to standard in.

The `--path` flag can be used to set a value inside a map or list:

  - `pulumi config set --path names[0] a` will set the value to a list with the first item `a`.
  - `pulumi config set --path parent.nested value` will set the value of `parent` to a map `nested: value`.
  - `pulumi config set --path '["parent.name"].["nested.name"]' value` will set the value of
    `parent.name` to a map `nested.name: value`.

Usage:
  pulumi config set <key> [value] [flags]
```

Example:
```
pulumi config set Azure_app_service_plan_sku P1v2
```

#### Getting Configuration Values
Using the `Pulumi Cli` can get the value of a key by using the `get` flag for the `pulumi config` command.

Example:
```
pulumi config get Azure_app_service_plan_sku
```

#### Getting config values from code
In our Pulumi Projects, we are able to pull in the config to our source code and call out specific `keys`, so that we can pull values into it.

Example:
```csharp
var config = new Pulumi.Config();
var name = config.Require("name");
var location = config.Require("location");
var stackName = Deployment.Instance.StackName;

var resourceGroup = new AzureNative.Resources.ResourceGroup("resourceGroup",
		new AzureNative.Resources.ResourceGroupArgs
        {
            Location = location,
            ResourceGroupName = "${name}-{stackName}-rg"
```

Looking at this, we can start to see how we would use our configuration values in our source code to set variables from our side for our code base that can also be pulled into other stacks. 

## What does configuration do for us?
Pulumi configuration enables us to have `variables` that are set outside our configurations themself. This way, we can use them in different ways, through scripts and source code. 

Happy Coding, My Friends! 