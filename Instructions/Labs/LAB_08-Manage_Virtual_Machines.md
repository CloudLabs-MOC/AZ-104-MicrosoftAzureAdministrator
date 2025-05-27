# Lab 01 - Manage Virtual Machines

## Lab Overview

This lab focuses on deploying scalable and high-availability applications in Azure using Virtual Machines (VMs) and Virtual Machine Scale Sets (VMSS).

## Lab objectives
In this lab, you will complete the following tasks:

+ Task 1: Deploy zone-resilient Azure virtual machines by using the Azure portal.
+ Task 2: Manage compute and storage scaling for virtual machines.
+ Task 3: Create and configure Azure Virtual Machine Scale Sets.
+ Task 4: Scale Azure Virtual Machine Scale Sets.

## Exercise 1: Manage Virtual Machines

In this exercise, you will manage Virtual Machines in Azure to deploy, configure, and maintain scalable and secure cloud-based infrastructure.

### Task 1: Deploy zone-resilient Azure virtual machines by using the Azure portal 

In this task, you will deploy two Azure virtual machines into different availability zones by using the Azure portal. Availability zones offer the highest level of uptime SLA for virtual machines at 99.99%. To achieve this SLA, you must deploy at least two virtual machines across different availability zones.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Virtual machines**, and then select **Virtual machines** under services.

   ![](../Labs/Images/pax8-image1.png)
   
1. Click **+ Create**, and then select in the drop-down **+ Azure virtual machine**. Notice your other choices.

   ![](../Labs/Images/pax8-image2.png)
   
1. On the **Basics** tab, in the **Availability zone** drop down menu, place a checkmark next to **Zone 2**. This should select both **Zone 1** and **Zone 2**.

    ![](../Labs/Images/l8i1.png)

   >**Note**: This will deploy two virtual machines in the selected region, one in each zone. You achieve the 99.99% uptime SLA because you have at least two VMs 
    distributed across at least two zones. In the scenario where you might only need one VM, it is a best practice to still deploy the VM to another zone.
     
1. On the **Basics** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you will be using in this lab  (1)|
    | Resource group | select the existing resource group **az104-08-rg01** (2) |
    | Virtual machine name |  `az104-vm1` and `az104-vm2` (After selecting both availability zones, select **Edit names** under the VM name field.) (3) |
    | Region | **<inject key="Region" enableCopy="false" />** (4) |
    | Availability options | **Availability zone** (5)|
    | Availability zone | **Zone 1, 2** (read the note about using virtual machine scale sets)   |
    | Security type | **Standard** (6) |
    | Image | **Windows Server 2019 Datacenter - Gen2**  (7)|
    | Run Azure Spot discount | **Unchecked** (8)|
    | Size | **Standard D2s v3** (9)|
    | Username | **Student** (10) |
    | Password | **Password.1!!** (11) |
    | Public inbound ports | **None** (12) |
    | Would you like to use an existing Windows Server license? | **Unchecked**  (13)|

     ![](../Labs/Images/l8i2.png)

     ![](../Labs/Images/l8i3.png)

1. Click **Next: Disks >** and, on the **Disks** tab of the **Create a virtual machine** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | OS disk type | **Premium SSD (1)** |
    | Delete with VM | **checked** (default) **(2)** |
    | Enable Ultra Disk compatibility | **Unchecked (3)** |

    ![](../Labs/Images/pax8-image3.png)

1.  Click **Next: Networking >** take the defaults but do not provide a load balancer.

    | Setting | Value |
    | --- | --- |
    | Delete public IP and NIC when VM is deleted | **Checked (1)** |
    | Load balancing options | **None (2)** |

    ![](../Labs/Images/pax8-image4.png)
    
1. Click **Next: Management >** and specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Patch orchestration options | **Azure orchestrated** |  

     ![](../Labs/Images/pax8-image5.png)

1. Click **Next: Monitoring >** and specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Boot diagnostics | **Disable** |

