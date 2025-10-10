# Lab - Implement Intersite Connectivity

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
   
1. In the Azure portal, open the **Azure Cloud Shell (1)** by clicking on the icon in the top right of the Azure Portal.

1. When prompted to select either **Bash** or **PowerShell**, select **PowerShell (2)**. 

     ![image](../media/7-10-lab3-25.png)

    >**Did you know?**  If you mostly work with Linux systems, Bash (CLI) feels more familiar. If you mostly work with Windows systems, Azure PowerShell feels more familiar. 

1. In the **Getting started** window, select **Mount storage account (1)**, choose the **subscription (2)** from the dropdown, and click **Apply (3)** to continue.

     ![image](../media/7-10-lab3-26.png)

1. On mount storage account page, select **I want to create a storage account (1)**. click on **Next (2)**.

    ![image](../media/7-10-lab3-27.png)

1. Provide the below details to create the storage account and click on **Create (6)**.

    
    | Settings | Values |
    |  -- | -- |
    | Subscription | Accept default **(1)**|
    | Resource Group | **az104-05-rg0-<inject key="DeploymentID" enableCopy="false" /> (2)** |
    | Region | **<inject key="Region" enableCopy="false" /> (3)** |
    | Storage account (Create new) | **str<inject key="DeploymentID" enableCopy="false" /> (4)** |
    | File share (Create new) | **none (5)** |

     ![image](../media/10-10-lab5-2.png)

     >**Note:** As you work with the Cloud Shell a storage account and file share is required. 

1. In the **Cloud Shell** toolbar, click the **Manage files (1)** drop-down and select **Upload (2)**.

      ![image](../media/7-10-lab3-42.png)

1. In the **Open** dialog box, browse to the path **C:\AllFiles\AZ-104-MicrosoftAzureAdministrator-Lab-Files\Allfiles\Labs\05 (1)**, select **azuredeploydisk.bicep (2)**, and click **Open (3)** to upload the file.

    ![image](../media/10-10-lab5-3.png)

    ![image](../media/10-10-lab5-4.png)

    >**NOte:** upload the the template and parameters file. You will need to upload each file separately one after the another.

1. Verify your files are available in the Cloud Shell storage. 

    ```powershell
    dir
    ```
    ![image](../media/10-10-lab5-5.png)

1. From the Cloud Shell pane, run the below command to set up the regions for your deployment. Replace **Azure_region_1** with the name of the first Azure region where you want to deploy your virtual machines, and **Azure_region_2** with a different Azure region for the third virtual machine. **For example**, you can use **$location1 = 'eastus'** and **$location2 = 'westus'**. The first two virtual networks and two virtual machines will be deployed in $location1, while the third virtual network and the third virtual machine will be deployed in $location2 within the same resource group. 

    ```powershell
   $location1 = 'Azure_region_1'

   $location2 = 'Azure_region_2'

   $rgName = 'az104-05-rg0-Deployment-id'
    ```

    >**Note:** In order to identify Azure regions, from the PowerShell session in Cloud Shell, run **(Get-AzLocation).Location** command.

    >**Important:** Replace Deployment-id with **<inject key="DeploymentID" enableCopy="false" />**.
   
    >**Note:** If you get a prompt stating **Provided resource group already exists. Are you sure you want to update it?** type N .

