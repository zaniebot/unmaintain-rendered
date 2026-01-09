---
number: 12851
title: "[Lint Request] Lint against `assertTrue`/`assertFalse` instead of more specific asserts"
type: issue
state: closed
author: alanhdu
labels: []
assignees: []
created_at: 2024-08-12T19:55:35Z
updated_at: 2024-08-13T08:09:22Z
url: https://github.com/astral-sh/ruff/issues/12851
synced_at: 2026-01-07T13:12:15-06:00
---

# [Lint Request] Lint against `assertTrue`/`assertFalse` instead of more specific asserts

---

_Issue opened by @alanhdu on 2024-08-12 19:55_

The default `unittest.TestCase` uses a set of specialized `assert*` functions to give better error messages (unlike `pytest` which does some metaprogramming to work with "bare" `assert` statements).

Would it be possible to have a lint that auto-detects (and corrects) using the less general `assertTrue/assertFalse` instead of the more specialized asserts (since those will have subpar error message)? For instance, a lint that would autofix
`self.assertTrue(isinstance(x, int))` to `self.assertIsInstance(x, int)`?

I think the actual implementation should be straightforward, but one subtlety is that this would be incompatible with the existing 

This would be incompatible with PT009 (which encourages "bare asserts"), but presumably we can just document that users should only use one or another?

I assume this is already implemented somewhere in some other linter, but I actually haven't seen one before -- in our codebase, I just have a set of regexes to apply (which isn't great, but so far hasn't led to too many false positives either).

---

_Comment by @Avasam on 2024-08-13 04:53_

> This would be incompatible with PT009 (which encourages "bare asserts"), but presumably we can just document that users should only use one or another?

That would be expected given you probably shouldn't be mixing unittest and pytest together ^^

---

_Comment by @dhruvmanila on 2024-08-13 08:09_

This should be addressed by https://github.com/astral-sh/ruff/issues/2417 which has rules for your specific use-case (https://github.com/jparise/flake8-assertive). I'll merge this issue into it.

---

_Closed by @dhruvmanila on 2024-08-13 08:09_

---
