# Lab : Manage Governance via Azure Policy

## Lab Overview

In this lab, you will implement management groups to organize your Azure subscriptions for centralized governance and access control. You'll create custom RBAC roles to define specific permissions, ensuring users only access needed resources.These roles will be assigned based on job responsibilities to enforce security and compliance. Finally, you'll use the Activity Log to monitor role assignments, track changes, and ensure adherence to access control policies.

## Lab objectives
In this lab, you will complete the following tasks:
+ Task 1: Create and assign tags via the Azure portal.
+ Task 2: Enforce tagging via an Azure Policy.
+ Task 3: Apply tagging via an Azure Policy.
+ Task 4: Configure and test resource locks. 

## Exercise 2: Manage Governance via Azure_Policy

In this exercise, you will learn to manage governance via Azure Policy that involves defining and enforcing rules that govern the resources in your Azure environment to ensure compliance with organizational standards.

### Task 1: Assign tags via the Azure portal

In this task, you will learn how to create and assign a tag to an Azure resource group through the Azure portal. Tags in Azure are key-value pairs that help in organizing and categorizing resources for better management and reporting.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, search **Resource groups (1)** and select **Resource groups (2)** under services. Select **Resource group AZ-104T02** from the list.

   ![image](./media/l2-image45.png)

1. On the resource group blade, click **Tags (1)** and create a tag with the following settings **(2)** and click on **Apply (3)** to save your change:

    | Setting | Value |
    | --- | --- |
    | Name | **Role** |
    | Value | **Infra** |

   ![image](./media/3-10-l2-48.png)
   
### Task 2: Enforce tagging via an Azure policy

In this task, you will explore how to enforce governance policies by assigning the built-in Require a tag and its value on resources policy to a specific Azure resource group. This policy ensures that all resources created within the resource group are tagged with a predefined key-value pair, promoting consistency and compliance with organizational standards.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, enter **Policy (1)**, and then select **Policy (2)** under services.

   ![image](./media/3-10-l2-49.png)

1. In the **Authoring** section, click **Definitions (1)**. Take a moment to browse through the list of built-in policy definitions that are available for you to use. List all built-in policies that involve the use of tags by selecting the **Tags (3)** entry (and de-selecting all other entries) in the **Category (2)** drop-down list and click on **Apply (4)**.

   ![image](./media/3-10-l2-50.png)

1. In search bar enter **require (1)** and click the entry representing the **Require a tag and its value on resources (2)** built-in policy and review its definition.

    ![image](./media/6-10-lab2-2.png)
   
1. On the **Require a tag and its value on resources** built-in policy definition blade, click **Assign policy**.

    ![image](./media/6-10-lab2-3.png)

1. Specify the **Scope** by clicking the ellipsis **(1)** button and selecting the following values and choose **Select (4)** :

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab **(2)** |
    | Resource Group | AZ-104T02 **(3)** |

   ![image](./media/6-10-lab2-4.png)
   
   >**Note:** A scope determines the resources or resource groups where the policy assignment takes effect. You could assign policies on the management group, subscription, or resource group level. You also have the option of specifying exclusions, such as individual subscriptions, resource groups, or resources (depending on the assignment scope). 

1. Configure the **Basics** properties of the assignment by specifying the following settings and click on **Next (4)**  (leave others with their defaults):

    | Setting | Value |
    | --- | --- |
    | Assignment name | **Require Role tag with Infra value (1)**|
    | Description | **Require Role tag with Infra value for all resources in the Cloud Shell resource group (2)**|
    | Policy enforcement | Enabled **(3)**|

    ![image](./media/3-10-l2-54.png)
   
    >**Note:** The **Assignment name** is automatically populated with the policy name you selected, but you can change it. You can also add an optional **Description**. **Assigned by** is automatically populated based on the user name creating the assignment. 

1. Set **Parameters** to the following values:

    | Setting | Value |
    | --- | --- |
    | Tag Name | **Role (1)** |
    | Tag Value | **Infra (2)** |

   ![image](./media/3-10-l2-55.png)
   
1. Click **Next (3)** and review the **Remediation** tab. Leave the **Create a Managed Identity (1)** checkbox unchecked. 

    >**Note:** This setting can be used when the policy or initiative includes the **deployIfNotExists** or **Modify** effect.

1. Click **Review + create (2)** and then click **Create**.

    ![image](../Labs/media/3-10-l2-56n.png)

    >**Note:** Now you will verify that the new policy assignment is in effect by attempting to create another Azure Storage account in the resource group without explicitly adding the required tag. 
    
    >**Note:** It might take between 5 and 15 minutes for the policy to take effect.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, search **Storage accounts (1)** and select **Storage accounts (2)** under services and then click **+ Create**.

    ![image](./media/3-10-l2-57.png)

    ![image](./media/3-10-l2-58.png)

