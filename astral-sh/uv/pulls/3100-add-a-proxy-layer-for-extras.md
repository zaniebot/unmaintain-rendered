```yaml
number: 3100
title: Add a proxy layer for extras
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/extras
created_at: 2024-04-17T17:15:12Z
updated_at: 2024-04-19T01:05:01Z
url: https://github.com/astral-sh/uv/pull/3100
synced_at: 2026-01-10T14:43:31Z
```

# Add a proxy layer for extras

---

_Pull request opened by @charliermarsh on 2024-04-17 17:15_

Given requirements like:

```
black==23.1.0
black[colorama]
```

The resolver will (on `main`) add a dependency on Black, and then try to use the most recent version of Black to satisfy `black[colorama]`. For sake of example, assume `black==24.0.0` is the most recent version. Once the selects this most recent version, it'll fetch the metadata, then return the dependencies for `black==24.0.0` with the `colorama` extra enabled. Finally, it will tack on `black==24.0.0` (a dependency on the base package). The resolver will then detect a conflict between `black==23.1.0` and `black==24.0.0`, and throw out `black[colorama]==24.0.0`, trying to next most-recent version.

This is both wasteful and can cause problems, since we're fetching metadata for versions that will _never_ satisfy the resolver. In the `apache-airflow[all]` case, I also ran into an issue whereby we were attempting to build very old versions of `apache-airflow` due to `apache-airflow[pandas]`, which in turn led to resolution failures.

The solution proposed here is that we create a new proxy package with exactly two dependencies: one on `black` and one of `black[colorama]`. Both of these packages must be at the same version as the proxy package, so the resolver knows much _earlier_ that (in the above example) the extra variant _must_ match `23.1.0`.


---

_Comment by @zanieb on 2024-04-17 17:17_

I like where this is going

---

_Comment by @charliermarsh on 2024-04-17 18:39_

I think the ideas here are sound but need a packse test and more documentation around the concepts.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-04-17 21:15_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-17 21:15_

---

_Review requested from @konstin by @charliermarsh on 2024-04-17 21:15_

---

_Marked ready for review by @charliermarsh on 2024-04-17 21:15_

---

_Label `bug` added by @charliermarsh on 2024-04-17 21:18_

---

_@zanieb reviewed on 2024-04-17 21:28_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:934 on 2024-04-17 21:28_

Epic

> Because only package-a[extra-c]==1.0.0 is available and package-a[extra-c]==1.0.0 depends on package-a[extra-c]==1.0.0, we can conclude that all versions of package-a[extra-c] depend on package-a[extra-c]==1.0.0.

---

_@charliermarsh reviewed on 2024-04-17 21:49_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install_scenarios.rs`:934 on 2024-04-17 21:49_

Yeah this is a real regression. Will look into it.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1025 on 2024-04-18 12:36_

Nice. I like it.

---

_@BurntSushi approved on 2024-04-18 12:37_

This is awesome.

---

_@konstin approved on 2024-04-18 12:42_

nice

---

_Comment by @Eh2406 on 2024-04-18 13:29_

This looks like a clever lowering. I'd love to talk it over to see if it helps with a similar problem with "features" in cargo. I was planing to use https://github.com/pubgrub-rs/pubgrub/pull/124 for the "features" problem, and would love to compare.

---

_Merged by @charliermarsh on 2024-04-19 01:05_

---

_Closed by @charliermarsh on 2024-04-19 01:05_

---

_Branch deleted on 2024-04-19 01:05_

---