1. From the Cloud Shell pane, run the following to create the three virtual networks and deploy virtual machines into them by using the template and parameter files you uploaded:

   ```powershell
   New-AzResourceGroupDeployment `
      -ResourceGroupName $rgName `
      -TemplateFile $HOME/az104-05-vnetvm-loop-template.json `
      -TemplateParameterFile $HOME/az104-05-vnetvm-loop-parameters.json `
      -location1 $location1 `
      -location2 $location2
   ```

    ![image](../media/10-10-lab5-6.png)

    >**Important:** You will be prompted to provide an admin password. Enter your own Password or give **Pa55w.rd1234**.
    
    >**Note:** Wait for the deployment to complete before proceeding to the next step. This should take about 2 minutes.

1. Close the Cloud Shell pane.

## Task 2: Use Network Watcher to test the connection between virtual machines 

In this task, you verify that resources in peered virtual networks can communicate with each other. Network Watcher will be used to test the connection. Before continuing, ensure both virtual machines have been deployed and are running. 

1. In Search resources, services, and docs (G+/) box at the top of the portal, enter **Network Watcher (1)**, and then select **Network Watcher (2)** from the results.

     ![image](../media/10-10-lab5-7.png)

1. In **Network Watcher**, expand **Network diagnostic tools (1)** from the left navigation pane, and select **Connection troubleshoot (2)**.

     ![image](../media/10-10-lab5-8.png)

1. Use the following information to complete the fields on the **Connection troubleshoot** page and select **Run diagnostic tests (9)**.

    | Field | Value | 
    | --- | --- |
    | Source type           | **Virtual machine (1)**  |
    | Virtual machine       | **az104-05-vm0 (2)** | 
    | Destination type      | **Virtual machine (3)**  |
    | Virtual machine       | **az104-05-vm1 (4)** | 
    | Preferred IP Version  | **Both  (5)**            | 
    | Protocol              | **TCP  (6)**           |
    | Destination port      | `3389`     **(7)**           |  
    | Source port           | *Blank*        |
    | Diagnostic tests      | *Defaults*  **(8)**     |

     ![image](../media/10-10-lab5-9.png)

    >**Note:** It may take a couple of minutes for the results to be returned. The screen selections will be greyed out while the results are being collected.. 

1. Notice the Connectivity test shows UnReachable. This makes sense because the virtual machines are in different virtual networks.

      ![image](../media/10-10-lab5-10.png)

### Task 3: Configure local and global virtual network peering

In this task, you will configure local and global peering between the virtual networks you deployed in the previous tasks.

1. In the Azure portal search bar, type **Virtual networks (1)** and select **Virtual networks (2)** from the search results.

    ![image](../media/10-10-lab5-11.png)

1. Review the virtual networks you created in the previous task and verify that the first two are located in the same Azure region and the third one in a different Azure region.

     ![image](../media/10-10-lab5-12.png)

    >**Note:** The template you used for the deployment of the three virtual networks ensures that the IP address ranges of the three virtual networks do not overlap.

1. In the list of virtual networks, click **az104-05-vnet0**.
     
      ![image](../media/10-10-lab5-16.png)
       
1. On the **az104-05-vnet0** virtual network blade, in the **Settings (1)** section, click **Peerings (2)** and then click **+ Add (3)**.

     ![image](../media/10-10-lab5-13.png)

1. Add a peering with the following settings (leave others with their default values) and click **Add (8)**:

    | Setting | Value|
    | --- | --- |
    | Remote virtual network: Peering link name | **az104-05-vnet1_to_az104-05-vnet0 (1)** |
    | I know my resource ID | unselected **(2)**|
    | Subscription | the name of the Azure subscription you are using in this lab **(3)** |
    | Virtual network | **az104-05-vnet1 (4)**|
    | Remote virtual network peering settings | **Ensure only the first three boxes are checked (5)** |
    | Local Peering link name | **az104-05-vnet0_to_az104-05-vnet1 (6)**|
    | Local virtual network peering settings | **Ensure only the first three boxes are checked (7)**|
   
    ![image](../media/10-10-lab5-14.png) 

    ![image](../media/10-10-lab5-15.png)
    
      >**Note:** You can ignore the warning stating that the vnet does not have a routing gateway.

      >**Note:** This step establishes two local peerings - one from az104-05-vnet0 to az104-05-vnet1 and the other from az104-05-vnet1 to az104-05-vnet0.

      >**Note:** In case you run into an issue with the Azure portal interface not displaying the virtual networks created in the previous task, you can configure peering by running the following PowerShell commands from Cloud Shell:

      >**Note:** Replace Deployment-id with **<inject key="DeploymentID" enableCopy="false" />**.
  
    ```powershell
       $rgName = 'az104-05-rg0-Deployment-id'
    
       $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname
    
       $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname
    
       Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet1' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet1.Id
    
       Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet0' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet0.Id
    ``` 

1. On the **az104-05-vnet0** virtual network blade, in the **Settings** section, click **Peerings** and then click **+ Add**.

     ![image](../media/10-10-lab5-17.png)

1. Add a peering with the following settings (leave others with their default values) and click **Add (8)**:

    | Setting | Value|
    | --- | --- |
    | Remote virtual network: Peering link name | **az104-05-vnet2_to_az104-05-vnet0 (1)** |
    | I know my resource ID | unselected **(2)**|
    | Subscription | the name of the Azure subscription you are using in this lab **(3)** |
    | Virtual network | **az104-05-vnet2 (4)**|
    | Remote virtual network peering settings | **Ensure only the first three boxes are checked (5)** |
    | Local Peering link name | **az104-05-vnet0_to_az104-05-vnet2 (6)**|
    | Local virtual network peering settings | **Ensure only the first three boxes are checked (7)**|

    ![image](../media/10-10-lab5-19.1.png)

    ![image](../media/10-10-lab5-19.2.png)

    >**Note:** You can ignore the warning stating that the vnet does not have a routing gateway.
 
    >**Note:** This step establishes two global peerings - one from az104-05-vnet0 to az104-05-vnet2 and the other from az104-05-vnet2 to az104-05-vnet0.

    >**Note:** In case you run into an issue with the Azure portal interface not displaying the virtual networks created in the previous task, you can configure peering by running the following PowerShell commands from Cloud Shell:

    >**Note:** Replace Deployment-id with **<inject key="DeploymentID" enableCopy="false" />**.
   
    
   ```powershell
   $rgName = 'az104-05-rg0-Deployment-id'

   $vnet0 = Get-AzVirtualNetwork -Name 'az104-05-vnet0' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet0_to_az104-05-vnet2' -VirtualNetwork $vnet0 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet0' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet0.Id
   ``` 

1. Navigate back to the **Virtual networks** blade and, in the list of virtual networks, click **az104-05-vnet1**.

     ![image](../media/10-10-lab5-18.png)

1. On the **az104-05-vnet1** virtual network blade, in the **Settings (1)** section, click **Peerings (2)** and then click **+ Add (3)**.

    ![image](../media/10-10-lab5-19.png)

1. Add a peering with the following settings (leave others with their default values) and click **Add (8)**:

    | Setting | Value|
    | --- | --- |
    | Remote virtual network: Peering link name | **az104-05-vnet2_to_az104-05-vnet1 (1)** |
    | I know my resource ID | unselected **(2)**|
    | Subscription | the name of the Azure subscription you are using in this lab **(3)** |
    | Virtual network | **az104-05-vnet2 (4)**|
    | Remote virtual network peering settings | **Ensure only the first three boxes are checked (5)** |
    | Local Peering link name | **az104-05-vnet1_to_az104-05-vnet2 (6)**|
    | Local virtual network peering settings | **Ensure only the first three boxes are checked (7)**|

    ![image](../media/10-10-lab5-20.png)

    ![image](../media/10-10-lab5-21.png)

    >**Note:** You can ignore the warning stating that the vnet does not have a routing gateway.
 
    >**Note:** This step establishes two global peerings - one from az104-05-vnet1 to az104-05-vnet2 and the other from az104-05-vnet2 to az104-05-vnet1.

    >**Note:** In case you run into an issue with the Azure portal interface not displaying the virtual networks created in the previous task, you can configure peering by running the following PowerShell commands from Cloud Shell:

    >**Note:** Replace Deployment-id with **<inject key="DeploymentID" enableCopy="false" />**.
    

   ```powershell
   $rgName = 'az104-05-rg0-Deployment-id'

   $vnet1 = Get-AzVirtualNetwork -Name 'az104-05-vnet1' -ResourceGroupName $rgname

   $vnet2 = Get-AzVirtualNetwork -Name 'az104-05-vnet2' -ResourceGroupName $rgname

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet1_to_az104-05-vnet2' -VirtualNetwork $vnet1 -RemoteVirtualNetworkId $vnet2.Id

   Add-AzVirtualNetworkPeering -Name 'az104-05-vnet2_to_az104-05-vnet1' -VirtualNetwork $vnet2 -RemoteVirtualNetworkId $vnet1.Id
   ``` 
   
   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help
   
   <validation step="7df6e281-5a25-416d-8ad3-7e0c8a9218d9" />
   
   <validation step="48e06831-31a7-4601-9840-89661d5d56dd" />
   
   <validation step="fb306908-e661-4cac-8769-3eb8b7108271" />


### Task 4: Test intersite connectivity

In this task, you will test connectivity between virtual machines on the three virtual networks that you connected via local and global peering in the previous task.

1. In the Azure portal search bar, type **Virtual machines (1)** and select **Virtual machines (2)** from the search results.

     ![Image](./Images/vm.png)

1. In the **Compute infrastructure | Virtual machines** blade, select the virtual machine **az104-05-vm0** from the list to open its overview page.

      ![image](../media/10-10-lab5-22.png)

1. On the **az104-05-vm0** blade, select **Connect (1)** from the top menu, then choose **Connect (2)** again from the dropdown.  

      ![image](../media/10-10-lab5-23.png)

1. On the **Native RDP** page, click **Download RDP file** and open it. When prompted with a security warning, select **Connect** to initiate the Remote Desktop session.  

     ![image](../media/10-10-lab5-24.png)
      
     ![image](../media/10-10-lab5-25.png)

     >**Note:** This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store,e and on Linux computers, you can use an open-source RDP client software.

     >**Note:** You can ignore any warning prompts when connecting to the target virtual machines and select Keep.

1. When prompted, sign in by using the **Student** username and the password **Pa55w.rd1234**.

   >**Note:** You can click on **Yes** in the pop-up that appears.
   
   >**Note:** If you get a prompt related to network discovery, click on Yes

1. Within the Remote Desktop session to **az104-05-vm0**, click the **Start (1)** button, right-click **Windows PowerShell (2)**, select **More (3)**, and then choose **Run as administrator (4)**.

     ![image](../media/10-10-lab5-25.1.png)
   
1. In the Windows PowerShell console window, run the following to test connectivity to **az104-05-vm1** (which has the private IP address of **10.51.0.4**) over TCP port 3389:

   ```powershell
   Test-NetConnection -ComputerName 10.51.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    ![image](../media/10-10-lab5-26.png)

    >**Note:** The test uses TCP 3389 since this port is allowed by default by the operating system firewall.