1. On the **Basics** tab of the **Create storage account** blade, verify that you are using the Resource Group that the Policy was applied to and specify the following settings (leave others with their defaults), click **Review + create (2)** and then click **Create**:

    | Setting | Value |
    | --- | --- |
    | Storage account name | storage<inject key="DeploymentID" enableCopy="false"/>  **(1)**|

     ![image](./media/3-10-l2-59.png)

1. Once you create the deployment, you should see the **Validation failed. Required information is missing or not valid** message.

    ![image](./media/6-10-lab2-6.png)

1. Verify whether the error message states that the resource deployment was disallowed by the policy by clicking **Previous** tags tab and select **Policy details** link to review the details.

   ![image](../Labs/media/6-10-lab2-7n.png)

    >**Note:** You can find more details about the error, including the name of the role definition **Require Role tag with Infra value**. The deployment failed because the storage account you attempted to create did not have a tag named **Role** with its value set to **Infra**.

### Task 3: Apply tagging via an Azure policy

In this task, you will focus on identifying and remediating non-compliant resources by leveraging a different Azure Policy definition. Non-compliant resources are those that do not meet the criteria defined in your organization's governance policies, such as missing mandatory tags or violating security configurations. 

1. In the Azure portal, search for **Policy (1)** and select **Policy (2)**. 

   ![image](./media/3-10-l2-49.png)

1. In the list of assignments, right click the ellipsis icon in the row representing the **Require Role tag with Infra value (1)** policy assignment and use the **Delete assignment (2)** menu item to delete the assignment and then select **Yes**.

   ![image](./media/3-10-l2-60.png)

1. In the **Authoring** section, click **Definitions (1)**. Take a moment to browse through the list of built-in policy definitions that are available for you to use. List all built-in policies that involve the use of tags by selecting the **Tags (3)** entry (and de-selecting all other entries) in the **Category (2)** drop-down list and click on **Apply (4)**.

     ![image](./media/3-10-l2-50.png)

1. In search bar enter **Inherit (1)** and click the entry representing the **Inherit a tag from the resource group if missing (2)** built-in policy and review its definition.

     ![image](./media/3-10-l2-61.png)

1. On the **Inherit a tag from the resource group if missing** built-in policy definition blade, click **Assign Policy**.

     ![image](../Labs/media/3-10-l2-62n.png)

1. Click **Assign policy** and specify the **Scope** by clicking the ellipsis button (...) **(1)** and selecting the following values and choose **Select (4)**:

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab **(2)**|
    | Resource Group | AZ-104T02 **(3)**|

      ![image](../Labs/media/l2-image37n.png)

1. To specify the **Policy definition**, click the ellipsis button **(1)** and then search for and select **Inherit a tag from the resource group if missing (2)** then click on **Add (3)** if not selected in the definition.

    ![image](./media/l2-image37.png)

    ![image](./media/l2-image38.png)

   >**Note:** You can ignore the above step if the policy definition has appeared automatically.

1. Configure the remaining **Basics** properties of the assignment by specifying the following settings (leave others with their defaults) and click on **Next (4)**.

    | Setting | Value |
    | --- | --- |
    | Assignment name | **Inherit the Role tag and its Infra value from the Cloud Shell resource group if missing (1)**|
    | Description | **Inherit the Role tag and its Infra value from the Cloud Shell resource group if missing (2)**|
    | Policy enforcement | Enabled **(3)** |

    ![image](./media/3-10-l2-64.png)

1. Set **Parameters** to the following values:

    | Setting | Value |
    | --- | --- |
    | Tag Name | **Role (1)** |

    ![image](./media/3-10-l2-65.png)
   
1. Click **Next (2)** and, on the **Remediation** tab, configure the following settings (leave others with their defaults) and click **Review + Create (3)**.

    | Setting | Value |
    | --- | --- |
    | Create a remediation task | enabled **(1)**|
    | Policy to remediate | **Inherit a tag from the resource group if missing (2)** |

    >**Note:** This policy definition includes the **Modify** effect.

    ![image](./media/3-10-l2-66.png)
  
1. Click  **Create**.

    >**Note:** To verify that the new policy assignment is in effect, you will create another Azure Storage account in the same resource group without explicitly adding the required tag. 
    
    >**Note:** It might take between 5 and 15 minutes for the policy to take effect.

1. On Azure Portal page, in **Search resources, services and docs (G+/)** box at the top of the portal, search and select **Storage accounts** under services and then click **+ Create**. 

