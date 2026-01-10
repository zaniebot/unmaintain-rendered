```yaml
number: 12138
title: "Refactor *Settings to clarify and reduce repetition"
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: main
head: jtfm/test-settingsref
created_at: 2025-03-12T16:32:41Z
updated_at: 2025-03-13T08:10:32Z
url: https://github.com/astral-sh/uv/pull/12138
synced_at: 2026-01-10T11:10:39Z
```

# Refactor *Settings to clarify and reduce repetition

---

_Pull request opened by @jtfmumm on 2025-03-12 16:32_

When making changes to uv that require new (or altered) settings, there are many places in the code that need to change. This slows down work, reduces confidence in changes for new developers, and adds noise to PRs. The goal of this PR is to reduce the number of points that need to change (and that the developer needs to understand) when making changes to settings.

This PR consolidates `ResolverSettings` and `ResolverInstallerSettings` by factoring out the shared settings and using a new field `resolver_settings` on `ResolverInstallerSettings`. This not only reduces repetition, but makes it easier for a human to parse the code without having to compare long lists of fields to spot differences (the difference was that `ResolverInstallerSettings` had two extra fields). 

This also removes `ResolverSettingsRef` and `ResolverInstallerSettingsRef`, using normal Rust references instead. For the time being, I've left `InstallerSettingsRef` in place because it appears to have a semantic meaning that might be relied upon. However, it would now be straightforward to refactor to pass `&ResolverInstallerSettings` wherever `InstallerSettingsRef` appears, further reducing sprawl.

The change has the downside of adding `settings.resolver_settings.<field>` and requiring dereferencing at various points where it was not required before (with the *SettingsRef approach). But this means there are significantly fewer places that must change to update settings.


---

_Label `internal` added by @jtfmumm on 2025-03-12 16:32_

---

_Review requested from @charliermarsh by @jtfmumm on 2025-03-12 16:48_

---

_Review requested from @konstin by @jtfmumm on 2025-03-12 16:48_

---

_Review requested from @zanieb by @jtfmumm on 2025-03-12 16:48_

---

_@charliermarsh reviewed on 2025-03-12 21:06_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/build_frontend.rs`:178 on 2025-03-12 21:06_

Does this need to take an owned value?

---

_@charliermarsh reviewed on 2025-03-12 21:07_

---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:2494 on 2025-03-12 21:07_

I'd prefer to call this `resolver`, what do you think?

---

_@charliermarsh approved on 2025-03-12 21:07_

---

_Review comment by @jtfmumm on `crates/uv/src/settings.rs`:2494 on 2025-03-13 08:10_

`resolver_settings` is definitely clunky. I went back and forth but I've updated it to `resolver` now.

---

_@jtfmumm reviewed on 2025-03-13 08:10_

---

_@jtfmumm reviewed on 2025-03-13 08:10_

---

_Review comment by @jtfmumm on `crates/uv/src/commands/build_frontend.rs`:178 on 2025-03-13 08:10_

Fixed

---

_Merged by @jtfmumm on 2025-03-13 08:10_

---

_Closed by @jtfmumm on 2025-03-13 08:10_

---

_Branch deleted on 2025-03-13 08:10_

---
