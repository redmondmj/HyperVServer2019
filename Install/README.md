# Server Installation
## Host Machine Prep
 - We'll most likely use an Azure Labs VM as our host machine, but this process would work on any machine with:
    - HyperV
    - 8Gb Ram
    - 40Gb Storage
 - Install HyperV and HyperV Manager
 - Click Start>Run type `appwiz.cpl`
 - Click "Turn Windows Features On or Off"
 - Place a check next to HyperV and click ok
 - Your host machine will likely need to restart
 - We'll need to allow our nested HyperV server to access CPU and network resources, to do that launch Powershell as administrator and run
   - `Set-VMProcessor -VMName Name -ExposeVirtualizationExtensions $true`
   - `Get-VMNetworkAdapter -VMName Name | Set-VMNetworkAdapter -MacAddressSpoofing On`
## Download 
 - We'll use the Free version of HyperV which is bundled with Windows Server Core (so no gui)
 - It's availabe from the [Evaluation Center](https://www.microsoft.com/en-us/evalcenter/evaluate-hyper-v-server-2019)

 ## Create Server VM
 - Launch HyperV Manager and use the "Create New Virtual Machine" wizard to create a VM
 - Choose a descriptive name like "HyperV Server - Child"
 - Make sure you choose "Generation 2" hardware
 - Assign your new server 8Gb (8000Mb) of memory and use Dynamic Allocation
 - Connect to your "Default Switch"
 - Default Hard disk is fine.
 - Connect to the ISO for installation source you downloaded above.
 - Start the vm and press any key to start to start the installation.
 - Choose "Custom" installation and select the disk to proceed with the install.
 - That's it!

## Configure Your Server
 - Connect to the console via Hyper V Manager
 - Set a password. I suggest using our standard password.
 - Note the "sconfig" menu, if you close this  you can launch is again from a command prompt by running `sconfig.exe`
 - Use the menu to complete the following tasks:
   1. Create a new administrative user with the same usename and password as your host machine i.e. itstudent
   2. Set the hostname, something simple but descriptive like `HYPERV-CHILD`
   3. Enable Ping replies.
   4. Enable REmote Desktop.
   5. Note the IP address of your server!


