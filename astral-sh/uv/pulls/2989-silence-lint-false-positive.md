```yaml
number: 2989
title: Silence lint false positive
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/fix-warnings
created_at: 2024-04-11T09:36:14Z
updated_at: 2024-04-11T09:45:51Z
url: https://github.com/astral-sh/uv/pull/2989
synced_at: 2026-01-10T14:43:31Z
```

# Silence lint false positive

---

_Pull request opened by @konstin on 2024-04-11 09:36_

When running the `uv-client` tests, i would previously get:

```
warning: field `0` is never read
  --> crates/uv-configuration/src/config_settings.rs:43:27
   |
43 | pub struct ConfigSettings(BTreeMap<String, ConfigSettingValue>);
   |            -------------- ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |            |
   |            field in this struct
   |
   = note: `ConfigSettings` has derived impls for the traits `Clone` and `Debug`, but these are intentionally ignored during dead code analysis
   = note: `#[warn(dead_code)]` on by default
help: consider changing the field to be of unit type to suppress this warning while preserving the field numbering, or remove the field
   |
43 | pub struct ConfigSettings(());
   |                           ~~

warning: `uv-configuration` (lib) generated 1 warning
```


---

_Label `internal` added by @konstin on 2024-04-11 09:36_

---

_Merged by @konstin on 2024-04-11 09:45_

---

_Closed by @konstin on 2024-04-11 09:45_

---

_Branch deleted on 2024-04-11 09:45_

---
