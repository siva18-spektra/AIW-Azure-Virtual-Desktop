# Lab 6: Setup FSLogix

### Estimated Duration: 50 Minutes

## Lab Scenario

Contoso was getting complaints from the end-users stating that they were losing their User Profile when they connected to a different session host. Contoso wants to implement FSLogix which will help the end users to have separate storage containers for their user data and will help in maintaining consistency. You will help Contoso implement FSLogix in the Azure virtual desktop environment.

The Azure Virtual Desktop service, recommends FSLogix profile containers as a user profile solution. FSLogix is designed to roam profiles in remote computing environments, such as Azure Virtual Desktop. It stores a complete user profile in a single container. At sign-in, this container is dynamically attached to the computing environment using natively supported Virtual Hard Disk (VHD) and Hyper-V Virtual Hard disk (VHDX). The user profile is immediately available and appears in the system exactly like a native user profile. This article describes how FSLogix profile containers are used with the Azure Files function in Azure Virtual Desktop.

## Lab objectives

In this lab, you will complete the following exercises:

- Exercise 1: Create Storage account and Classic File Share
- Exercise 2: Configure Classic File Share
- Exercise 3: Configure Session Hosts
- Exercise 4: Verifying the User profiles stored in Classic File Share

## Exercise 1: Create Storage account and Classic File Share

In the exercsie, we will be creating a storage account with a Classic File Share which will be used to store user profiles for FSlogix.

1. Navigate to the Azure portal, search for **Storage accounts (1)** in the search bar, and select **Storage accounts (2)** from the suggestions.

   ![ws name.](media/up10.png)
   
2. Click on **+ Create** to create a new storage account.

   ![ws name.](media/wvd5.png)

3. Use the following configuration for the storage account.
   
   - Subscription: Leave it to **default (1)**.
   
   - Resource Group: Select **AVD-RG (2)** from the drop-down. 
   
   - Storage account name: **<inject key="Storage Account Name" /> (3)**   
      
   - Region: Select **<inject key="Region" enableCopy="false"/> (4)** from the drop-down list.  
   
   - Performance: **Standard (5)**   
   
   - Redundancy: **Geo-redundant storage (GRS) (6)**
   
   - **Select** Make read access to the data available in the event of regional unavailability. **(7)**
   
   - At last, click on **Next (8)**
   
      ![ws name.](media/avdstoargen.png)

1. On the **Advanced** tab of the Create a storage account page, leave all settings at their default values, and then click **Next** to continue.

4. In the **Networking** tab, use the following configurations:

   - Public network access: **Enable (1)** 
   - Public network access scope: **Enable from selected virtual networks and IP addresses (2)**
     >**Note:** This will make sure that your storage account is not accessible from the public network, making it more secure.
   - Virtual network subscription: Leave it to **default (3)**.
   - Virtual Network: **aadds-vnet (4)**
   - Subnets: **sessionhosts-subnet (10.0.1.0/24) (5)**
   - Leave the rest to default settings.
   - Click on **Next (6)**.

      ![ws name.](media/lab6-3n.png)

1. On the **Data protection** tab of the Create a storage account page, leave all settings at their default values, and then click **Next** to proceed.

5. On the **Security** tab, make sure   to enable **Require secure t   ransfer for REST API operations**, **Allow enabling anonymous access on individual containers**, and **Enable storage account key access** **(1)** options. Once enabled, click on the **Review + create (2)** button.

   ![ws name.](media/lab6-2n.png)

6. Click on **Create**.

   ![ws name.](media/up3.png)

7. After deployment completes, click on **Go to resource**.

   ![ws name.](media/a59n.png)
   
8. In the storage account, click on **Classic File Shares (1)** present under **Data storage** blade. Then click on **Not configured (2)** under **Classic File Share settings** page.

   ![ws name.](media-2/L6E1S8.png)

9. Click on **Set up** under **Microsoft Entra Domain Services** for enabling Identity-based access to users.

   ![ws name.](media-2/L6E1S9-new.png)

10. Select **Enable Microsoft Entra Domain Services (Microsoft Entra DS) for this Classic File Share (1)** and then click on **Save (2)**.
     
    ![ws name.](media-2/L6E1S10.png)
    
    >**Note:** Setting this property implicitly **domain joins** the storage account with the associated Azure AD DS deployment. Azure AD DS authentication over SMB is then enabled for all new and existing Classic File Share in the storage account.
 
