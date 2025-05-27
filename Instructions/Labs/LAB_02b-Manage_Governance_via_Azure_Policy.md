# Lab 03a: Manage Azure resources by using the Azure Portal 

## Lab Overview

In this lab, you will implement management groups to organize your Azure subscriptions for centralized governance and access control. You'll create custom RBAC roles to define specific permissions, ensuring users only access needed resources. These roles will be assigned based on job responsibilities to enforce security and compliance. Finally, you'll use the Activity Log to monitor role assignments, track changes, and ensure adherence to access control policies.

## Lab objectives
In this lab, you will complete the following tasks:
+ Task 1: Create and assign tags via the Azure portal.
+ Task 2: Enforce tagging via an Azure Policy.
+ Task 3: Apply tagging via an Azure Policy.
+ Task 4: Configure and test resource locks. 

## Exercise 2: Manage Azure Resources

In this exercise, you will learn to manage governance via Azure Policy, which involves defining and enforcing rules that govern the resources in your Azure environment to ensure compliance with organizational standards.

### Task 1: Assign tags via the Azure portal

In this task, you will learn how to create and assign a tag to an Azure resource group through the Azure portal. Tags in Azure are key-value pairs that help in organizing and categorizing resources for better management and reporting.

1. On the Azure Portal page, in the **Search resources, services and docs (G+/)** box at the top of the portal, search and select **Resource group** under services. Select **Resource group AZ-104T02** from the list.

   ![image](./media/l2-image45.png)

1. On the resource group blade, click **Tags** and create a tag with the following settings, and click on **Apply** to save your change:

    | Setting | Value |
    | --- | --- |
    | Name | **Role** |
    | Value | **Infra** |

   ![image](./media/l2-image27.png)
   
### Task 2: Enforce tagging via an Azure policy

In this task, you will explore how to enforce governance policies by assigning the built-in Require a tag and its value on the  resources policy to a specific Azure resource group. This policy ensures that all resources created within the resource group are tagged with a predefined key-value pair, promoting consistency and compliance with organizational standards.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Policy**, and then select **Policy** under services.

   ![image](./media/l2-image28.png)

1. In the **Authoring** section, click **Definitions**. Take a moment to browse through the list of built-in policy definitions that are available for you to use. List all built-in policies that involve the use of tags by selecting the **Tags** entry (and deselecting all other entries) in the **Category** drop-down list and clicking on **Apply**.

   ![image](./media/l2-image29.png)

1. In the search bar, enter **require** and click the entry representing the **Require a tag and its value on resources** built-in policy, and review its definition.

    ![image](./media/l2-image30.png)
   
1. On the **Require a tag and its value on resources group** built-in policy definition blade, click **Assign policy**.

    ![image](./media/lab02-new-2.png)

1. Specify the **Scope** by clicking the ellipsis button and selecting the following values, and choose **Select** :

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource Group | AZ-104T02 |

   ![image](./media/lab02-new-3.png)
   
   >**Note**: A scope determines the resources or resource groups where the policy assignment takes effect. You could assign policies on the management group, subscription, or resource group level. You also have the option of specifying exclusions, such as individual subscriptions, resource groups, or resources (depending on the assignment scope). 

1. Configure the **Basics** properties of the assignment by specifying the following settings and click on **Next**  (leave others with their defaults):

    | Setting | Value |
    | --- | --- |
    | Assignment name | **Require Role tag with Infra value**|
    | Description | **Require Role tag with Infra value for all resources in the Cloud Shell resource group**|
    | Policy enforcement | Enabled |

    ![image](./media/lab02-new-4.png)
   
    >**Note**: The **Assignment name** is automatically populated with the policy name you selected, but you can change it. You can also add an optional **Description**. **Assigned by** is automatically populated based on the user name creating the assignment. 

1. Set **Parameters** to the following values:

    | Setting | Value |
    | --- | --- |
    | Tag Name | **Role** |
    | Tag Value | **Infra** |

   ![image](./media/lab02-new-7.png)
   
1. Click **Next** and review the **Remediation** tab. Leave the **Create a Managed Identity** checkbox unchecked. 

    >**Note**: This setting can be used when the policy or initiative includes the **deployIfNotExists** or **Modify** effect.

1. Click **Review + create** and then click **Create**.

    >**Note**: Now you will verify that the new policy assignment is in effect by attempting to create another Azure Storage account in the resource group without explicitly adding the required tag. 
    
    >**Note**: It might take between 5 and 15 minutes for the policy to take effect.

1. On the Azure Portal page, in the **Search resources, services and docs (G+/)** box at the top of the portal, search and select **Storage accounts** under services, and then click **+ Create**.

1. On the **Basics** tab of the **Create storage account** blade, verify that you are using the Resource Group that the Policy was applied to and specify the following settings (leave others with their defaults), click **Review + create**, and then click **Create**:

    | Setting | Value |
    | --- | --- |
    | Storage account name | storage<inject key="DeploymentID" enableCopy="false"/> |

1. Once you create the deployment, you should see the **Validation failed. Required information is missing or not valid** message.

    ![image](./media/l2-image35.png)

1. Verify whether the error message states that the resource deployment was disallowed by the policy by clicking the **Previous** tags tab and selecting the **Policy details** link to review the details.

   ![image](./media/lab02-new-5.png)

    >**Note**: You can find more details about the error, including the name of the role definition **Require Role tag with Infra value**. The deployment failed because the storage account you attempted to create did not have a tag named **Role** with its value set to **Infra**.

### Task 3: Apply tagging via an Azure policy

