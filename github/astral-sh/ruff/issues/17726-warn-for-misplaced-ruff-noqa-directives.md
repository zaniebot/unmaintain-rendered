---
number: 17726
title: "Warn for misplaced `# ruff: noqa: ` directives"
type: issue
state: open
author: aaron-skydio
labels:
  - suppression
assignees: []
created_at: 2025-04-30T03:22:40Z
updated_at: 2025-04-30T06:25:04Z
url: https://github.com/astral-sh/ruff/issues/17726
synced_at: 2026-01-07T13:12:16-06:00
---

# Warn for misplaced `# ruff: noqa: ` directives

---

_Issue opened by @aaron-skydio on 2025-04-30 03:22_

### Summary

Related to these, but distinct I think:
- https://github.com/astral-sh/ruff/issues/6157
- https://github.com/astral-sh/ruff/issues/8157

Is it possible to have a rule that bans something like this:

```py
def my_function():
    # ruff: noqa: PLR0914
    ...
```

This is a valid noqa which ruff currently respects, but usually isn't what the author intended; they intended to ignore the rule for the function but not the whole file (which afaik is not a thing, I'm not asking to make it a thing, it should be a `# noqa: PLR0914` on the function definition).  Something like "only allow `ruff: noqa: ` near the top of the file" might make sense, but disallowing `ruff: noqa: ` comments that aren't at the start of a line (including whitespace) seems maybe better defined?

---

_Label `suppression` added by @MichaReiser on 2025-04-30 06:25_

---
