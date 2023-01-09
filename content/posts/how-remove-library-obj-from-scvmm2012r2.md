---
title: "How to remove a library object &quot;VM Template&quot; of a VHD(X) from SCVMM 2012 R2 after you getting error ID: 848"
date: 2014-09-12
draft: false
toc: false
images:
  - /2A8D9.PNG
  - /B35E0.PNG
tags:
  - anksos
  - microsoft
  - error-id-848
  - hyper-v
  - windows-server
---

Hello folks again,

Today I wanted to delete a VM Template from the Library server but after deleting the VM Template I took an error with the `ID: 848`.

Basically the correct way to do this is to go in your Library Server (through SCVMM console), Delete the VM Template that you want and then from the Library Share you delete the vhd/vhdx file without problem. But sometimes, maybe you will get this error:

![](/2A8D9.PNG)

The workaround to delete the dependency between the VM Template and the vhd/vhdx file is to open an SCVMM Powershell console (if you open it from the button inside the SCVMM console it will ask for administrator user credentials, so you must login with a user that has administrator previleges) and execute the below two (2) simple powershell commands:

![](/B35E0.PNG)

The first command is to save the template ID on the varialbe `$temp1` and the second is the `Remove-SCVMTemplate` which actually removes the template that you add it on the first command. It's better to do it like this because, if you want to mass delete some Temporary templates (that have stack on your SCVMM dependencies). So if you change the command with something like this `Temporary *` you will delete all the Temporary dependencies.

P.S. Inside the double quotes you write the Temporary template with the ID exact as the error above shows it. After doing this try to Delete again from the Library share the vhd/vhdx you want.

If you need any advice/or something please contact me. Thank you!