```yaml
number: 17933
title: "[`pyupgrade`] Add `resource.error` as deprecated alias of `OSError` (`UP024`)"
type: pull_request
state: merged
author: DimitriPapadopoulos
labels:
  - rule
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-05-08T05:56:36Z
updated_at: 2025-05-14T18:24:34Z
url: https://github.com/astral-sh/ruff/pull/17933
synced_at: 2026-01-12T15:56:08Z
```

# [`pyupgrade`] Add `resource.error` as deprecated alias of `OSError` (`UP024`)

---

_@DimitriPapadopoulos_

## Summary

Partially addresses #17935.

[`resource.error`](https://docs.python.org/3/library/resource.html#resource.error) is a deprecated alias of [`OSError`](https://docs.python.org/3/library/exceptions.html#OSError).
> _Changed in version 3.3:_ Following [**PEP 3151**](https://peps.python.org/pep-3151/), this class was made an alias of [`OSError`](https://docs.python.org/3/library/exceptions.html#OSError).

Add it to the list of `OSError` aliases found by [os-error-alias (UP024)](https://docs.astral.sh/ruff/rules/os-error-alias/#os-error-alias-up024).

## Test Plan

Sorry, I usually don't program in Rust. Could you at least point me to the test I would need to modify?

---

_Renamed from "Add another alias of OSError: ressource.error alias" to "Add another alias of OSError: ressource.error" by @DimitriPapadopoulos on 2025-05-08 05:57_

---

_Comment by @github-actions[bot] on 2025-05-08 06:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "Add another alias of OSError: ressource.error" to "Add deprecated alias of OSError: ressource.error" by @DimitriPapadopoulos on 2025-05-08 06:07_

---

_Renamed from "Add deprecated alias of OSError: ressource.error" to "[pyupgrade] Add deprecated alias of OSError: ressource.error" by @DimitriPapadopoulos on 2025-05-08 06:43_

---

_Review requested from @ntBre by @MichaReiser on 2025-05-09 06:32_

---

_@ntBre reviewed on 2025-05-09 22:29_

Nice, this makes sense to me! For a test, you'll want to add a new UP024 test fixture like this:

https://github.com/astral-sh/ruff/blob/ec2fdadb35e618624d865e7a241452d6aff833e4/crates/ruff_linter/resources/test/fixtures/pyupgrade/UP024_3.py#L1-L8

It looks like `UP024_5.py` is the next available name (I linked to 3 since it's a bit more minimal of an example than 4).

And then you can add a new `test_case` here:

https://github.com/astral-sh/ruff/blob/ec2fdadb35e618624d865e7a241452d6aff833e4/crates/ruff_linter/src/rules/pyupgrade/mod.rs#L48-L53

That should be all you need to generate new snapshots, which you can update with cargo-insta. The [prerequisites](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#prerequisites) section in the CONTRIBUTING.md file has some links for setting that up, if you need it. I'm happy to help if you run into any issues! I think this is good to go once we have a simple test case :)

---

_Comment by @DimitriPapadopoulos on 2025-05-10 11:37_

I chose to modify the existing test instead:
https://github.com/astral-sh/ruff/blob/ec2fdadb35e618624d865e7a241452d6aff833e4/crates/ruff_linter/resources/test/fixtures/pyupgrade/UP024_2.py#L8-L16

Unfortunately, the whole toolchain eats too much disk space, I cannot run tests on my computer.  I'll try on a machine with more disk space next week.

---

_Label `rule` added by @MichaReiser on 2025-05-12 07:45_

---

_Comment by @ntBre on 2025-05-12 20:40_

Thanks for adding the tests. Let me know if you want me to update the snapshots for you.

---

_Comment by @DimitriPapadopoulos on 2025-05-13 13:37_

Snapshots updated.

---

_@ntBre approved on 2025-05-14 14:35_

Thanks!

---

_Renamed from "[pyupgrade] Add deprecated alias of OSError: ressource.error" to "[`pyupgrade`] Add `resource.error` as deprecated alias of `OSError` (`UP024`)" by @ntBre on 2025-05-14 14:37_

---

_Merged by @ntBre on 2025-05-14 14:37_

---

_Closed by @ntBre on 2025-05-14 14:37_

---

_Branch deleted on 2025-05-14 18:24_

---
