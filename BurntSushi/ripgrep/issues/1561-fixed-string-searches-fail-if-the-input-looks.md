```yaml
number: 1561
title: Fixed string searches fail if the input looks like a cli option
type: issue
state: closed
author: Ophirr33
labels:
  - duplicate
assignees: []
created_at: 2020-04-23T21:22:53Z
updated_at: 2020-04-23T21:37:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1561
synced_at: 2026-01-12T16:13:23Z
```

# Fixed string searches fail if the input looks like a cli option

---

_@Ophirr33_

#### What version of ripgrep are you using?

```
ripgrep 12.0.1 (rev 1d5b1011e5)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

https://github.com/BurntSushi/ripgrep/releases/download/12.0.1/ripgrep_12.0.1_amd64.deb

#### What operating system are you using ripgrep on?

```
$ cat /etc/os-release
NAME="Pop!_OS"
VERSION="19.10"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Pop!_OS 19.10"
VERSION_ID="19.10"
HOME_URL="https://system76.com/pop"
SUPPORT_URL="http://support.system76.com"
BUG_REPORT_URL="https://github.com/pop-os/pop/issues"
PRIVACY_POLICY_URL="https://system76.com/privacy"
VERSION_CODENAME=eoan
UBUNTU_CODENAME=eoan
LOGO=distributor-logo-pop-os
```

#### Describe your bug.

Can't do a fixed string for things that look like cli args.

#### What are the steps to reproduce the behavior?

```
rg -F '-a'
rg -F '-e'
rg -F '->'
rg -F '-['
rg -F '--regexp'
```

#### What is the actual behavior?

`ripgrep` seems to interpret the fixed string to search as another cli arg. This does not occur when using `-e` instead of `-F`. 

e.g.:
```
$ rg -F '-a' # fails with the following error
error: The following required arguments were not provided:
    <PATTERN>

USAGE:
    
    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help

$ rg -e '-a' # searches for -a
```

It's not that big of a deal, since you can do a pattern search and escape characters as needed, but it can be annoying in scripts.

#### What is the expected behavior?

A search is performed searching for strings that are equal to the given fixed string, even when it looks like a cli arg.

---

_Comment by @BurntSushi on 2020-04-23 21:37_

Duplicate of #1560. Which was incidentally just reported today!

---

_Closed by @BurntSushi on 2020-04-23 21:37_

---

_Label `duplicate` added by @BurntSushi on 2020-04-23 21:37_

---
