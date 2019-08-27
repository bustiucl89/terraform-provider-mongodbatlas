# ARCHIVED ---- MongoDB Atlas Provider

#This repo is now available within the HashiCorp Terraform Repo at https://github.com/terraform-providers/terraform-provider-mongodbatlas



This is the repository for the Terraform MongoDB Atlas Provider, which allows one to use Terraform with MongoDB's Database as a Service offering, Atlas.
Learn more about Atlas at  [https://www.mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)

For general information about Terraform, visit the [official website](https://www.terraform.io) and the [GitHub project page](https://github.com/hashicorp/terraform).


# Requirements
- [Terraform](https://www.terraform.io/downloads.html) 0.12+
- [Go](https://golang.org/doc/install) 1.12 (to build the provider plugin)

# Developing the Provider
If you wish to work on the provider, you'll first need [Go](https://golang.org/doc/install) installed on your machine (please check the [requirements](#Requirements) before proceeding).

Note: This project uses [Go Modules](https://blog.golang.org/using-go-modules) making it safe to work with it outside of your existing [GOPATH](https://golang.org/doc/code.html#GOPATH). The instructions that follow assume a directory in your home directory outside of the standard GOPATH (i.e $HOME/development/terraform-providers/).

Clone repository to: `$HOME/development/terraform-providers/`

```
$ mkdir -p $HOME/development/terraform-providers/; cd $HOME/development/terraform-providers/
$ git clone git@github.com:mongodb/terraform-mongodbatlas
...
```

Enter the provider directory and run `make tools`. This will install the needed tools for the provider.
```
$ make tools
```

To compile the provider, run `make build`. This will build the provider and put the provider binary in the $GOPATH/bin directory.

```
$ make build
...
$ $GOPATH/bin/terraform-provider-mongodbatlas
...
```

# Using the Provider

To use a released provider in your Terraform environment, run [`terraform init`](https://www.terraform.io/docs/commands/init.html) and Terraform will automatically install the provider. To specify a particular provider version when installing released providers, see the [`Terraform documentation on provider versioning`](https://www.terraform.io/docs/configuration/providers.html#version-provider-versions).

To instead use a custom-built provider in your Terraform environment (e.g. the provider binary from the build instructions above), follow the instructions to [install it as a plugin](https://www.terraform.io/docs/plugins/basics.html#installing-a-plugin). After placing it into your plugins directory, run `terraform init` to initialize it.

For either installation method, documentation about the provider specific configuration options can be found on the [provider's website](https://www.terraform.io/docs/providers/).

# Testing the Provider

In order to test the provider, you can run `make test`. You can use [meta-arguments](https://www.terraform.io/docs/configuration/providers.html) such as `alias` and `version`. The following arguments are supported in the MongoDB Atlas `provider` block:

* `public_key` - (Optional) This is the MongoDB Atlas API public_key. It must be
  provided, but it can also be sourced from the `MONGODB_ATLAS_PUBLIC_KEY`
  environment variable.

* `private_key` - (Optional) This is the MongoDB Atlas private_key. It must be
  provided, but it can also be sourced from the `MONGODB_ATLAS_PRIVATE_KEY`
  environment variable.

~> **Notice:**  If you do not have a `public_key` and `private_key` you must create a programmatic API key to configure the provider (see [Creating Programmatic API key](#Creating-Programmatic-API-key)). If you already have one, you can continue with [Configuring environment variables](#Configuring-environment-variables)

# Running the acceptance test

#### Programmatic API key

It's necessary to generate and configure an API key for your organization for the acceptance test to succeed. To grant programmatic access to an organization or project using only the [API](https://docs.atlas.mongodb.com/api/) you need to know:

  1. The programmatic API key has two parts: a Public Key and a Private Key. To see more details on how to create a programmatic API key visit https://docs.atlas.mongodb.com/configure-api-access/#programmatic-api-keys.

  1. The programmatic API key must be granted roles sufficient for the acceptance test to succeed. The Organization Owner and Project Owner roles should be sufficient. You can see the available roles at https://docs.atlas.mongodb.com/reference/user-roles.

  1. You must [configure Atlas API Access](https://docs.atlas.mongodb.com/configure-api-access/) for your programmatic API key. You should allow API access for the IP address from which the acceptance test runs.

#### Configuring environment variables

You must also configure the following environment variables before running the test:

##### MongoDB Atlas env variables
```sh
$ export MONGODB_ATLAS_PROJECT_ID=<YOUR_PROJECT_ID>
$ export MONGODB_ATLAS_ORG_ID=<YOUR_ORG_ID>
```
##### AWS env variables

- For `Network Peering` resource configuration:
```sh
$ export AWS_ACCOUNT_ID=<YOUR_ACCOUNT_ID>
$ export AWS_VPC_ID=<YOUR_VPC_ID>
$ export AWS_VPC_CIDR_BLOCK=<YOUR_VPC_CIDR_BLOCK>
$ export AWS_REGION=<YOUR_REGION>
```
~> **Notice:** For more information about the Network Peering resource, see: https://docs.atlas.mongodb.com/reference/api/vpc/

- For `Encryption at Rest` resource configuration:
```sh
$ export AWS_ACCESS_KEY_ID=<YOUR_ACCESS_KEY_ID>
$ export AWS_SECRET_ACCESS_KEY=<YOUR_SECRET_ACCESS_KEY>
$ export AWS_CUSTOMER_MASTER_KEY_ID=<YOUR_CUSTOMER_MASTER_KEY_ID>
```
~> **Notice:** For more information about the Encryption at Rest resource, see: https://docs.atlas.mongodb.com/reference/api/encryption-at-rest/


In order to run the full suite of Acceptance tests, run ``make testacc``.

~> **Notice:** Acceptance tests create real resources, and often cost money to run. Please note in any PRs made if you are unable to pay to run acceptance tests for your contribution. We will accept "best effort" implementations of acceptance tests in this case and run them for you on our side. This may delay the contribution but we do not want your contribution blocked by funding.

```
$ make testacc
```

Contributing
---------------------------

Terraform is the work of thousands of contributors. We appreciate your help!

We welcome issues of all kinds including feature requests, bug reports, and general questions within this repo.

To contribute, please read the Terraform contribution guidelines:
https://www.terraform.io/docs/extend/community/contributing.html

Note: Additional guidelines for this Provider may be added in a future CONTRIBUTING file.

If you have issues on GitHub, they are intended to be related to bugs or feature requests with provider codebase. See https://www.terraform.io/docs/extend/community/index.html for a list of community resources to ask questions about Terraform.

Thanks
---------------------------
We'd like to thank [Akshay Karle](https://github.com/akshaykarle) for writing the first version of a Terraform Provider for MongoDB Atlas and paving the way for the creation of this one.
