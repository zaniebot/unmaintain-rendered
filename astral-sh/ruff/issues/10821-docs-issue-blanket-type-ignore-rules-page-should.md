```yaml
number: 10821
title: "Docs issue: `blanket-type-ignore` rules page should link to a reference of possible codes"
type: issue
state: open
author: aaronsteers
labels:
  - documentation
assignees: []
created_at: 2024-04-07T22:07:23Z
updated_at: 2024-04-29T05:40:24Z
url: https://github.com/astral-sh/ruff/issues/10821
synced_at: 2026-01-10T11:09:53Z
```

# Docs issue: `blanket-type-ignore` rules page should link to a reference of possible codes

---

_Issue opened by @aaronsteers on 2024-04-07 22:07_

On the docs page, https://docs.astral.sh/ruff/rules/blanket-type-ignore/, it would be extremely helpful to link to a reference of possible ignore-type codes. Otherwise, the rule is not actionable as-is.

I would be happy to contribute back to the docs, if anyone else knows of a reference page for those mypy-specific type codes.


---

_Label `documentation` added by @MichaReiser on 2024-04-08 06:37_

---

_Comment by @bendoerry on 2024-04-08 08:17_

I think for this some sort of detection/configuration of which type checker is being used would be useful, since most type checkers have their own error codes.

I don't really want to be suggesting mypy ignore codes when we're using pyright and vice versa

---

_Comment by @Avasam on 2024-04-29 05:40_

@bendoerry FWIW pyright ignores whatever is in between `[]` brackets completely with `type: ignore` and will always blanket ignore. You need to use `pyright: ignore[]` to ignore specific errors.

---
