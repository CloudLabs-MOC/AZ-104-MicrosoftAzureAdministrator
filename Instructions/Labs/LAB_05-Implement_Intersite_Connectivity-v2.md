# Lab 05: Implement Intersite Connectivity

## Lab Objectives
In this lab, you will complete the following tasks:


+ Task 1:  Create a core services virtual machine and virtual network
+ Task 2: Create a virtual machine in a different virtual network
+ Task 3: Use Network Watcher to test the connection between virtual machines
+ Task 4: Configure virtual network peerings between different virtual networks
+ Task 5: Use Azure PowerShell to test the connection between virtual machines
+ Task 6: Create a custom route

## Task 1:  Create a core services virtual machine and virtual network

1. From the Azure Portal, search for **Virtual Machines (1)** and select **Virtual Machines (2)**.

    ![image](./media/h1.png)

1. From the virtual machines page, select **Create (1)** then select **Virtual machine (2)**.

    ![image](./media/h2.png)

1. On the Basics tab, use the following information to complete the form, and then select **Next : Disks > (13)**. For any setting not specified, leave the default value.
 
    | Setting | Value | 
    | --- | --- |
    | Subscription |  **Leave the default Subscription (1)** |
    | Resource group |  **az104-05-rg0-<inject key="DeploymentID" enableCopy="false" /> (2)** |
    | Virtual machine name |    **CoreServicesVM<inject key="DeploymentID" enableCopy="false" /> (3)** |
    | Region | **<inject key="DeploymentID" enableCopy="false" /> (4)** |
    | Availability options | **No infrastructure redundancy required (5)** |
    | Security type | **Standard (6)** |
    | Image (See all images) | **Windows Server 2025 Datacenter - x64 Gen2 (7)** (notice your other choices) |
    | Size | **Standard_D2s_v3 (8)** |
    | Username | `azureuser` **(9)** | 
    | Password | **Pa55w.rd1234 (10)** |
    | Confirm Password | **Pa55w.rd1234 (11)** |    
    | Public inbound ports | **None (12)** |

    ![image](./media/h3.png)
    ![image](./media/h4.png)        

1. On the **Disks** tab take the defaults and then select **Next : Networking >**.

1. On the **Networking** tab, for Virtual network, select **Create new**.

    ![image](./media/h5.png) 

1. Use the following information to configure the virtual network, and then select **OK (5)**. If necessary, remove or replace the existing information.

    | Setting | Value | 
    | --- | --- |
    | Name | `CoreServicesVnet` (Create or edit) **(1)** |
    | Address range | `10.0.0.0/16` **(2)** |
    | Subnet Name | `Core` **(3)** (Edit the **Default**) | 
    | Subnet address range | `10.0.0.0/24` **(4)** |

    ![image](./media/h8.png)     

1. Select the **Monitoring (1)** tab. For Boot diagnostics, select **Disable (2)** and then select **Review + create (3)**.

    ![image](./media/h7.png) 

1. Then select **Create**.

1. **You do not need to wait for the resources to be created. Continue on to the next task.**

    >**Note:** Did you notice in this task you created the virtual network as you created the virtual machine? You could also create the virtual network infrastructure then add the virtual machines. 

## Task 2: Create a virtual machine in a different virtual network

In this task, you create a manufacturing services virtual network with a virtual machine. 

1. From the Azure Portal, search for **Virtual Machines (1)** and select **Virtual Machines (2)**.

    ![image](./media/h1.png)

1. From the virtual machines page, select **Create (1)** then select **Virtual machine (2)**.

    ![image](./media/h2.png)

1. On the Basics tab, use the following information to complete the form, and then select **Next : Disks > (13)**. For any setting not specified, leave the default value.
 
    | Setting | Value | 
    | --- | --- |
    | Subscription |  **Leave the default Subscription (1)** |
    | Resource group |  **az104-05-rg0-<inject key="DeploymentID" enableCopy="false" /> (2)** |
    | Virtual machine name |    **ManufacturingVM<inject key="DeploymentID" enableCopy="false" /> (3)** |
    | Region | **<inject key="DeploymentID" enableCopy="false" /> (4)** |
    | Availability options | **No infrastructure redundancy required (5)** |
    | Security type | **Standard (6)** |
    | Image (See all images) | **Windows Server 2025 Datacenter - x64 Gen2 (7)** (notice your other choices) |
    | Size | **Standard_D2s_v3 (8)** |
    | Username | `azureuser` **(9)** | 
    | Password | **Pa55w.rd1234 (10)** |
    | Confirm Password | **Pa55w.rd1234 (11)** |    
    | Public inbound ports | **None (12)** |

    ![image](./media/h9.png)
    ![image](./media/h4.png) 

1. On the **Disks** tab take the defaults and then select **Next : Networking >**.

1. On the Networking tab, for Virtual network, select **Create new**.

    ![image](./media/h10.png)

1. Use the following information to configure the virtual network, and then select **OK (5)**.  If necessary, remove or replace the existing address range.

    | Setting | Value | 
    | --- | --- |
    | Name | `ManufacturingVnet` **(1)** |
    | Address range | `172.16.0.0/16` **(2)** |
    | Subnet Name | `Manufacturing` **(3)** |
    | Subnet address range | `172.16.0.0/24` **(4)** |

    ![image](./media/h11.png)    

1. Select the **Monitoring** tab. For Boot Diagnostics, select **Disable** and then elect **Review + create**.

    ![image](./media/h7.png) 

1. Then select **Create**.

1. Wait for the resources to be created, it may take some time to create.
