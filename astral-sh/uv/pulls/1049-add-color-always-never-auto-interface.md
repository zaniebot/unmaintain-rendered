```yaml
number: 1049
title: "Add `--color always|never|auto` interface"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/color-options
created_at: 2024-01-22T22:01:22Z
updated_at: 2024-01-23T05:41:50Z
url: https://github.com/astral-sh/uv/pull/1049
synced_at: 2026-01-10T15:39:03Z
```

# Add `--color always|never|auto` interface

---

_Pull request opened by @zanieb on 2024-01-22 22:01_

Extends #1048 interface providing a more general interface that I think should be standard.

Allows forcing colors to be on _or_ off. e.g. `NO_COLOR=1 pip install pip-tools --color always` would be colored.

Hides the `--no-color` option as it only exists for compatibility (and seems better than throwing an error when people assume it will exist).

Has a nice side-effect of documenting our coloring behaviors e.g.

```
--color <COLOR>
    Control colors in output
    
    [default: auto]

    Possible values:
    - auto:   Enables colored output only when the output is going to a terminal or TTY with support
    - always: Enables colored output regardless of the detected environment
    - never:  Disables colored output
```

---

_Label `enhancement` added by @zanieb on 2024-01-22 22:01_

---

_Review requested from @charliermarsh by @zanieb on 2024-01-22 22:06_

---

_@charliermarsh approved on 2024-01-23 00:04_

---

_Merged by @zanieb on 2024-01-23 05:01_

---

_Closed by @zanieb on 2024-01-23 05:01_

---

_Branch deleted on 2024-01-23 05:01_

---

_Comment by @charliermarsh on 2024-01-23 05:41_

What Would Cargo Do...

---
