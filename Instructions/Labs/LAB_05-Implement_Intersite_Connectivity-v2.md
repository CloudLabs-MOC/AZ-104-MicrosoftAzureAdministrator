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

1. Search for and select `Virtual Machines`.

1. From the virtual machines page, select **Create** then select **Virtual machine**.

1. On the Basics tab, use the following information to complete the form, and then select **Next : Disks >**. For any setting not specified, leave the default value.
 
    | Setting | Value | 
    | --- | --- |
    | Subscription |  *your subscription* |
    | Resource group |  **az104-05-rg0-<inject key="DeploymentID" enableCopy="false" /> (2)** |
    | Virtual machine name |    **CoreServicesVM<inject key="DeploymentID" enableCopy="false" />** |
    | Region | **<inject key="DeploymentID" enableCopy="false" />** |
    | Availability options | No infrastructure redundancy required |
    | Security type | **Standard** |
    | Image (See all images) | **Windows Server 2025 Datacenter - x64 Gen2** (notice your other choices) |
    | Size | **Standard_D2s_v3** |
    | Username | `localadmin` | 
    | Password | **Provide a complex password** |
    | Public inbound ports | **None** |

