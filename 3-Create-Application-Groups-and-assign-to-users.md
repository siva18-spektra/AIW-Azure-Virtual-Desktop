# Lab 3: Create Application Groups and assign them to users

### Estimated Duration: 30 Minutes

## Lab Scenario

Contoso wants to restrict access to the applications used by different teams in the organization. With Azure Virtual Desktop, admins can create unique application groups for users that require access to a specific set of applications. In this lab, you will help Contoso to configure and create an application group and add applications to it.


As explained in the General Hierarchy section, an Application Group is a logical grouping of applications installed on session hosts in the host pool. There are two types of application groups: 

1. RemoteApp 
2. Desktop 

## Lab objective

In this lab, You will complete the following exercise:

- Exercise 1: Create an Application Group

### Exercise 1: Create an Application Group

An application group of type ‘Desktop’ was created automatically while creating the Session Host in the previous exercise. 

In this exercsie, You will create a new application group of type "RemoteApp" and publish two applications in it. Also, we will assign users to both application groups.

1. Navigate to the Azure portal, Search for **Azure Virtual Desktop (1)** in the search bar and select **Azure Virtual Desktop (2)** from the search results.

   ![ws name.](media/w1.png)

1. You will be directed towards the **Azure Virtual Desktop** management window.  

   ![ws name.](media-1/Ex3-task1-step2n.png)

1. Click on the **Application Groups** tab under ***Manage*** blade and you will see the default Application Group there. 

   ![ws name.](media-2/applicationgroup.png)
   
1. Click on the **GS-AVD-HP-DAG** application group.

   ![ws name.](media-2/gsavd.png)
      
1. Under **Manage** blade, open **Assignments (1)** and then click on **+ Add (2)**. 

   ![ws name.](media-2/assignments.png)   
 
1. Now in the search bar, copy and paste your **Username: <inject key="AzureAdUserEmail"></inject> (1)**. Then under the search bar, click on your **Username: <inject key="AzureAdUserEmail"></inject> (2)** to select it then click on the **Select(3)** button.

   ![ws name.](media/lab3-1.png)
   
1. We will now create a new Application Group of type ‘RemoteApp’. To do this, navigate back to the **Azure Virtual Desktop** and click on the **Host Pools (1)** button, then click on the **GS-AVD-HP (2)** pool.

   ![ws name.](media-2/avd-6.png)

1. In the **GS-AVD-HP** host pool select **Application Groups** under **Applications**.

   ![ws name.](media/avd-53a.png)

1. Then select **+ Add** in the **GS-AVD-HP - Application groups** 

   ![ws name.](media/L3E1S8.png)

1. In the **Basics** tab, do the following configuration: 

   i. Leave the following parameters to default:
   
      - *Subscription*
      - *Location*
         
   ii. Fill in the remaining parameters below:  
   
      - Resource Group: Select **AVD-Hostpool-RG-avd (1)** from the dropdown.
      - Application Group Type: **RemoteApp (2)** 
      - Application Group Name: **AVD-AG-01 (3)**
      - Click on **Next: Applications > (4)**

         ![ws name.](media-2/avd-38n.png)

1. On the **Applications** tab, click on **+ Add Applications** to add applications to this application group.

   ![ws name.](media/ag1.png)

1. In this window, choose the parameters mentioned below: 

    - Application Source: **Start Menu (1)** *(choose from the dropdown)*  
    - Application: **Excel (2)** *(choose from the dropdown)* 
    - Display Name: **Excel (3)**
    - Leave the rest of the parameters as default and click on **Review + Add (4)**  
   
      ![ws name.](media-1/avd-7.png)

1. Click on **Add.**

   ![ws name.](media/L3E1S13.png)
 
1. Click on **+ Add Applications** again. 

   ![ws name.](media/ag2.png)

1. Choose the parameters as mentioned below: 

    - Application Source: **Start Menu (1)** *(choose from the dropdown)*   
    - Application: **Word (2)** *(choose from the dropdown)*
    - Display Name: **Word (3)**    
    - Leave the rest of the parameters to default and click on **Review + Add (4)** 
   
      ![ws name.](media-1/avd-8.png)

1. Click on **Add.**

   ![ws name.](media/L3E1S16.png)

1. Click on **Next: Assignments >**.

   ![ws name.](media/ag3.png)

1. Click on the **+Add Microsoft Entra users or user groups (1)**, then copy and paste your username **<inject key="AzureAdUserEmail"></inject>** **(2)** in the search bar. When your username appears under the search bar, click on the  **username (3)**, and then click on the **Select (4)** button. This will give you access to the application group.
 
   ![ws name.](media/L3-E1-S16a-1.png)

1. Click on **Next: Workspace >**.

   ![ws name.](media/ag6.png)

1. On the **Workspace** tab, choose the parameters as mentioned below:  

    - Register application Group: **Yes**
    - Register application Group: Leave the value to default
    - Click on **Review + Create**.

      ![ws name.](media/lab3-4.png)

1. The last window helps us to verify if the parameters we filled in are correct. Wait for validation to pass, then click on **Create** to initiate the deployment. 

   ![ws name.](media-2/createappliction1.png)

    >**Note:** The deployment will take about a minute to succeed.

1. Once the deployment is complete, open notifications and click on **Go to Resource**. 

   ![ws name.](media/81.png)

1. In the Application Group Window, click on **Applications** under the **Manage** section of settings and you will see that the applications are published in the new application Group. 

   ![ws name.](media/uiupdate04.png)

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
- Scroll down and hit the Validate button in the lab guide for the corresponding task. If you receive a success message, you can proceed to the next task.
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="3816cf87-5d86-4cff-a599-63b4332838e5" />
   
## Summary

In this lab, you created a new RemoteApp application group, published applications such as Excel and Word, and assigned user access alongside the existing desktop application group for your AVD environment.

Now, click on the **Next** button present in the bottom-right corner of this lab guide. 

![Start Your Azure Journey](./media/Next.png) 