1. On the **Basics** tab of the **Create storage account** blade, verify that you are using the Resource Group that the Policy was applied to, and specify the following settings (leave others with their defaults) and click **Review + create (5)**:

    | Setting | Value |
    | --- | --- |
    | Subscription | the name of the Azure subscription you are using in this lab **(1)** |
    | Resource Group | AZ-104T02 **(2)**|
    | Storage account name |  **storage<inject key="DeploymentID" enableCopy="false"/> (3)** |
    | Redundancy |  **Locally-redundant storage (LRS) (4)** |

    ![image](./media/3-10-l2-68.png)
   
1. Verify that this time the validation passed and click **Create**.

     ![image](./media/3-10-l2-69.png)

   >**Note:** If the validation fails, kindly wait for some time as it might take some time for the policy to take effect for the validation to pass through.

1. Once the new storage account is provisioned, click **Go to resource** button and, on the **Overview** blade of the newly created storage account, note that the tag **Role** with the value **Infra** has been automatically assigned to the resource.

   ![image](./media/3-10-l2-70.png)

   <validation step="7dd291d1-35d1-40bb-857b-8ea685202e58" />

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
   > - Hit the Validate button for the corresponding task. If you receive a success message, you can proceed to the next task. 
   > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
   > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help

## Task 4: Configure and test resource locks

In this task, you will configure and test a resource lock to safeguard critical Azure resources from unintended changes or deletions. Resource locks are a powerful governance feature in Azure.

1. Search for and select your resource group **AZ-104T02**.
   
1. In the **Settings (1)** blade, select **Locks (2)**.

1. Select **+ Add (3)** and complete the resource lock information. When finished select **Ok (6)**.
   
    | Setting | Value |
    | --- | --- |
    | Lock name | `rg-lock` **(4)** |
    | Lock type | **Delete (5)** (notice the selection for read-only) |
    
    ![image](./media/3-10-l2-72.png)

1. Navigate to the resource group **Overview** blade, and select **Delete resource group**.

    ![image](./media/3-10-l2-73.png)

1. In the **Enter resource group name to confirm deletion** textbox provide the resource group name, `AZ-104T02` **(1)**. Notice you can copy and paste the resource group name and click on **Delete (2)** twice. 

    ![image](./media/3-10-l2-74.png)
   
1. You should receive a notification denying the deletion. 

    ![image](./media/l2-image64.png)

1. From the **AZ-104T02** resource group **Overview** blade, under **Settings** section select **Locks (1)** and Select Locks and proceed to click **Delete (2)** to remove the existing **rg-lock** locks.

    ![image](./media/3-10-l2-75.png)

### Review

In this lab, you have completed:

- Created and assigned tags to resource groups using the Azure portal.
- Enforced tagging compliance by assigning an Azure Policy.
- Applied tagging to resources automatically using Azure Policy.
- Configured and validated resource locks to prevent accidental changes or deletions.

## Extend your learning with Copilot
Copilot can assist you in learning how to use the Azure scripting tools. Copilot can also assist in areas not covered in the lab or where you need more information. Open an Edge browser and choose Copilot (top right) or navigate to *copilot.microsoft.com*. Take a few minutes to try these prompts.
+ What are the Azure PowerShell and CLI commands for adding and deleting resource locks on a resource group?
+ Tabulate the differences between Azure policy and Azure RBAC, include examples.
+ What are the steps to enforce Azure policy and remediate resources which are not compliant?
+ How can I get a report of Azure resources with specific tags?

## Learn more with self-paced training

+ [Design an enterprise governance strategy](https://learn.microsoft.com/training/modules/enterprise-governance/). Use RBAC and Azure Policy to limit access to your Azure solutions, and determine which method is right for your security goals.

## Key takeaways

Congratulations on completing the lab. Here are the main takeaways for this lab. 

+ Azure tags are metadata that consists of a key-value pair. Tags describe a particular resource in your environment. In particular, tagging in Azure enables you to label your resources in a logical manner.
+ Azure Policy establishes conventions for resources. Policy definitions describe resource compliance conditions and the effect to take if a condition is met. A condition compares a resource property field or a value to a required value. There are many built-in policy definitions and you can customize the policies. 
+ The Azure Policy remediation task feature is used to bring resources into compliance based on a definition and assignment. Resources that are non-compliant to a modify or deployIfNotExist definition assignment, can be brought into compliance using a remediation task.
+ You can configure a resource lock on a subscription, resource group, or resource. The lock can protect a resource from accidental user deletions and modifications. The lock overrides any user permissions.
+ Azure Policy is pre-deployment security practice. RBAC and resource locks are post-deployment security practice.
  
### You have successfully completed the lab
