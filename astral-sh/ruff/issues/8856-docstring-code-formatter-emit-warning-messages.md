```yaml
number: 8856
title: "docstring code formatter: emit warning messages when code snippets fail to format"
type: issue
state: open
author: BurntSushi
labels:
  - docstring
  - formatter
  - wish
assignees: []
created_at: 2023-11-27T16:05:36Z
updated_at: 2023-12-23T12:39:11Z
url: https://github.com/astral-sh/ruff/issues/8856
synced_at: 2026-01-12T15:54:48Z
```

# docstring code formatter: emit warning messages when code snippets fail to format

---

_@BurntSushi_

#8811 added initial support for formatting doctest code snippets in docstrings, but it does have some silent behavior that should probably be louder. For example, if it detects a code snippet that is invalid Python, then it will silently skip the code snippet. Another example is a bit more pathological, but it is possible for the code snippet formatter to generate invalid Python in some case (as a result of nested triple quoted strings in some cases). In this case, we detect it and bail out of reformatting the code snippet. Ideally, we would emit a warning about this too, although this is arguably more of a bug in the current implementation.

This was [brought up in review](https://github.com/astral-sh/ruff/pull/8811#discussion_r1401252822), and it was suggested that perhaps we split the warning/logging infrastructure out from the linter and use it in the formatter.

---

_Label `docstring` added by @BurntSushi on 2023-11-27 16:05_

---

_Label `formatter` added by @BurntSushi on 2023-11-27 16:05_

---

_Comment by @MichaReiser on 2023-11-27 23:28_

> This was https://github.com/astral-sh/ruff/pull/8811#discussion_r1401252822, and it was suggested that perhaps we split the warning/logging infrastructure out from the linter and use it in the formatter.

I'm a bit hesitant of adopting the linter's `warn` macros because they assume a CLI client and either mess with `stderr` if you would call ruff through a python or rust API or are silently ignored in the LSP. 

Ideally we would return diagnostics as part of the formatted result and it's the caller's responsibility to present them somehow to the user. 

---

_Comment by @charliermarsh on 2023-11-28 06:15_

If we don't think we're going to add diagnostics to the formatter API in the near future, though, warning is not unreasonable IMO.

---

_Added to milestone `Formatter: Stable` by @BurntSushi on 2023-11-29 13:21_

---

_Comment by @MichaReiser on 2023-12-22 06:08_

@BurntSushi are you planning to work on this for the stable release? If not, then I recommend removing it from Stable and mark it as wish (I agree we should do it, but I think we could ship the stable without it)

---

_Removed from milestone `Formatter: Stable` by @BurntSushi on 2023-12-23 12:39_

---

_Label `wish` added by @BurntSushi on 2023-12-23 12:39_

---
