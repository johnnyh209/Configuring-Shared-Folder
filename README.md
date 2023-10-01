# Configuring-Shared-Folder

The goal of this project is to share a folder from Windows Server 2019, and map that shared drive on the Windows 10 Enterprise systems. All of this is done using VirtualBox.

# Adding a New Disk Drive

I started off by adding a new drive to the Windows Server 2019 VM so as to isolate the shared folders from the main C: drive. To add a new drive, I right-clicked on the Windows Server 2019 VM and clicked on `Settings`. Within the `Settings` window, I clicked on `Storage`. This window will show you a list of the storage drives active in the VM. From here, there are two buttons that you can click: (1) the button with an image of a hard disk and a green plus sign at the top, or (2) the button with a floppy disc icon and green plus sign at the bottom, and then choosing `Hard Disk`. I highlighted these buttons in red squares; Refer to the image below:

![2  Add drive part 1 2](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/0cfd0d30-d082-4217-98ec-e3f651d1ab05)

The `Medium Selector` window will then open up where I clicked on the `Add` button on the top left corner. Doing so opened the `Create Virtual Hard Disk` wizard. I quickly run through the wizard, choosing `VDI (VirtualBox Disk Image)`, `Pre-allocate Full Size`, designate 35 GB of total disk space, and named the drive `Department_Share`. 

![3  Add drive part 2](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/4ca9443e-a3e6-4805-9403-a2ead69a0c9a)
![4  Add drive part 3 (2)](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/a5336ed5-3c4a-48aa-8cee-5994bcdca66f)
![7  Attach the new drive (by double clicking)](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/136a1dc0-8a1a-4b5e-bd21-2679a5c21ac5)

I then double-clicked on `Department_Share.vdi` to attach it to the VM. When I go back into the `Settings` page for my Windows Server 2019 VM, this new drive should be listed as seen in the image below:

![8  Show the drive lists](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/f39665fd-b54f-48f5-8ab5-a5cd05b3e4a3)

# Initializing, Partitioning, and Formatting Disk Drive

While I have added a new disk to the VM, I will still need to initialize it within the Operating System. Upon logging in, I opened `Computer Management`, clicked on `Storage`, and clicked on `Disk Management`. An `Initialize Disk` window automatically opened. Due to having only 1 disk that wasn't initialized, only one disk was listed and has already been selected automatically. I left the partition style as `MBR (Master Boot Record)`, and clicked `OK` to initialize the disk. 

![10  Initialize disk (this automatically shows up)](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/14579f6d-255a-4c44-96fd-842a3f435cee)

