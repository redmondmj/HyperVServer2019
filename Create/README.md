# Creating Guests

When create guest VM's on your nested HyperV Server 2019 (HYPERV-GUEST) it's important to remember that you won't have direct access to the local file system from the client you are using to manage the server.

There are a couple techniques you can use to gain access to the file system.

## Allow Remote Connections to Shared Folders

- Connect to HyperV Server 2019 (HYPERV-GUEST) via HyperV Manager
- Open the command prompt or Powershell and run
- `netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=Yes`
- This will allow access to the default share of the system volume - C:

- On your host machine (Azure Labs Machine) click start, type `run` and hit enter
  - Or open file manager and click in the address bar
- enter the path to your network share ```\\HYPERV-GUEST\c$``` *note the $, this is a hidden share
- You should now have access to the file system of your child server. 
- Create an ISO folder and upload any disk images required for your guest VM's i.e. [Ubuntu Server](https://releases.ubuntu.com/22.04.3/ubuntu-22.04.3-live-server-amd64.iso)
  - You can even download an ISO with PS! Easy as:
    1. `$Path="C:\Users\itstudent\Downloads\Ubuntu.iso"` or `$Path="\\HYPERV-GUEST\C$\ISO\Ubuntu.iso""`
    2. `$URL="https://releases.ubuntu.com/22.04.3/ubuntu-22.04.3-live-server-amd64.iso"`
    3. `Invoke-WebRequest -URI $URL -OutFile $Path`

## Create New Guest VM

- Use the wizard in HyperV Manager
- Post creation considerations - edit settings
  - Firmware - review/change the boot order
  - Security - For linux VM's you may need to disable secure boot
  - SCSI Controller - add/edit DVD Drive
    - if you already have a DVD drive you may need to specify the boot media
    - click "Image File:" and browse to your ISO


