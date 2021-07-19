## Section 5. Built-in Functions and Dynamic Blocks

### Bult-in Funtions

* Futions are pre-packaged to transform and combine values
* User defined functions (UDF) arent allowed
  * Example: *fuctionName(arg1, arg2...)*
* Built-in functions are useful to make dynamic and flexible code

```ruby
# def variable
variable "test" {
  type    = string
  default = example
}

# def resource
resource "instance" "test" {
   ...
   tags = {
     Name = join("_", ["this", "is", "and", var.test])
   }
}

# Tag: { "Name" = "this_is_and_example" } 
```

* Complete function list: <https://www.terraform.io/docs/language/functions/index.html>

### Type Constraints

* Control the type of variable
* Primitive (single type value):
  * number: `replicas = 3`
  * string: `name = "string"`
  * bool: `backup = true`
* Complex (multriple types in single var):
  * list
  * tuple
  * map
  * object

#### Complex Type

* **Collection** types allow multiple values of one primitive type
  * Constructors:
    * list(\<type>)
    * map(\<type)
    * set(\<type>)

Example

```ruby
variable "test" {
  type    = list(string)
  default = ["test", "test"]
}
```

* **Structural** types allow multiple values of different primitive types to be grouped together
  * Constructors:
    * list(\<type>)
    * map(\<type)
    * set(\<type>)

Example

```ruby
variable "test" {
  type = object({
    name  = string
    human = bool
  })
}
```

#### Dynamic Types

* `any` is a placeholder for a primitive type to be decided on runtime

Example

```ruby
variable "test" {
  type    = list(any)
  default = [1, 2, 3]
}

# recognizes all values as numbers
```

### Dynamic Blocks

* Constructs repeatable nested config blocks inside Terraform resources
* Expect a complex variable type to iterate
* Acts like a for loop and outputs a nested block for each element in your variable
* *Be careful*: can be hard to read and mantain
* Available on following block types:
  * resource
  * data
  * provider
  * provisioner
* Case of uses:
  * code look cleaner

**Example without dynamic blocks**

```ruby
resource "aws_security_group" "test" {
  name        = "test"
  ...

  ingress {
    description      = "test"
    from_port        = 443
    to_port          = 443
    protocol         = "tcp"
    cidr_blocks      = [aws_vpc.main.cidr_block]
  }
  
  ingress {
    description      = "test2"
    ...
  }
}
```

**Example with dynamic blocks**

```ruby
# resource
resource "aws_security_group" "test" {
  name        = "test"
  ...

  dynamic "ingress" {
    for_each = var.rules
    content {
      from_port = ingress.value["from_port"]
      to_port   = ingress.value["tp_port"]
      protocol  = ingress.value["protocol"]
      ...
    }
  }
}

# var.rules

rules = [
  # rule 1
  {
    from_port = 443
    to_port   = 443
    protocol  = "tcp"
    ...
  },
  # rule 2
  {
    from_port = 22
    to_port   = "tcp"
    ...
  },
]
```