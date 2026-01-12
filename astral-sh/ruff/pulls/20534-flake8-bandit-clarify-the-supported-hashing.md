```yaml
number: 20534
title: "[`flake8-bandit`] Clarify the supported hashing functions (`S324`)"
type: pull_request
state: merged
author: danparizher
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-16572
created_at: 2025-09-23T13:58:37Z
updated_at: 2025-09-24T20:35:45Z
url: https://github.com/astral-sh/ruff/pull/20534
synced_at: 2026-01-12T15:57:04Z
```

# [`flake8-bandit`] Clarify the supported hashing functions (`S324`)

---

_@danparizher_

## Summary

Fixes #16572


---

_Comment by @github-actions[bot] on 2025-09-23 14:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre reviewed on 2025-09-23 20:58_

Thanks for working on this, and I think it's nice to have some note like this, but it doesn't seem exactly accurate to say that we support the values in `algorithms_guaranteed`. I checked all the versions supported by uv (3.7 through 3.14), and `sha` and `md4` haven't been guaranteed algorithms at least since 3.7. I also checked in the [CPython source](https://github.com/python/cpython/blob/076ca6c3c8df3030307e548d9be792ce3c1c6eea/Lib/hashlib.py#L57) as far back as 3.2, when the constant was added.

I was able to call `hashlib.new("md4")` on 3.7, but `sha` didn't work on any version I tested.

Fortunately, I think we can just drop the reference to `algorithms_guaranteed` and the note still reads well. Since we're effectively enumerating every function checked by the rule, it might make sense to mention the `crypt` functions (`crypt` and `mksalt`) as well.

---

_Label `documentation` added by @ntBre on 2025-09-23 20:58_

---

_Comment by @danparizher on 2025-09-23 21:09_

Thanks for checking, that makes sense. I changed it to explicitly list what the rule actually targets, plus `crypt.crypt` and `crypt.mksalt` when used with the weak methods (`METHOD_CRYPT`, `METHOD_MD5`, `METHOD_BLOWFISH`)

---

_Review requested from @ntBre by @danparizher on 2025-09-23 21:12_

---

_@ntBre reviewed on 2025-09-24 20:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/rules/hashlib_insecure_hash_functions.rs`:61 on 2025-09-24 20:03_

```suggestion
/// - [Python documentation: `FIPS` - FIPS compliant hashlib implementation](https://docs.python.org/3/library/hashlib.html#hashlib.algorithms_guaranteed)
```

---

_@ntBre approved on 2025-09-24 20:04_

Thank you! I just made one suggestion to restore the reference link. Even if we're not checking that list directly, I think the link could still be helpful.

---

_Renamed from "[`flake8-bandit`] Clarify `S324` only targets Python-guaranteed hashlib algorithms (`S324`)" to "[`flake8-bandit`] Clarify the supported hashing functions (`S324`)" by @ntBre on 2025-09-24 20:07_

---

_Merged by @ntBre on 2025-09-24 20:10_

---

_Closed by @ntBre on 2025-09-24 20:10_

---

_Branch deleted on 2025-09-24 20:35_

---
