## Day 7 - Pulumi Component Resources.

# Day 7 - Pulumi Component Resources

## What are component resources?

When we group our resources, this is what we can call a `Component Resources`. 
Components also instantiate a set of related resources in their constructor, aggregate them as children, and create a ledger.

Some examples of `Component Resources` are: 
- Azure `KeyVault` with all the required settings. 
- Aws `VPN` would have all the best practices.
- A `KubernetesCluster` that can create EKS, AKS, and GKE clusters, depending on the target.


## How can we use component resources?

To create a new component, either in a program or for a reusable library, create a subclass of `ComponentResource`. Inside of its constructor, chain to the base constructor, passing its type string, name, arguments, and options. Also inside of its constructor, allocate any child resources, passing the [`parent`](https://www.pulumi.com/docs/intro/concepts/resources/options/parent/) option as appropriate to ensure component resource children are parented correctly.

Component Example:

```csharp
class ComponentXYZ : Pulumi.ComponentResource
{
    public MyComponent(string name, ComponentResourceOptions opts)
        : base("pkg:index:MyComponent", name, opts)
    {
        // initialization logic.

        // Signal to the UI that this resource has completed construction.
        this.RegisterOutputs();
    }
}
```

When we initiate a new instance of `ComponentXYZ`; the call to the base constructor (using `super/base`) registers the component resource instance with the Pulumi engine. 

This records the resource’s state and tracks it with your deployments so that you see diffs during updates. Component must also have a unique `name` this is a part of the state file that we touch on [Day-4-stacks](https://blog.justjordant.com/day-4-pulumi-stacks) and [Day-5-state-and-backends](https://blog.justjordant.com/day-5-pulumi-state-and-backends) this name needs to be unique across your pulumi project; best practice is to name these as followed, `pkg:index:MyComponent`. I.e `azure:appService:MyApp`.

### Creating Child Resources

Component resources are comprised of child resources, so we have all of our settings in one given module. When building a resource, children must be registered as such; To do this, pass the Customer resource as the `parent` option.

```csharp
var bucket = new Aws.S3.Bucket($"{stackName}-bucket",new Aws.S3.BucketArgs
{
//ARGS
}, 
new CustomResourceOptions
{ 
	Parent = this 
});
```

### Component Outputs

We are also able to set our `Outputs`  on a component level as well. This can enable us to pass certain information our of our `Component Resources`, this can be very useful when passing around information, passing a resource id from one component to another.

C#
```csharp
[Output("keyVaultId")] 
public InputList<string> KevVaultId { get; private set; }
```
Python
```python
self.register_outputs({
    "bucketDnsName": bucket.bucketDomainName
})
```


## Why should we be using component resources?
`Component resource` can help in managing our resources by pairing them together in a manageable manner; just think of this from a container aspect, we are packaging all of our resources into one given instance to be called, later on, this way it has all the need configuration. At a high level, we are able to make a clone of our `Component Resources`, and give it a name; but at the end of the day, it's all the same under the hood.

### Resources
[Components | Pulumi](https://www.pulumi.com/docs/intro/concepts/resources/components/)

[Resources | Pulumi](https://www.pulumi.com/docs/intro/concepts/resources/)

[Getter Functions | Pulumi](https://www.pulumi.com/docs/intro/concepts/resources/get/)

[Resource Options | Pulumi](https://www.pulumi.com/docs/intro/concepts/resources/options/)

[Resource Names | Pulumi](https://www.pulumi.com/docs/intro/concepts/resources/names/)


As always happy coding my friends!