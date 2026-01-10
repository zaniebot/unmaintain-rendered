```yaml
number: 5122
title: Add reference documentation for resolver settings
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/api-reference
created_at: 2024-07-16T19:20:47Z
updated_at: 2024-07-16T20:39:24Z
url: https://github.com/astral-sh/uv/pull/5122
synced_at: 2026-01-10T13:42:52Z
```

# Add reference documentation for resolver settings

---

_Pull request opened by @charliermarsh on 2024-07-16 19:20_

## Summary

First part of https://github.com/astral-sh/uv/issues/5093.

Remaining:

- Global settings
- `pip`-specific settings (some will be copied-over from here)
- Auto-generating the "Possible values" for enums


---

_Review requested from @zanieb by @charliermarsh on 2024-07-16 19:20_

---

_Label `documentation` added by @charliermarsh on 2024-07-16 19:20_

---

_Marked ready for review by @charliermarsh on 2024-07-16 19:20_

---

_@charliermarsh reviewed on 2024-07-16 19:21_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:194 on 2024-07-16 19:21_

Stopped adding these because I'm going to explore auto-generating them in the macro (separately).

---

_@zanieb reviewed on 2024-07-16 20:04_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2275 on 2024-07-16 20:04_

Should we reference the index strategy here?

---

_@zanieb reviewed on 2024-07-16 20:05_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:47 on 2024-07-16 20:05_

Just wondering, why this change? Should we reference `ruff` instead?

---

_@charliermarsh reviewed on 2024-07-16 20:06_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:47 on 2024-07-16 20:06_

I dunno, just thought I would use something a little more modern, but we can do whatever. Ruff seems good yeah.

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:205 on 2024-07-16 20:06_

Do we need this if covered in the `option` macro?

---

_@zanieb reviewed on 2024-07-16 20:06_

---

_@zanieb reviewed on 2024-07-16 20:08_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:205 on 2024-07-16 20:08_

per https://github.com/astral-sh/uv/pull/5122/files#diff-cd5ab0390b5750a658337173d28e15c9631a156470fb8036633f3fefc4bf96a4R225-R227 this looks redundant

---

_@zanieb approved on 2024-07-16 20:08_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:47 on 2024-07-16 20:08_

I like to use Ruff when it's okay to have something that's not tiny.

---

_@zanieb reviewed on 2024-07-16 20:08_

---

_Merged by @charliermarsh on 2024-07-16 20:39_

---

_Closed by @charliermarsh on 2024-07-16 20:39_

---

_Branch deleted on 2024-07-16 20:39_

---
