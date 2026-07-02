# Lab 12: App Masking (Read-Only)

### Estimated Duration: 20 Minutes

### Overview

Application Masking is used to manage user access to installed components. Application Masking may be used in both physical and virtual environments. Application Masking is most often applied to manage non-persistent, virtual environments, such as Virtual Desktops.

##  Exercise 1: Create Rule Set using FSLogix Apps RuleEditor (Read-Only)

In this exercise, you go through the steps to create a rule set in **FSLogix Apps RuleEditor** to disable access to the application in the session hosts.

1. In your JumpVM, go to Start and search for **FSLogix Apps RuleEditor** and open the FSLogix Apps RuleEditor application from the search results.

   ![](media/selectRE.png)
    
1. On the FSLogix Apps RuleEditor application, click on **New**.

   ![](media/new.png)

1. Provide a name for the Rule Set as **hiderule (1)** and click on **Enter file name (2)**.

   ![](media-1/hiderule.png)

1. On the **Rule Set : hiderule** window, choose **Blank Rule Set (1)** and click on **Ok (2)**.

   ![](media-1/blankrule.png)
   
1. On **FSLogix Apps RuleEditor**, click on **+ New Rule**.

   ![](media-1/newrule.png)
    
1. Click on **Browse (1)** and select **File (2)**.

   ![](media-1/file.png)
    
1. Navigate to the path **C:\Program Files\Microsoft Office\Office16 (1)**, select **MSAccess (2)** and click on **Open (3)**

   ![](media-1/new-avd-lab13-2.jpg)
   
1. After selecting the application, click on **OK**.

   ![](media-1/new-avd-lab13-3.jpg)
   
   >**Note:** The rule sets are already created as part of prerequisites.


##  Exercise 2: App Masking (Read-Only)

In this task, you will download the pre-created rule sets into the session host using a PowerShell script.

1. In your Azure portal search for **Virtual Machines (1)** in the search bar and click on **Virtual Machines (2)** from the suggestions.

   ![](media-1/task2step1.png)

1. Click on **AVD-HP01-S-0**.

   ![](media-1/new-avd-lab13-4.png)
   
1. Then click on the **Run command** under **Operations**.

   ![](media-1/new-avd-lab13-5.png)

1. Now select RunPowerShellScript.
 
   ![](media-1/task2step4.png)

1. A similar window to that of the below image will appear.

   ![](media-1/task2step5.png)

1. Copy the script given below and paste it by using Ctrl + V in the Powershell window.

   ```
   $WebClient = New-Object System.Net.WebClient
   $WebClient.DownloadFile("https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-Azure-Virtual-Desktop/Azure-Virtual-Desktop-v3/LabFiles/hiderule.fxa","C:\Program Files\FSLogix\Apps\Rules\hiderule.fxa")
   $WebClient = New-Object System.Net.WebClient
   $WebClient.DownloadFile("https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-Azure-Virtual-Desktop/Azure-Virtual-Desktop-v3/LabFiles/hiderule.fxr","C:\Program Files\FSLogix\Apps\Rules\hiderule.fxr")
   Start-Process -Wait -FilePath "C:\LabFiles\fslogix\x64\Release\FSLogixAppsRuleEditorSetup.exe" -ArgumentList "/S" -PassThru
   Start-Process -Wait -FilePath "C:\LabFiles\fslogix\x64\Release\FSLogixAppsSetup.exe" -ArgumentList "/S" -PassThru

   #Display script completion in the console
   Write-Host "Script Executed successfully"
   ```

   ![](media-1/task2step6.png)
  
1. Then click on **Run** to execute the script.

   ![](media-1/task2step7.png)

1. Wait for some time for the script to execute. Once done, it will show an output saying **Script Executed successfully**.

   ![](media-1/task2step8.png)

   >**Note**: It will take around 1-2 minutes for the script to execute.
   
1. Navigate to virtual machines and click on **AVD-HP01-SH-1**.

   ![](media-1/task2step9.png)

1. Click on **Run command (1)** under Operations. Then select **RunPowerShellScript (2)**.

   ![](media-1/task2step10.png)

1. Copy the script given below and paste it by using Ctrl + V in the Powershell window.

   ```
   $WebClient = New-Object System.Net.WebClient
   $WebClient.DownloadFile("https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-Azure-Virtual-Desktop/Azure-Virtual-Desktop-v3/LabFiles/hiderule.fxa","C:\Program Files\FSLogix\Apps\Rules\hiderule.fxa")

   $WebClient = New-Object System.Net.WebClient
   $WebClient.DownloadFile("https://raw.githubusercontent.com/CloudLabsAI-Azure/AIW-Azure-Virtual-Desktop/Azure-Virtual-Desktop-v3/LabFiles/hiderule.fxr","C:\Program Files\FSLogix\Apps\Rules\hiderule.fxr")
   Start-Process -Wait -FilePath "C:\LabFiles\fslogix\x64\Release\FSLogixAppsRuleEditorSetup.exe" -ArgumentList "/S" -PassThru
   Start-Process -Wait -FilePath "C:\LabFiles\fslogix\x64\Release\FSLogixAppsSetup.exe" -ArgumentList "/S" -PassThru

   #Display script completion in the console
   Write-Host "Script Executed successfully"
   ```

   ![image](media-1/task2step11.png)

