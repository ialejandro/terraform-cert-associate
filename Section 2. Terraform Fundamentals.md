## Section 2. Terraform Fundamentals

### Terraform State: The concept

This is a `json` file containing all meteadata about your Terraform deployment and resources deployed.

* Store in flat files, default: `terraform.tfstate`
* Helps to calculate deployment delta and create new deployments plan
* If you lose your Terraform state file... all code will recreate again

### Terraform Variables and Outputs

#### Declare variable

```ruby
variable "test" {
  description = "test"
  type        = number
  default     = 0
}
```

This is a simple snippet for the AWS resource instance:
* variable: reserved keyword
* test: arbitrary string (must be unique with the same resource)
* { \<code> }: variable config

If you want reference var: `var.test`

To save all default vars, recommended create new file called: `terraform.tfvars`

**Validate variables**

```ruby
variable "test" {
  description = "test"
  type        = number
  default     = 0
  validation {
    condition = var.test < 10
      error_mesage = "The value must be less than 10"
  }
}
```

If you want **hide sensitive** values, add next param: `sensitive = true`.

#### Variable types

***Simple***
* string
* number 
* bool

```ruby
variable "test" {
  type    = number
  default = 0
}
```

***Complex***
* list
* set
* map
* object
* tuple

```ruby
variable "test_complex" {
  type    = list(number)
  default = [0,1,2]
}
```

#### Outputs

```ruby
output "test" {
  description = "test output"
  value        = aws_instance.ec2.public_ip
}
```

This is a simple snippet for the AWS resource instance:
* output: reserved keyword
* test: arbitrary string (must be unique with the same resource)
* { \<code> }: output params

The outputs show after execute `terraform apply`, you can use to track the resources

### Provisioners: When to use them

Provisioners can be used to model specific actions on the local machine or on a remote machine in order to prepare servers or other infrastructure objects for service.

* When you want to invoke actions not covered by Teraform
* If we execute a command in provisioners and return code is different that zero, will be fail state

```ruby
resource "null_resource" "test" {

  # terraform apply
  provisioner "local-exec" {
    command = "echo 0 > test"
  }
  
  # terraform destroy
  provisioner "local-exec" {
    when    = destroy
    command = "echo 0 > test"
  }
}
```

Another example, if we want wait until AWS EC2 instance is ok:

```ruby
resource "aws_instance" "ec2" {
  ami           = "ami-XXXXX"
  instance_type = "m5a.large"
  ...
  
  provider "local-exec" {
    command = "aws ec2 wait instance-status-ok --region eu-west-1 --instance-ids ${self.id}"
  }
}
```