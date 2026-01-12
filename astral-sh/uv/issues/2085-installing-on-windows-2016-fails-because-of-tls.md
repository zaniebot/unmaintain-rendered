```yaml
number: 2085
title: Installing on Windows 2016 fails because of TLS versions
type: issue
state: open
author: gozdal
labels:
  - windows
  - external
assignees: []
created_at: 2024-02-29T16:32:33Z
updated_at: 2024-02-29T23:48:05Z
url: https://github.com/astral-sh/uv/issues/2085
synced_at: 2026-01-12T15:58:35Z
```

# Installing on Windows 2016 fails because of TLS versions

---

_@gozdal_

When trying to install `uv` in our CI env I noticed that Windows 2016 boxes failed to download
the install script because of `Could not create SSL/TLS secure channel`. Later Windows versions were fine.

I worked around it with:
```powershell
[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12;
irm https://astral.sh/uv/install.ps1 | iex
```

Perhaphs it'd be good to add it to the docs?

---

_Comment by @Gankra on 2024-02-29 16:44_

We explicitly pass `--tlsv1.2` to curl for the shell installer already, so that seems supremely reasonable for the powershell installer to set too.

---

_Comment by @Gankra on 2024-02-29 16:52_

Could you confirm for me that this expression works on your windows server?

```
irm -SslProtocol Tls12 https://astral.sh/uv/install.ps1 | iex
```

(It's probable once the script downloads it will then error trying to fetch the actual binaries, but that's easier to fix than the raw `irm | iex`)

---

_Comment by @gozdal on 2024-02-29 16:55_

Unfortunately not:
![obraz](https://github.com/astral-sh/uv/assets/619444/9e6b2eb4-226b-40a6-9d34-71d6c433a3af)


---

_Comment by @Gankra on 2024-02-29 16:58_

Ah shoot, yeah that flag is relatively bleeding edge (and maybe Powershell Core exclusive?).

---

_Label `windows` added by @charliermarsh on 2024-02-29 23:47_

---

_Label `upstream` added by @charliermarsh on 2024-02-29 23:48_

---
