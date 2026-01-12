```yaml
number: 6313
title: "Document how to use `uv run` with a shebang"
type: pull_request
state: closed
author: PaarthShah
labels:
  - documentation
assignees: []
base: main
head: shebang
created_at: 2024-08-21T09:28:55Z
updated_at: 2025-11-02T20:10:15Z
url: https://github.com/astral-sh/uv/pull/6313
synced_at: 2026-01-12T16:07:19Z
```

# Document how to use `uv run` with a shebang

---

_@PaarthShah_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add documentation about how to use a shebang (`#!/usr/bin/env -S uv run`) to run a python script with `uv run` without actually inputting `uv run` directly in the shell.

## Test Plan

Tested locally using `uv` `0.3.0` on Pop_OS! 22.04


---

_Marked ready for review by @PaarthShah on 2024-08-21 09:47_

---

_Renamed from "Document how to use 'uv run' with a shebang" to "Document how to use `uv run` with a shebang" by @PaarthShah on 2024-08-21 09:47_

---

_Assigned to @zanieb by @zanieb on 2024-08-21 12:37_

---

_Label `documentation` added by @zanieb on 2024-08-21 12:37_

---

_Comment by @ngnpope on 2024-08-21 17:24_

Related: https://github.com/astral-sh/ruff/issues/13021#issuecomment-2302585295

---

_@PaarthShah reviewed on 2024-08-21 19:37_

---

_Review comment by @PaarthShah on `docs/guides/scripts.md`:1 on 2024-08-21 19:37_

Please feel very free to nitpick for consistency/readability: I'm not attached to any formatting/phrasing so much as the actual content.

One thing that I wanted to comment on but wasn't sure how to: why the `-S` is needed in the shebang:

from `man env`:
```
       -S, --split-string=S
              process  and split S into separate arguments; used to pass multiple argu‐
              ments on shebang lines
```
Basically, since `uv run` is "multiple arguments" insofar as that `run` is an argument to `uv` (along with any others), the `-S` is needed

---

_@fschulze reviewed on 2024-08-22 07:52_

---

_Review comment by @fschulze on `docs/guides/scripts.md`:1 on 2024-08-22 07:52_

Only saw this here now, according to https://stackoverflow.com/a/16365367/3748142 ``/usr/bin/env`` isn't very portable between different systems.

---

_@PaarthShah reviewed on 2024-08-22 08:16_

---

_Review comment by @PaarthShah on `docs/guides/scripts.md`:1 on 2024-08-22 08:16_

@fschulze That answer is very, very outdated (from 2013, edited 2018). It's far more portable nowadays.

---

_Comment by @zanieb on 2024-08-22 14:14_

Cool. I'm hesitant to merge this until we address https://github.com/astral-sh/uv/issues/6360

---

_Unassigned @zanieb by @zanieb on 2024-09-17 19:44_

---

_Comment by @ilyagr on 2024-12-16 08:14_

This now needs a `--script` at the end of the shebang line, and probably a warning in the docs about how important it is to avoid https://github.com/astral-sh/uv/issues/6360

~~I also wonder whether it will work if the first argument to the script could be confused with a `uv run` option. Not sure if there's any way for shebang and `env` to put `--` after the script. Maybe this would need a special `uv` command or behavior.~~

**Update:** I don't think my worry from the last paragraph is a real problem. I thought `--script` takes an argument, so `--script prog.py` is the same as `--script=prog.py`, but that is not the case, and  `uv help run` says that "Arguments following the command (or script) are not interpreted as arguments to uv."

---

_Review comment by @andrei-korshikov on `docs/guides/scripts.md`:221 on 2025-03-18 09:00_

1. `git+https` dependencies lead to "Resolving dependencies..." message with ~1 sec delay for network requests on every run, and with `--quiet` it looks like a strange pause for no reasons. I.e. I think `--quiet` is opinionated and not good for all cases.
2. We have infinite recursion without `--script` option if a script doesn't have `.py` extension, this should be noted.

---

_@andrei-korshikov reviewed on 2025-03-18 09:00_

---

_Comment by @charliermarsh on 2025-11-02 20:10_

I believe this is now in the docs: https://docs.astral.sh/uv/guides/scripts/#using-a-shebang-to-create-an-executable-file

---

_Closed by @charliermarsh on 2025-11-02 20:10_

---
