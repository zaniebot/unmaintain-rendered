```yaml
number: 18633
title: "[`pyupgrade`] Extend version detection to include `sys.version_info.major` (`UP036`)"
type: pull_request
state: merged
author: IDrokin117
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/ruff-18165
created_at: 2025-06-11T19:49:03Z
updated_at: 2025-06-23T20:09:18Z
url: https://github.com/astral-sh/ruff/pull/18633
synced_at: 2026-01-10T18:39:08Z
```

# [`pyupgrade`] Extend version detection to include `sys.version_info.major` (`UP036`)

---

_Pull request opened by @IDrokin117 on 2025-06-11 19:49_

## Summary

Resolves #18165 

Added pattern `["sys", "version_info", "major"]` to the existing matches for `sys.version_info` to ensure consistent handling of both the base object and its major version attribute.

## Test Plan
`cargo nextest run` and `cargo insta test`


---

_Label `rule` added by @ntBre on 2025-06-11 20:16_

---

_Comment by @github-actions[bot] on 2025-06-11 20:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:114 on 2025-06-11 21:29_

This seems pretty minor, especially because the fix is always unsafe, but this also calls `map_subscript` on `sys.version_info.major`, which we might not _really_ want to allow. In other words, I think this would fix code like

```python
import sys

if sys.version_info.major[1] > 3:
    print(3)
else:
    print(2)
```

to avoid the `TypeError` that would otherwise occur:

```pycon
>>> import sys
>>> sys.version_info.major[1]
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    sys.version_info.major[1]
    ~~~~~~~~~~~~~~~~~~~~~~^^^
TypeError: 'int' object is not subscriptable
```

It's much more convenient to add the check here, but I might lean slightly toward a separate `resolve_qualified_name` call to avoid `map_subscript` on `major`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:114 on 2025-06-11 21:31_

This isn't related to your changes, but I guess this rule is pretty lenient with these comparisons anyway. For example, it still applies to comparisons like these that would also fail at runtime:

```python
import sys

if sys.version_info < 3: ...
if sys.version_info[0] < (3, 0): ...
```

---

_@ntBre reviewed on 2025-06-11 21:34_

Thanks for working on this! I think this looks good, I just had one suggestion for making the check a bit stricter. What do you think?

---

_@IDrokin117 reviewed on 2025-06-12 20:24_

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:114 on 2025-06-12 20:24_

@ntBre Thanks for the review. Point taken. I've added a dedicated `resolve_qualified_name` call for the `.major` case. Could you take a look at the revised approach?
I also want to implement the fix for your second remark. Should this go into the current PR or would you prefer a separate PR?

---

_Review requested from @ntBre by @IDrokin117 on 2025-06-14 15:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:458 on 2025-06-16 19:25_

nit: if this is all we use the `Checker` for, we could also just pass a `semantic: &SemanticModel` argument here.

I think it was a good idea to factor out a helper function here, though!

---

_@ntBre approved on 2025-06-16 19:32_

Nice, thanks!

> I also want to implement the fix for your second remark. Should this go into the current PR or would you prefer a separate PR?

Let's go ahead and merge the changes from this PR and leave the rest for a follow-up. We should probably also open an issue for the invalid comparisons to see if anyone else has feedback on possible fixes. There's currently a related rule ([unrecognized-version-info-check (PYI003)](https://docs.astral.sh/ruff/rules/unrecognized-version-info-check/#unrecognized-version-info-check-pyi003), and really PYI003-PYI006) that I think only applies in `.pyi` files. So there are already at least two options for detecting these issues: adding them to UP036 or PYI003.

---

_@IDrokin117 reviewed on 2025-06-17 06:02_

---

_Review comment by @IDrokin117 on `crates/ruff_linter/src/rules/pyupgrade/rules/outdated_version_block.rs`:458 on 2025-06-17 06:02_

Got it. I've replaced `checker` with `semantic`.

---

_@ntBre approved on 2025-06-20 13:39_

---

_Comment by @IDrokin117 on 2025-06-23 19:43_

@ntBre Seems one of the actions failed due to "curl: (22) The requested URL returned error: 403". Could you please rerun or skip it to merge?

---

_Comment by @charliermarsh on 2025-06-23 19:46_

I got it.

---

_Comment by @ntBre on 2025-06-23 19:56_

Oops, I guess I shouldn't have dismissed the notification until I saw it actually merge, sorry! 

It looks like the workflow might still be using an old version of the `ci.yaml` file with an old version of install-action. This error should be resolved on `main`.

---

_Merged by @ntBre on 2025-06-23 20:01_

---

_Closed by @ntBre on 2025-06-23 20:01_

---
