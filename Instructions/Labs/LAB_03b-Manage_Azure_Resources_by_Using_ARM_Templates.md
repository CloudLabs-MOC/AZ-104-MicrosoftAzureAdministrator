
# Lab 03b - Manage Azure resources by Using ARM Templates
# Student lab manual

## Lab scenario
Now that you explored the basic Azure administration capabilities associated with provisioning resources and organizing them based on resource groups by using the Azure portal, you need to carry out the equivalent task by using Azure Resource Manager templates.

## Objectives

In this lab, you will:

+ Task 1: Review an ARM template for deployment of an Azure managed disk
+ Task 2: Create an Azure managed disk by using an ARM template
+ Task 3: Review the ARM template-based deployment of the managed disk

## Architecture diagram

![image](../media/lab03b.png)

## Instructions

#### Task 1: Review an ARM template for deployment of an Azure managed disk

In this task, you will create an Azure disk resource by using an Azure Resource Manager template.

1. Sign in to the [**Azure portal**](https://portal.azure.com).

1. In the Azure portal, search for and select **Resource groups**. 

1. In the list of resource groups, click **az104-03a-rg1**.

     ![image](../media/selectrg1.png)

1. On the **az104-03a-rg1** resource group blade, in the **Settings** section, click **Deployments(1)**.

1. On the **az104-03a-rg1 - Deployments** blade, click the first entry in the list of deployments.

1. On the **Microsoft.ManagedDisk-*YYYYMMDD-DeploymentID*(2) \| Overview** blade, click **Template**.

     >**Note**: Review the content of the template and note that you have the option to **Download** it to the local computer, **Add to library**, or **Deploy** it again.

     ![image](../media/deployement.png)
     
     ![image](../media/clicktemplete.png)

1. Click **Download** and save the compressed file containing the template and parameters files to the **Downloads** folder on your lab computer.

     ![image](../media/download.png)

1. On the **Microsoft.ManagedDisk-*YYYYMMDD-DeploymentID* \| Template** blade, click **Inputs(1)**.

1. Note the value of the **location(2)** parameter. You will need it in the next task.

     ![image](../media/input.png)

1. Extract the content of the downloaded file into the **Downloads** folder on your lab computer.

    >**Note**: These files are also available as **\\Allfiles\\Labs\\03\\az104-03b-md-template.json** and **\\Allfiles\\Labs\\03\\az104-03b-md-parameters.json**
    
1. Close all **File Explorer** windows.

#### Task 2: Create an Azure managed disk by using an ARM template

1. In the Azure portal, search for and select **Deploy a custom template**.

      ![image](../media/searchforcustomtempete.png)

1. Click **Template deployment (deploy using custom templates)** found under the **Marketplace** group.

1. On the **Custom deployment** blade, click **Build your own template in the editor**.

      ![image](../media/buildyourown.png)

1. On the **Edit template** blade, click **Load file** and upload the **template.json** file you downloaded in the previous task.

      ![image](../media/loadfile.png)
      
      ![image](../media/templete.png)

1. Within the editor pane, remove the following lines:

   ```json
   "sourceResourceId": {
       "type": "String"
   },
   "sourceUri": {
       "type": "String"
   },
   "osType": {
       "type": "String"
   },
   ```

   ```json
   "hyperVGeneration": {
       "defaultValue": "V1",
       "type": "String"
   },      
   ```

   ```json
   "osType": "[parameters('osType')]",
   ```

    >**Note**: These parameters are removed since they are not applicable to the current deployment. In particular, sourceResourceId, sourceUri, osType, and hyperVGeneration parameters are applicable to creating an Azure disk from an existing VHD file.

     ![image](../media/editorpan.png)
     
     ![image](../media/hypervgeneration.png)
     
 1. **Save** the changes.

1. Back on the **Custom deployment** blade, click **Edit parameters**. 

     ![image](../media/editparameters.png)

1. On the **Edit parameters** blade, click **Load file** and upload the **parameters.json** file you downloaded in the previous task, and **Save** the changes.

     ![image](../media/loadfile1.png)
     
     ![image](../media/parameter.png)
     
     ![image](../media/saveparameter.png)

1. Back on the **Custom deployment** blade, specify the following settings:

    | Setting | Value |
    | --- |--- |
    | Subscription | *the name of the Azure subscription you are using in this lab* |
    | Resource Group | the name of a **new** resource group **az104-03b-rg1(1)** |
    | Region | the name of any Azure region available in the subscription you are using in this lab |
    | Disk Name | **az104-03b-disk1(2)** |
    | Location | the value of the location parameter you noted in the previous task |
    | Sku | **Standard_LRS(4)** |
    | Disk Size Gb | **32(5)** |
    | Create Option | **empty(6)** |
    | Disk Encryption Set Type | **EncryptionAtRestWithPlatformKey(7)** |
    | Network Access Policy | **AllowAll(8)** |
    
    ![image](../media/customdeployement.png)

1. Select **Review + Create(9)** and then select **Create**.

    ![image](../media/createdeployment.png)

1. Verify that the deployment completed successfully.

#### Task 3: Review the ARM template-based deployment of the managed disk

1. In the Azure portal, search for and select **Resource groups**. 

1. In the list of resource groups, click **az104-03b-rg1**.

    ![image](../media/3brg1.png)

1. On the **az104-03b-rg1** resource group blade, in the **Settings** section, click **Deployments(1)**.

1. From the **az104-03b-rg1 - Deployments** blade, click the first entry in the list of deployments and review the content of the **Input** and **Template** blades.

   ![image](../media/deploement3b.png)

#### Clean up resources

   >**Note**: Do not delete resources you deployed in this lab. You will reference them in the next lab of this module.

#### Review

In this lab, you have:

- Reviewed an ARM template for deployment of an Azure managed disk
- Created an Azure managed disk by using an ARM template
- Reviewed the ARM template-based deployment of the managed disk
