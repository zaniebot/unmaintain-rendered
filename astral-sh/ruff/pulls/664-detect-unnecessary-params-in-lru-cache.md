```yaml
number: 664
title: "Detect unnecessary params in `lru_cache` "
type: pull_request
state: merged
author: chammika-become
labels: []
assignees: []
merged: true
base: main
head: rewrite-lru-cache
created_at: 2022-11-09T05:51:08Z
updated_at: 2022-11-09T16:02:15Z
url: https://github.com/astral-sh/ruff/pull/664
synced_at: 2026-01-12T05:48:45Z
```

# Detect unnecessary params in `lru_cache` 

---

_Pull request opened by @chammika-become on 2022-11-09 05:51_

This PR address #642
Only detects atm. Fix for this check WIP.

- [x]  Define the `pyupgrade` rule
- [x]  Define the ast based logic for triggering
- [x]  Add test fixture
- [x]  Update the generated files (documentation and generated code)
- [x]  More rigorous check with `match_name_or_attr_from_module`

---

_Comment by @chammika-become on 2022-11-09 05:55_

Plan to make this check more rigorous as suggested, before the merge.
https://github.com/charliermarsh/ruff/issues/642#issuecomment-1308224389


---

_Review comment by @squiddy on `src/checks.rs`:1453 on 2022-11-09 06:04_

Cool stuff. Just a small nitpick: 

```suggestion
                "Unnessary parameters to functools.lru_cache".to_string()
```

---

_@squiddy reviewed on 2022-11-09 06:05_

---

_Merged by @charliermarsh on 2022-11-09 15:02_

---

_Closed by @charliermarsh on 2022-11-09 15:02_

---

_Comment by @charliermarsh on 2022-11-09 15:02_

Awesome! Thanks @chammika-become!

---

_@charliermarsh reviewed on 2022-11-09 15:03_

---

_Review comment by @charliermarsh on `src/checks.rs`:1718 on 2022-11-09 15:03_

@chammika-become - I added `CheckKind::UnnecessaryLRUCacheParams` to this `fixable` list to ensure that it's marked as such in the `README.md`.

---

_@charliermarsh reviewed on 2022-11-09 15:03_

---

_Review comment by @charliermarsh on `src/pyupgrade/checks.rs`:229 on 2022-11-09 15:03_

@chammika-become - I made this a little more conservative (used `map` then `upwrap_or_default` instead of force `unwrap`).

---

_@chammika-become reviewed on 2022-11-09 16:02_

---

_Review comment by @chammika-become on `src/pyupgrade/checks.rs`:229 on 2022-11-09 16:02_

Great! I've learned a lot from the improvements you made. Thanks!

---
