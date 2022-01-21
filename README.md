# HyperVServer2019 - Client Configuration

## The Trick:

Create account on Hyper-V that matches your client machine (itstudent & It$tudent5)

### Hyper-V Server 2019 (Server 2 - Child):

Set the server's hostname
- Launch Powershell via cmd.exe
  - `powershell -Command "Start-Process PowerShell -Verb RunAs"`
- Allow remote access on public/private zones, enable firewall rules for CredSSP and WinRM:
  - `Enable-PSRemoting`
  - `Enable-WSManCredSSP -Role server`

### Host Machine (Server 1 - Host):

- If not already installed, Install HyperV Manager (only the manager!) using Powershell:
win+x>Programs And Features>Turn Windows Features On or Off>Hyper-V>Hyper-V Management Tools
  - Or via Powershell `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All`
- Add your Hyper-V server to the hosts file via Powershell as admin
  - ```Add-Content -Path C:\Windows\System32\drivers\etc\hosts -Value "`n192.168.0.101 `HYPERVMATT"```
  - OR via notepad.exe as admin
  - edit `C:\Windows\System32\drivers\etc\hosts`  (note it has no extension so make sure you view all file types)
  - Add your server IP and hostname to the bottom of the file:
  - i.e. "192.168.0.101   MATTHYPERV"
 - Add your Hyper-V server to TrustedHosts and allow authentication
  - via Powershell as admin (replace "fqdn-of-hyper-v-host" your  server's hostname i.e. HYPERVMATT) 
  - `Enable-PSRemoting`
  - `Set-Item WSMan:\localhost\Client\TrustedHosts -Value "fqdn-of-hyper-v-host"`
  - `Enable-WSManCredSSP -Role client -DelegateComputer "fqdn-of-hyper-v-host"`
### Connect to Server
 - Launch Hyper-V manager (make sure you're logged in to the account that matches the server)
 - Choose other and enter the hostname of your Hyper-V server (i.e. HYPERVMATT).
 - If you connect successfully, you'll see menu options populate on the right
 - You may still see an access denied message in the list, this will require additional troubleshooting
D - ouble click the VM to open. If you encounter a CredSSP issue, run the .reg file provided below.

What to submit:

Nothing. This will be verified in class :-)
