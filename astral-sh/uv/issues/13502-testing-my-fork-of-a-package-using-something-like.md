```yaml
number: 13502
title: Testing my fork of a package using something like dependency overrides?
type: issue
state: closed
author: mattgiles
labels:
  - question
assignees: []
created_at: 2025-05-17T03:16:25Z
updated_at: 2025-05-19T16:57:36Z
url: https://github.com/astral-sh/uv/issues/13502
synced_at: 2026-01-12T16:01:30Z
```

# Testing my fork of a package using something like dependency overrides?

---

_@mattgiles_

### Question

Take the package [`dagster-dbt`](https://pypi.org/project/dagster-dbt/). This is a package which lives inside the [dagster monorepo](https://github.com/dagster-io/dagster/tree/master/python_modules/libraries/dagster-dbt). Using this library involves a lot of cross dependencies between different dagster packages.

I have a fork of the `dagster` repo that changes the code in `dagster-dbt`. I would love to just point uv at my github repo for this dependency, have everything in my dependency tree that needs `dagster-dbt` told to use my github repo, and drop-in test my patch.

This seems spiritually aligned with some of the other never-before-possible (or never-before-easy) capabilities of `uv`. Is it possible?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @mattgiles on 2025-05-17 03:16_

---

_Comment by @konstin on 2025-05-19 08:00_

There's two slightly different features: One is replacing just one package. You can do that by requiring `dagster-dbt @ git+https://.../my-fork/...`: A URL for a package replaces all requirements that only use a version. The other feature would be using `dagster-dbt` from the fork, and all its path dependencies from the fork, too. This only works if the repository is a uv workspace, due to limitations around path dependencies.

---

_Closed by @mattgiles on 2025-05-19 16:57_

---
