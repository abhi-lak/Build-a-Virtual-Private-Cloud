

# Build a Virtual Private Cloud

**Author:** Abhishek Iyer  
**Email:** aiyer084@gmail.com

## Build a Virtual Private Cloud (VPC)


## Introducing Today's Project!

In this project, I will demonstrate how to create and configure an Amazon VPC from scratch, including setting up a public subnet and attaching an internet gateway to enable internet connectivity for resources within the VPC.

I'm doing this project to learn the fundamentals of AWS networking, how cloud resources communicate within a private network, and how to securely control access to AWS infrastructure using VPC components such as subnets, route tables, and internet gateways.

### What is Amazon VPC?

Amazon VPC (Virtual Private Cloud) is an AWS networking service that allows users to create a logically isolated virtual network within the AWS cloud. Within a VPC, users can define IP address ranges, create subnets, configure route tables, attach internet gateways, and control network access for their AWS resources.

It is useful because it provides security, flexibility, and control over how cloud resources communicate with each other and with external networks. A VPC enables users to design custom network architectures, isolate sensitive resources, manage internet access, and implement security measures that meet organizational requirements. It serves as the foundation for deploying and managing applications securely in AWS.

In today's project, I used Amazon VPC to create and configure a private virtual network in AWS. I created a VPC with a custom IPv4 CIDR block, launched a subnet within the VPC, enabled automatic public IP address assignment for resources in the subnet, and attached an Internet Gateway to provide internet connectivity. This allowed me to understand how AWS networking components work together to create a secure and organized environment for deploying cloud resources.

### Personal reflection

This project took me 1-2 hrs 

One thing I didn't expect in this project was how many components are required to make a subnet truly public. I initially thought that creating a subnet and assigning public IP addresses would automatically provide internet access, but I learned that an Internet Gateway and the correct route table configuration are also necessary. This helped me better understand how AWS networking works and how different VPC components work together to enable secure internet connectivity.

---

## Virtual Private Clouds (VPCs)

### What I did in this step

In this step, I will access the AWS VPC console and create a new Virtual Private Cloud (VPC) with a custom IP address range. A VPC provides an isolated virtual network where AWS resources can be launched and managed securely. It serves as the foundation for configuring subnets, routing, internet access, and other networking components required in the project.

### How VPCs work

VPCs (Virtual Private Clouds) are logically isolated virtual networks within AWS that allow users to launch and manage cloud resources in a secure and controlled environment. A VPC acts like a private data center in the cloud, giving you complete control over your network configuration, including IP address ranges, subnets, route tables, internet gateways, and security settings.

By creating a VPC, organizations can separate their resources from those of other AWS customers while still benefiting from AWS's scalable infrastructure. Within a VPC, you can define public and private subnets, control inbound and outbound traffic using security groups and network access control lists (NACLs), and establish connectivity to the internet, other VPCs, or on-premises networks.

VPCs are essential because they provide security, flexibility, and network isolation. 

### Why there is a default VPC in AWS accounts

There was already a default VPC in my account ever since my AWS account was created. This is because AWS automatically creates a default VPC in each AWS Region for new accounts to help users quickly launch resources without having to configure networking manually. The default VPC comes preconfigured with subnets, route tables, an internet gateway, and basic settings, making it easier to get started with AWS services such as EC2 instances. This allows users to deploy resources immediately while learning AWS, although custom VPCs are often created in production environments to provide greater control, security, and network customization.

