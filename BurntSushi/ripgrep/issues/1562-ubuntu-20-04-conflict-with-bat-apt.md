```yaml
number: 1562
title: Ubuntu 20.04 conflict with bat (apt)
type: issue
state: closed
author: Offpics
labels:
  - invalid
assignees: []
created_at: 2020-04-27T10:19:07Z
updated_at: 2020-07-12T15:06:42Z
url: https://github.com/BurntSushi/ripgrep/issues/1562
synced_at: 2026-01-12T16:13:23Z
```

# Ubuntu 20.04 conflict with bat (apt)

---

_@Offpics_

#### How did you install ripgrep?

Ubuntu apt

#### What operating system are you using ripgrep on?

Ubuntu 20.04

#### Describe your bug.

Ripgrep has a conflict with bat (https://github.com/sharkdp/bat) while installing from apt reposotiry.

#### What are the steps to reproduce the behavior?

```
sudo apt install ripgrep
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  ripgrep
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 0 B/1 228 kB of archives.
After this operation, 4 502 kB of additional disk space will be used.
(Reading database ... 156191 files and directories currently installed.)
Preparing to unpack .../ripgrep_11.0.2-1build1_amd64.deb ...
Unpacking ripgrep (11.0.2-1build1) ...
dpkg: error processing archive /var/cache/apt/archives/ripgrep_11.0.2-1build1_amd64.deb (--unpack):
 trying to overwrite '/usr/.crates2.json', which is also in package bat 0.12.1-1build1
dpkg-deb: error: paste subprocess was killed by signal (Broken pipe)
Errors were encountered while processing:
 /var/cache/apt/archives/ripgrep_11.0.2-1build1_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

---

_Comment by @BurntSushi on 2020-04-27 11:34_

Thanks for reporting this, but you need to report it with the package maintainers downstream. I do not maintain the Ubuntu packages.

---

_Closed by @BurntSushi on 2020-04-27 11:34_

---

_Label `invalid` added by @BurntSushi on 2020-04-27 11:35_

---

_Comment by @berkes on 2020-05-26 10:20_

For reference and people, like me, arriving here. The packager is dealing with this bug here: https://bugs.launchpad.net/ubuntu/+source/rust-bat/+bug/1868517

---

_Comment by @yevgenykuz on 2020-07-12 15:06_

If someone still sees this after upgrading to 20.04, you can try the workaround suggested here:
https://bugs.launchpad.net/ubuntu/+source/rust-bat/+bug/1868517/comments/32

---
