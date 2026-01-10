```yaml
number: 13680
title: Avoid using 3.8 in tests when feasible
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/38-eol
created_at: 2025-05-27T14:22:46Z
updated_at: 2025-05-27T18:34:50Z
url: https://github.com/astral-sh/uv/pull/13680
synced_at: 2026-01-10T11:10:42Z
```

# Avoid using 3.8 in tests when feasible

---

_Pull request opened by @zanieb on 2025-05-27 14:22_

See https://github.com/astral-sh/uv/issues/13676

Python 3.8 is EOL, and we should avoid using it for tests unless we're covering 3.8-specific behaviors.

In some cases, the Python version we require for the test is arbitrary, for which I've added a new constant (set to 3.12). In other cases, we require an older Python version for compatibility with various packages, like `setuptools`. For those, we can _probably_ switch to a newer version but it'd be more invasive as we'd need to change our constraints or `exclude-newer`.

Adds a feature flag for cases where we still need to use the EOL version.

There's a lot of usage in packse scenarios too, I'll update those separately because the diff will be large.

---

_Label `testing` added by @zanieb on 2025-05-27 14:22_

---

_Marked ready for review by @zanieb on 2025-05-27 16:03_

---

_Review comment by @jtfmumm on `crates/uv/tests/it/common/mod.rs`:36 on 2025-05-27 18:03_

This will be useful. 

---

_@jtfmumm reviewed on 2025-05-27 18:03_

---

_Review comment by @konstin on `crates/uv/tests/it/build.rs`:901 on 2025-05-27 18:17_

any reason no to bump those to 3.12?

---

_Review comment by @konstin on `crates/uv/tests/it/venv.rs`:890 on 2025-05-27 18:20_

The variable names need updating

---

_@konstin approved on 2025-05-27 18:21_

---

_@zanieb reviewed on 2025-05-27 18:23_

---

_Review comment by @zanieb on `crates/uv/tests/it/build.rs`:901 on 2025-05-27 18:23_

Most things I didn't bump to 3.12 failed on that version, let me double check this one though..

---

_@zanieb reviewed on 2025-05-27 18:27_

---

_Review comment by @zanieb on `crates/uv/tests/it/build.rs`:901 on 2025-05-27 18:27_

(This one does pass with 3.12)

---

_Merged by @zanieb on 2025-05-27 18:34_

---

_Closed by @zanieb on 2025-05-27 18:34_

---

_Branch deleted on 2025-05-27 18:34_

---