1. Click **Next: Advanced >**, take the defaults, then click **Review + Create**.

1. After the validation, click **Create**.

    >**Note:** Notice as the virtual machine deploys the NIC, disk, and public IP address (if configured) are independently created and managed resources.

1. Wait for the deployment to complete, then select **Go to resource**.

   >**Note:** Monitor the **Notification** messages.
   
> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help

<validation step="2d5deae0-7c9b-46ac-8594-89a6ad9f51f8" />

## Task 2: Manage compute and storage scaling for virtual machines

In this task, you will scale a virtual machine by adjusting its size to a different SKU. Azure provides flexibility in VM size selection so that you can adjust a VM for periods of time if it needs more (or less) compute and memory allocated. This concept is extended to disks, where you can modify the performance of the disk, or increase the allocated capacity.

1. On the **az104-vm1** virtual machine, in the **Availability + scale** blade, select **Size** (1).Set the virtual machine size to **DS1_v2** (2) and click **Resize** (3). When prompted, confirm the change.

   ![](../Labs/Images/pax8-image6.png)

    >**Note**: Choose another size if **Standard DS1_v2** is not available. Resizing is also known as vertical , up or down.

1. In the **Settings** area, select **Disks**.

1. Under **Data disks** select **+ Create and attach a new disk** (1). Configure the settings (leave other settings at their default values).

    | Setting | Value |
    | --- | --- |
    | Disk name | `vm1-disk1` (2)|
    | Storage type | **Standard HDD** (3) |
    | Size (GiB) | `32`(4) |
    |  Click **Apply** (5) |

     ![](../Labs/Images/l8i5.png)

    >**Note** : If another row is added while performing the above step, please delete it by selecting the **Detach** icon shown in the screenshot below.

      ![](../Labs/Images/pax8-image11.png)
   
1. After the disk has been created, click **Detach** (if necessary, scroll to the right to view the detach icon), and then click **Apply**.

   ![](../Labs/Images/l8i6.png)

     >**Note**: Detaching removes the disk from the VM but keeps it in storage for later use.

1. In the azure portal, search for and select **Disks**.

     ![](../Labs/Images/pax8-image8.png)
   
1. From the list of disks, select the **vm1-disk1** object.

    ![](../Labs/Images/pax8-image9.png)
   
    >**Note:** The **Overview** blade also provides performance and usage information for the disk.

1. From the left navigation pane, Under the **Settings (1)** blade, select **Size + performance (2)**. Set the storage type to **Standard SSD (3)**, and then click **Save (4)**.

   ![](../Labs/Images/pax8-image10.png)

1. In the azure portal, search for and select **Virtual machines** then select **az104-vm1** virtual machine from the list.

1. In the **Data disk** section, select **Attach existing disks**. and in the **Disk name** drop-down, select **VM1-DISK1**.

    ![](../Labs/Images/l8i8.png)

1. Verify the disk is now **Standard SSD**.

1. Select **Apply** to save your changes. 

    >**Note:** You have now created a virtual machine, scaled the SKU and the data disk size. In the next task we use Virtual Machine Scale Sets to automate the scaling process.

## Azure Virtual Machine Scale Sets Architecture Diagram

   ![](../Labs/Images/az104-lab08-vmss-architecture.png)

## Task 3: Create and configure Azure Virtual Machine Scale Sets

In this task, you will deploy an Azure virtual machine scale set across availability zones. VM Scale Sets reduce the administrative overhead of automation by enabling you to configure metrics or conditions that allow the scale set to horizontally scale, scale in or scale out.

1. In the Azure portal, search for and select `Virtual machine scale sets` and, on the **Virtual machine scale sets** blade, click **+ Create**.

