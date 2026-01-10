```yaml
number: 11131
title: "Improve performance of uv-python crate's manylinux submodule"
type: pull_request
state: merged
author: cthoyt
labels:
  - performance
  - external
assignees: []
merged: true
base: main
head: manylinux-performance-improvements
created_at: 2025-01-31T11:09:30Z
updated_at: 2025-05-28T13:25:54Z
url: https://github.com/astral-sh/uv/pull/11131
synced_at: 2026-01-10T11:10:34Z
```

# Improve performance of uv-python crate's manylinux submodule

---

_Pull request opened by @cthoyt on 2025-01-31 11:09_

## Summary

This PR makes a few performance improvements:

1. Reduces the need to unpack and repack a `_GLibCVersion` tuple
2. Reduces the doubled call to `_is_compatible(arch, glibc_version)`
3. Moves the assignment of the `tag` variable directly into the yield, reducing memory allocation in case this is never used when `_is_compatible(arch, glibc_version)` is false
4. Combines the check of the `glibc_version` being in `_LEGACY_MANYLINUX_MAP` and the assignment to the variable together. I'm not sure if this is actually better, but using the assignment expression reduces this from 4 lines to 2

## Test Plan

I upstreamed these changes in https://github.com/pypa/packaging/pull/869. Otherwise, I'm pretty confident this is a minor change that works the same

---

_Comment by @charliermarsh on 2025-01-31 14:57_

Thanks for this. I'm hesitant to make changes here since the code is vendored... Personally, I'd prefer to wait for this to merge upstream.

---

_Comment by @cthoyt on 2025-01-31 15:03_

@charliermarsh totally agreed! I didn't realize it was vendored until after I had already made the PR... sorry for missing the README. I will watch out to see what the upstream group says, and ping you if we ever get some feedback!

Cheers

---

_Comment by @charliermarsh on 2025-01-31 15:04_

Thank you!

---

_Label `performance` added by @charliermarsh on 2025-01-31 15:04_

---

_Label `upstream` added by @charliermarsh on 2025-01-31 15:04_

---

_Comment by @cthoyt on 2025-05-22 22:05_

FYI @charliermarsh my changes got merged into upstream! They haven't been minted into a new release yet. I'm not sure how you'd want to proceed with changing the vendored code in uv. I can check back in when the next release is made for `packaging`

---

_Comment by @konstin on 2025-05-28 13:08_

Thanks for upstreaming!

---

_@konstin approved on 2025-05-28 13:09_

---

_Merged by @konstin on 2025-05-28 13:09_

---

_Closed by @konstin on 2025-05-28 13:09_

---

_Branch deleted on 2025-05-28 13:25_

---
