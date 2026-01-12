```yaml
number: 711
title: Add pypi 10k packages with most dependents dataset
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/pypi_10k_most_dependents
created_at: 2023-12-20T11:01:13Z
updated_at: 2023-12-24T18:31:53Z
url: https://github.com/astral-sh/uv/pull/711
synced_at: 2026-01-12T16:04:08Z
```

# Add pypi 10k packages with most dependents dataset

---

_@konstin_

From manual inspection, this dataset generated through the [libraries.io API](https://libraries.io/api#project-search) seems more mainstream than the current 8k one, which is also preserved. I've added the dataset to the repo because the API requires an API key.

---

_Marked ready for review by @konstin on 2023-12-21 12:23_

---

_@charliermarsh approved on 2023-12-24 18:21_

---

_Label `internal` added by @charliermarsh on 2023-12-24 18:21_

---

_@charliermarsh approved on 2023-12-24 18:26_

I'm mixed on checking in the actual list of 10k packages, I'm going remove for now to err on the side of getting merged. The raw list continues to exist here, and I've linked it as a Gist in the notebook.

---

_Merged by @charliermarsh on 2023-12-24 18:31_

---

_Closed by @charliermarsh on 2023-12-24 18:31_

---

_Branch deleted on 2023-12-24 18:31_

---
