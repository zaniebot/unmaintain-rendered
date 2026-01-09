---
number: 1908
title: Enable all rules by default or adding an option to enable all rules
type: issue
state: closed
author: zeshuaro
labels: []
assignees: []
created_at: 2023-01-16T07:34:06Z
updated_at: 2023-01-16T07:53:42Z
url: https://github.com/astral-sh/ruff/issues/1908
synced_at: 2026-01-07T13:12:14-06:00
---

# Enable all rules by default or adding an option to enable all rules

---

_Issue opened by @zeshuaro on 2023-01-16 07:34_

People would have different opinions on this and probably worth a discussion.

With the rapid development happening in Ruff, occasionally I come back and enable the newly supported rules for my project because they all seem to be very useful. Some of them have caught actual bugs that I was not aware of previously.

I would argue that if a rule is supported by Ruff, then there's probably a good reason for it to be added in the first place and thus should be enforced by default.

Obviously, there's the other side of the argument where this can easily break people's CI/CD etc. That's why I was hoping there's an option as an alternative to specify for Ruff to enable all supported rules. But people should still be able to ignore/exclude rules with this option, if that makes sense.

---

_Comment by @charliermarsh on 2023-01-16 07:47_

We support a special code for this! `ALL`

E.g.

```toml
[tool.ruff]
select = ["ALL"]
```

---

_Comment by @zeshuaro on 2023-01-16 07:50_

Aha, that's exactly what I need. Must have missed it from the docs. Thanks for pointing it out and great work on the library!

---

_Closed by @zeshuaro on 2023-01-16 07:50_

---

_Comment by @charliermarsh on 2023-01-16 07:53_

It’s buried in there 😂 Maybe deserves a question in the FAQ. (Eventually we will completely rework the docs.)

Thank you, and thanks for using Ruff!

---
