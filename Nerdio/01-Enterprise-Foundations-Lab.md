### Lab 1
Creating an image source virtual machine

1. Log into the sandbox.
	• See your email for an access link.
2. Click the **Desktop Images blade**
3. Click **Add from Azure Library**
4. Configure the following:


 Name	|	 Enter a unique name. Save the name for later!
 Description	|	 Leave this blank
 Network	|	 Leave at default.
 Azure Image	|	Windows 11 (23H2) AVD + Microsoft 365 Apps - Gen2 (multi-session)
 VM Size	|	 D2s_v5
 OS Disk	|	 P10
 Resource Group	|	 Leave at default.
 Security Type	|	 Leave at default.
 Join to AD	|	 Uncheck this box.
 Do not create image	|	 Check this box.
 Set Time Zone	|	 Set to the local time zone.
 Optimize Disk Type when Desktop Image is Stopped	|	 Check this box.
 Provide custom credentials for local admin	|	 Toggle ON and Enter local administrator credentials for the image source VM.

 Imagem2
 
 
 ### Lab 3.1
 Creating an AVD workspace
 
1. Click the **Workspaces** blade
2. Click New Workspace.
3. Configure the following:

 Name(internal only)	|	 Enter a unique name. Save the name for later!
 Friendly Name (visible to users)	|	 Enter a unique name.
 Description	|	 Leave blank.
 Resource Group	|	 Leave at default.
 Location	|	select a Region Close to you​.
 
Imagem4
 
### Lab 3.2
Creating a dynamic host pool
 
1. Find your Workspace.
2. Click the dropdown arrow next to **Workspace** and select **Dynamic Host Pools**.
3. Click **New host pool**
4. Configure the following:

 HP Name(internal only)	|	Enter a unique name. Save the name for later!
 Description	|	 Leave blank.
 Resource Group	|	 Leave at default.
 Desktop Experience	|	 Multi user desktop (pooled)
 Directory	|	 Leave at default.
 FSLogix	|	 Leave at default.
 RDP Profile	|	 Leave at default.
 Name	|	 Enter a name.
 Network	|	 Leave at default.
 Desktop Image	|	Windows 11 (23H2) AVD + Microsoft 365 Apps - Gen2 (multi-session)
VM Size	|	 D2s_v5
OS Disk	|	 Leave at default (E10)
 Resource Group	|	Leave at default.
 Quick-Assign	|	 Leave Empty.

 Imagem6
 
 
### Lab 3.3
Creating session hosts
1. Find the host pool you created.
2. Click on **the Name of your Host Pool**
3. Click **New Host**
4. Configure the following:

 Host Count	|	 2
 Host Name	|	 Enter a name.
 Network	|	 Leave at default.
 Desktop Image	|	 Leave at default.
 VM Size	|	 Leave at default.
 OS Disk	|	 Leave at default.
 Resource Group	|	 Leave at default.
 Process Hosts in Groups of	|	2
  Number of failures before Abort	|	5
  
  Imagem7