```yaml
number: 1745
title: "feat: allow passing in a custom reqwest Client"
type: pull_request
state: merged
author: baszalmstra
labels:
  - enhancement
assignees: []
merged: true
base: main
head: feat/custom_client
created_at: 2024-02-20T10:46:39Z
updated_at: 2024-02-20T14:50:18Z
url: https://github.com/astral-sh/uv/pull/1745
synced_at: 2026-01-12T16:04:42Z
```

# feat: allow passing in a custom reqwest Client

---

_@baszalmstra_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I am looking to instantiate a `RegistryClient`. However, when using the `RegistryClientBuilder` a new reqwest client is always constructed. I would like to pass in a custom `reqwest::Client` to be able to share the http resources with other parts of my application.

## Test Plan

The uv codebase does not use my addition to the builder and all tests still succeed. And in my code I can pass a custom Client.


---

_@charliermarsh approved on 2024-02-20 14:49_

Thanks!

---

_@charliermarsh reviewed on 2024-02-20 14:50_

---

_Review comment by @charliermarsh on `crates/uv-client/src/registry_client.rs`:93 on 2024-02-20 14:50_

Perhaps mildly confusing that `UV_REQUEST_TIMEOUT` is ignored here, but not an issue in practice.

---

_Label `enhancement` added by @charliermarsh on 2024-02-20 14:50_

---

_Merged by @charliermarsh on 2024-02-20 14:50_

---

_Closed by @charliermarsh on 2024-02-20 14:50_

---
