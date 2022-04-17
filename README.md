# Deploy Intune Win32 Applications automated with chocolatey
## Description
With this script you automate the APP-Deployment.
You can publish Chocolatey APPs with intune and keep them up2date.
The Script automaticly creates the application, and installs automaticly the required chocolatey application.
It also downloads the Application-Icons from Chocolatey to make it watching nice in the Customer Portal, if you dont want to install it automaticly
The Application is published automaticly for every user.
You can also make it be installed automaticly

For keeping the installed software updated, the Installer creates a Scheduled Task for every application with fires a "choco upgrade <application> -y" on every startup for every deployed application

## Install-Guide
1. Start powershell with elevated rights.
2. Download The Script and Save it for example to C:\admin
3. Install and Import all required Modules, chocolatey and Software for icon-converting.
```powershell

Install-Module IntuneWin32App -Force
Import-Module IntuneWin32App
Install-Module Microsoft.Graph.Intune -Force
Import-Module Microsoft.Graph.Intune
Install-Module -Name chocolatey
Import-Module -Name chocolatey
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
choco install inkscape -y

```
4. Connect to Intune Graph API (Multi-Tenant Use is possible
```powershell
Connect-MSIntuneGraph -TenantID "contoso.com"
```
5. Import Downloaded Script (in this sample from C:\admin)
```powershell
import-module "C:\admin\ChocoIntuneDeployment-Main.ps1"
```
## How to Use
### Install Chocolatey Manually (optional)
This step is only required, if you want to install it manually or the automatic process is not working.
Normaly, chocolaty will be installed automaticly befor you publish the first application
```powershell
install-chocolatey
```
### Create a new Application
Installing a new Application is done simply by running the main function
```powershell
New-IntuneWin32ChocoApplication
```
You will be prompted to enter a pattern of what Software you want to install.
After that select the wanted software.

By default the Software is published but not autoinstalled for every user in the intune Tenant.
To make it automaticly installed add the Switch -Required
```powershell
New-IntuneWin32ChocoApplication -Required
```

If you know the exact chocolatey name, you can also deploy the software without any userpromt
```powershell
New-IntuneWin32ChocoApplication "googlechrome"
```
