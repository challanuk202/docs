page_main_title: Google Container Engine
main_section: Platform
sub_section: Integrations
page_title: GKE integration

# Google Cloud Integration

Available under the Integration Family: **generic**

`Google Cloud` Integration is used to connect Shippable DevOps Assembly Lines platform to Google Cloud and manage entities and services provided by Google Cloud.

You can create this from the integrations page by following instructions here: [Adding an account integration](/platform/management/integrations/#adding-an-account-integration).

This is the information you would require to create this integration:

* **Name** -- friendly name for the integration
* **Project Name** -- Project Name from Google Developers Console
* **Service Account Email** -- Email for the service account
* **Credential File** -- JSON Security Key for Google Cloud

## Resources that use this Integration
Resources are the building blocks of assembly lines and some types of resources refer to integrations by their names. The following resource types can be created with a `Google Cloud` integration.

* [image](/platform/workflow/resource/image)
* [integration](/platform/workflow/resource/integration)

## Default Environment Variables
When you create a resource with this integration, and use it as an `IN` or `OUT` for a `runSh` or `runCI` job, a set of environment variables is automatically made available that you can use in your scripts.

`<NAME>` is the the friendly name of the resource with all letters capitalized and all characters that are not letters, numbers or underscores removed. For example, `my-key-1` will be converted to `MYKEY1`, and `my_key_1` will be converted to `MY_KEY_1`.

| Environment variable						| Description      |
| ------			 							|----------------- |
| `<NAME>`\_INTEGRATION\_NAME   			      | Name supplied in the integration |
| `<NAME>`\_INTEGRATION\_PROJECTNAME		      | Project Name supplied in the integration |
| `<NAME>`\_INTEGRATION\_SERVICEACCOUNTEMAIL  | Email address supplied in the integration |
| `<NAME>`\_INTEGRATION\_CREDENTIALFILE	      | Key supplied in the integration |

## Shippable Utility Functions
To make it easy to use these environment variables, the platform provides a command line utility that can be used to work with these values.

How to use these utility functions is [documented here](/platform/tutorial/workflow/using-shipctl).

## Further Reading
* [Quick Start to CI](/getting-started/ci-sample)
* [RunSh Job](/platform/workflow/job/runsh)
* [Jobs](/platform/workflow/job/overview)
* [Resources](/platform/workflow/resource/overview)
