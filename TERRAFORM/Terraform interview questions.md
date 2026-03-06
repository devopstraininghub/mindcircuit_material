
1. **What is Terraform?**
   
   Terraform is an open-source Infrastructure as Code (IaC) tool by HashiCorp used to provision and manage infrastructure across multiple cloud providers.

3. **Difference between Terraform and Ansible?**

   * Terraform → Infrastructure provisioning (IaC).
   * Ansible → Configuration management (CM).
     Terraform is declarative; Ansible is procedural.

4. **What language does Terraform use?**

   HCL (HashiCorp Configuration Language).

5. **What are Providers in Terraform?**

   Plugins that interact with cloud platforms (AWS, Azure, GCP, etc.). Example: `provider "aws" { region = "us-east-1" }`.

6. **What are Terraform Resources?**

   The fundamental blocks (e.g., `aws_instance`, `aws_s3_bucket`) that define infrastructure components.

7. **What are Variables in Terraform?**

   Inputs used to parameterize configurations. Declared with `variable`, assigned via `.tfvars` or CLI.

8. **What is a Terraform Module?**

   A collection of `.tf` files organized to be reusable. Helps maintain DRY (Don’t Repeat Yourself).

9. **What is a Terraform Backend?**

   Defines where Terraform stores its state file (local, S3, etc.).

10. **What is a Terraform State File?**

   `terraform.tfstate` keeps track of real-world resources and their mapping to Terraform configuration.

11. **Why is Remote State used?**

    For team collaboration, consistency, and locking. Common backends: AWS S3 + DynamoDB for locking.

12. **How do you manage Terraform State locking?**

    Using backends like S3 with DynamoDB (AWS).

13. **What is the difference between `terraform refresh` and `terraform apply`?**

* `refresh` → Syncs state with real infrastructure without making changes.
* `apply` → Applies changes to match configuration.

13. **What are Terraform Workspaces?**

    Used for managing multiple environments (dev, staging, prod) with the same configuration.

14. **Difference between `terraform plan` and `terraform apply`?**

* `plan` → Shows what will change.
* `apply` → Actually applies the changes.

15. **What is `terraform import`?**

    Brings an existing infrastructure resource under Terraform management.

16. **What is the `taint` command in Terraform?**

The `terraform taint` command marks a resource as tainted, meaning Terraform will destroy and recreate it during the next `terraform apply`.

####  Use case:

If a resource (like an EC2 instance) is unhealthy or misconfigured but still exists in state, and you want Terraform to replace it, run:

```bash
terraform taint aws_instance.example
```

Then on next `terraform apply`, Terraform will destroy and recreate that instance.

>  Note: In newer Terraform versions (`>= v1.0`), `terraform taint` is deprecated in favor of:

```bash
terraform apply -replace=aws_instance.example
```

17. **How do you handle secrets in Terraform?**

* Use environment variables, Vault, SSM, or remote secret managers.
* Never hardcode in `.tf` files.

18. **What are Terraform Provisioners?**

    Used to execute scripts or commands on resources (e.g., `remote-exec`). Not recommended for heavy config mgmt.

19. **How do you ensure Terraform code reusability?**

* Store modules in private registries/Git.

* Modules are reusable sets of Terraform configuration files.

* Benefits: Reusability, consistency, maintainability, and organization of code.

20. **How do you handle dependencies between resources?**

    Terraform automatically creates a dependency graph. For explicit dependency: `depends_on`.

21. **What is a Data Source in Terraform?**

    Reads existing data from providers (e.g., `data "aws_ami"`) without creating resources.

22. **How do you version Terraform code?**


* Use Git for version control.
* Pin provider versions in `required_providers`.
* Use `terraform.tfstate` in remote backend.


23. **How do you handle multi-cloud deployments?**

    Define multiple providers in `.tf` files and use aliases to distinguish.

24. **How do you upgrade Terraform version safely?**

* Run `terraform init -upgrade`.
* Check compatibility with `terraform plan`.
* Upgrade incrementally.

25. **Do you have any experience using functions in Terraform? If yes, explain.**

Yes, functions like lookup(), join(), split(), cidrsubnet(), and coalesce() are used to manipulate data during infrastructure creation.


26. **How do you avoid conflicts with the Terraform state file? Where do you store it?**

Store the state in Amazon S3, and use DynamoDB for state locking to prevent race conditions in team environments.

27. **What is the difference between for_each and for in Terraform?**

for_each is used to create multiple resources (like EC2 instances or subnets) from a map or set.

for is used inside expressions to transform lists or maps (e.g., locals, output, dynamic blocks).


  28. **How do you import existing cloud infrastructure under Terraform management?**

To bring already-created AWS infrastructure (or any cloud resource) under Terraform’s control without recreating it, use the `terraform import` command.

#### Steps:

1. Define the resource block in your Terraform code (without properties for now):

   ```hcl
   resource "aws_instance" "example" {
     # leave blank for now or minimal attributes
   }
   ```

2. Run the import command:

   ```bash
   terraform import aws_instance.example i-0abcd1234efgh5678
   ```

3. After import, run:

   ```bash
   terraform plan
   ```

   → to see differences. Then manually update your code to match the real resource configuration.

29. **How to run terraform plan and terraform apply from Jenkins for different environments?**

* Use environment-specific `.tfvars` and Jenkins parameters:

```sh
terraform init
terraform plan -var-file=dev.tfvars
terraform apply -var-file=dev.tfvars
```
