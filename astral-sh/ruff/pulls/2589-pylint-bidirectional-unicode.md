```yaml
number: 2589
title: "[`pylint`]: bidirectional-unicode"
type: pull_request
state: merged
author: colin99d
labels:
  - rule
assignees: []
merged: true
base: main
head: bidirectional-unicode
created_at: 2023-02-05T20:45:11Z
updated_at: 2023-02-07T03:49:19Z
url: https://github.com/astral-sh/ruff/pull/2589
synced_at: 2026-01-12T04:52:00Z
```

# [`pylint`]: bidirectional-unicode

---

_Pull request opened by @colin99d on 2023-02-05 20:45_

Ref: #970 

---

_@charliermarsh reviewed on 2023-02-06 19:29_

---

_Review comment by @charliermarsh on `src/rules/pylint/rules/bidirectional_unicode.rs`:28 on 2023-02-06 19:29_

What's the source for this list?

---

_@colin99d reviewed on 2023-02-07 02:42_

---

_Review comment by @colin99d on `src/rules/pylint/rules/bidirectional_unicode.rs`:28 on 2023-02-07 02:42_

https://github.com/PyCQA/pylint/blob/main/pylint/checkers/unicode.py#L37

---

_@charliermarsh reviewed on 2023-02-07 02:46_

---

_Review comment by @charliermarsh on `src/rules/pylint/rules/bidirectional_unicode.rs`:28 on 2023-02-07 02:46_

Thank you :)

---

_Comment by @colin99d on 2023-02-07 02:51_

<img width="1950" alt="Screenshot 2023-02-06 at 9 51 41 PM" src="https://user-images.githubusercontent.com/72827203/217135818-703c2508-9b3b-4f3a-9a96-2e208e8b317a.png">
Looks like Github doesn't like it either!

---

_Comment by @colin99d on 2023-02-07 02:53_

Also, the file `src/registry.rs` should be deleted, right?

---

_Comment by @charliermarsh on 2023-02-07 02:55_

Yes although I'm wondering if we lost any rules in the refactor that moved that file.

---

_Comment by @colin99d on 2023-02-07 03:13_

When I tried to add a rule there on accident it threw a bunch of errors, I would imagine that would happen with other errors as well.

---

_Label `rule` added by @charliermarsh on 2023-02-07 03:49_

---

_Merged by @charliermarsh on 2023-02-07 03:49_

---

_Closed by @charliermarsh on 2023-02-07 03:49_

---