11. Return to the **<inject key="Storage Account Name" enableCopy="false"/>**  storage account and on the left pane, click on **Classic File Share (1)** present under **Data Storage**, then click on **Refresh (2)** a few times until the status of the Active Directory changes to **Configured (3)** before continuing.

    ![ws name.](media-2/avd-48.png)
 
12. On **Classic File Share (1)** page, click on  **+ Classic File Share (2)**.

    ![ws name.](media-2/avd-49.png)
 
13. Enter the following name for your Classic File Share.
    
    - Name: **userprofile (1)**   
    - Access tier: **Transaction Optimized (2)**
    - Click on **Review + create (3)**, and then **Create** this will create the Classic File Share.
    
      ![ws name.](media/avd-50n.png)

      ![ws name.](media/lab6-5n.png)

## Exercise 2: Configure Classic File Share

In this exercise, we will give Storage File Data SMB Share Contributor permissions to **permission - fslogixcontainer** group which you'll be creating so that their profiles can be stored in the Classic File Share.

1. Navigate to the Azure portal, then search for **Microsoft Entra ID (1)** in the search bar and select **Microsoft Entra ID (2)** from the suggestions.
   
   ![ws name.](media/dev3.png)
   
1. Click on **Groups** under **Manage** blade.

   ![ws name.](media/avd-17.png)
   
1. Click on **+ New group** to add a new group.

   ![ws name.](media/groups-v2.png)
   
1. Add the following configurations and leave the rest to default:

   - Group name: **permission - fslogixcontainer (1)**
   - Click on **Create (2)**.

      ![ws name.](media/avd-18.png)
   >**Note:** If you face errors such as "**name already exists**" or "**name not available**" while creating the group, Your organization might have already created it. In that case, you can find it in the next step under the All groups section.

1. Go to **All Groups (1)**, then click on the **permission - fslogixcontainer (2)** group to open it.

   ![ws name.](media/vd14.png)
   
1. Click on **Members (1)** under **Manage** and select **+ Add members (2)**.

   ![ws name.](media-1/Ex6-task2-step6.png)
   
1. Search username **<inject key="AzureAdUserEmail"></inject> (1)** and select username **<inject key="AzureAdUserEmail"></inject> (2)** and click on **Select (3)**.

   ![ws name.](media/vd15.png)
   
1. Navigate to Storage Account **<inject key="Storage Account Name" enableCopy="false"/>**, select **Classic File Share (1)** under Data Storage and click on **userprofile (2)** to open Classic File Share we created earlier.

   ![ws name.](media/avd-19.png)
     
   >**Note:** The user won't have access to Classic File Share until we perform the next steps of this task. 

1. Click on **Access Control (IAM) (1)**, then click on **Add (2)** and select **Add role assignment (3)**.

   ![ws name.](media/vd16.png)
   
1. Select the following configuration for role assignment:  
   
   - Role: Search for **Storage File Data SMB Share Contributor (1)** and select it, then click on **Next (2)**.

     ![ws name.](media/avd-20.png)
   
      >**Note:** There are three Azure built-in roles for granting share-level permissions to users:
      > - **Storage File Data SMB Share Reader** allows read access in Azure Storage Classic File Shares over SMB.
      > - **Storage File Data SMB Share Contributor** allows read, write, and delete access in Azure Storage Classic File Shares over SMB.
      > - **Storage File Data SMB Share Elevated Contributor** allows read, write, delete, and modify Windows ACLs in Azure Storage Classic File Shares over SMB.
   
   - Under the **Members** tab, follow the below steps:

      - Assign access to: Select **User, group, or service principal (1)**
      
      - Click on **+  Select members (2)**.

      - Under **Select members,** paste your group name **permission - fslogixcontainer (3)** and select it.
   
      - Then click on **Select (4)**.
   
         ![ws name.](media-1/avd-21.png)
     
      - Click on **Review + assign**

         ![ws name.](media/review%2Bassign-v2.png)
  
      - Click on **Review + assign**

         ![ws name.](media-1/Ex6-task2-step10note2.png)
        
> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
- Scroll down and hit the Validate button in the lab guide for the corresponding task.If you receive a success message, you can proceed to the next task.
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
 
<validation step="90d9f024-95c5-4a96-a375-e73e80626e96" />

## Exercise 3: Configure Session Hosts

In this Exercise, we will install and configure FSLogix in the **AVD-HP01-SH-0** session host using a Powershell script.

1. In your Azure portal, search for **Virtual Machines (1)** in the search bar and click on **Virtual Machines (2)** from the suggestions.

     ![ws name.](media/up11.png)
      
