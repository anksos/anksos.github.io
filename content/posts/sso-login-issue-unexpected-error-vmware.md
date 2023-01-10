---
title: "Single Sign-On login issue: The authentication server returned an unexpected error"
date: 2016-04-26
draft: false
toc: false
images:
tags:
  - anksos
  - vmware
  - operations
  - vcenter-server
---

I had a question about an issue earlier today from one of my colleagues, that it was quite strange for me, as it was the first time I was pointing it this thing.

The scenario is that a guy from another team wanted to create a new user to the vCenter Server with the permissions of monitoring only (storage guy). The problem came when the guy had as a requirement not to have a domain user but a local user, because the local user was already in place in the server and it was more easy just to take the approve for the permissions instead of creating a new user in the customer's domain.

Okay so far so good, logged in to the Single Sign-on server and navigated through the vSphere Web Client -> Administration -> Configuration -> Identity Services. There I saw three identities, the system-domain (default master SSO domain) and the two customer's domain names integrated with it. So we added another one identity service the Local OS and we tried to login with the user's account.

The login worked without any problem but then I tried to login with the domain name account and I took the following error: `The authentication server returned an unexpected error: ns0:RequestFailed: Internal Error while creating SAML 2.0 Token. The error may be caused by a malfunctioning identity source.`

So after a little search we found that the problem resolves if you remove the Local OS identity. So the solution is to remove the Local OS from identity services and use only domain name if exists. Otherwise you can use the Local OS without problem.

After a little search we found that this thing is problem only for vSphere 5.1. At later releases the problem doesn't exist. The problem as I understand more is about the propagation of the Local User/Groups with the Domain User/Groups and because the Local OS in the Identity services get in charge when you put it instead of the Domain.

Some references about the problem: 

- [https://kb.vmware.com/2043070](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2043070) 
- [https://kb.vmware.com/2050941](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2050941)
- [https://kb.vmware.com/2034798](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2034798)
