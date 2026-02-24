# Lab - Implement Data Protection

## Lab Overview

In this lab, you learn about backup and recovery of Azure virtual machines. You learn to create a Recovery Service vault and a backup policy for Azure virtual machines. You learn about disaster recovery with Azure Site Recovery. 

## Lab Objectives

In this lab, you will complete the following tasks:

+ Task 1: Use a template to provision an infrastructure.
+ Task 2: Create and configure a Recovery Services vault.
+ Task 3: Configure Azure virtual machine-level backup.
+ Task 4: Monitor Azure Backup.
+ Task 5: Enable virtual machine replication.  


### Task 1: Provision the lab environment

In this task, you will deploy two virtual machines that will serve as test environments to explore and evaluate various backup scenarios.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Deploy a custom template (1)**, and then select **Deploy a custom template (2)** under services.

    ![image](../media/7-10-lab4-12.png)

1. On the custom deployment page, select **Build you own template in the editor**.

   ![image](../media/7-10-lab4-13.png)

1. On the edit template page, select **Load file** option  from the top navigation pane.

   ![image](../media/7-10-lab3-15.png)

1. In the **Open** dialog box, navigate to **C:\AllFiles\AZ-104-MicrosoftAzureAdministrator-Lab-Files\Allfiles\Labs\10\ (1)**, select **az104-10-vms-edge-template.json (2)**, and click **Open (3)**.

    ![image](../media/14-10-lab10-2.png)

    >**Note:** Take a moment to review the template. We are deploying a virtual network and virtual machine so we can demonstrate backup and recovery. 

1. **Save** your changes.

     ![image](../media/14-10-lab10-3.png)

1. Select **Edit parameters** and then select the **Load file** option.

    ![image](../media/14-10-lab10-4.png)

    ![image](../media/14-10-lab10-5.png)

1. In the **Open** dialog box, navigate to **C:\AllFiles\AZ-104-MicrosoftAzureAdministrator-Lab-Files\Allfiles\Labs\10\ (1)**, select **az104-10-vms-edge-parameters.json (2)**, and click **Open (3)**.

     ![image](../media/14-10-lab10-6.png)

1. **Save** your changes.

     ![image](../media/14-10-lab10-7.png)

1. Use the following information to complete the custom deployment fields, leaving all other fields with their default values and select **Review + Create (5)**.

    | Setting       | Value         | 
    | ---           | ---           |
    | Subscription  | Leave it as the default subscription **(1)**|
    | Resource group| az104-10-rg1 **(2)** |
    | Region        | **<inject key="Region" enableCopy="false"/> (3)**    |
    | Admin Password      | **Password.11! (4)** |

     ![image](../media/up14-10-lab10-8.png)

1. On the **Review + create** tab, review the deployment details and then select **Create**.

     ![image](../media/14-10-lab10-9.png)

     >**Note:** Wait for the template to deploy, then select **Go to resource**. You should have one virtual machine in one virtual network. 

### Task 2: Create a Recovery Services vault

In this task, you will create a Recovery Services vault, an essential component for managing and safeguarding your backup and disaster recovery needs.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Recovery Services vaults (1)**, and then select **Recovery Services vaults (2)** under services.

    ![image](../media/14-10-lab10-10.png)

1. On the **Recovery Services vaults** page, select **+ Create** to start creating a new vault.

     ![image](../media/14-10-lab10-11.png)

1. On the **Create Recovery Services vault** blade, specify the following settings and click **Review + create (5)**.

    | Settings | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab **(1)**|
    | Resource group | Select resource group **az104-10-rg1 (2)** |
    | Vault Name | **az104-10-rsv1 (3)** |
    | Region | **<inject key="Region" enableCopy="false"/> (4)**  |

     ![image](../media/14-10-lab10-12.png)

     >**Note:** Make sure that you specify the same region into which you deployed virtual machines in the previous task.
     
1. Ensure that the validation has passed, and click **Create**.

     ![image](../media/14-10-lab10-13.png)

     >**Note:** Wait for the deployment to complete. The deployment should take less than 1 minute.

1. When the deployment is completed, click **Go to Resource**.

     ![image](../media/14-10-lab10-14.png)

