```yaml
number: 3467
title: "pylint: E1507 invalid-envvar-value"
type: pull_request
state: merged
author: latonis
labels:
  - rule
assignees: []
merged: true
base: main
head: invalid-envvar-value-pylint
created_at: 2023-03-12T17:49:08Z
updated_at: 2023-03-12T21:49:19Z
url: https://github.com/astral-sh/ruff/pull/3467
synced_at: 2026-01-12T04:39:44Z
```

# pylint: E1507 invalid-envvar-value

---

_Pull request opened by @latonis on 2023-03-12 17:49_

Rule reference: https://pycodequ.al/docs/pylint-messages/e1507-invalid-envvar-value.html

Implemented for Pylint parent issue: #970 

---

_Comment by @github-actions[bot] on 2023-03-12 18:11_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pylint/invalid_envvar_value.py`:10 on 2023-03-12 18:32_

Nit: can you add a newline here? Or better yet, run this file through Black?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/invalid_envvar_value.rs`:44 on 2023-03-12 18:33_

I think `os.getenv(key="foo", default_value="bar")` is also valid. Can we also check if `key` is provided as a kwarg?

---

_@charliermarsh requested changes on 2023-03-12 18:33_

Thanks!

---

_@latonis reviewed on 2023-03-12 18:54_

---

_Review comment by @latonis on `crates/ruff/resources/test/fixtures/pylint/invalid_envvar_value.py`:10 on 2023-03-12 18:54_

Fixed :)

---

_Review comment by @latonis on `crates/ruff/src/rules/pylint/rules/invalid_envvar_value.rs`:44 on 2023-03-12 18:55_

Implemented :)

---

_@latonis reviewed on 2023-03-12 18:55_

---

_Comment by @latonis on 2023-03-12 19:10_

> information_source ecosystem check **detected changes**. (+0, -360, 0 error(s))
> 
> zulip (+0, -2)
> ```diff
> - zerver/apps.py:32:9: B018 Found useless attribute access. Either assign it to a variable or remove it.
> - zerver/views/realm.py:185:5: B018 Found useless expression. Either assign it to a variable or remove it.
> ```
> 
> bokeh (+0, -309)
> airflow (+0, -49)

Changes have been implemented @charliermarsh ~~, but I'm unsure why these changes happened in the ecosystem checks, I don't believe this has anything to do with my changes~~?

Ah, I see https://github.com/charliermarsh/ruff/issues/3458 is open for this.

---

_Review requested from @charliermarsh by @latonis on 2023-03-12 19:10_

---

_Comment by @charliermarsh on 2023-03-12 19:27_

@latonis - Yeah all good -- there's something wrong with the base-branch comparison.

---

_Label `rule` added by @charliermarsh on 2023-03-12 21:37_

---

_Merged by @charliermarsh on 2023-03-12 21:43_

---

_Closed by @charliermarsh on 2023-03-12 21:43_

---

_Branch deleted on 2023-03-12 21:49_

---
