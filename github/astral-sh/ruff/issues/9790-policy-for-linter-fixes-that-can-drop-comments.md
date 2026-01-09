---
number: 9790
title: Policy for linter fixes that can drop comments
type: issue
state: closed
author: zanieb
labels:
  - fixes
assignees: []
created_at: 2024-02-02T16:34:47Z
updated_at: 2024-11-13T13:04:00Z
url: https://github.com/astral-sh/ruff/issues/9790
synced_at: 2026-01-07T13:12:15-06:00
---

# Policy for linter fixes that can drop comments

---

_Issue opened by @zanieb on 2024-02-02 16:34_

In some cases, a lint fix can drop comments. Retaining all comments was a major goal in the Ruff formatter and we've learned that it is very hard to accomplish. There is significant foundational work that would need to be invested in the linter to prevent it from ever dropping comments. While we would like to accomplish this eventually, we're a small team with a lot of priorities to balance. In the meantime, we want to formalize a policy for fixes that may drop comments. 

A few options:

1. A fix should not be offered if the range contains a comment
1. A fix that may drop a comment must always be unsafe
1. A fix that may drop a comment must be unsafe if a comment is present in the fix range
1. A fix will only drop comments in ranges that use dangerous syntax e.g. line continuations
1. A fix that may drop a comment is always safe — we do not make guarantees about comments

We currently apply a mix of these options, but we should be consistent. More options and discussion welcome.

Related issues and reports:
- https://github.com/astral-sh/ruff/issues/9779
- https://github.com/astral-sh/ruff/issues/9715
- https://github.com/astral-sh/ruff/issues/4296

---

_Label `autofix` added by @zanieb on 2024-02-02 16:34_

---

_Referenced in [astral-sh/ruff#9779](../../astral-sh/ruff/issues/9779.md) on 2024-02-02 16:35_

---

_Comment by @charliermarsh on 2024-02-02 16:35_

Another thing we do in some places (which IMO is incorrect): if the range contains a comment, we don't offer the fix at all. For example, this is true of `SIM401`.

---

_Comment by @charliermarsh on 2024-02-02 19:24_

There also used to be some cases in which we wouldn't flag the violation _at all_ if the range contained comments, but I think we removed these. (I'm not certain.)

---

_Comment by @charliermarsh on 2024-02-02 19:28_

Just to put a decision out there for discussion, I'd propose...

> Dropping comments should be treated as unsafe (and should be documented as such -- it's not unsafe per se based on the current definition in the rustdoc, in my opinion). But we should avoid _disabling_ violations and _disabling_ fixes based on the presence of comments. We will go through and audit rules that rewrites large blocks of code (this tends _not_ to be the norm), e.g., those that modify entire statements, and add appropriate guardrails to them.

---

_Comment by @charliermarsh on 2024-02-02 21:42_

Since I was asked elsewhere to clarify, the above suggestion is meant to say:

We should not disable a rule due to the presence of comments. 

We should not disable a fix due to the presence of comments.

We should mark a fix as unsafe if there's a comment in the range.

We should only prioritize proactively changing the behavior of rules that rewrite large blocks of code (modify entire statements, or convert statements to expressions or vice versa).


---

_Comment by @MichaReiser on 2024-02-04 11:31_

The ideal situation would be that all rules handle comments correctly and only remove comments if a fix removes the entire code block containing the comment. 

Now, what the short-term ideal should be is something I find much harder to implement, and I don't feel I have the understanding to decide on that. What's important to me is that we can come up with a proposal that avoids making the `safe` and `unsafe` rule separation meaningless because too many rules are `unsafe`, so that users who want to use most of our fixes are forced (or motivated) to opt-in to enable unsafe fixes by default. 

That's why I need a better understanding of how many rules would be affected by any of the above proposals and the ideal solution to me is the solution where only enabling safe fixes remains the best default for users while being as explicit (and defensive) in which situation ruff removes comments.


---

_Comment by @UnknownPlatypus on 2024-02-04 14:54_

Chiming in because I recently encountered a somehow related issue.
I got bitten by a combinaison of `TID252` with a `noqa: F401` on one of the imports.

Basically something like that:
```diff
-from . import (
-    foo,
-    bar,
-    MyVeryImportantClass, # noqa: F401
-)
+ from yay import foo, bar
```
I think what you proposed above about demoting the fix safety to unsafe in case there are comments would have solve my issue here.


---

_Referenced in [astral-sh/ruff#10063](../../astral-sh/ruff/issues/10063.md) on 2024-02-20 16:28_

---

_Referenced in [astral-sh/ruff#10245](../../astral-sh/ruff/issues/10245.md) on 2024-03-06 14:10_

---

_Comment by @jakkdl on 2024-03-06 15:53_

My 2 cents:
`unsafe-because-it-may-remove-comments` vs `unsafe-because-it-may-impact-logic` (e.g. sort stability in C413/C414) should perhaps not be treated the same in terms of flags or warnings. Depending on what you settle on, you might want different flags or different levels of unsafe-ness to differentiate them.

If possible it would be great if ruff could *detect* whether it has lost comments, and surface that to the user. I don't have as strong opinions between
1. dump lost comments somewhere in the vicinity for the user to deal with
2. warn the user about comments that have been deleted
3. say that a fix was attempted, but failed because it would delete a comment, and prompt the user to possibly add a flag that would force the `--fix` and delete the comment.

I do agree that the current status quo is kinda iffy, but I've also spent an unreasonable amount of time trying to preserve comments in AST & libcst parsers that I can empathize <3

---

_Referenced in [astral-sh/ruff#11233](../../astral-sh/ruff/pulls/11233.md) on 2024-05-03 11:59_

---

_Referenced in [astral-sh/ruff#13545](../../astral-sh/ruff/issues/13545.md) on 2024-09-28 12:01_

---

_Referenced in [astral-sh/ruff#14268](../../astral-sh/ruff/pulls/14268.md) on 2024-11-11 11:56_

---

_Referenced in [astral-sh/ruff#14270](../../astral-sh/ruff/pulls/14270.md) on 2024-11-12 10:23_

---

_Referenced in [astral-sh/ruff#14300](../../astral-sh/ruff/pulls/14300.md) on 2024-11-12 20:53_

---

_Closed by @charliermarsh on 2024-11-13 13:04_

---

_Referenced in [astral-sh/ruff#15521](../../astral-sh/ruff/pulls/15521.md) on 2025-01-17 05:57_

---
