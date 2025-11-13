# Lab - Implement Network Traffic Management

## Lab Overview

In this lab, you will learn to configure and manage Azure resources, including Virtual Networks, Load Balancers, Virtual Machines, and Network Security Groups, to create a secure and scalable infrastructure.
  
## Lab objectives
In this lab, you will complete the following tasks:
+ Task 1: Use a template to provision an infrastructure.
+ Task 2: Configure an Azure Load Balancer.
+ Task 3: Configure an Azure Application Gateway.

## Excercise 1: Implement Traffic Management

In this exercise, you will configure and implement Azure Traffic Manager to manage traffic distribution across multiple Azure resources for high availability and performance.

## Task 1: Use a template to provision an infrastructure

In this task, you will use a template to deploy one virtual network, one network security group, and two virtual machines.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Deploy a custom template (1)**, and then select **Deploy a custom template (2)** under services.

    ![image](../media/7-10-lab4-12.png)

1. On the custom deployment page, select **Build you own template in the editor**.

   ![image](../media/7-10-lab4-13.png)
   
1. On the edit template page, select **Load file**.

   ![image](../media/7-10-lab3-15.png)
   
1. In the **Open** dialog box, navigate to **C:\AllFiles\AZ-104-MicrosoftAzureAdministrator-Lab-Files\Allfiles\Labs\06 (1)**, select **az104-06-vms-template (2)**, and click **Open (3)**.

   ![image](../media/10-10-lab6-2.png)

1. Select **Save**.

   ![image](../media/10-10-lab6-5.png)
   
1. Select **Edit parameters** > **load file** and locate and select **az104-06-vms-parameters.json (2)**, and click **Open (3)**.

   ![image](../media/10-10-lab6-4.png)

   ![](../Labs/media/l6-image7.png)

   ![image](../media/10-10-lab6-3.png)

1. Select **Save**.

    ![image](../media/10-10-lab6-6.png)
   
1. Use the following information to complete the fields on the custom deployment page, leaving all other fields with the default value and select **Review + Create (4)**.

    | Setting       | Value         |
    | ---           | ---           |
    | Subscription  | your Azure subscription **(1)** |
    | Resource group | **az104-06-rg1 (2)** |
    | Password      | Provide a secure password (Please make sure the password contains uppercase, lowercase letters, digits and a special character and it is at least 8 characters long.) **(3)**|

     ![image](../media/10-10-lab6-7.png)
   
    >**Note:** If you receive an error that the VM size is unavailable, select a SKU that is available in your subscription and has at least 2 cores.
  
1. Select **Create**.

     ![image](../media/10-10-lab6-8.png)

    >**Note:** Wait for the deployment to complete before moving to the next task. The deployment should take approximately 5 minutes.

    >**Note:** Review the resources being deployed. There will be one virtual network with three subnets. Each subnet will have a virtual machine.

### Task 2: Implement Azure Load Balancer

In this task, you will implement an Azure Load Balancer in front of the two Azure virtual machines in the hub virtual network

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Load balancers (1)**, and then select **Load balancers (2)** under services.

   ![image](../media/10-10-lab6-9.png)

1. On **Load balancing and content delivery | Load Balancer** blade, click on **+ Create (1)** and select **Standard Loadbalancer (2)**

     ![image](../media/10-10-lab6-10.png)

1. Create a load balancer with the following settings (leave others with their default values) and click **Next: Frontend IP configuration > (8)** 

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab **(1)** |
    | Resource group | az104-06-rg1 **(2)**|
    | Name | **az104-06-lb4 (3)** |
    | Region| **<inject key="Region" enableCopy="false"/> (4)** |
    | SKU | **Standard (5)** |
    | Type | **Public (6)** |
    | Tier | **Regional (7)** |
    
      ![image](../media/10-10-lab6-11.png)

