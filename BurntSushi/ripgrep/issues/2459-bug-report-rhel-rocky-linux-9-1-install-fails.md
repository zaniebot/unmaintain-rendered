```yaml
number: 2459
title: "[BUG REPORT] RHEL/Rocky Linux 9.1 install fails / SHA1 deprecated"
type: issue
state: closed
author: tracure1337
labels:
  - invalid
assignees: []
created_at: 2023-03-15T21:23:12Z
updated_at: 2023-03-15T21:47:47Z
url: https://github.com/BurntSushi/ripgrep/issues/2459
synced_at: 2026-01-12T16:13:24Z
```

# [BUG REPORT] RHEL/Rocky Linux 9.1 install fails / SHA1 deprecated

---

_@tracure1337_

#### What version of ripgrep are you using?

latest

#### How did you install ripgrep?

```
yum-config-manager --add-repo=https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/repo/epel-7/carlwgeorge-ripgrep-epel-7.repo
dnf install ripgrep
```

#### What operating system are you using ripgrep on?

Rocky Linux 9 which is 100% compatible with RHEL/CentOS

#### Describe your bug.

warning: Signature not supported. Hash algorithm SHA1 not available.


#### What are the steps to reproduce the behavior?

try it out as I did



additonal information '>

RHEL 9 deprecating and no longer enabling SHA1 out of the box

Please upgrade your key :)


---

_Comment by @BurntSushi on 2023-03-15 21:34_

I don't know why you're reporting problems with your distro's package manager install here. You need to go ask them for help. Not me. I don't have anything to do with how RHEL/Rocky packages their stuff.

---

_Closed by @BurntSushi on 2023-03-15 21:34_

---

_Label `invalid` added by @BurntSushi on 2023-03-15 21:34_

---

_Comment by @tracure1337 on 2023-03-15 21:47_

Apology. I thought you maintain the package and it's your key.!

---
