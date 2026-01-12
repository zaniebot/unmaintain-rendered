```yaml
number: 4515
title: "Unify dependency iteration in `ResolverState::get_dependencies`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/unify-dependency-iteration-in-get-dependencies
created_at: 2024-06-25T13:59:11Z
updated_at: 2024-06-25T21:05:20Z
url: https://github.com/astral-sh/uv/pull/4515
synced_at: 2026-01-12T16:06:17Z
```

# Unify dependency iteration in `ResolverState::get_dependencies`

---

_@konstin_

Upstack PR: #4430

Split out from #4430 according to https://github.com/astral-sh/uv/pull/4430#discussion_r1650192338.


---

_@konstin reviewed on 2024-06-25 14:00_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:965 on 2024-06-25 14:00_

@charliermarsh Do you know what this was about? It's only in this branch (in the non-root package branch we short-circuit on error) and the errors we're handling here look like they should be raised.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:965 on 2024-06-25 19:45_

Why should they be raised? You don't think we should backtrack in these cases?

---

_@charliermarsh reviewed on 2024-06-25 19:45_

---

_@konstin reviewed on 2024-06-25 19:54_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:965 on 2024-06-25 19:54_

As far as i can tell, the current implementation means that if there is a direct dependency (a root package dependency) where `PubGrubSpecifier::try_from` fails or which uses a banned URL, we mark those dependencies as unavailable, so the root package becomes unavailable(?). For regular dependencies, we already raise those errors directly using `from_requirements(...)?`.

This code looks like an artifact to me, but going through the git history unfortunately didn't clear this up either.

---

_@charliermarsh reviewed on 2024-06-25 20:22_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:965 on 2024-06-25 20:22_

It looks like there are _some_ errors that we'd want to recover from though, like `InvalidVersion` or perhaps `PubGrubSpecifierError`?

---

_@konstin reviewed on 2024-06-25 20:37_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:965 on 2024-06-25 20:37_

I don't think they are recoverable, as far as i can tell they lead to the root package being rejected in the next step, so you get the same error except it's wrapped in a message about the root package being unavailable.

---

_@konstin reviewed on 2024-06-25 20:37_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:965 on 2024-06-25 20:37_

We also do this only for the root package, which looks wrong.

---

_@charliermarsh reviewed on 2024-06-25 20:40_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:965 on 2024-06-25 20:40_

Where are they rejected? Note that this does not return an error; it returns `Ok(Dependencies::Unavailable)`.

---

_Merged by @konstin on 2024-06-25 21:04_

---

_Closed by @konstin on 2024-06-25 21:04_

---

_Branch deleted on 2024-06-25 21:04_

---

_@konstin reviewed on 2024-06-25 21:05_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:965 on 2024-06-25 21:05_

This code is unreachable (both error cases are raised earlier), so i'm removing it

---
