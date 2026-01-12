```yaml
number: 3417
title: "fix: Emit a more useful error if an `extend` points at a non-existent ruff.toml file."
type: pull_request
state: merged
author: DanCardin
labels: []
assignees: []
merged: true
base: main
head: dc/humanize-ruff-toml-error
created_at: 2023-03-09T13:06:18Z
updated_at: 2023-03-09T19:55:54Z
url: https://github.com/astral-sh/ruff/pull/3417
synced_at: 2026-01-12T15:55:12Z
```

# fix: Emit a more useful error if an `extend` points at a non-existent ruff.toml file.

---

_@DanCardin_

Today some `extend` usage pointing at a ruff.toml (which does not exist):

```
[tool.ruff]
extend = '../ruff.toml'
```

For some ruff invocation, say, `ruff src tests`, you just get `No such file or directory (os error 2)`. When reading this, i initially thought it was telling me that for `ruff src tests`, src or tests didn't exist.

It seems like pyproject.toml handling already has an existing error map which prettifies the error and includes the offending file path, but ruff.toml doesnt benefit from being handled in the same place. I basically just moved that logic up a level in order to ensure it applied to either option.

---
I was originally trying to add (what i think is nicer behavior) to show the whole path. Something to the end result like:

```
error: Following `extends` through pyproject.toml -> ../ruff.toml -> ../ruff.toml, failed to parse: No such file or directory (os error 2)
```

but given the required changes i wasn't sure that it would be accepted, since it's much more involved for a marginal benefit.

---

_Marked ready for review by @DanCardin on 2023-03-09 13:11_

---

_@charliermarsh reviewed on 2023-03-09 19:15_

---

_Review comment by @charliermarsh on `crates/ruff/src/resolver.rs`:131 on 2023-03-09 19:15_

Does `Failed to parse` now appear twice, if it _is_ a `pyproject.toml`?

---

_Review comment by @DanCardin on `crates/ruff/src/resolver.rs`:131 on 2023-03-09 19:50_

Oh definitely yes. I appear to have undone the removal bit from `pyproject.rs`. Updated

---

_@DanCardin reviewed on 2023-03-09 19:50_

---

_Review comment by @charliermarsh on `crates/ruff/src/resolver.rs`:131 on 2023-03-09 19:52_

Thanks :)

---

_@charliermarsh reviewed on 2023-03-09 19:52_

---

_Merged by @charliermarsh on 2023-03-09 19:55_

---

_Closed by @charliermarsh on 2023-03-09 19:55_

---

_Branch deleted on 2023-03-09 19:55_

---
