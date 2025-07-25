Infrastructure as Code (IaC): Terraform & Pulumi
“If it’s not version-controlled, it’s not real infrastructure.”

Infrastructure as Code (IaC) is the practice of managing and provisioning cloud resources using code, enabling repeatability, scalability, and automation for your systems.

🚀 Why Infrastructure as Code?
Benefit	Explanation
🔄 Repeatable Deployments	Avoid manual steps and human error
🧪 Testable Infrastructure	Validate changes in CI before deploying
📚 Documentation via Code	The code itself becomes living documentation
🔧 Automation Friendly	Works with pipelines, auto-scaling, monitoring
🛑 Disaster Recovery	Rebuild infra from scratch quickly if needed
🧯 Rollback Safe	Version control lets you revert changes easily

🧰 Popular IaC Tools
Tool	Language Style	Highlights
Terraform	Declarative (HCL)	Provider-agnostic, great community, state mgmt
Pulumi	Imperative (TS, Python)	Write infra in familiar languages, more dynamic
Ansible	YAML + tasks	Good for config mgmt, not cloud-native IaC
CloudFormation	Declarative (JSON/YAML)	AWS-native, deeply integrated with AWS services

🔍 Terraform vs Pulumi – Quick Comparison
Feature	Terraform	Pulumi
Language	HCL (HashiCorp Config Lang)	Python, TypeScript, Go, etc.
State Management	Built-in or remote (e.g. S3)	Built-in + cloud backend
Modularity	Modules	Packages
Learning Curve	Simple & declarative	Easier if you're a developer
Ecosystem	Mature	Rapidly growing

🧱 Basic Terraform Example
hcl
Copiar
Editar
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "example" {
  bucket = "my-app-bucket"
  acl    = "private"
}
🧠 Pulumi Equivalent (Python)
python
Copiar
Editar
import pulumi
import pulumi_aws as aws

bucket = aws.s3.Bucket("example", acl="private")
📁 IaC Best Practices
Best Practice	Why It Matters
Use version control (Git)	Enables collaboration, history, rollback
Store state remotely (e.g., S3 + DynamoDB)	Avoid state conflicts, enable team usage
Use modules (Terraform) / packages (Pulumi)	Improve reusability and organization
Apply policy-as-code (e.g., OPA)	Enforce security and cost controls
Tag all resources	For billing, monitoring, and cleanup

🛠️ DevOps Integration Flow (Text Diagram)
text
Copiar
Editar
+------------+     +-----------+     +-----------+     +-----------+
|   Dev Git  | --> |   CI/CD   | --> |  Terraform | -->|   Cloud   |
|  Changes   |     |  Pipeline |     |   /Pulumi  |     | Resources |
+------------+     +-----------+     +-----------+     +-----------+
        |
    Pull Request → Plan → Review → Apply → Monitor
📋 Real-World Use Cases
Use Case	Example
Environment provisioning	Spin up QA/staging environments identical to prod
Auto-scaling infrastructure	Define ASGs, launch templates, cloudwatch alarms
Immutable infrastructure	Replace, not mutate servers — better reliability
Secure resource setup	Private subnets, IAM roles, KMS keys as code

📦 Interview Soundbite
“I use Terraform to codify infrastructure into reusable modules with remote state storage in S3 and DynamoDB locking. For app-specific needs, I might reach for Pulumi with Python for better control flow.”

✅ Summary
IaC Trait	Description
Code-Driven Infra	Infra becomes versioned, reviewed, tested
Reusable Modules	Avoid duplication, enforce standards
Tooling Options	Choose Terraform for maturity, Pulumi for code
Works with CI/CD	Automate deployment and rollback safely