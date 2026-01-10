```yaml
number: 9112
title: Include version constraints in derivation chains
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/chain-ii
created_at: 2024-11-14T04:16:28Z
updated_at: 2024-11-15T20:06:26Z
url: https://github.com/astral-sh/uv/pull/9112
synced_at: 2026-01-10T12:00:00Z
```

# Include version constraints in derivation chains

---

_Pull request opened by @charliermarsh on 2024-11-14 04:16_

## Summary

Derivation chains can now include the versions at which a package was requested.


---

_Label `error messages` added by @charliermarsh on 2024-11-14 04:16_

---

_@charliermarsh reviewed on 2024-11-14 04:16_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/diagnostics.rs`:353 on 2024-11-14 04:16_

I need to DRY this up with the error reporting in the resolver.

---

_@charliermarsh reviewed on 2024-11-14 04:17_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/derivation.rs`:58 on 2024-11-14 04:17_

This is somewhat confusing but it is the most normalized representation...

---

_@zanieb reviewed on 2024-11-14 05:01_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:19885 on 2024-11-14 05:01_

I'm not sure if the parenthesis are helpful for readability here. Is there a particular reason you went with that approach instead of `wsgiref==0.1.2`?

---

_@charliermarsh reviewed on 2024-11-15 02:27_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19885 on 2024-11-15 02:27_

In prior entries, I want to show both the selected version and the version range -- like `django==1.4.0 (>=1.0.0)`. So it seemed nice to be consistent with the use of parentheses here?

---

_@charliermarsh reviewed on 2024-11-15 02:27_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19885 on 2024-11-15 02:27_

Maybe it should be like: `django v1.4.0 (>=1.0.0)`?

---

_Review requested from @zanieb by @charliermarsh on 2024-11-15 02:55_

---

_Marked ready for review by @charliermarsh on 2024-11-15 02:55_

---

_@charliermarsh reviewed on 2024-11-15 02:56_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/error.rs`:912 on 2024-11-15 02:56_

Just moved and restructured to make it public.

---

_@charliermarsh reviewed on 2024-11-15 02:58_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19885 on 2024-11-15 02:58_

There are like three different cases to consider in the chain:

- The first dependency (`project==0.1.0`, which doesn't have a set of constraints, because it's just attached to the root).
- Intermediary dependencies (not visible here), which have both a pinned version and a set of constraints, so we want to show both (like, `django==1.4.0 (>=1.0.0)`).
- The final dependency (`wsgiref (==0.1.2)`), which we don't show the version for just to be succinct, but could.

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:19885 on 2024-11-15 18:05_

Interesting, thanks for explaining. I'm happy with whatever you do here. `django v1.4.0 (>=1.0.0)` seems easier to understand.

Regarding the root package, we might want to do something more consistent with the resolver reporter https://github.com/astral-sh/uv/blob/744a9091a2a35b6b3f4e5f8f2a3394e7261f7e14/crates/uv-resolver/src/pubgrub/report.rs#L341-L355 

---

_@zanieb reviewed on 2024-11-15 18:05_

---

_@zanieb reviewed on 2024-11-15 18:10_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:19885 on 2024-11-15 18:10_

Sorry looking at the snapshots again..

> help: `wsgiref` was included because `project==0.1.0` depends on `wsgiref (>=0.1)`

If the language is "depends on" I would expect the version specifiers to be first-class? e.g.

> help: `wsgiref v0.1` was included because `project==0.1.0` depends on `wsgiref>=0.1`

The chain case is a bit tricky, because we then say "which depends on" in which the concrete version needs to be first-class. e.g.,

> help: `wsgiref` was included because `project==0.1.0` depends on `child==0.1.0 (*)` which depends on `wsgiref (*)`

In this case perhaps we can say:

> help: `wsgiref` was included because `project==0.1.0` depends on `child` and `child==0.1.0` depends on `wsgiref`

I don't love the increase in verbosity though it's hard to say how often we'll get a particularly long chain.  Another option is to put the concrete version inside the parenthesis:

> help: `wsgiref` was included because `project==0.1.0` depends on `child (v0.1.0)` which depends on `wsgiref`

For a pretend case with a specifier:

> help: `wsgiref` was included because `project==0.1.0` depends on `child>=0.1.0 (v0.1.0)` which depends on `wsgiref`

Maybe more context is the move?

> help: `wsgiref` was included because `project==0.1.0` depends on `child>=0.1.0 (selected: v0.1.0)` which depends on `wsgiref`

---

_@zanieb reviewed on 2024-11-15 18:11_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:19885 on 2024-11-15 18:11_

I'd omit the `*` throughout, I don't think we usually show that to users.

---

_@charliermarsh reviewed on 2024-11-15 18:34_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19885 on 2024-11-15 18:34_

I'm kind of into `wsgiref was included because project v0.1.0 depends on child>=0.1.0 and child v0.1.4 depends on wsgiref`

---

_@charliermarsh reviewed on 2024-11-15 18:34_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19885 on 2024-11-15 18:34_

Actually the last one you gave isn't bad either.

---

_@zanieb reviewed on 2024-11-15 19:17_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:19885 on 2024-11-15 19:17_

Whatever you're more into — we can iterate later if we need to.

---

_@charliermarsh reviewed on 2024-11-15 19:35_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/lock.rs`:19885 on 2024-11-15 19:35_

Okay, I went with:

```
help: `wsgiref` (v0.1.2) was included because `project` (v0.1.0) depends on `child` (v0.1.0) which depends on `wsgiref>=0.1.2`
```

---

_@zanieb approved on 2024-11-15 19:42_

---

_Merged by @charliermarsh on 2024-11-15 20:06_

---

_Closed by @charliermarsh on 2024-11-15 20:06_

---

_Branch deleted on 2024-11-15 20:06_

---
