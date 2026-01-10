```yaml
number: 5513
title: Support xz compressed packages
type: pull_request
state: merged
author: krishnan-chandra
labels:
  - compatibility
assignees: []
merged: true
base: main
head: support-xz-compressed-packages
created_at: 2024-07-28T12:58:10Z
updated_at: 2024-07-28T21:41:20Z
url: https://github.com/astral-sh/uv/pull/5513
synced_at: 2026-01-10T13:37:23Z
```

# Support xz compressed packages

---

_Pull request opened by @krishnan-chandra on 2024-07-28 12:58_

## Summary

Closes #2187.

The [xz backdoor](https://gist.github.com/thesamesam/223949d5a074ebc3dce9ee78baad9e27) is still fairly recent, but luckily the [Rust `xz2` crate bundles version 5.2.5 of the C `xz` package](https://github.com/alexcrichton/xz2-rs/tree/main/lzma-sys), which is before the backdoor was introduced.

It's worth noting that a security risk still exists if you have a compromised version of `xz` installed on your system, but that risk is not introduced by `uv` or the Rust packages in general.

## Test Plan

Tried installing the package mentioned in the linked issue: `python-apt @ https://launchpad.net/ubuntu/+archive/primary/+sourcefiles/python-apt/2.7.6/python-apt_2.7.6.tar.xz`

(Note that this will only work on Ubuntu - I tried on a Mac and while the archive was extracted properly, the package did not install because of some missing files)


---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-28 17:09_

---

_@charliermarsh reviewed on 2024-07-28 18:26_

---

_Review comment by @charliermarsh on `Cargo.toml`:229 on 2024-07-28 18:26_

I think this actually _is_ the default, but we changed it for uv below.

---

_@charliermarsh approved on 2024-07-28 18:27_

Thanks!

---

_Label `compatibility` added by @charliermarsh on 2024-07-28 18:27_

---

_Comment by @charliermarsh on 2024-07-28 18:28_

Nice PR.

---

_Merged by @charliermarsh on 2024-07-28 18:37_

---

_Closed by @charliermarsh on 2024-07-28 18:37_

---

_@krishnan-chandra reviewed on 2024-07-28 21:39_

---

_Review comment by @krishnan-chandra on `Cargo.toml`:229 on 2024-07-28 21:39_

Gotcha, this was just really confusing because the comment doesn't match the implementation. Maybe best to clarify.

---

_Branch deleted on 2024-07-28 21:41_

---
