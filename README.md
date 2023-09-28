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


