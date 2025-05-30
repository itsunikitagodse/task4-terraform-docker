#  Task 3: Infrastructure as Code (IaC) with Terraform – Docker Container Provisioning

## 🛠 Tools Used

- **Terraform**
- **Docker**

## Files

- `main.tf` – Terraform configuration file
- `apply.log` – Output log of `terraform apply`
- `destroy.log` – Output log of `terraform destroy`

## ✅ Prerequisites

Ensure the following tools are installed and configured:

- [Docker](https://docs.docker.com/get-docker/)  
- [Terraform](https://developer.hashicorp.com/terraform/downloads)

_Check if Docker is running:_
>>docker --version
>>docker ps

_Check Terraform version:_

>>terraform version

**Project Structure**

terraform-docker/
│

├── main.tf

├── README.md

├── apply.log

└── destroy.log

**⚙️ Setup Instructions**

_1. Clone/Create Project Directory_

>>mkdir terraform-docker && cd terraform-docker

_2. Create main.tf_

Paste the following Terraform code into main.tf:


terraform {
  required_providers {
      docker = {
          source  = "kreuzwerker/docker"
      version = "~> 3.0.2"
          }
      }
  }


provider "docker" {}


resource "docker_image" "nginx" {

  name         = "nginx:latest"
  keep_locally = false
  
}

resource "docker_container" "nginx_container" 

{

  name  = "nginx_terraform"
  
  image = docker_image.nginx.latest
  
  ports {
    internal = 80
    
    external = 8080
  }
  
}



**How To Do**

**Step 1: Initialize Terraform**

>>terraform init

Initializes the working directory and installs the Docker provider.

**Step 2: Preview Execution Plan**

>>terraform plan

See what Terraform will create or change.

**Step 3: Apply Configuration**

>>terraform apply | tee apply.log

Deploys the Docker container.

**Step 4: Verify the Container**

>>docker ps

You should see a container named nginx_terraform running on port 8080.

_Try accessing it in your browser:_
http://localhost:8080

**Step 5: Check Terraform State**

>>terraform state list

Lists all resources Terraform is managing.

**Clean Up(To destroy the infrastructure)**

>>terraform destroy | tee destroy.log

This will remove the container and image.

**Notes**-
This setup runs NGINX in a Docker container using port mapping 8080:80.

Customize the image or container settings in main.tf as needed.

**Files That Will NOT Be Pushed (Ignored by .gitignore) 
**These are excluded to avoid clutter or security risks:

.terraform/ (Terraform's local cache)

*.tfstate, 

*.tfstate.backup (state files with resource metadata)

.terraform.lock.hcl

*.log (if added to .gitignore)

.DS_Store (macOS system file)



