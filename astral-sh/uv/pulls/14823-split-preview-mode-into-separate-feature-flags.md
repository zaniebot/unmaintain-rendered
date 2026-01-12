```yaml
number: 14823
title: Split preview mode into separate feature flags
type: pull_request
state: merged
author: zanieb
labels:
  - configuration
  - preview
assignees: []
merged: true
base: main
head: zb/preview-flags
created_at: 2025-07-22T17:15:37Z
updated_at: 2025-07-25T16:01:59Z
url: https://github.com/astral-sh/uv/pull/14823
synced_at: 2026-01-12T16:11:26Z
```

# Split preview mode into separate feature flags

---

_@zanieb_

I think this would give us better hygiene than a global flag. It makes it easier for users to opt-in to overlapping features, such as Python upgrades and Python bin installations and to disable warnings for preview mode without opting in to a bunch of other features. In general, I want to reduce the burden for putting something under preview.

The `--preview` and `--no-preview` flags are retained as global overrides. A new `--preview-features` option is added which accepts comma separated features or can be passed multiple times, e.g., `--preview-features add-bounds,pylock`. There's a `UV_PREVIEW_FEATURES` environment variable for that option (I'm not sure if we should overload `UV_PREVIEW`, but could be convinced).

---

_Comment by @zanieb on 2025-07-24 00:24_

Why in the world does this fail in CI but not locally?

```
        FAIL [   1.540s] uv::it show_settings::preview_features
  stdout ───

    running 1 test
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: preview_features
    Source: crates/uv/tests/it/show_settings.rs:7295
    ────────────────────────────────────────────────────────────────────────────────
    Expression: snapshot
    ────────────────────────────────────────────────────────────────────────────────
    -old snapshot
    +new results
    ────────────┬───────────────────────────────────────────────────────────────────
       84    84 │                 no_index: false,
       85    85 │             },
       86    86 │             index_strategy: FirstIndex,
       87    87 │             keyring_provider: Disabled,
       88       │-            link_mode: Clone,
             88 │+            link_mode: Hardlink,
       89    89 │             no_build_isolation: false,
       90    90 │             no_build_isolation_package: [],
       91    91 │             prerelease: IfNecessaryOrExplicit,
       92    92 │             resolution: Highest,
```

---

_Label `configuration` added by @zanieb on 2025-07-24 01:40_

---

_Label `preview` added by @zanieb on 2025-07-24 01:40_

---

_Marked ready for review by @zanieb on 2025-07-24 01:41_

---

_Comment by @zanieb on 2025-07-24 21:04_

I'll update the documentation separately.

---

_Review requested from @konstin by @zanieb on 2025-07-24 22:26_

---

_Review requested from @jtfmumm by @zanieb on 2025-07-24 22:26_

---

_Review comment by @konstin on `crates/uv-configuration/src/preview.rs`:125 on 2025-07-25 07:03_

This should be in `impl Display for PreviewFeatures`. 

---

_Review comment by @konstin on `crates/uv/src/commands/project/add.rs`:102 on 2025-07-25 07:05_

We should be consistent on whether we use the name inline or `as_str()`

---

_@konstin approved on 2025-07-25 07:07_

---

_@jtfmumm reviewed on 2025-07-25 11:16_

---

_Review comment by @jtfmumm on `crates/uv-cli/src/lib.rs`:289 on 2025-07-25 11:16_

```suggestion
    /// Use comma-separated values or pass multiple times to enable multiple features.
```

---

_@jtfmumm reviewed on 2025-07-25 11:22_

---

_Review comment by @jtfmumm on `crates/uv-configuration/src/preview.rs`:21 on 2025-07-25 11:22_

Should this `str` be capable of including more than one feature?

---

_@jtfmumm reviewed on 2025-07-25 11:24_

---

_Review comment by @jtfmumm on `crates/uv-configuration/src/preview.rs`:21 on 2025-07-25 11:24_

May be useful to have one or two unit tests

---

_@jtfmumm approved on 2025-07-25 11:25_

---

_@zanieb reviewed on 2025-07-25 12:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/add.rs`:102 on 2025-07-25 12:55_

I'd like to use `as_str`, thanks!

---

_@zanieb reviewed on 2025-07-25 15:51_

---

_Review comment by @zanieb on `crates/uv-configuration/src/preview.rs`:21 on 2025-07-25 15:51_

No, it can't be. I updated it to clarify that.

---

_Merged by @zanieb on 2025-07-25 16:01_

---

_Closed by @zanieb on 2025-07-25 16:01_

---

_Branch deleted on 2025-07-25 16:01_

---
