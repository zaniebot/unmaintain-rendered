```yaml
number: 6979
title: Use nextest to run tests in CI
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: main
head: nextest
created_at: 2023-08-29T14:45:51Z
updated_at: 2023-08-29T19:23:08Z
url: https://github.com/astral-sh/ruff/pull/6979
synced_at: 2026-01-12T15:55:23Z
```

# Use nextest to run tests in CI

---

_@zanieb_

_No description provided._

---

_Comment by @zanieb on 2023-08-29 15:12_

`cargo insta test` doesn't properly pass arguments to the test runner as [their documentation describes](https://insta.rs/docs/cli/#test) e.g.

```
❯ cargo insta test --all --all-features --unreferenced reject --test-runner nextest -- --status-level skip
error: failed to parse test binary arguments `--status-level`: arguments are unsupported
```

Which means that if we want to use `nextest` then we have to choose if we want to pass arguments to `nextest` _or_ `insta` and insta does not appear to provide any other way to configure the `--unreferenced` option so we'd lose that functionaliy.

---

_Comment by @zanieb on 2023-08-29 15:33_

I can resolve that problem with https://github.com/zanieb/insta/commit/5bde316c6f11024b0d1549b8de5c91afb5fabc6a, I guess I'll raise upstream

---

_Comment by @zanieb on 2023-08-29 19:23_

Requires https://github.com/mitsuhiko/insta/issues/381 or dropping use of `--unreferenced` I think.

---

_Closed by @zanieb on 2023-08-29 19:23_

---
