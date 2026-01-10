```yaml
number: 14368
title: Workaround for panic due to missing global validation in clap
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/dont-crash-with-boolean-flags
created_at: 2025-06-30T07:53:24Z
updated_at: 2025-07-01T18:39:48Z
url: https://github.com/astral-sh/uv/pull/14368
synced_at: 2026-01-10T06:53:01Z
```

# Workaround for panic due to missing global validation in clap

---

_Pull request opened by @konstin on 2025-06-30 07:53_

Clap does not perform global validation, so flag that are declared as overriding can be set at the same time: https://github.com/clap-rs/clap/issues/6049. This would previously cause a panic. We work around this by choosing the yes-value always and writing a warning.

An alternative would be erroring when both are set, but it's unclear to me if this may break things we want to support. (`UV_OFFLINE=1 cargo run -q pip --no-offline install tqdm --no-cache` is already banned).

Fixes https://github.com/astral-sh/uv/pull/14299

**Test Plan**

```
$ cargo run -q pip --offline install --no-offline tqdm --no-cache
  warning: Boolean flags on different levels are not correctly supported (https://github.com/clap-rs/clap/issues/6049)
    × No solution found when resolving dependencies:
    ╰─▶ Because tqdm was not found in the cache and you require tqdm, we can conclude that your requirements are unsatisfiable.

        hint: Packages were unavailable because the network was disabled. When the network is disabled, registry packages may only be read from the cache.
```

---

_Label `bug` added by @konstin on 2025-06-30 07:53_

---

_@zanieb reviewed on 2025-06-30 14:50_

---

_Review comment by @zanieb on `crates/uv-cli/src/options.rs`:23 on 2025-06-30 14:50_

I think we want `None` so the default is chosen? I think I prefer the hard error right now though, tbh, the behavior will just be wrong.

---

_Comment by @zanieb on 2025-06-30 14:50_

cc @Gankra as this was one of your goals for the month

---

_@Gankra reviewed on 2025-06-30 20:06_

---

_Review comment by @Gankra on `crates/uv-cli/src/options.rs`:23 on 2025-06-30 20:06_

I agree a hard error is preferable.

---

_@konstin reviewed on 2025-07-01 12:06_

---

_Review comment by @konstin on `crates/uv-cli/src/options.rs`:31 on 2025-07-01 12:06_

Given that this is a clap bug, I don't think it's worth it introducing `Result`s in the whole resolve chain.

The exit code is the same that clap uses for parsing errors.

---

_@zanieb approved on 2025-07-01 12:31_

---

_Merged by @zanieb on 2025-07-01 18:39_

---

_Closed by @zanieb on 2025-07-01 18:39_

---

_Branch deleted on 2025-07-01 18:39_

---
