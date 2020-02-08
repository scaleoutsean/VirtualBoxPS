# **VirtualBoxPS**
A PowerShell module to manage a Virtual Box environment. This module is being developed using VirtualBox API v6.1.0.

---

# IMPORTANT

**This module is on hold for the time being. This is not to say it won't be completed, but there seems to be little interest and life still goes on. Work will resume whenever possible. If you send any messages regarding this project, don't expect an immediate response.**

This module is currently in a **very** early development phase. You should not try to use it in a production environment. If you do use it, you do so at your own risk! Please realize that it uses the VirtualBox Web Service in plain text mode by default. If you provide any information to this web service, it will **NOT** be encrypted. Adding SSL/TLS support is possible, but has not been added natively to this module's installer. This, and other milestones, will likely need to be a request to the developers to be enabled by default.

---

### **TOPIC**
about_VirtualBoxPS

### **SHORT DESCRIPTION**
These functions are wrappers to the APIs that you can use to manage a virtual machine infrastructure based on the VirtualBox application, which you can download for free from Oracle at [VirtualBox's homepage](http://www.virtualbox.org).

### **LONG DESCRIPTION**
The free virtualization application from Oracle, VirtualBox, offers an application SDK which at this point does not include native PowerShell support. This module is an attempt to utilize the VirtualBox Web Service, and the VirtualBox.VirtualBox COM object to perform common management tasks for virtual machines running in the VirtualBox environment.

This module currently requires PowerShell 5.0 (use $PSVersionTable.PSVersion to check your primary version and $Host.Version to check the version of each host type like PS vs. PS_ISE) or higher to run. However, like any ongoing project, all of this is subject to change without notice. The module has currently been tested on Windows 7 and Windows 10, and is being developed on Windows 7.

### **THE GOAL**
This module is being designed to provide as much of the capability as VirtualBox's VBoxManage.exe command line tool or better. The idea is that the module will support greater security, portability, and expandability. That being said, the primary API being used will be the VirtualBox Web Service. VBoxWebSrv.exe can be launched using certificate based encryption, so it can be accessed using https (still working on a certificate provider for this). It's also web based so it can be setup to be accessed remotely.

COM support is currently being integrated. There may still be bugs which are not present in the web service operation of this module. Also, new bugs may have introduced into the web service operation. Please be aware of this before attempting to use this module on anything important.

### **CONTRIBUTION**
If you would like to contribute to this project, visit [our thread](https://forums.virtualbox.org/viewtopic.php?f=34&t=54027) at the VirtualBox API Forum. Also, note that there is at least one other project on GitHub pursuing a similar goal, which is also posted in that thread. It is planned to merge [NNVirtualBoxPowerShellMode](https://github.com/ajbrehm/NNVirtualBoxPowerShellModule) and [VirtualBoxPS](#-virtualboxps) in the future after local bugs have been ironed out. This will more than likely be done in the form of a Web Service/COM alternative.
	
### **INSTALLATION INSTRUCTIONS**
For this module to work, a simple "installation" script has been included for Windows to copy all of the required files to the correct locations. Install-VirtualBoxPS.ps1 will need to have vboxweb.wsdl, vboxwebService.wsdl, VirtualBox API Web Service.xml, VirtualBoxPS.psd1, and VirtualBoxPS.psm1 in the same folder. To install, run Install-VirtualBoxPS.ps1 with an elevated PowerShell session. This script will automatically copy the files into the following locations:
	
	$($env:VBOX_MSI_INSTALL_PATH)sdk\bindings\webservice\vboxweb.wsdl
	$($env:VBOX_MSI_INSTALL_PATH)sdk\bindings\webservice\vboxwebService.wsdl
	$($env:VBOX_MSI_INSTALL_PATH)sdk\MediumFormat.json
	$($env:VBOX_MSI_INSTALL_PATH)sdk\MediumFormatPso.json
	C:\Windows\system32\WindowsPowerShell\v1.0\Modules\VirtualBoxPS\VirtualBoxPS.psd1
	C:\Windows\system32\WindowsPowerShell\v1.0\Modules\VirtualBoxPS\VirtualBoxPS.psm1
	
	
Additionally, a new startup task will be created in Task Scheduler by importing the xml file:
	
	\Pseudo Services\VirtualBox\VirtualBox API Web Service
    
### **MODULE USE**
Positional parameters have been added to the module to assist dual operation of the module. The module will import using the web service as the default API provider using:
	
	Import-Module VirtualBoxPS

To login to the web service while importing the module you must provide valid credentials (the $protocol, $address, and $tcpport parameters are optional):
	
	Import-Module VirtualBoxPS -ArgumentList 'WebSrv',$creds,$protocol,$address,$tcpport

To import the module using the COM API provider:
	
	Import-Module VirtualBoxPS -ArgumentList 'Com'

These commands are only imported when using the web service:
	
	Start-VirtualBoxSession
	Stop-VirtualBoxSession
	Start-VirtualBoxWebSrv
	Stop-VirtualBoxWebSrv
	Restart-VirtualBoxWebSrv
	Update-VirtualBoxWebSrv

These commands are only imported when using the COM:
	
	Open-VirtualBoxVMConsole
    
### **CUSTOM OBJECTS**
The Get-VirtualBoxVM is used to get virtual machine objects and most other functions in the module will take pipelined input from this function. Get-VirtualBoxVM writes an array of custom objects to the pipeline with commonly used properties.

### **KNOWN ISSUES**
* Some of the custom parameters for Import-VirtualBoxOVF and Export-VirtualBoxOVF do not work properly
	>Continuing to bug test against SDKRef

### **WORK AROUNDS**
* Submit-VirtualBoxVMProcess will crash and abort your VM if VBox tools closes before the command completes.
	>(Workaround) If you are sending a command that will cause this (shutdown commands), use the supplied -NoWait switch.
    
### **VERSION**
	0.5.2.33
	February 8, 2020
    
### **SEE ALSO**
	Get-VirtualBox
	Start-VirtualBoxSession
	Stop-VirtualBoxSession
	Start-VirtualBoxWebSrv
	Stop-VirtualBoxWebSrv
	Restart-VirtualBoxWebSrv
	Update-VirtualBoxWebSrv
	Get-VirtualBoxVM
	Suspend-VirtualBoxVM
	Resume-VirtualBoxVM
	Start-VirtualBoxVM
	Stop-VirtualBoxVM
	New-VirtualBoxVM
	Remove-VirtualBoxVM
	Import-VirtualBoxVM
	Edit-VirtualBoxVM
	Set-VirtualBoxVMGuestProperty
	Remove-VirtualBoxVMGuestProperty
	Open-VirtualBoxVMConsole
	Enable-VirtualBoxVMVRDEServer
	Disable-VirtualBoxVMVRDEServer
	Edit-VirtualBoxVMVRDEServer
	Connect-VirtualBoxVMVRDEServer
	Import-VirtualBoxOVF
	Export-VirtualBoxOVF
	Get-VirtualBoxDisk
	New-VirtualBoxDisk
	Import-VirtualBoxDisk
	Remove-VirtualBoxDisk
	Mount-VirtualBoxDisk
	Dismount-VirtualBoxDisk
	Submit-VirtualBoxVMProcess
	Submit-VirtualBoxVMPowerShellScript
