```yaml
number: 10798
title: "Respect `# noqa` directives on `__all__` openers"
type: pull_request
state: merged
author: charliermarsh
labels:
  - suppression
assignees: []
merged: true
base: main
head: charlie/nqa
created_at: 2024-04-05T23:40:59Z
updated_at: 2024-04-06T15:00:45Z
url: https://github.com/astral-sh/ruff/pull/10798
synced_at: 2026-01-10T22:47:03Z
```

# Respect `# noqa` directives on `__all__` openers

---

_Pull request opened by @charliermarsh on 2024-04-05 23:40_

## Summary

Historically, given:

```python
__all__ = [  # noqa: F822
    "Bernoulli",
    "Beta",
    "Binomial",
]
```

The F822 violations would be attached to the `__all__`, so this `# noqa` would be enforced for _all_ definitions in the list. This changed in https://github.com/astral-sh/ruff/pull/10525 for the better, in that we now use the range of each string. But these `# noqa` directives stopped working.

This PR sets the `__all__` as a parent range in the diagnostic, so that these directives are respected once again.

Closes https://github.com/astral-sh/ruff/issues/10795.

## Test Plan

`cargo test`


---

_Label `suppression` added by @charliermarsh on 2024-04-05 23:40_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-04-05 23:41_

---

_Comment by @github-actions[bot] on 2024-04-05 23:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F822_3.py`:7 on 2024-04-06 09:48_

Is it worth also adding this to the test, to illustrate that if you _do_ only `noqa` _one_ of the items in an `__all__` definition or augmentation, you'll still get an `F822` error on the other items (if the other items are also undefined)? That seems desirable to me, so I think probably worth asserting?

```suggestion
]

if True:
    __all__ += [
        "ContinuousBernoulli",  # noqa: F822
        "ExponentialFamily",  # F822 emitted here
    ]

```

---

_@AlexWaygood approved on 2024-04-06 09:48_

Nice, thanks for the fix!

---

_@AlexWaygood reviewed on 2024-04-06 09:49_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/all.rs`:43 on 2024-04-06 09:49_

Nit: maybe this could be called `DunderAllDefinition` rather than `DunderAllNames`? The name of the struct is very similar to `DunderAllName` currently

---

_Merged by @charliermarsh on 2024-04-06 14:51_

---

_Closed by @charliermarsh on 2024-04-06 14:51_

---

_Branch deleted on 2024-04-06 14:51_

---
