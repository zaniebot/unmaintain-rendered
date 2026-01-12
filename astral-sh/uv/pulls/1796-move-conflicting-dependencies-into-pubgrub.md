```yaml
number: 1796
title: Move conflicting dependencies into PubGrub
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/transitive
created_at: 2024-02-21T05:39:47Z
updated_at: 2024-02-22T02:27:59Z
url: https://github.com/astral-sh/uv/pull/1796
synced_at: 2026-01-12T16:04:44Z
```

# Move conflicting dependencies into PubGrub

---

_@charliermarsh_

## Summary

This revives a PR from long ago (https://github.com/astral-sh/uv/pull/383 and https://github.com/zanieb/pubgrub/pull/24) that modifies how we deal with dependencies that are declared multiple times within a single package.

To quote from the originating PR:

> Uses an experimental pubgrub branch (#370) that allows us to handle multiple version ranges for a single dependency to the solver which results in better error messages because the derivation tree contains all of the relevant versions. Previously, the version ranges were merged (by us) in the resolver before handing them to pubgrub since only one range could be provided per package. Since we don't merge the versions anymore, we no longer give the solver an empty range for conflicting requirements; instead the solver comes to that conclusion from the provided versions. You can see the improved error message for direct dependencies in [this snapshot](https://github.com/astral-sh/puffin/pull/383/files#diff-a0437f2c20cde5e2f15199a3bf81a102b92580063268417847ec9c793a115bd0).

The main issue with that PR was around its handling of URL dependencies, so this PR _also_ refactors how we handle those. Previously, we stored URL dependencies on `PubGrubPackage`, but they were omitted from the hash and equality implementations of `PubGrubPackage`. This led to some really careful codepaths wherein we had to ensure that we always visited URLs before non-URL packages, so that the URL-inclusive versions were included in any hashmaps, etc. I considered preserving this approach, but it would require us to rely on lots of internal details of PubGrub (since we'd now be relying on PubGrub to merge those packages in the "right" order).

So, instead, we now _always_ set the URL on a given package, whenever that package was _given_ a URL upfront. I think this is easier to reason about: if the user provided a URL for `flask`, then we should just always add the URL for `flask`. If we see some other URL for `flask`, we error, like before. If we see some unknown URL for `flask`, we error, like before.

Closes https://github.com/astral-sh/uv/issues/1522.

Closes https://github.com/astral-sh/uv/issues/1821.

Closes https://github.com/astral-sh/uv/issues/1615.

---

_Comment by @charliermarsh on 2024-02-21 05:40_

(No reviews yet, this doesn't pass the test suite.)

---

_Comment by @charliermarsh on 2024-02-21 05:50_

\cc @zanieb for visibility.

---

_Review requested from @zanieb by @charliermarsh on 2024-02-21 18:19_

---

_Review request for @zanieb removed by @charliermarsh on 2024-02-21 18:20_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-21 19:24_

---

_Marked ready for review by @charliermarsh on 2024-02-21 19:24_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install_scenarios.rs`:1947 on 2024-02-21 19:25_

I believe these are generally the result of dependencies being presented to PubGrub in a different order.

---

_@charliermarsh reviewed on 2024-02-21 19:25_

---

_Label `bug` added by @charliermarsh on 2024-02-21 19:26_

---

_@zanieb reviewed on 2024-02-21 20:19_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:1281 on 2024-02-21 20:19_

Is this an improvement? Should we clarify that the URLs come from the provided requirements?

---

_@zanieb reviewed on 2024-02-21 20:20_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:1626 on 2024-02-21 20:20_

Nice. It'll be better once we combine clauses #1009, maybe someday we should even track the source of dependencies so we can say what file each of these came from.

---

_@zanieb reviewed on 2024-02-21 20:21_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:3782 on 2024-02-21 20:21_

This reads a little confusing

---

_@zanieb reviewed on 2024-02-21 20:22_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:4030 on 2024-02-21 20:22_

This one definitely makes me want to open an issue to display the locations of direct requirements in error messages.

---

_@zanieb reviewed on 2024-02-21 20:23_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:1947 on 2024-02-21 20:23_

Yeah it's a little weird but a known issue.

---

_@zanieb reviewed on 2024-02-21 20:26_

---

_Review comment by @zanieb on `Cargo.toml`:69 on 2024-02-21 20:26_

Let's make sure to merge upstream and use the `main` commit before merging this one.

---

_@zanieb approved on 2024-02-21 20:26_

Looks nice. Is there anything you wanted me to give a closer look?

---

_Merged by @charliermarsh on 2024-02-22 02:27_

---

_Closed by @charliermarsh on 2024-02-22 02:27_

---

_Branch deleted on 2024-02-22 02:27_

---