1.  On the Frontend IP configuration tab click **+ Add frontend IP configuration (1)** , under **Add frontend IP configuration** window add the following settings and click on **Save**
 
    | Setting | Value |
    | --- | --- |
    | Name | **az104-06-pip4 (2)** |
    | IP version | **IPv4 (3)** |
    | IP type | **IP address (4)** |
    | Public IP address | **Create new (5)** |

    ![image](../media/10-10-lab6-12.png)

    | Setting | Value |
    | --- | --- |
    | Name | **az104-06-pip4 (1)** |
    | Availability zone | Choose **1 (2)** and click **Save (3)** |

     ![image](../media/10-10-lab6-13.png)

1. Back on **Add frontend IP configuration** click on **Save** and click on **Next : Backend pools>**

    ![image](../media/10-10-lab6-14.png)

    ![image](../media/10-10-lab6-15.png)

1. On **Backend pools** tab, and click **+ Add a Backend pools**.

1. Add a backend pool with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **az104-06-lb4-be1 (1)** |
    | Virtual network | **az104-06-vnet1 (2)** |
    | Backend Pool Configuration | **NIC (3)** |
    
    ![image](../media/10-10-lab6-16.png)

1. Click **+ Add (4)**, under **IP Configurations** on the **Add backend pool** blade, select all the virtual machines **(5)** on the **Add IP configurations to backend pool** window and click on **Add (6)** and then click **Save** to save the IP configurations to the backend pool.

    ![image](../media/10-10-lab6-17.png)

    ![image](../media/10-10-lab6-18.png)

1. Click **Next: Inbound rules >**, **+ Add a load balancing rule** with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **az104-06-lb4-lbrule1 (1)** |
    | IP Version | **IPv4 (2)** |
    | Frontend IP Address | select the LoadBalancerFrontEnd from the drop down **(3)** |
    | Backend pool | **az104-06-lb4-be1 (4)** |    
    | Protocol | **TCP (5)** |
    | Port | **80 (6)** |
    | Backend port | **80 (7)** |
    | Session persistence | **None (8)** |
    | Idle timeout (minutes) | **4 (9)** |
    | Enable TCP reset | **Disabled (10)** |
    | Enable Floating IP | **Disabled (11)** |
    | Health probe | **Create new** |
    
    ![image](../media/10-10-lab6-19.png)

1. Click **create new (1)** under **Health probe**, on the **Add load balancing rules** blade.

    Add a health probe with the following settings:

    | Setting | Value |
    | --- | --- |
    | Name | **az104-06-lb4-hp1 (2)** |
    | Protocol | **TCP (3)** |
    | Port | **80 (4)** |
    | Interval | **5 (5)** |

    ![image](../media/10-10-lab6-20.png)

1. Click **Save (6)** and back on the **Add load balancing rules** blade, click **Save**.

     ![image](../media/10-10-lab6-21.png)

1. Click **Next: Outbound rules >**, followed by **Next: Tags >**, followed by **Next: Review + create >**. Let validation occur, and then click **Create** to submit your deployment.

    ![image](../media/10-10-lab6-22.png)

    ![image](../media/10-10-lab6-23.png)

    > **Note:** Wait for the Azure load balancer to be provisioned. This should take about 2 minutes.

1. Wait for the load balancing rule to be created, click **Go to resource** and on the **az104-06-lb4** load balancers blade, in the **Settings (1)** section, click **Frontend IP configuration (2)** and note the value of the **Public IP address**.

     ![image](../media/10-10-lab6-24.png)

     ![image](../media/10-10-lab6-25.png)

1. Start another browser window and navigate to the IP address you identified in the previous step.

1. Verify that the browser window displays the message **Hello World from az104-06-vm0** or **Hello World from az104-06-vm1**.

    ![image](../media/10-10-lab6-25.1.png)

1. Open another browser window but this time by using InPrivate mode and verify whether the target vm changes (as indicated by the message).

    > **Note:** You might need to refresh the browser window or open it again by using InPrivate mode.
    
> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com
. We are available 24/7 to help

<validation step="05e14bc6-a387-4fb8-baa1-c4dbacc8ae56" />

### Task 3: Implement Azure Application Gateway

In this task, you will implement an Azure Application Gateway in front of the two Azure virtual machines in the spoke virtual networks.

1. In the Azure portal search bar, type **Virtual networks (1)** and select **Virtual networks (2)** from the search results.

    ![image](../media/10-10-lab5-11.png)

1. On the **Virtual networks** blade, in the list of virtual networks, click **az104-06-vnet1**.

     ![image](../media/10-10-lab6-26.png)

1. On the  **az104-06-vnet1** virtual network blade, in the **Settings (1)** section, click **Subnets (2)**, and then click **+ Subnet (3)**.

    ![image](../media/10-10-lab6-27.png)

1. Add a subnet with the following settings (leave others with their default values) and click **Add (4)**.

    | Setting | Value |
    | --- | --- |
    | Name | **subnet-appgw (1)**|
    | Starting address | **10.60.3.224 (2)**|
    | Size | **/27(32 addresses) (3)**|

    ![image](../media/10-10-lab6-28.png)

    > **Note:** This subnet will be used by the Azure Application Gateway instances, which you will deploy later in this task. The Application Gateway requires a dedicated subnet of /27 or larger size.

1. In the Azure portal, search **Application Gateways (1)** and click on the **Application gateways (2)**.

    ![image](../media/10-10-lab6-29.png)

1. On the **Load balancing and content delivery | Application gateway** blade, click **+ Create (1)**. and select **Application Gateway (2)**

     ![image](../media/10-10-lab6-30.png)

1. On the **Basics** tab of the **Create an application gateway** blade, specify the following settings (leave others with their default values) and click **Next: Frontends > (12)**

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab **(1)** |
    | Resource group | az104-06-rg1 **(2)** |
    | Application gateway name | **az104-06-appgw5 (3)** |
    | Region | **<inject key="Region" enableCopy="false"/> (4)** |
    | Tier | **Standard V2 (5)** |
    | Enable autoscaling | **No (6)** |
    | Instance count | **2 (7)** |
    | IP address type | **IPv4 only (8)** |
    | HTTP2 | **Disabled (9)** |
    | Virtual network | **az104-06-vnet1 (10)** |
    | Subnet | **subnet-appgw (11)** |

     ![image](../media/up10-10-lab6-31.png)

1. On **Frontends** tab, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Frontend IP address type | **Public (1)** |
    | Public IP address | Select **Add new (2)** |
    
    ![image](../media/10-10-lab6-36.png)

1. On **Add a Public IP**, Specify the following settings(leave others with their default values) and click **Next: Backends > (5)**  

    | Setting | Value |
    | --- | --- |
    | Name | **az104-06-pip5 (3)** and click on **Ok (4)** |
    
    ![image](../media/10-10-lab6-33.png)

1. On **Backends**, click **Add a backend pool (1)** and on the **Add a backend pool** blade, specify the following settings (leave others with their default values) and click on **Add (6)**.

    | Setting | Value |
    | --- | --- |
    | Name | **az104-06-appgw5-be1 (2)** |
    | Add backend pool without targets | **No (3)** |
    | Target Type |  **Virtual machine** and choose **az104-06-nic1 (10.60.1.4) (4)** as Target |
    | Target Type |  **Virtual machine** and choose **az104-06-nic2 (10.60.2.4) (5)** as Target |

     ![image](../media/10-10-lab6-37.png)

    > **Note:** The targets represent the private IP addresses of virtual machines in the spoke virtual networks **az104-06-vm2** and **az104-06-vm3**.

1. Click **Add a backend pool (1)**. This is the backend pool for **images**. Specify the following settings (leave others with their default values). When completed click **Add (5)**.

    | Setting | Value |
    | --- | --- |
    | Name | `az104-imagebe` **(2)** |
    | Add backend pool without targets | **No (3)** |
    | Target Type | **Virtual machine** and choose **az104-06-nic1 (10.60.1.4) (4)** as Target|

     ![image](../media/10-10-lab6-38.png)

