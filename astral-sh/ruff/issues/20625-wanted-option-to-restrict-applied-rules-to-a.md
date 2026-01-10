```yaml
number: 20625
title: "Wanted: Option to restrict applied rules to a subset"
type: issue
state: closed
author: smurfix
labels:
  - question
assignees: []
created_at: 2025-09-29T08:09:46Z
updated_at: 2025-10-12T06:38:20Z
url: https://github.com/astral-sh/ruff/issues/20625
synced_at: 2026-01-10T11:09:59Z
```

# Wanted: Option to restrict applied rules to a subset

---

_Issue opened by @smurfix on 2025-09-29 08:09_

Hi,

my usecase: I have a `pyproject.toml` with a rather large set of ignored rules (legacy codebase and all that).
I now want to examine and/or fix all S* rules which are **not** excluded.

However, `--select S` applies *all* those rules, ignoring my presets. Manually building a command line that excludes everything from pyproject.toml is tedious and error-prone.

IMHO a good way of doing this would be a `--limit-to RULES` option …?

---

_Label `question` added by @MichaReiser on 2025-09-29 08:25_

---

_Comment by @MichaReiser on 2025-09-29 08:25_

See my comment on your other issue. You can use `--fixable` and `--unfixable` to limit which rules should be fixed.

You can use `--extend-select` to enable **additional** rules`. You can use `--ignore` to disable additional rules

---

_Closed by @MichaReiser on 2025-10-12 06:38_

---
