# HyperV Manager Client/Server Configuration

## The Trick:

**Create account on the Guest Hyper-V Server that matches your Host server!!**

A succesfful connection requires you to configure both your **Server (HYPERV-GUEST - Server 2019 Guest)** and the **Client (Host Machine - Azure Labs)**. The steps for each are outlined below.

### Hyper-V Server 2019 (HYPERV-GUEST):

- Launch Powershell via cmd.exe
  - `powershell -Command "Start-Process PowerShell -Verb RunAs"`
- Allow remote access on public/private zones, enable firewall rules for CredSSP and WinRM:
  - `Enable-PSRemoting`
  - `Enable-WSManCredSSP -Role server`

### Host Machine - Azure Labs (Host):

- Add your Hyper-V server to the hosts file via Powershell as admin (replace IP and Hostname!)
  - `Add-Content -Path C:\Windows\System32\drivers\etc\hosts -Value "192.168.1.100 HYPERV-GUEST"`
  - OR edit manually via notepad.exe as admin
    - launch notepad.exe as administrator
    - edit `C:\Windows\System32\drivers\etc\hosts`  (note it has no extension so make sure you view all file types)
    - Add your server IP and hostname to the bottom of the file: i.e. "192.168.1.100    HYPERV-GUEST"
 - Add your Hyper-V server to TrustedHosts and allow authentication
   - launch Powershell as admin and run each of the following: 
   - `Enable-PSRemoting` (safe to ignore Firewall errors)
   - `Set-Item WSMan:\localhost\Client\TrustedHosts -Value "HYPERV-GUEST"`
   - `Enable-WSManCredSSP -Role client -DelegateComputer "HYPERV-GUEST"`
### Connect to Server
 - Launch **Hyper-V manager** (make sure you're logged in to the account that matches the server)
 - Select or right click "Hyper-V Manager">Action>**Connect to Server**
 - Choose other and enter the **hostname** of your Hyper-V server (i.e. HYPERV-GUEST)
 - If you connect successfully, you'll see menu options populate on the right
 - If you encounter a CredSSP issue, run the .reg file provided
