```yaml
number: 11291
title: Patch pkg-config files to be relocatable
type: pull_request
state: merged
author: geofft
labels: []
assignees: []
merged: true
base: main
head: pkgconfig-relocatable
created_at: 2025-02-06T19:49:58Z
updated_at: 2025-02-07T23:03:55Z
url: https://github.com/astral-sh/uv/pull/11291
synced_at: 2026-01-12T16:09:46Z
```

# Patch pkg-config files to be relocatable

---

_@geofft_

Previously, we patched pkg-config .pc files to have the absolute path to the directory where we unpack a python-build-standalone release.  As discussed in #11028, we can use ${pcfiledir} in a .pc file to indicate paths relative to the location of the file itself.

This change was implemented in astral-sh/python-build-standalone#507, so for newer python-build-standalone releases, we don't need to do any patching. Optimize this case by only modifying the .pc file if an actual change is needed (which might be helpful down the line with hard links or something). For older releases, change uv's patch to match what python-build-standalone now does.

---

_Review requested from @charliermarsh by @geofft on 2025-02-06 19:50_

---

_Review requested from @zanieb by @geofft on 2025-02-06 19:50_

---

_@zanieb approved on 2025-02-06 19:56_

---

_@charliermarsh reviewed on 2025-02-06 19:57_

---

_Review comment by @charliermarsh on `crates/uv-python/src/sysconfig/mod.rs`:352 on 2025-02-06 19:57_

Does this mean we aren't applying this to `.pc` files that we've already patched? Since, in that case, I guess the RHS of the `=` would _not_ be `/install`?

---

_@zanieb reviewed on 2025-02-06 19:59_

---

_Review comment by @zanieb on `crates/uv-python/src/sysconfig/mod.rs`:352 on 2025-02-06 19:59_

Unrelated, but does the comment above

>  // Given, e.g., `prefix=/install`, replace with `prefix=/real/prefix`.

Need to be updated?

---

_@zanieb reviewed on 2025-02-06 20:00_

---

_Review comment by @zanieb on `crates/uv-python/src/sysconfig/mod.rs`:352 on 2025-02-06 20:00_

I don't mind if we avoid patching already patched `.pc` files without `--reinstall`, though I would have to double check how the patch on reinstall behavior works in practice.

---

_@geofft reviewed on 2025-02-06 20:43_

---

_Review comment by @geofft on `crates/uv-python/src/sysconfig/mod.rs`:352 on 2025-02-06 20:43_

Wait, does `--reinstall` not delete and re-unpack the entire directory? I don't follow the whole flow of this code but that surprises me if so.

(Or do you mean that this doesn't apply to .pc files that we patched during the python-build-standalone release process? Yes.)

---

_@charliermarsh reviewed on 2025-02-06 21:31_

---

_Review comment by @charliermarsh on `crates/uv-python/src/sysconfig/mod.rs`:352 on 2025-02-06 21:31_

Yeah I believe `--reinstall` does delete and re-unpack the entire directory. I was referring to the case of: you run `uv python install 3.12`, and `3.12` is already installed. In that case, we _do_ call through to these patch routines, but I don't think this patch would take effect (since the already-patched `.pc` file wouldn't match the above pattern). I defer to you two on whether that's important (seems fine).

---

_@zanieb reviewed on 2025-02-06 22:46_

---

_Review comment by @zanieb on `crates/uv-python/src/sysconfig/mod.rs`:352 on 2025-02-06 22:46_

I agree it seems fine without an explicit reinstall. Thanks for clarifying the behavior.

---

_@charliermarsh approved on 2025-02-07 22:44_

---

_Merged by @zanieb on 2025-02-07 23:03_

---

_Closed by @zanieb on 2025-02-07 23:03_

---
