---
number: 2532
title: "`rule` command: show configuration options for a rule"
type: issue
state: closed
author: ngnpope
labels:
  - cli
assignees: []
created_at: 2023-02-03T12:19:04Z
updated_at: 2023-02-11T03:11:15Z
url: https://github.com/astral-sh/ruff/issues/2532
synced_at: 2026-01-07T13:12:14-06:00
---

# `rule` command: show configuration options for a rule

---

_Issue opened by @ngnpope on 2023-02-03 12:19_

Looking at the current output in v0.0.240, e.g. for `PLR0913`:

```
❯ ruff rule PLR0913
too-many-args

Code: PLR0913 (Pylint)

Message formats:

* Too many arguments to function call ({c_args}/{max_args})
```

I have the following observations:

- There is no description of `{c_args}` or `{max_args}`. What is from configuration, what is from the execution?
  - What is "c_args"? Map placeholders to more friendly names for display?
- We could explain that this rule is affected by `max-args` in `[tool.ruff.pylint]`
- We could show the value, including whether it is the default or which configuration file it came from
- We could show whether the rule is enabled/selected or disabled/ignored
  - Show which configuration file decided this
  - State whether the rule conflicts with another rule or configuration option and show which got disabled

---

_Label `cli` added by @charliermarsh on 2023-02-03 13:41_

---

_Comment by @charliermarsh on 2023-02-03 13:41_

Some overlap (arguably a dupe, maybe) with #1467. This will all be dramatically improved, I hope!

---

_Comment by @ngnpope on 2023-02-03 13:45_

👍🏻

---

_Comment by @not-my-profile on 2023-02-03 15:09_

>There is no description of {c_args} or {max_args}

I don't think we should document the placeholders used in the format strings. If they are unclear they should be renamed e.g. `c_args` isn't really a good variable name (what does the "c" stand for?).

We are planning on adding written descriptions of the rules which would make the format strings much less important.

I agree about all your other points though :)

---

_Comment by @charliermarsh on 2023-02-11 03:11_

I'm going to close this just because we've started to include the configuration settings in the Markdown docs as a convention, so it's part of #2646.

---

_Closed by @charliermarsh on 2023-02-11 03:11_

---
