```yaml
number: 21855
title: "Clash between `FURB120` and `B905`"
type: issue
state: open
author: jamesbraza
labels:
  - question
assignees: []
created_at: 2025-12-09T00:10:06Z
updated_at: 2025-12-09T08:06:00Z
url: https://github.com/astral-sh/ruff/issues/21855
synced_at: 2026-01-12T15:54:58Z
```

# Clash between `FURB120` and `B905`

---

_@jamesbraza_

### Summary

https://github.com/python/typeshed/pull/14824 added knowledge of `strict=False` default for `zip`. This now exposes a clash between [`refurb`'s `FURB120`](https://github.com/dosisod/refurb/blob/master/docs/checks.md#furb120-use-implicit-default) and [`ruff`'s `B905`](https://docs.astral.sh/ruff/rules/zip-without-explicit-strict/):

- `FURB120`: wants to remove `strict=False`, as it's the default argument
- `B905`: wants to keep `strict=False`, to be explicit

Here's a minimal reproducer:

```python
zip([1], [2], strict=False)
```

For posterity, I have `refurb==2.2.0` and `mypy==0.19.0` (which pulls in https://github.com/python/typeshed/pull/14824).

Perhaps should `B905` not flag `strict=False` if `external = ["FURB"]` is configured? This feels hacky but it's an idea.

### Version

ruff 0.14.8 (9d4f1c6ae 2025-12-04)

---

_Comment by @ntBre on 2025-12-09 00:35_

Hmm, that's interesting. I'm curious what others think and how we've handled similar conflicts in the past. My first thought is that we can't really avoid all possible conflicts with other linters, and you may just want to pick one of these two rules to enable.

This probably also means we shouldn't implement FURB120 ourselves, even though it's still listed in https://github.com/astral-sh/ruff/issues/1348.

---

_Label `question` added by @ntBre on 2025-12-09 00:35_

---

_Comment by @MichaReiser on 2025-12-09 08:06_

If those were both Ruff rules, I'd change `FURB120` to not flag `strict=False`. I don't think we want to make any assumption about other linter and the solution here is to manually disable `B905` (or `FURB120`, depending on what is more importnt to you)

---
