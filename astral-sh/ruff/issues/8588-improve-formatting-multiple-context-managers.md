```yaml
number: 8588
title: Improve formatting multiple context managers
type: issue
state: closed
author: smurfix
labels:
  - formatter
  - preview
assignees: []
created_at: 2023-11-09T18:29:37Z
updated_at: 2023-11-27T11:25:41Z
url: https://github.com/astral-sh/ruff/issues/8588
synced_at: 2026-01-10T11:09:50Z
```

# Improve formatting multiple context managers

---

_Issue opened by @smurfix on 2023-11-09 18:29_

`ruff --format` currently reformats this code:
```
async with \
        Dispatch(obj.cfg, run=True, sig=True) as dsp, \
        dsp.cfg_at(*cfg["path"], *cfg_path) as cf, \
        dsp.sub_at(*cfg["path"], *fs_path) as fs:
```
as
```
async with Dispatch(obj.cfg, run=True, sig=True) as dsp, dsp.cfg_at(
        *cfg["path"],
        *cfg_path,
    ) as cf, dsp.sub_at(*cfg["path"], *fs_path) as fs:
```

This is … really unhelpful. It would be a lot nicer to add parentheses instead:

```
async with (
    Dispatch(obj.cfg, run=True, sig=True) as dsp,
    dsp.cfg_at(*cfg["path"], *cfg_path) as cf,
    dsp.sub_at(*cfg["path"], *fs_path) as fs,
):
```

which I currently need to do manually.

This syntax refinement was added in Py3.10.

---

_Renamed from "Prettify formatting multiple context managers" to "Improve formatting multiple context managers" by @smurfix on 2023-11-09 18:29_

---

_Label `formatter` added by @charliermarsh on 2023-11-09 19:07_

---

_Label `preview` added by @charliermarsh on 2023-11-09 19:07_

---

_Comment by @charliermarsh on 2023-11-09 19:08_

Parenthesized context managers aren't supported on older versions of Python, but we will enforce this as part of implementing Black's preview style.

---

_Comment by @MichaReiser on 2023-11-27 11:25_

I'll close this because it is tracked in #8678 (see wrap_multiple_context_managers_in_parens)

---

_Closed by @MichaReiser on 2023-11-27 11:25_

---
