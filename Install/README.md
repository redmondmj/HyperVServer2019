# Server Installation
We will use nested virtualization to create our lab environment. This means we will work with the following:
- Host Server - Windows Server 2019 (Your Azure Labs Server) &#x2935;
   - Guest Server - Hyper-V Server 2019 (no GUI) &#x2935;
      - Guest Client - Ubuntu Desktop

The "Host Server" will also serve as your management console machine (aka tech machine). We will use it to connect "remotely" to the Guest server and manage Hyper-V.


## Host Machine Prep
 - We'll most likely use an Azure Labs VM as our host machine, but this process would work on any machine with:
    - HyperV
    - 8Gb Ram
    - 40Gb Storage
 - Install HyperV and HyperV Manager
   - Use Server Manager to "Add Role"
      - Choose HyperV and accept defaults
   - Or Click Start>Run type `appwiz.cpl`
      - Click "Turn Windows Features On or Off"
      - Place a check next to HyperV and click ok
   - OR use Powershell: `Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart`
 - We'll need to allow our nested HyperV server to access CPU and network resources, to do that launch Powershell as administrator and run
   - `Set-VMProcessor -VMName Name -ExposeVirtualizationExtensions $true`
   - `Get-VMNetworkAdapter -VMName Name | Set-VMNetworkAdapter -MacAddressSpoofing On`
## Download 
 - We'll use the Free version of HyperV which is bundled with Windows Server Core (so no gui)
 - It's availabe from the [Evaluation Center](https://www.microsoft.com/en-us/evalcenter/evaluate-hyper-v-server-2019)

 ## Configure the network
 - Launch HyperV Manager and create a new "Internal" Virtual Switch named "Internal Virtual Switch"
 - This will add a new network adaptor to your host with a matching name.
 - Set a static IPv4 address on this adapter of 192.168.1.1, Mask 255.255.255.0, no gateway, no dns.

 ## Create Guest Server VM
 - Launch HyperV Manager and use the "Create New Virtual Machine" wizard to create a VM
 - Choose a descriptive name like "Guest HyperV Server"
 - Make sure you choose "Generation 2" hardware
 - Assign your new server 8Gb (8000Mb) of memory and use Dynamic Allocation
 - Connect to your "Internal Virtual Switch"
 - Default Hard disk is fine.
 - Connect to the ISO for installation source you downloaded above.
 - Start the vm and press any key to start to start the installation.
 - Choose "Custom" installation and select the unallocated disk to proceed with the install.
 - That's it!

## Configure Your Server
 - Connect to the console via Hyper V Manager
 - Set a password. I suggest using our standard password.
 - Note the "sconfig" menu, if you close this  you can launch is again from a command prompt by running `sconfig.exe`
 - Use the menu to complete the following tasks:
   1. Create a new administrative user with the same usename and password as your host machine i.e. itstudent
   2. Set the hostname, something simple but descriptive like `HYPERV-GUEST`
   3. Enable Ping replies.
   4. Enable REmote Desktop.
   5. Set a static IPv4 address of 192.168.1.100, mask 255.255.255.0, no gateway.
   6. Test pinging your Guest HyeperV server from the host server.


