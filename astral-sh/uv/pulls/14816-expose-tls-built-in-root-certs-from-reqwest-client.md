```yaml
number: 14816
title: "expose `tls_built_in_root_certs` from reqwest client"
type: pull_request
state: merged
author: nilskch
labels:
  - rustlib
assignees: []
merged: true
base: main
head: nk/built-in-root-certs
created_at: 2025-07-22T16:18:44Z
updated_at: 2025-07-22T19:25:41Z
url: https://github.com/astral-sh/uv/pull/14816
synced_at: 2026-01-10T06:53:02Z
```

# expose `tls_built_in_root_certs` from reqwest client

---

_Pull request opened by @nilskch on 2025-07-22 16:18_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

We are using UV as a library and need to set `tls_built_in_root_certs` on the reqwest client.

This PR exposes this property in the `BaseClientBuilder` and in the `RegistryClientBuilder`. The default is set to `false`, so this does not change any behaviour unless you explicitly opt into it.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Marked ready for review by @nilskch on 2025-07-22 16:26_

---

_@zanieb approved on 2025-07-22 19:25_

---

_Label `rustlib` added by @zanieb on 2025-07-22 19:25_

---

_Merged by @zanieb on 2025-07-22 19:25_

---

_Closed by @zanieb on 2025-07-22 19:25_

---

_Branch deleted on 2025-07-22 19:25_

---