2. Click on **AVD-HP01-SH-0**.

     ![ws name.](media/vd17.png)
      
3. Then click on **Run command** under **Operations**.

   ![ws name.](media-1/Ex6-task3-step3a.png)
  
4. Now select **RunPowerShellScript**.

    ![ws name.](media/a68.png)
   
5. A similar window to that of the below image will appear.

    ![ws name.](media/a69.png)
   
6. **Copy** the script given below and paste it by using **Ctrl + V** in the Powershell window. 

   >**Note** : **Do not** run the script right away.

   ```
   # Variables
   $storageAccountName = "NameofStorageAccount"

   # Create Directories
   $LabFilesDirectory = "C:\LabFiles"

   if (!(Test-path -Path "$LabFilesDirectory")) {
      New-Item -Path $LabFilesDirectory -ItemType Directory | Out-Null
   }
   if (!(Test-path -Path "$LabFilesDirectory\FSLogix")) {
      New-Item -Path "$LabFilesDirectory\FSLogix" -ItemType Directory | Out-Null
   }

   # Download FSLogix Installation bundle
   $fsLogixZipPath = "$LabFilesDirectory\FSLogix_Apps_Installation.zip" # Store the full path

   if (!(Test-path -Path $fsLogixZipPath)) {
      try { # Add a try-catch block for better error handling
         Invoke-WebRequest -Uri "https://experienceazure.blob.core.windows.net/templates/aiw-avd-v3/FSLogix_25.06.zip" -OutFile $fsLogixZipPath -UseBasicParsing
      }
      catch {
         Write-Error "Failed to download FSLogix bundle: $_"
         return # Exit the script if the download fails
      }

      # Extract the downloaded FSLogix bundle
      function Expand-ZIPFile($file, $destination) {
         $shell = New-Object -ComObject shell.application
         $zip = $shell.NameSpace($file)
         foreach ($item in $zip.items()) {
               $shell.Namespace($destination).CopyHere($item)
         }
      }

      Expand-ZIPFile -File $fsLogixZipPath -Destination "$LabFilesDirectory\FSLogix"
   }

   # Install FSLogix
   if (!(Get-WmiObject -Class Win32_Product | where vendor -eq "FSLogix, Inc." | select Name, Version)) {
      $pathvargs = "C:\LabFiles\FSLogix\x64\Release\FSLogixAppsSetup.exe /quiet /install" # No need for a scriptblock here
      Invoke-Expression $pathvargs # Use Invoke-Expression for executing the string
   }

   # Create registry key 'Profiles' under 'HKLM:\SOFTWARE\FSLogix'
   $registryPath = "HKLM:\SOFTWARE\FSLogix\Profiles"
   if (!(Test-path $registryPath)) {
      New-Item -Path $registryPath -Force | Out-Null
   }

   # Add registry values to enable FSLogix profiles, add VHD Locations, Delete local profile, and FlipFlop Directory name
   New-ItemProperty -Path $registryPath -Name "VHDLocations" -Value "\\$storageAccountName.file.core.windows.net\userprofile" -PropertyType String -Force | Out-Null
   New-ItemProperty -Path $registryPath -Name "Enabled" -Value 1 -PropertyType DWord -Force | Out-Null
   New-ItemProperty -Path $registryPath -Name "DeleteLocalProfileWhenVHDShouldApply" -Value 1 -PropertyType DWord -Force | Out-Null
   New-ItemProperty -Path $registryPath -Name "FlipFlopProfileDirectoryName" -Value 1 -PropertyType DWord -Force | Out-Null

   # Display script completion in the console
   Write-Host "Script Executed successfully"
   ```

   ![ws name.](media/uiupdate12.png)
   
   >**Note:** The above script will :
   >
   >i) Install FSLogix Profile Container application
   >
   >ii) Configure the required registries
   > 
   >iii) Set the profile container location to the Azure Classic File Share location we created.

7. In line 2 in the script, replace **NameofStorageAccount** with **<inject key="Storage Account Name"></inject>** and then click on **Run** to execute the script.

     ![ws name.](media/jvm24.png)

9. Wait for some time for the script to execute. Once done, it will show an output saying **Script Executed successfully**.

     ![ws name.](media/up6.png)
   
   >**Note:** It will take around five minutes for the script to execute.
   
10. Navigate to virtual machines and click on **AVD-HP01-SH-1**.

     ![ws name.](media/vd18.png)

