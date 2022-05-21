## Day 8 - Pulumi Logging and Troubleshooting

# Day 8 - Pulumi Logging and troubleshooting

[GitHub Repo for this blog.](https://github.com/JustJordanT/pulumi-docker-example)

## What is Pulumi logging and troubleshooting?
Pulumi has a collection of functions available to us such as. 

- Info
- Debug
- Warning
- Error

These are displayed, with all other Pulumi output, in the Pulumi CLI. Events are logged and kept for historical purposes in case you want to audit or diagnose a past event.

We can also use certain arguments when running `pulumi up`, The `--logtostderr` flag can be added to send this verbose logging directly to `stderr` for easier access. Pulumi also has verbosity levels that we can utilize as well; these levels `1-9`. 

## How can we use logging and troubleshooting?

### Loggings 
We are able to set our logging from within our code to be able to have more control over how our infrastructure is being run. Picking up from [Day-7] we went over `Component Resources`, let's make one!

```csharp
using Pulumi;  
using Pulumi.Docker.Inputs;  
using Docker = Pulumi.Docker;  
  
namespace docker_example  
{  
    public class OurDocker : ComponentResource  
    {  
        public OurDocker(string name, OurDockerArgs args,   
ComponentResourceOptions? options = null, bool remote = false) :  
            base("dockerexample:demo:container", name, args, options, remote)  
        {            // Find the latest Ubuntu precise image.  
            var remoteImage = new Docker.RemoteImage(name, new Docker.RemoteImageArgs  
            {  
                Name = args.ImageName,  
            });  
            // Start a container  
            var ubuntuContainer = new Docker.Container("ubuntuContainer", new Docker.ContainerArgs  
            {  
                Name = args.ContainerName,  
                Image = remoteImage.Latest,  
                Ports = new ContainerPortArgs  
                {  
                    External = args.ExternalPort,  
                    Internal = args.InternalPort,  
                }  
            });  
        }  
        public sealed class OurDockerArgs : ResourceArgs  
        {  
            [Input("containerName")]   
			public Input<string> ContainerName { get; set; } = null!;  
            
            [Input("imageName")]   
			public Input<string> ImageName { get; set; } = "nginx:1.21.3-alpine";  
            
            [Input("externalPort")]   
			public Input<int> ExternalPort { get; set; } = null!;  
            
            [Input("internalPort")]   
			public Input<int> InternalPort { get; set; } = null!;  
        }  
    }}
```
 
Above is our `Component Resource` that we have made; looking through this we are creating a Docker image and using that image for our container, we're also using `Inputs` for the container name, image name ("with a default being the Nginx"), and the ports for the container. 

Now looking at our `MyStack.cs` file we view the logging that we have set up. 

```csharp
using docker_example;  
using Pulumi;  
  
class MyStack : Stack  
{  
    public MyStack()  
    {        
    
    var stackName = Deployment.Instance.StackName;  
  
        if (stackName == "prod")  
        {            new OurDocker(stackName, new OurDocker.OurDockerArgs  
            {  
                ExternalPort = 8080,  
                InternalPort = 80  
            });  
        }  
        else  
        {  
            Pulumi.Log.Error("Deploying to the wrong stack; check stack configuration.");
        }  
            }  
}
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653148429440/PYsi9K1Sc.png align="left")


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653148439509/G0dM8Bs3w.png align="left")

Here we see that we have our logging set up so that if the environment is not `prod`
then we want to throw an error for this case; the sky is the limit as to how we can use logging. Using the `pulumi cli` we can use the following command to output our logging to a file as followed. 

```sh
pulumi up --logtostderr -v=9 2> log_out.txt
```

### Tracing

Pulumi also allows us to use tracing as a part of our deployments. Let's spin up our tracing server. 

```sh
docker run -d --name jaeger \
  -e COLLECTOR_ZIPKIN_HOST_PORT=:9411 \
  -p 16686:16686 \
  -p 9411:9411 \
  jaegertracing/all-in-one:1.22
```

Now we can view it on the tracing server at the following address [Jeager Tracer](http://localhost:16686/search).
Our server is up and running now if we run our pulumi stack we can view our traces here. 

```sh
pulumi up --tracing http://localhost:9411/api/v1/spans
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1653148499857/TgNjfv6O1.png align="left")

From our server, we view our traces for all of our pulumi operations. 

## Why can logging and troubleshooting be beneficial?
Logging can be a big part of our troubleshooting steps when finding root causes, this is why we can focus our attention on the real issues behind certain failures in our environments and stacks. 

If you are seeing the unexpectedly slow performance, you can gather a trace to understand what operations are being performed throughout the deployment and what the long poles are for your deployment.

### Resources

Be sure to take a look at the following resources for additional information.

[Logging | Pulumi](https://www.pulumi.com/docs/intro/concepts/logging/)

[Troubleshooting | Pulumi](https://www.pulumi.com/docs/support/troubleshooting/)


Happy coding, my friends! 🤗