---
title: "VMware PowerCLI 10.0.0 Released with MacOS installation!!!"
date: 2018-03-07
draft: false
toc: false
images:
  - /vmware-powercli10.jpg
tags:
  - anksos
  - vmware
  - powercli
  - announcement
  - vexpert
---

Hello, after a long break I found a small amount of time to write really fast what made my excited last week (unfortunately I didn't had the time because of work to finish this blog when I started it)...

PowerCLI 10.0.0 Released!!! This means that finally I can work easily from my MacOS with the PowerCLI without having to do jungle stuff (and most of the times was not working).

First of all just to make sure, in order the PowerCLI 10.0.0 to work under your system it needs to have Powershell Core 6.0 (and later) already installed. You can install Powershell Core with the following:

`brew tap caskroom/cask`

and after this you can install the Powershell 6.0

`brew cask install powershell`

In order to upgrade the Powershell in later time (after some release or whatever just run the following commands: `brew update` and `brew cask reinstall powershell`

So after the installation have been finished you can proceed with the installation of the PowerCLI it self.

`Install-Module -Name VMware.PowerCLI -Scope CurrentUser`

And almost that was all about, the installation will be finished and you can get into the PowerCLI. In order to get into the powershell just type `pwsh` and then to check the available modules you can run the following command:

![vmware-powercli10.jpg](/vmware-powercli10.jpg)

Another thing that you may want to do is if you have been tired seeing the yellow messages on this version they changed the way certificates are handled when connecting to a vCenter server or ESXi host with the Connect-VIServer cmdlet. If your connection endpoint is using an invalid certificate (self-signed or otherwise), PowerCLI would previously return back a warning. The handling has been updated to be more secure and now return back an error.If you are using an invalid certificate, you can correct the error with the `Set-PowerCLIConfiguration` cmdlet. The parameter needing to be configured is `InvalidCertificateAction` and the available settings are Fail, Warn, Ignore, Prompt, and Unset.

The following code will configure the `InvalidCertificateAction` parameter to be ignored: `Set-PowerCLIConfiguration -InvalidCertificateAction Ignore`

Last but not least in order to update your modules now is that easy as running this: `Update-Module VMware.PowerCLI`.

Thanks and have a nice day!