In this task, you will focus on identifying and remediating non-compliant resources by leveraging a different Azure Policy definition. Non-compliant resources are those that do not meet the criteria defined in your organization's governance policies, such as missing mandatory tags or violating security configurations. 

1. In the Azure portal, search for and select **Policy**. 

   ![image](./media/l2-image28.png)

1. In the list of assignments, right-click the ellipsis icon in the row representing the **Require Role tag with Infra value** policy assignment and use the **Delete assignment** menu item to delete the assignment, and then select **Yes**.

   ![image](./media/l2-image66.png)

1. In the **Authoring** section, click **Definitions**. Take a moment to browse through the list of built-in policy definitions that are available for you to use. List all built-in policies that involve the use of tags by selecting the **Tags** entry (and deselecting all other entries) in the **Category** drop-down list and clicking on **Apply**.

1. In the search bar, enter **Inherit** and click the entry representing the **Inherit a tag from the resource group if missing** built-in policy, and review its definition.

1. On the **Inherit a tag from the resource group if missing** built-in policy definition blade, click **Assign Policy**.

1. Click **Assign policy** and specify the **Scope** by clicking the ellipsis button and selecting the following values, and choose **Select**:

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource Group | AZ-104T02 |

1. To specify the **Policy definition**, click the ellipsis button and then search for and select **Inherit a tag from the resource group if missing**, then click on **Add** if not selected in the definition.

    ![image](./media/l2-image37.png)

    ![image](./media/l2-image38.png)

   >**Note**: You can ignore the above step if the policy definition has appeared automatically.

1. Configure the remaining **Basics** properties of the assignment by specifying the following settings (leave others with their defaults) and click on **Next**.

    | Setting | Value |
    | --- | --- |
    | Assignment name | **Inherit the Role tag and its Infra value from the Cloud Shell resource group if missing**|
    | Description | **Inherit the Role tag and its Infra value from the Cloud Shell resource group if missing**|
    | Policy enforcement | Enabled |

    ![image](./media/lab02-new-8.png)

1. Click **Next** and set **Parameters** to the following values:

    | Setting | Value |
    | --- | --- |
    | Tag Name | **Role** |

    ![image](./media/lab02-new-9.png)
   
1. Click **Next** and, on the **Remediation** tab, configure the following settings (leave others with their defaults) and click **Review + Create**.

    | Setting | Value |
    | --- | --- |
    | Create a remediation task | enabled |
    | Policy to remediate | **Inherit a tag from the resource group if missing** |

    >**Note**: This policy definition includes the **Modify** effect.

    ![image](./media/lab02-new-10.png)
  
1. Click  **Create**.

    >**Note**: To verify that the new policy assignment is in effect, you will create another Azure Storage account in the same resource group without explicitly adding the required tag. 
    
    >**Note**: It might take between 5 and 15 minutes for the policy to take effect.

1. On the Azure Portal page, in the **Search resources, services and docs (G+/)** box at the top of the portal, search and select **Storage accounts** under services, and then click **+ Create**. 

1. On the **Basics** tab of the **Create storage account** blade, verify that you are using the Resource Group that the Policy was applied to, and specify the following settings (leave others with their defaults) and click **Review + create**:

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab |
    | Resource Group | AZ-104T02 |
    | Storage account name |  **storage<inject key="DeploymentID" enableCopy="false"/>** |
    | Redundancy |  **Locally-redundant storage (LRS)** |

    ![image](./media/lab02-new-11.png)
   
1. Verify that this time the validation passed and click **Create**.

   >**Note**: If the validation fails, kindly wait for some time as it might take some time for the policy to take effect for the validation to pass through.

1. Once the new storage account is provisioned, click the **Go to resource** button and, on the **Overview** blade of the newly created storage account, note that the tag **Role** with the value **Infra** has been automatically assigned to the resource.

   ![image](./media/l2-image43.png)
    
   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at labs-support@spektrasystems.com. We are available 24/7 to help

   <validation step="7dd291d1-35d1-40bb-857b-8ea685202e58" />

## Task 4: Configure and test resource locks

In this task, you will configure and test a resource lock to safeguard critical Azure resources from unintended changes or deletions. Resource locks are a powerful governance feature in Azure.

1. Search for and select your resource group **AZ-104T02**.
   
1. In the **Settings** blade, select **Locks**.

1. Select **+ Add** and complete the resource lock information. When finished, select **Ok**. 

    | Setting | Value |
    | --- | --- |
    | Lock name | `rg-lock` |
    | Lock type | **Delete** (notice the selection for read-only) |
    
1. Navigate to the resource group **Overview** blade, and select **Delete resource group**.

   ![image](./media/l2-image62.png)

1. In the **Enter resource group name to confirm deletion** textbox, provide the resource group name, `AZ-104T02`. Notice you can copy and paste the resource group name and click on **Delete** twice. 

   ![image](./media/l2-image63.png)
   
1. You should receive a notification denying the deletion. 

    ![image](./media/l2-image64.png)

1. From the **AZ-104T02** resource group **Overview** blade, under **Settings** section select **Locks** and Select Locks and proceed to click **Delete** to remove the existing **rg-lock** locks.

   ![image](./media/l2-image65.png)

### Review

In this lab, you have completed:

- Created and assigned tags to resource groups using the Azure portal.
- Enforced tagging compliance by assigning an Azure Policy.
- Applied tagging to resources automatically using Azure Policy.
- Configured and validated resource locks to prevent accidental changes or deletions.

### You have successfully completed the lab
