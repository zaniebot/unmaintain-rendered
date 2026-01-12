```yaml
number: 5742
title: "Stack overflow when analyzing non-`BitOr` separated types"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-07-13T17:21:09Z
updated_at: 2023-07-13T18:23:41Z
url: https://github.com/astral-sh/ruff/issues/5742
synced_at: 2026-01-12T15:54:45Z
```

# Stack overflow when analyzing non-`BitOr` separated types

---

_@charliermarsh_

Run, e.g., `cargo run -p ruff_cli -- check /Users/crmarsh/workspace/PlasmaPy/plasmapy/formulary/braginskii.py -n --verbose --select ANN401` on PlasmaPy at `6594aff352688328597f35608a66462b075548be`.


---

_Label `bug` added by @charliermarsh on 2023-07-13 17:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-13 17:21_

---

_Closed by @charliermarsh on 2023-07-13 18:23_

---
