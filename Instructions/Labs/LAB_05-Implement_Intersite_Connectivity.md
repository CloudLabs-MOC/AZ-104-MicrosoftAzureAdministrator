# Lab 05 - Implement Intersite Connectivity

## Lab Overview
 
 In this lab, you will set up and configure a virtual network, create subnets to organize resources, and implement network security through Network Security Groups (NSGs). 

## Lab objectives
In this lab, you will complete the following tasks:
+ Task 1: Provision the lab environment.
+ Task 2: Use Network Watcher to test the connection between virtual machines.
+ Task 3: Configure local and global virtual network peering.
+ Task 4: Test intersite connectivity.
+ Task 5: Create a custom route.
 
## Exercise 1: Configure local and global virtual network peering

   Configure local and global virtual network peering to enable secure communication between Azure VNets, both within the same region (local) and across different regions (global).

### Task 1: Provision the lab environment

In this task, you will deploy three virtual machines, each into a separate virtual network, with two of them in the same Azure region and the third one in another Azure region.
   
1. In the Azure portal, open the **Azure Cloud Shell** by clicking on the icon in the top right of the Azure Portal.

    ![Image](./Images/cloudshell.png)

1. On the Cloudshell window, click on **Switch to Powershell** and click on **Confirm**.  

    ![Image](./Images/switch-to-powershell-0905.png)

1. In the toolbar of the Cloud Shell pane, click on the **Manage files** dropdown, click **Upload**, and upload one by one the files **C:\AllFiles\AZ-104-MicrosoftAzureAdministrator-Lab-Files\Allfiles\Labs\05\\az104-05-vnetvm-loop-template.json** and **C:\AllFiles\AZ-104-MicrosoftAzureAdministrator-Lab-Files\Allfiles\Labs\05\\az104-05-vnetvm-loop-parameters.json** into the Cloud Shell home directory.

    ![Image](./Images/upload-files-incloudshell-0905a.png)
    ![Image](./Images/upload-files-incloudshell-0905.png)

1.  From the Cloud Shell pane, run the following command to set up the regions for your deployment. Replace **Azure_region_1** with the name of the first Azure region where you want to deploy your virtual machines, and **Azure_region_2** with a different Azure region for the third virtual machine. **For example**, you can use **$location1 = 'eastus'** and **$location2 = 'westus'**. The first two virtual networks and two virtual machines will be deployed in $location1, while the third virtual network and the third virtual machine will be deployed in $location2 within the same resource group. 

    ```powershell
        $location1 = 'Azure_region_1'

        $location2 = 'Azure_region_2'

        $rgName = 'az104-rg2'
    ```

    >**Note**: In order to identify Azure regions, from the PowerShell session in Cloud Shell, run the **(Get-AzLocation).Location** command.
   
    >**Note**: If you get a prompt stating **Provided resource group already exists. Are you sure you want to update it?** type **N**.

