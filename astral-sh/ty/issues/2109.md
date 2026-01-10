```yaml
number: 2109
title: "Configuration error: `possibly-unbound` is not a recognized rule name"
type: issue
state: closed
author: The-Red-Dragons
labels:
  - documentation
  - needs-info
assignees: []
created_at: 2025-12-19T11:14:11Z
updated_at: 2025-12-19T12:02:12Z
url: https://github.com/astral-sh/ty/issues/2109
synced_at: 2026-01-10T01:54:00Z
```

# Configuration error: `possibly-unbound` is not a recognized rule name

---

_Issue opened by @The-Red-Dragons on 2025-12-19 11:14_

### Summary

## Summary

The rule `possibly-unbound` is listed in documentation/examples but ty reports it as unknown.

## Environment

- ty version: 0.0.4

## Reproduction

`pyproject.toml`:
```toml
[tool.ty.rules]
possibly-unbound = "error"
```

## Actual Behavior

```
warning[unknown-rule]: Unknown rule `possibly-unbound`
```

## Expected Behavior

Either:
1. The rule should be recognized, or
2. Documentation should be updated to remove/rename it

## Question

What is the correct rule name for checking possibly unbound variables?

### Version

0.0.4

---

_Comment by @MichaReiser on 2025-12-19 11:17_

Could you say where the documentation lists `possibly-unbound` because we've renamed that rule and I can't find any mention of it in the rules reference documentation.

---

_Label `documentation` added by @MichaReiser on 2025-12-19 11:17_

---

_Label `needs-info` added by @MichaReiser on 2025-12-19 11:17_

---

_Comment by @The-Red-Dragons on 2025-12-19 11:18_

> Could you say where the documentation lists `possibly-unbound` because we've renamed that rule and I can't find any mention of it in the rules reference documentation.

You're right - I apologize for the confusion. 

I likely assumed `possibly-unbound` existed because Pyright/Pylance has a similar rule (`reportPossiblyUnbound`). When configuring ty, I tried adding common type checker rules without checking if they exist in ty specifically.

Could you point me to where I can find the complete list of available ty rules? I searched the documentation but couldn't find a comprehensive rules reference.

Also, what is the equivalent rule name in ty for detecting potentially unbound variables (if it exists)?

---

_Comment by @AlexWaygood on 2025-12-19 11:30_

Our complete list of rules is at https://docs.astral.sh/ty/reference/rules/. The rule you're looking for is https://docs.astral.sh/ty/reference/rules/#possibly-unresolved-reference

---

_Comment by @AlexWaygood on 2025-12-19 11:31_

I wouldn't be opposed to adding a table to the docs mapping mypy/pyright rules to ty rules, in cases where there's a 1:1 mapping. But there often isn't a 1:1 mapping.

---

_Closed by @MichaReiser on 2025-12-19 11:45_

---

_Comment by @AlexWaygood on 2025-12-19 12:02_

> I wouldn't be opposed to adding a table to the docs mapping mypy/pyright rules to ty rules, in cases where there's a 1:1 mapping. But there often isn't a 1:1 mapping.

I opened https://github.com/astral-sh/ty/issues/2111 for this, but I don't think we should do it now because we're still constantly adding, renaming, changing and removing rules at the moment. Maybe when we're closer to stable :-)

---
