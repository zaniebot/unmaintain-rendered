```yaml
number: 6065
title: Propagate fork markers to extras
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - resolver
  - preview
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2024-08-13T18:01:38Z
updated_at: 2024-08-14T13:55:42Z
url: https://github.com/astral-sh/uv/pull/6065
synced_at: 2026-01-10T13:09:50Z
```

# Propagate fork markers to extras

---

_Pull request opened by @charliermarsh on 2024-08-13 18:01_

## Summary

When constructing the `Resolution`, we only propagated the fork markers to the package node, but not the extras node. This led to cases in which an extra could be included unconditionally or otherwise diverge from the base package version.

Closes https://github.com/astral-sh/uv/issues/6062.


---

_Label `bug` added by @charliermarsh on 2024-08-13 18:01_

---

_Label `resolver` added by @charliermarsh on 2024-08-13 18:01_

---

_Label `preview` added by @charliermarsh on 2024-08-13 18:01_

---

_Comment by @charliermarsh on 2024-08-13 18:05_

This isn't quite doing the right thing in `lock_python_version_marker_complement`. I'm getting an instability there.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-13 18:07_

---

_Comment by @charliermarsh on 2024-08-13 18:07_

I think my change is correct but now hitting the aforementioned instability.

---

_@charliermarsh reviewed on 2024-08-13 18:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2356 on 2024-08-13 18:07_

I moved this into graph construction, since we're now AND-ing the fork markers with _every_ edge.

---

_@charliermarsh reviewed on 2024-08-13 18:11_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock_scenarios.rs`:1417 on 2024-08-13 18:11_

These may not be strictly necessary... Because you can only reach this node on `sys_platform == 'darwin'`.

---

_@BurntSushi approved on 2024-08-13 18:15_

I think this looks right to me.

In terms of moving this PR forward, maybe I cherry pick this PR into my branch that will (hopefully) fix the instability issue? So it won't get merged right away, but it will be in an active branch somewhere.

---

_@charliermarsh reviewed on 2024-08-13 18:15_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock_scenarios.rs`:1417 on 2024-08-13 18:15_

(Yeah confirmed. Is it wrong to include then?)

---

_Review comment by @charliermarsh on `crates/uv/tests/snapshots/ecosystem__transformers-lock-file.snap`:1905 on 2024-08-13 18:15_

Ouch, not good!

---

_@charliermarsh reviewed on 2024-08-13 18:15_

---

_@charliermarsh reviewed on 2024-08-13 18:15_

---

_Review comment by @charliermarsh on `crates/uv/tests/snapshots/ecosystem__transformers-lock-file.snap`:1901 on 2024-08-13 18:15_

So that's a bugfix.

---

_@BurntSushi reviewed on 2024-08-13 18:17_

---

_Review comment by @BurntSushi on `crates/uv/tests/snapshots/ecosystem__transformers-lock-file.snap`:1901 on 2024-08-13 18:17_

Yup! ANd there's no extra here.

---

_@charliermarsh reviewed on 2024-08-13 18:18_

---

_Review comment by @charliermarsh on `crates/uv/tests/snapshots/ecosystem__warehouse-lock-file.snap`:3147 on 2024-08-13 18:18_

This looks wrong. I don't know where it's coming from.

---

_@BurntSushi reviewed on 2024-08-13 18:19_

---

_Review comment by @BurntSushi on `crates/uv/tests/lock_scenarios.rs`:1417 on 2024-08-13 18:19_

I don't think it's _wrong_, but it does seem superfluous. But I'm okay to run with it for now.

---

_Review comment by @charliermarsh on `crates/uv/tests/snapshots/ecosystem__warehouse-lock-file.snap`:3147 on 2024-08-13 18:24_

Debugging this.

---

_@charliermarsh reviewed on 2024-08-13 18:24_

---

_Review comment by @charliermarsh on `crates/uv/tests/snapshots/ecosystem__warehouse-lock-file.snap`:3147 on 2024-08-13 18:38_

I haven’t figured this out yet, I think I should understand why this is happening before we merge @BurntSushi though you’re welcome to cherry-pick.

---

_@charliermarsh reviewed on 2024-08-13 18:38_

---

_@charliermarsh reviewed on 2024-08-13 20:51_

---

_Review comment by @charliermarsh on `crates/uv/tests/snapshots/ecosystem__warehouse-lock-file.snap`:3147 on 2024-08-13 20:51_

Oh... I guess this is actually fine.

---

_@charliermarsh reviewed on 2024-08-13 20:51_

---

_Review comment by @charliermarsh on `crates/uv/tests/snapshots/ecosystem__warehouse-lock-file.snap`:3147 on 2024-08-13 20:51_

It's just redundant.

---

_@charliermarsh reviewed on 2024-08-13 20:52_

---

_Review comment by @charliermarsh on `crates/uv/tests/snapshots/ecosystem__warehouse-lock-file.snap`:3147 on 2024-08-13 20:52_

I needed to see it with fresh eyes.

---

_Comment by @charliermarsh on 2024-08-13 22:19_

Very hard to explain but regarding the instability: it has to do with the fact that when we resolve without a lockfile, we create the fork _on the dependencies_ of the `project`. So we iterate over all dependencies, then create forks.

When we resolve _with_ a lockfile, we fork on the project itself... So when we go to solve for that project's dependencies, we already have a `requires-python` marker which we then apply to filter out certain dependencies.


---

_Comment by @charliermarsh on 2024-08-13 22:23_

That might need to be solved holistically, though fixing the normalization to detect disjointness for these would also help.

---

_Comment by @charliermarsh on 2024-08-13 22:23_

Basically, we end up with this bad fork due to the confusion:

```
DEBUG skipping iniconfig ; python_version >= '3.10' because of context resolver markers python_full_version > '3.10' and python_version < '3.10'
DEBUG skipping iniconfig ; python_version < '3.10' because of Requires-Python python_full_version > '3.10' and python_version > '3.10'
```

---

_Comment by @charliermarsh on 2024-08-14 01:13_

Rebasing on https://github.com/astral-sh/uv/pull/6076 does fix one of the instabilities, but `github_wikidata_bot` is still unstable...

---

_Comment by @BurntSushi on 2024-08-14 13:37_

> but `github_wikidata_bot` is still unstable

Is this the only issue? If so, I'd say flip this test to non-deterministic (like `transformers`) via `lock_ecosystem_package_non_deterministic` and merge it. That way we're working from a common base and because this PR fixes bugs where there are multiple versions of the same package for the same marker environment in the lock file.

---

_Comment by @charliermarsh on 2024-08-14 13:40_

Yeah will do. `lock_python_version_marker_complement` and `lock_conditional_dependency_extra` also became non-deterministic, but some of those are fixed upstream I think. I'm gonna document and merge.

---

_Merged by @charliermarsh on 2024-08-14 13:55_

---

_Closed by @charliermarsh on 2024-08-14 13:55_

---

_Branch deleted on 2024-08-14 13:55_

---
