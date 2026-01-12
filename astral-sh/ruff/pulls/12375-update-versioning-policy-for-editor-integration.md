```yaml
number: 12375
title: Update versioning policy for editor integration
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
assignees: []
merged: true
base: main
head: dhruv/versioning
created_at: 2024-07-18T10:15:43Z
updated_at: 2024-07-18T15:17:38Z
url: https://github.com/astral-sh/ruff/pull/12375
synced_at: 2026-01-12T15:55:41Z
```

# Update versioning policy for editor integration

---

_@dhruvmanila_

## Summary

Following the stabilization of the Ruff language server, we need to update our versioning policy to account for any changes in it. This could be server settings, capability, etc.

This PR also adds a new section for the VS Code extension which is adopted from [Biome's versioning policy](https://biomejs.dev/internals/versioning/#visual-studio-code-extension) for the same.

---

_Label `documentation` added by @dhruvmanila on 2024-07-18 10:15_

---

_Marked ready for review by @dhruvmanila on 2024-07-18 10:50_

---

_@T-256 reviewed on 2024-07-18 10:54_

---

_Review comment by @T-256 on `docs/versioning.md`:90 on 2024-07-18 10:54_

Update examples to currently used numbers:
```suggestion
Stable releases use even numbers in minor part of versions: 2024.30.0, 2024.32.0, 2024.34.0, …
Previews use odd numbers in minor part of versions: 2024.31.0, 2024.33.0, 2024.35.0, …
```

---

_Comment by @dhruvmanila on 2024-07-18 10:59_

I initially thought that adding a new capability shouldn't require a minor bump but maybe that's not true. What if we add completions and then when a user upgrades the Ruff version, the editor will start showing completions from Ruff, a lot of which might be duplicates from an existing language server.

Edit: I think for now this is fine.

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-07-18 11:53_

---

_Review requested from @zanieb by @MichaReiser on 2024-07-18 12:10_

---

_Review comment by @MichaReiser on `docs/versioning.md`:27 on 2024-07-18 12:15_

I think this is overly strict and at the same time not specific enough. Adding new lint rules changes the lint capability. 

I also think that it's fine to add new actions to the *code action* capability. 

I wonder if we should remove this for now and limit it to the removal of capabilities or settings. 

---

_@MichaReiser approved on 2024-07-18 12:15_

---

_Review comment by @dhruvmanila on `docs/versioning.md`:27 on 2024-07-18 15:06_

Yeah, I think that makes sense. It's unclear what kind of restrictions we want right now so I'll remove this part now.

---

_@dhruvmanila reviewed on 2024-07-18 15:06_

---

_@dhruvmanila reviewed on 2024-07-18 15:07_

---

_Review comment by @dhruvmanila on `docs/versioning.md`:27 on 2024-07-18 15:07_

Thanks for the feedback!

---

_@zanieb reviewed on 2024-07-18 15:12_

---

_Review comment by @zanieb on `docs/versioning.md`:86 on 2024-07-18 15:12_

```suggestion
for extensions. Consequently, Ruff uses the following scheme to distinguish between stable and
preview releases:
```

---

_@zanieb reviewed on 2024-07-18 15:13_

---

_Review comment by @zanieb on `docs/versioning.md`:89 on 2024-07-18 15:13_

```suggestion
Stable releases use even numbers in minor version component: `2024.30.0`, `2024.32.0`, `2024.34.0`, …
Preview releases use odd numbers in minor version component: `2024.31.0`, `2024.33.0`, `2024.35.0`, …
```

---

_@zanieb approved on 2024-07-18 15:13_

---

_Merged by @dhruvmanila on 2024-07-18 15:17_

---

_Closed by @dhruvmanila on 2024-07-18 15:17_

---

_Branch deleted on 2024-07-18 15:17_

---