![Image](http://learn.nextwork.org/enthusiastic_navy_agile_cape_gooseberry/uploads/aws-networks-vpc_2facf927)

### Defining IPv4 CIDR blocks

To set up my VPC, I had to define an IPv4 CIDR block, which is a range of IP addresses that can be used within the VPC. CIDR (Classless Inter-Domain Routing) notation specifies both the network address and the size of the network using a suffix such as /16, /24, or /28. For example, a CIDR block of 10.0.0.0/16 provides 65,536 IP addresses that can be assigned to resources and subnets within the VPC. Defining a CIDR block is important because it determines the address space available for communication between resources in the network and helps organize and manage network traffic efficiently.

---

## Subnets

### What I did in this step

In this step, I will create a subnet within my VPC and assign it a portion of the VPC's IP address range.

Because subnets allow me to divide the VPC into smaller, organized network segments where AWS resources can be deployed. This helps improve network management, security, and resource organization by controlling how resources communicate and access the internet.

### Creating and configuring subnets

Subnets are smaller network segments created within a VPC by dividing its IP address range into separate sections. They help organize resources, control network traffic, and determine how resources connect to other parts of the network and the internet. Resources such as EC2 instances, databases, and load balancers are launched inside subnets.

There are already subnets existing in my account, one for every Availability Zone in the Region, because AWS automatically creates default subnets when it creates a default VPC. These default subnets are distributed across the Region's Availability Zones to provide high availability and make it easier to launch resources without needing to configure networking manually.

### Public vs private subnets

The difference between public and private subnets is that resources in a public subnet can communicate directly with the internet, while resources in a private subnet cannot be accessed directly from the internet and typically communicate through a NAT device or other controlled methods.

For a subnet to be considered public, it has to have a route in its route table that directs internet-bound traffic (0.0.0.0/0) to an Internet Gateway attached to the VPC. My subnet is not considered public yet because, although it has been created, it does not yet have a route to an Internet Gateway that would allow resources within it to access the internet.

![Image](http://learn.nextwork.org/enthusiastic_navy_agile_cape_gooseberry/uploads/aws-networks-vpc_157c4219)

### Auto-assigning public IPv4 addresses

Once I created my subnet, I enabled auto-assign public IPv4 addresses. This setting makes sure that any new EC2 instance launched in the subnet automatically receives a public IPv4 address.

This setting makes sure instances can communicate directly with the internet so that they can be accessed remotely (for example, via SSH or a web browser), download updates, and interact with external services without requiring additional network configuration. This is a common setting for public subnets where internet connectivity is needed.

---

## Internet gateways

### What I did in this step

In this step, I will create and attach an Internet Gateway to my VPC and configure routing so that internet traffic can flow between my VPC and the internet.

Because an Internet Gateway provides a connection between resources in the VPC and the public internet. Without it, resources in the subnet would not be able to send or receive internet traffic, even if they have public IP addresses assigned. This step is essential for making a subnet truly public and enabling external connectivity.

### Setting up internet gateways

Internet gateways are AWS networking components that connect a VPC to the public internet. They act as a gateway between resources inside the VPC and external networks, allowing internet-bound traffic to leave the VPC and incoming traffic to reach resources that have public IP addresses and the appropriate security rules.

An Internet Gateway is highly available, scalable, and managed by AWS. When attached to a VPC and referenced in a route table, it enables resources in public subnets to communicate with the internet. Without an Internet Gateway, resources in a VPC cannot directly access or be accessed from the internet, even if they have public IP addresses assigned.

Attaching an internet gateway to a VPC means that the VPC now has a path to communicate with the public internet. When combined with the appropriate route table entries and public IP addresses, resources in public subnets can send and receive internet traffic, allowing them to be accessed remotely and interact with external services.

If I missed this step, resources in the VPC would not be able to access the internet or receive traffic from it, even if they were located in a public subnet and had public IP addresses assigned. The Internet Gateway is a critical component that enables external connectivity for resources within the VPC.

![Image](http://learn.nextwork.org/enthusiastic_navy_agile_cape_gooseberry/uploads/aws-networks-vpc_4ae90410)

---

## Using the AWS CLI

### What I'm doing in this extension

In this project extension, I will use AWS CloudShell and AWS CLI commands to create a VPC, subnet, and internet gateway instead of configuring them manually through the AWS Management Console.

Because the AWS CLI provides a faster, more efficient, and automated way to manage cloud infrastructure. It allows network resources to be created using commands, improves consistency, reduces manual effort, and helps develop Infrastructure as Code (IaC) skills that are commonly used by cloud and DevOps professionals. By completing this secret mission, I can compare the CLI approach with the console-based approach and understand the advantages of automation in AWS.

### Exploring CloudShell and CLI

VPC resources could also be created with CloudShell, which is a browser-based shell environment provided by AWS that allows users to run commands directly from the AWS Management Console without installing any software on their local machine. CloudShell comes preconfigured with the AWS CLI and is automatically authenticated with the user's AWS account permissions.

CLI (Command Line Interface) is a tool that allows users to interact with AWS services by typing commands instead of using the graphical AWS Management Console. The AWS CLI can be used to create, manage, and automate cloud resources efficiently. It is especially useful for repetitive tasks, scripting, and infrastructure automation, making it a valuable tool for cloud engineers and system administrators.

### Debugging my setup

o set up a VPC or a subnet, you can use the command aws ec2 create-vpc or aws ec2 create-subnet with the appropriate CIDR block and configuration options.

Make sure to avoid errors by including a correctly formatted CIDR block, ensuring that the subnet CIDR range falls completely within the VPC's CIDR range, and using a subnet size that is smaller than the VPC. It's also important to verify that the CIDR blocks do not overlap with existing networks and that all command parameters are entered correctly. Incorrect CIDR notation or an invalid subnet range are common causes of errors when creating VPC resources with the AWS CLI.

![Image](http://learn.nextwork.org/enthusiastic_navy_agile_cape_gooseberry/uploads/aws-networks-vpc_9b2465411)

### Comparing CloudShell vs AWS Console

Compared to using the AWS Console, an advantage of using commands is that they are faster, easier to automate, and can be reused in scripts to create infrastructure consistently. The AWS CLI is especially useful when managing multiple resources or performing repetitive tasks because it reduces manual effort and supports Infrastructure as Code practices.

An advantage of using the Console is that it provides a visual interface that is easier for beginners to understand. It guides users through the configuration process, makes it simpler to review settings, and reduces the chance of syntax errors that can occur when typing commands.

Overall, I preferred using the AWS Console for learning because it helped me understand the relationships between VPC components visually. However, CloudShell and the AWS CLI are more efficient for automation and managing resources at scale, making them valuable tools for cloud engineers and DevOps professionals.

---

---
