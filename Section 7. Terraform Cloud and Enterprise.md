## Section 7. Terraform Cloud and Enterprise

### Hashicorp Sentinel - Policy as Code

* Enforces policies (restrinctions )on your code
* Has its own policy language - Sentinel language
* Designed to be approachable by non-programmers
* Benefits:
  *  Sandboxing - Guardrails for automation
  *  Codification - Easier undestarnding -> better collaboration
  *  Version control
  *  Testing and Automation
* Use cases:
  * Enforcing CIS (Center Information of Security) standards across AWS accounts
  * Checking make sure only instance types are used
  * Ensuring Security Groups don't allow some rules
  * Enseure that all EC2 instances have at least 1 tag

### Terraform Vault Provider

* Hashicorp Vault is a secrets management software
* Dynamically provisions credentials and rotates them
* Encrypts sensitive data in transit and at arest and provides ACL to access to secrets
* You can inject vault secrets on fly

* Benefits
  * You don't need to manage long-lived credentials
  * Inject secrets in Terraform at runtime
  * ACL to access temporary credentials

### Terraform Registry and Terraform Cloud Workspaces

#### Terraform Registry

Link: <https://registry.terraform.io>

* Public repository available Terraform providers and modules
* By default, Terraform looking for Terraform registry
* You can publish and share your own modules, you can use GitHub to store your content
* Can collaborate with others contributos to make changes to providers and modules
* Can be directly refrenced in your Terraform code

#### Terraform Cloud Workspaces

* The feature is the same that local Workspaces, but its slightly different in the sense because is hosted in the cloud and call with API
* Directories hosted in Terraform Cloud
* Stores old versions of state files by default
* Maintains a record of all execution
* Terraform commands executed on "managed" Terraform Cloud VMs
* You can integrate with CI, such as GitHub Actions, GitLab CI... and the Terraform instances will execute "plan", "apply", etc.

### Terraform Cloud

* Collaboration oriented Terraform Workflow:
  * Remote Terraform exeuction
  * Version control integration (GitHub, GitLab, etc)
  * Private Terraform Module registry
  * Workspace based org model
  * Remote state managment and CLI integration
  * Cost estimation and Setinel integration

### Summary

* Best practices for securing secrets and deployments:
  * Sentinel: constraints with policies
  * Vault provider: store and inject secrets securely during Terraform deployments
* Terraform OSS workspaces vs. Terraform Cloud workspaces:
  * Terraform OSS: workspace generate and track different state files against the same Terraform code in a directory, hosted locally on your system
  * Terraform Cloud: workspaces are hosted in the cloud and track configuration (ariables, state, secrets, etc)
* Terraform Cloud and Enterprise:
  * Collaboration (API-based workflos, version control and trigger-based deployments)
  * Sentinel integration
  * Cost estimation, shared workspaces, accross-based controls
