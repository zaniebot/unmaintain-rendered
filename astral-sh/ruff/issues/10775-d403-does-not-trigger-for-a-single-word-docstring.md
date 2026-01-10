---
number: 10775
title: D403 does not trigger for a single-word docstring that ends with a period
type: issue
state: closed
author: jbmoorhouse
labels:
  - bug
  - docstring
assignees: []
created_at: 2024-04-04T14:41:48Z
updated_at: 2024-04-05T06:42:01Z
url: https://github.com/astral-sh/ruff/issues/10775
synced_at: 2026-01-10T01:22:50Z
---

# D403 does not trigger for a single-word docstring that ends with a period

---

_Issue opened by @jbmoorhouse on 2024-04-04 14:41_

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

# Keywords searched for

`D403` -> Potentially related to https://github.com/astral-sh/ruff/issues/2989

# Ruff version

`0.3.0`

# Issue

For a docstring with one word (admittedly not a typical docstring :wink:), if the word is not capitalised and there is no fullstop, the correct errors are raised

![image](https://github.com/astral-sh/ruff/assets/32223462/ef3031de-f9fc-4eda-8942-15b0120831d0)

However, when adding a full stop, `D403` is suppressed/not raised

![image](https://github.com/astral-sh/ruff/assets/32223462/fd1ff5d7-f14f-42fc-99d3-310d9a0d0c1b)




---

_Label `docstring` added by @AlexWaygood on 2024-04-04 14:47_

---

_Comment by @MichaReiser on 2024-04-04 14:51_

I think this is probably a bug in the check here  

https://github.com/astral-sh/ruff/blob/230c9ce23603515245c55e3c0cfbd23c3f22cbe5/crates/ruff_linter/src/rules/pydocstyle/rules/capitalized.rs#L62-L64

It splits by whitespace to get the first word. But that means it returns early if it's a single word. We should probably change this to 

```
let first_word = body.split(' ').next().unwrap_or(&body);
```

---

_Renamed from "D403 issue" to "D403 does not trigger for a single-word docstring that ends with a period" by @AlexWaygood on 2024-04-04 14:55_

---

_Label `bug` added by @AlexWaygood on 2024-04-04 14:56_

---

_Comment by @MichaReiser on 2024-04-04 14:58_

Okay, that's not sufficient, because it then gets skipped in 

```
    for char in first_word.chars() {
        if !char.is_ascii_alphabetic() && char != '\'' {
            dbg!("Non ascii character");
            return;
        }
    }
```

I need to look into the pycodestyle implementation if this is intentional.

---

_Comment by @MichaReiser on 2024-04-04 15:28_

Okay, I was wrong. Split is used correctly. It always returns the string even if the split character isn't found. It bails because `.` isn't an ASCII character, and this matches pydocstyle's implementation. 

I'll put up a PR but I"m not sure if we should change the current behavior because a single word isn't really a phrase... 

---

_Referenced in [astral-sh/ruff#10776](../../astral-sh/ruff/pulls/10776.md) on 2024-04-04 16:20_

---

_Closed by @MichaReiser on 2024-04-05 06:42_

---

_Referenced in [astral-sh/ruff#13167](../../astral-sh/ruff/issues/13167.md) on 2024-08-30 18:44_

---
