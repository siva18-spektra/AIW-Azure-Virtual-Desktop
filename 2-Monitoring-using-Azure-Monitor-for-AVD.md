# Lab 2(A): Monitoring using Log Analytics

### Estimated Duration: 30 Minutes

##  Lab Scenario

Contoso is interested in setting up an operation center focused on monitoring the host pools, user access, and many more. You will help Contoso set up a monitoring solution with the help of features available in Azure virtual desktop and Azure monitoring resources. You will create a Log Analytics workspace and map it to the AVD environment using Azure Insights.

Azure Virtual Desktop uses Azure Monitor for monitoring and alerts like many other Azure services. This lets admins identify issues through a single interface. The service creates activity logs for both user and administrative actions.

## Lab Objectives

In this lab, you will complete the following exercises:

- Exercise 1: Create Log Analytics
- Exercise 2: Enable diagnostics for Workspace

## Exercise 1: Create Log Analytics

In this exercise, you will create a Log Analytics workspace by configuring basic settings, reviewing the deployment parameters, and deploying it to support monitoring for your AVD environment.

1. In the search bar of the Azure portal, type **Log Analytics workspace (1)**. From the search results, select **Log Analytics workspace (2)**.

   ![ws name.](media/L2AE1S1.png)

1. Click on **+ Create**.

   ![ws name.](media/L2AE1S1A.png)

1. Now add the following configurations:

   - Subscription: Leave it to **Default (1)**
  
   - Resource group: Select **AVD-Hostpool-RG-avd (2)** from the drop-down.
  
   - Name: **<inject key="Log Analytics Workspace Name"></inject> (3)**
  
   - Region: Select **<inject key="Region" enableCopy="false"/> (4)** from the drop-down list
  
   - Click on **Review + Create (5)**

      ![ws name.](media/vd10.png) 

1. The last window helps us to verify if the parameters we filled are correct. Wait for validation to pass, then click on **Create** to initiate the deployment.

   ![ws name.](media-1/Ex2-task1-step5.png)

1. Once the deployment succeeds, it will look like the image shown below:

   ![ws name.](media-1/Ex2-task1-step6.png)

## Exercise 2: Enable diagnostics for Workspace

In this exercise, you will enable diagnostics for the AVD workspace by configuring monitoring settings, deploying required templates, adding extensions and identities, and validating that all diagnostic components are correctly applied.
 
1. On the **Azure portal**, search for **Azure Virtual Desktop (1)** in the search bar and select **Azure Virtual Desktop** **(2)** from the search results.

   ![ws name.](media/L2AE2S1.png) 

1. You will be directed towards the Azure Virtual Desktop (hereafter referred to as AVD) management window. Select **Insights** under **Monitoring** blade.

   ![ws name.](media/L2AE2S2.png)
   
1. On the **Insights** page, Select the following values.
   
   - Subscription: **Choose the default subscription (1)**
   - Resource group: **avd-hostpool-rg-avd (2)**
   - Host Pool: **GS-AVD-HP (3)**
   - Time range: **Leave it to default (4)**
   - Then, click on **GS-AVD-HP (5)**

      ![ws name.](media/L2AE2S3.png)
   
1. On the **GS-AVD-HP | Insights** **(1)** hostpool page, click on **Open Configuration Workbook** **(2)**. 

   ![ws name.](media/L2AE2S4.png)

1. On the **CheckAMAConfiguration** page, re-select the resource group and host pool name as mentioned below. 

   - Resource group: **avd-hostpool-rg-avd (1)**
   - Host Pool: **GS-AVD-HP (2)**
   - After that select the **<inject key="Log Analytics Workspace Name" enableCopy="false" />** workspace **(3)** from the drop-down menu under the **Check diagnostic settings for  host pool** section of the page. 

      ![ws name.](media/L2AE2S5.png)
   
1. Scroll down on the same page and click on **Configure host pool**.

   >**Note**: Sometimes, monitoring for the host pool gets configured automatically. Please **re-configure** monitoring for the host pool as a few components might not be configured.

    ![ws name.](media/L2AE2S6.png)
   
1. On the **Deploy template** page, the diagnostic settings for the host pool are automated using a template. Look through the categories seleted and click on **deploy**.

   ![ws name.](media/avd-5.png)

1. Once the deployment is successful, **Refresh** the **CheckAMAConfiguration** page. You'll be able to see the settings applied to the host pool.

   ![ws name.](media/L2AE2S8.png)
   
1. Scroll down on the same page and click on **Configure workspace**.

   ![ws name.](media/L2AE2S9.png)
   
1. On the **Deploy template** page, click on **deploy**. (Note: The diagnostic settings for the host pool are automated using a template).

   ![ws name.](media/lab2-8n.png) 

1. Once the deployment is successful, **Refresh** the **CheckAMAConfiguration** page 2-3 times as it takes some time to load the details. You'll be able to see the settings applied to the workspace.

   ![ws name.](media/L2AE2S11.png)
   
1. On **CheckAMAConfiguration** page, Select **Session host data settings (1)**. Then, select the **<inject key="Log Analytics Workspace Name" enableCopy="false" /> (2)** analytics workspace as the **Workspace destination**.Click on the **Create data collection rule (3)**.

   ![ws name.](media/L2AE2S12.png)

1. On the **Deploy template** page, Click on **deploy**.

   ![ws name.](media/L2AE2S12A.png)

1. Once the deployment is successful, **Refresh** the **CheckAMAConfiguration** page 2-3 times as it takes some time to load the details. You'll be able to see the Data Collection rule has been created.

   ![ws name.](media/L2AE2S12B.png)

1. On the **Check AMA Configuration** page, scroll down to the **Session hosts missing Azure Monitor extension** section and then click on **Add extension**.

   ![ws name.](media/L2AE2S13.png)
   
1. On the **Deploy template** page, click on **Deploy**. (Note: the diagnostic settings for the workspace are automated using a template).

   ![ws name.](media/L2AE2S14.png)

1. Now scroll down and click on the **Add system managed identity**

   ![ws name.](media/L2AE2S15.png)

1. Click on the **Add system managed identity**

   ![ws name.](media/avdmon4.1b.png)

1. Once the deployment is successful, **Refresh** the **Check Configuration** page. You'll see a message as **No session hosts missing AMA extension.**.

   ![ws name.](media/L2AE2S17.png)


   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
- Scroll down and hit the Validate button in the lab guide for the corresponding task. If you receive a success message, you can proceed to the next task.
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="53c5dd73-f7c0-41ed-bd9a-9f6a03ef630a" />

## Summary
In this lab, you created a Log Analytics workspace and enabled diagnostics for the AVD environment by configuring monitoring, deploying diagnostic templates, adding required extensions and identities, and validating that all components are properly connected to Azure Monitor.

Now, click on the **Next** button present in the bottom-right corner of this lab guide.

![Start Your Azure Journey](./media/Next.png) 
