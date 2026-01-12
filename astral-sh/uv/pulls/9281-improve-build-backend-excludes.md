```yaml
number: 9281
title: Improve build backend excludes
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-fixes
created_at: 2024-11-20T16:39:02Z
updated_at: 2024-11-21T11:20:31Z
url: https://github.com/astral-sh/uv/pull/9281
synced_at: 2026-01-12T16:08:44Z
```

# Improve build backend excludes

---

_@konstin_

This PR contains three smaller improvements:
* Improve the include/exclude logging. We're still showing the current directory as empty backticks, not sure what to do about that
* Add early stopping to license file globbing, so we don't traverse the whole directory recursively when license files can only be in few places
* Support explicit wheel excludes. These are still not entirely right, but at least we're correctly excluding compiled python files now. The next step is to make sure that the wheel excludes contain all pattern from source dist excludes, to make sure source tree -> wheel can't have more files than source tree -> source dist -> wheel.

---

_Label `preview` added by @konstin on 2024-11-20 16:39_

---

_Review requested from @BurntSushi by @konstin on 2024-11-20 16:39_

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:509 on 2024-11-20 16:54_

Is this the thing that will emit an empty string for the CWD? Can we make it emit `./` instead for that case?

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:323 on 2024-11-20 16:55_

Can you say why you don't want to get more files in `source tree -> wheel` versus `source tree -> source dist -> wheel`?

---

_@BurntSushi approved on 2024-11-20 16:56_

---

_@konstin reviewed on 2024-11-21 11:18_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:509 on 2024-11-21 11:18_

I have an idea, but it affects more places so I'll follow up

---

_@konstin reviewed on 2024-11-21 11:20_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:323 on 2024-11-21 11:20_

Imagine we exclude `src/a/b.py` in source dist, but not in wheel. That means in source dist -> wheel, `src/a/b.py` is part of the final packages, but we remove it in source tree -> source dist, so the source dist -> wheel step can't include it (the file doesn't exist in the `.tar.gz`), so `src/a/b.py` isn't part of the final package.

---

_Merged by @konstin on 2024-11-21 11:20_

---

_Closed by @konstin on 2024-11-21 11:20_

---

_Branch deleted on 2024-11-21 11:20_

---
