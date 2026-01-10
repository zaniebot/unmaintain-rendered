```yaml
number: 628
title: Make warnings user-facing
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/warnings
created_at: 2023-12-13T01:53:22Z
updated_at: 2023-12-13T08:18:47Z
url: https://github.com/astral-sh/uv/pull/628
synced_at: 2026-01-10T15:44:44Z
```

# Make warnings user-facing

---

_Pull request opened by @charliermarsh on 2023-12-13 01:53_

## Summary

Now, `puffin_warnings::warn_once` and `puffin_warnings::warn` will go to `stderr`, as long as the user isn't running under `--quiet`. Previously, these went through `tracing`, and so were only visible when running under `--verbose`.

---

_Review requested from @zanieb by @charliermarsh on 2023-12-13 01:53_

---

_Review requested from @konstin by @charliermarsh on 2023-12-13 01:53_

---

_@zanieb approved on 2023-12-13 02:03_

---

_Merged by @charliermarsh on 2023-12-13 02:24_

---

_Closed by @charliermarsh on 2023-12-13 02:24_

---

_Branch deleted on 2023-12-13 02:24_

---

_@konstin reviewed on 2023-12-13 08:16_

---

_Review comment by @konstin on `crates/puffin-resolver/src/pubgrub/dependencies.rs`:33 on 2023-12-13 08:16_

Why aren't these `warn_once` anymore?

---

_@konstin reviewed on 2023-12-13 08:18_

---

_Review comment by @konstin on `crates/puffin-cli/src/main.rs`:370 on 2023-12-13 08:18_

It would be great if the warnings would go to tracing again if `RUST_LOG` is set.

---
