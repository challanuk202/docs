page_main_title: cliConfig
main_section: Platform
sub_section: Workflow
sub_sub_section: Resources

# cliConfig
`cliConfig` is a resource used to store configuration information needed to setup a Command Line Interface.

You can create a `cliConfig` resource by [adding](/platform/tutorial/workflow/crud-resource#adding) it to `shippable.resources.yml`.

Multiple cliConfig resources may be used as `IN`s and their respective CLIs are configured automatically. If more than one cliConfig of the same integration type is added, the last one used in `IN` statements wins.

```
resources:
  - name:           <string>
    type:           cliConfig
    integration:    <string>
    pointer:        <object>
```

* **`name`** -- should be an easy to remember text string

* **`type`** -- is set to `cliConfig`

* **`integration`** -- name of the subscription integration. Currently supported integration types are:
	* [AWS Keys](/platform/integration/aws-keys)
	* [Docker Hub](/platform/integration/docker-hub)
	* [Docker Trusted Registry](/platform/integration/docker-trusted-registry)
	* [Docker Private Registry](/platform/integration/docker-private-registry)
	* [Google Container Registry](/platform/integration/gcr)
	* [Google Container Engine](/platform/integration/gke)
	* [JFrog Artifactory](/platform/integration/jfrog-artifactory)
	* [Kubernetes](/platform/integration/kubernetes)
	* [Quay](/platform/integration/quay)

* **`pointer`** -- is an object that contains integration specific properties
	* For an AWS integration:

	        pointer:
	           region: <AWS region, e.g., us-east-1, us-west-1, etc.>

      * If you need the CLI to also configure ECR, you need to pass it in as a scope in the job. Example:

            jobs:
              - name: runSh-success-1
                type: runSh
                steps:
                  - IN: aws-keys-integration
                    scopes:
                      - ecr
                  - TASK:
                    - script: ls

	* For Google integrations, if region and clusterName are provided `gcloud` and `kubectl` will be automatically configured to use that region and cluster. If not provided, just the authentication to Google Cloud is done automatically.

	        pointer:
	          region:      <region, e.g., us-central1-a, us-west1-b, etc.>
	          clusterName: <cluster name>

<a name="cliConfigTools"></a>
## Configured CLI tools

A runSh job uses the subscription integration specified in the
cliConfig to determine which CLI tools to configure.
These tools are configured with the credentials contained in the subscription
integration. Here is a list of the tools configured for each integration type:

| Integration Type                    | Configured Tools           |
| ------------------------------------|-------------|
| AWS                                 | [AWS CLI](/platform/runtime/cli/aws); [AWS Elastic Beanstalk CLI](/platform/runtime/cli/awseb) |
| AWS with `ecr` scope                | [Docker Engine](/platform/runtime/cli/docker) |
| Docker Hub                          | [Docker Engine](/platform/runtime/cli/docker) |
| Docker Private Registry             | [Docker Engine](/platform/runtime/cli/docker) |
| Docker Trusted Registry             | [Docker Engine](/platform/runtime/cli/docker) |
| Google Container Engine             | [gcloud](/platform/runtime/cli/gke); [kubectl](/platform/runtime/cli/kubectl) |
| Google Container Registry (GCR)     | [Docker Engine](/platform/runtime/cli/docker) |
| JFrog Artifactory                   | [JFrog CLI](/platform/runtime/cli/jfrog) |
| Kubernetes                          | [kubectl](/platform/runtime/cli/kubectl) |
| Quay.io                             | [Docker Engine](/platform/runtime/cli/docker) |

## Used in Jobs
This resource is used as an `IN` for the following jobs

* [runCI](/platform/workflow/job/runci/)
* [runSh](/platform/workflow/job/runsh/)

## Default Environment Variables
Whenever `cliConfig` is used as an `IN` or `OUT` for a `runSh` or `runCI` job, a set of environment variables is automatically made available that you can use in your scripts.

`<NAME>` is the the friendly name of the resource with all letters capitalized and all characters that are not letters, numbers or underscores removed. For example, `my-key-1` will be converted to `MYKEY1`, and `my_key_1` will be converted to `MY_KEY_1`.

| Environment variable						| Description                         |
| ------------- 								|------------------------------------ |
| `<NAME>`\_NAME 							| The name of the resource. |
| `<NAME>`\_ID 								| The ID of the resource. |
| `<NAME>`\_TYPE 							| The type of the resource. In this case `cliConfig`. |
| `<NAME>`\_INTEGRATION\_`<FIELDNAME>`	| Values from the integration that was used. More info on the specific integration page. |
| `<NAME>`\_OPERATION 						| The operation of the resource; either `IN` or `OUT`. |
| `<NAME>`\_PATH 							| The directory containing files for the resource. |
| `<NAME>`\_POINTER\_REGION 				| Region defined in the pointer. Available if the integration is AWS or Google. |
| `<NAME>`\_POINTER\_CLUSTERNAME 			| ClusterName defined in the pointer. Available if the integration is Google. |
| `<NAME>`\_VERSIONNUMBER 					| The number of the version of the resource being used. |
| `<NAME>`\_VERSIONNAME						| The versionName of the version of the resource being used. |
| `<NAME>`\_VERSIONID    					| The ID of the version of the resource being used. |

## Shippable Utility Functions
To make it easy to use these environment variables, the platform provides a command line utility that can be used to work with these values.

How to use these utility functions is [documented here](/platform/tutorial/workflow/using-shipctl).

## Further Reading
* [Jobs](/platform/workflow/job/overview)
* [Resource](/platform/workflow/resource/overview)
* [Supported CLIs](/platform/runtime/overview#cli)
