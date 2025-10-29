# Terraform

### Core Concepts & Principles

These questions test your fundamental understanding of IaC and Terraform's philosophy.

**1. What is Infrastructure as Code (IaC) and why is it important for a company like Navigraph?** * **Answer**: IaC is the practice of managing and provisioning infrastructure (like servers, databases, and networks) using code instead of manual, physical processes. It's important because it enables **consistency** (every environment is identical), **scalability** (we can spin up new environments on-demand), and **version control** (we can track, review, and roll back infrastructure changes just like application code).

**2. Explain the difference between Terraform's declarative approach and an imperative approach (e.g., Ansible).**

- **Answer**: A **declarative** approach focuses on *what* the final state of the infrastructure should be. You describe the desired state in a configuration file, and Terraform figures out *how* to get there. An **imperative** approach focuses on *how* to get there, defining a step-by-step procedure to provision resources. Terraform is ideal for provisioning a real-time data ingestion system because you simply declare the number of EC2 instances, and Terraform handles the creation and scaling, whereas an imperative tool would require you to write a script detailing each step.

---

### Terraform Specifics

These questions dive into Terraform's core features and workflow.

**3. What is a Terraform state file, and what are the best practices for managing it in a team environment?**

- **Answer**: The **Terraform state file** (`terraform.tfstate`) is a JSON file that maps the resources in your code to the real-world infrastructure. It's Terraform's source of truth for understanding the current state of your environment. For a team, storing the state file locally is a huge risk because it can lead to conflicts and data corruption. The best practice is to use **remote state**, storing the file in a centralized, secure location like an AWS S3 bucket. You should also enable **state locking** (e.g., using an AWS DynamoDB table) to prevent multiple users from running `terraform apply` at the same time and corrupting the state file.
- How to recover if tf state file got corrupted?
    
    If your Terraform state file is corrupted, you can take a few steps to fix it. The best approach depends on whether you're using a local state file or a remote backend. The goal is to either recover the state from a backup or manually fix the file so it reflects your actual infrastructure.
    
    ### 1. Restore from Backup 💾
    
    This is the fastest and most reliable method if you're using a remote backend. Remote backends like S3, Azure Storage, or Terraform Cloud automatically version your state files, providing a built-in backup.
    
    - **For S3**: The state file is stored as an object in your S3 bucket. If you have **versioning enabled** on the bucket (which is highly recommended for Terraform state), you can simply find the last known good version of the state file and restore it.
    - **For Terraform Cloud/Enterprise**: These services have an integrated state management feature that automatically handles versioning and backups.You can navigate to your workspace and select a previous state version to restore.
    
    ### 2. Manual Recovery ✍️
    
    If you're using a local state file and don't have backups, or if the corruption is minor, you can try to fix it manually. This method is risky and should only be used as a last resort.
    
    - **Make a Copy**: First, make a backup of the corrupted `terraform.tfstate` file before you start.
    - **Use `terraform state pull`**: Run `terraform state pull` to download the state from the backend (if you have one). This might give you a slightly more readable version of the file. You can then try to edit the JSON file to remove the corrupted sections.
    - **Use `terraform state rm` and `terraform import`**: If the corruption is isolated to a single resource, you can use these commands to remove the corrupted resource from the state and then re-import it.4
        - `terraform state rm <resource_address>`: This command removes the specified resource from the state file.
        - `terraform import <resource_address> <resource_id>`: This command imports an existing resource into your state file. This is a very effective way to fix a single corrupted resource entry.
    
    ### 3. Last-Resort Strategy 🚨
    
    If all else fails, you can generate a new state file from your existing infrastructure. This is a dangerous operation and should be handled with extreme care to avoid data loss.
    
    - **Delete the Corrupted State File**: Remove the corrupted `terraform.tfstate` file.
    - **Use `terraform import` extensively**: You will need to manually import every single resource from your cloud provider back into your new state file. This is a very time-consuming and error-prone process.
    - **Plan a Small Change**: After importing, run `terraform plan` to confirm that the state file now accurately reflects your infrastructure and that no unintended changes will be made.
    
    The best practice is to **always use a remote backend with versioning enabled**. This completely mitigates the risk of a corrupted local state file and simplifies recovery in the event of an issue.
    

**4. How would you use Terraform modules to build infrastructure?**

- **Answer**: Terraform **modules** are reusable, self-contained packages of Terraform code. Instead of writing the same code to create an RDS database for every environment, I'd create a `database` module that takes input variables like `environment_name` or `instance_size`. This promotes the **DRY (Don't Repeat Yourself)** principle, reduces errors, and allows for consistency across our development, staging, and production environments.

**5. Explain the purpose of `terraform plan` and `terraform apply`.**

- **Answer**: The `terraform plan` command creates an **execution plan**. It's a dry run that shows you exactly what changes Terraform will make to your infrastructure (e.g., which resources will be created, modified, or destroyed) before you commit to them. The `terraform apply` command then takes that plan and executes it, provisioning or updating the resources to match the desired state in your configuration files.

---

### Real-World Scenarios

These questions test your ability to apply IaC concepts to practical, real-world problems.