1. On the **az104-10-rsv1** Recovery Services vault blade, in the left navigation pane in the  **Settings (1)** section, click **Properties (2)**.

1. On the **az104-10-rsv1 - Properties** blade, click the **Update (3)** link under **Backup Configuration** label.

     ![image](../media/14-10-lab10-15.png)

1. On the **Backup Configuration** blade, review the choices for **Storage replication type**. Leave the default setting of **Geo-redundant** in place and close the blade.

     ![image](../media/14-10-lab10-16.png)

     >**Note:** This setting can be configured only if there are no existing backup items.

1. Back on the **az104-10-rsv1 - Properties** blade, click the **Update** link under **Security Settings > Soft Delete Settings** label.

    ![image](../media/softdelt.png)

1. On the **Soft delete Settings** blade, note that the **soft delete retention period** is **14** days. 

    ![image](../media/retn.png)

1. Return to the Recovery Services vault blade, select the **Overview** blade.

>**Did you know?** Azure has two types of vaults: Recovery Services vaults and Backup vaults. The main difference is the datasources that can be backed up. Learn more about [the differences](https://learn.microsoft.com/answers/questions/405915/what-is-difference-between-recovery-services-vault).

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help

<validation step="4a350ebe-5f23-43de-a6f0-6794d3d9e7cd" />

### Task 3: Implement Azure virtual machine-level backup

In this task, you will implement Azure virtual-machine level backup to ensure data protection and recovery for your virtual machines.

   >**Note:** Before you start this task, make sure that the deployment you initiated in the first task of this lab has successfully completed. You can check that by going to the respected resource group in the Azure portal and on the overview page of the resource group click on **Deployments**.

1. On the **az104-10-rsv1** Recovery Services vault blade, click **Overview (1)**, then click **+ Backup (2)**.

     ![image](../media/14-10-lab10-18.png)

1. On the **Backup Goal** blade, specify the following settings:

    | Settings | Value |
    | --- | --- |
    | Where is your workload running? | **Azure (1)** |
    | What do you want to backup? | **Virtual machine (2)** |

1. Under **Configure Backup** , click **Backup (3)**.

     ![image](../media/14-10-lab10-19.png)

1. On **Configure backup** in Policy sub type click **Standard (1)** review the options.

1. On **Configure backup** in **Backup policy**, review the **DefaultPolicy** settings and select **Create a new policy (2)**.

     ![image](../media/14-10-lab10-20.png)

1. Define a new backup policy with the following settings (leave others with their default values):

    | Setting | Value |
    | ---- | ---- |
    | Policy name | **az104-10-backup-policy (1)** |
    | Frequency | **Daily (2)** |
    | Time | **12:00 AM (3)** |
    | Timezone | the name of your local time zone |
    | Retain instant recovery snapshot(s) for | **2** Days(s) **(4)** |

     - Click **OK (5)** to create the policy.

       ![image](../media/14-10-lab10-21.png)
  
1. In the **Virtual Machines** section, select **Add (1)**.

     - On the **Select virtual machines** blade, select **az-104-10-vm0 (2)**, click **OK (3)**.

       ![image](../media/14-10-lab10-23.png)
     
     - Back on the **Backup** blade, click **Enable backup**.

       ![image](../media/14-10-lab10-24.png)
  
        >**Note:** Wait for the backup to be enabled. This should take about 2 minutes.

1. Once deployment finish click on **Go to Resouces**.
   
    ![image](../media/14-10-lab10-25.png)

1. From the left navigation pane, in the **Protected items (1)** section, click **Backup items (2)**, and then under Backup Management type select the **Azure virtual machines (3)**  entry.

     ![image](../media/14-10-lab10-26.png)

1. On the **Backup Items (Azure Virtual Machine)** blade, select the **View details** link for **az104-10-vm0**, and review the values of the Backup Pre-Check and Last Backup Status entries.

     ![image](../media/14-10-lab10-27.png)

1. On the **az104-10-vm0** Backup Item blade, click **Backup now**.

     ![image](../media/14-10-lab10-28.png)

1. Accept the default value in the **Retain backup Till** drop-down list, and click **OK**.     

     ![image](../media/14-10-lab10-29.png)

     >**Note:** Do not wait for the backup to complete but instead proceed to the next task.

## Task 4: Monitor Azure Backup

In this task, you will deploy an Azure storage account. Then you will configure the vault to send the logs and metrics to the storage account. This repository can then be used with Log Analytics or other third-party monitoring solutions.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Storage accounts (1)**, and then select **Storage accounts (2)** under services.

     ![image](../media/14-10-lab10-30.png)

1. On the Storage accounts page, select **+ Create**.

     ![image](../media/r49.png)

1. Use the following information to define the storage account, click **Next (5)** and navigate to **Data protection** tab.

    | Settings | Value |
    | --- | --- | 
    | Subscription          | **Your subscription (1)**    |
    | Resource group        | **az104-10-rg1 (2)**       |
    | Storage account name  | **storage<inject key="DeploymentID" enableCopy="false"/> (3)**   |
    | Region                | **<inject key="Region" enableCopy="false"/> (4)**  |

     ![image](../media/up14-10-lab10-32.png)

1. On **Data Protection** tab, uncheck the **Enable soft delete for blobs (1)** check box then and select **Review + Create (2)**.

     ![image](../media/14-10-lab10-33.png)

1. On the Review + Create tab, select **Create**.

     ![image](../media/14-10-lab10-34.png)

     >**Note:** Wait for the deployment to complete. It should take about a minute.

1. In the Azure portal, locate and select the **Recovery Services vault** that was created in the previous task.

     ![image](../media/14-10-lab10-35.png)

1. From the left navigation pane,select **Diagnostic Settings (2)** under **Monitoring (1)** and then select **+ Add diagnostic setting (3)**.

     ![image](../media/14-10-lab10-36.png)

1. Name the setting as **Logs and Metrics to storage (1)**.

1. Place a checkmark next to the following log **(2)** and metric categories:

    - **Azure Backup Reporting Data**
    - **Addon Azure Backup Job Data**
    - **Addon Azure Backup Alert Data**
    - **Azure Site Recovery Jobs**
    - **Azure Site Recovery Events**

      ![image](../media/14-10-lab10-37.png)

1. In the **Destination details**, place a checkmark next to **Archive to a storage account (1)**. In the Storage account drop-down field, select the storage account **(2)**  that you deployed earlier in this task.

     ![image](../media/up14-10-lab10-38.png)

1. Select **Save**.

1. Return to your Recovery Services vault, in the **Monitoring (1)** blade select **Backup jobs (2)**.

1. Locate the backup operation for the **az104-10-vm0** virtual machine. 

1. Scroll to the right and click **View Details (3)** to review the backup job details.

     ![image](../media/14-10-lab10-39.png)

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help
   
<validation step="9516efe5-7bb7-4459-944a-9ca93b16bebc" />

## Task 5: Enable virtual machine replication

In this task, you will enable replication for a virtual machine to ensure business continuity and disaster recovery. 

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Recovery Services vaults (1)**, and then select **Recovery Services vaults (2)** under services.

     ![image](../media/14-10-lab10-10.png)

1. On the **Recovery Services vaults** page, click on **+ Create** to start creating a new vault. 

     ![image](../media/14-10-lab10-40.png)

1. On the **Create Recovery Services vault** blade, specify the following settings and click **Review + create (5)**

    | Settings | Value |
    | --- | --- |
    | Subscription | Leave it as the default subscription **(1)** |
    | Resource group |az104-10-rg1  **(2)**   |
    | Vault Name | **az104-10-rsv2 (3)**  |
    | Region | **West US 3 (4)** |

     ![image](../media/up14-10-lab10-41.png)

     >**Note:** Make sure that you specify a **different** region than the virtual machine.

1. On the **Review + create** tab, review the deployment details, select **Create**.

     ![image](../media/up14-10-lab10-42.png)

1. Wait for the deployment to complete. The deployment should take a couple of minutes. 

1. In the Azure portal, search for **Virtual Machine (1)** and select the **Virtual Machine (2)** resource.

     ![Image](./media/r36.png)

1. On the **Compute infrastructure | Virtual machines** page, click the **az104-10-vm0** virtual machine.

     ![image](../media/up14-10-lab10-43.png)

1. From the left navigation pane,in the **Backup + disaster recovery (1)** blade, select **Disaster recovery (2)**. 

     - On the **Basics** tab, notice the **Target region (3)**.

     - Move to the **Advanced settings (4)** tab. Resource selections have been made for you. It is important to review them. 

       ![image](../media/up14-10-lab10-45.1.png)

1. Verify your subscription, vm resource group, virtual network, and availability (take the default) settings.

1. In **Storage settings** select **Show details** and then provide the following details: 

    | Setting | Value |
    | ---- | ---- |
    | Churn for the vm | **Normal churn (1)**  |
    | Cache storage account | **storage<inject key="DeploymentID" enableCopy="false"/> (2)**  |

     ![image](../media/14-10-lab10-47.png)

   >**Note:** It is important that both of these settings be populated, or the validation will fail. If values are not present, try refreshing the page. If that doesn't work, create an empty storage account and then return to this page.

1. In **Replication settings** select **Show details**. Notice your recovery resources vault 2 was automatically selected.

     ![image](../media/14-10-lab10-48.png)

1. Scroll down for **Automation account** accept the default value and click on **Create**

    ![image](../media/14-10-lab10-49.png)
   
1. Select **Review + Start replication**.

     ![image](../media/14-10-lab10-50.png)

1. Then **Start replication**.     

     ![image](../media/14-10-lab10-51.png)

     >**Note:** `Enabling replication will take a 15-20 minutes. Watch the notification messages in the upper right of the portal. While you wait, consider reviewing the self-paced training links at the end of this page`.
    
1. Once the replication is complete, search for and locate your Recovery Services Vault, **az104-10-rsv2**. You may need to **Refresh** the page. 

     ![image](../media/14-10-lab10-52.png)

1. From the left navigation pane, in the **Protected items (1)** section, select **Replicated items (2)**.

    ![image](../media/14-10-lab10-53.png)

1. Check that the virtual machine is showing as healthy for the replication health. Note that the status will show the synchronization (starting at 0%) status and ultimately show **Protected** after the initial synchronization completes.

    ![image](../media/14-10-lab10-54-new.png)

1. Select the Virtual machine to view more details.
   
>**Did you know?** It is a good practice to [test the failover of a protected VM](https://learn.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure#run-a-test-failover-for-a-single-vm).

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help

<validation step="3e780a0f-ab1a-48d9-a0a0-996c526c7c12" />

### Review

In this lab, you have completed the following:

- Used a template to provision an infrastructure.
- Created and configure a Recovery Services vault.
- Configured Azure virtual machine-level backup.
- Monitored Azure Backup.
- Enabled virtual machine replication.

## Extend your learning with Copilot
Copilot can assist you in learning how to use the Azure scripting tools. Copilot can also assist in areas not covered in the lab or where you need more information. Open an Edge browser and choose Copilot (top right) or navigate to *copilot.microsoft.com*. Take a few minutes to try these prompts.

+ What products does Azure Backup support?
+ Summarize the steps for backing up and restoring an Azure virtual machine with Azure Backup.
+ How can I use Azure PowerShell or the CLI to check the status of an Azure Backup job.
+ Provide at least five best practices for configuring Azure virtual machine backups.

## Learn more with self-paced training

+ [Protect your virtual machines by using Azure Backup](https://learn.microsoft.com/training/modules/protect-virtual-machines-with-azure-backup/). Use Azure Backup to help protect on-premises servers, virtual machines, SQL Server, Azure file shares, and other workloads.
+ [Protect your Azure infrastructure with Azure Site Recovery](https://learn.microsoft.com/en-us/training/modules/protect-infrastructure-with-site-recovery/). Provide disaster recovery for your Azure infrastructure by customizing replication, failover, and failback of Azure virtual machines with Azure Site Recovery.

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 

+ Azure Backup service provides simple, secure, and cost-effective solutions to back up and recover your data.
+ Azure Backup can protect on-premises and cloud resources including virtual machines and file shares.
+ Azure Backup policies configure the frequency of backups and the retention period for recovery points. 
+ Azure Site Recovery is a disaster recovery solution that provides protection for your virtual machines and applications.
+ Azure Site Recovery replicates your workloads to a secondary site, and in the event of an outage or disaster, you can failover to the secondary site and resume operations with minimal downtime.
+ A Recovery Services vault stores your backup data and minimizes management overhead.


### You have successfully completed the lab
