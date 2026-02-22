# Lab - Manage Azure resources by using Azure Resource Manager Templates

## Lab Overview
 
 In this hands-on lab, you will learn to manage Azure resources using ARM templates, automating resource deployment and management. You'll explore editing, redeploying templates, and deploying resources using Azure PowerShell, CLI, and Bicep. 
 
## Lab Objectives

-  Task 1: Create an Azure Resource Manager template.
-  Task 2: Edit an Azure Resource Manager template and redeploy the template.
-  Task 3: Configure the Cloud Shell and deploy a template with Azure PowerShell.
-  Task 4: Deploy a template with the CLI. 
-  Task 5: Deploy a resource by using Azure Bicep.


### Task 1: Create an Azure Resource Manager template

In this task, we will create a managed disk in the Azure portal. Managed disks are storage designed to be used with virtual machines. Once the disk is deployed you will export a template that you can use in other deployments.

1. In Search resources, services, and docs (G+/) box at the top of the portal, enter **Disks (1)**, and then select **Disks (2)** from the results.

   ![image](../media/7-10-lab3-6.png)

1. On the **Storage center | Disks** page, select **+ Create**.

    ![image](../media/7-10-lab3-7.png)

1. On the **Create a managed disk** page, configure the disk with the following details: 
    
    | Setting | Value |
    | --- | --- |
    | Subscription | **your default subscription (1)** | 
    | Resource Group | **az104-03b-rg1-<inject key="DeploymentID" enableCopy="false" /> (2)** |
    | Disk name | **az104-03b-disk1 (3)** | 
    | Region | **<inject key="Region" enableCopy="false" /> (4)**|
    | Availability zone | **No infrastructure redundancy required (5)** | 
    | Source type | **None (6)**|
    | Size | Click on **change size (7)** link |
    -------------------------------------------------------------------------------------------------------------------------------------
    ![image](../Labs/media/r17.png)

    - Select a disk size: Under **Storage type** select **Standard HDD (8)** then select **32 GIB (9)** and click on **OK (10)**. |

      ![image](../media/7-10-lab3-9.png)

    - Then select **Review + create**.

      ![image](../Labs/media/r18.png)

       >**Note:** We are creating a simple managed disk so you can practice with templates. Azure managed disks are block-level storage volumes that are managed by Azure.

1. Click **Create**.

    ![image](../Labs/media/r20.png)

1. Monitor the notifications (upper right) and after the deployment select **Go to resource**. 

     ![image](../media/7-10-lab3-12.png)

1. In the **Automation (1)** blade, select **Export template (2)**. 

    ![image](../media/7-10-lab3-13.png)

1. Take a minute to review the **Template** and **Parameters** files.

1. From the **Template** section, click **Download** and save the template to the local drive.  

    ![image](../media/lab3-02-1.png)

1. Then switch to the **Parameters** section and do the same.    

1. In File Explorer open the **Downloads** folder on LabVM. Notice there are two JSON files (template and parameters). 

     ![image](../media/lab3-02-2.png)

     >**Did you know?**  You can export an entire resource group or just specific resources within that resource group.

## Task 2: Edit an Azure Resource Manager template and then redeploy the template

In this task, you will utilize the previously downloaded Azure Resource Manager (ARM) template to deploy a new managed disk. The template, which includes the configuration and settings for the disk, allows for rapid and consistent deployment of resources.

1. In Search resources, services, and docs (G+/) box at the top of the portal, enter **Deploy a custom template (1)**, and then select **Deploy a custom template (2)** from the results.

   ![image](../media/7-10-lab4-12.png)
   
1. On the **Custom deployment** blade, notice there is the ability to use a **Quickstart template**. There are many built-in templates as shown in the drop-down menu. 

1. Instead of using a Quickstart, select **Build your own template in the editor**.

     ![image](../media/7-10-lab4-13.png)

1. On the **Edit template** blade, click **Load file** and upload the **template.json** file you downloaded to the local disk.

    ![image](../media/7-10-lab3-15.png)

1.  In the **Open** dialog box, navigate to **Downloads (1)** and select the **template (2)** file and click **Open (3)**.

    ![image](../Labs/media/r19.png)

