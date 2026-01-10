```yaml
number: 15455
title: Clarify documentation for A005
type: issue
state: closed
author: DaniBodor
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-01-13T12:41:06Z
updated_at: 2025-01-14T07:42:14Z
url: https://github.com/astral-sh/ruff/issues/15455
synced_at: 2026-01-10T11:09:56Z
```

# Clarify documentation for A005

---

_Issue opened by @DaniBodor on 2025-01-13 12:41_

I got an error for this rule (as the newest version of ruff got installed) and was reading the documentation and it took me quite a while to realize that the issue was the name of the module I was working on. As the error message tracks to line 1 of the file in question, which is an import X as Y statement, I kept thinking that the Y was the problem, but that didn't make sense given that the error message stated something else than Y.

Anyway, I think it would be useful to include an example in the documentation and/or be more explicit that the issue refers to the filename rather than the code.

In general, it would be useful if issues relating to file names, missing inits (INP001), etc (not sure if there are many of these) don't track to line 1 of a given file, but to something else. I guess this is not so easy to implement though.
However, again, for such cases it would be useful if there were a comment in the documentation for such rules that although the issue tracks to line 1 of a given file, that that line is not the actual problem.

---

_Label `documentation` added by @MichaReiser on 2025-01-13 12:42_

---

_Label `help wanted` added by @MichaReiser on 2025-01-13 12:42_

---

_Closed by @MichaReiser on 2025-01-14 07:42_

---
