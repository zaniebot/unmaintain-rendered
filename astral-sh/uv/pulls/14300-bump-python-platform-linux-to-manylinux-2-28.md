```yaml
number: 14300
title: "Bump `--python-platform linux` to `manylinux_2_28`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - breaking
assignees: []
merged: true
base: release/080
head: charlie/many
created_at: 2025-06-26T23:48:02Z
updated_at: 2025-07-18T11:12:24Z
url: https://github.com/astral-sh/uv/pull/14300
synced_at: 2026-01-10T06:53:01Z
```

# Bump `--python-platform linux` to `manylinux_2_28`

---

_Pull request opened by @charliermarsh on 2025-06-26 23:48_

## Summary

Right now, `--python-platform linux` to defaults to `manylinux_2_17`. Defaulting to `manylinux_2_17` causes some problems for users, since it means we can't use (e.g.) `manylinux_2_28` wheels, and end up having to build from source.

cibuildwheel made `manylinux_2_28` their default in https://github.com/pypa/cibuildwheel/pull/1988, and there's a lot of discussion in https://github.com/pypa/cibuildwheel/issues/1772 and https://github.com/pypa/cibuildwheel/issues/2047. In short, the `manylinux_2014` image is EOL, and the vast majority of consumers now run at least glibc 2.28 (https://mayeut.github.io/manylinux-timeline/):

![Screenshot 2025-06-26 at 7 47 23 PM](https://github.com/user-attachments/assets/2672d91b-f9eb-4442-b680-7e4cd7cade91)

Note that this only changes the _default_. Users can still compile against `manylinux_2_17` by specifying it.


---

_Label `enhancement` added by @charliermarsh on 2025-06-26 23:48_

---

_Label `breaking` added by @charliermarsh on 2025-06-26 23:48_

---

_Review requested from @Gankra by @charliermarsh on 2025-06-26 23:48_

---

_Review requested from @konstin by @charliermarsh on 2025-06-26 23:48_

---

_Added to milestone `v0.8.0` by @charliermarsh on 2025-06-26 23:48_

---

_Comment by @zanieb on 2025-06-27 00:18_

I changed the base to #14164 — can you rebase? Otherwise I will later.

---

_@zanieb approved on 2025-06-27 00:18_

---

_Comment by @charliermarsh on 2025-06-27 00:27_

Done, thanks.

---

_Merged by @charliermarsh on 2025-06-27 02:45_

---

_Closed by @charliermarsh on 2025-06-27 02:45_

---

_Branch deleted on 2025-06-27 02:45_

---

_Comment by @paveldikov on 2025-07-18 11:00_

Question: what is the default behaviour for `--python-platform` (when unspecified, on a Linux box)?

Will it pick naive `linux`, or will it intelligently determine the correct `manylinux` tag from the local `glibc` version?

(asking because we have a significant proportion of users on RHEL7)

---

_Comment by @konstin on 2025-07-18 11:12_

By default, we'll use the same libc as the Python interpreter, i.e. the host glibc version.

`--python-platform` is a special case where users want to `uv pip compile` or `uv pip install` on a platform that is different from their current Python interpreter and host. For example, if you could run `uv pip compile --python-platform linux` on Windows to resolve Linux requirements, or run `uv pip install --python-platform windows` on a Mac to install the Windows packages to.

If you don't set `--python-platform` explicitly, this change does not affect you.

---
