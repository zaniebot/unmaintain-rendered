```yaml
number: 7092
title: "Cache comment lookups in `suite.rs`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: charlie/comments
created_at: 2023-09-03T16:49:45Z
updated_at: 2023-09-04T08:45:16Z
url: https://github.com/astral-sh/ruff/pull/7092
synced_at: 2026-01-12T02:45:38Z
```

# Cache comment lookups in `suite.rs`

---

_Pull request opened by @charliermarsh on 2023-09-03 16:49_

## Summary

This was feedback from a prior PR that I forgot to act on.

## Test Plan

`cargo test`


---

_Label `formatter` added by @charliermarsh on 2023-09-03 16:49_

---

_Label `internal` added by @charliermarsh on 2023-09-03 16:49_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-03 16:49_

---

_@charliermarsh reviewed on 2023-09-03 16:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/mod.rs`:504 on 2023-09-03 16:50_

This is entirely optional but I do think it's nice to have the same API here as with `comments. has_trailing_own_line(node)`.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:155 on 2023-09-04 06:23_

I preferred testing for preceding first because it better fits the preceding before following mental model
```suggestion
            if (is_class_or_function_definition(preceding)
                    && !preceding_comments.has_trailing_own_line())
                || is_class_or_function_definition(following)
```

---

_@MichaReiser approved on 2023-09-04 06:23_

---

_Merged by @charliermarsh on 2023-09-04 08:45_

---

_Closed by @charliermarsh on 2023-09-04 08:45_

---

_Branch deleted on 2023-09-04 08:45_

---
