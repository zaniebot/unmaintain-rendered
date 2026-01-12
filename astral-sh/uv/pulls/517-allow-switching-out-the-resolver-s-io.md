```yaml
number: 517
title: "Allow switching out the resolver's IO"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/resolver-tests
created_at: 2023-11-29T19:54:16Z
updated_at: 2023-12-06T17:53:17Z
url: https://github.com/astral-sh/uv/pull/517
synced_at: 2026-01-12T16:04:00Z
```

# Allow switching out the resolver's IO

---

_@zanieb_

I'm working off of @konstin's commit here to implement arbitrary unsat test cases for the resolver.

The entirety of the resolver's io are two functions: Get the version map for a package (PEP 440 version -> distribution) and get the metadata for a distribution. A new trait `ResolverIo` abstracts these two away and allows replacing the real network requests e.g. with stored responses (https://github.com/pradyunsg/pip-resolver-benchmarks/blob/main/scenarios/pyrax_198.json).


---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:46 on 2023-11-29 20:28_

More commonly I've sen this kind of thing referred to as (e.g.) `ResolverProvider`.

---

_@charliermarsh reviewed on 2023-11-29 20:28_

---

_Comment by @zanieb on 2023-11-30 21:06_

I'm leaving this as a draft while I build on top of it, I'd like a working test case that uses a stubbed resolver. Unless we want it immediately in which case it's ready... it may be easier to coordinate by merging.

---

_Comment by @konstin on 2023-12-04 07:47_

I'd merge the provider refactor to avoid merge conflicts

---

_Marked ready for review by @zanieb on 2023-12-06 17:06_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:171 on 2023-12-06 17:18_

This should be `provider`

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:193 on 2023-12-06 17:19_

Here too it should be provider.

---

_@konstin approved on 2023-12-06 17:20_

---

_Merged by @zanieb on 2023-12-06 17:53_

---

_Closed by @zanieb on 2023-12-06 17:53_

---

_Branch deleted on 2023-12-06 17:53_

---