11. Click on **Run command (1)** under **Operations**. Then select **RunPowerShellScript (2)**.

     ![ws name.](media-1/Ex6-task3-step11.png)
        
12. **Copy** the script given below and paste it by using **Ctrl + V** in the Powershell window. 

      >**Note :** **Please Do Not** run the script right away.

      ```
      # Variables
      $storageAccountName = "NameofStorageAccount"

      # Create Directories
      $LabFilesDirectory = "C:\LabFiles"

      if (!(Test-path -Path "$LabFilesDirectory")) {
         New-Item -Path $LabFilesDirectory -ItemType Directory | Out-Null
      }
      if (!(Test-path -Path "$LabFilesDirectory\FSLogix")) {
         New-Item -Path "$LabFilesDirectory\FSLogix" -ItemType Directory | Out-Null
      }

      # Download FSLogix Installation bundle
      $fsLogixZipPath = "$LabFilesDirectory\FSLogix_Apps_Installation.zip" # Store the full path

      if (!(Test-path -Path $fsLogixZipPath)) {
         try { # Add a try-catch block for better error handling
            Invoke-WebRequest -Uri "https://experienceazure.blob.core.windows.net/templates/aiw-avd-v3/FSLogix_25.06.zip" -OutFile $fsLogixZipPath -UseBasicParsing
         }
         catch {
            Write-Error "Failed to download FSLogix bundle: $_"
            return # Exit the script if the download fails
         }

         # Extract the downloaded FSLogix bundle
         function Expand-ZIPFile($file, $destination) {
            $shell = New-Object -ComObject shell.application
            $zip = $shell.NameSpace($file)
            foreach ($item in $zip.items()) {
                  $shell.Namespace($destination).CopyHere($item)
            }
         }

         Expand-ZIPFile -File $fsLogixZipPath -Destination "$LabFilesDirectory\FSLogix"
      }

      # Install FSLogix
      if (!(Get-WmiObject -Class Win32_Product | where vendor -eq "FSLogix, Inc." | select Name, Version)) {
         $pathvargs = "C:\LabFiles\FSLogix\x64\Release\FSLogixAppsSetup.exe /quiet /install" # No need for a scriptblock here
         Invoke-Expression $pathvargs # Use Invoke-Expression for executing the string
      }

      # Create registry key 'Profiles' under 'HKLM:\SOFTWARE\FSLogix'
      $registryPath = "HKLM:\SOFTWARE\FSLogix\Profiles"
      if (!(Test-path $registryPath)) {
         New-Item -Path $registryPath -Force | Out-Null
      }

      # Add registry values to enable FSLogix profiles, add VHD Locations, Delete local profile, and FlipFlop Directory name
      New-ItemProperty -Path $registryPath -Name "VHDLocations" -Value "\\$storageAccountName.file.core.windows.net\userprofile" -PropertyType String -Force | Out-Null
      New-ItemProperty -Path $registryPath -Name "Enabled" -Value 1 -PropertyType DWord -Force | Out-Null
      New-ItemProperty -Path $registryPath -Name "DeleteLocalProfileWhenVHDShouldApply" -Value 1 -PropertyType DWord -Force | Out-Null
      New-ItemProperty -Path $registryPath -Name "FlipFlopProfileDirectoryName" -Value 1 -PropertyType DWord -Force | Out-Null

      # Display script completion in the console
      Write-Host "Script Executed successfully"
      ```

      ![ws name.](media/uiupdate12.png)
 
      > **Note:** The above script will :
      >
      > i) Install FSLogix Profile Container application
      > 
      > ii) Configure the required registries
      > 
      >iii) Set the profile container location to the Azure Classic File Share location we created.

13. In line 2 in the script, replace **NameofStorageAccount** with **<inject key="Storage Account Name"></inject>** and then click on **Run** to execute the script.

    ![ws name.](media/jvm24.png)
       
15. Wait for some time for the script to execute.  Once done, it will show an output saying **Script Executed successfully**.

    ![ws name.](media/up6.png)
   
    >**Note:** It will take around five minutes for the script to execute.
  
16. Now search for **Azure virtual desktop (1)** in the search bar and select **Azure Virtual Desktop (2)** from the suggestions.

    ![ws name.](media/w1.png)
     
17. Click on **Users (1)**, then in the search bar paste your username **<inject key="AzureAdUserEmail"></inject> (2)** and then click on your user **(3)**.

    ![ws name.](media/vd19.png)
    
18. Switch to **Sessions (1)** tab, then select **Host Pools (2)** and click on **Sign out (3)**.

    ![ws name.](media-1/avd-47.png)
    
