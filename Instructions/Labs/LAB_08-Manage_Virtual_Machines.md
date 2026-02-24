# Lab - Manage Virtual Machines

## Lab Overview

This lab focuses on deploying scalable and high-availability applications in Azure using Virtual Machines (VMs) and Virtual Machine Scale Sets (VMSS).

## Lab objectives
In this lab, you will complete the following tasks:

+ Task 1: Deploy zone-resilient Azure virtual machines by using the Azure portal.
+ Task 2: Manage compute and storage scaling for virtual machines.
+ Task 3: Create and configure Azure Virtual Machine Scale Sets.
+ Task 4: Scale Azure Virtual Machine Scale Sets.
+ Task 5: Create a virtual machine using Azure PowerShell (optional 1).
+ Task 6: Create a virtual machine using the CLI (optional 2).


### Task 1: Deploy zone-resilient Azure virtual machines by using the Azure portal 

In this task, you will deploy two Azure virtual machines into different availability zones by using the Azure portal. Availability zones offer the highest level of uptime SLA for virtual machines at 99.99%. To achieve this SLA, you must deploy at least two virtual machines across different availability zones.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual machines (1)**, and then select **Virtual machines (2)** under services.

      ![Image](../Labs/media/r26.png)

1. On the **Compute infrastructure | Virtual machines** blade, click **+ Create (1)**, and then select in the drop-down **Virtual machine**. Notice your other choices.

    ![image](../media/13-10-lab8-2.png)

1. On the **Basics** tab, in the **Availability zone** drop down menu, place a checkmark next to **Zone 2**. This should select both **Zone 1** and **Zone 2**.

    ![image](../media/13-10-lab8-3.png)

   >**Note:** This will deploy two virtual machines in the selected region, one in each zone. You achieve the 99.99% uptime SLA because you have at least two VMs 
    distributed across at least two zones. In the scenario where you might only need one VM, it is a best practice to still deploy the VM to another zone.
     
1. On the **Basics** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab  **(1)**|
    | Resource group | select the existing resource group **az104-08-rg01 (2)** |
    | Virtual machine name |  `az104-vm1` and `az104-vm2` (After selecting both availability zones, select **Edit names** under the VM name field.) **(3)** |
    | Region | **<inject key="Region" enableCopy="false" /> (4)** |
    | Availability options | **Availability zone (5)**|
    | Availability zone | **Zone 1, 2** (read the note about using virtual machine scale sets)   |
    | Security type | **Standard (6)** |

     ![image](./media/mv4.png)

    - Image: Select **See all images** to choose `Windows Server 2025 Datacenter - x64 Gen2`

      ![image](./media/mv1.png)

    - Select **Windows Server** dropdown

      ![image](./media/mv2.png)    

    - Select **Windows Server 2025 Datacenter - x64 Gen2**  

      ![image](../Labs/media/r27.png)   

      ![image](../Labs/media/r28.png)        

    | Setting | Value |
    | --- | --- |
    | Run Azure Spot discount | **Unchecked (8)**|
    | Size | **Standard D2s v3 (9)**|
    | Username | **Student (10)** |
    | Password | **Password.1!! (11)** |
    | Public inbound ports | **None (12)** |
    | Would you like to use an existing Windows Server license? | **Unchecked  (13)**|

     ![image](../media/13-10-lab8-5.png)

1. Click **Next: Disks > (14)** and, on the **Disks** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values) and then click on **Next: Networking > (4)**:

    | Setting | Value |
    | --- | --- |
    | OS disk type | **Premium SSD (1)** |
    | Delete with VM | **checked (2)** (default) |
    | Enable Ultra Disk compatibility | **Unchecked (3)** |

    ![image](../media/13-10-lab8-6.png)

1. On the **Networking** take the defaults but do not provide a load balancer.

    | Setting | Value |
    | --- | --- |
    | Delete public IP and NIC when VM is deleted | **Checked (1)** |
    | Load balancing options | **None (2)** |
    
    - Click **Next: Management > (3)**

     ![image](../media/13-10-lab8-7.png)

