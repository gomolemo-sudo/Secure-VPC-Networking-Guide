# Building a Secure VPC Networking Environment: A Step-by-Step Guide  

## Introduction  
When setting up a cloud-based architecture, security and scalability are key priorities. In this article, we walk through the creation of a secure Virtual Private Cloud (VPC) environment on AWS, focusing on a two-tier architecture with a bastion host for enhanced security. This lab explores setting up public and private subnets, configuring secure access, and controlling traffic to protect sensitive resources.

## Creating the Public Subnet 
To start, I created a public subnet within the existing Lab VPC. I assigned it the IPv4 CIDR block 10.0.0.0/24 and ensured it was in Availability Zone 'a' of my region. Next, I set up an internet gateway and attached it to the Lab VPC. I updated the associated route table to direct all outbound traffic (0.0.0.0/0) through this internet gateway, allowing the subnet access to the internet.

 ## Setting Up a Bastion Host
I deployed an EC2 instance in the public subnet to act as a bastion host. Using the Amazon Linux 2023 AMI, I chose a t2.micro instance type and disabled the Auto-assign Public IP option. I then configured a security group to allow SSH traffic only from my IP address, enhancing security. The bastion host enables secure access to instances in the private subnet.

## Allocating an Elastic IP Address
Since the bastion host requires a consistent public IP, I allocated an Elastic IP address and associated it with the instance. This ensures that administrative access remains stable, even if the instance is stopped and restarted. The Elastic IP simplifies firewall configurations by always providing the same trusted IP address.

## Testing SSH Connection to the Bastion Host
Using the SSH key pair provided, I tested the connection to the bastion host via SSH. For Windows, this involved configuring PuTTY, while macOS and Linux users can use the ssh command directly. The successful connection confirmed that the Elastic IP and security configurations were set correctly.

## Creating a Private Subnet
I proceeded to create a private subnet with the CIDR block 10.0.1.0/24 in the same Availability Zone as the public subnet. This subnet will host internal resources like the application server, which does not need direct internet access.

## Configuring a NAT Gateway
To allow the private subnet to access the internet for updates while remaining unreachable from outside, I created a NAT gateway in the public subnet. This required allocating another Elastic IP. I then set up a route table for the private subnet, directing outbound internet traffic through the NAT gateway.

## Launching an EC2 Instance in the Private Subnet
An application server was launched in the private subnet using the Amazon Linux 2023 AMI with a t2.micro instance type. I configured its security group to only allow SSH access from the bastion host’s security group. This setup ensures that administrative access is tightly controlled.

## Configuring SSH Passthrough
To connect from the bastion host to the private instance, I used SSH agent forwarding. This method securely passes the private key stored on my local system to the bastion host, allowing seamless access to the private instance without exposing the key. On Windows, I used Pageant, while on macOS and Linux, the ssh-add command was used.

## Testing Connectivity from Bastion to Private Instance
After connecting to the bastion host, I used SSH to access the EC2 instance in the private subnet. I verified that the instance could access the internet using the ping command, ensuring the NAT gateway was functioning correctly.

## Creating a Custom Network ACL
To enhance security further, I created a custom network ACL for the private subnet. By default, the custom ACL denies all inbound and outbound traffic, so I added rules to allow necessary communication. This adds another layer of control over network traffic, complementing the security group.

## Testing the Network ACL
I launched a test EC2 instance in the public subnet and allowed ICMP traffic via its security group. Using the private instance, I tested connectivity to the test instance using the ping command. I then updated the network ACL to block ICMP traffic specifically for the test instance’s IP. This immediately stopped the pings, confirming that the ACL was effectively blocking traffic as intended.

## Skills Demonstrated  
- AWS Networking (VPC, Subnets, Route Tables, Internet/NAT Gateways)  
- EC2 Configuration and Security Groups  
- Elastic IP Management  
- SSH Key Management and Passthrough  
- Network Access Control Lists (ACLs)

## Conclusion  
By setting up this VPC architecture, we’ve implemented a secure environment with isolated subnets, controlled access via a bastion host, and restricted traffic using network ACLs. This ensures that critical resources are protected while still enabling essential connectivity for updates and management.
This lab demonstrates best practices for secure VPC design, allowing organizations to safely experiment and deploy cloud-based architectures without risking production environments.  

---

### How to Use  
1. Clone this repository.  
2. Follow the step-by-step guide in the provided documentation to recreate the environment.  
3. Adapt configurations as needed for specific use cases.  

## Author  
**Gomolemo Hlatshwayo**  
- [LinkedIn](https://www.linkedin.com/in/gomolemo-hlatshwayo-2b844522a/)  
- [GitHub](https://github.com/gomolemo-sudo)  
