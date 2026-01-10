```yaml
number: 6984
title: "Concerning `assert (S101)` example and the change of `>`"
type: issue
state: closed
author: Anselmoo
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2023-08-29T16:53:26Z
updated_at: 2023-08-29T21:08:09Z
url: https://github.com/astral-sh/ruff/issues/6984
synced_at: 2026-01-10T11:09:49Z
```

# Concerning `assert (S101)` example and the change of `>`

---

_Issue opened by @Anselmoo on 2023-08-29 16:53_

When using the assertion `assert(S101)`, changing from `x > 0` to `x <= 0` can be confusing because these statements might have different meanings.

`x <= 0` checks if `x` is **less than** or **equal** to **zero**, while `x > 0` checks if x is greater **than zero.**

For example, if x is -1, then `x <= 0` is true, but `x > 0` is false. On the other hand, if x is 1, then `x <= 0` is false, but `x > 0` is true.

https://github.com/astral-sh/ruff/blob/25c374856a57c3a58fdbd2b347ed656fd30fd9c0/crates/ruff/src/rules/flake8_bandit/rules/assert_used.rs#L19-L28


---

_Closed by @Anselmoo on 2023-08-29 16:57_

---

_Comment by @zanieb on 2023-08-29 16:58_

Hi!

Since the `if` statement will raise an error if the expression is true and an assert will raise an error if the expression is false this should be working as intended.

Would you prefer the documentation called this out explicitly? Or used `if not x > 0:`?

---

_Reopened by @Anselmoo on 2023-08-29 17:00_

---

_Comment by @Anselmoo on 2023-08-29 17:06_

@zanieb Thank you very much!

I agree with you! Although the information is technically the same, it can be difficult to understand just by quickly reading through the documentation. Your suggestion of using `if not x > 0` is helpful because it highlights the main point for the reader using the `assert` condition, which is how I understand the documentation. 

---

_Label `documentation` added by @zanieb on 2023-08-29 17:16_

---

_Label `good first issue` added by @zanieb on 2023-08-29 17:16_

---

_Comment by @zanieb on 2023-08-29 17:18_

No problem!

I think including an `if not ...` example is clearer. We can retain the existing example after an "or..." if we want. Want to contribute? :)

---

_Comment by @Anselmoo on 2023-08-29 17:24_

Based on the suggestions for refactoring found at https://github.com/sourcery-ai/sourcery/wiki/Current-Refactorings#de-morgan-identity, it appears that you may be introducing `morgan-identity-cause`. So I would refer to the optional "or..."

---

_Closed by @zanieb on 2023-08-29 21:08_

---
