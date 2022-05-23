## Day - 9 Pulumi Testing.

# Day 9 - Pulumi Testing 

## What does testing in pulumi look like.
Since we are using general-purpose programming languages, we can use native testing frameworks in our language of choice, i.e., `NUnit` for .NET or `Jest` for the JS ecosystem.

We are also able to take advantage of multiple styles of automated testing.

- **Unit tests**: testing of individual parts of our code.
- **Property tests**: run resource-level assertions _while_ infrastructure is being deployed.
- **Integration Tests**: deploy ephemeral infrastructure and run external tests against it.

In this blog, we will mainly be looking at `Unit tests`  and `Integration tests`, `Property tests` are not supported in all languages.

## How can we use testing to our advantage?

### Unit Testing.

Using Unit testing, we can enforce specific resources to make sure that we have consistent  
tagging, exposed ports, and so on. 

#### Unit testing with Go.

```go
package main

import (
	"sync"
	"testing"

	"github.com/pulumi/pulumi-aws/sdk/go/aws/ec2"
	"github.com/pulumi/pulumi/sdk/go/common/resource"
	"github.com/pulumi/pulumi/sdk/go/pulumi"
	"github.com/stretchr/testify/assert"
)

type mocks int

// Create the mock.
func (mocks) NewResource(typeToken, name string, inputs resource.PropertyMap, provider, id string) (string, resource.PropertyMap, error) {
	return name + "_id", inputs, nil
}

func (mocks) Call(token string, args resource.PropertyMap, provider string) (resource.PropertyMap, error) {
	return args, nil
}

// Applying unit tests.
func TestInfrastructure(t *testing.T) {
	err := pulumi.RunErr(func(ctx *pulumi.Context) error {
		infra, err := createInfrastructure(ctx)
		assert.NoError(t, err)

		var wg sync.WaitGroup
		wg.Add(3)

		// Test if the service has tags and a name tag.
		pulumi.All(infra.server.URN(), infra.server.Tags).ApplyT(func(all []interface{}) error {
			urn := all[0].(pulumi.URN)
			tags := all[1].(map[string]interface{})

			assert.Containsf(t, tags, "Name", "missing a Name tag on server %v", urn)
			wg.Done()
			return nil
		})

		// Test if the instance is configured with user_data.
		pulumi.All(infra.server.URN(), infra.server.UserData).ApplyT(func(all []interface{}) error {
			urn := all[0].(pulumi.URN)
			userData := all[1].(*string)

			assert.Nilf(t, userData, "illegal use of userData on server %v", urn)
			wg.Done()
			return nil
		})

		// Test if port 22 for ssh is exposed.
		pulumi.All(infra.group.URN(), infra.group.Ingress).ApplyT(func(all []interface{}) error {
			urn := all[0].(pulumi.URN)
			ingress := all[1].([]ec2.SecurityGroupIngress)

			for _, i := range ingress {
				openToInternet := false
				for _, b := range i.CidrBlocks {
					if b == "0.0.0.0/0" {
						openToInternet = true
						break
					}
				}

				assert.Falsef(t, i.FromPort == 22 && openToInternet, "illegal SSH port 22 open to the Internet (CIDR 0.0.0.0/0) on group %v", urn)
			}

			wg.Done()
			return nil
		})

		wg.Wait()
		return nil
	}, pulumi.WithMocks("project", "stack", mocks(0)))
	assert.NoError(t, err)
}
```

