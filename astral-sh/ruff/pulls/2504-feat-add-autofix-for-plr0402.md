```yaml
number: 2504
title: "feat: add autofix for PLR0402"
type: pull_request
state: merged
author: spaceone
labels: []
assignees: []
merged: true
base: main
head: 2199-autofix-PLR0402
created_at: 2023-02-02T23:45:30Z
updated_at: 2023-02-06T14:07:38Z
url: https://github.com/astral-sh/ruff/pull/2504
synced_at: 2026-01-12T15:55:08Z
```

# feat: add autofix for PLR0402

---

_@spaceone_

Issue #2199

---

_@charliermarsh reviewed on 2023-02-02 23:46_

---

_Review comment by @charliermarsh on `src/rules/pylint/rules/use_from_import.rs`:51 on 2023-02-02 23:46_

Should this not be `import_stmt.end_location.unwrap()`?

---

_@spaceone reviewed on 2023-02-03 00:06_

---

_Review comment by @spaceone on `src/rules/pylint/rules/use_from_import.rs`:51 on 2023-02-03 00:06_

OK, changed. it should be the same.
I also fixed a special case when there are multiple imports in that import statement.

---

_Comment by @spaceone on 2023-02-03 03:40_

@charliermarsh Off-Topic:
I just merged the branch which does our tab to spaces migration and many more ruff-enhanced things ([merge commit: 1,969 changed files with 203,016 additions and 203,292 deletions](https://github.com/univention/univention-corporate-server/commit/2e5688d5636a453b266ddf6a8315552ce6862383)).
If your are interested: this is our [pyproject.toml](https://github.com/univention/univention-corporate-server/blob/5.0-3/pyproject.toml).

Therefore I wanted to thank you for your effort in this project!!! The raw tabs-to-spaces migration was done with `autopep8` but `ruff` really helped me with the docstrings, which was not correctly done by `autopep8` and it made a lot of useful code refactoring things.

And I also wanted to appreciate your kindness in our collaboration: you took every issue of me serious and communicated clearly. You gave good hints in MRs. I enjoyed working with you so far!

I hope I will (have more time to) learn rust and contribute back to ruff in the future.
Best regards
spaceone

---

_Comment by @charliermarsh on 2023-02-03 04:00_

@spaceone - That's amazing! Thank you so much for this kind note, and I'm really glad that Ruff is working well for you. I've enjoyed our collaboration too. You filed a lot of issues 🤣 and found tons of edge cases -- I appreciate _your_ contributions too (and I'm glad we could fix most of those issues!).


---

_Merged by @charliermarsh on 2023-02-03 04:25_

---

_Closed by @charliermarsh on 2023-02-03 04:25_

---
