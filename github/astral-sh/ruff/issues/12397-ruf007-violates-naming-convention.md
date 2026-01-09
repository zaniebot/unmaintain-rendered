---
number: 12397
title: RUF007 violates naming convention
type: issue
state: closed
author: dylwil3
labels:
  - rule
assignees: []
created_at: 2024-07-18T22:30:38Z
updated_at: 2024-07-18T23:26:28Z
url: https://github.com/astral-sh/ruff/issues/12397
synced_at: 2026-01-07T13:12:15-06:00
---

# RUF007 violates naming convention

---

_Issue opened by @dylwil3 on 2024-07-18 22:30_

[pairwise-over-zipped (RUF007)](https://docs.astral.sh/ruff/rules/pairwise-over-zipped/) appears to violate the [naming convention](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#rule-naming-convention), unless I'm misunderstanding either the rule or the convention.

Perhaps `zip-over-pairwise` or `zip-for-pairwise` would be more appropriate? Happy to make the change, or have the issue closed if this seems like not a big deal.

Here is the rule to save you a click:

> When iterating over successive pairs of elements, prefer `itertools.pairwise()` over `zip()`.
> 
> 
> ```python
> letters = "ABCD"
> zip(letters, letters[1:])  # ("A", "B"), ("B", "C"), ("C", "D")
> ```
> 
> Use instead:
> 
> ```python
> from itertools import pairwise
> 
> letters = "ABCD"
> pairwise(letters)  # ("A", "B"), ("B", "C"), ("C", "D")
> ```

And here is the relevant part of the naming convention:

> Like Clippy, Ruff's rule names should make grammatical and logical sense when read as "allow ${rule}" or "allow ${rule} items", 
> as in the context of suppression comments.
> 
> For example, AssertFalse fits this convention: it flags assert False statements, and so a suppression comment would be framed as "allow assert False".


---

_Comment by @charliermarsh on 2024-07-18 22:31_

Yeah this seems wrong (and we can change it since the human-readable names aren't part of our versioning policy).

---

_Comment by @charliermarsh on 2024-07-18 22:33_

I think `zip-instead-of-pairwise` would be most consistent with other rules.

---

_Label `rule` added by @charliermarsh on 2024-07-18 22:46_

---

_Comment by @charliermarsh on 2024-07-18 22:46_

If you're interested in updating, I'd happily merge a PR.

---

_Comment by @dylwil3 on 2024-07-18 22:53_

Sure - on it!

---

_Referenced in [astral-sh/ruff#12399](../../astral-sh/ruff/pulls/12399.md) on 2024-07-18 23:22_

---

_Closed by @charliermarsh on 2024-07-18 23:26_

---

_Closed by @charliermarsh on 2024-07-18 23:26_

---
