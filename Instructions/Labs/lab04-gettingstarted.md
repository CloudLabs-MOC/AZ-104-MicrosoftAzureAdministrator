# AZ-104: Microsoft Azure Administrator Workshop

### Overall Estimated Duration: 60 minutes

## Overview

In this hands-on lab, you'll gain practical experience in setting up and managing Azure virtual networking. You will learn how to create a virtual network and subnets using both the Azure portal and a template, ensuring you can handle network configuration efficiently. Additionally, you'll configure security settings by linking an Application Security Group with a Network Security Group to control traffic and enhance security. Finally, you’ll set up public and private DNS zones to manage domain name resolution. By the end of this lab, you'll be skilled in creating, configuring, and securing virtual networks, and managing DNS in Azure, equipping you with essential networking skills for your Azure environment.

## Objective

**Create a virtual network with subnets using the portal Objective**: Learn how to create a virtual network and its associated subnets using the Azure portal. This includes configuring the network address space and subnet details to organize and manage network resources effectively.

**Create a virtual network and subnets using a template Objective**: Understand how to use an Azure Resource Manager (ARM) template to deploy a virtual network and subnets. This task helps you automate network creation and ensures consistency in network configurations.

**Create and configure communication between an Application Security Group and a Network Security Group Objective**: Gain experience in setting up security rules to manage traffic between an Application Security Group (ASG) and a Network Security Group (NSG). This includes configuring inbound and outbound rules to control access and enhance network security.

**Configure public and private Azure DNS zones Objective**: Learn how to create and manage public and private DNS zones in Azure. This involves setting up DNS records for name resolution both within the virtual network and for external access, ensuring proper connectivity and domain name management.

## Pre-requisites

Fundamental knowledge in configuring and managing Azure virtual networks and DNS, including an understanding of network subnets, security groups, and DNS zones, essential for effective network management and security in Azure.

## Architecture

The architecture focuses on setting up and managing Azure virtual networks to ensure proper network segmentation and security. You start by creating a virtual network and subnets to organize network resources efficiently. The architecture includes configuring an Application Security Group (ASG) and a Network Security Group (NSG) to control traffic and enhance security between different network segments. Additionally, the setup involves configuring public and private DNS zones to handle domain name resolution for both internal and external access, ensuring reliable connectivity and proper management of network resources.


## Architecture Diagram

![image](../media/az104-lab04-architecture.png)

## Explanation of Components

**Microsoft Azure Virtual Network**: A foundational element of Azure that provides a secure and isolated network environment for your Azure resources. It enables you to define your own network address space and create subnets for organizing resources.

**Subnets**: Logical divisions within a virtual network that help in segmenting the network to improve management and security. Each subnet can host different types of resources and be assigned specific network security rules.

**Application Security Group (ASG)**: A feature that simplifies the management of network security policies by grouping virtual machines and other resources. It allows you to apply security rules to a group of resources rather than individually, making it easier to manage network traffic.

**Network Security Group (NSG)**: A network security feature that contains a list of security rules to control inbound and outbound network traffic to Azure resources. NSGs can be associated with subnets or individual network interfaces to enforce traffic control policies.

**Public DNS Zone**: A DNS zone in Azure that handles name resolution for domain names accessible from the internet. It allows you to manage DNS records, such as A records, to map domain names to IP addresses.

**Private DNS Zone**: A DNS zone in Azure used for name resolution within a virtual network. It ensures that DNS queries for internal domain names are resolved within the network, providing private name resolution for internal resources.


# Getting Started with the lab
 
 Once the environment is provisioned, a virtual machine (JumpVM) and lab guide will get loaded in your browser. Use this virtual machine throughout the workshop to perform the lab. You can see the number on the bottom of the Lab guide to switch to different exercises of the lab guide.
 
## Accessing Your Lab Environment
 
Once you're ready to dive in, your virtual machine and lab guide will be right at your fingertips within your web browser.
 
![Access Your VM and Lab Guide](../media/labguide.png)

### Virtual Machine & Lab Guide
 
Your virtual machine is your workhorse throughout the workshop. The lab guide is your roadmap to success.
 
## Exploring Your Lab Resources
 
To get a better understanding of your lab resources and credentials, navigate to the **Environment** tab.
 
![Explore Lab Resources](../media/env.png)
 
## Utilizing the Split Window Feature
 
For convenience, you can open the lab guide in a separate window by selecting the **Split Window** button from the top right corner.
 
![Use the Split Window Feature](../media/split.png)
 
## Managing Your Virtual Machine
 
Feel free to start, stop, or restart your virtual machine as needed from the **Resources** tab. Your experience is in your hands!
 
![Manage Your Virtual Machine](../media/resourses.png)

## **Lab Duration Extension**

1. To extend the duration of the lab, kindly click the **Hourglass** icon in the top right corner of the lab environment. 

    ![Manage Your Virtual Machine](../Labs/Images/gext.png)

    >**Note:** You will get the **Hourglass** icon when 10 minutes are remaining in the lab.

2. Click **OK** to extend your lab duration.
 
   ![Manage Your Virtual Machine](../Labs/Images/gext2.png)

3. If you have not extended the duration prior to when the lab is about to end, a pop-up will appear, giving you the option to extend. Click **OK** to proceed.
 
## Let's Get Started with Azure Portal
 
1. On your virtual machine, click on the Azure Portal icon as shown below:
 
    ![Launch Azure Portal](../Labs/Images/azure.png)
 
2. You'll see the **Sign into Microsoft Azure** tab. Here, enter your credentials:
 
   - **Email/Username:** <inject key="AzureAdUserEmail"></inject>
 
      ![](../Labs/Images/image7.png)
 
3. Next, provide your password:
 
   - **Password:** <inject key="AzureAdUserPassword"></inject>
 
      ![](../Labs/Images/image8.png)

1. If you see the pop-up **Action Required**, click **Ask Later**.
   
     ![](../Labs/Images/asklater.png)

1. First-time users are often prompted to Stay Signed In, if you see any such pop-up, click on No.

1. If a **Welcome to Microsoft Azure** popup window appears, click **Cancel** to skip the tour.
    
     ![](./media/gettingstarted-new-2.png)  

1. Click **Next** from the bottom right corner to embark on your Lab journey!
 
    ![Start Your Azure Journey](../media/num.png)

  In this hands-on lab, you'll be equipped with the skills to effectively manage Azure virtual networks, including creating and configuring virtual networks, subnets, security groups, and DNS zones. This experience will enable you to design and secure network infrastructure in Azure, ensuring robust network management and connectivity.

## Support Contact

1. The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels tailored specifically for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

   Learner Support Contacts:

   - Email Support: labs-support@spektrasystems.com
   - Live Chat Support: https://cloudlabs.ai/labs-support

1. Now, click on Next from the lower right corner to move on to the next page.
   
## Happy Learning!!
