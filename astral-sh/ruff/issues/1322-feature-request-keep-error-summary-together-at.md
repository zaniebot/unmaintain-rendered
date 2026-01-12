```yaml
number: 1322
title: "[feature-request] Keep error summary together at bottom of error list"
type: issue
state: closed
author: smackesey
labels: []
assignees: []
created_at: 2022-12-22T01:30:13Z
updated_at: 2022-12-22T02:04:27Z
url: https://github.com/astral-sh/ruff/issues/1322
synced_at: 2026-01-12T15:54:41Z
```

# [feature-request] Keep error summary together at bottom of error list

---

_@smackesey_

When I run `ruff` with a subset of fixable errors, I get output like this:

```
Found 62 error(s).
dagster-cloud/integration_tests/python_modules/dagster-cloud-test-infra/dagster_cloud_test_infra/pex/sample-repos/repo-with-partition-set/repo1/repo1/__init__.py:1:25: F401 `repository.repo1` imported but unused
dagster-cloud/integration_tests/test_suites/kubernetes_dagster_cloud/test_kubernetes.py:137:31: F811 Redefinition of unused `dagster_cloud_most_recent_release` from line 31
... 60 more lines ...
11 potentially fixable with the --fix option.
```

IMO it's a bit confusing to have the number of fixable errors at the bottom and the total at the top (especially if there are a lot of errors and you have to scroll to the get the total)-- they should both be at the bottom.

---

_Comment by @charliermarsh on 2022-12-22 02:04_

Yeah, agree.

---

_Closed by @charliermarsh on 2022-12-22 02:04_

---