1. Within the editor pane, make these changes.

    -  Change **disks_az104_03b_disk1_name** to `disk_name` (two places to change line number `5` and `15`)

       ![image](../media/7-10-lab3-17.png)
    
    - Change **az104-03b-disk1** to **az104-03b-disk2 (1)** (one place to change line number 6)

    - Click on **Save (2)** your changes.

      ![image](../media/7-10-lab3-19.png)

1. Don't forget the parameters file. Select **Edit parameters**.

    ![image](../media/7-10-lab3-20.png)

1. Click **Load file** and upload the **parameters.json**.     

    ![image](../media/7-10-lab3-21.png)

1. Make this change so it matches the template file.

    Change **disks_az104_03b_disk1_name** to **disk_name** (one place to change)

     ![image](../media/7-10-lab3-21.png)

1. **Save** your changes. 

1. Complete the custom deployment settings:

    | Setting | Value |
    | --- |--- |
    | Subscription | **your subscription (2)** |
    | Resource Group | **az104-03b-rg1-<inject key="DeploymentID" enableCopy="false" /> (2)** |
    | Region | **<inject key="Region" enableCopy="false" /> (3)** |
    | Disk_name | **az104-03b-disk2 (4)**|

1. Select **Review + Create (5)** and then select **Create**.

    ![image](../media/lab3-02-6.png)

1. Select **Go to resource**. Verify **az104-03b-disk2** was created.

1. On the **Overview** blade, select the resource group, **az104-03b-rg1-<inject key="DeploymentID" enableCopy="false" />**. You should now have two disks.

      ![image](../media/7-10-lab3-23.png)

1. In the **Settings (1)** section, click **Deployments (2)**.

     ![image](../media/7-10-lab3-24.png)

   >**Note:** All deployments details are documented in the resource group. It is a good practice to review the first few template-based deployments to ensure success prior to using the templates for large-scale operations.

## Task 3: Configure the Cloud Shell and deploy a template with Azure PowerShell

In this task, you work with the Azure Cloud Shell and Azure PowerShell. Azure Cloud Shell is an interactive, authenticated, browser-accessible terminal for managing Azure resources. It provides the flexibility of choosing the shell experience that best suits the way you work, either Bash or PowerShell. In this task, you use PowerShell to deploy a template. 

1. Select the **Cloud Shell (1)** icon in the top right of the Azure Portal. 

1. When prompted to select either **Bash** or **PowerShell**, select **PowerShell (2)**. 

    ![image](../media/7-10-lab3-25.png)

   >**Did you know?**  If you mostly work with Linux systems, Bash (CLI) feels more familiar. If you mostly work with Windows systems, Azure PowerShell feels more familiar. 

1. In the **Getting started** window, select **Mount storage account (1)**, choose the **subscription (2)** from the dropdown, and click **Apply (3)** to continue.

     ![image](../media/7-10-lab3-26.png)

1. On mount storage account page, select **I want to create a storage account (1)**, click on **Next (2)**.

    ![image](../media/7-10-lab3-27.png)

1. Provide the below details to create the storage account and click on **Create (6)**.

    >**Note:** As you work with the Cloud Shell a storage account and file share is required. 

    | Settings | Values |
    |  -- | -- |
    | Subscription | Accept default **(1)**|
    | Resource Group | **az104-03b-rg1-<inject key="DeploymentID" enableCopy="false" /> (2)** |
    | Region | **<inject key="Region" enableCopy="false" /> (3)** |
    | Storage account (Create new) | **str<inject key="DeploymentID" enableCopy="false" /> (4)** |
    | File share (Create new) | **none (5)** |

    ![image](../Labs/media/r22.png)

1. In the Cloud Shell toolbar, open the **Settings (1)** menu and choose **Go to Classic version (2)** from the drop-down.

     ![image](../media/7-10-lab3-30.png)

1. Select the **Upload/Download (1)** files icon (top bar) and then select **Upload (2)**.

      ![image](../media/7-10-lab3-31.png)

 1. Upload the **template** and **parameters** file from the downloads directory. `You will need to upload each file separately one after the another..`.      

      ![image](../media/lab3-02-3.png)

