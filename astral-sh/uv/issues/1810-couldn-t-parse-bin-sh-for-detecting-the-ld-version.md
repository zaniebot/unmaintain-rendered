```yaml
number: 1810
title: "Couldn't parse /bin/sh for detecting the ld version"
type: issue
state: closed
author: Ch3ri0ur
labels:
  - bug
assignees: []
created_at: 2024-02-21T14:26:11Z
updated_at: 2024-02-21T16:49:50Z
url: https://github.com/astral-sh/uv/issues/1810
synced_at: 2026-01-10T05:40:32Z
```

# Couldn't parse /bin/sh for detecting the ld version

---

_Issue opened by @Ch3ri0ur on 2024-02-21 14:26_

When I try to use `uv venv` I get: 
```
uv venv
  × Failed to detect the operating system version: Couldn't parse /bin/sh for detecting the ld version: Invalid magic
  │ number: 0x642f6e69622f2123
```
I am on a wsl x86 ubuntu 22.04 variant from my company.
uv --version => uv 0.1.6


I took a look at our `/bin/sh`:
It looks like my company replaced the usual symlink to dash with:

```sh
#!/bin/dash

# some proxy exports

dash "$@"
```

I just wanted to mention that. Apparently one can not rely on anything. :-)

Thanks for your effort and keep up the good work!


---

_Assigned to @BurntSushi by @BurntSushi on 2024-02-21 15:44_

---

_Closed by @BurntSushi on 2024-02-21 16:49_

---

_Label `bug` added by @BurntSushi on 2024-02-21 16:49_

---
