```yaml
number: 383
title: Improve handling of dependency conflicts resulting in empty version ranges
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: charlie/pubgrub
head: zanie/pubgrub-merge
created_at: 2023-11-09T21:32:16Z
updated_at: 2024-02-21T05:31:18Z
url: https://github.com/astral-sh/uv/pull/383
synced_at: 2026-01-12T16:03:55Z
```

# Improve handling of dependency conflicts resulting in empty version ranges

---

_@zanieb_

Uses an experimental pubgrub branch (#370) that allows us to handle multiple version ranges for a single dependency to the solver which results in better error messages because the derivation tree contains all of the relevant versions. Previously, the version ranges were merged (by us) in the resolver before handing them to pubgrub since only one range could be provided per package. Since we don't merge the versions anymore, we no longer give the solver an empty range for conflicting requirements; instead the solver comes to that conclusion from the provided versions. You can see the improved error message for direct dependencies in [this snapshot](https://github.com/astral-sh/puffin/pull/383/files#diff-a0437f2c20cde5e2f15199a3bf81a102b92580063268417847ec9c793a115bd0).

Since we are no longer merging packages, we do not raise an error on conflicting urls in dependencies anymore. Our previous solution was kind of a hack, so I'm exploring better solutions such as:

- Removing `Option<Url>` from `PubGrubPackage::Package` in favor of a separate `PubGrubPackage::UrlPackage` variant e.g. https://github.com/astral-sh/puffin/pull/383/commits/a510a2f882875310f46fde707c9a99eb9f26b70c
- Adding a new `PubGrubPackage::Url` variant that is added as a dummy dependency with fake conflicting version numbers for each package name and url combination e.g. 4343f66...08509a5

The crux of the problem is that we must have pubgrub consider packages with the same name (specified with or without a url) as the same package with possibly compatible or incompatible versions. However, we do not have a good way to treat different urls as distinct versions while also allowing a url to satisfy versions ranges required by other dependencies relying on the package.

Supersedes #338 
Requires https://github.com/astral-sh/puffin/pull/370


---

_Comment by @zanieb on 2024-01-04 22:50_

Still need to investigate support for this in PubGrub...

---

_Comment by @zanieb on 2024-01-05 22:43_

As demonstrated by

https://github.com/astral-sh/puffin/blob/88adba83a01357dec22bc7e2a0d79ab53e589d5c/crates/puffin-cli/tests/pip_install_scenarios.rs#L1906 

this appears to be resolved on `main`. The message in this pull request is comparable

https://github.com/astral-sh/puffin/blob/08509a5c02b190281ac97162b3b4cdc7cf86b919/crates/puffin-cli/tests/snapshots/pip_compile__compile_unsolvable_requirements.snap#L19-L20

---

_Closed by @zanieb on 2024-01-05 22:43_

---

_Comment by @charliermarsh on 2024-02-21 05:31_

I've revived the original PubGrub change in https://github.com/zanieb/pubgrub/tree/charlie/into.

---
