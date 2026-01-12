```yaml
number: 13864
title: "D204: enabled or disabled by default for Google convention?"
type: issue
state: closed
author: void-rooster
labels:
  - documentation
  - docstring
assignees: []
created_at: 2024-10-21T19:00:23Z
updated_at: 2024-10-22T14:51:59Z
url: https://github.com/astral-sh/ruff/issues/13864
synced_at: 2026-01-12T15:54:53Z
```

# D204: enabled or disabled by default for Google convention?

---

_@void-rooster_

The docs have conflicting information about the default behavior for D204 in the Google convention. The rule page says D204 is enabled by default:

https://github.com/astral-sh/ruff/blob/fa7626160bd89ff76ac877ee9c71e15c5bad3ff8/crates/ruff_linter/src/rules/pydocstyle/rules/blank_before_after_class.rs#L68

while the answer to the FAQ says D204 is disabled by default:

https://github.com/astral-sh/ruff/blob/fa7626160bd89ff76ac877ee9c71e15c5bad3ff8/docs/faq.md?plain=1#L532

---

_Renamed from "D204: enabled or disabled for Google convention?" to "D204: enabled or disabled by default for Google convention?" by @void-rooster on 2024-10-21 19:00_

---

_Comment by @MichaReiser on 2024-10-21 19:45_

The FAQ is correct and the rule documentation is the exact opposite. 

Source of truth:

https://github.com/astral-sh/ruff/blob/83b1c48a935f17869b9ee618faadc99213f83d95/crates/ruff_linter/src/rules/pydocstyle/settings.rs#L30-L43

---

_Label `documentation` added by @MichaReiser on 2024-10-21 19:45_

---

_Label `docstring` added by @AlexWaygood on 2024-10-21 21:47_

---

_Closed by @MichaReiser on 2024-10-22 14:51_

---
