```yaml
number: 19591
title: "[`flake8-simplify`] Fix raw string handling in `SIM905` for embedded quotes"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19577
created_at: 2025-07-28T03:14:51Z
updated_at: 2025-08-03T16:34:04Z
url: https://github.com/astral-sh/ruff/pull/19591
synced_at: 2026-01-12T15:56:43Z
```

# [`flake8-simplify`] Fix raw string handling in `SIM905` for embedded quotes

---

_@danparizher_

## Summary

Fixes #19577


---

_Comment by @github-actions[bot] on 2025-07-28 03:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @dylwil3 by @dylwil3 on 2025-07-29 13:06_

---

_Review requested from @ntBre by @MichaReiser on 2025-07-31 17:07_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:125 on 2025-08-01 19:09_

can you combine these conditions to reduce nesting and remove one of the `else` branches?

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:163 on 2025-08-01 19:09_

it would be nice to preserve this comment - maybe in the definition of `element_flags`?

---

_@dylwil3 requested changes on 2025-08-01 19:15_

This looks good, thank you! A couple of nits and then a design point:

I think we should gate this fix behavior under preview, and when not in preview just skip the check in this situation. 

The reason is that it's unclear to me if the best thing to do here is to offer your fix (which would change part of a raw string into a string with escapes), to skip the check here, or to offer the fix with the "ugly" triple quotes for the relevant list items (or some compromise - like offer your fix but make it `Unsafe`). On the other hand, we have to do _something_ to fix the syntax error, and I think this arrangements allows us to consider the other options in the meantime without making a breaking change.

(But I could be overruled on this)

---

_Comment by @tdulcet on 2025-08-02 08:58_

> The reason is that it's unclear to me if the best thing to do here is to offer your fix (which would change part of a raw string into a string with escapes), to skip the check here, or to offer the fix with the "ugly" triple quotes for the relevant list items

At least in the example from #19577, there are no embedded single quote characters, so one should be able to cleanly convert it to a raw single quoted string without needing to add any additional escaping:
```py
test_email_addresses = [
    r"simple@example.com",
    r"very.common@example.com",
    r"FirstName.LastName@EasierReading.org",
    r"x@example.com",
    r"long.email-address-with-hyphens@and.subdomains.example.com",
    r"user.name+tag+sorting@example.com",
    r"name/surname@example.com",
    r"xample@s.example",
    r'" "@example.org',
    r'"john..doe"@example.org',
    r"mailhost!username@example.org",
    r'"very.(),:;<>[]\".VERY.\"very@\\ \"very\".unusual"@strange.example.com',
    r"user%example.com@example.org",
    r"user-@example.org",
    r"I❤️CHOCOLATE@example.com",
    r"this\ still\"not\\allowed@example.com",
    r"stellyamburrr985@example.com",
    r"Abc.123@example.com",
    r"user+mailbox/department=shipping@example.com",
    r"!#$%&'*+-/=?^_`.{|}~@example.com",
    r'"Abc@def"@example.com',
    r'"Fred\ Bloggs"@example.com',
    r'"Joe.\\Blow"@example.com',
]
```

In (rare) cases where a triple quoted string does include both single and double quote characters, I would argue that the keeping the triple quotes for those items would be better than adding escaping, which would make the resulting items very difficult to read, especially if they already include backslash characters.

---

_Comment by @dylwil3 on 2025-08-03 16:21_

That's a good point @tdulcet , thank you! I think at that point it's maybe unobtrusive enough to fix the bug this way without gating behind preview. Let me know if the new snapshots look objectionable - otherwise I'll merge this in shortly

---

_@dylwil3 approved on 2025-08-03 16:24_

Thanks @danparizher !

---

_Label `bug` added by @dylwil3 on 2025-08-03 16:30_

---

_Label `fixes` added by @dylwil3 on 2025-08-03 16:30_

---

_Renamed from "[`flake8_simplify`] Fix raw string handling in `SIM905` for embedded quotes" to "[`flake8-simplify`] Fix raw string handling in `SIM905` for embedded quotes" by @dylwil3 on 2025-08-03 16:30_

---

_Merged by @dylwil3 on 2025-08-03 16:31_

---

_Closed by @dylwil3 on 2025-08-03 16:31_

---

_Branch deleted on 2025-08-03 16:34_

---
