```yaml
number: 14259
title: "Fix handling of inverted `~=` with `python_version` and `python_full_version`"
type: pull_request
state: closed
author: konstin
labels:
  - bug
assignees: []
base: main
head: konsti/fix-tilde-equal-python
created_at: 2025-06-25T12:01:23Z
updated_at: 2025-10-09T13:46:25Z
url: https://github.com/astral-sh/uv/pull/14259
synced_at: 2026-01-10T06:36:15Z
```

# Fix handling of inverted `~=` with `python_version` and `python_full_version`

---

_Pull request opened by @konstin on 2025-06-25 12:01_

Fix the handling of `"<version>" ~= python_version` and `"<version>" ~= python_full_version`, which need custom handling as we know the number of digits on the RHS, but not their meaning.

---

_@ibraheemdev reviewed on 2025-06-25 14:36_

---

_Review comment by @ibraheemdev on `crates/uv-pep508/src/marker/algebra.rs`:188 on 2025-06-25 14:36_

All the other branches call `normalize_specifier`, this one probably should to. That might be the cause of the test failure (but I'm not sure why the non-normalized version would be comparing equal to the normalized).

---

_@konstin reviewed on 2025-06-26 10:43_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/algebra.rs`:188 on 2025-06-26 10:43_

I found some pre-existing inconsistencies: https://github.com/astral-sh/uv/issues/14270

---

_Comment by @konstin on 2025-06-26 10:45_

While the isolated `cargo nextest run` now passes, `cargo test -p uv-pep508` tends to still fail locally for me, I assume we also have to handle a trailing zero in patch position.

---

_@konstin reviewed on 2025-06-30 09:32_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/algebra.rs`:188 on 2025-06-30 09:32_

Rebased on #14271 it now seems to work except a windows arm test with `requires-python "~= 3.12.0"` I haven't investigated yet.

---

_@konstin reviewed on 2025-07-07 08:13_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/algebra.rs`:188 on 2025-07-07 08:13_

We're now doing entirely different normalization

---

_Label `bug` added by @konstin on 2025-07-07 08:14_

---

_Renamed from "Fix handling of `~=` with `python_version` and `python_full_version`" to "Fix handling of inverted `~=` with `python_version` and `python_full_version`" by @konstin on 2025-07-07 08:14_

---

_Marked ready for review by @konstin on 2025-07-07 08:27_

---

_Comment by @konstin on 2025-07-07 08:35_

With #14271 applied, this simpler fix now seems to work.

---

_Comment by @konstin on 2025-07-24 09:59_

@ibraheemdev Given that this works and it only changes (fixes) the behavior of an edge case, should we go ahead and merge this?

---

_Closed by @konstin on 2025-10-09 13:46_

---