#### Unit testing with Python. 
```python 
import unittest
import pulumi

class MyMocks(pulumi.runtime.Mocks):
    def new_resource(self, type_, name, inputs, provider, id_):
        return [name + '_id', inputs]
    def call(self, token, args, provider):
        return {}

pulumi.runtime.set_mocks(MyMocks())

# Now, import the code that creates resources and then test it.
import infra

class TestingWithMocks(unittest.TestCase):
    # Test if the service has tags and a name tag.
    @pulumi.runtime.test
    def test_server_tags(self):
        def check_tags(args):
            urn, tags = args
            self.assertIsNotNone(tags, f'server {urn} must have tags')
            self.assertIn('Name', tags, 'server {urn} must have a name tag')

        return pulumi.Output.all(infra.server.urn, infra.server.tags).apply(check_tags)

    # Test if the instance is configured with user_data.
    @pulumi.runtime.test
    def test_server_userdata(self):
        def check_user_data(args):
            urn, user_data = args
            self.assertFalse(user_data, f'illegal use of user_data on server {urn}')

        return pulumi.Output.all(infra.server.urn, infra.server.user_data).apply(check_user_data)

    # Test if port 22 for ssh is exposed.
    @pulumi.runtime.test
    def test_security_group_rules(self):
        def check_security_group_rules(args):
            urn, ingress = args
            ssh_open = any([rule['from_port'] == 22 and any([block == "0.0.0.0/0" for block in rule['cidr_blocks']]) for rule in ingress])
            self.assertFalse(ssh_open, f'security group {urn} exposes port 22 to the Internet (CIDR 0.0.0.0/0)')

        # Return the results of the unit tests.
        return pulumi.Output.all(infra.group.urn, infra.group.ingress).apply(check_security_group_rules)
```

Using testing frames that we are already a-custom to can benefit since these tests can be highly customizable.

### Integration Testing.

Using integration testing, we can look at testing from a different standpoint since we are testing against live natural resources to test their actual behavior.

An integration test invokes the Pulumi command-line interface (CLI) to deploy infrastructure to an [ephemeral environment](https://about.gitlab.com/blog/2020/01/27/kubecon-na-2019-are-you-about-to-break-prod/).

A good part about integration testing is that we are testing on natural resources in our cloud infrastructures. Since we are trying on actual resources, these tests can take longer than unit testing.

Benefits of running integration tests:

-  Project’s code is syntactically formed and runs without errors.
-  Stack’s configuration and secrets work and are interpreted correctly.
- Your project can be successfully deployed to your cloud provider of choice.

#### Integration testing with Go.
```Go
package test

import (
    "os"
    "path"
    "testing"

    "github.com/pulumi/pulumi/pkg/v2/testing/integration"
)

func TestExamples(t *testing.T) {
    awsRegion := os.Getenv("AWS_REGION")
    if awsRegion == "" {
        awsRegion = "us-west-1"
    }
    cwd, _ := os.Getwd()
    integration.ProgramTest(t, &integration.ProgramTestOptions{
        Quick:       true,
        SkipRefresh: true,
        Dir:         path.Join(cwd, "..", "..", "aws-js-s3-folder"),
        Config: map[string]string{
            "aws:region": awsRegion,
        },
    })
}
```


## Why should we use test with our Pulumi code?
Unit tests are ideal for validating inputs and ensuring that cloud resources are created. We can use [mocks](https://devopedia.org/mock-testing) to test if the webserver has a name tag. First, we’ll make the mocks and import the infrastructure that uses the mocks. Integration testing requires deploying resources in either a test environment or an ephemeral environment.


### Resources

[Testing | Pululmi](https://www.pulumi.com/docs/guides/testing/)

[Are you about to break Prod?](https://about.gitlab.com/blog/2020/01/27/kubecon-na-2019-are-you-about-to-break-prod/)

[Integration Testing | Pulumi ](https://www.pulumi.com/docs/guides/testing/integration/)

[Infrastructure Testing in Practice ](https://www.pulumi.com/blog/testing-in-practice/)

[Testing Practices for Cloud Engineering](https://www.pulumi.com/blog/infrastructure-testing-concepts/)

[Integration test with Pulumi and Azure Kubernetes Service](https://blexin.com/en/blog-en/integration-test-with-pulumi-and-azure-kubernetes-service/)

As always, happy coding, my friends! 😊
