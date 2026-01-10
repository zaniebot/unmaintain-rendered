```yaml
number: 16645
title: "feat(cli): expose `UV_NO_DEFAULT_GROUPS` environment variable"
type: pull_request
state: merged
author: mkniewallner
labels:
  - configuration
assignees: []
merged: true
base: main
head: feat/expose-uv-no-default-groups-env-var
created_at: 2025-11-09T00:06:47Z
updated_at: 2025-11-10T21:34:16Z
url: https://github.com/astral-sh/uv/pull/16645
synced_at: 2026-01-10T06:28:12Z
```

# feat(cli): expose `UV_NO_DEFAULT_GROUPS` environment variable

---

_Pull request opened by @mkniewallner on 2025-11-09 00:06_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Similarly to #16529 that adds `UV_NO_GROUP`, this adds `UV_NO_DEFAULT_GROUPS` that does the same as `--no-default-groups`. This can be useful on the CI, to disable default groups on a job without having to set the argument in all commands that could trigger a sync (for instance [here](https://github.com/fpgmaas/deptry/blob/8757b318e9974bbfa7ec65dabf999bc935ac026f/.github/workflows/main.yml#L105-L116)).

## Test Plan

Snapshot tests.

---

_Marked ready for review by @mkniewallner on 2025-11-09 00:24_

---

_@samypr100 reviewed on 2025-11-09 02:47_

---

_Review comment by @samypr100 on `crates/uv-static/src/env_vars.rs`:247 on 2025-11-09 02:47_

```suggestion
    #[attr_added_in("next release")]
```

Using "next release" will automatically take care of replacing it with the next release version whichever that may be

---

_@samypr100 reviewed on 2025-11-09 02:48_

---

_Review comment by @samypr100 on `crates/uv/tests/it/sync.rs`:4474 on 2025-11-09 02:48_

```suggestion
    uv_snapshot!(context.filters(), context.sync().env(EnvVars::UV_NO_DEFAULT_GROUPS, "true"), @r"
```

---

_Review comment by @samypr100 on `crates/uv/tests/it/sync.rs`:4515 on 2025-11-09 02:48_

```suggestion
        .env(EnvVars::UV_NO_DEFAULT_GROUPS, "true"), @r"
```

---

_@samypr100 reviewed on 2025-11-09 02:48_

---

_@samypr100 reviewed on 2025-11-09 02:48_

---

_Review comment by @samypr100 on `crates/uv/tests/it/sync.rs`:4575 on 2025-11-09 02:48_

```suggestion
    uv_snapshot!(context.filters(), context.sync().env(EnvVars::UV_NO_DEFAULT_GROUPS, "true"), @r"
```

---

_@zanieb approved on 2025-11-10 20:43_

---

_Merged by @zanieb on 2025-11-10 20:43_

---

_Closed by @zanieb on 2025-11-10 20:43_

---

_Label `configuration` added by @zanieb on 2025-11-10 20:43_

---

_Branch deleted on 2025-11-10 21:34_

---
