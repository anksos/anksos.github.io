---
title: "Keep your SUSE rolling!"
date: 2011-04-05T12:14:39+01:00
draft: true
toc: false
images:
tags:
  - cli
  - anksos
  - operations
  - suse
---

One of the new features the recent major release of openSUSE brought with it that *really* caught my attention is the ability to turn the distribution into a rolling one, effectively bringing it to the camp of the likes of Gentoo and Arch. As a former long time user of Gentoo I thought I’d install openSUSE and take it for a drive – but most importantly add the Tumbleweed repo to it and see what happens from there. 

[Tumbleweed](https://www.suse.com/suse-defines/definition/opensuse-tumbleweed/) is the name of the repository that once added to your openSUSE installation allows the whole system to be regularly upgraded to the latest and greatest software, without the need of ever upgrading the OS to a newer, major version of the distribution. The good news is that "latest and greatest" doesn’t mean "bleeding edge". That may be the case with the openSUSE Factory repo, but not with Tumbleweed. The bad news is that by turning openSUSE into a rolling distro you’ll find yourself re-compiling and re-installing any closed-source drivers you rely on more often than you’re probably used to – and in most cases that entails some extra labor.
But that couldn’t possibly stop me from trying Tumbleweed, so I set off to a quest for transforming a local openSUSE 11.4 VMware VM (GNOME edition) into an installation of a rolling distribution. Well, it very soon turned out that I was in for a pretty short yet quite enjoyable quest. Before I go any further, I should point out that I’m no openSUSE expert. I have used the distribution in the past but it never really won me. Despite that, I always enjoy trying out new operating systems in general and Linux distributions in particular – especially when there’s a new version of a popular brand out. I should probably mention here that this time around openSUSE left me with an excellent impression and that’s mainly because of zypper, the distribution’s command-line tool for package management.

First thing I did before attempting the addition of Tumbleweed was to update my installation. I was never a big fan of YaST so without a second thought I resorted to zypper:

``` shell
sub0@opensuse:~> sudo zypper refresh
root's password:
Repository 'openSUSE-11.4-Non-Oss' is up to date.
Repository 'openSUSE-11.4-Oss' is up to date.
Repository 'openSUSE-11.4-Update' is up to date.
All repositories have been refreshed.
 
sub0@opensuse:~> zypper list-updates
Loading repository data...
Reading installed packages...
No updates found.
```

Since I like to update regularly, I should have expected that :) It was time for the addition of Tumbleweed. But first I thought I’d take a look at the list of existing repositories:

``` shell
sub0@opensuse:~> zypper repos
# | Alias             | Name                       | Enabled | Refresh
--+-------------------+----------------------------+---------+--------
1 | repo-debug        | openSUSE-11.4-Debug        | No      | Yes
2 | repo-debug-update | openSUSE-11.4-Update-Debug | No      | Yes
3 | repo-non-oss      | openSUSE-11.4-Non-Oss      | Yes     | Yes
4 | repo-oss          | openSUSE-11.4-Oss          | Yes     | Yes
5 | repo-source       | openSUSE-11.4-Source       | No      | Yes
6 | repo-update       | openSUSE-11.4-Update       | Yes     | Yes
```

