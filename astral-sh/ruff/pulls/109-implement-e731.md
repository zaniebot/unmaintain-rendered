```yaml
number: 109
title: Implement E731
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: E731
created_at: 2022-09-06T13:34:09Z
updated_at: 2022-09-06T14:45:13Z
url: https://github.com/astral-sh/ruff/pull/109
synced_at: 2026-01-12T05:48:44Z
```

# Implement E731

---

_Pull request opened by @harupy on 2022-09-06 13:34_

Implement https://www.flake8rules.com/rules/E731.html.

---

_@charliermarsh approved on 2022-09-06 13:48_

---

_Merged by @charliermarsh on 2022-09-06 13:48_

---

_Closed by @charliermarsh on 2022-09-06 13:48_

---

_Comment by @harupy on 2022-09-06 13:57_

@charliermarsh Thanks for merging the PR! Is there a plan to support pylint rules?

---

_Comment by @charliermarsh on 2022-09-06 14:33_

Good question :)

In my head, the current focus is achieving Flake8 parity _when Flake8 is used with Black_. So that includes most (all?) of the the pyflakes rules (the `F` series), plus a handful of the pycodestyle rules (the `E` series), although many of those are made redundant by Black.

That said: I'm definitely not opposed to adding pylint rules (`R001` is actually a pylint rule, though they call it `R0205`). The only rules I'd want to avoid are those that are low utility and/or have a very complex implementation.

We can always disable them by default if we want to stick to Flake8 parity out-of-the-box. We'd also need to figure out the right nomenclature for those rules (I originally copied pylint and used `R0205` for that rule, but it felt out-of-place with the three-letter codes, and I realized we could run into conflicts and other confusing situations, so I just reindexed them for ruff).


---

_Comment by @harupy on 2022-09-06 14:45_

Makes sense! Thanks for the explanation :)

---
