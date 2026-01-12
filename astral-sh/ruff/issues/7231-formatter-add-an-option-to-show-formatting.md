```yaml
number: 7231
title: "Formatter: Add an option to show formatting changes when running `--check`"
type: issue
state: closed
author: MichaReiser
labels:
  - cli
  - formatter
assignees: []
created_at: 2023-09-08T07:00:45Z
updated_at: 2023-10-18T11:55:07Z
url: https://github.com/astral-sh/ruff/issues/7231
synced_at: 2026-01-12T15:54:46Z
```

# Formatter: Add an option to show formatting changes when running `--check`

---

_@MichaReiser_

There are two options:

1. Add a new `--diff` option to `ruff format --check` that prints the formatting changes similar to Black
2. `--check` uses Ruff's diagnostic system and shows diffs for `format: human` but not for `format: compact` (which would enable diffs by default)

I prefer option 2. 

Open questions
* Should this be integrated into our diagnostic system? 
* Should it print the entire diff or collapse very large code frames
* Which diffing library to use. We ran into exponential back-tracking cases in our `cargo dev` script for large files / large changes


CC @zanieb regarding CLI design


---

_Label `cli` added by @MichaReiser on 2023-09-08 07:00_

---

_Label `formatter` added by @MichaReiser on 2023-09-08 07:00_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-08 07:01_

---

_Renamed from "Formatter: Add `--diff` option to CLI" to "Formatter: Add `--diff` option to CLI or respect `format` configuration" by @MichaReiser on 2023-09-08 08:55_

---

_Renamed from "Formatter: Add `--diff` option to CLI or respect `format` configuration" to "Formatter: Add an option to show formatting changes when running `--check`" by @MichaReiser on 2023-09-08 08:55_

---

_Comment by @charliermarsh on 2023-09-22 21:17_

I believe we've settled on `ruff format --diff`, which will have the same semantics as `ruff check --diff` (i.e., it exits with status code 1 if there are any diffs).


---

_Label `help wanted` added by @charliermarsh on 2023-09-22 21:17_

---

_Assigned to @konstin by @MichaReiser on 2023-09-29 11:22_

---

_Label `help wanted` removed by @konstin on 2023-10-13 09:32_

---

_Closed by @konstin on 2023-10-18 11:55_

---
