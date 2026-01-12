```yaml
number: 15345
title: Version Mismatch Between uv --version and Installed Version Using curl Installer
type: issue
state: closed
author: ashuguptahere
labels:
  - question
assignees: []
created_at: 2025-08-18T09:33:56Z
updated_at: 2025-08-18T17:12:12Z
url: https://github.com/astral-sh/uv/issues/15345
synced_at: 2026-01-12T16:02:08Z
```

# Version Mismatch Between uv --version and Installed Version Using curl Installer

---

_@ashuguptahere_

### Summary

After installing uv via the curl command from https://astral.sh/uv/install.sh, the installed version (0.8.11) does not match the version output from the command uv --version (0.4.25). This version mismatch seems to be an issue post-installation, as the version of uv reported by the installer and the actual version in use differ.

Steps to Reproduce:
- Run the installation script to install uv: ```curl -LsSf https://astral.sh/uv/install.sh | sh```
- Observe that the following output shows uv 0.8.11 is being installed:

```
downloading uv 0.8.11 x86_64-unknown-linux-gnu
no checksums to verify
installing to /home/qulith-jr/.local/bin
  uv
  uvx
everything's installed!
WARN: The following commands are shadowed by other commands in your PATH: uv uvx
```
- Run uv --version: ```uv --version```
- Run the command: ```uv self version```
- It results in this error: ```error: unrecognized subcommand 'version'```
- EDIT: ```uv self --version``` is working instead of ```uv self version``` which displays ```uv-self 0.4.25```


### Platform

Ubuntu 24.04

### Version

0.8.11 or 0.4.25 (probably the former)

### Python version

_No response_

---

_Label `bug` added by @ashuguptahere on 2025-08-18 09:33_

---

_Comment by @topher96 on 2025-08-18 12:49_

WARN: The following commands are shadowed by other commands in your PATH: uv uvx

The warning above is hinting you need to update your PATH yourself.   In your .bashrc or similar.    Run which uv to verify.  



---

_Comment by @zanieb on 2025-08-18 13:41_

> WARN: The following commands are shadowed by other commands in your PATH: uv uvx

This warning is saying there's another version of uv on your PATH in front of the one that was just installed. Use `where uv` to see them all and remove that old one.

---

_Label `bug` removed by @zanieb on 2025-08-18 13:41_

---

_Label `question` added by @zanieb on 2025-08-18 13:41_

---

_Comment by @ashuguptahere on 2025-08-18 17:10_

I removed the `uv` and `uvx` from below locations, reinstalled it again and now it's working as expected
```
rm -rf .local/bin/uv
rm -rf .local/bin/uvx
rm -rf .cargo/bin/uv
rm -rf .local/bin/uvx
```

Thank you so much for the quick resolution. Closing the issue now!

---

_Closed by @ashuguptahere on 2025-08-18 17:10_

---
