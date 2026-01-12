```yaml
number: 7766
title: "crlf -> cr-lf in docs"
type: pull_request
state: merged
author: cberry31
labels:
  - documentation
assignees: []
merged: true
base: main
head: line-ending-docs
created_at: 2023-10-02T21:01:31Z
updated_at: 2023-10-02T21:15:36Z
url: https://github.com/astral-sh/ruff/pull/7766
synced_at: 2026-01-12T15:55:24Z
```

# crlf -> cr-lf in docs

---

_@cberry31_

## Summary
This change fixes an error in the documentation where cr-lf was displayed as crlf which if you tried to enter into the configuration file running ruff would break. 

## Test Plan
I ran the tests locally and I ran the documentation server locally and verified the edit 

### [Documentation Site](https://docs.astral.sh/ruff/settings/#format-line-ending)
![image](https://github.com/astral-sh/ruff/assets/50351006/8e63e49c-63ff-4027-a583-537c710e1305)

### Local
![image](https://github.com/astral-sh/ruff/assets/50351006/8845a235-8b2c-4157-99c8-908ee8f039b3)


---

_Label `documentation` added by @charliermarsh on 2023-10-02 21:03_

---

_Comment by @charliermarsh on 2023-10-02 21:03_

Thanks!

---

_@charliermarsh approved on 2023-10-02 21:06_

---

_Merged by @charliermarsh on 2023-10-02 21:09_

---

_Closed by @charliermarsh on 2023-10-02 21:09_

---

_Comment by @github-actions[bot] on 2023-10-02 21:15_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
