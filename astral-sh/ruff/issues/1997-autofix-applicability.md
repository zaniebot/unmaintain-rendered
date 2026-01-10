```yaml
number: 1997
title: Autofix applicability
type: issue
state: closed
author: not-my-profile
labels:
  - core
assignees: []
created_at: 2023-01-19T15:07:07Z
updated_at: 2023-06-14T22:28:44Z
url: https://github.com/astral-sh/ruff/issues/1997
synced_at: 2026-01-10T11:09:44Z
```

# Autofix applicability

---

_Issue opened by @not-my-profile on 2023-01-19 15:07_

Like categorization (#1774) this is another very useful concept we can steal from Rust, see [rustc_errors::Applicability](https://doc.rust-lang.org/stable/nightly-rustc/rustc_errors/enum.Applicability.html).

Ideally users would be able to choose how much they want to risk when applying lints.

I think for rule applicability is even more relevant for Python than for Rust since pretty much all of the behavior of Python types can be overriden by implementing custom `__dunder__` methods.

---

_Label `core` added by @charliermarsh on 2023-01-25 04:47_

---

_Renamed from "Rule applicability" to "Autofix applicability" by @not-my-profile on 2023-02-10 15:22_

---

_Comment by @sbrugman on 2023-02-16 08:24_

Referenced this in a couple of issues that could use this functionality

---

_Comment by @charliermarsh on 2023-02-16 14:09_

Thank you!

---

_Comment by @charliermarsh on 2023-02-26 20:18_

Would this primarily be for _autofix_ risk, or also, flagging violations in the first place?

---

_Comment by @not-my-profile on 2023-02-27 03:56_

What do you mean with "flagging violations in the first place"?

I think there are different factors we could possibly differentiate:

1. How accurate is the violation detection? e.g. can false positives / false negatives occur? how likely is that?
2. How safe is the suggested autofix? Can it be wrong? Can it break the program? Could it have side-effects?

---

_Comment by @charliermarsh on 2023-02-27 04:11_

Right -- those are the two factors that I was trying to tease apart, though I think I was just tired and missed that the issue _title_ is about autofixing, so any analogous concept for "How accurate is the violation detection?" would probably go by a different name.

---

_Comment by @not-my-profile on 2023-02-27 04:24_

I think it makes sense to raise this question here. I think rules that will have many false positives should just be disabled by default and categorized (#1774) accordingly.

---

_Comment by @zanieb on 2023-06-14 22:19_

I think this work is better captured by #4181 now, should we close this issue in favor of that one?

As an additional xref, here's the pull request that added applicability levels https://github.com/astral-sh/ruff/pull/4303

---

_Comment by @charliermarsh on 2023-06-14 22:28_

Yeah, good call -- closing in favor of #4181.

---

_Closed by @charliermarsh on 2023-06-14 22:28_

---
