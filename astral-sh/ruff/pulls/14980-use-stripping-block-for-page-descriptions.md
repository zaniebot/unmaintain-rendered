```yaml
number: 14980
title: "Use stripping block (`|-`) for page descriptions"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - documentation
assignees: []
merged: true
base: main
head: mkdocs-descriptions
created_at: 2024-12-15T03:07:32Z
updated_at: 2024-12-15T16:20:28Z
url: https://github.com/astral-sh/ruff/pull/14980
synced_at: 2026-01-12T15:55:49Z
```

# Use stripping block (`|-`) for page descriptions

---

_@InSyncWithFoo_

## Summary

Resolves #14976.

Currently, we uses this "[plain scalar](https://yaml.org/spec/1.2.2/#733-plain-style)" format:

```yaml
description: Checks for `if key in dictionary: del dictionary[key]`.
```

Plain scalar must not contain the sequence `: `, however, so the above is invalid.

This PR changes that to:

```yaml
description: |-
  Checks for `if key in dictionary: del dictionary[key]`.
```

`|` denotes a "[block scalar](https://yaml.org/spec/1.2.2/#81-block-scalar-styles)", whereas [the `-` chomping indicator](https://yaml.org/spec/1.2.2/#8112-block-chomping-indicator) requires that a trailing newline, if any, must be stripped.

## Test Plan

![](https://github.com/user-attachments/assets/f00b606a-d6fe-46ac-a1c5-6a8665204ea3)

---

_Comment by @charliermarsh on 2024-12-15 03:13_

Can you include a screenshot of the before and after in the test plan?

---

_Label `documentation` added by @charliermarsh on 2024-12-15 03:13_

---

_Comment by @github-actions[bot] on 2024-12-15 03:15_

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

_Comment by @InSyncWithFoo on 2024-12-15 03:21_

Done. I'm not sure if this breaks something else, though.

---

_Comment by @MichaReiser on 2024-12-15 16:07_

Oh nice find. This change is a bit scary because there's no good way to test its impact. I clicked through a random selection of rules and I didn't see any issues. Let's give this a try. Thank you

---

_@MichaReiser approved on 2024-12-15 16:07_

---

_Merged by @MichaReiser on 2024-12-15 16:07_

---

_Closed by @MichaReiser on 2024-12-15 16:07_

---

_Branch deleted on 2024-12-15 16:20_

---