Now that the disk has been initialized, Windows is able to use it. I then needed to create a volume on this drive. This pretty much entailed me paritioning the drive (logically separating the drive) and formatting it (creating a file system). To make things simpler, I decided to create 1 partition with the maximum storage possible, assigned the drive letter `E`, and chose the NTFS file system (Microsoft's proprietary file system), and assigned the name of `Department Share`. With the drive formatting, I was able to see it listed in `This PC` in File Explorer.

![14  Format drive part 4](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/c16ad439-5ef1-47b8-9f1e-3a4a9edbada8)
![15  Formatted drive](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/1b364aa6-daf4-44c3-a88f-1738a1b9ed95)
![16  Formatted Drive](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/0c23901e-04c2-4e63-b6d7-e65875131f4f)

# Creating a Share

The next step was to create a new share. Server Manager will be used to do so. From the front page of Server Manager, I clicked into `File and Storage Services`. I then clicked on `Shares`, which opened up a list of active shares. I then right-clicked in the empty space below the list of shares, and clicked on `New Share...` to open the `New Share Wizard`. 

![18  Shares, Right click, New Share](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/d13a6e7f-4525-4885-86ef-4620d8b06dc8)

In the `New Share Wizard`, I chose `SMB Share - Quick` designated `E:` as the share location, and named the share `Accounting` (where I using a scenario where the Windows 10 Enterprise systems that will be connecting to this share are used by employees in the accounting department). 

![19  Choose SMB Share - Quick](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/3f5b2f34-df0c-45d3-8d9d-76a777d1aec7)
![21  Define share name v2](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/c2bdb8e0-08a4-49d4-980a-17288bd9fcdd)
![24  Summary v2](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/1abdf1a7-9b43-4c97-a2e9-4fa0c12b8e5a)

I also created a second share named `Personal`. This is so that each user will have their own personal shared drive. In total, I created two shares named `Accounting` and `Personal`.

![24  Summary v2](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/8a564b36-67e2-4575-a3fa-f5a1bcf9b0d5)
![25  List of Shares (With Personal share)](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/217ebc09-c7bf-4ed9-8107-0247e238ed1e)

To determine what users I want to have access to these shares, I will be creating security groups. Security groups allow me to manage many users based on the level of rights and access to resources that they should be granted. For example, while I may have an organizational unit (OU) named `Employee` where I have grouped all of the employees into, I may not want all of then to have access to a shared drive. By dividing that OU into different security groups, I have more granular control to what users can access. For this project, I created two security groups named `Accounting` and `Personal`; these names were chosen to correlate with their respective shared drives for easier management.

To create the security groups, I went into `Active Directory Users and Computers`, right-clicked on `Users`, clicked on `New`, and then clicked on `Group`. This will open the `New Object - Group` wizard. I filled out the `Group Name:` field and clicked the `OK` button to create the groups.

![27  Create Security Group part 1](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/82aba6f7-9c08-4b1c-a50b-ccbf55061fd0)
![28  Create Security Group part 2](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/c78fee85-6a73-497d-8e43-edffd277b795)
![29  Create Security Group part 3](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/0d89cb8a-7dca-499d-b429-2fd96eed4b3e)

Now that the two security groups have been created, I proceeded to add the appropriate users to these groups. For the `Accounting` security group, I will add both `Staff LabOrg` and `Staff2 LabOrg`. For the `Personal` security group, I will just be adding `Staff LabOrg`. To add members to the group, right-click on the name of the group, and click on `Properties`. In the `Properties` windows, go into the `Members` tab and click on `Add...`.  This will bring up the `Select Users, Contacts, Computers, Service Accounts, or Groups` window in which I entered in the name of the user I want to add into the group in the `Enter the object names to select` text box. You will have to do this on a per-user basis. The screenshots below only show the `Accounting` security group, but this process was replicated for any other security groups as well.

![32  Right click, properties](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/d12be55b-7997-44ae-9d3b-dfbb0ae1db1d)
![33  Members, Add](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/63dfc60b-faa9-4c57-b7bd-9ffc6a4d237e)
![34  Adding Staff LabOrg](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/c635413d-648c-47f0-9c54-4af7d63304f6)
![35  Staff LabOrg Added](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/0132716a-0119-4a9e-9a3e-12060e7ac4bb)

With the users added to the security groups, my next step was to configure their permission levels. I will be using `File Explorer` to do so. Navigate through the following: `File Explorer > This PC > Department Share (E:) > Shares`. This will show you a list of shares. For each share, I right-clicked and clicked on `Properties`, went into the `Security` tab and clicked on `Advanced`. 

![37  Share Properties part 1](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/ce6b62c2-90d5-43ad-9eb2-be08099132ed)

In the `Advanced` windows, I convered inherited permissions into explicit permissions using the `Disable inheritance` button. Doing so allowed me to remove the `Users` principals from the list so that not all of the users will have access to the share. 

![38  Disable inheritance](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/91242732-424b-4b2f-ad73-6cb35cc76985)

Clicking on the `Add` button opened a new window that allowed me to add a new principal. The word principal is also referred to as a security principal, which is an "entity that can be authenticated by the operating system, such as a user account, a computer account, or a thread or process that runs in the security context of a user or computer account, or the security groups for these accounts" ([Microsoft](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-principals)). I clicked on `Select a principal` to add the security groups I created earlier. For the the `Accounting` share, I added the Accounting security group and for the `Personal` share I added the Personal security group. Furthermore, I added `helpdesk` to both shares so that the IT techs will have access to troubleshoot the shares if a user reports any issues. For both of these I granted them the following permissions: Modify, Read & Execute, List folder contents, Read, and Write. 

![46  Adding Accounting Security Group](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/21a8b15d-875b-4c98-8fc1-b1a0e6e662d5)

As a reminder, the above steps are also repeated for the `Personal` shared folder as well. After the Principals/security groups have been applied, I clicked `Apply` to confirm the changes and clicked `OK` to close the window. I then went into the `Sharing` tab and clicked on the `Share...` button. This opens up the `Network access` window where I will change the permission level of `helpdesk`, `Personal`, and `Accounting` from Contribute to Read/Write. This ensures that when users from any of those group connect to the shares that they have the necessary permission to do so. 

![44  Change to readwrite](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/6fd37517-6852-4d9a-afe8-6e6b8bc116a6)
![45](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/485212a1-ffb7-4423-90f0-1209c820fcf6)

With that, the sharing of the folders should be complete.

# Connecting To The Shares

This last stage required me to map the users' systems to the shared folders. But first, I needed to make sure that the systems are enabled to detect and connect to shares, and share files. To do so, navigate through the following path: `Control Panel > Network and Internet > Network and Sharing Center > Advanced sharing settings`. From here, I made sure that `Turn on network discovery` and `Turn on file and printer sharing` are both enabled. 

![48  Turn on network discovery and network sharing](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/f96f484a-f4e4-45d0-b0c6-9ff9277b3a30)

With those settings enabled, I head into `File Explorer`. On the menu bar, I clicked on `Computer`, and then clicked on `Map network drive`. 

![49  Map network drive](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/b93aee9b-234b-411e-9a2e-d1672a2bd0e2)

Doing so opened the `Map Network Drive` window. Here, I chose a drive letter, `Z:`, and entered in the network path of the shared folder. To find this network path, go onto the system of which the shared folder was created on. Enter into the shared folder's `Properties` page, and into the `Sharing` tab like so:

![43  Sharing, Share](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/677f8df2-139c-4a52-8908-4cc75c0bb26a)

After entering in the network path, I kept the `Reconnect at sign-in` enabled and kept `Connect using different credentials` disabled and pressed `OK`. 

![50  Map network drive part 2](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/51dc863c-f409-4540-aaad-f6016cd578a5)

It was at this stage, however, that I couldn't map the drive. I received the error: "Windows cannot access \\server-name\folder-name". I wasn't sure what was causing the issue. I double checked to make sure that the shared folder has been shared. In an attempt to resolve this issue, I tried enabling SMB 1.0, but still continued to receive the same error. 

![51  Turn on SMB](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/50836844-e78a-4ffc-a9a7-203eca831d5e)

Furthermore, to make sure that I was able to communicate with Windows Server in which the shared folder resides, I opened `Command Prompt` and used the `Ping` command and found that the packets were sent from the Windows 10 Enterprise system to the Windows Server 2019 system. I then, however, noticed that the device name of Windows Server 2019 and the server name were different. The device name (as found in the `Local Server` page of Server Manager) was `WindowsServer-2019` while the server name (found in `All Servers` page of Server Manager) was `WINDOWSSERVER-2`. My thought process then turned to the possibility of a DNS resolution issue. That there is a possibility of issues occurring during resolution of the server name and the IP address due to the differing device name and server name on the share's network path. I first tried to connect the share using the following network path: `\\IP-Address\folder-name` where IP Address is the IP of Windows Server 2019. Doing so, I was able to successfully connect the share. I then tried using `\\WindowsServer-2019\folder-name` rather than `\\WINDOWSSERVER-2\folder-name` and that allowed me to connect the shares successfully as well. However, I could not figure out why the given network path didn't work.

I decided, instead, to delete all of the current VMs and start from scratch. I once again created 1 Windows Server 2019 VM, and naming it `Server2019`. This time, I noticed that the device name and server name matched. I then created 3 Windows 10 Enterprise systems, set up the active directory, and recreated the same shares, repeating the steps above. When connecting to the `Accounting` share this time, I received another error that prevented me from connecting to the share. While looking through the `Properties` page of the `Accounting` and `Personal` shares, the `Everyone` group was missing. To add `Everyone` to the permissions list, I went into the `Security` tab of the share's `Properties` page, clicked on `Edit...` which opened up a new window as seen below:

![VirtualBoxVM_Vr46VzV9wU](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/299d70d8-bfe0-473a-b7b3-aff6c4906022)

I then clicked on `Add...`, typed in `Everyone` into the text box, and clicked `OK` to add the `Everyone` group to both the `Accounting` and `Personal` shares. After doing so, I first reattempted to connect to the `Accounting` share and was successful. By going to the `This PC` page on `File Explorer`, the shared drive should appear under `Network Locations`.

![VirtualBoxVM_enFgz1bwnR](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/fc62048a-8e4d-4945-b679-dbb2775e89a6)

I connected the `Personal` share differently. My plans for this share was to have it function as a home folder or personal share. What I mean by this is that within a department in a company, and in this case the Accounting department, where everyone in the department has access to the shared drive, each user will have their own drive named after them. I will be connecting the `Personal` share with the Windows 10 system using Server Manager on the Windows Server 2019 system. In Server Manager, I opened up `Active Directory Users and Computers`. I then located the users, and in my case, they were placed in the `Employee` OU. I then right clicked on the users, and then selected `Properties`. In the `Properties` page, I went to the `Profile` tab. In the `Profile` tab, under the `Home folder` section, I selected `Connect`, assigned a drive letter, and entered in the network path. The network path was the following: `\\Server2019\Personal\%username%`. This will not only connect the users to the `Personal` share, but will create a folder for the user within the share named after the user's username.

![VirtualBoxVM_5hataBZfh6](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/c9d20c3b-4a1e-4f9b-9d1e-5c69b5e9919b)

Once that is connected, I went back into each user's system and opened `File Explorer` to confirm that the `Personal` share has successfully connected.

![VirtualBoxVM_v7nexN4TZb](https://github.com/johnnyh209/Configuring-Shared-Folder/assets/33064730/c45899d5-ff20-4a30-988b-de38754f010a)
