```yaml
number: 2087
title: Option for the Windows installer to setup to a specific dir
type: issue
state: closed
author: gozdal
labels:
  - external
assignees: []
created_at: 2024-02-29T16:35:49Z
updated_at: 2024-07-28T00:52:28Z
url: https://github.com/astral-sh/uv/issues/2087
synced_at: 2026-01-10T04:53:49Z
```

# Option for the Windows installer to setup to a specific dir

---

_Issue opened by @gozdal on 2024-02-29 16:35_

When working with `uv 0.1.12` installer in our CI env I had to use `CARGO_HOME` env var to direct the installer to a directory of my choosing with something like:
```powershell
$env:CARGO_HOME = "c:\Python\uv.0.1.12";
irm https://astral.sh/uv/install.ps1 | iex
```

I think it'd be a good addition for the installer to have a destination directory option (say `c:\tools` which might already be in `$PATH`).

---

_Comment by @woutervh on 2024-02-29 19:05_

Why only the windows-installer?

A lot of installer-scripts have a "--destination" or "--target" flag. 
I would certainly make use of it.


---

_Label `upstream` added by @charliermarsh on 2024-02-29 23:47_

---

_Comment by @charliermarsh on 2024-07-28 00:52_

I think the env var is ok but needs to be documented (see: https://github.com/astral-sh/uv/issues/5449).

---

_Closed by @charliermarsh on 2024-07-28 00:52_

---