1. Then click on Run to execute the script.

   ![](media-1/task2step12.png)

1. Wait for some time for the script to execute. Once done, it will show an output saying Script Executed successfully.

   ![](media-1/task2step13.png)

   > **Note:** It will take around 1-2 minutes for the script to execute.

1. On your PC, go to **Start** and search for **Windows App** and open the remote desktop application with the exact icon as shown below.

   ![ws name.](media/137.png)
   
1. Click on the **account icon** in the top-right corner, then select **Sign in with another account**.

   ![ws name.](media/lb16.png)
  
1. Enter your **credentials** to access the workspace.

   - Username: Paste the username **<inject key="AzureAdUserEmail"></inject>** and then click on **Next**.
   
     ![ws name.](media/95.png)

   - Password: Paste the password **<inject key="AzureAdUserPassword"></inject>** and click on **Sign in**.

     ![ws name.](media/96.png)
   
      >**Note:** If there's a dialog box saying **Stay signed in**, then select the **No** option.

      ![](media/login1.png)
      
1. The AVD dashboard will launch, then double-click on the **SessionDesktop** application to access it.

   ![](media-1/new-avd-lab13-9.jpg)
   
1. A window saying **Connecting to: Session Desktop** will appear. Wait for a few seconds, then enter your password to access the Desktop.

   - Password: **<inject key="AzureAdUserPassword"></inject>**
   
     ![ws name.](media/avd-14.png)

1. Wait for the Session Desktop to connect.

   ![ws name.](media/ex4t2s4.png)

1. Once connected, In the **start menu** search for **Rule Editor (1)** then right-click on **FSLogix Apps RuleEditor (2)** and click on **Run as Administrator (3)**.

   ![](media-1/runasadmin.png)

1. Select **Yes** from **Do you want to allow this app to make changes to your device?** tab.

   ![](media-1/new-avd-lab13-10.jpg)
    
1. On the FSLogix Apps RuleEditor application, click on **Open**.

   ![](media/L3-E1-SFSLogixOpen.png)
    
1. Navigate to **C:\Program Files\FSLogix\Apps\Rules (1)**, select **hiderule (2)** and click on **Open (3)**.

   ![](media-1/openhiderule.png)
   
1. Once you have imported the rule, click on **Manage Assignments**.

   ![](media-1/manage.png)
    
1. On the Assignments tab, you can review the hiding rule applied on both **AVD users (1)**. After reviewing, click on **Cancel**.

   ![](media-1/ruleoneuser.png)
    
1. Now click on **Apply Rules to System**.

   ![](media-1/applyrules.png)

1. Paste the below-mentioned link in your browser in the **JumpVM** and enter your **credentials** to log in. 

   ```
   aka.ms/wvdarmweb
   ```

   - Username: Enter the username  **<inject key="Avd User 01"></inject>** then click on **Next**.
   
      ![ws name.](media/username.png)

   - Password: Paste the password  **<inject key="AVD User Password"></inject>** and click on **Sign in**.

      ![ws name.](media/password.png)

      >**Note:** If there's a dialog box saying **Stay signed in**, then select the **No** option.

      ![](media/login1.png)

1. Now in the AVD dashboard, click on the **Session Desktop** to access it. 

   ![ws name.](media/desktp-v2.png)

1. Select **Allow** on the prompt asking permission to *Access local resources*.

1. On the **In Session Settings** screen, leave **Clipboard** checked so that you can copy and paste between your local device and the session, then click **Connect**.

   ![ws name.](media/lab4-10.png)

1. Enter your **credentials** to access the application and click on **Submit**.

   - Username: Paste the username  **<inject key="Avd User 01"></inject>** then click on **Next**.
   
   - Password: Paste the password  **<inject key="AVD User Password"></inject>** and click on **Submit**.
   
      ![ws name.](media/lab4-2-1.png)
     
1. Within the session desktop, go to Start and search for **Access (1)** and double-click on **Access (2)** to open the application. Here you will not be able to open the app due to the hiding rule applied to your session desktop through JumpVM. 

   ![](media-1/accessblock.png)

1. You have successfully added the hiding rule through App Masking for both the JumpVM and Session host.

Now, click on Next from the lower right corner to move on to the next page.

 ![Start Your Azure Journey](./media/Next.png) 

