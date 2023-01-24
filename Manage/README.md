# HyperV Manager Client/Server Configuration

## The Trick:

**Create account on the Guest Hyper-V Server that matches your Host server (in this case it is technically the client machine)!!**

A succesffuly connection requires you to configure both your **Server (HYPERV-GUEST - Server 2019 Guest)** and the **Client (Host Machine - Azure Labs)**. The steps for each are outlined below.

### Hyper-V Server 2019 (HYPERV-GUEST):

Set the server's hostname
- Launch Powershell via cmd.exe
  - `powershell -Command "Start-Process PowerShell -Verb RunAs"`
- Allow remote access on public/private zones, enable firewall rules for CredSSP and WinRM:
  - `Enable-PSRemoting`
  - `Enable-WSManCredSSP -Role server`

### Host Machine - Azure Labs (Host):

- If not already installed, Install HyperV Manager using Powershell:
win+x>Programs And Features>Turn Windows Features On or Off>Hyper-V>Hyper-V Management Tools
  - Or via Powershell `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All`
- Add your Hyper-V server to the hosts file via Powershell as admin
  - ```Add-Content -Path C:\Windows\System32\drivers\etc\hosts -Value "`n192.168.0.101 `HYPERVMATT"```
  - OR via notepad.exe as admin
  - edit `C:\Windows\System32\drivers\etc\hosts`  (note it has no extension so make sure you view all file types)
  - Add your server IP and hostname to the bottom of the file:
  - i.e. "192.168.0.101   HYPERV-GUEST"
 - Add your Hyper-V server to TrustedHosts and allow authentication
   - via Powershell as admin (replace "fqdn-of-hyper-v-host" your  server's hostname i.e. HYPERV-GUEST) 
   - `Enable-PSRemoting` (safe to ignore Firewall errors)
   - `Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"`
   - `Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"`
### Connect to Server
 - Launch **Hyper-V manager** (make sure you're logged in to the account that matches the server)
 - Select or right click "Hyper-V Manager">Action>**Connect to Server**
 - Choose other and enter the **hostname** of your Hyper-V server (i.e. HYPERV-GUEST)
 - If you connect successfully, you'll see menu options populate on the right
 - If you encounter a CredSSP issue, run the .reg file provided
