---
number: 11820
title: "formatter deviation: line break not occurring on assignments where LHS goes over line-length"
type: issue
state: open
author: jonathan-hill-visasq
labels:
  - bug
  - formatter
  - style
assignees: []
created_at: 2024-06-10T05:33:15Z
updated_at: 2024-06-10T08:24:39Z
url: https://github.com/astral-sh/ruff/issues/11820
synced_at: 2026-01-07T13:12:15-06:00
---

# formatter deviation: line break not occurring on assignments where LHS goes over line-length

---

_Issue opened by @jonathan-hill-visasq on 2024-06-10 05:33_

I'm seeing a case where black is expanding an assignment but ruff is not - even re-writing the black formatted result.

To keep it concise, this example uses an ultra-short line-length of 14:

Input
```
long_variable_name = 1234
```

Black:
```
long_variable_name = (
    1234
)
```

Ruff:
```
long_variable_name = 1234

```

This seems to occur only when the name of the variable assigned to, is longer than line-length (so expanding the line won't bring it within the allowed length).

I can't find this in the documented deviations or other issues at present, and this doesn't seem to be a duplicate of another issue. 

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Label `formatter` added by @MichaReiser on 2024-06-10 08:18_

---

_Comment by @MichaReiser on 2024-06-10 08:18_

I think this is related to https://github.com/astral-sh/ruff/pull/8940

---

_Label `bug` added by @MichaReiser on 2024-06-10 08:21_

---

_Comment by @MichaReiser on 2024-06-10 08:23_

Yes, I can confirm that #8940 would fix the deviation. The problem is, that the PR has a few regressions around call expression formatting, which is why the PR was abandoned. So fixing this requires some more work and is also something that I don't think we can fix outside of a new formatter style guide as it would change how already formatted code gets formatted.

But we should have a second look at this when we work on the next formatter style guide to see if we can match Black more closely. 

The main issue here is that Ruff doesn't parenthesize the expression if the `(` doesn't fit inside the line length, whereas Black only avoids the parentheses if the content doesn't fit into the line length.

---

_Label `style` added by @MichaReiser on 2024-06-10 08:24_

---

_Referenced in [astral-sh/ruff#13371](../../astral-sh/ruff/issues/13371.md) on 2024-09-16 16:19_

---

_Referenced in [astral-sh/ruff#14025](../../astral-sh/ruff/issues/14025.md) on 2024-10-31 20:42_

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-09-19 13:37_

---

_Referenced in [astral-sh/ruff#20794](../../astral-sh/ruff/pulls/20794.md) on 2025-10-13 18:11_

---