1. From the Cloud Shell pane, run the following to create the three virtual networks and deploy virtual machines into them by using the template and parameter files you uploaded:

      ```powershell
      New-AzResourceGroupDeployment `
         -ResourceGroupName $rgName `
         -TemplateFile $HOME/az104-05-vnetvm-loop-template.json `
         -TemplateParameterFile $HOME/az104-05-vnetvm-loop-parameters.json `
         -location1 $location1 `
         -location2 $location2
      ```

    >**Note**: If you face any issues in copying and pasting the commands, please paste the command first on a Notepad inside the VM, then paste it in Cloud Shell pane. 
    
    >**Important**: You will be prompted to provide an admin password. Enter your own Password or give **Pa55w.rd1234**.
    
    >**Note**: Wait for the deployment to complete before proceeding to the next step. This should take about 2 minutes.

1. Close the Cloud Shell pane.

## Task 2: Use Network Watcher to test the connection between virtual machines 

In this task, you verify that resources in peered virtual networks can communicate with each other. Network Watcher will be used to test the connection. Before continuing, ensure both virtual machines have been deployed and are running. 

1. From the Azure portal, search for and select **Network Watcher**.

1. From Network Watcher, in the **Network diagnostic tools** menu in the left navigation pane, select **Connection troubleshoot**.

1. Use the following information to complete the fields on the **Connection troubleshoot** page and select **Run diagnostic tests** (8).

    | Field | Value | 
    | --- | --- |
    | Source type           | **Virtual machine** (1)  |
    | Virtual machine       | **az104-05-vm0**  (2) | 
    | Destination type      | **Select a Virtual machine** (3)  |
    | Virtual machine       | **az104-05-vm1**  (4) | 
    | Preferred IP Version  | **Both**  (5)            | 
    | Protocol              | **TCP**    (6)           |
    | Destination port      | `3389`     (7)           |  
    | Source port           | *Blank*         |
    | Diagnostic tests      | *Defaults*      |

    ![Azure Portal showing Connection Troubleshoot settings.](./Images/L5T2S3-0905.png)

    >**Note**: It may take a couple of minutes for the results to be returned. The screen selections will be greyed out while the results are being collected.. 

1. Notice the Connectivity test shows UnReachable. This makes sense because the virtual machines are in different virtual networks.
   
### Task 3: Configure local and global virtual network peering

In this task, you will configure local and global peering between the virtual networks you deployed in the previous tasks.

1. In the Azure portal, search for and select **Virtual networks**.

    ![Image](./Images/selectvnet.png)

1. Review the virtual networks you created in the previous task and verify that the first two are located in the same Azure region and the third one in a different Azure region.

    >**Note**: The template you used for the deployment of the three virtual networks ensures that the IP address ranges of the three virtual networks do not overlap.

1. In the list of virtual networks, click **az104-05-vnet0**.
     
      ![Image](./Images/L5T3S3-0905.png)
      
1. On the **az104-05-vnet0** virtual network blade, in the **Settings** section, click **Peerings (1)** and then click **+ Add (2)**.

     ![Image](./Images/L5T3S4-0905.png)

1. Add a peering with the following settings (leave others with their default values) and click **Add**:

    | Setting | Value|
    | --- | --- |
    | Remote virtual network: Peering link name | **az104-05-vnet1_to_az104-05-vnet0** (1) |
    | Virtual network deployment model | **Resource manager** (2)|
    | I know my resource ID | unselected (3)|
    | Subscription | the name of the Azure subscription you are using in this lab (4) |
    | Virtual network | **az104-05-vnet1** (5)|
    | Remote virtual network peering settings | **Ensure only the first three boxes are checked** (6) |
    | Local Peering link name | **az104-05-vnet0_to_az104-05-vnet1** (7)|
    | Local virtual network peering settings | **Ensure only the first three boxes are checked** (8)|
   
   ![Image](./Images/az-104-3.png)

   ![Image](./Images/az-104-6.png)
    
      >**Note**: You can ignore the warning stating that the VNet does not have a routing gateway.

      >**Note**: This step establishes two local peerings - one from az104-05-vnet0 to az104-05-vnet1 and the other from az104-05-vnet1 to az104-05-vnet0.

      >**Note**: In case you run into an issue with the Azure portal interface not displaying the virtual networks created in the previous task, you can configure peering by running the following PowerShell commands from Cloud Shell:

      >**Note**: Replace Deployment-id with **<inject key="DeploymentID" enableCopy="false" />**.
  
    ```powershell
       $rgName = 'az104-rg2'
    
       $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname
    
       $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname
    
       Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet1' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet1.Id
    
       Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet0' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet0.Id
    ``` 

1. On the **az104-05-vnet0** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Add a peering with the following settings (leave others with their default values) and click **Add**:

    | Setting | Value|
    | --- | --- |
    | Remote virtual network: Peering link name | **az104-05-vnet2_to_az104-05-vnet0** (1) |
    | Virtual network deployment model | **Resource manager** (2)|
    | I know my resource ID | unselected (3)|
    | Subscription | the name of the Azure subscription you are using in this lab (4) |
    | Virtual network | **az104-05-vnet2** (5)|
    | Remote virtual network peering settings | **Ensure only the first three boxes are checked** (6) |
    | Local Peering link name | **az104-05-vnet0_to_az104-05-vnet2** (7)|
    | Local virtual network peering settings | **Ensure only the first three boxes are checked** (8)|

    >**Note**: You can ignore the warning stating that the VNet does not have a routing gateway.
 
    >**Note**: This step establishes two global peerings - one from az104-05-vnet0 to az104-05-vnet2 and the other from az104-05-vnet2 to az104-05-vnet0.

    >**Note**: In case you run into an issue with the Azure portal interface not displaying the virtual networks created in the previous task, you can configure peering by running the following PowerShell commands from Cloud Shell:

    >**Note**: Replace Deployment-id with **<inject key="DeploymentID" enableCopy="false" />**.
   
    
   ```powershell
   $rgName = 'az104-rg2'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet2' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet0' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. Navigate back to the **Virtual networks** blade and, in the list of virtual networks, click **az104-05-vnet1**.

