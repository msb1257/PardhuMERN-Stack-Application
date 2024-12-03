# Create EKS on AWS using Terraform.

### Prerequisites

1. We need to install AWS CLI as we will use the AWS CLI (aws configure) command to connect Terraform with AWS in the next steps.
```bash
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
```

2. Next, Install Terraform using the below link.
```bash
https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli
```

3. Connect Terraform with AWS
```bash
aws configure
aws configure list
```

4. Navigate to the directory where terrafrom config files are available and initialize terraform, this will intialize the terraform environment for you and download the modules, providers and other configuration required.
```bash
terraform init
```
