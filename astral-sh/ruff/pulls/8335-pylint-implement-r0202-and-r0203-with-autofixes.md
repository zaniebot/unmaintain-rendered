```yaml
number: 8335
title: "[pylint] - implement R0202 and R0203 with autofixes"
type: pull_request
state: merged
author: diceroll123
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: autofix-R0202-and-R0203
created_at: 2023-10-30T02:54:03Z
updated_at: 2023-11-30T22:18:15Z
url: https://github.com/astral-sh/ruff/pull/8335
synced_at: 2026-01-12T15:55:26Z
```

# [pylint] - implement R0202 and R0203 with autofixes

---

_@diceroll123_

## Summary

Implements [`no-classmethod-decorator`/`R0202`](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/no-classmethod-decorator.html) and [`no-staticmethod-decorator`/`R0203`](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/no-staticmethod-decorator.html) with autofixes.

They're similar enough that all code is reusable for both.

See: #970 

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2023-10-30 04:33_

## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@diceroll123 reviewed on 2023-10-30 05:55_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pylint/rules/no_method_decorator.rs`:173 on 2023-10-30 05:55_

I don't really know what range we should highlight with the diagnostic, please advise!

---

_Review comment by @danielhollas on `crates/ruff_linter/src/rules/pylint/rules/no_method_decorator.rs`:12 on 2023-10-30 10:10_

It would be nice to add some examples here. Personally I don't even know how you define a classmethod without a decorator.  😊

(just a random observer here who's interested in new Pylint rules, thanks for working on this!)

---

_@danielhollas reviewed on 2023-10-30 10:11_

---

_@diceroll123 reviewed on 2023-10-30 15:20_

---

_Review comment by @diceroll123 on `crates/ruff_linter/src/rules/pylint/rules/no_method_decorator.rs`:12 on 2023-10-30 15:20_

Ah, my mistake!

---

_Comment by @diceroll123 on 2023-10-30 16:27_

I've also had the idea that this could very easily work for `property`, but that doesn't seem to be a pylint rule. 🤔 

And making it a new RUF rule would mean moving some code around to make this reusable between rules, all ears for ideas there.

---

_Comment by @zanieb on 2023-11-02 01:55_

Thanks for the contribution!

I wonder if people are actually writing class/staticmethods like this still?

Would you mind updating with `main` so we get preview ecosystem checks?

---

_Comment by @github-actions[bot] on 2023-11-24 20:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@zanieb approved on 2023-11-30 22:18_

Thanks!

---

_Merged by @zanieb on 2023-11-30 22:18_

---

_Closed by @zanieb on 2023-11-30 22:18_

---

_Label `rule` added by @zanieb on 2023-11-30 22:18_

---

_Label `preview` added by @zanieb on 2023-11-30 22:18_

---
