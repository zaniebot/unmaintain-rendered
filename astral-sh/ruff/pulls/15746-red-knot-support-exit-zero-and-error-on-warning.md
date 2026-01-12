```yaml
number: 15746
title: "[red-knot] Support `--exit-zero` and `--error-on-warning`"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-cli-exit-codes
created_at: 2025-01-26T00:49:42Z
updated_at: 2025-02-03T19:14:47Z
url: https://github.com/astral-sh/ruff/pull/15746
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Support `--exit-zero` and `--error-on-warning`

---

_@InSyncWithFoo_

## Summary

Resolves astral-sh/ruff#15500, resolves astral-sh/ruff#15501.

Red Knot now accepts `--exit-zero` and `--error-on-warning` as CLI flags. The following table summarizes their effect on the exit code:

| Output               | No errors | At least one warning | At least one error | Panic |
|----------------------|:---------:|:--------------------:|:------------------:|:-----:|
| Default              |     0     |          0           |         1          |   2   |
| `--exit-zero`        |     0     |          0           |         0          |   2   |
| `--error-on-warning` |     0     |          1           |         1          |   2   |

## Test Plan

Unit tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-01-26 00:49_

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2025-01-26 00:49_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-26 00:49_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-01-26 00:49_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-26 00:52_

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/metadata/options.rs`:32 on 2025-02-02 10:53_

We should move the `error_on_warning` setting into a new `terminal` section so that it can be configured using 

```toml
[terminal]
error-on-warning = true
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/main.rs`:231 on 2025-02-02 10:59_

I think I'd rewrite this to determining the lowest allowed severity and then testing if there's any such diagnostic in the `self.watcher.is_none` branch

```
    let error_severity = if self.cli_options.error_on_warnings.unwrap_or_default() {
        Severity::Warning
    } else {
        Severity::Error
    };

    if results.iter().any(|diagnostic| diagnostic.severity() >= error_severity) {
        ExitStatus::Failure
    } else {
        ExitStatus::Success
    }
```

---

_@MichaReiser requested changes on 2025-02-02 11:01_

Thanks. We can't use `self.cli_options` in `main_loop`. We instead need to use `project.options` (ideally settings) to respect the same value being set in a configuration file. 

---

_@MichaReiser approved on 2025-02-03 07:31_

Thanks, this is great. 

I decided to revert the `Options` changes again and add configuration support as part of https://github.com/astral-sh/ruff/issues/15491. The reason I reverted is mainly that we shouldn't use `Options` outside the "reading and merging the configuration" phase. We should introduce a dedicated `Settings` struct, similar to what we have in Ruff, that stores the resolved `Options` (with defaults filled in)

---

_Merged by @MichaReiser on 2025-02-03 07:35_

---

_Closed by @MichaReiser on 2025-02-03 07:35_

---

_Branch deleted on 2025-02-03 19:14_

---
