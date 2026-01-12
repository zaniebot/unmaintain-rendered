```yaml
number: 14568
title: option for remapped/removed rules to report an error instead of a warning
type: issue
state: open
author: DetachHead
labels:
  - cli
assignees: []
created_at: 2024-11-25T01:12:08Z
updated_at: 2024-12-29T01:03:09Z
url: https://github.com/astral-sh/ruff/issues/14568
synced_at: 2026-01-12T15:54:54Z
```

# option for remapped/removed rules to report an error instead of a warning

---

_@DetachHead_

#14435 made it so that these are now reported as a warning instead of an error. however in my project i like having all checks as strict as possible, and would like my CI to fail if any outdated rules are present in my config. it would be nice if there was an option to enable the previous more strict behavior

---

_Renamed from "option for remapped/removed rules to report and error instead of a warning" to "option for remapped/removed rules to report an error instead of a warning" by @DetachHead on 2024-11-25 01:20_

---

_Label `cli` added by @MichaReiser on 2024-11-25 07:55_

---

_Comment by @MichaReiser on 2024-11-25 07:59_

A specific option just for remapped/removed rules seems too narrow to me. There are other configuration warnings (e.g. incompatible rules) that you may want to error on, to be as "strict as possible"

The way I envision this in Red Knot is that:

* There's a new warning severity
* Warnings in configurations are emitted as regular "diagnostics" except that they point to the configuration file rather than a python file
* There's a CLI flag (I don't remember the exact name) that allows promoting all warnings to errors OR specifying the maximum number of allowed warnings

This has the advantage that it is a generic concept and allows for workflows where warnings are accepted locally (e.g. because a possibly undefined variable isn't something that needs fixing right away) but fail in CI.

Getting there for Ruff will require refactoring our diagnostic system first and introducing a warning severity (not necessarily for rules, but the diagnostic system must support it)



---

_Comment by @DetachHead on 2024-11-25 08:25_

sounds great, looking forward to red knot!

---

_Comment by @Avasam on 2024-12-29 01:03_

> * There's a new warning severity

Referencing https://github.com/astral-sh/ruff/issues/1256, that's something I've been wanting since the start ^^

---