1. On the **Management** tab, review the settings. Do not make any changes and then click **Next: Monitoring >**.

1. Specify the following settings (leave others with their default values) and then click on **Next: Advanced > (2)**:

    | Setting | Value |
    | --- | --- |
    | Boot diagnostics | **Disable (1)** |

     ![image](../media/13-10-lab8-9.png)

1. On the **Advanced** tab, take the defaults, then click **Review + Create**.

     ![image](../media/13-10-lab8-10.png)

1. After the validation, click **Create**.

     ![](../Labs/media/r29.png)

    >**Note:** Notice as the virtual machine deploys the NIC, disk, and public IP address (if configured) are independently created and managed resources.

1. Wait for the deployment to complete, then select **Go to resource**.

     ![image](../media/13-10-lab8-12.png)

     >**Note:** Monitor the **Notification** messages.
   
> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com
. We are available 24/7 to help

<validation step="2d5deae0-7c9b-46ac-8594-89a6ad9f51f8" />

## Task 2: Manage compute and storage scaling for virtual machines

In this task, you will scale a virtual machine by adjusting its size to a different SKU. Azure provides flexibility in VM size selection so that you can adjust a VM for periods of time if it needs more (or less) compute and memory allocated. This concept is extended to disks, where you can modify the performance of the disk, or increase the allocated capacity.

1. On the **az104-vm1** virtual machine, in the **Availability + scale (1)** blade, select **Size (2)**.Set the virtual machine size to **DS1_v2 (3)** and click **Resize (4)**. 

     ![image](../media/resize.png)

     >**Note:** Choose another size if **Standard DS1_v2** is not available. Resizing is also known as vertical , up or down.

1. When prompted, confirm the change by clicking on **Resize**.

1. After 2–3 minutes, refresh the portal to reflect the updated VM size.

1. In the **Settings** area, select **Disks**.

1. Under **Data disks** select **+ Create and attach a new disk (1)**. Configure the settings (leave other settings at their default values).

    | Setting | Value |
    | --- | --- |
    | Disk name | `vm1-disk1` **(2)**|
    | Storage type | **Standard HDD (3)** |
    | Size (GiB) | `32` **(4)** |
    |  Click **Apply (5)** |

     ![](../Labs/Images/l8i5.png)

1. After the disk has been created, click **Detach** (if necessary, scroll to the right to view the detach icon), and then click **Apply**.

   ![](../Labs/Images/l8i6.png)

     >**Note:** Detaching removes the disk from the VM but keeps it in storage for later use.

1. Wait for the virtual machine update to complete before proceeding, as you will only be able to change the storage type in the upcoming steps once the update finishes.
     
     ![](../Labs/media/r30.png)

1. In the azure portal, search for and select `Disks`. From the list of disks, select the **vm1-disk1** object.

    >**Note:** The **Overview** blade also provides performance and usage information for the disk.

1. From the left navigation pane, Under the **Settings (1)** blade, select **Size + performance (2)**.

1. Set the storage type to **Standard SSD (3)**, and then click **Save (4)**.

   ![image](../media/13-10-lab8-14.png)

1. Navigate back to the **az104-vm1** virtual machine and select **Disks (1)**.

1. In the **Data disk** section, select **Attach existing disks (2)**. and in the **Disk name** drop-down, select **VM1-DISK1 (3)**.

    ![image](../media/13-10-lab8-15.png)

1. Verify the disk is now **Standard SSD**.

1. Select **Apply (4)** to save your changes. 

    >**Note:** You have now created a virtual machine, scaled the SKU and the data disk size. In the next task we use Virtual Machine Scale Sets to automate the scaling process.

## Azure Virtual Machine Scale Sets Architecture Diagram

   ![](../Labs/Images/az104-lab08-vmss-architecture.png)

## Task 3: Create and configure Azure Virtual Machine Scale Sets

In this task, you will deploy an Azure virtual machine scale set across availability zones. VM Scale Sets reduce the administrative overhead of automation by enabling you to configure metrics or conditions that allow the scale set to horizontally scale, scale in or scale out.

