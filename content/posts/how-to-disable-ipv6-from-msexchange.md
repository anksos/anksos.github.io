---
title: "How to disable IPv6 from MS Exchange 2010/2013 server - Gmail issue"
date: 2013-11-26
draft: false
toc: false
images:
tags:
  - anksos
  - ms-exchange
  - regedit
  - windows-server
---

Hello folks,

After an upgrade of SPF in Gmail servers, seems that Gmail is blocking e-mails with dynamic IPv6 addresses which have not PTR records in the RDNS. Because of that Gmail is also checking for every IPv6 address that is containing in the e-mail headers and not only the IP that is assigned on this server (the IP of the server that sending the e-mail to the Gmail's mail exchanges (MX)).

The solution is to completely disable the IPv6 addresses on the Edge servers (MX) of your Exchange Infrastracture. See below:

1. Regedit and go to `HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\TCPIP6\\Parameters\\`  
    1. Edit or Create a `DWORD (32-bit)` with the name `"DisabledComponents"` 
    1. By default the value is `0x00000000 (0)`, you have to **right click** and modify this value with the Hexademical `0xffffffff` (you have to erase the `0` and write only the `"ffffffff"`), in Decimal it is the `"4294967295"` value. 
1. You have to Restart your servers (edge exchange servers).
1. If you want to verify that you have set it up correctly just open a cmd and run this:
    1. `C:\\> reg query HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\TCPIP6\\Parameters /v DisabledComponents`
    1. The output you must take is this: `HKEY\_LOCAL\_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\TCPIP6\\Parameters DisabledComponents    REG\_DWORD    0xffffffff`

That's all. Have a nice day.