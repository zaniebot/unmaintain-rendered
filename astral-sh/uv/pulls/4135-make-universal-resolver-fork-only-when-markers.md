```yaml
number: 4135
title: make universal resolver fork only when markers are disjoint
type: pull_request
state: merged
author: BurntSushi
labels:
  - preview
assignees: []
merged: true
base: main
head: ag/fork-markers
created_at: 2024-06-07T15:56:42Z
updated_at: 2024-06-10T12:42:17Z
url: https://github.com/astral-sh/uv/pull/4135
synced_at: 2026-01-10T13:54:02Z
```

# make universal resolver fork only when markers are disjoint

---

_Pull request opened by @BurntSushi on 2024-06-07 15:56_

The basic idea here is to make it so forking can only ever result in a resolution that, for a particular marker environment, will only install at most one version of a package. We can guarantee this by ensuring we only fork on conflicting dependency specifications only when their corresponding markers are completely disjoint. If they aren't, then resolution _must_ find a single version of the package in the intersection of the two dependency specifications.

A test for this case has been added to packse here: https://github.com/astral-sh/packse/pull/182. Previously, that test would result in a resolution with two different unconditional versions of the same package. With this change, resolution fails (as it should).

A commit-by-commit review should be helpful here, since the first commit is a refactor to make the second commit a bit more digestible.

Closes #3926 

---

_Review requested from @charliermarsh by @BurntSushi on 2024-06-07 17:40_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:3 on 2024-06-07 22:44_

Why this?

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1773 on 2024-06-07 23:12_

We can _probably_ remove these comments then.

---

_@charliermarsh approved on 2024-06-07 23:17_

This looks good to me! I'd like to see some tests eventually for combinations of forks and markers, like...

```
flask
flask[dotenv] ; python_version > '3.7'
flask[async] ; python_version <= '3.7'
```

---

_Comment by @charliermarsh on 2024-06-07 23:18_

I'm gonna merge this so that I can build atop it if need be.

---

_@charliermarsh reviewed on 2024-06-07 23:29_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1773 on 2024-06-07 23:29_

I removed them...

---

_@charliermarsh reviewed on 2024-06-07 23:32_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:3 on 2024-06-07 23:32_

I removed it and fixed the lint issues, hope that's ok.

---

_@charliermarsh reviewed on 2024-06-07 23:33_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1811 on 2024-06-07 23:33_

I removed the `ref name` and `ref marker` here and now use `marker.as_ref()`, because the `PubGrubPackageInner::Marker` arm needs needs to return `Option<&Marker>` not `&Option<Marker>`.

---

_Label `preview` added by @charliermarsh on 2024-06-07 23:33_

---

_Merged by @charliermarsh on 2024-06-07 23:40_

---

_Closed by @charliermarsh on 2024-06-07 23:40_

---

_Branch deleted on 2024-06-07 23:40_

---

_@BurntSushi reviewed on 2024-06-10 11:46_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:3 on 2024-06-10 11:46_

Ug yeah. Sometimes I add this while writing new code (to silence lots of "unused code" warnings) and forget to remove it. Yes, it's okay!

---