1. Click **Add a backend pool (1)**. This is the backend pool for **video**. Specify the following settings (leave others with their default values). When completed click **Add (5)**.

    | Setting | Value |
    | --- | --- |
    | Name | `az104-videobe` **(2)** |
    | Add backend pool without targets | **No (3)** |
    | Target Type | **Virtual machine** and choose **az104-06-nic2 (10.60.2.4) (4)** as Target|
   
     ![image](../media/10-10-lab6-39.png)

1. Click **Next: Configuration >** and, on the **Configuration** tab of the **Create an application gateway** blade, click **+ Add a routing rule**.

    ![image](../media/10-10-lab6-40.png)

    ![image](../media/10-10-lab6-41.png)

1. On the **Add a routing rule** blade, on the **Listener** tab, specify the following settings:

    | Setting | Value |
    | --- | --- |
    | Rule name | **az104-06-appgw5-rl1 (1)** |
    | Priority | **10 (2)** |
    | Listener name | **az104-06-appgw5-rl1l1 (3)** |
    | Frontend IP | **Public IPv4 (4)** |
    | Protocol | **HTTP (5)** |
    | Port | **80 (6)** |
    | Listener type | **Basic (7)** |

    ![image](../media/10-10-lab6-42.png)
   
1. Switch to the **Backend targets (1)** tab of the **Add a routing rule** blade and specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Target type | **Backend pool (2)** |
    | Backend target | **az104-06-appgw5-be1 (3)** |
    | Backend setting | Click **Add new (4)** |

    ![image](../media/10-10-lab6-43.png)
   
1. On the **Add Backend setting** blade, specify the following settings (leave others with their default values) and click **Add (6)**.

    | Setting | Value |
    | --- | --- |
    | Backend settings name | **az104-06-appgw5-http1 (1)** |
    | Backend protocol | **HTTP (2)** |
    | Backend port | **80 (3)** |
    | Cookie-based affinity | **Disable** |
    | Connection draining | **Disable** |
    |Dedicated backend connection | **Disable** |
    | Request time-out (seconds) | **20 (5)** |

    ![image](../media/10-10-lab6-44.png)
   
1. On the **Add a routing rule** blade. In the **Path based routing** section, select **Add multiple targets to create a path-based rule**. You will create two rules. Click **Add** after the first rule and then **Add** after the second rule. 

     ![image](../media/10-10-lab6-45.png)
   
    **Rule - routing to the images backend**

    | Setting | Value |
    | --- | --- |
    | Path | `/image/*` **(1)** |
    | Target name | `images` **(2)** |
    | Backend settings | **az104-06-appgw5-http1 (3)** |
    | Backend target | `az104-imagebe` **(4)** |

    ![image](../media/10-10-lab6-47.png)
   
    **Rule - routing to the videos backend**

    | Setting | Value |
    | --- | --- |
    | Path | `/video/*` **(1)** |
    | Target name | `videos` **(2)** |
    | Backend settings | **az104-06-appgw5-http1 (3)** |
    | Backend target | `az104-videobe` **(4)** |
      
    ![image](../media/10-10-lab6-49.png)

1. Click **Add** on the **Add routing rule** blade. and back on the **Configuration** blade.

    ![image](../media/10-10-lab6-50.png)

1. Click **Next: Tags >**, followed by **Next: Review + create >** and then click **Create**.

     ![image](../media/10-10-lab6-51.png)

     > **Note:** Wait for the Application Gateway instance to be created. This might take about 8 minutes.

1. In the Azure portal, search and select **Application Gateways** and, on the **Load balancing and content delivery | Application gateway** blade, click **az104-06-appgw5**.

    ![image](../media/10-10-lab6-52.png)

