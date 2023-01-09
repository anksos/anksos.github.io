---
title: "Windows Server 2012/2012 R2 Hyper-V Replica between two different domains (stand alone and cluster)"
date: 2014-06-26
draft: false
toc: false
images:
tags:
  - anksos
  - active-directory
  - failover-cluster
  - windows-server
  - hyper-v
  - hyper-v-replica
---

Hello folks,

Our scenario is about Hyper-V Replica between two different domains, one with the stand alone Hyper-V 2012 Nodes (primary site) and the other with a Clustered Infrastructure of Hyper-V 2012 R2 (repilica site).

After a lot of trial and error with some configurations for this scenario i ended to the following config.

First of all this config is based on Certificates and not Kerberos because of the different domains between the Hyper-V nodes. Bellow you will find the steps to make it work, let's start.

1. We must create the Hyper-V Replica Broker (on the Replica site, where our cluster nodes exists).
    1. Open the Failover Cluster
    1. Configure Role
    1. Select Hyper-V Replica Broker and hit **Next**
    1. Add the Name of the replica broker e.g replicabroker (Note: this will be translated as an fqdn and also will be add on the domain controller the a record `replicabroker.domain.local`)
    1. Add the IP for the Replica Broker (you have to add one unused IP from your local/public network (of course must be an ip from the same network as hosts and generally a routable IP) this will be a Virtual IP for the Host so you don't need to add another network interface)
    1. Then click **Finish**
1. We must open the Inbound Replica Broker rule on the Advanced Firewall of all Hyper-V nodes (the rule has been automatically created and named as: `Hyper-V Replica HTTPS Listener (TCP-In))`
1. We must create the certificates and the CAs, to do that we will use the `makecert.exe` tool. This tool you can download it if you don't have it from [here](http://msdn.microsoft.com/en-us/library/windows/desktop/aa386968%28v=vs.85%29.aspx)`
1. After you install and locate the `makecert.exe` utility copy & paste it to the Primary site on the Primary server node you want to enable replication.
1. Run the following command from an elevated command prompt (cmd) on the primary server. This commands creates a self-signed root authority certificate. Also installs a Certificate in the root store of the local machine and is saved as a file locally to the current directory:
    1. In the primary server run this: `makecert -pe -n "CN=PrimaryRootCA" -ss root -sr LocalMachine -sky signature -r "PrimaryRootCA.cer"`
    1. `makecert -pe -n "CN=" -ss my -sr LocalMachine -sky exchange -eku 1.3.6.1.5.5.7.3.1,1.3.6.1.5.5.7.3.2 -in "PrimaryRootCA" -is root -ir LocalMachine -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12 .cer` (this will have to do it as times as the stand alone Hyper-V nodes we need to enable replication, the only thing we must change is the and the ).
    1. We run one more time the upper command with the difference instead of the will add `*.domain.local` and in the you add something to remembers you that is for the replica site so lets say it `ReplicaSite.cer`.
1. We need to export the replica site certificate that we created in earlier step so we open the `MMC -> Add/Remove Snap-In -> Add Certificate -> Computer Account -> Next,Next & Finish`
1. We go to `Personal -> Certificates` and with right-click `Export the ReplicaSite Certificate`. We proceed with Export including the key and the file will be as `.pfx` also you have to give a password for the certificate.
1. After this we copy and paste this exported certificate, the certificate of the CA that we have been created at earlier step (this will be located on the current directory that you run the cmd commands) on all Hyper-V Cluster nodes of the Replica Site (a good directory is `C:\`).
1. We open an elevated command prompt (cmd) and we run the certutil: `certutil -addstore -f Root "C:\PrimaryRootCA.cer"` (this will have to do it on every Hyper-V cluster node in the Replica Site).
1. After this we have to import the ReplicaSite certificate that we have exported as `.pfx` from the Primary Site to the Hyper-V Cluster nodes (again we must do it on every Hyper-V Cluster node in our Replica Site). To do this we open `MMC -> Add/Remove Snap-In -> Add Certificates -> Computer Account -> Next, Next & Finish`
1. Then we navigate to `Personal -> Certificates -> Right-click and Import -> `You must give the password that you have setup on the step 7.
1. Before we proceed with the replica configuration we have to disable the Revocation Check. This we have to do it on every Hyper-V server (primary site (stand alone nodes) and replica site (cluster nodes). To do this we must run this two commands bellow from an elevated command prompt (cmd):
    1. `reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Virtualization\FailoverReplication" /v DisableCertRevocationCheck /d 1 /t REG_DWORD /f`
    1. `eg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Virtualization\Replication" /v DisableCertRevocationCheck /d 1 /t REG_DWORD /f`
1. After we have finished with the Import of the CA certificate and the ReplicaSite certificate and also with the disable of the Revocation Check we must select it to the Replica Broker configuration. To do this follow the steps bellow:
    1. Open The Failover Cluster and Navigate to `Roles`
    1. Right-click on the replica broker and select `Replication Settings`
    1.  Check the `Enable this Cluster as a Replica Server`
    1. Check the `Use certificate-based Authentication (HTTPS)`
    1. Specify the port on 443 (leave it as it is)
    1. Now you must select the ReplicaSite Certificate that we have created and imported it to the Hyper-V Cluster nodes
    1. Specify the Cluster Storage directory
    1. And click OK
1. Now you have to Enable Replication in a VM on the Primary Server **15.** To do this following the instruction bellow:
    1. Right-Click on the VM you want to replicate and  select `Enable Replication`
    1. Just hit `Next` on the first page with the description `Before You Begin`
    1. Specify the `Replica Server`, you must add the `FQDN` for the replica site (just to mention, all the Hyper-V nodes and the Replica Broker must have access to Internet and have FQDNs in the public dns servers of your Infrastructure so they can "communicate" also they must have open the port 443 on the local firewall or if you use a dedicated appliance and NAT you must do the Network config there too) for me the FQDN is `replicabroker.domain.local` and hit `Next`
    1. After the Verification of the Replica we must specify the Connection Parameters. The only thing in that page that we must change (of course based on our scenario) is the Certificate, so we must select the Certificate with FQDN that we have been created based on the current server
    1. `Next` on the Replication VHD (except we have 2 vhds and we want to replicate only one of them)
    1. In the Configure Recovery History you can configure whatever you want on your scenario and `Next`
    1. Initial Replication again whatever you want to do on your scenario `Next`
    1. `Finish` if all of the above is setup correct you must see the Enable Replication pop-up window and after a second (based on your network) you must see in the Hyper-V Manager on the Status, the percentage of the Replication.

That's it guys. If you need any advice/or something please contact me.
Thank you a lot.