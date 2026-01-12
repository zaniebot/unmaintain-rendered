```yaml
number: 8759
title: "fix bug where `python_version < '0'` could appear in a final resolution"
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - lock
assignees: []
merged: true
base: main
head: ag/python-version-zero
created_at: 2024-11-01T18:28:57Z
updated_at: 2024-11-03T15:37:29Z
url: https://github.com/astral-sh/uv/pull/8759
synced_at: 2026-01-12T16:08:29Z
```

# fix bug where `python_version < '0'` could appear in a final resolution

---

_@BurntSushi_

This PR fixes a bug where it was possible for dependencies to be
included in a final resolution with markers that always evaluate to
false. Specifically, `python_version < '0'`.

While we do filter based on Python markers during forking, it turns out
that the markers for each fork are "combined" *after* this filtering
step. But the process of combination can result in a more specific
marker that is always false for the configured Python requirement. This
could result in dependencies with markers that are always false (like
`python_version < '0'`) appearing in the resolution.

The first commit in this PR adds a regression test (with an undesirable
result), and the second commit fixes the regression and updates the
test.

Fixes #8676


---

_Label `bug` added by @BurntSushi on 2024-11-01 18:29_

---

_Label `lock` added by @BurntSushi on 2024-11-01 18:29_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-11-01 18:29_

---

_Comment by @charliermarsh on 2024-11-01 18:42_

My only question here is... should it not be impossible for these to have made it into the resolution in the first place?

---

_Comment by @BurntSushi on 2024-11-01 19:21_

That's a darn good question. You're instinct is right. We are actually combining markers in forks _after_ doing a filtering step based on the Python requirement. I added a second filtering step in this PR to fix the bug in the short term. I'll see about cleaning this up as part of my conflicting groups work.

---

_@charliermarsh approved on 2024-11-01 19:24_

---

_Merged by @BurntSushi on 2024-11-01 20:11_

---

_Closed by @BurntSushi on 2024-11-01 20:11_

---

_Branch deleted on 2024-11-01 20:11_

---

_@konstin reviewed on 2024-11-03 15:37_

---

_Review comment by @konstin on `crates/uv/tests/it/pip_compile.rs`:420 on 2024-11-03 15:37_

This test is slow (6s - 7s), can we replace it by a faster, more minimal example? This problem should model well in packse.

---