**6. How would you provision different environments (dev, staging, prod) using Terraform?**

- **Answer**: I would use a combination of **Terraform workspaces** and a well-structured directory layout. Each environment would have its own workspace, which corresponds to a separate state file (e.g., `dev.tfstate`, `prod.tfstate`). This ensures that changes in one environment don't accidentally affect another. The environments would share the same core Terraform code, with the differences (like instance size or database name) managed using environment-specific `.tfvars` files.

**7. How would you handle sensitive data, like database passwords or API keys, in your Terraform configurations?**

- **Answer**: You should **never** hard-code sensitive data directly into your Terraform files or commit it to version control. Instead, I would use a **secrets management tool** like AWS Secrets Manager or HashiCorp Vault. In my Terraform code, I'd use a data source to dynamically fetch the secret at runtime. This keeps the sensitive information secure and out of the codebase.

**8. How do you prevent "drift" and ensure your infrastructure matches your code?**

- **Answer**: **Drift** is when your infrastructure's real-world state diverges from the state defined in your Terraform code. This can happen from manual changes. To prevent it, you should:
    1. **Restrict manual changes**: Use IAM policies to prevent direct changes to resources via the AWS console.
    2. **Use a CI/CD pipeline**: Automate all deployments through a pipeline that always runs `terraform plan` and `terraform apply` on every code change.
    3. **Regularly run `terraform plan`**: Periodically running `terraform plan` will detect any drift and alert you to resources that are out of sync.

---

Here are some common Terraform interview questions and answers.

## What is a Terraform Provider?

A **provider** is a plugin that enables Terraform to interact with a specific API to manage resources. Providers are the foundation of Terraform's multi-cloud capabilities. Each provider (e.g., AWS, Azure, GCP, Kubernetes) defines a set of resources that Terraform can create, read, update, and delete. Without a provider, Terraform wouldn't know how to communicate with a cloud service. For example, to create an AWS S3 bucket, your configuration must first declare the AWS provider.

---

## Explain the purpose of the Terraform state file.

The **Terraform state file** (`terraform.tfstate`) is a crucial file that maps the resources defined in your configuration files to the real-world infrastructure provisioned by Terraform. It stores metadata and a unique ID for each resource, allowing Terraform to track its current state. The state file helps Terraform:

- **Track Resources**: It knows which resources it has created and manages.
- **Plan Changes**: It compares the desired state (your code) with the actual state (the state file) to determine what changes to make during a `terraform plan`.
- **Handle Dependencies**: It uses the state to understand the relationships between resources, ensuring they are created or updated in the correct order.

---

## What are Terraform Modules and why are they used?

A **module** is a container for multiple Terraform configuration files that are grouped together to create a reusable component. Modules are used to:

- **Promote Reusability**: You can define a complex set of resources (e.g., a VPC with subnets, route tables, and gateways) once and reuse it across multiple projects or environments.
- **Improve Organization**: They help break down large, monolithic configurations into smaller, manageable, and logical components.
- **Maintain Consistency**: By using the same module across different environments (dev, staging, prod), you ensure that your infrastructure is provisioned consistently, reducing the chance of manual errors.

---

## What is the difference between `terraform plan` and `terraform apply`?

- **`terraform plan`**: This command creates an **execution plan** that shows you what actions Terraform will take to reach the desired state defined in your configuration. It is a dry-run command that gives you a preview of the changes (creations, modifications, or destructions) without actually making them. It is a critical step for verifying your changes before applying them.
- **`terraform apply`**: This command executes the actions defined in the plan. It provisions or modifies your infrastructure to match the state specified in your configuration files. After running `terraform apply`, Terraform updates the state file to reflect the new state of your infrastructure.

---

## What is a remote backend and why is it important?

A **remote backend** is a secure, shared location for storing your Terraform state file. By default, Terraform stores the state file locally, which is not suitable for team collaboration.

- **Collaboration**: A remote backend allows multiple team members to work on the same infrastructure code without conflicts.
- **State Locking**: Many remote backends, like Amazon S3 with DynamoDB, provide **state locking**, which prevents multiple people from running `terraform apply` at the same time, avoiding state corruption.
- **Security and Backup**: Storing the state file remotely, especially in a service with versioning (like S3), provides a secure backup and a single source of truth for your infrastructure. It also keeps sensitive data out of local development machines.

---

## How does Terraform handle secrets and sensitive data?

Terraform state files can contain sensitive information like passwords and keys, which should not be stored in plain text in a version control system.

- **Sensitive Variables**: You can mark a variable as sensitive in your configuration using `sensitive = true`. While the value is still stored in the state file, it will be redacted from the CLI output when running commands like `terraform plan` and `terraform apply`.
- **Environment Variables**: A better approach is to use environment variables (`TF_VAR_`) for secrets. This keeps the sensitive data out of your configuration files.
- **Secret Management Services**: The most secure and recommended method is to use a dedicated secret management service like **HashiCorp Vault**, **AWS Secrets Manager**, or **Azure Key Vault**. Terraform can be configured to fetch secrets from these services at runtime, ensuring the data is never exposed in the code or state file.