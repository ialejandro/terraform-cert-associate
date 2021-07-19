## Section 1. IaC with Terraform

### Terraform Workflow

* Write your Terraform code
* Plan or review the changes
* Deploy your changes on infraestructure

### Terraform Init (Initializing the working directory)

* `terraform init`: initialzes the working directory that contains your Terraform code
  1. Download modules and plugins
  2. Configure the backend to store Terraform state file

**This is the first command to execute**

### Terraform Plan, Apply and Destroy

* `terraform plan`: after write your code you need review resources that you will deploy:
  * this command doesn't deploy anything. Its `read-only` command
  * Allows the user to review the action before execute anything
  * Auth credentials are required if use cloud provider (*for example*)
* `terraform apply`: deploy infraestructure
  * deploys statements in the code
  * update the state file
* `terraform destroy`: destroy infraestructure

### Understanding Terraform Code

**Configuring the provider**

```ruby
provider "aws" {
  alias  = "default"
  region = "eu-west-1"
}
```

This is a simple snippet for the AWS provider:
* provider: reserved keyword
* aws: provider name
* { \<code> }: config params

You can reference in every resource and provider what you want, for example:

```ruby
resource "aws_instance" "ec2" {
  provider      = aws.default

  ami           = "<ami-id>"
  instance_type = "m5a.2xlarge"
}
```

**Resource block**

```ruby
resource "aws_instance" "ec2" {
  ami           = "<ami-id>"
  instance_type = "m5a.2xlarge"
}
```

This is a simple snippet for the AWS resource instance:
* resouce: reserved keyword
* aws_instance: resource from AWS Terraform provider
* ec2: arbitrary string (must be unique with the same resource)
* { \<code> }: aws_instance params

If you want call resource into Terraform, the name complete is: `aws_instance.ec2`

**Data resource block**

```ruby
data "aws_instance" "ec2" {
  instance_id = "i-XXXXXXXXXX"
}
```

This is a simple snippet for the AWS resource instance:
* data: reserved keyword
* aws_instance: resource from AWS Terraform provider
* ec2: arbitrary string (must be unique with the same resource)
* { \<code> }: data params

If you want call resource into Terraform, the name complete is: `data.aws_instance.ec2`

**Keypoints**

* Terraform execute `.tf` extension
* Page with all [providers](https://registry.terraform.io/browse/providers)

### Installing Terrraform annd Providers

**Installing**

* Method 1. Download, unzip and use
* Method 2. Add Terraform repository (linux)
* Method 3: [tfenv (macosx | linux | windows git-bash)](https://github.com/tfutils/tfenv)

**Providers**

* Abstracting integrations with API control layer of the infra
* Page with all [providers](https://registry.terraform.io/browse/providers)
* Providers are plugin, so they have their release separete from the core Terraform release
* Providers install when initializing working directory (`terraform init`)
* Recommends to use a specific version of the provider