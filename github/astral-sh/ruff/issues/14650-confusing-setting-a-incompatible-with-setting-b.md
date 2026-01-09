---
number: 14650
title: Confusing setting A incompatible with setting B warning
type: issue
state: closed
author: dimaqq
labels:
  - question
assignees: []
created_at: 2024-11-28T01:05:48Z
updated_at: 2024-12-02T01:50:11Z
url: https://github.com/astral-sh/ruff/issues/14650
synced_at: 2026-01-07T13:12:16-06:00
---

# Confusing setting A incompatible with setting B warning

---

_Issue opened by @dimaqq on 2024-11-28 01:05_

```command
> uvx ruff -v format --preview tests/unit/test_proxy.py
[2024-11-28][09:57:06][ruff::resolve][DEBUG] Using configuration file (via parent) at: /code/python-libjuju/pyproject.toml
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
[2024-11-28][09:57:06][ruff::commands::format][DEBUG] format_path; path=/code/python-libjuju/tests/unit/test_proxy.py
[2024-11-28][09:57:06][ruff::commands::format][DEBUG] Formatted 1 files in 662.75µs
1 file left unchanged
```

The thing is, my repo doesn't mention any of `one-blank-line-before-class`, `no-blank-line-before-class`, D203, D211...

The warnings are gone if I remove the `pyproject.toml` entirely, or if I remove the "D" from `[tool.ruff.lint] select = [...]`

Could it be that selecting D selects all Dnnn flags and there are incompatible flags in that set?
Could this be improved?

---

_Comment by @dimaqq on 2024-11-28 01:06_

Config: https://github.com/juju/python-libjuju/blob/main/pyproject.toml

---

_Comment by @dhruvmanila on 2024-11-28 05:26_

> Could it be that selecting D selects all Dnnn flags and there are incompatible flags in that set?

Yes, that's correct. Including "D" selects the entire category which will include all the rules that falls under that category which includes "Dxxx" rule codes.

---

_Comment by @dimaqq on 2024-11-29 05:07_

I wish ruff would only include the setting that takes effect out of the two.

In the meantime, is there a syntax to "include all D, but exclude specific D123" ?

---

_Comment by @dhruvmanila on 2024-11-29 05:25_

> In the meantime, is there a syntax to "include all D, but exclude specific D123" ?

Yes, you can use the [`lint.ignore`](https://docs.astral.sh/ruff/settings/#lint_ignore) setting for that:

```toml
[tool.ruff.lint]
extend-select = ["D"]
ignore = ["D123"]
```

Additionally, you might be interested in this list: https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules

---

_Label `question` added by @dhruvmanila on 2024-11-29 08:24_

---

_Comment by @Avasam on 2024-11-30 20:55_

There's a few mutually-exclusive conflicting rules (the only ones I can think off are in D, namely D203 vs D211 and D212 vs D213) that would be better off as configs.

---

_Closed by @MichaReiser on 2024-11-30 21:25_

---

_Comment by @dimaqq on 2024-12-01 07:10_

This is not very... user-friendly, I guess.
In other words this weakens that statement that "ruff is a drop-in replacement for flake8, black, etc."

I'm happy to use the work-around, and thank you for the amazing tool!

---
