# AZ-104: Microsoft Azure Administrator Workshop

### Overall Estimated Duration: 120 minutes

## Overview

In these labs, you'll gain hands-on experience with Azure’s management and governance tools. First, you'll manage Azure subscriptions and control access using Role-Based Access Control (RBAC). This includes creating management groups, assigning built-in and custom roles, and monitoring role assignments with the Activity Log. Next, you'll implement governance by applying Azure policies and resource tags to ensure compliance and improve resource management. You'll also configure resource locks to protect critical resources from accidental changes or deletions, enhancing both organizational oversight and resource security.

## Objective

**Implement management groups**: Create and organize management groups to streamline the management of multiple Azure subscriptions and ensure consistent policy application.

**Review and assign a built-in Azure role**: Explore Azure’s built-in roles to understand their capabilities, and assign the appropriate role to a user or group to manage specific Azure resources effectively.

**Create a custom RBAC role**: Develop a custom role with specific permissions tailored to your organization’s needs, ensuring users have access only to the resources they need.

**Assign RBAC roles**: Assign the newly created or built-in roles to users or groups, controlling their access and permissions across Azure resources.

**Monitor role assignments with the Activity Log**: Use the Activity Log to track changes in role assignments, monitor who has been assigned what roles, and ensure that permissions are being applied correctly.

**Create and assign tags via the Azure portal**: Add metadata tags to Azure resources to categorize and manage them more efficiently, making it easier to track and report on resource usage.

**Enforce tagging via an Azure Policy**: Implement a policy that mandates the use of specific tags for new resources, ensuring consistency and adherence to organizational tagging standards.

**Apply tagging via an Azure Policy**: Use a policy to automatically apply required tags to existing resources that are missing them, improving resource management and compliance.

**Configure and test resource locks**: Set up resource locks to prevent accidental or unauthorized modifications or deletions, and verify that these locks are effectively protecting your resources.

## Pre-requisites

Fundamental Knowledge on managing Azure subscriptions, RBAC roles, and Azure Policy for governance. Familiarity with using the Azure Portal for role assignments, policy enforcement, and resource management.

## Architecture

The architecture involves organizing Azure subscriptions using management groups for better resource management and policy enforcement. Role-based access control (RBAC) is implemented to assign built-in and custom roles within these management groups, determining user access to resources. The activity log helps track role assignments and modifications. Additionally, governance is handled through Azure policies that enforce and apply resource tags, while resource locks are used to prevent accidental changes or deletions of critical resources. The Azure Portal is utilized for configuration and monitoring to ensure effective management and governance of resources.

## Architecture Diagram

**Lab 02a - Manage Subscriptions and RBAC**

![image](../media/lab02a.png)

**Lab 02b - Manage Governance via Azure Policy**

![Diagram of the task architecture.](./media/az104-lab02b-architecture.png)

## Explanation of Components

**Management Groups**: These are containers that help organize and manage multiple Azure subscriptions. They allow for hierarchical structuring of subscriptions, making it easier to apply policies and access controls across a large organization.

**Role-Based Access Control (RBAC)**: RBAC is a system for managing user permissions in Azure. It allows administrators to define roles with specific permissions and assign these roles to users, groups, or service principals. This ensures that individuals have the necessary access to perform their job functions while maintaining security.

**Activity Log**: This log provides a record of operations performed on resources within Azure, such as role assignments and resource modifications. It helps in tracking changes and diagnosing issues related to resource management.

**Azure Policies**: Policies in Azure enforce rules and effects over your resources to ensure compliance with organizational requirements. They can automatically apply resource tags, enforce tagging standards, and remediate non-compliance.

**Resource Tags**: Tags are metadata that help organize and manage Azure resources. They consist of a key-value pair and can be used for categorization, cost tracking, and reporting.

**Resource Locks**: These prevent accidental or unauthorized modifications and deletions of critical resources. Locks can be set to either prevent deletions or restrict changes, ensuring that important resources remain protected.

# Getting Started with the lab
 
Welcome to your AZ-104: Microsoft Azure Administrator  workshop! We've prepared a seamless environment for you to explore and learn Azure Services. Let's begin by making the most of this experience:
 
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

## Support Contact

1. The CloudLabs support team is available 24/7, 365 days a year, via email and live chat to ensure seamless assistance at any time. We offer dedicated support channels tailored specifically for both learners and instructors, ensuring that all your needs are promptly and efficiently addressed.

   Learner Support Contacts:

   - Email Support: labs-support@spektrasystems.com
   - Live Chat Support: https://cloudlabs.ai/labs-support

1. Now, click on Next from the lower right corner to move on to the next page.
   
## Happy Learning!!
