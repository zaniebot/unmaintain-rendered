```yaml
number: 11013
title: "Fix incorrect error message when specifying `tool.uv.sources.(package).workspace` with other options"
type: pull_request
state: merged
author: micolous
labels:
  - error messages
assignees: []
merged: true
base: main
head: cannot-specify-workspace
created_at: 2025-01-28T07:42:22Z
updated_at: 2025-01-28T14:25:34Z
url: https://github.com/astral-sh/uv/pull/11013
synced_at: 2026-01-10T11:45:23Z
```

# Fix incorrect error message when specifying `tool.uv.sources.(package).workspace` with other options

---

_Pull request opened by @micolous on 2025-01-28 07:42_

## Summary

When a `pyproject.toml` `[tool.uv.sources.(package)]` section specifies `workspace` and one or more of (`index`, `git`, `url`, `path`, `rev`, `tag`, `branch`, `editable`), running `uv` to build or sync the package gives the error:

```
cannot specify both `index` and `(parameter name)`
```

The error should actually say:

```
cannot specify both `workspace` and `(parameter name)`
```

## Test Plan

I ran `cargo test`, and all tests still passed.



---

_@charliermarsh approved on 2025-01-28 14:25_

Thank you!

---

_Label `error messages` added by @charliermarsh on 2025-01-28 14:25_

---

_Merged by @charliermarsh on 2025-01-28 14:25_

---

_Closed by @charliermarsh on 2025-01-28 14:25_

---
