```yaml
number: 10945
title: "Platform-dependent dependency bug (`sling-mac-arm64`)"
type: issue
state: closed
author: smackesey
labels:
  - bug
assignees: []
created_at: 2025-01-24T20:20:37Z
updated_at: 2025-02-12T18:04:21Z
url: https://github.com/astral-sh/uv/issues/10945
synced_at: 2026-01-12T16:00:24Z
```

# Platform-dependent dependency bug (`sling-mac-arm64`)

---

_@smackesey_

### Summary

On macOS w/ Apple Silicon, installing `sling` should also install `sling-mac-arm64`. However, I find that an install of `sling` in a fresh virtual environment using `--no-cache` will instead install `sling-linux-amd64`. If I do not disable the cache, then sometimes I correctly get `sling-mac-arm64`

### Platform

macOS 15 arm64

### Version

uv 0.5.22 (Homebrew 2025-01-21)

### Python version

3.11.9

---

_Label `bug` added by @smackesey on 2025-01-24 20:20_

---

_Comment by @charliermarsh on 2025-01-24 20:23_

It looks like the package ships with incorrect metadata inside it. I'll see if we can avoid respecting it.

---

_Comment by @smackesey on 2025-01-24 20:24_

Interesting-- FWIW I do not encounter the same behavior when I use `pip`.

---

_Comment by @charliermarsh on 2025-01-24 20:29_

Right, pip doesn't read this metadata.

---

_Comment by @charliermarsh on 2025-01-24 20:40_

If helpful, you should be able to add this to your configuration file in the interim:

```toml
[[tool.uv.dependency-metadata]]
name = "sling"
version = "1.4.0"
requires-dist = [
    "sling-linux-arm64==1.4.0 ; sys_platform == 'linux' and platform_machine == 'aarch64'",
    "sling-linux-amd64==1.4.0 ; sys_platform == 'linux' and platform_machine != 'aarch64'",
    "sling-windows-amd64==1.4.0 ; sys_platform == 'win32' and platform_machine == 'x86_64'",
    "sling-mac-arm64==1.4.0 ; sys_platform == 'darwin' and platform_machine == 'arm64'",
    "sling-mac-amd64==1.4.0 ; sys_platform == 'darwin' and platform_machine != 'arm64'",
]
```


---

_Comment by @charliermarsh on 2025-01-24 20:42_

(Edit: I tweaked it a little bit)

---

_Comment by @charliermarsh on 2025-01-24 21:52_

I think the right solution here is for us to stop reading from `requires.txt` in v0.6. It's not reliable and is arguably a hack that we do so today.

---

_Added to milestone `v0.6.0` by @charliermarsh on 2025-01-24 21:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-24 21:53_

---

_Comment by @charliermarsh on 2025-02-12 18:04_

Fixed in v0.6.

---

_Closed by @charliermarsh on 2025-02-12 18:04_

---
