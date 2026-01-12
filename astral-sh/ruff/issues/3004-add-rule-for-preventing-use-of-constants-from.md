```yaml
number: 3004
title: "Add rule for preventing use of constants from `site` module"
type: issue
state: open
author: zunda-arrow
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-02-18T07:24:40Z
updated_at: 2023-07-10T01:31:36Z
url: https://github.com/astral-sh/ruff/issues/3004
synced_at: 2026-01-12T15:54:43Z
```

# Add rule for preventing use of constants from `site` module

---

_@zunda-arrow_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

According to the [python docs](https://docs.python.org/3/library/constants.html#constants-added-by-the-site-module) 

> The [site](https://docs.python.org/3/library/site.html#module-site) module (which is imported automatically during startup, except if the [-S](https://docs.python.org/3/using/cmdline.html#cmdoption-S) command-line option is given) adds several constants to the built-in namespace. They are useful for the interactive interpreter shell and should not be used in programs.

These constants are `exit`, `quit`, `copyright`, `credits`, and `license`. Currently, only `exit` and `quit` have a rule preventing their use (PLR1722). A rule preventing these constants from being used may be useful. As these are pretty rarely touched anyway I completely get not wanting to add a rule for this though.


---

_Comment by @JonathanPlasse on 2023-02-18 07:54_

`quit` is also included in `PLR1722`.

---

_Comment by @zunda-arrow on 2023-02-18 07:57_

Noted 👍

---

_Label `rule` added by @charliermarsh on 2023-02-19 12:18_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:31_

---
