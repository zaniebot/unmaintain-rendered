```yaml
number: 7741
title: Enable environment variable authentication for named indexes
type: pull_request
state: merged
author: charliermarsh
labels:
  - registry
  - security
assignees: []
merged: true
base: main
head: charlie/index-api-environment-vars
created_at: 2024-09-27T17:01:34Z
updated_at: 2024-10-15T22:35:08Z
url: https://github.com/astral-sh/uv/pull/7741
synced_at: 2026-01-12T16:07:58Z
```

# Enable environment variable authentication for named indexes

---

_@charliermarsh_

## Summary

This PR enables users to provide index credentials via named environment variables.

For example, given an index named `internal` that requires a username (`public`) and password
(`koala`), you can define the index (without credentials) in your `pyproject.toml`:

```toml
[[tool.uv.index]]
name = "internal"
url = "https://pypi-proxy.corp.dev/simple"
```

Then set the `UV_INDEX_INTERNAL_USERNAME` and `UV_INDEX_INTERNAL_PASSWORD`
environment variables, where `INTERNAL` is the uppercase version of the index name:

```sh
export UV_INDEX_INTERNAL_USERNAME=public
export UV_INDEX_INTERNAL_PASSWORD=koala
```


---

_Review requested from @zanieb by @charliermarsh on 2024-09-27 17:01_

---

_Label `registry` added by @charliermarsh on 2024-09-27 17:01_

---

_Label `security` added by @charliermarsh on 2024-09-27 17:01_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:112 on 2024-09-30 14:20_

We should use `example.com`, I think.

---

_@zanieb reviewed on 2024-09-30 14:20_

---

_@zanieb approved on 2024-09-30 14:20_

---

_Merged by @charliermarsh on 2024-10-15 22:35_

---

_Closed by @charliermarsh on 2024-10-15 22:35_

---

_Branch deleted on 2024-10-15 22:35_

---