As per the instructions on the [official Tumbleweed portal](https://en.opensuse.org/Portal:Tumbleweed), I proceeded as follows:

``` shell
sub0@opensuse:~> sudo zypper addrepo --refresh http://download.opensuse.org/repositories/openSUSE:/Tumbleweed/standard/ Tumbleweed
root's password:
Adding repository 'Tumbleweed' [done]
Repository 'Tumbleweed' successfully added
Enabled: Yes
Autorefresh: Yes
URI: http://download.opensuse.org/repositories/openSUSE:/Tumbleweed/standard/
```

The _addrepo_ command adds a new repository and assigns an alias to it. The repository can be specified by a URI (Uniform Resource Indentifier) and in our case that’s: `http://download.opensuse.org/repositories/openSUSE:/Tumbleweed/standard/`

As for the refresh switch, it enables the auto-refresh feature of the newly added repository. By that moment, my repo list was looking like this:

``` shell
sub0@opensuse:~> zypper repos
# | Alias             | Name                       | Enabled | Refresh
--+-------------------+----------------------------+---------+--------
1 | Tumbleweed        | Tumbleweed                 | Yes     | Yes
2 | repo-debug        | openSUSE-11.4-Debug        | No      | Yes
3 | repo-debug-update | openSUSE-11.4-Update-Debug | No      | Yes
4 | repo-non-oss      | openSUSE-11.4-Non-Oss      | Yes     | Yes
5 | repo-oss          | openSUSE-11.4-Oss          | Yes     | Yes
6 | repo-source       | openSUSE-11.4-Source       | No      | Yes
7 | repo-update       | openSUSE-11.4-Update       | Yes     | Yes
```

It was time for a full system upgrade – using the Tumbleweed repository, of course:

``` shell
sub0@opensuse:~> sudo zypper dist-upgrade --from Tumbleweed
root's password:
Retrieving repository 'Tumbleweed' metadata [\]

New repository or package signing key received:
Key ID: 01A7DE5AA840F92C
Key Name: openSUSE:Tumbleweed OBS Project
Key Fingerprint: 76050872919849D2A36BEE8901A7DE5AA840F92C
Key Created: Tue 07 Dec 2010 06:31:27 PM EET
Key Expires: Thu 14 Feb 2013 06:31:27 PM EET
Repository: Tumbleweed

Do you want to reject the key, trust temporarily, or trust always? [r/t/a/?] (r): a
Retrieving repository 'Tumbleweed' metadata [done]
Building repository 'Tumbleweed' cache [done]
Loading repository data...
Reading installed packages...
Computing distribution upgrade...
```

I had no reason to not trust Tumbleweed’s signing key so I typed "a". And then I was greeted by…

``` shell
3 Problems:
Problem: ndiswrapper-kmp-default-1.56_k2.6.37.1_1.2-11.3.i586 requires ksym(default:current_task) = eb56bb3, but this requirement cannot be provided
Problem: ndiswrapper-kmp-desktop-1.56_k2.6.37.1_1.2-11.3.i586 requires ksym(desktop:current_task) = de1c42d1, but this requirement cannot be provided
Problem: ndiswrapper-kmp-pae-1.56_k2.6.37.1_1.2-11.3.i586 requires ksym(pae:__mutex_init) = 976e829f, but this requirement cannot be provided

Problem: ndiswrapper-kmp-default-1.56_k2.6.37.1_1.2-11.3.i586 requires ksym(default:current_task) = eb56bb3, but this requirement cannot be provided
  uninstallable providers: kernel-default-base-2.6.37.1-1.2.2.i586[repo-oss]
 Solution 1: deinstallation of ndiswrapper-kmp-default-1.56_k2.6.37.1_1.2-11.3.i586
 Solution 2: keep obsolete kernel-default-2.6.37.1-1.2.2.i586
 Solution 3: install kernel-default-base-2.6.37.1-1.2.2.i586 from excluded repository
 Solution 4: break ndiswrapper-kmp-default by ignoring some of its dependencies
```

Apparently, I had hit upon the wall of unmet dependencies and conflicts. Fortunately I have no need for ndiswrapper in my VM, so the solution was to de-install: `Choose from above solutions by number or skip, retry or cancel [1/2/3/4/s/r/c] (c): 1`

There were some more ndiswrapper-related problems. Like the first one, I took care of them by choosing to de-install:

``` shell
Problem: ndiswrapper-kmp-desktop-1.56_k2.6.37.1_1.2-11.3.i586 requires ksym(desktop:current_task) = de1c42d1, but this requirement cannot be provided
  uninstallable providers: kernel-desktop-base-2.6.37.1-1.2.2.i586[repo-oss]
 Solution 1: deinstallation of ndiswrapper-kmp-desktop-1.56_k2.6.37.1_1.2-11.3.i586
 Solution 2: keep obsolete kernel-desktop-2.6.37.1-1.2.2.i586
 Solution 3: install kernel-desktop-base-2.6.37.1-1.2.2.i586 from excluded repository
 Solution 4: break ndiswrapper-kmp-desktop by ignoring some of its dependencies


Choose from above solutions by number or skip, retry or cancel [1/2/3/4/s/r/c] (c): 1


Problem: ndiswrapper-kmp-pae-1.56_k2.6.37.1_1.2-11.3.i586 requires ksym(pae:__mutex_init) = 976e829f, but this requirement cannot be provided
  uninstallable providers: kernel-pae-base-2.6.37.1-1.2.2.i586[repo-oss]
 Solution 1: deinstallation of ndiswrapper-kmp-pae-1.56_k2.6.37.1_1.2-11.3.i586
 Solution 2: keep obsolete kernel-pae-2.6.37.1-1.2.2.i586
 Solution 3: install kernel-pae-base-2.6.37.1-1.2.2.i586 from excluded repository
 Solution 4: break ndiswrapper-kmp-pae by ignoring some of its dependencies


Choose from above solutions by number or skip, retry or cancel [1/2/3/4/s/r/c] (c): 1
Resolving dependencies...
Computing distribution upgrade...


Problem: ndiswrapper-1.56-11.3.i586 requires ndiswrapper-kmp, but this requirement cannot be provided
 Solution 1: deinstallation of ndiswrapper-1.56-11.3.i586
 Solution 2: keep ndiswrapper-kmp-pae-1.56_k2.6.37.1_1.2-11.3.i586
 Solution 3: keep ndiswrapper-kmp-desktop-1.56_k2.6.37.1_1.2-11.3.i586
 Solution 4: keep ndiswrapper-kmp-default-1.56_k2.6.37.1_1.2-11.3.i586
 Solution 5: break ndiswrapper by ignoring some of its dependencies


Choose from above solutions by number or cancel [1/2/3/4/5/c] (c): 1
```

A few seconds later the dependencies were all taken care of and zypper listed the new packages that were about to be installed, the ones that were going to be removed and all those who needed an upgrade:

``` shell
Resolving dependencies...
Computing distribution upgrade...

The following NEW packages are going to be installed:
  libreoffice-icon-theme-galaxy libreoffice-icon-theme-hicontrast

The following packages are going to be REMOVED:
  ndiswrapper ndiswrapper-kmp-default ndiswrapper-kmp-desktop ndiswrapper-kmp-pae

The following packages are going to be upgraded:
  btrfsprogs cifs-utils ethtool insserv iproute2 kernel-default kernel-default-devel kernel-desktop kernel-desktop-devel kernel-devel kernel-firmware kernel-pae kernel-pae-devel kernel-source libreoffice
  libreoffice-base libreoffice-calc libreoffice-components libreoffice-draw libreoffice-filters libreoffice-filters-optional libreoffice-gnome libreoffice-help-de libreoffice-help-en-US libreoffice-help-ru
  libreoffice-hyphen libreoffice-icon-theme-tango libreoffice-impress libreoffice-l10n-de libreoffice-l10n-extras libreoffice-l10n-ru libreoffice-libs-core libreoffice-libs-extern libreoffice-libs-gui
  libreoffice-mailmerge libreoffice-math libreoffice-pyuno libreoffice-templates-en libreoffice-templates-labels-a4 libreoffice-templates-labels-letter libreoffice-templates-presentation-layouts
  libreoffice-thesaurus-de libreoffice-thesaurus-en-US libreoffice-ure libreoffice-writer libsmbclient0 libwbclient0 preload preload-kmp-default preload-kmp-desktop samba samba-client systemtap-runtime usbutils
```

There were also some packages that needed a downgrade…

``` shell
The following packages are going to be downgraded:
  libldb0 libreoffice-branding-openSUSE libtalloc2 libtdb1 libtevent0
```

…and many more that were about to change vendor:

``` shell
The following packages are going to change vendor:

  btrfsprogs              openSUSE -> obs://build.opensuse.org/openSUSE:Tumbleweed
  cifs-utils              openSUSE -> obs://build.opensuse.org/openSUSE:Tumbleweed
  ethtool                 openSUSE -> obs://build.opensuse.org/openSUSE:Tumbleweed
[snip]
  vim                     openSUSE -> obs://build.opensuse.org/openSUSE:Tumbleweed
  vim-base                openSUSE -> obs://build.opensuse.org/openSUSE:Tumbleweed
  vim-data                openSUSE -> obs://build.opensuse.org/openSUSE:Tumbleweed
```

By that moment Tumbleweed’s power was more than apparent. The upgrade process went on smoothly without any interruption:

``` shell
57 packages to upgrade, 5 to downgrade, 2 new, 4 to remove, 62  to change vendor.
Overall download size: 356.1 MiB. After the operation, additional 19.0 MiB will be used.
Continue? [y/n/?] (y): y
Retrieving package vim-base-7.3-8.2.i586 (1/64), 159.0 KiB (325.0 KiB unpacked)
Retrieving: vim-base-7.3-8.2.i586.rpm [done (267.8 KiB/s)]
[snip]

Installing: libreoffice-base-3.3.2.2-1.2 [done]
Installing: libreoffice-filters-optional-3.3.2.2-1.2 [done]
Additional rpm output:
Unregistering the older LibreOffice optional filter extensions...
```

After the upgrade completed I was informed of some running programs that use files deleted in the process:
`There are some running programs that use files deleted by recent upgrade. You may wish to restart some of them. Run 'zypper ps' to list these programs.`

I gave the proposed command and got nothing back.
``` shell
sub0@opensuse:~> zypper ps
No processes using deleted files found.
```

I tried again, this time with administrative privileges:

``` shell
sub0@opensuse:~> sudo zypper ps
root's password:
The following running processes use deleted files:

PID  | PPID | UID | Login | Command | Service | Files
-----+------+-----+-------+---------+---------+------------------------
1217 | 1212 | 0   | root  | Xorg    |         | /usr/lib/libtalloc.so.2
```

I didn’t bother with restarting the X server as I was about to reboot the system. During the upgrade process I noticed a newer version of the running kernel coming in, so a reboot was necessary to activate it. When the system booted up, I opened a terminal window and checked for the kernel version:

``` shell
sub0@opensuse:~> uname -a
Linux opensuse.vmlan.net 2.6.38-18-desktop #1 SMP PREEMPT 2011-03-20 22:25:37 +0100 i686 i686 i386 GNU/Linux
```

Very well! The previous active version had been 2.6.37.1-1.2-desktop and the then current was 2.6.38-18-desktop. Now, in most cases a kernel upgrade begs for the upgrade of any closed-source drivers they hook on it. For my particular case I had to re-install VMware tools. Hopefully the process went on flawlessly.
It’s been a few days since I transformed my openSUSE installation into a rolling release distribution and everything seems to be working fine. I will continue using the distribution as often as I can and report back anything worth, well, reporting back. Until that time, take care and have fun!

Thank's to my friends in Parabing for this BEAUTIFUL post and for letting me reposting it here. You can find the original post [here](https://parabing.com/)