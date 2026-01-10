```yaml
number: 12615
title: "Rule request: Floating point numbers should not be tested for equality (Sonar - `RSPEC-1244`)"
type: issue
state: open
author: daria-shaw
labels:
  - rule
assignees: []
created_at: 2024-08-01T15:42:22Z
updated_at: 2024-08-01T21:13:17Z
url: https://github.com/astral-sh/ruff/issues/12615
synced_at: 2026-01-10T11:09:54Z
```

# Rule request: Floating point numbers should not be tested for equality (Sonar - `RSPEC-1244`)

---

_Issue opened by @daria-shaw on 2024-08-01 15:42_

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
Keywords: RSPEC-1244, float, sonar

`RSPEC-1244` from [sonar rules](https://rules.sonarsource.com/python/RSPEC-1244/) checks whether equality checks are made on floating point numbers, which often leads to non-deterministic results in tests.

This rule forms part of Sonar's Python scanner checks, which I've seen is [open as a feature request](https://github.com/astral-sh/ruff/issues/4935). 

Would it be possible to add this individual rule either as a one-off or as a starting point in porting sonar's rules over?

---

_Renamed from "Rule request: Floating point numbers should not be tested for equality (from sonar rule RSPEC-1244)" to "Rule request: Floating point numbers should not be tested for equality (Sonar rule `RSPEC-1244`)" by @daria-shaw on 2024-08-01 15:42_

---

_Renamed from "Rule request: Floating point numbers should not be tested for equality (Sonar rule `RSPEC-1244`)" to "Rule request: Floating point numbers should not be tested for equality (Sonar - `RSPEC-1244`)" by @daria-shaw on 2024-08-01 15:42_

---

_Label `rule` added by @MichaReiser on 2024-08-01 21:12_

---

_Comment by @MichaReiser on 2024-08-01 21:13_

Makes sense to me. The one big question is where we should add the rule. 

---
