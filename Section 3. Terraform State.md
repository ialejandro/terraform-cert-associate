## Section 3. Terraform State

### Terraform State

* Maps real resources to Terraform config
* State is stored locally in a file called `terraform.tfstate` by default
* Any modification, Terraform refreshes the state file
* Resource dependency is tracked via the state file
* Boost deployment performance by caching resource attributes for subsequent use

### Terraform State Command

* `terraform state` is a utily to manipulate and read `terraform.tfstate`:
  * manualy remove a resource
  * listing tracked resources
  * advanced state management

Common commands:

* `terraform state list`: get all list resources tracked on state file
* `terraform state rm`: remove manually a resource form state file
* terraform state show: get all info from one resource

### Local and Remote State Storage

* By default save state locally
* Saves state to a remote data source: AWS S3, Google Storage, Azure Blob...
* Allow share state file between differents teams
* Allows locking state enable by default, but isn't compatible with all rmeote storage
* You can share "output" values with other Terraform config or code