1. On the **az104-05-vnet1** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

1. Add a peering with the following settings (leave others with their default values) and click **Add**:

    | Setting | Value|
    | --- | --- |
    | Remote virtual network: Peering link name | **az104-05-vnet2_to_az104-05-vnet1** (1) |
    | Virtual network deployment model | **Resource manager** (2)|
    | I know my resource ID | unselected (3)|
    | Subscription | the name of the Azure subscription you are using in this lab (4) |
    | Virtual network | **az104-05-vnet2** (5)|
    | Remote virtual network peering settings | **Ensure only the first three boxes are checked** (6) |
    | Local Peering link name | **az104-05-vnet1_to_az104-05-vnet2** (7)|
    | Local virtual network peering settings | **Ensure only the first three boxes are checked** (8)|

    >**Note**: You can ignore the warning stating that the VNet does not have a routing gateway.
 
    >**Note**: This step establishes two global peerings - one from az104-05-vnet1 to az104-05-vnet2 and the other from az104-05-vnet2 to az104-05-vnet1.

    >**Note**: In case you run into an issue with the Azure portal interface not displaying the virtual networks created in the previous task, you can configure peering by running the following PowerShell commands from Cloud Shell:

    >**Note**: Replace Deployment-id with **<inject key="DeploymentID" enableCopy="false" />**.
    

   ```powershell
   $rgName = 'az104-rg2'

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet2' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet1' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id
   ``` 
   
    > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
    > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
    > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
    > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help

<validation step="af96cff1-7868-4911-818a-f05d61c27835" />

### Task 4: Test intersite connectivity

In this task, you will test connectivity between virtual machines on the three virtual networks that you connected via local and global peering in the previous task.

1. In the Azure portal, search for and select **Virtual machines**.

     ![Image](./Images/vm.png)

2. In the list of virtual machines, click **az104-05-vm0**.

      ![Image](./Images/L5T4S2-0905.png)

3. On the **az104-05-vm0** blade, click **Connect (1)** and select **Connect (2)** from the dropdown menu. Click **Connect via RDP** blade, click **Download RDP File**, and follow the prompts to start the Remote Desktop session.

      ![Image](./Images/L5T4S3.1-0905.png)
      
      ![Image](./Images/L5T4S3.2-0905.png)
      
      ![Image](./Images/rdp.png)

    >**Note**: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store,e and on Linux computers, you can use an open-source RDP client software.

    >**Note**: You can ignore any warning prompts when connecting to the target virtual machines and select Keep.

4. When prompted, sign in by using the **Student** username and the password **Pa55w.rd1234**.

   >**Note**: You can click on **Yes** in the pop-up that appears.
   
   >**Note**: If you get a prompt related to network discovery, click on Yes

6. Within the Remote Desktop session to **az104-05-vm0**, right-click the **Start** button and, in the right-click menu, click **Windows PowerShell (Admin)** and slect Run as Administrator option.

     ![Image](./Images/Virtual%20Networking%20Ex1-t5-p10.png)
   
