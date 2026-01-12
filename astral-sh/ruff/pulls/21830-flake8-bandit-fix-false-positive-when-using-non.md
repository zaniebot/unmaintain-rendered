```yaml
number: 21830
title: "[flake8-bandit] Fix false positive when using non-standard `CSafeLoader` path (S506)."
type: pull_request
state: merged
author: prakhar1144
labels: []
assignees: []
merged: true
base: main
head: fix-unsafe-yaml-load
created_at: 2025-12-07T09:58:39Z
updated_at: 2025-12-07T10:40:47Z
url: https://github.com/astral-sh/ruff/pull/21830
synced_at: 2026-01-12T15:57:34Z
```

# [flake8-bandit] Fix false positive when using non-standard `CSafeLoader` path (S506).

---

_@prakhar1144_

## Summary

Using `CSafeLoader` from non-standard path i.e `yaml.cyaml.CSafeLoader` resulted in false reporting of `unsafe-yaml-load`.

Example:
```py
import yaml

yaml.load("{}", Loader=yaml.cyaml.CSafeLoader)
```

resulted in:
```sh
S506 Probable use of unsafe loader `CSafeLoader` with `yaml.load`. Allows instantiation of arbitrary objects. Consider `yaml.safe_load`.
 --> /Users/foo/ruff-input/main.py:5:24
  |
3 | import yaml
4 |
5 | yaml.load("{}", Loader=yaml.cyaml.CSafeLoader)
  |                        ^^^^^^^^^^^^^^^^^^^^^^
```

The PR fixes the bug by considering `yaml.cyaml.CSafeLoader` as a possible way to use `CSafeLoader`.

Fixes #21673.

## Test Plan

Added example scenarios in `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S506.py`.


---

_Comment by @astral-sh-bot[bot] on 2025-12-07 10:10_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @prakhar1144 on 2025-12-07 10:25_

---

_@MichaReiser approved on 2025-12-07 10:40_

Thank you

---

_Merged by @MichaReiser on 2025-12-07 10:40_

---

_Closed by @MichaReiser on 2025-12-07 10:40_

---
