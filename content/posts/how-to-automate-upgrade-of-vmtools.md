---
title: "How to automate the Updgrade of VMware Tools to multiple Virtual Machines"
date: 2015-12-23
draft: false
toc: false
images:
tags:
  - anksos
  - vmware
  - operations
  - powercli
---

Hey, I had this issue before some days, we would like to do some patching on Windows Virtual Machines and upgrade the VMware tools. The problem came when we didn't want to do more than 1 reboot. Also we were kind of afraid to use the -NoReboot action with upgrading the VMware tools because of network disruption due to the Upgrade. The solution was to write a script, that will change the VMware Tools Upgrade Policy from manual to Power Cycle. So, after the patching and due to the reboot the VMware tools would be installed automatically.

Also have in mind, that the script takes a text file with all the Virtual Machines I wanted to do the change and parses it one-by-one. You could do the same with .csv file, the difference is that you must use the Import-Csv command before the foreach.

In order to run the script you must change the execution policy to Unrestricted. You can do this with the command `Set-ExecutionPolicy RemoteSigned`

Thanks for the help to LucD from VMware community, of changing the code from `$vm in $VirtualMachines` to `$vm in (Get-VM -Name $VirtualMachines)`.

```powershell
$vCname = Read-Host “Enter the vCenter or ESXi host name to connect:”
if (-not (Get-PSSnapin VMware.VimAutomation.Core -ErrorAction SilentlyContinue)) {
    Add-PSSnapin VMware.VimAutomation.Core
}
Connect-VIserver $vcname
$VirtualMachines = Get-Content "VMList.txt"
foreach ($vm in (Get-VM -Name $VirtualMachines)){
    if($vm.config.Tools.ToolsUpgradePolicy -ne "UpgradeAtPowerCycle") {
        $config = New-Object VMware.Vim.VirtualMachineConfigSpec
        $config.ChangeVersion = $vm.ExtensionData.Config.ChangeVersion
        $config.Tools = New-Object VMware.Vim.ToolsConfigInfo
        $config.Tools.ToolsUpgradePolicy = "UpgradeAtPowerCycle"
        $vm.ExtensionData.ReconfigVM($config)
        Write-Host "Update Tools Policy on $vm completed" -ForegroundColor green
    }
    else {
        Write-Host "$vm couldn't found on vCenter Server" -ForegroundColor red
    }
}
```

If you want to check before and after the change the version of the VMware tools based on the same text file you can run the following script:

```powershell
$vCname = Read-Host “Enter the vCenter or ESXi host name to connect”
if (-not (Get-PSSnapin VMware.VimAutomation.Core -ErrorAction SilentlyContinue)) {
    Add-PSSnapin VMware.VimAutomation.Core
}
# We create a new VIProperty in order to take also the Guest VM tools version
New-VIProperty -Name ToolsVersion -ObjectType VirtualMachine `
-ValueFromExtensionProperty 'Config.tools.ToolsVersion' `
-Force
# We create a new VIProperty in order to take also the Guest VM tools status
New-VIProperty -Name ToolsVersionStatus -ObjectType VirtualMachine `
-ValueFromExtensionProperty 'Guest.ToolsVersionStatus' `
-Force
Connect-viserver $vcname
$VirtualMachines = Get-Content "VMList.txt"
foreach ($vm in (Get-VM -Name $VirtualMachines)) {
    Get-VM -Name $vm | Select Name, Version, ToolsVersion, ToolsVersionStatus
}
```