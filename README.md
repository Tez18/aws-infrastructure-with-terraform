# Terraform AWS Infrastructure & Web Server Deployment

![AWS Architecture Diagram](./aws_deployment_architecture_v2.png)

This project uses **Terraform** to provision a resilient AWS network infrastructure and automatically deploys a web server. It sets up a Virtual Private Cloud (VPC), public subnets across multiple availability zones, and an EC2 instance that serves a custom web page.

## 🏗️ Architecture Overview

The infrastructure follows AWS best practices for high availability and network isolation:

*   **VPC**: `10.0.0.0/16` (Name: `Terraform-Assignment-VPC`) - The core network container.
*   **Availability Zones**:
    *   **us-east-1a**: Hosts the active web server.
    *   **us-east-1b**: Reserved for future high-availability scaling.
*   **Public Subnets**:
    *   `Public-Subnet-1` (10.0.1.0/24) in AZ 1a.
    *   `Public-Subnet-2` (10.0.2.0/24) in AZ 1b.
*   **Internet Gateway**: Provides internet access to the public subnets.
*   **Security Group**: Acts as a virtual firewall for the instance.
*   **EC2 Instance**: 
    *   **Name**: `Terraform-Web-Server`
    *   **Type**: `t2.micro`
    *   **OS**: Amazon Linux 2
    *   **Software**: Apache HTTP Server (`httpd`)

### Deployment Visualization
The diagram above illustrates the traffic flow:
**Internet** -> **Internet Gateway** -> **Route Table** -> **Public Subnet** -> **Web Server (EC2)**.

## 📸 Deployment Validation

*As tested on 2026-01-08:*

1.  **VPC & Subnets**: Successfully created with correct CIDR blocks and associations.
2.  **Instance State**: The `Terraform-Web-Server` instance successfully boots and reaches the **Running** state.
3.  **Web Access**: The Apache server starts automatically, identifying itself and serving the `index.html` page (which can be customized to show a "Deployment Successful" landing page).

## 🚀 Getting Started

### Prerequisites
*   [Terraform](https://www.terraform.io/) (v1.0+)
*   AWS CLI (Configured with `aws configure`)

### Quick Start

1.  **Initialize the Project**
    ```bash
    terraform init
    ```

2.  **Verify the Plan**
    ```bash
    terraform plan
    ```

3.  **Deploy Infrastructure**
    ```bash
    terraform apply -auto-approve
    ```
    *Wait for the "Apply complete!" message.*

4.  **Access the Application**
    *   Copy the `ec2_public_ip` from the output.
    *   Open your browser and navigate to: `http://<ec2_public_ip>`
    *   You should see the web page served by your new instance.

5.  **Clean Up**
    To remove all resources:
    ```bash
    terraform destroy -auto-approve
    ```

## 📂 File Structure

*   `main.tf`: The primary logic defining the VPC, Subnets, SG, and EC2.
*   `variable.tf`: Configuration variables (Region: `us-east-1`, Type: `t2.micro`).
*   `provider.tf`: AWS provider settings.
*   `output.tf`: Displays critical info (Public IP, VPC ID) after deployment.
*   `index.html`: The HTML file used for the web server content.

---
*Generated with ❤️ by Antigravity*
