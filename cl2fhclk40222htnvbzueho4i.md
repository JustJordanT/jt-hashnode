## Day 2 - Pulumi Projects

# Day 2 Pulumi In Projects

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1650936291363/1o2jntgKk.png)

### Why we use projects?
We can think of pulumi Projects as a kind of repository for our pulumi source code, at a high level; we can see how this encapsulates all other things in the pulumi-projects In our YAML file we can specify a binary to be executed from here.

We can also call the runtime from here. `dotnet, go, nodejs`; we can add settings in our stack files needed for our stacks. from `dev, prod, stagging`  **ie** `Pulumi.dev.yml`

### What are Projects consist of?
The `Pulumi.yaml` project file specifies metadata about your projects, such as the project name, applicable runtime for your program, and other higher-level information.

### What are stack files?
We will touch on stack file a little here but we will be diving deeper in `day-3`; stacks that is created in a project will have a file named `Pulumi.<stackname>.yaml` that contains the configuration specific to this stack. This file typically resides in the root of the project directory.

For stacks that are actively developed by multiple members of a team aka (Friends), the recommended practice is to check them into source control as a means of collaboration and the source of truth for your stack. Since secret values can be encrypted, it is safe to check in these stack settings into a repo given that your have the `--secret` flag while using the `config set` command. 

```bash
pulumi config set aws:region us-west-2 --secret
```

### What can Projects and Stacks enable us to do?
Using Projects and Stacks can empower us to have a main repo for our pulumi code with (state), add having Stacks makes it possible to easily differ certain args from the environment ie `prod`, `dev` to `staging`. 


### Questions
- Why we use projects?
- How are Projects used?
- How are Stack files used?
- What can Projects and Stacks enable us to do?

### Lab 

1. Install Pulumi on your system. [Download and Install | Pulumi](https://www.pulumi.com/docs/get-started/install/)
2. Create a new directory, `myPulumiProject`
	1. on macOS or windows, Linux
3. Use the `Pulumi` command `Pulumi new`
	- Use one of the following examples.  `
		- `pulumi new aws-csharp`
		- `pulumi new aws-python`
		- `pulumi new aws-typescript`
		- `pulumi new azure-csharp`
		- `pulumi new azure-python`
		- `pulumi new azure-typescript`
4. Now that we have the project we can review all of this together with the code that we are seeing in our project.  


### Summary

- Walkthrough the branch for `day 2`  and see what you find and what kind of questions you can ask yourself to better understand [[pulumi-projects]] in the Pulumi environment. 
- Remember documentation is your friend, so don't be scared to get your hands dirty!
	- [Documentation | Pulumi](https://www.pulumi.com/docs/)
	- [Pulumi Registry | Pulumi](https://www.pulumi.com/registry/)
    - [Project File Reference | Pulumi](https://www.pulumi.com/docs/reference/pulumi-yaml/)

- The last one is a ☝️ good resource for the pulumi-projects good overview and resource to bookmark when it comes to learning about them.

We will dive into stacks on `day-3` and go into more detail regarding them. Happy coding my friends! 😊