1. Verify your files are available in the Cloud Shell storage. 

    ```powershell
    dir
    ```
    ![image](../media/7-10-lab3-32.png)

    >**Note:** If you need to, you can use **cls** to clear the command window. You can use the arrow keys to move the command history.

1. Select the **Editor (1)** (curly brackets) icon and navigate to the template JSON file.

1. Select **template.json (2)** and make a change. For example, change the disk name to **az104-03b-disk3 (3)**. Use **Ctrl +S** to save your changes.

    ![image](../media/7-10-lab3-33.png)

    >**Note:** You can target your template deployment to a resource group, subscription, management group, or tenant. Depending on the scope of the deployment, you use different commands.

1. To deploy to a resource group, use **New-AzResourceGroupDeployment**.

    ```powershell
    New-AzResourceGroupDeployment -ResourceGroupName az104-03b-rg1-<inject key="DeploymentID" enableCopy="false"/> -TemplateFile template.json -TemplateParameterFile parameters.json
    ```

    ![image](../media/7-10-lab3-34.png)

1. Ensure the command completes and the ProvisioningState is **Succeeded**.

1. Confirm the disk was created.

   ```powershell
   Get-AzDisk | ft
   ```

   ![image](../media/7-10-lab3-35.png)

## Task 4: Deploy a template with the CLI 

In this task, you will deploy an Azure Resource Manager (ARM) template using the Command-Line Interface (CLI). The Azure CLI provides a powerful, scriptable interface to interact with Azure resources.

1. In the **Cloud Shell**, click the drop-down arrow next to **PowerShell (1)** and select **Bash (2)**.

    ![image](../media/7-10-lab3-36.png)

1. When prompted to switch, click **Confirm** to continue using **Bash** in Cloud Shell.

    ![image](../media/7-10-lab3-37.png)

1. Verify your files are available in the Cloud Shell storage. If you completed the previous task your template files should be available. 

    ```sh
    ls
    ```
     ![image](../media/7-10-lab3-38.png)

1. Select the **Editor (1)** (curly brackets) icon and navigate to the template JSON file.

1. Make a change. For example, change the disk name to **az104-03b-disk4 (2)**. Use **Ctrl +S** to save your changes. 

   ![image](../media/7-10-lab3-39.png)

    >**Note:** You can target your template deployment to a resource group, subscription, management group, or tenant. Depending on the scope of the deployment, you use different commands.

1. To deploy to a resource group, use **az deployment group create**.

   ```sh
    az deployment group create --resource-group az104-03b-rg1-<inject key="DeploymentID" enableCopy="false"/> --template-file template.json --parameters parameters.json
    ```    
     ![image](../media/7-10-lab3-40.png)

1. Ensure the command completes and the ProvisioningState is **Succeeded**.

1. Confirm the disk was created.

     ```sh
     az disk list --resource-group az104-03b-rg1-<inject key="DeploymentID" enableCopy="false"/> --output table
     ```

    ![image](../media/lab3-02-4.png)

## Task 5: Deploy a resource by using Azure Bicep

In this task, you will use a Bicep file to deploy a managed disk. Bicep is a declarative automation tool that is built on ARM templates.

1. Close and reopen **Cloud Shell** in a **Bash** session.

1. In the **Cloud Shell** toolbar, click the **Manage files (1)** drop-down and select **Upload (2)**.

    ![image](../media/7-10-lab3-42.png)

1. In the **Open** dialog box, browse to the path **C:\AllFiles\AZ-104-MicrosoftAzureAdministrator-Lab-Files\Allfiles\Labs\03 (1)**, select **azuredeploydisk.bicep (2)**, and click **Open (3)** to upload the file..

   ![image](../media/7-10-lab3-43.png)

1. Select **Editor (1)** and click **Confirm (2)** on **Switch to classsic Cloud Shell**.

    ![image](../media/7-10-lab3-44.png)

1. Select the **Editor** (curly brackets) icon and navigate to **azuredeploydisk.bicep** file.

1. Take a minute to read through the bicep template file. Notice how the disk resource is defined. 
   