1. Examine the output of the command and verify that the connection was successful.

1. In the Windows PowerShell console window, run the following to test connectivity to **az104-05-vm2** (which has the private IP address of **10.52.0.4**):

   ```powershell
   Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
   ```

    ![image](../media/10-10-lab5-27.png)

1. Switch back to the Azure portal on your lab computer and navigate back to the blade of the **Virtual machine**.

1. In the list of virtual machines, click **az104-05-vm1**.

    ![image](../media/10-10-lab5-28.1.png)

1. On the **az104-05-vm1** blade, select **Connect (1)** from the top menu, then choose **Connect (2)** again from the dropdown.  

    ![image](../media/10-10-lab5-28.png)

1. On the **Native RDP** page, click **Download RDP file** and open it. When prompted with a security warning, select **Connect** to initiate the Remote Desktop session. 

    >**Note:** This step refers to connecting via Remote Desktop from a Windows computer. On a Mac, you can use Remote Desktop Client from the Mac App Store, and on Linux computers, you can use an open source RDP client software.

    >**Note:** You can ignore any warning prompts when connecting to the target virtual machines.

1. When prompted, sign in by using the **Student** username and the password **Pa55w.rd1234**.

1. Within the Remote Desktop session to **az104-05-vm0**, click the **Start (1)** button, right-click **Windows PowerShell (2)**, select **More (3)**, and then choose **Run as administrator (4)**.

     ![image](../media/10-10-lab5-25.1.png)

