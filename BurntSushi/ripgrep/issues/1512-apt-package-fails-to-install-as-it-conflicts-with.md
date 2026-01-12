```yaml
number: 1512
title: apt package fails to install as it conflicts with a file present in fish-common
type: issue
state: closed
author: mdesantis
labels:
  - duplicate
assignees: []
created_at: 2020-03-10T12:47:25Z
updated_at: 2020-03-10T12:59:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1512
synced_at: 2026-01-12T16:13:23Z
```

# apt package fails to install as it conflicts with a file present in fish-common

---

_@mdesantis_

I'm on Ubuntu 18.04, I use fish shell; the ripgrep apt package fails to install as it includes a file that is already added by the fish shell package:

```
> env LANG=C sudo dpkg -i ~/Scaricati/ripgrep_11.0.2_amd64.deb
(Reading database ... 233822 files and directories currently installed.)
Preparing to unpack .../ripgrep_11.0.2_amd64.deb ...
Unpacking ripgrep (11.0.2) ...
dpkg: error processing archive /home/maurizio/Scaricati/ripgrep_11.0.2_amd64.deb (--install):
 trying to overwrite '/usr/share/fish/completions/rg.fish', which is also in package fish-common 3.1.0-1~bionic
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
Errors were encountered while processing:
 /home/maurizio/Scaricati/ripgrep_11.0.2_amd64.deb
```

Here some details of my environment:

```
> lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.4 LTS
Release:	18.04
Codename:	bionic

> env LANG=C apt show fish-common
Package: fish-common
Version: 3.1.0-1~bionic
Priority: extra
Section: shells
Source: fish
Maintainer: ridiculous_fish <corydoras@ridiculousfish.com>
Installed-Size: 9734 kB
Depends: fish (= 3.1.0-1~bionic)
Recommends: python3 (>= 3.3) | python (>= 2.7)
Suggests: xdg-utils
Replaces: fish (<= 2.1.1.dfsg-2)
Download-Size: 1393 kB
APT-Manual-Installed: no
APT-Sources: http://ppa.launchpad.net/fish-shell/release-3/ubuntu bionic/main amd64 Packages
Description: friendly interactive shell (architecture-independent files)
 Fish is a command-line shell for modern systems, focusing on user-friendliness,
 sensibility and discoverability in interactive use. The syntax is simple, but
 not POSIX compliant.
 .
 This package contains the common fish files shared by all architectures.

N: There is 1 additional record. Please use the '-a' switch to see it

> env LANG=C sudo dpkg --info ~/Scaricati/ripgrep_11.0.2_amd64.deb
 new Debian package, version 2.0.
 size 1458988 bytes: control archive=904 bytes.
     756 bytes,    19 lines      control              
     771 bytes,    12 lines      md5sums              
 Package: ripgrep
 Version: 11.0.2
 Architecture: amd64
 Vcs-Browser: https://github.com/BurntSushi/ripgrep
 Vcs-Git: https://github.com/BurntSushi/ripgrep
 Homepage: https://github.com/BurntSushi/ripgrep
 Section: utils
 Priority: optional
 Standards-Version: 3.9.4
 Maintainer: Andrew Gallant <jamslam@gmail.com>
 Installed-Size: 5363
 Depends: 
 Description: ripgrep is a line-oriented search tool that recursively searches your current
  directory for a regex pattern while respecting your gitignore rules. ripgrep
  has first class support on Windows, macOS and Linux.
  ripgrep (rg) recursively searches your current directory for a regex pattern.
  By default, ripgrep will respect your .gitignore and automatically skip hidden
  files/directories and binary files.
```

---

_Renamed from "apt package fails to install on Ubuntu as it conflicts with a file present in fish-common" to "apt package fails to install as it conflicts with a file present in fish-common" by @mdesantis on 2020-03-10 12:48_

---

_Label `duplicate` added by @BurntSushi on 2020-03-10 12:51_

---

_Comment by @BurntSushi on 2020-03-10 12:51_

Duplicate of #1485. 

---

_Closed by @BurntSushi on 2020-03-10 12:51_

---

_Comment by @mdesantis on 2020-03-10 12:59_

Yeah sorry, I couldn't find that one when I searched for closed issues :relieved: 

---
