```yaml
number: 909
title: Add flake8-debugger
type: pull_request
state: merged
author: karpa4o4
labels: []
assignees: []
merged: true
base: main
head: add-flake8-debugger
created_at: 2022-11-26T09:56:24Z
updated_at: 2023-01-21T16:26:38Z
url: https://github.com/astral-sh/ruff/pull/909
synced_at: 2026-01-12T04:51:59Z
```

# Add flake8-debugger

---

_Pull request opened by @karpa4o4 on 2022-11-26 09:56_

_No description provided._

---

_Comment by @karpa4o4 on 2022-11-26 10:07_

@charliermarsh, hi. I tried to bring the logic as close to the original as possible, but in debugger_call it was not completely successful. I will be glad to receive feedback.

---

_Comment by @charliermarsh on 2022-11-26 19:48_

Awesome! Will review this shortly. Thanks for putting this together.

---

_Comment by @charliermarsh on 2022-11-26 21:20_

@karpa4o4 - I did some small refactors to use `match_call_path` and friends, which are some utilities we have to do this kind of "Is a function call an invocation of a specific member, in a specific module?" logic.

---

_Merged by @charliermarsh on 2022-11-26 21:21_

---

_Closed by @charliermarsh on 2022-11-26 21:21_

---

_Comment by @charliermarsh on 2022-11-26 21:21_

But let me know if I messed anything up please :)

---

_Branch deleted on 2022-11-26 21:44_

---

_Comment by @karpa4o4 on 2022-11-26 21:53_

@charliermarsh, thanks. Sorry, I forgot to write "Closed" in the pull request description. So issue [#546](https://github.com/charliermarsh/ruff/issues/546) stays open.
Can I help with the implementation of some other rule?

---

_Comment by @charliermarsh on 2022-11-26 22:06_

Oh good catch! Just closed the issue.

> Can I help with the implementation of some other rule?

Definitely! There's a bunch of unclaimed stuff in https://github.com/charliermarsh/ruff/issues/827, if you're interested in those? Alternatively, there's https://github.com/charliermarsh/ruff/issues/458 but it's a bit more involved as I'd like it to be integrated with some of the existing docstring checks.


---

_Comment by @charliermarsh on 2022-11-26 22:07_

Oh, or https://github.com/charliermarsh/ruff/issues/870 -- we only need the first rule (U100). It'd probably look like the implementation we have for `unused_variables`, but we have to check for `BindingKind::Argument`.

---

_Comment by @karpa4o4 on 2023-01-21 14:54_

@charliermarsh, hi. Unfortunately I could not do the issues that you described above. But now I want to participate in the development of ruff again, can you tell me how I can help? 

---

_Comment by @charliermarsh on 2023-01-21 16:26_

No worries! If you're interested, you could consider working on the [`tryceratops`](https://github.com/charliermarsh/ruff/issues/2056) plugin.

---
