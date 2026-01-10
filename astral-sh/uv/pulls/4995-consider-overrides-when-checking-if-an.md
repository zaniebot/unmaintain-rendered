```yaml
number: 4995
title: Consider overrides when checking if an environment satisfies requirements
type: pull_request
state: closed
author: zanieb
labels:
  - enhancement
assignees: []
draft: true
base: main
head: zb/override-satisfies
created_at: 2024-07-11T17:24:55Z
updated_at: 2024-07-19T20:48:49Z
url: https://github.com/astral-sh/uv/pull/4995
synced_at: 2026-01-10T13:42:52Z
```

# Consider overrides when checking if an environment satisfies requirements

---

_Pull request opened by @zanieb on 2024-07-11 17:24_

I noticed this while implementing #4973 — it seems like an oversight.

---

_Label `enhancement` added by @zanieb on 2024-07-11 17:29_

---

_Marked ready for review by @zanieb on 2024-07-11 17:29_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-11 20:44_

---

_Review requested from @ibraheemdev by @zanieb on 2024-07-11 20:44_

---

_@ibraheemdev reviewed on 2024-07-11 20:48_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/install.rs`:178 on 2024-07-11 20:48_

Looks like `overrides` is empty here?

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/install.rs`:178 on 2024-07-11 21:15_

Ah interesting, I just updated all the call-sites. Maybe this behavior isn't changed at all in `uv pip` at all then — it just always did a full resolution when any overrides were provided so they were respected.

---

_@zanieb reviewed on 2024-07-11 21:15_

---

_@charliermarsh reviewed on 2024-07-12 00:45_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/pip/install.rs`:178 on 2024-07-12 00:45_

Yeah I think `satisfies` doesn't quite work with overrides as-is.

---

_@charliermarsh reviewed on 2024-07-12 00:45_

---

_Review comment by @charliermarsh on `crates/uv-installer/src/site_packages.rs`:323 on 2024-07-12 00:45_

I think we'd need to change the code such that all _dependencies_ also respect overrides.

---

_@zanieb reviewed on 2024-07-12 03:35_

---

_Review comment by @zanieb on `crates/uv-installer/src/site_packages.rs`:323 on 2024-07-12 03:35_

Yeah I noticed this in my test at https://github.com/astral-sh/uv/pull/4973/files#diff-a7df5406f8301d2dd603b5355f282be5afa0198ff71f03144ab6d8aed56c07a3R713-R719. I'll explore fixing this before merging here. I'm not sure how I'll write a nice test case though without the `uv run` changes — I guess I could call `SitePackages::satisfies` directly in a test instead of going through the CLI. Or maybe I can drop the `overrides.is_empty()` shortcut on `pip install`.

---

_Converted to draft by @zanieb on 2024-07-15 20:16_

---

_Comment by @zanieb on 2024-07-19 20:24_

It turns out this is quite complicated because overrides can be unnamed which isn't compatible with the current design of `satisfies` — we cannot check that the override is respected without significant changes.

It also looks like there's a bug in the constraint enforcement in this method, where it does not check that the constraint name matches the installed distribution name. Addressed in #5231 

---

_Closed by @zanieb on 2024-07-19 20:24_

---
