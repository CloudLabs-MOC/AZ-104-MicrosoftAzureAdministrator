# AZ-104: Microsoft Azure Administrator Workshop

### Overall Estimated Duration: 60 minutes

## Overview

In this hands-on lab, you'll learn how to manage Azure resources using ARM (Azure Resource Manager) templates. You'll practice creating, editing, and deploying templates using different methods, including Azure PowerShell, Azure CLI, and Azure Bicep.

## Objective

By the end of this lab, you will be able to create, edit, and deploy ARM templates using various methods, including Azure PowerShell, Azure CLI, and Azure Bicep.

**Create an ARM Template**: Learn to create and export an ARM template for a managed disk.

**Edit and Redeploy an ARM Template**: Modify an existing template and redeploy it to create a new resource.

**Deploy Using Azure PowerShell**: Use Azure PowerShell in Cloud Shell to deploy a template.

**Deploy Using Azure CLI**: Use Azure CLI in Cloud Shell to deploy a template.

**Deploy Using Azure Bicep**: Deploy a managed disk using a Bicep file.

## Pre-requisites

Fundamental knowledge in managing Azure resources and familiarity with ARM templates, Azure PowerShell, Azure CLI, and Azure Bicep, which are essential for automating and managing deployments in Azure.

## Architecture

In this lab, you’ll use ARM templates, which are JSON files that define and configure Azure resources. You’ll deploy these templates using Azure PowerShell or Azure CLI in Cloud Shell, an online tool that provides access to both environments. Additionally, you’ll work with Azure Bicep, a simpler language for writing templates that gets converted into ARM templates. This setup allows you to manage and automate the deployment of Azure resources efficiently.

## Architecture Diagram

![image](../media/az104-lab03-architecture.png)

## Explanation of Components

**ARM Templates**: Define the resources and their settings. They are reusable and can be versioned for different deployments.

**Azure PowerShell and CLI**: Used to deploy ARM templates. PowerShell is more familiar to Windows users, while CLI is often preferred by Linux users.

**Azure Bicep**: Provides a more user-friendly way to create ARM templates, simplifying complex deployments.

**Cloud Shell**: Offers an integrated environment for deploying and managing resources with minimal setup.

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
