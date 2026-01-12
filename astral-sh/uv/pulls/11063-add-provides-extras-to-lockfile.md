```yaml
number: 11063
title: add provides-extras to lockfile
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: tracking/060
head: gankra/lock-extra
created_at: 2025-01-29T14:20:00Z
updated_at: 2025-02-11T19:38:41Z
url: https://github.com/astral-sh/uv/pull/11063
synced_at: 2026-01-12T16:09:39Z
```

# add provides-extras to lockfile

---

_@Gankra_

Fixes #10953 

---

_Review requested from @charliermarsh by @Gankra on 2025-01-29 14:20_

---

_Comment by @Gankra on 2025-01-29 14:20_

I still need to add a snapshot test where this is non-empty.

---

_@charliermarsh reviewed on 2025-01-29 15:03_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 15:03_

Something we need to consider: if we want to start using this to _enforce_ that a given extra exists (at install time, by reading from the lockfile), we won't be able to tell the difference between "empty" and "absent". So we either need to _not_ do that, or bump the lockfile version so we can tell the difference, or write these even when empty.

---

_@charliermarsh reviewed on 2025-01-29 15:03_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:1807 on 2025-01-29 15:03_

I think you need to change `to_toml` to write these out. The impl is manual.

---

_@Gankra reviewed on 2025-01-29 15:07_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 15:07_

Do you have a preference on which approach? "bump lockfile version" sounds principled but dunno if it's a Big Deal.

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 15:13_

I'm not sure. @zanieb, do you have an opinion? Bumping the lockfile version means that older uv versions will reject newer lockfiles (but we can still read older lockfiles in newer uv versions).

---

_@charliermarsh reviewed on 2025-01-29 15:13_

---

_@charliermarsh reviewed on 2025-01-29 15:14_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 15:14_

We might just want to always write this (and always write `requires-dist` too). It's not, like, a huge change... Since we only show this for local packages anyway.

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 15:26_

In addition to always writing this we'd also need to make this like... an `Option<Vec<ExtraName>>` to distinguish whether we found it in the lockfile, right?

---

_@Gankra reviewed on 2025-01-29 15:26_

---

_@charliermarsh reviewed on 2025-01-29 15:36_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 15:36_

Yeah that’s right 

---

_@Gankra reviewed on 2025-01-29 15:59_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 15:59_

Added the commit, I'm not sure we're gonna love how effectively `[package.metadata]` is no longer ever empty-and-therefore-omitable.

---

_@charliermarsh reviewed on 2025-01-29 16:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 16:07_

I was thinking that we'd continue to omit it for immutable sources, is that doable? It should massively reduce the diff.

---

_@Gankra reviewed on 2025-01-29 16:54_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 16:54_

It did remove a lot of them, yeah. (updated the commit)

---

_@zanieb reviewed on 2025-01-29 17:59_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 17:59_

Generally in favor of less breaking lockfile changes :) it's decent timing with 0.6 coming up, but from what I understand this doesn't justify the churn? 

---

_@Gankra reviewed on 2025-01-29 18:47_

---

_Review comment by @Gankra on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 18:47_

It's just to enable more validation on cli arguments, yeah. Not really worth a break.

---

_@charliermarsh reviewed on 2025-01-29 18:48_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock/mod.rs`:1790 on 2025-01-29 18:48_

Yeah I don't think we should bump for it. I do wonder if we should treat `requires-dist` the same way, though: always write it, even if empty?

I think users will likely be annoyed that the lockfile is churning more (even without a bump) in that you're now getting different results if you lock with a later version (even though it should be backwards-compatible), but I don't know how we avoid that.


---

_Added to milestone `v0.6.0` by @Gankra on 2025-01-29 23:08_

---

_Comment by @Gankra on 2025-02-11 19:20_

> CI / check system | python3.13 on windows x86-64 (pull_request) 

this task is extremely haunted on this PR and I wish I had any reason to think this PR breaks it...

---

_Comment by @Gankra on 2025-02-11 19:21_

> Error: Cache folder path is retrieved for pip but doesn't exist on disk: c:\users\runneradmin\appdata\local\pip\cache. This likely indicates that there are no dependencies to cache. Consider removing the cache step if it is not needed.

god worst reason to die too

---

_@charliermarsh approved on 2025-02-11 19:29_

---

_Comment by @charliermarsh on 2025-02-11 19:30_

> god worst reason to die too

I think @zanieb fixed this on `main` so this branch might just be behind.

---

_Merged by @Gankra on 2025-02-11 19:38_

---

_Closed by @Gankra on 2025-02-11 19:38_

---

_Branch deleted on 2025-02-11 19:38_

---