19. Click on **OK** to sign out the user from VMs.

    ![ws name.](media/avd-23.png)

    >**Note:** This will log off the user **<inject key="AzureAdUserEmail" enableCopy="false"/>** from both the session hosts, so that when the user signs in again to the session hosts, FSLogix will start functioning.
        
20. Now paste the below-mentioned link in your browser in the JumpVM, and enter your **credentials** to log in.

    ```
    aka.ms/wvdarmweb
    ```
    - Username: Paste username **<inject key="AzureAdUserEmail"></inject>**, then click on **Next**.
   
      ![ws name.](media/w24.png)

    - Password: Paste password **<inject key="AzureAdUserPassword"></inject>** and click on **Sign in**.

      ![ws name.](media/vd21.png)

      >**Note:** If there's a dialog box saying **Stay signed in**, then select the **No** option.

      ![](media/g10.png)

21. Click on the **Session Desktop** Desktop to launch it.

    ![ws name.](media/labinst24.png)

22. Select **Allow** on the prompt asking permission to access local resources.

1. On the **In Session Settings** screen, leave **Clipboard** checked so that you can copy and paste between your local device and the session, then click **Connect**.

    ![ws name.](media/lab4-10.png)

23. Enter your **Credentials** to access the desktop.

    > **Note:** If you are unable to sign in using the provided password, try logging in with the **AzurePassword** available in the **AzureCreds** file on the desktop.

    - Username: **<inject key="AzureAdUserEmail"></inject>**
    - Password: **<inject key="AzureAdUserPassword"></inject>**

      ![ws name.](media/lab4-2.png)
        
24. The desktop display will look similar to the screenshot below, showing **Please wait for the FSLogix Apps Services**.

    ![ws name.](media/lab6-7.png)
    
    >**Note:** This means that the user profile is being managed by FSLogix.

25. The virtual desktop will launch and look similar to the screenshot below.

    ![ws name.](./media/sessiondesktop.png)

26. At last, click on **User Account** and click on **Sign Out**.

    ![ws name.](./media/vd22.png)
   
## Exercise 4: Verifying the User profiles stored in Classic File Share

In this exercise, we will be accessing the Classic File Share to verify the user profiles stored in the .vhd format.

1. Return to the Azure Portal, search for **storage accounts (1)** in the search bar and click on **Storage Accounts (2)** from the suggestions.

      ![ws name.](media/up10.png)
    
2. Click on the storage account **<inject key="Storage Account Name" enablecopy="false"/> (1)**, then under security + networking blade click on  **Networking (2)**.

      ![ws name.](media/vd23.png)
   
3. Under **Public access (1)**, Click on **Enabled from all networks (2)**.

   ![ws name.](media/avd-25.png)

1. For **Public network access**, select **Enable (1)**, then for **Public network access scope**, choose **Enable from all networks (2)**, and finally click on **Save (3)**.

   ![ws name.](media/avd-25a.png)

   >**Note:** This will enable access to your storage account on the public network so that you can see the user profiles stored in the Classic File Shares.
    
4. Open the storage account we created earlier **(1)**, then select **Fileshare (2)** from the left side menu and the select **userprofile (3)** fileshare.

      ![ws name.](media/vd24.png)
      
5. Click on **Browse (1)**, and you will see the user **folder (2)** created in the Classic File Share, click on the folder.

      ![ws name.](media-1/avd-27.png) 

6. Now you will be able to see the user profile data stored in the fileshare in a **.vhd** format.

      ![ws name.](media-2/userprofile.png)

   >**Note:** It might take some time for the User Profile folder to appear in the Classic File Share. If you do not see the folder now, continue with the lab and check back later after completing it.

   > **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
- Scroll down and hit the Validate button in the lab guide for the corresponding task. If you receive a success message, you can proceed to the next task.
- If not, carefully read the error message and retry the step, following the instructions in the lab guide.
- If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.
<validation step="ad962332-47cc-4a42-899e-29bb55f5a4bd" />

## Summary

In this lab, you configured FSLogix profile containers for Azure Virtual Desktop by creating a storage account, enabling Entra Domain Services authentication, and setting up a profile Classic File Share. You then assigned appropriate access permissions, installed and configured FSLogix on the session hosts, updated AVD host pool settings, and verified successful roaming profile functionality across the virtual desktop environment.

Now, click on **Next** from the lower right corner to move on to the next page.

 ![Start Your Azure Journey](./media/Next.png) 
