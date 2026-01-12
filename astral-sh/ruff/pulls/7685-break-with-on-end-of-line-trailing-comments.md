```yaml
number: 7685
title: "Break `with` on end-of-line trailing comments"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/with-comment
created_at: 2023-09-28T00:07:47Z
updated_at: 2023-09-28T12:45:26Z
url: https://github.com/astral-sh/ruff/pull/7685
synced_at: 2026-01-12T02:39:10Z
```

# Break `with` on end-of-line trailing comments

---

_Pull request opened by @charliermarsh on 2023-09-28 00:07_

## Summary

Ensures that:

```python
with (
    a  # comment
):
    pass
```

Retains its parentheses.

Closes https://github.com/astral-sh/ruff/issues/6750.

## Test Plan

`cargo test`

No change in similarity.


---

_Label `formatter` added by @charliermarsh on 2023-09-28 00:08_

---

_Merged by @charliermarsh on 2023-09-28 00:16_

---

_Closed by @charliermarsh on 2023-09-28 00:16_

---

_Branch deleted on 2023-09-28 00:16_

---

_Comment by @MichaReiser on 2023-09-28 06:38_

Does this change the similarity index in anyway? I checked. The similarity index remains unchanged.

---

_Comment by @charliermarsh on 2023-09-28 12:45_

I checked too, it’s in the PR summary :)

---