1. In the Windows PowerShell console window, run the following to test connectivity to **az104-05-vm2** (which has the private IP address of **10.52.0.4**) over TCP port 3389:

    ```powershell
    Test-NetConnection -ComputerName 10.52.0.4 -Port 3389 -InformationLevel 'Detailed'
    ```

    >**Note:** The test uses TCP 3389 since this port is allowed by default by the operating system firewall.

1. Examine the output of the command and verify that the connection was successful.

     ![image](../media/10-10-lab5-29.png)


## Task 5: Create a custom route 

In this task, you want to control network traffic between the perimeter subnet and the internal core services subnet. A virtual network appliance will be installed in the core services subnet and all traffic should be routed there. 

1. Back in the Azure portal, navigate to Virtual networks resource and select the **az104-05-vnet0** from the list of virtual networks.

1. On the **az104-05-vnet0** page, under **Settings (1)**, select **Subnets (2)**, then click **+ Subnet (3)** to add a new subnet.

     ![image](../media/10-10-lab5-30.png)

1. In the **Add a subnet** pane, enter the following details and click **Add (3)** to create the subnet.

    | Setting | Value | 
    | --- | --- |
    | Name | `perimeter` **(1)** |
    | Starting address  | `10.50.1.0/24` **(2)** |

    ![image](../media/10-10-lab5-31.png)

