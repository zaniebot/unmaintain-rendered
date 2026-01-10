```yaml
number: 319
title: Require URL dependencies to be declared upfront
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/strict-dependency
created_at: 2023-11-04T19:31:26Z
updated_at: 2023-11-05T17:09:59Z
url: https://github.com/astral-sh/uv/pull/319
synced_at: 2026-01-10T15:50:28Z
```

# Require URL dependencies to be declared upfront

---

_Pull request opened by @charliermarsh on 2023-11-04 19:31_

In the resolver, our current model for solving URL dependencies requires that we visit the URL dependency _before_ the registry-based dependency. This PR encodes a strict requirement that all URL dependencies be declared upfront, either as requirements or constraints.

I wrote more about how it works and why it's necessary in documentation [here](https://github.com/astral-sh/puffin/pull/319/files#diff-2b1c4f36af0c62a2b7bebeae9473ae083588f2a6b18a3ec52393a24266adecbbR20). I think we could relax this constraint over time, but it requires a more sophisticated model -- and for now, I just want something that's (1) correct, (2) easy for us to reason about, and (3) easy for users to reason about.

As additional motivation... allowing arbitrary URL dependencies anywhere in the tree creates some really confusing situations in which I'm not even sure what the right answers are. For example, assume you declare a direct dependency on `Werkzeug==2.0.0`. You then depend on a version of Flask that depends on a version of `Werkzeug` from some arbitrary URL. You build the source distribution at that arbitrary URL, and it turns out it _does_ build to a declared version of 2.0.0. What should happen? (And if it resolves to a version that _isn't_ 2.0.0, what should happen _then_?) I suspect different tools handle this differently, but it must lead to a lot of "silent" failures. In my testing of Poetry, it seems like Poetry just ignores the URL dependency, which seems wrong, but is also a behavior we could implement in the future.

Closes https://github.com/astral-sh/puffin/issues/303.
Closes https://github.com/astral-sh/puffin/issues/284.

---

_@charliermarsh reviewed on 2023-11-04 19:51_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/mod.rs`:22 on 2023-11-04 19:51_

This was all moved into the new `PubGrubDependencies` struct (and modified).

---

_@charliermarsh reviewed on 2023-11-04 19:52_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:137 on 2023-11-04 19:52_

This isn't necessary -- we end up visiting this immediately after anyway by way of visiting the `Root` package.

---

_Review requested from @zanieb by @charliermarsh on 2023-11-04 19:52_

---

_Review requested from @konstin by @charliermarsh on 2023-11-04 19:52_

---

_@charliermarsh reviewed on 2023-11-04 19:55_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/snapshots/pip_compile__conflicting_direct_url_dependency.snap`:19 on 2023-11-04 19:55_

Bad error message, sigh...

---

_Review comment by @konstin on `crates/puffin-resolver/src/pubgrub/package.rs`:49 on 2023-11-05 16:59_

```suggestion
        /// even clear. For example, imagine that you define a direct dependency on Werkzeug, and
```
also below

---

_@konstin approved on 2023-11-05 17:03_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/pubgrub/package.rs`:49 on 2023-11-05 17:07_

FYI WerkZeug means tool in German.

---

_@charliermarsh reviewed on 2023-11-05 17:07_

---

_Merged by @charliermarsh on 2023-11-05 17:09_

---

_Closed by @charliermarsh on 2023-11-05 17:09_

---

_Branch deleted on 2023-11-05 17:09_

---
