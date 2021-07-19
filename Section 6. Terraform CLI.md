## Section 6. Terraform CLI

### Terraform CLI

#### Terraform fmt

* Formats code for readability
* Helps code consistent easy to maintain
* Use for:
  * Before pushing code on your version control
  * After upgrade Terraform or modules
  * If you made any changes on code

Example
```ruby
terraform fmt
```

#### Terraform taint

* Recreate a resource (destroyed and created) in the next `terraform apply` execution
* Modify the state file (because recreate the resource)
* Maybe can force to recreate other resources (if they're referenced)
* Use for:
  * Run provisioners
  * Replace misbehaving resources
  * Check recreate doesn't affect or change any attributes

Example
```ruby
terraform taint <resource>
```

#### Terraform import

* Maps existing resources to Terraform using a resource `id`
* `id` depends of provider. AWS, GCP, Azure, etc.
* Don't import the same resources to multiple Terraform code because create conflicts in the future
* Use for:
  * You need to work with existing rsources
  * Not allowed create new resources
  * You don't have any control of creation procress of infrastructure

Example
```ruby
terraform import <resource>.<id>
```

#### Terraform Configuration Block

* Config for controlling Terraform behavior
* Block only allows constant values. Don't allow named resources or variables
* Use for:
  * Configure vendor backend for store state files
  * Terraform version
  * Terraform providers
  * Experimental Terraform features
  * Passing metadata to providers

Example
```ruby
provider "aws" {
  region = var.region
}

provider "kubernetes" {
  host                   = data.aws_eks_cluster.cluster.endpoint
  cluster_ca_certificate = base64decode(data.aws_eks_cluster.cluster.certificate_authority.0.data)
  token                  = data.aws_eks_cluster_auth.cluster.token
}

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 3.30.0"
    }
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = "2.0.2"
    }
  }

  required_version = ">= 0.15"

  backend "s3" {
    bucket = "CHANGE-ME"
    key    = "CHANGE-ME"
    region = "CHANGE-ME"
  }
}
```

### Terraform Workspaces (CLI)

* Workspaces are alternate state files within the same working directory
* By default, starts with a single workspace: `default`. Cannot be deleted
* Are meant to share resources and to help enable collaboration

Examples

```ruby
# create workspace
terraform workspace new <name>

# select workspace
terraform workspace select <name>

# access workspace name
${terraform.workspace}
```

* Cases of use:
  * Test changes using parallel, distinct copy of infra
  * Modeled againsts brances in version control
  * Use logic to deploy some code. For example, if the workspace is "developer" you can make some tests and if it's "default" you can deploy directly

Examples

```ruby
# create resources based of workspace name
resource "aws_instance" "test" {
  count = terraform.workspace == "default" ? 2 : 1
  ...
}
  
# create resources based of workspace name
resource "aws_s3_bucket" "test" {
  bucket = terraform.workspace
  ...
}
```

### Debugging Terraform

* Use `TF_LOG` env var for enable verbose in Terraform. By default, send logs to `stderr`
* Allows: `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`
* Send logs to path: `TF_LOG_PATH`
* Set logging env vars:
  * `export TF_LOG=XXXX`
  * `export TF_LOG_PATH=XXXX`