1. In the Azure portal, search for `Virtual machine scale sets` **(1)**  and select Virtual machine scale sets **(2)** from results.

     ![image](../media/13-10-lab8-16.png)

1. On the **Compute infrastructure | Virtual Machine Scale Sets (VMSS)** blade, click **+ Create**.

    ![image](../media/13-10-lab8-17.png)

1. On the **Basics** tab of the **Create a virtual machine scale set** blade, specify the following settings (leave others with their default values) and click **Next: Spot (16) >:**

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of your Azure subscription **(1)**  |
    | Resource group | **az104-08-rg01 (2)**  |
    | Virtual machine scale set name | **vmss1 (3)** |
    | Region | **<inject key="Region" enableCopy="false" /> (4)** |
    | Availability zone | **Zones 1, 2, 3 (5)** |
    | Orchestration mode | **Uniform (6)** |
    | Security type | **Standard (7)** |
    | Scaling mode | **Manually update the capacity (8)** |
    | Instance count | **2 (9)** |
    | Image | **Windows Server 2025 Datacenter - x64 Gen2 (10)** |
    | Size | **Standard D2s_v3 (11)** |
    | Username | **Student (12)** |
    | Password | **Provide a secure password (13)**  |
    | Confirm Password | **Provide th password again to confirm (14)**  |    
    | Already have a Windows Server license? | **Unchecked (15)** |

    ![image](../media/up13-10-lab8-18-new.png)

    ![image](../Labs/media/r31.png)
    ![image](../Labs/media/r32.png)    

    >**Note:** For the list of Azure regions which support deployment of Windows virtual machines to availability zones, refer to [What are Availability Zones in Azure?](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

1. On the **Spot (15)** tab, accept the defaults and select **Next: Disks >**.

      ![image](../media/spotdflt.png)      

1. On the **Disks** tab, accept the default values and click **Next : Networking >**.

      ![image](../media/diskdflt.png)

1. On the **Networking** page, click the **Edit virtual network** link below the **Virtual network** textbox and create a new virtual network with the following settings (leave others with their default values).

    | Setting | Value |
    | --- | --- |
    | Name | **vmss-vnet (1)** |
    | Address range | `10.82.0.0/20` (delete the existing address range) **(2)** |
    | Under subnets click on **Edit (3)** |

    ![image](../media/up13-10-lab8-20.png) 

1. Provide the below details and click on **Save** twice **(3)** 

    | Setting | Value |
    | --- | --- |
    | Subnet name | `subnet0` **(1)** |
    | Subnet range | `10.82.0.0/24` **(2)** |
    
     ![image](../media/13-10-lab8-21.png)

     ![image](../media/up13-10-lab8-22.png)

1. In the **Networking** tab, click the **Edit network interface** icon to the right of the network interface entry.

   ![image](../media/13-10-lab8-23.png)

1. For **NIC network security group** section, select **Advanced (1)** and then click **Create new (2)** under the **Configure network security group** drop-down list.

     ![image](../media/13-10-lab8-24.png)

1. On the **Create network security group** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **vmss1-nsg (1)** |

     ![image](../media/13-10-lab8-25.png)

1. Click **+ Add an inbound rule (2)** and add an inbound security rule with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Source | **Any (1)** |
    | Source port ranges | * **(2)** |
    | Destination | **Any (3)** |
    | Service | **HTTP (4)** |
    | Action | **Allow (5)** |
    | Priority | **1010 (6)** |
    | Name | `allow-http` **(7)** |

     ![image](../media/13-10-lab8-26.png)

1. Click **Add (8)** and, back on the **Create network security group** blade, click **OK**.

    ![image](../media/13-10-lab8-27.png)

1. In the **Edit network interface** blade, in the **Public IP address** section, click **Enabled (1)** and click **OK (2)**.

    ![image](../media/13-10-lab8-28.png)

1. In the **Networking** tab, under the **Load balancing** section, specify the following (leave others with their default values).

    | Setting | Value |
    | --- | --- |
    | Load balancing options | **Azure load balancer (1)** |
    | Select a load balancer | **Create a load balancer (2)** |

     ![image](../media/13-10-lab8-29.png)

1. On the **Create a load balancer** page, specify the load balancer name and take the defaults. Click **Create (2)** when you are done click **Next** and Next again to go to the **Management** tab.

    | Setting | Value |
    | --- | --- |
    | Load balancer name | `vmss-lb` **(1)** |

     ![image](../media/13-10-lab8-30.png)

     >**Note:** Pause for a minute and review what you done. At this point, you have configured the virtual machine scale set with disks and networking. In the network configuration you have created a network security group and allowed HTTP. You have also created a load balancer with a public IP address.

1. On the **Management** tab, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Boot diagnostics | **Disable (1)** |

     ![image](../media/13-10-lab8-31.png)

1. Click **Next : Health > (2)**.

1. On the **Health** tab, review the default settings without making any changes and click **Next : Advanced >**.

1. On the **Advanced** tab, click **Review + create**.

     ![image](../media/13-10-lab8-32.png)

1. On the **Review + create** tab, ensure that the validation passed and click **Create**.

     ![image](../media/createvmss.png)

     >**Note:** Wait for the virtual machine scale set deployment to complete. This should take approximately 5 minutes. While you wait review the [documentation](https://learn.microsoft.com/azure/virtual-machine-scale-sets/overview).

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help

<validation step="bd5e6aae-4787-489b-9bce-6aadf5bcfe0d" />

## Task 4: Scale Azure Virtual Machine Scale Sets

In this task, you will scale the Virtual Machine (VM) Scale Set in Azure using a custom scale rule to ensure optimal performance and resource utilization based on specific criteria. A VM Scale Set allows you to manage a group of identical, load-balanced virtual machines that automatically scale in or out depending on the demand.

1. Select **Go to resource** or search for and select the **vmss1** scale set.

     ![image](../media/13-10-lab8-34.png)

1. Choose **Availability + Scale (1)** from the left side menu, then choose **Scaling (2)**.

     ![image](../media/13-10-lab8-35.png)

     >**Did you know?** You can **Manual scale** or **Custom autoscale**. In scale sets with a small number of VM instances, increasing or decreasing the instance count (Manual scale) may be best. In scale sets with a large number of VM instances, scaling based on metrics (Custom autoscale) may be more appropriate.

 ### Scale-out rule

1. Select **Custom autoscale (1)**. then change the **Scale mode** to **Scale based on metric (2)**. And then select **Add rule (3)**.

    ![image](../media/13-10-lab8-36.png) 

1. Let's create a rule that automatically increases the number of VM instances. This rule scales out when the average CPU load is greater than 70% over a 10-minute period. When the rule triggers, the number of VM instances is increased by 50%.

    | Setting | Value |
    | --- | --- |
    | Metric source | **Current resource (vmss1) (1)** |
    | Metric namespace | **Virtual Machine Host (2)** |
    | Metric name | **Percentage CPU (3)** (review your other choices) |

     ![image](../media/13-10-lab8-37.png)

    | Setting | Value |
    | --- | --- |
    | Operator | **Greater than (1)** |
    | Metric threshold to trigger scale action | **70 (2)** |
    | Duration (minutes) | **10 (3)** |
    | Time grain statistic | **Average (4)** |
    | Operation | **Increase percent by** (review other choices) **(5)** |
    | Cool down (minutes) | **5 (6)** |
    | Percentage | **50 (7)** |

    - Click on **Add (8)** to save the rule.

      ![image](../media/13-10-lab8-37.1.png)



### Scale in rule

1. During evenings or weekends, demand may decrease so it is important to create a scale in rule.

1. Let's create a rule that decreases the number of VM instances in a scale set. The number of instances should decrease when the average CPU load drops below 30% over a 10-minute period. When the rule triggers, the number of VM instances is decreased by 20%.

1. Select **+ Add a rule**.

    ![image](../Labs/media/r33.png)

1. Adjust the settings, then select **Add (5)**.

    | Setting | Value |
    | --- | --- |
    | Operator | **Less than (1)** |
    | Threshold | **30 (2)** |
    | Operation | **Decrease percentage by (review your other choices) (3)** |
    | Percentage | **20 (4)** |

    - Click on **Add (5)** to save the rule.

      ![image](../media/13-10-lab8-39.png) 

### Set the instance limits

1. When your autoscale rules are applied, instance limits make sure that you do not scale out beyond the maximum number of instances or scale in beyond the minimum number of instances.

1. **Instance limits** are shown on the **Scaling** page after the rules.

    | Setting | Value |
    | --- | --- |
    | Minimum | **2 (1)** |
    | Maximum | **10 (1)** |
    | Default | **2 (1)** |

1. Be sure to **Save (2)** your changes

     ![image](../media/13-10-lab8-40.png)

1. On the **vmss1** page, select **Instances**. This is where you would monitor the number of virtual machine instances.

    ![image](../Labs/media/r34.png)

    >**Note:** If you are interested in using Azure PowerShell for virtual machine creation, try Task 5. If you are interested in using the CLI to create virtual machines, try Task 6.

## Task 5: Create a virtual machine using Azure PowerShell (option 1)

1. On the Azure portal, select the **Cloud shell** (**[>_]**) **(1)** button at the top of the page to the right of the search box. This opens a cloud shell pane at the bottom of the portal.

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (*Bash* or *PowerShell*). If so, select **PowerShell (2)**.

      ![image](../media/7-10-lab3-25.png)

1. In the **Getting started** window, select **Mount storage account (1)**, choose the **subscription (2)** from the dropdown, and click **Apply (3)** to continue.

     ![image](../media/7-10-lab3-26.png)

1. On mount storage account page, select **I want to create a storage account (1)**, click on **Next (2)**.

    ![image](../media/7-10-lab3-27.png)

    >**Note:** As you work with the Cloud Shell a storage account and file share is required. 

1. Specify the following then click on **Create**.
   
    | Settings | Values |
    |  -- | -- |
    | Subscription | Accept default **(1)**|
    | Resource Group | **az104-08-rg01 (2)** |
    | Region         | **<inject key="Region" enableCopy="false"/> (3)** |
    | Storage account name | **str<inject key="DeploymentID" enableCopy="false" /> (4)** |
    | File share  | **none (5)** |

     ![image](../media/13-10-lab8-41.png)

1. Run the following command to create a virtual machine. When prompted, provide a username and password for the VM. While you wait check out the [New-AzVM](https://learn.microsoft.com/powershell/module/az.compute/new-azvm?view=azps-11.1.0) command reference for all the parameters associated with creating a virtual machine.

    ```powershell
    New-AzVm `
    -ResourceGroupName 'az104-08-rg01' `
    -Name 'myPSVM' `
    -Location 'East US' `
    -Image 'Win2019Datacenter' `
    -Zone '1' `
    -Size 'Standard_D2s_v3' ` 
    ```

    ![image](../media/13-10-lab8-42.png)

    >**Note:** When prompted, please provide a Username as **Student** and Password to create the new VM.

1. Once the command completes, use **Get-AzVM** to list the virtual machines in your resource group.

    ```powershell
    Get-AzVM `
    -ResourceGroupName 'az104-08-rg01' `
    -Status
    ```

     ![image](../media/13-10-lab8-43.png)

1. Verify your new virtual machine is listed and the **Status** is **Running**.

1. Use **Stop-AzVM** to deallocate your virtual machine. Type **Yes** to confirm.

    ```powershell
    Stop-AzVM `
    -ResourceGroupName 'az104-08-rg01' `
    -Name 'myPSVM' 
    ```

    ![image](../media/13-10-lab8-44.png)

1. Use **`Get-AzVM -Status`**  to verify the machine is **deallocated**.

     ![image](../media/13-10-lab8-46.png)

    >**Did you know?** When you use Azure to stop your virtual machine, the status is *deallocated*. This means that any non-static public IPs are released, and you stop paying for the VM’s compute costs.

## Task 6: Create a virtual machine using the CLI (option 2)

1. Select **Switch to Bash** to switch to bash terminal.

    ![image](../Labs/media/r37.png)

1. Select **Confirm**.

    ![image](../Labs/media/r38.png)

1. Run the following command to create a virtual machine. When prompted, provide a username and password for the VM. While you wait check out the [az vm create](https://learn.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) command reference for all the parameters associated with creating a virtual machine.

    ```sh
    az vm create --name myCLIVM --resource-group az104-08-rg01 --image Win2019Datacenter --admin-username localadmin --generate-ssh-keys
    ```

     ![image](../media/13-10-lab8-47-new.png)

     >**Note:** Give Admin password as  **Password.1!!** and Password will be not visible
   
1. Once the command completes, use **az vm show** to verify your machine was created.

    ```sh
    az vm show --name  myCLIVM --resource-group az104-08-rg01 --show-details
    ```

     ![image](../media/13-10-lab8-48.png)

1. Verify the **powerState** is **VM Running**.

1. Use **az vm deallocate** to deallocate your virtual machine.

    ```sh
   az vm deallocate --resource-group az104-08-rg01 --name myCLIVM
    ```

1. Use **az vm show** to ensure the **powerState** is **VM deallocated**.

    ```sh
    az vm show --name  myCLIVM --resource-group az104-08-rg01 --show-details
    ```

    ![image](../Labs/media/r40.png)    

  >**Did you know?** When you use Azure to stop your virtual machine, the status is *deallocated*. This means that any non-static public IPs are released, and you stop paying for the VM’s compute costs.

### Review

In this lab, you have completed the following:

- Deployed zone-resilient Azure virtual machines using the Azure portal, ensuring high availability across different availability zones.
- Managed the scaling of compute and storage resources for virtual machines to meet performance and capacity requirements.
- Created and configured Azure Virtual Machine Scale Sets to automatically manage and scale a group of identical virtual machines based on demand.
- Scaled Azure Virtual Machine Scale Sets to adjust the number of instances according to changing workload demands, ensuring optimal resource utilization.
- Created a virtual machine using Azure PowerShell (optional), gaining hands-on experience with automation and scripting for VM provisioning.
- Created a virtual machine using the Azure CLI (optional), leveraging command-line tools to streamline the VM deployment process.


## Extend your learning with Copilot
Copilot can assist you in learning how to use the Azure scripting tools. Copilot can also assist in areas not covered in the lab or where you need more information. Open an Edge browser and choose Copilot (top right) or navigate to *copilot.microsoft.com*. Take a few minutes to try these prompts.

+ Provide the steps and the Azure CLI commands to create a Linux virtual machine. 
+ Review the ways you can scale virtual machines and improve performance.
+ Describe Azure storage lifecycle management policies and how they can optimize costs.

## Learn more with self-paced training

+ [Create a Windows virtual machine in Azure](https://learn.microsoft.com/training/modules/create-windows-virtual-machine-in-azure/). Create a Windows virtual machine using the Azure portal. Connect to a running Windows virtual machine using Remote Desktop
+ [Build a scalable application with Virtual Machine Scale Sets](https://learn.microsoft.com/training/modules/build-app-with-scale-sets/). Enable your application to automatically adjust to changes in load while minimizing costs with Virtual Machine Scale Sets.
+ [Connect to virtual machines through the Azure portal by using Azure Bastion](https://learn.microsoft.com/en-us/training/modules/connect-vm-with-azure-bastion/). Deploy Azure Bastion to securely connect to Azure virtual machines directly within the Azure portal to effectively replace an existing jumpbox solution, monitor remote sessions by using diagnostic logs, and manage remote sessions by disconnecting a user session.

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab.

+ Azure virtual machines are on-demand, scalable computing resources.
+ Azure virtual machines provide both vertical and horizontal scaling options.
+ Configuring Azure virtual machines includes choosing an operating system, size, storage and networking settings.
+ Azure Virtual Machine Scale Sets let you create and manage a group of load balanced VMs.
+ The virtual machines in a Virtual Machine Scale Set are created from the same image and configuration.
+ In a Virtual Machine Scale Set the number of VM instances can automatically increase or decrease in response to demand or a defined schedule.

### You have successfully completed the lab
