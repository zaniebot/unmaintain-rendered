```yaml
number: 12070
title: Improve Emacs configuration
type: pull_request
state: merged
author: bersace
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-06-27T13:58:46Z
updated_at: 2024-06-28T07:51:11Z
url: https://github.com/astral-sh/ruff/pull/12070
synced_at: 2026-01-12T15:55:40Z
```

# Improve Emacs configuration

---

_@bersace_

Replace black and combine `ruff check --select=I --fix` and `ruff format`.


---

_Comment by @bersace on 2024-06-27 13:59_

Based on apheleia user guide : https://github.com/radian-software/apheleia/?tab=readme-ov-file#user-guide .

---

_@dhruvmanila approved on 2024-06-28 04:19_

This seems correct as per the linked documentation.

Do you know why does the old configuration not work or is this just a better way to do it? Sorry to ask a basic question but I don't think anyone in the team uses emacs.

---

_Label `documentation` added by @dhruvmanila on 2024-06-28 04:19_

---

_Comment by @bersace on 2024-06-28 06:05_

> Do you know why does the old configuration not work or is this just a better way to do it? Sorry to ask a basic question but I don't think anyone in the team uses emacs.

The old configuration works with two limitations:

- `black` may still be called, especially if `ruff` fails.
- User have to choose whether to format or to sort imports.

The suggested configuration is more efficient, reliable and featureful.

---

_Comment by @dhruvmanila on 2024-06-28 07:38_

Thank you for providing the details. I'll go ahead and merge this. Welcome to Ruff!

---

_Comment by @dhruvmanila on 2024-06-28 07:39_

(I've updated the PR description to reflect that this combines formatting and import sorting.)

---

_Merged by @dhruvmanila on 2024-06-28 07:39_

---

_Closed by @dhruvmanila on 2024-06-28 07:39_

---

_Branch deleted on 2024-06-28 07:51_

---
