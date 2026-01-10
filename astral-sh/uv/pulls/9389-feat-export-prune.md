```yaml
number: 9389
title: "feat: export --prune"
type: pull_request
state: merged
author: dead10ck
labels:
  - enhancement
assignees: []
merged: true
base: main
head: export-prune
created_at: 2024-11-23T21:49:42Z
updated_at: 2024-11-24T02:15:51Z
url: https://github.com/astral-sh/uv/pull/9389
synced_at: 2026-01-10T12:00:00Z
```

# feat: export --prune

---

_Pull request opened by @dead10ck on 2024-11-23 21:49_

## Summary

This adds a `--prune` flag to the `export` command to correspond with the `--prune` flag of the `tree` command.

The purpose is for generating a `requirements.txt` that omits a package and all of that package's unique dependencies. This is useful for cases where the project has a dependency on a common core package, but where that package does not need to be installed in the target environment.

For example, a pyspark job needs spark for development, but when installing into a cluster that already has pyspark installed, it is desirable to omit pyspark's whole dependency tree so that only the unique dependencies that your job needs get installed, and do not risk breaking the pyspark dependencies with something incompatible.

Dev groups cannot always cover this case because there are other projects where this common dependency occurs as a transitive. One example is Airflow providers, which include Airflow itself as a dependency, but it is unnecessary and undesirable to include Airflow's dependency tree in the `requirements.txt` for your DAGs.

Partly related to #7214, though I'm not sure it covers the ask in that one of having this functionality extend to the project's actual published metadata.


## Test Plan

An integration test was added, and some manual testing. Let me know if more would be better.


---

_@charliermarsh approved on 2024-11-24 01:55_

This makes sense to me, thanks.

---

_Label `enhancement` added by @charliermarsh on 2024-11-24 01:55_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-24 01:55_

---

_Merged by @charliermarsh on 2024-11-24 02:11_

---

_Closed by @charliermarsh on 2024-11-24 02:11_

---

_Comment by @charliermarsh on 2024-11-24 02:15_

Great PR!

---
