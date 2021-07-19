## Section 4. Terraform Modules

* Module is a container for multiple resources that are used together
* Every Terraform config has at leat one module, called **root** module, which consists of code files in your main working directory

### Accessing Terraform Modules

* Modules can be downloaded or ref from:
  * Terraform Public Registry
  * Private Registry
  * Your Local system
* All modules saves on: `.terraform/modules`

```ruby
module "test" {
  source   = "./modules/test"
  version  = 0.0.1
  instance = var.instance
}
```

This is a simple snippet for module `test`:
* module: reserved keyword
* test: module name
* { \<code> }: config paramsthe module
  * source: where is 
  * version: module version ref
  * instance: input var

You can use with: `count`, `for_each`, `providers`, `depends_on`

### Using Terraform Modules

* Modules can take input and provide outputs which you can use on your main code

```ruby
resource "aws_instance" "test" {
  # input param from module
  instance = module.test.instance
}
```

`module.test.instance` is a ref from module `test`

### Interacting with Terraform module Inputs and Outputs

#### Declaring Module in Code

* Module inputs are arbitrary and you must pass inside the `module` codeblock

```ruby
module "test" {
  source   = "./modules/test"
  version  = 0.0.1
  instance = var.instance
}
```

This is a simple snippet for module `test`:
* module: reserved keyword
* test: module name
* { \<code> }: config paramsthe module
  * source: where is 
  * version: module version ref
  * **instance: input var**

* Inside the module you can see:

```ruby
resource "aws_instance" "test" {
  # input param from module
  instance = var.instance
}
```

`var.instance` is var from input module codeblock

#### Outputs

* outputs declared inside Terraform module code can be call into the root module or main code

Example when the resource is on main code
```ruby
output "test" {
  value = aws_instance.test.instance
}
```

Example when the resource is on module code
```ruby
output "test" {
  value = module.test.instance
}
```

NOTE: *`module` must be the output value with `instance` var*