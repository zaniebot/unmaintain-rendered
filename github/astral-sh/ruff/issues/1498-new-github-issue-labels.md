---
number: 1498
title: New GitHub issue labels
type: issue
state: closed
author: not-my-profile
labels:
  - internal
assignees: []
created_at: 2022-12-31T05:31:54Z
updated_at: 2022-12-31T18:22:25Z
url: https://github.com/astral-sh/ruff/issues/1498
synced_at: 2026-01-07T13:12:14-06:00
---

# New GitHub issue labels

---

_Issue opened by @not-my-profile on 2022-12-31 05:31_

I think it would make sense to introduce the following labels:

* `core` for issues about changing something fundamental about ruff (e.g. introducing a plugin architecture, introducing error severity (warnings vs errors), etc.)
* `CLI`, for issues related to the command-line interface (currently [1473](https://www.github.com/charliermarsh/ruff/issues/1473), [1470](https://www.github.com/charliermarsh/ruff/issues/1470), [995](https://www.github.com/charliermarsh/ruff/issues/995), [455](https://www.github.com/charliermarsh/ruff/issues/455))
* `autofix`, for issues about implementing/improving `--fix` support for an existing check (several current issues)
* `docstrings`, for issues related to docstrings (currently [993](https://www.github.com/charliermarsh/ruff/issues/993), [924](https://www.github.com/charliermarsh/ruff/issues/924), [458](https://www.github.com/charliermarsh/ruff/issues/458))


The `rule` label is currently described as "Implementing a known but unsupported lint rule" and apparently not added to issues proposing a new rule when there is no clear existing lint rule it can be modeled after ... I think it would make sense to expand the scope of `rule` to new rule requests in general (no matter if the rule exists in an existing linter or not).

I also think it would make sense to rename the label `enhancement` to `new feature` because arguably any issue here is for some kind of enhancement ... enhancement is too ambiguous.

I am happy to help with triaging (adding labels and updating issue titles to be more descriptive).

---

_Comment by @charliermarsh on 2022-12-31 13:18_

I like the new labels you've proposed. I'll go ahead and add them today. I also want to add a `plugin` label since we have a few issues that are "meta-issues" for an entire plugin.

`enhancement` vs. `new feature` I'm unsure about. I agree `enhancement` is too ambiguous...


---

_Label `documentation` added by @charliermarsh on 2022-12-31 13:18_

---

_Label `internal` added by @charliermarsh on 2022-12-31 13:18_

---

_Label `documentation` removed by @charliermarsh on 2022-12-31 13:18_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-31 18:06_

---

_Closed by @charliermarsh on 2022-12-31 18:22_

---

_Comment by @charliermarsh on 2022-12-31 18:22_

Done. Triaged all issues too.

---
