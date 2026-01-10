```yaml
number: 6584
title: "Ruff reports F704 `await` statement outside of a function for notebooks"
type: issue
state: closed
author: david-waterworth
labels:
  - bug
assignees: []
created_at: 2023-08-15T04:06:57Z
updated_at: 2023-08-16T03:59:06Z
url: https://github.com/astral-sh/ruff/issues/6584
synced_at: 2026-01-10T11:09:48Z
```

# Ruff reports F704 `await` statement outside of a function for notebooks

---

_Issue opened by @david-waterworth on 2023-08-15 04:06_

In notebooks `await` is allowed in a cell (i.e. outside of a function), but `ruff` reports F704 - see also https://github.com/microsoft/pylance-release/issues/1754 and https://ipython.readthedocs.io/en/stable/interactive/autoawait.html 

---

_Label `bug` added by @charliermarsh on 2023-08-15 04:16_

---

_Comment by @charliermarsh on 2023-08-15 04:17_

Wow, interesting, TIL. Thanks for filing.

---

_Comment by @dhruvmanila on 2023-08-16 03:46_

Interesting! It also seems that this can be disabled using `%autoawait False` or using `c.InteractiveShell.autoawait` in the config file.

I'm not sure what the long term solution would be but one idea is to store these as a context while processing the Notebook and then using that in some kind of a middleware to filter out such diagnostics. Or, instead of middleware this context can be passed around to the checkers. For example, in this example the context would give us information if `autoawait` is disabled or not. This will still be limited as it can be disabled in the config file.

---

_Closed by @charliermarsh on 2023-08-16 03:59_

---
