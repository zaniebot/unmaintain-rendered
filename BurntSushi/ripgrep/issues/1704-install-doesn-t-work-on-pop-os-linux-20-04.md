```yaml
number: 1704
title: "Install doesn't work on Pop OS Linux 20.04"
type: issue
state: closed
author: dodalovic
labels:
  - invalid
assignees: []
created_at: 2020-10-15T10:00:06Z
updated_at: 2020-10-15T11:42:10Z
url: https://github.com/BurntSushi/ripgrep/issues/1704
synced_at: 2026-01-12T16:13:24Z
```

# Install doesn't work on Pop OS Linux 20.04

---

_@dodalovic_

```
➜  ~ sudo apt-get install ripgrep
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  ripgrep
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 0 B/1,228 kB of archives.
After this operation, 4,502 kB of additional disk space will be used.
(Reading database ... 268064 files and directories currently installed.)
Preparing to unpack .../ripgrep_11.0.2-1build1_amd64.deb ...
Unpacking ripgrep (11.0.2-1build1) ...
dpkg: error processing archive /var/cache/apt/archives/ripgrep_11.0.2-1build1_amd64.deb (--unpack):
 trying to overwrite '/usr/.crates2.json', which is also in package bat 0.12.1-1build1
dpkg-deb: error: paste subprocess was killed by signal (Broken pipe)
Errors were encountered while processing:
 /var/cache/apt/archives/ripgrep_11.0.2-1build1_amd64.deb
```

---

_Comment by @BurntSushi on 2020-10-15 11:41_

I have nothing to do with the apt package, so I'm not sure why you're reporting this here.

As far as I know, this is a known bug and a problem with the package itself, not ripgrep. Google the error message.

---

_Closed by @BurntSushi on 2020-10-15 11:41_

---

_Label `invalid` added by @BurntSushi on 2020-10-15 11:42_

---