1. In the **Application Gateway** resource, in the **Monitoring (1)** sectionfrom the left navigation pane, select **Backend health (2)**.

1. Ensure the servers in the backend pool display **Healthy**.

     ![image](../media/10-10-lab6-53.png) 

1. On the **az104-06-appgw5** Application Gateway blade, note the value of the **Frontend public IP address**.

     ![image](../media/appfrntip.png)

1. Start another browser window and test this URL - `<frontend ip address>/image/` and verify you are directed to the image server (vm1).

   ![image](../media/10-10-lab6-54.png)

   >**Note:** Replace the frontend IP address with the IP address you copied in the previous step.

1. Start another browser window and test this URL - `<frontend ip address>/video/` and  verify you are directed to the video server (vm2).

   ![image](../media/10-10-lab6-55.png)

   >**Note:** Replace the frontend IP address with the IP address you copied in the previous step.

   > **Note:** You may need to refresh more than once or open a new browser window in InPrivate mode.

   > **Note:** Targeting virtual machines on multiple virtual networks is not a common configuration, but it is meant to illustrate the point that Application Gateway is capable of targeting virtual machines on multiple virtual networks (as well as endpoints in other Azure regions or even outside of Azure), unlike Azure Load Balancer, which load balances across virtual machines in the same virtual network.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com
. We are available 24/7 to help

<validation step="c5b0893d-fb8a-40ab-9566-65332fc9b45e" />

### Review

In this lab, you have completed the following:

- Used an Azure Resource Manager (ARM) template to automate the provisioning of the infrastructure.
- Configured an Azure Load Balancer to distribute incoming traffic across multiple resources for high availability and reliability.
- Set up an Azure Application Gateway to enable advanced traffic routing, SSL termination, and web application firewall protection.


## Extend your learning with Copilot

Copilot can assist you in learning how to use the Azure scripting tools. Copilot can also assist in areas not covered in the lab or where you need more information. Open an Edge browser and choose Copilot (top right) or navigate to *copilot.microsoft.com*. Take a few minutes to try these prompts.

+ Compare and contrast the Azure Load Balancer with the Azure Application Gateway. Help me decide in which scenarios I should use each product. 
+ What tools are available to troubleshoot connections to an Azure Load Balancer? 
+ What are the basic steps for configuring the Azure Application Gateway? Provide a high-level checklist. 
+ Create a table highlighting three Azure load balancing solutions. For each solution show supported protocols, routing policies, session affinity, and TLS offloading.

## Learn more with self-paced training

+ [Improve application scalability and resiliency by using Azure Load Balancer](https://learn.microsoft.com/training/modules/improve-app-scalability-resiliency-with-load-balancer/). Discuss the different load balancers in Azure and how to choose the right Azure load balancer solution to meet your requirements.
+ [Load balance your web service traffic with Application Gateway](https://learn.microsoft.com/training/modules/load-balance-web-traffic-with-application-gateway/). Improve application resilience by distributing load across multiple servers and use path-based routing to direct web traffic.

## Key takeaways

Congratulations on completing the lab. Here are the key points for this lab.

+ Azure Load Balancer is an excellent choice for distributing network traffic across multiple virtual machines at the transport layer (OSI layer 4 - TCP and UDP).
+ Public Load Balancers are used to load balance internet traffic to your VMs. An internal (or private) load balancer is used where private IPs are needed at the frontend only.
+ The Basic load balancer is for small-scale applications that don't need high availability or redundancy. The Standard load balancer is for high performance and ultra-low latency.
+ Azure Application Gateway is a web traffic (OSI layer 7) load balancer that enables you to manage traffic to your web applications.
+ The Application Gateway Standard tier offers all the L7 functionality, including load balancing, The WAF tier adds a firewall to check for malicious traffic.
+ An Application Gateway can make routing decisions based on additional attributes of an HTTP request, for example URI path or host headers.

### You have successfully completed the lab