1. In the Azure portal search bar, type **Route tables (1)** and select **Route tables (2)** from the search results.

     ![image](../media/10-10-lab5-32.png)

1. On the **Create Route table** page, provide the following details and click **Review + create (6)** to proceed and subsequently click on **Create**. 

    | Setting | Value | 
    | --- | --- |
    | Subscription | Default Subscription **(1)** |
    | Resource group | **az104-05-rg0--<inject key="DeploymentID" enableCopy="false" /> (2)**  |
    | Region | **East US (3)** |
    | Name | **az104-05-vm0 (4)** |
    | Propagate gateway routes | **No (5)** |

     ![image](../media/10-10-lab5-33.png)

1. After the route table deploys, select **Go to resource**.

    ![image](../media/10-10-lab5-34.png)

1. From the left navigation pane, under **Settings (1)** select **Routes (2)** and then **+ Add (3)**. Create a route from the future NVA to the CoreServices virtual network. 

    ![image](../media/10-10-lab5-35.png)

    | Setting | Value | 
    | --- | --- |
    | Route name | `PerimetertoCore` **(1)**|
    | Destination type | **IP Addresses (2)** |
    | Destination IP addresses | `10.50.0.0/22` (core services virtual network) **(3)** |
    | Next hop type | **Virtual appliance** (notice your other choices) **(4)** |
    | Next hop address | `10.50.1.7` (future NVA) **(5)** |

    ![image](../media/10-10-lab5-36.png)

1. Select **Add (6)** when the route is completed. The last thing to do is associate the route with the subnet.

1. Select **Subnets (1)** from the left navigation pane and then  click on **+ Associate (2)**. Complete the configuration.

    ![image](../media/10-10-lab5-37.png)

    | Setting | Value | 
    | --- | --- |
    | Virtual network | **az104-05-vnet0 (1)** |
    | Subnet | **subnet0 (2)** |  
    | Click | **Add (3)** |  

    ![image](../media/10-10-lab5-38.png)

     >**Note:** You have created a user-defined route to direct traffic from the DMZ to the new NVA.  

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
+ The traffic between virtual machines in peered virtual networks uses the Microsoft backbone infrastructure.
+ System defined routes are automatically created for each subnet in a virtual network. User-defined routes override or add to the default system routes. 
+ Azure Network Watcher provides a suite of tools to monitor, diagnose, and view metrics and logs for Azure IaaS resources.

### You have successfully completed the lab
