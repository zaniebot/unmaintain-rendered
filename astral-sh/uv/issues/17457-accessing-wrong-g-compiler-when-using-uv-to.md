```yaml
number: 17457
title: Accessing wrong g++ compiler when using uv to install the package qutip on linux redhat
type: issue
state: open
author: BenjaminDAnjou
labels:
  - bug
assignees: []
created_at: 2026-01-14T02:48:15Z
updated_at: 2026-01-14T12:15:52Z
url: https://github.com/astral-sh/uv/issues/17457
synced_at: 2026-01-14T12:44:04Z
```

# Accessing wrong g++ compiler when using uv to install the package qutip on linux redhat

---

_@BenjaminDAnjou_

### Summary

I am working on a remote server that runs Linux RedHat and I have a strange error that I'm having a hard time to pin down.

I am trying to install the [qutip](https://qutip.readthedocs.io/en/stable/) package using `uv add`:

```
cd
mkdir project
cd project
uv init --python 3.13.3
uv --verbose add qutip
```
This produces the error:

```
      [stderr]
      error: command '/usr/bin/aarch64-linux-gnu-g++' failed: No such file
      or directory

      hint: This usually indicates a problem with the package or the build
      environment.
  help: If you want to add the package regardless of the failed resolution,
        provide the `--frozen` flag to skip locking and syncing.
DEBUG Released lock at `/net/nfs-iq/home-gh/username/project/.venv/.lock`
DEBUG Released lock at `/home/username/.cache/uv/.lock`
```
It seems like uv is looking for the wrong compiler. It feels like it should be looking for

```
/usr/bin/aarch64-redhat-linux-g++
```

which is the compiler that exists on my distribution.

Weirdly, this problem only occurs for the `qutip` package. The other packages I use don't have this issue. But if I try to install `qutip` with `pyenv` and `pyenv-virtualenv` using `pip`, I have no issue at all. So there seems to be part of this that is `uv` specific.

Let me know what you think, and whether you think this is issue is more appropriately directed to the devs of `qutip`. This is a bit beyond me.

**UPDATE:** I managed to get someone else to try on the same machine, and they did not have the same issue. So there must be something in my environment that causes this issue when interacting with both `qutip` and `uv`. I will report back if I get more information.

**OS INFO:** For completeness, the output of `cat /etc/os-release` is:

```
NAME="Rocky Linux"
VERSION="9.6 (Blue Onyx)"
ID="rocky"
ID_LIKE="rhel centos fedora"
VERSION_ID="9.6"
PLATFORM_ID="platform:el9"
PRETTY_NAME="Rocky Linux 9.6 (Blue Onyx)"
ANSI_COLOR="0;32"
LOGO="fedora-logo-icon"
CPE_NAME="cpe:/o:rocky:rocky:9::baseos"
HOME_URL="https://rockylinux.org/"
VENDOR_NAME="RESF"
VENDOR_URL="https://resf.org/"
BUG_REPORT_URL="https://bugs.rockylinux.org/"
SUPPORT_END="2032-05-31"
ROCKY_SUPPORT_PRODUCT="Rocky-Linux-9"
ROCKY_SUPPORT_PRODUCT_VERSION="9.6"
REDHAT_SUPPORT_PRODUCT="Rocky Linux"
REDHAT_SUPPORT_PRODUCT_VERSION="9.6"
```

### Platform

Linux 5.14.0-427.16.1.el9_4.aarch64+64k aarch64 GNU/Linux

### Version

uv 0.9.25

### Python version

Python 3.13.3

---

_Label `bug` added by @BenjaminDAnjou on 2026-01-14 02:48_

---

_Renamed from "Error using uv to install the package qutip on linux redhat" to "Accessing wrong g++ compiler when using uv to install the package qutip on linux redhat" by @BenjaminDAnjou on 2026-01-14 02:49_

---

_Comment by @konstin on 2026-01-14 09:35_

CC @geofft @jjhelmus 

---