1. Make the following changes:

   - Change the **managedDiskName** value to **az104-03b-disk5 (1)** .
   - Change the **diskSizeinGiB** value to **32 (2)**.
   - Change the **sku name** value to `StandardSSD_LRS` **(3)**.

     ![image](../media/7-10-lab3-45.png)

1. Use **Ctrl +S** to save your changes.

1. Now, deploy the template.

    ```
    az deployment group create --resource-group az104-03b-rg1-<inject key="DeploymentID" enableCopy="false"/> --template-file azuredeploydisk.bicep
    ```

    ![image](../media/7-10-lab3-46.png)

1. Confirm the disk was created.

    ```sh
    az disk list --resource-group az104-03b-rg1-<inject key="DeploymentID" enableCopy="false"/> --output table
    ```

    ![image](../media/lab3-02-5.png)


  > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
  > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next  task. 
  > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
  > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help

<validation step="95c0111f-ab42-4cdd-a5ca-abd36982cc45" />

### Review

In this lab, you have completed the following:

- Created an Azure Resource Manager (ARM) template: You initiated the process by creating a template to define the resources, such as managed disks, in your Azure environment.
- Edited and redeployed an Azure Resource Manager (ARM) template: You modified the initial template to change specific parameters, such as disk names, and redeployed it to replicate resource creation easily.
- Configured the Cloud Shell and deployed a template using Azure PowerShell: You set up the Azure Cloud Shell, configured it for PowerShell, and deployed a resource template using PowerShell commands.
- Deployed a template using the Azure Command-Line Interface (CLI): You used the Azure CLI in Cloud Shell to deploy the template, practicing automation through command-line tools.
- Deployed a resource using Azure Bicep: You utilized Azure Bicep, a declarative language, to deploy resources and modify the template for a more efficient and scalable deployment process.

## Extend your learning with Copilot

Copilot can assist you in learning how to use the Azure scripting tools. Copilot can also assist in areas not covered in the lab or where you need more information. Open an Edge browser and choose Copilot (top right) or navigate to *copilot.microsoft.com*. Take a few minutes to try these prompts.

+ What is the format of the Azure Resource Manager template file? Explain each component with examples. 
+ How do I use an existing Azure Resource Manager template?
+ Compare and contrast Azure Resource Manager templates and Azure Bicep templates. 


## Learn more with self-paced training

+ [Deploy Azure infrastructure by using JSON ARM templates](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/). Write JSON Azure Resource Manager templates (ARM templates) by using Visual Studio Code to deploy your infrastructure to Azure consistently and reliably.
+ [Review the features and tools for Azure Cloud Shell](https://learn.microsoft.com/training/modules/review-features-tools-for-azure-cloud-shell/). Cloud Shell features and tools. 
+ [Manage Azure resources with Windows PowerShell](https://learn.microsoft.com/training/modules/manage-azure-resources-windows-powershell/). This module explains how to install the necessary modules for cloud services management and use PowerShell commands to perform simple administrative tasks on cloud resources like Azure virtual machines, Azure subscriptions and Azure storage accounts.
+ [Introduction to Bash](https://learn.microsoft.com/training/modules/bash-introduction/). Use Bash to manage IT infrastructure.
+ [Build your first Bicep template](https://learn.microsoft.com/training/modules/build-first-bicep-template/). Define Azure resources within a Bicep template. Improve the consistency and reliability of your deployments, reduce the manual effort required, and scale your deployments across environments. Your template will be flexible and reusable by using parameters, variables, expressions, and modules.

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 

+ Azure Resource Manager templates let you deploy, manage, and monitor all the resources for your solution as a group, rather than handling these resources individually.
+ An Azure Resource Manager template is a JavaScript Object Notation (JSON) file that lets you manage your infrastructure declaratively rather than with scripts.
+ Rather than passing parameters as inline values in your template, you can use a separate JSON file that contains the parameter values.
+ Azure Resource Manager templates can be deployed in a variety of ways including the Azure portal, Azure PowerShell, and CLI.
+ Bicep is an alternative to Azure Resource Manager templates. Bicep uses a declarative syntax to deploy Azure resources.
+ Bicep provides concise syntax, reliable type safety, and support for code reuse. Bicep offers a first-class authoring experience for your infrastructure-as-code solutions in Azure.

### You have successfully completed the lab
