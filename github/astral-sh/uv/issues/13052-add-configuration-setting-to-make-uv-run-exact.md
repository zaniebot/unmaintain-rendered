---
number: 13052
title: "Add configuration setting to make `uv run --exact` the default"
type: issue
state: open
author: ptc-swalk
labels:
  - enhancement
assignees: []
created_at: 2025-04-22T11:14:11Z
updated_at: 2025-05-30T02:06:56Z
url: https://github.com/astral-sh/uv/issues/13052
synced_at: 2026-01-07T13:12:18-06:00
---

# Add configuration setting to make `uv run --exact` the default

---

_Issue opened by @ptc-swalk on 2025-04-22 11:14_

### Summary

Currently, `uv sync` is exact by default and `uv run` is inexact by default. `uv run` being inexact by default has led to some "this worked for me when I tried it" confusion here so we'd find it helpful if there was a configuration setting that would make `uv run` exact by default as well.

### Example

For example, a setting
```
[tool.uv]
run-exact = true
```
That would have the effect of passing `--exact` to every invocation of `uv run`.

---

_Label `enhancement` added by @ptc-swalk on 2025-04-22 11:14_

---

_Comment by @anuraaga on 2025-05-30 02:06_

Ran into this too - the difference in behavior between `sync` and `run` is really mysterious and does lead to confusion. I think a common mental model is that "uv run automatically syncs before running", so if the behavior between the two differs, it becomes not the case.

I'm not sure what the backwards compatibility constraints are for the project, but if at all possible, it would be great to

- Change the default for `run` to `--exact` to match `sync`
- Allow adding a setting to use `inexact` by default when desired / to preserve existing behavior

Otherwise being able to at least force exact at the config level would still help.

---
