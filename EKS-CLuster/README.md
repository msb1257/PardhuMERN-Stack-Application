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

5. Configure access entry for the current role

<img width="1433" alt="Screenshot 2024-12-03 at 12 08 29 PM" src="https://github.com/user-attachments/assets/f7fdbe1d-b4dd-4cfd-97f0-66077fb4417c">

6. Add role as "AmazonEKSAdminPolicy" && "AmazonEKSClusterAdminPolicy"

<img width="1433" alt="Screenshot 2024-12-03 at 12 09 54 PM" src="https://github.com/user-attachments/assets/3d659247-4aa2-4810-bc09-7753b6674cfa">

7. Once the cluster is up, you can configure kubectl to interact with it by using the aws eks update-kubeconfig command. Here's the command to set up the kubeconfig file:
```bash
aws eks --region ap-south-1 update-kubeconfig --name mern-cluster
```

8. Verify resources
<img width="1433" alt="Screenshot 2024-12-03 at 12 12 54 PM" src="https://github.com/user-attachments/assets/caa1c4f1-b311-4ab7-aaec-174dc8a15354">

### NOTE: Make sure to delete the resources:
```bash
terraform destroy
```
