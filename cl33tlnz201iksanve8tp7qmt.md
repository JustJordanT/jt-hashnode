## Day 5 - Pulumi State and Backends.

# States and backends. 

### What are states and backends?
We can think of the state as where all of our configurations get stored. Each one of our stacks has its state. Pulumi can store this state in a backend that we specify. 

Pulumi state doesn't add your cloud credentials; these are only kept locally on your system.

Pulumi supports two types of `backends` for storing your state.
1. Service: a managed cloud portal that is online or a self-hosted one.
2. Self Managed: a managed object store; this could be AWS, Azure, or GCP.

### How can we manage state and backends?
We can select a custom backend that we would like to store our state using the `pulumi login` command. When we log in, we are using the default Pulumi cloud.

Using different backends is pretty simple; we can add the argument specifying where we would like our backend. 

To use the filesystem backend to store your checkpoint files locally on your machine, pass the `--local` flag when logging in:

```sh
pulumi login --local
```

To use the [AWS S3](https://aws.amazon.com/s3/) backend, we give it the `s3://<bucket-name>` as your `<backend-url>`:

```sh
pulumi login s3://my_bucket
```

We are also able to use different file store as well, including [Minio](https://www.minio.io/), [Ceph](https://ceph.io/), or [SeaweedFS](https://github.com/chrislusf/seaweedfs).

### Why would we want to manage state and backends?
Being able to manage state and backends gives us the flexibility to not rely on one shown provider; this way, we can utilize different vendors for our back-end/state needs.

If we have some compliance reason for not being able to use `pulumi cloud,` no worries, we can store our state in a local server on-premise; or use a vendor that we may already be using for our storage needs.

### Resources

[State and Backends | Pulumi](https://www.pulumi.com/docs/intro/concepts/state/)

[How Pulumi Works | Pulumi](https://www.pulumi.com/docs/intro/concepts/how-pulumi-works/)

[Secrets | Pulumi](https://www.pulumi.com/docs/intro/concepts/secrets/)

[Assets and Archives | Pulumi](https://www.pulumi.com/docs/intro/concepts/assets-archives/)

And always; happy coding my friends! 🤗