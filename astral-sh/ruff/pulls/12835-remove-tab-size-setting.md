```yaml
number: 12835
title: Remove tab-size setting
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - configuration
assignees: []
merged: true
base: ruff-0.7
head: remove-tab-size-setting
created_at: 2024-08-12T09:11:36Z
updated_at: 2024-10-08T12:40:25Z
url: https://github.com/astral-sh/ruff/pull/12835
synced_at: 2026-01-10T20:59:36Z
```

# Remove tab-size setting

---

_Pull request opened by @MichaReiser on 2024-08-12 09:11_

## Summary

The `tab-size` setting was deprecated in Ruff 0.1.2 and using it is an error since Ruff 0.5 (https://github.com/astral-sh/ruff/pull/12006).

This PR removes the option entirely.

Closes https://github.com/astral-sh/ruff/issues/12041

## Test Plan

```
ruff failed
  Cause: Failed to parse /home/micha/astral/test/pyproject.toml
  Cause: TOML parse error at line 1, column 1
  |
1 | [tool.ruff]
  | ^^^^^^^^^^^
unknown field `tab-size`

```


---

_Label `configuration` added by @MichaReiser on 2024-08-12 09:11_

---

_@AlexWaygood approved on 2024-08-12 11:51_

It's been deprecated for a very long time so I'm okay with removing it, but I also think it would be fine to leave the nice error message there for another minor version. It's not really a maintenance burden 😄

---

_Comment by @codspeed-hq[bot] on 2024-08-12 11:51_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/remove-tab-size-setting)

### Merging #12835 will **not alter performance**

<sub>Comparing <code>remove-tab-size-setting</code> (233ce67) with <code>ruff-0.7</code> (2a615d2)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-08-12 11:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-08-12 12:02_

> It's been deprecated for a very long time so I'm okay with removing it, but I also think it would be fine to leave the nice error message there for another minor version. It's not really a maintenance burden 😄

I consider it a maintenance burden because we consider it for removal in every minor but never go through with it. That's why we should either decide to close the tasks and not remove them until 1.0 (or in ten years when the above argument no longer applies) or to go forward with it.

---

_Added to milestone `v0.7` by @MichaReiser on 2024-08-12 14:00_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-10-08 11:33_

---

_@AlexWaygood approved on 2024-10-08 11:36_

Thanks!

---

_Label `internal` added by @MichaReiser on 2024-10-08 11:47_

---

_Merged by @MichaReiser on 2024-10-08 12:40_

---

_Closed by @MichaReiser on 2024-10-08 12:40_

---

_Branch deleted on 2024-10-08 12:40_

---
