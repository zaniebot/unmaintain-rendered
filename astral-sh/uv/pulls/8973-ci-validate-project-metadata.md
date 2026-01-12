```yaml
number: 8973
title: "ci: validate project metadata"
type: pull_request
state: merged
author: mkniewallner
labels:
  - testing
assignees: []
merged: true
base: main
head: ci/validate-pyproject
created_at: 2024-11-09T14:04:14Z
updated_at: 2024-11-09T17:48:36Z
url: https://github.com/astral-sh/uv/pull/8973
synced_at: 2026-01-12T16:08:34Z
```

# ci: validate project metadata

---

_@mkniewallner_

## Summary

As per https://github.com/astral-sh/uv/pull/8943#discussion_r1835065562, adding a CI step to validate project metadata. Documentation for the tool: https://validate-pyproject.readthedocs.io/en/stable/readme.html. `store` is an extra that uses [this package](https://github.com/henryiii/validate-pyproject-schema-store) to get a weekly update of the schema in SchemaStore.

## Test Plan

Step passes on CI, and testing the same command locally while voluntarily using a wrong classifier fails:
```console
$ uvx --from 'validate-pyproject[all,store]' validate-pyproject pyproject.toml
Invalid file: pyproject.toml
[ERROR] `project.classifiers[5]` must be trove-classifier
```

---

_@charliermarsh approved on 2024-11-09 14:04_

---

_Marked ready for review by @mkniewallner on 2024-11-09 14:25_

---

_@zanieb approved on 2024-11-09 14:48_

---

_Merged by @zanieb on 2024-11-09 14:48_

---

_Closed by @zanieb on 2024-11-09 14:48_

---

_Label `testing` added by @zanieb on 2024-11-09 14:48_

---

_Branch deleted on 2024-11-09 17:48_

---
