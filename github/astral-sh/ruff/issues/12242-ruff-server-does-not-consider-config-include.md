---
number: 12242
title: "`ruff server` does not consider config `include`"
type: issue
state: closed
author: christianbrugger
labels:
  - bug
  - server
assignees: []
created_at: 2024-07-08T10:40:26Z
updated_at: 2024-07-10T04:12:59Z
url: https://github.com/astral-sh/ruff/issues/12242
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff server` does not consider config `include`

---

_Issue opened by @christianbrugger on 2024-07-08 10:40_

I want to use `ruff server` on a project where I want to introduce formatting and linting gradually. 

My idea was to use the `include` feature in the `ruff.toml` config:
```toml

# only lint & format opt-in files
include = [
    'src/plots.py',
    'src/reports.py',
]
```
This perfectly works when using `ruff format .` and `ruff check .`  from the CLI, meaning that only the two files listed are formatted and linted, nothing else.

However when using the `ruff server` all files are formatted and linted. This is especially problematic when using the format on save or auto-fix on save feature via an IDE. Also way to many diagnostics from the linting are shown.

I looked at the [formatting handler source code](https://github.com/astral-sh/ruff/blob/64855c5f06f0ff796eb54b8db57017f0e11017f9/crates/ruff_server/src/server/api/requests/format.rs#L86-L98) and can see that the `exclude` list is already parsed from the config. However the `include` property is not considered at all. The same is true for [fixes](https://github.com/astral-sh/ruff/blob/64855c5f06f0ff796eb54b8db57017f0e11017f9/crates/ruff_server/src/fix.rs#L36-L49) and [linting](https://github.com/astral-sh/ruff/blob/64855c5f06f0ff796eb54b8db57017f0e11017f9/crates/ruff_server/src/lint.rs#L73-L88).

I would really appreciate, if the `include` mechanism could be added to the `ruff server` as well. Meaning that the server ignores format and fix requests, and returns no diagnostics for files not in the `include` list. In the same way it works for the CLI tool.


---

_Label `bug` added by @MichaReiser on 2024-07-08 11:12_

---

_Label `server` added by @MichaReiser on 2024-07-08 11:12_

---

_Comment by @dhruvmanila on 2024-07-08 14:11_

Looking into this...

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-07-08 14:11_

---

_Comment by @dhruvmanila on 2024-07-08 14:29_

Your assessment is correct. The new server isn't considering anything from the file-resolver settings apart from `exclude` and `extend-exclude`.

---

_Comment by @dhruvmanila on 2024-07-08 14:56_

I think the simplest solution here is to use the logic of the various `FileResolverSettings` information from `python_files_in_path` function and incorporate it into the linting and formatting entry point for the `ruff server`.

But, a long term goal would to have some kind of abstraction which can resolve the path based on the given settings.

---

_Referenced in [astral-sh/ruff#12252](../../astral-sh/ruff/pulls/12252.md) on 2024-07-09 06:15_

---

_Referenced in [astral-sh/ruff#12254](../../astral-sh/ruff/issues/12254.md) on 2024-07-09 07:39_

---

_Added to milestone `Ruff Server: Stable` by @dhruvmanila on 2024-07-09 07:41_

---

_Closed by @dhruvmanila on 2024-07-10 04:12_

---

_Closed by @dhruvmanila on 2024-07-10 04:12_

---
