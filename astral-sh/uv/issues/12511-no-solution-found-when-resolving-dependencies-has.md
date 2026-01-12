```yaml
number: 12511
title: "\"No solution found when resolving dependencies\" has extra requirements that are confusing"
type: issue
state: open
author: notatallshaw
labels:
  - error messages
assignees: []
created_at: 2025-03-27T16:39:21Z
updated_at: 2025-03-28T02:33:50Z
url: https://github.com/astral-sh/uv/issues/12511
synced_at: 2026-01-12T16:01:05Z
```

# "No solution found when resolving dependencies" has extra requirements that are confusing

---

_@notatallshaw_

### Summary

Sometimes uv's "No solution found when resolving dependencies" gives me extra requirements, and that I don't understand where they come from.

For example:

1. `uv venv -p 3.10`
2. `source .venv/bin/activate`
3. `uv pip install --dry-run --only-binary ":all:" "python-nvd3>=0.15.0"`

Output:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of python-nvd3 are available:
          python-nvd3<=0.15.0
          python-nvd3==0.16.0
      and python-nvd3>=0.15.0 has no usable wheels, we can conclude that python-nvd3>=0.15.0 cannot be used.
      And because you require python-nvd3>=0.15.0, we can conclude that your requirements are unsatisfiable.

      hint: Wheels are required for `python-nvd3` because building from source is disabled for all packages (i.e., with `--no-build`)
```

Where did "python-nvd3<=0.15.0" and "python-nvd3==0.16.0" come from? And what are they trying to tell me?

Expected output:

```
  × No solution found when resolving dependencies:
  ╰─▶ python-nvd3>=0.15.0 has no usable wheels, we can conclude that python-nvd3>=0.15.0 cannot be used.
      And because you require python-nvd3>=0.15.0, we can conclude that your requirements are unsatisfiable.

      hint: Wheels are required for `python-nvd3` because building from source is disabled for all packages (i.e., with `--no-build`)
```


### Platform

Linux

### Version

0.6.10

### Python version

Python 3.10

---

_Label `bug` added by @notatallshaw on 2025-03-27 16:39_

---

_Label `bug` removed by @konstin on 2025-03-27 21:45_

---

_Label `error messages` added by @konstin on 2025-03-27 21:45_

---

_Comment by @konstin on 2025-03-27 21:46_

We're failing to collapse the versions here

---

_Comment by @zanieb on 2025-03-28 02:33_

I don't think that's quite true, there are cases where we list available versions to state the proof and those are intentionally not collapsed.

Here, we shouldn't show the available versions at all and short-circuit to another framing, e.g.:

>  Because you require python-nvd3>=0.15.0, and python-nvd3>=0.15.0 has no usable wheels, we can conclude that your requirements are unsatisfiable.

Probably hard.

---