1. On the **Basics** tab of the **Create a virtual machine scale set** blade, specify the following settings (leave others with their default values) and click **Next : Spot >**:

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of your Azure subscription (1)  |
    | Resource group | **az104-08-rg01** (2)  |
    | Virtual machine scale set name | **vmss1** (3) |
    | Region | **<inject key="Region" enableCopy="false" />** (4) |
    | Availability zone | **Zones 1, 2, 3** (5) |
    | Orchestration mode | **Uniform** (6) |
    | Security type | **Standard** (7) |
    | Image | **Windows Server 2019 Datacenter - x64 Gen2** (8) |
    | Size | **Standard D2s_v3** (9) |
    | Username | **Student** (10) |
    | Password | **Provide a secure password** (11)  (12)|
    | Already have a Windows Server license? | **Unchecked** (13) |

    >**Note**: For the list of Azure regions which support deployment of Windows virtual machines to availability zones, refer to [What are Availability Zones in Azure?](https://docs.microsoft.com/en-us/azure/availability-zones/az-overview)

    ![](../Labs/Images/pax8-image14.png)

    ![](../Labs/Images/pax8-image15.png)

    ![](../Labs/Images/pax8-image16.png)

1. On the **Spot** tab, accept the defaults and select **Next: Disks >**.

1. On the **Disks** tab, accept the default values and click **Next : Networking >**.

1. On the **Networking** page, click the **Edit virtual network** link below the **Virtual network** textbox and create a new virtual network with the following settings (leave others with their default values).

    | Setting | Value |
    | --- | --- |
    | Name | **vmss-vnet** (1) |
    | Address range | `10.82.0.0/20` (delete the existing address range) (2) |

    ![](../Labs/Images/az104xxx3.png) 

1. Under subnets click on **Edit**

    | Subnet name | `subnet0` (1) |
    | Subnet range | `10.82.0.0/24` (2) |
    | Click on **Save** twice (3) |
    
    ![](../Labs/Images/az104xxx2.png) 

1. In the **Networking** tab, click the **Edit network interface** icon to the right of the network interface entry.

   ![](../Labs/Images/l8i12.png)

1. For **NIC network security group** section, select **Advanced** and then click **Create new** under the **Configure network security group** drop-down list.

1. On the **Create network security group** blade, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Name | **vmss1-nsg** |

1. Click **Add an inbound rule** and add an inbound security rule with the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Source | **Any** |
    | Source port ranges | * |
    | Destination | **Any** |
    | Service | **HTTP** |
    | Action | **Allow** |
    | Priority | **1010** |
    | Name | `allow-http` |

    ![](../Labs/Images/pax8-image17.png)

1. Click **Add** and, back on the **Create network security group** blade, click **OK**.

1. In the **Edit network interface** blade, in the **Public IP address** section, click **Enabled** and click **OK**.

    ![](../Labs/Images/l8i13.png)

1. In the **Networking** tab, under the **Load balancing** section, specify the following (leave others with their default values).

    | Setting | Value |
    | --- | --- |
    | Load balancing options | **Azure load balancer** |
    | Select a load balancer | **Create a load balancer** |

1. On the **Create a load balancer** page, specify the load balancer name and take the defaults. Click **Create** when you are done click **Next** and Next again to go to the **Management** tab.

    | Setting | Value |
    | --- | --- |
    | Load balancer name | `vmss-lb` |

     ![](../Labs/Images/pax8-image20.png)
   
    >**Note:** Pause for a minute and review what you done. At this point, you have configured the virtual machine scale set with disks and networking. In the network configuration you have created a network security group and allowed HTTP. You have also created a load balancer with a public IP address.

1. On the **Management** tab, specify the following settings (leave others with their default values):

    | Setting | Value |
    | --- | --- |
    | Boot diagnostics | **Disable** |

1. Click **Next : Health >**.

1. On the **Health** tab, review the default settings without making any changes and click **Next : Advanced >**.

1. On the **Advanced** tab, click **Review + create**.

1. On the **Review + create** tab, ensure that the validation passed and click **Create**.

    >**Note**: Wait for the virtual machine scale set deployment to complete. This should take approximately 5 minutes. While you wait review the [documentation](https://learn.microsoft.com/azure/virtual-machine-scale-sets/overview).

## Task 4: Scale Azure Virtual Machine Scale Sets

In this task, you will scale the Virtual Machine (VM) Scale Set in Azure using a custom scale rule to ensure optimal performance and resource utilization based on specific criteria. A VM Scale Set allows you to manage a group of identical, load-balanced virtual machines that automatically scale in or out depending on the demand.

1. Select **Go to resource** or search for and select the **vmss1** scale set.

1. Choose **Availability + Scale** from the left side menu, then choose **Scaling**.

>**Did you know?** You can **Manual scale** or **Custom autoscale**. In scale sets with a small number of VM instances, increasing or decreasing the instance count (Manual scale) may be best. In scale sets with a large number of VM instances, scaling based on metrics (Custom autoscale) may be more appropriate.

### Scale out rule

1. Select **Custom autoscale** (1). then change the **Scale mode** to **Scale based on metric**. And then select **Add rule**.

   ![](../Labs/Images/l8i14.png) 

1. Let's create a rule that automatically increases the number of VM instances. This rule scales out when the average CPU load is greater than 70% over a 10-minute period. When the rule triggers, the number of VM instances is increased by 20%.

    | Setting | Value |
    | --- | --- |
    | Metric source | **Current resource (vmss1)** |
    | Metric namespace | **Virtual Machine Host** |
    | Metric name | **Percentage CPU** (review your other choices) |

    | Setting | Value |
    | --- | --- |
    | Operator | **Greater than** (1) |
    | Metric threshold to trigger scale action | **70** (2) |
    | Duration (minutes) | **10** (3) |
    | Time grain statistic | **Average** (4) |
    | Operation | **Increase percent by** (review other choices) (5) |
    | Cool down (minutes) | **5** (6) |
    | Percentage | **50** (7) |
      
    ![](../Labs/Images/pax8-image18.png) 

1. Be sure to **Save** your changes.

    ![](../Labs/Images/pax8-image21.png)

### Scale in rule

1. During evenings or weekends, demand may decrease so it is important to create a scale in rule.

1. Let's create a rule that decreases the number of VM instances in a scale set. The number of instances should decrease when the average CPU load drops below 30% over a 10-minute period. When the rule triggers, the number of VM instances is decreased by 20%.

1. Select **Add a rule**, adjust the settings, then select **Add**.

    | Setting | Value |
    | --- | --- |
    | Operator | **Less than** |
    | Threshold | **30** |
    | Operation | **decrease percentage by** (review your other choices) |
    | Percentage | **20** |

1. Be sure to **Save** your changes.

### Set the instance limits

1. When your autoscale rules are applied, instance limits make sure that you do not scale out beyond the maximum number of instances or scale in beyond the minimum number of instances.

1. **Instance limits** are shown on the **Scaling** page after the rules.

    | Setting | Value |
    | --- | --- |
    | Minimum | **2** |
    | Maximum | **10** |
    | Default | **2** |

1. Be sure to **Save** your changes

1. On the **vmss1** page, select **Instances**. This is where you would monitor the number of virtual machine instances.

    ![](../Labs/Images/pax8-image19.png)

    >**Note:** If you are interested in using Azure PowerShell for virtual machine creation, try Task 5. If you are interested in using the CLI to create virtual machines, try Task 6.

      > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
      > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next lab. 
      > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
      > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help
      
      <validation step="bd5e6aae-4787-489b-9bce-6aadf5bcfe0d" />


### Review

In this lab, you have completed the following:

- Deployed zone-resilient Azure virtual machines using the Azure portal, ensuring high availability across different availability zones.
- Managed the scaling of compute and storage resources for virtual machines to meet performance and capacity requirements.
- Created and configured Azure Virtual Machine Scale Sets to automatically manage and scale a group of identical virtual machines based on demand.
- Scaled Azure Virtual Machine Scale Sets to adjust the number of instances according to changing workload demands, ensuring optimal resource utilization.

## Click Next to proceed with the next lab.
