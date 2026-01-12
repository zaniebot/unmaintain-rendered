```yaml
number: 3414
title: Remove empty line after RUF100 auto-fix
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: remove-empty-line-after-ruf100-auto-fix
created_at: 2023-03-09T09:53:51Z
updated_at: 2023-03-11T06:40:17Z
url: https://github.com/astral-sh/ruff/pull/3414
synced_at: 2026-01-12T04:39:44Z
```

# Remove empty line after RUF100 auto-fix

---

_Pull request opened by @JonathanPlasse on 2023-03-09 09:53_

- Closes #2845
- Comments after `noqa`s are removed after the auto-fix, this is not a regression. It was already the case.
I opened an issue to track it #3413.

Edit:

- Closes #3413

---

_@JonathanPlasse reviewed on 2023-03-10 10:53_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/noqa.rs`:23 on 2023-03-10 10:53_

`spaces_after_noqa` is used to conserve spaces when there is
```python
print(a)  # noqa: E501, F821 # comment
```
which would become this without matching for `spaces_after_noqa`.
```python
print(a)  # noqa: F821# comment
```
Instead of this
```python
print(a)  # noqa: F821 # comment
```
It is also used to have a better error range.

---

_Comment by @charliermarsh on 2023-03-10 22:51_

Nice! Thank you.

---

_Merged by @charliermarsh on 2023-03-10 22:57_

---

_Closed by @charliermarsh on 2023-03-10 22:57_

---

_Comment by @github-actions[bot] on 2023-03-10 23:00_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Branch deleted on 2023-03-11 06:40_

---