7. In the Windows PowerShell console window, run the following to test connectivity to **az104-05-vm1** (which has the private IP address of **10.51.0.4**) over TCP port 3389:

   ```powershell
   Test-NetConnection -ComputerName 10.51.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    >**Note**: The test uses TCP 3389 since this port is allowed by default by the operating system firewall.

8. Examine the output of the command and verify that the connection was successful.

9. In the Windows PowerShell console window, run the following to test connectivity to **az104-05-vm2** (which has the private IP address of **10.52.0.4**):

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

10. Switch back to the Azure portal on your lab computer and navigate back to the blade of the **Virtual machine**.

11. In the list of virtual machines, click **az104-05-vm1**.

    ![Image](./Images/L5T4S11-0905.png)

12. On the **az104-05-vm1** blade, click **Connect (1)** and select **Connect (2)** from the dropdown menu. Click **Connect via RDP** blade, click **Download RDP File**, and follow the prompts to start the Remote Desktop session.

    >**Note**: This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store, and on Linux computers, you can use an open-source RDP client software.

    >**Note**: You can ignore any warning prompts when connecting to the target virtual machines.

13. When prompted, sign in by using the **Student** username and the password **Pa55w.rd1234**.

14. Within the Remote Desktop session to **az104-05-vm1**, right-click the **Start** button and, in the right-click menu, click **Windows PowerShell (Admin)**.

15. In the Windows PowerShell console window, run the following to test connectivity to **az104-05-vm2** (which has the private IP address of **10.52.0.4**) over TCP port 3389:

    ```powershell
    Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
    ```

    >**Note**: The test uses TCP 3389 since this port is allowed by default by the operating system firewall.

16. Examine the output of the command and verify that the connection was successful.


## Task 5: Create a custom route 

In this task, you want to control network traffic between the perimeter subnet and the internal core services subnet. A virtual network appliance will be installed in the core services subnet, and all traffic should be routed there. 

1. Back in the Azure portal, navigate to the  Virtual networks resource and select the **az104-05-vnet0** from the list of virtual networks 
   networks.

1. Select **Subnets** and then click on **+ Subnet** and click on **Add**

    | Setting | Value | 
    | --- | --- |
    | Name | `perimeter` |
    | Starting address  | `10.50.1.0/24`  |

1. In the Azure portal, search for and select **Route tables** resource, and then select **Review + Create** and subsequently 
   click on **Create**. 

    | Setting | Value | 
    | --- | --- |
    | Subscription | Default Subscription |
    | Resource group | **az104-rg2**  |
    | Region | Select the region of **az104-05-vnet0** virtual newtwork resource. |
    | Name | **az104-05-vm0** |
    | Propagate gateway routes | **No** |

1. After the route table deploys, select **Go to resource**.

1. From the left navigation pane, under **Settings** select **Routes** and then **+ Add**. Create a route from the future NVA  
   to the CoreServices virtual network. 

    | Setting | Value | 
    | --- | --- |
    | Route name | `PerimetertoCore` |
    | Destination type | **IP Addresses** |
    | Destination IP addresses | `10.50.0.0/22` (core services virtual network) |
    | Next hop type | **Virtual appliance** (notice your other choices) |
    | Next hop address | `10.50.1.7` (future NVA) |

1. Select **Add**. When the route is completed. The last thing to do is associate the route with the subnet.

1. Select **Subnets** from the left navigation pane and then  click on **+ Associate**. Complete the configuration.

    | Setting | Value | 
    | --- | --- |
    | Virtual network | **az104-05-vnet0** |
    | Subnet | **subnet0** |    

     >**Note**: You have created a user-defined route to direct traffic from the DMZ to the new NVA.  

### Review

In this lab, you have completed the following:

- Provisioned the lab environment, setting up the necessary resources for the network configuration.
- Utilized Network Watcher to test and troubleshoot the connection between virtual machines, ensuring proper network communication.
- Configured both local and global virtual network peering, enabling seamless communication between Azure VNets within the same region (local) and across different regions (global).
- Tested intersite connectivity to verify that communication is established between virtual networks in different locations.
- Created a custom route to control network traffic flow, optimizing the routing of data between various subnets and resources.

## Extend your learning with Copilot
Copilot can assist you in learning how to use the Azure scripting tools. Copilot can also assist in areas not covered in the lab or where you need more information. Open an Edge browser and choose Copilot (top right) or navigate to *copilot.microsoft.com*. Take a few minutes to try these prompts.

+ How can I use Azure PowerShell or Azure CLI commands to add a virtual network peering between vnet1 and vnet2?
+ Create a table highlighting various Azure and 3rd party monitoring tools supported on Azure. Highlight when to use each tool. 
+ When would I create a custom network route in Azure?

## Learn more with self-paced training

+ [Distribute your services across Azure virtual networks and integrate them by using virtual network peering](https://learn.microsoft.com/en-us/training/modules/integrate-vnets-with-vnet-peering/). Use virtual network peering to enable communication across virtual networks in a way that's secure and minimally complex.
+ [Manage and control traffic flow in your Azure deployment with routes](https://learn.microsoft.com/training/modules/control-network-traffic-flow-with-routes/). Learn how to control Azure virtual network traffic by implementing custom routes.


## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 

+ By default, resources in different virtual networks cannot communicate.
+ Virtual network peering enables you to seamlessly connect two or more virtual networks in Azure.
+ Peered virtual networks appear as one for connectivity purposes.
+ The traffic between virtual machines in peer virtual networks uses the Microsoft backbone infrastructure.
+ System-defined routes are automatically created for each subnet in a virtual network. User-defined routes override or add to the default system routes. 
+ Azure Network Watcher provides a suite of tools to monitor, diagnose, and view metrics and logs for Azure IaaS resources.

### You have successfully completed the lab
