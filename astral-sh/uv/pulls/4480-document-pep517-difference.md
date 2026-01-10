```yaml
number: 4480
title: Document PEP517 difference
type: pull_request
state: merged
author: covracer
labels:
  - documentation
assignees: []
merged: true
base: main
head: pep517-differnce
created_at: 2024-06-24T17:54:13Z
updated_at: 2024-06-25T10:33:05Z
url: https://github.com/astral-sh/uv/pull/4480
synced_at: 2026-01-10T13:48:28Z
```

# Document PEP517 difference

---

_Pull request opened by @covracer on 2024-06-24 17:54_

## Summary
PEP 517 build isolation https://github.com/astral-sh/uv/pull/843 has not yet been mentioned in the PIP compatibility documentation. Add a section for it.

## Test Plan
Visual inspection only

## Open Questions
> in most cases, swapping out `pip install` for `uv pip install` should "just work".

In the first non-trivial case I tried, it worked for a short time and then [started failing](https://github.com/astral-sh/uv/issues/4069#issuecomment-2186762048). Is there any data out there on how many top 100 or top 1000 packages work with PEP 517 build isolation?

How can someone specify `--no-build-isolation` for just one package/line in `requirements.txt`?

---

_Comment by @charliermarsh on 2024-06-24 17:58_

Good question... My guess is that ~all of the top 100 and top 1000 packages work with PEP 517 build isolation, though I haven't tried it.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-24 18:01_

---

_@charliermarsh approved on 2024-06-24 20:32_

---

_Label `documentation` added by @charliermarsh on 2024-06-24 20:32_

---

_Merged by @charliermarsh on 2024-06-24 20:41_

---

_Closed by @charliermarsh on 2024-06-24 20:41_

---

_Branch deleted on 2024-06-25 10:33_

---
