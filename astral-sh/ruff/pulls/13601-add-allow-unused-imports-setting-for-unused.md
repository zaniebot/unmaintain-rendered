```yaml
number: 13601
title: "Add `allow-unused-imports` setting for `unused-import` rule (`F401`)"
type: pull_request
state: merged
author: hoxbro
labels:
  - configuration
assignees: []
merged: true
base: main
head: enh_f401
created_at: 2024-10-02T10:59:33Z
updated_at: 2024-10-03T19:59:12Z
url: https://github.com/astral-sh/ruff/pull/13601
synced_at: 2026-01-10T20:59:36Z
```

# Add `allow-unused-imports` setting for `unused-import` rule (`F401`)

---

_Pull request opened by @hoxbro on 2024-10-02 10:59_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Resolves https://github.com/astral-sh/ruff/issues/9962 by allowing a configuration setting `allowed-unused-imports`

TODO:
- [x] Figure out the correct name and place for the setting; currently, I have added it top level.
- [x] The comparison is pretty naive. I tried using `glob::Pattern` but couldn't get it to work in the configuration.
- [x] Add tests
- [x] Update documentations

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
`cargo test`

---

_Comment by @github-actions[bot] on 2024-10-02 14:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @hoxbro on 2024-10-02 14:53_

Maybe a more descriptive name could be `allowed-unused-imports`.

---

_Renamed from "Add allowed-imports for F401" to "Add allowed-unused-imports for F401" by @hoxbro on 2024-10-03 09:03_

---

_Marked ready for review by @hoxbro on 2024-10-03 10:10_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-03 14:36_

---

_Comment by @charliermarsh on 2024-10-03 14:36_

I can take this one.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-10-03 14:36_

---

_Label `configuration` added by @charliermarsh on 2024-10-03 14:36_

---

_@charliermarsh approved on 2024-10-03 18:50_

---

_Renamed from "Add allowed-unused-imports for F401" to "Add `ignore-unused-imports` setting for `unused-import` rule (`F401`)" by @charliermarsh on 2024-10-03 18:50_

---

_Comment by @charliermarsh on 2024-10-03 18:50_

I moved it onto the `[pyflakes]` section. Thanks!

---

_Renamed from "Add `ignore-unused-imports` setting for `unused-import` rule (`F401`)" to "Add `allow-unused-imports` setting for `unused-import` rule (`F401`)" by @charliermarsh on 2024-10-03 19:40_

---

_Merged by @charliermarsh on 2024-10-03 19:44_

---

_Closed by @charliermarsh on 2024-10-03 19:44_

---

_Branch deleted on 2024-10-03 19:59_

---
