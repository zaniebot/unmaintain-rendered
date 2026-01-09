---
number: 21945
title: "Detect duplicates in `__all__`"
type: issue
state: open
author: chirizxc
labels:
  - rule
assignees: []
created_at: 2025-12-12T13:05:57Z
updated_at: 2025-12-16T16:24:20Z
url: https://github.com/astral-sh/ruff/issues/21945
synced_at: 2026-01-07T13:12:16-06:00
---

# Detect duplicates in `__all__`

---

_Issue opened by @chirizxc on 2025-12-12 13:05_

### Summary

[Playground*](https://play.ruff.rs/1aa72d45-0c70-4a77-be1f-a782c4f4c8dd)
```python
# ruff: noqa: D100, D101, CPY001
__all__ = (
    "DialogProtocol",
    "DialogProtocol",
)


class DialogProtocol: ...

```

<img width="1143" height="721" alt="Image" src="https://github.com/user-attachments/assets/e32825ab-731c-47ed-ab5b-cf3fbaa44530" />

Neither Ruff nor PyCharm told me anything about this, only [CodeRabbit was able to detect it](https://github.com/Tishka17/aiogram_dialog/pull/504#discussion_r2603382353).

I think it would be possible to add such a rule, especially since it is not so difficult to implement.

---

_Label `rule` added by @MichaReiser on 2025-12-12 13:39_

---

_Comment by @leandrobbraga on 2025-12-14 21:21_

May I be assigned to this issue? I'm new to the codebase, so my first implementation might be a bit rough and I may need some maintainer help to iron it out.

For now my plan is to create the `RUF067` rule named `DuplicateEntryInDunderAll`.

---

_Comment by @chirizxc on 2025-12-14 22:52_

> May I be assigned to this issue? I'm new to the codebase, so my first implementation might be a bit rough and I may need some maintainer help to iron it out.
> 
> For now my plan is to create the `RUF067` rule named `DuplicateEntryInDunderAll`.

https://github.com/astral-sh/ruff/pull/21079

It seems to be already taken, the next available code is `RUF069`


---

_Assigned to @leandrobbraga by @ntBre on 2025-12-15 14:05_

---

_Comment by @leandrobbraga on 2025-12-16 16:02_

OK, I'll use `RUF069`.

I have a question: is removing the duplicate entry a safe fix?

---

_Comment by @ntBre on 2025-12-16 16:10_

I always have to study our [fix safety docs](https://docs.astral.sh/ruff/linter/#fix-safety) again to be sure, but based on the main two criteria (safe fixes don't change runtime behavior or remove comments), I think it could be a safe fix.

One thing to watch out for would be any comments in the range of the edit that might get removed, though. We use this pattern in a lot of rules:

https://github.com/astral-sh/ruff/blob/0f373603ebd007196af4f211e14d421024a6b947/crates/ruff_linter/src/rules/flake8_type_checking/rules/type_alias_quotes.rs#L320-L324

---

_Comment by @leandrobbraga on 2025-12-16 16:14_

Thank you for your input, @ntBre, it was really helpful.

I wasn't aware of [fix safety docs](https://docs.astral.sh/ruff/linter/#fix-safety) nor the comment in the range pattern.

---

_Referenced in [astral-sh/ruff#22114](../../astral-sh/ruff/pulls/22114.md) on 2025-12-20 15:36_

---
