```yaml
number: 674
title: "Should respect isort:skip comments"
type: issue
state: closed
author: andersk
labels:
  - isort
assignees: []
created_at: 2022-11-11T00:46:12Z
updated_at: 2022-11-11T17:38:02Z
url: https://github.com/astral-sh/ruff/issues/674
synced_at: 2026-01-10T15:56:05Z
```

# Should respect isort:skip comments

---

_Issue opened by @andersk on 2022-11-11 00:46_

`ruff` currently respects `noqa: I001` comments to prevent sorting certain imports. For improved isort compatibility, it should also respect `isort:skip` and `isort:skip_file` comments ([documentation](https://pycqa.github.io/isort/#skip-processing-of-imports-outside-of-configuration)).

---

_Comment by @charliermarsh on 2022-11-11 00:57_

Good call.

---

_Comment by @charliermarsh on 2022-11-11 03:29_

It looks like `isort:off` and `isort:on` are [recommended](https://pycqa.github.io/isort/docs/configuration/action_comments.html#isort-off) in the docs, so maybe I'll do those first.

---

_Label `isort` added by @charliermarsh on 2022-11-11 03:32_

---

_Comment by @charliermarsh on 2022-11-11 03:34_

Some extremely crude metrics from code search:

<img width="274" alt="Screen Shot 2022-11-10 at 10 34 02 PM" src="https://user-images.githubusercontent.com/1309177/201257440-14ed2eaf-13f2-47be-b104-a47baad57afb.png">
<img width="282" alt="Screen Shot 2022-11-10 at 10 34 10 PM" src="https://user-images.githubusercontent.com/1309177/201257446-4f32c83e-3cba-4ea4-88a0-1890040596e1.png">
<img width="228" alt="Screen Shot 2022-11-10 at 10 34 17 PM" src="https://user-images.githubusercontent.com/1309177/201257447-da94ca14-8d4e-4d7a-b8f7-0c6f92a14576.png">


---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-11 04:23_

---

_Closed by @charliermarsh on 2022-11-11 17:38_

---
