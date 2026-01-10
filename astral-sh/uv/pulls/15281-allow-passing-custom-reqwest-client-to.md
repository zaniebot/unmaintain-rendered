```yaml
number: 15281
title: Allow passing custom reqwest client to RegistryClient
type: pull_request
state: merged
author: nilskch
labels:
  - rustlib
assignees: []
merged: true
base: main
head: nk/custom-reqwest-client
created_at: 2025-08-14T14:28:41Z
updated_at: 2025-08-14T17:06:41Z
url: https://github.com/astral-sh/uv/pull/15281
synced_at: 2026-01-10T06:44:33Z
```

# Allow passing custom reqwest client to RegistryClient

---

_Pull request opened by @nilskch on 2025-08-14 14:28_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
We are using UV as a library and we would like to provide an custom reqwest client to the `RegistryClient`/`BaseClient`. We have a central place in our repo where we configure the reqwest client to our needs (certs, proxy, ...) and it is safer for us to just pass the same client to UV rather than trying to reproduce the same client config with the APIs that UV exposes.

Are you ok with that change?


## Test Plan

<!-- How was it tested? -->


---

_Marked ready for review by @nilskch on 2025-08-14 14:42_

---

_@zanieb reviewed on 2025-08-14 14:49_

---

_Review comment by @zanieb on `crates/uv-client/src/base_client.rs`:164 on 2025-08-14 14:49_

nit: missing newline here

---

_Comment by @zanieb on 2025-08-14 14:50_

I'm pretty wary of this change but I don't have a strong reason to deny it. I'm mostly worried about the increased complexity in a very critical component of uv. Perhaps we could refactor this to avoid the big `if / else` branch somehow?

---

_Label `rustlib` added by @zanieb on 2025-08-14 14:50_

---

_Comment by @nilskch on 2025-08-14 15:24_

I moved the logic of creating the `raw_client` and `raw_dangerous_client` into a separate function.

I totally see your point. This would make our life much easier, but I would absolutely understand if you do not feel comfortable maintaining this change.

---

_@zanieb approved on 2025-08-14 16:56_

---

_Merged by @zanieb on 2025-08-14 17:00_

---

_Closed by @zanieb on 2025-08-14 17:00_

---

_Branch deleted on 2025-08-14 17:06_

---
