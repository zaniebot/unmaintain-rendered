```yaml
number: 1449
title: "docs(README): add missing `flake8-simplify`"
type: pull_request
state: merged
author: mkniewallner
labels: []
assignees: []
merged: true
base: main
head: docs/add-missing-flake8-simplify-readme
created_at: 2022-12-29T20:47:29Z
updated_at: 2022-12-29T22:04:27Z
url: https://github.com/astral-sh/ruff/pull/1449
synced_at: 2026-01-12T15:55:06Z
```

# docs(README): add missing `flake8-simplify`

---

_@mkniewallner_

Noticed that `flake8-simplify` is mentioned [here](https://github.com/charliermarsh/ruff#flake8-simplify-sim), but not in the list of tools that `ruff` can replace.

I based the total number of rules on what's listed in https://github.com/MartinThoma/flake8-simplify#rules (without the "legacyfix" and "removed" ones) and included the experimental rules (`SIM9`), but if you prefer not to include the experimental ones, it totals to 30 rules.

---

_Comment by @charliermarsh on 2022-12-29 21:40_

I think I omitted it initially because we'd only done one rule so far, but this is also fine :)

---

_Comment by @charliermarsh on 2022-12-29 21:42_

Somehow I got 33 rules when I created the table [here](https://github.com/charliermarsh/ruff/issues/998) -- is the discrepancy obvious to you? Is that table missing some, or the other way around?

---

_Comment by @mkniewallner on 2022-12-29 21:49_

> I think I omitted it initially because we'd only done one rule so far, but this is also fine :)

Ha, I was wondering if it was intentional or not, but since there are other plugins listed with only 1 rule (`autoflake` and `flake8-tidy-imports`, although they have way less total rules), I wasn't sure.

> Somehow I got 33 rules when I created the table [here](https://github.com/charliermarsh/ruff/issues/998) -- is the discrepancy obvious to you? Is that table missing some, or the other way around?

Differences I can see:
- `SIM104` is in the list, and I didn't count it as it is listed as a "legacyfix"
- `SIM224` is in the list, but it's a placeholder for an experimental rule
- `SIM124` is in the list, but it's a placeholder for an experimental rule

---

_Comment by @mkniewallner on 2022-12-29 21:52_

The list also doesn't include experimental rules under `SIM9` (although it's probably intended), so without the 3 extra rules, it totals to 30 (and 37 if we count the experimental ones).

---

_Comment by @charliermarsh on 2022-12-29 22:01_

Ok cool -- let's go with your list, those explanations make sense! I'll merge and edit the issue.

---

_Merged by @charliermarsh on 2022-12-29 22:02_

---

_Closed by @charliermarsh on 2022-12-29 22:02_

---

_Branch deleted on 2022-12-29 22:04_

---
