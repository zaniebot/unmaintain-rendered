```yaml
number: 1321
title: Infer package roots when running via stdin
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/init
created_at: 2022-12-22T01:27:22Z
updated_at: 2022-12-22T01:30:12Z
url: https://github.com/astral-sh/ruff/pull/1321
synced_at: 2026-01-12T15:55:06Z
```

# Infer package roots when running via stdin

---

_@charliermarsh_

Resolves: #1319.

## Test Plan

Ran `cat python_modules/libraries/dagster-aws/dagster_aws/emr/emr.py | ../ruff/target/debug/ruff --select=I001 --stdin-filename python_modules/libraries/dagster-aws/dagster_aws/emr/emr.py --fix -` from `sean/ruff` in the Dagster repo.

Verified that `dagster_aws` was classified as first-party:

![Screen Shot 2022-12-21 at 8 28 29 PM](https://user-images.githubusercontent.com/1309177/209035028-78b1e547-d2b5-4523-a41e-1803be69b6e5.png)

Omitted the filename, by running `cat python_modules/libraries/dagster-aws/dagster_aws/emr/emr.py | ../ruff/target/debug/ruff --select=I001 --fix -` from `sean/ruff` in the Dagster repo.

Verified that `dagster_aws` was classified as third-party:

![Screen Shot 2022-12-21 at 8 29 54 PM](https://user-images.githubusercontent.com/1309177/209035106-fd358305-1001-4bf9-bf8c-095dd7fd6d27.png)


---

_Merged by @charliermarsh on 2022-12-22 01:30_

---

_Closed by @charliermarsh on 2022-12-22 01:30_

---

_Branch deleted on 2022-12-22 01:30_

---
