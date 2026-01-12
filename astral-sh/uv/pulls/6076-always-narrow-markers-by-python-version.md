```yaml
number: 6076
title: Always narrow markers by Python version
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/py-fork
created_at: 2024-08-14T01:00:35Z
updated_at: 2024-08-15T15:53:44Z
url: https://github.com/astral-sh/uv/pull/6076
synced_at: 2026-01-12T16:07:11Z
```

# Always narrow markers by Python version

---

_@charliermarsh_

## Summary

Using https://github.com/astral-sh/uv/issues/6064 as a motivating example: at present, on main, we're not properly propagating the `Requires-Python` simplifications. In that case, for example, we end up solving for a branch with `python_version < 3.11`, and a branch `>= 3.11`, even though `Requires-Python` is `>=3.11`. Later, when we get to the graph, we apply version simplification based on `Requires-Python`, which causes us to _remove_ the `python_version < 3.11` markers entirely, leaving us with duplicate dependencies for `pylint`.

This PR instead tries to ensure that we always apply this narrowing to requirements and forks, so that we don't need to apply the same simplification when constructing the graph at all.

Closes https://github.com/astral-sh/uv/issues/6064.

Closes #6059.


---

_Label `bug` added by @charliermarsh on 2024-08-14 01:01_

---

_Label `preview` added by @charliermarsh on 2024-08-14 01:01_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-14 01:05_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-14 01:05_

---

_@charliermarsh reviewed on 2024-08-14 01:06_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2770 on 2024-08-14 01:06_

Honestly, I can't figure out if the `simplify_python` here is necessary. It's definitely necessary on `not_markers` above... But I'm not sure about here. I don't think any tests change when I remove it.

---

_@ibraheemdev reviewed on 2024-08-14 02:11_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:1517 on 2024-08-14 02:11_

Hmm.. I'm surprised this works. It's important to note that calling `simplify_python_versions` creates a *different* marker tree than the same tree parsed from the lockfile. For example, if we restrict the range of `python_version >= 3.12` given `requires-python >= 3.9`, we now get the marker tree `[3.9,3.12)=>false,[3.12,inf)=>true`. If you parse that same expression from the lockfile, you get `[0,3.12)=>false,[3.12,inf)=>true`, which is not equal. This can cause false negatives in a `--locked` operation. Current we solve this [by reapplying the requires-python restriction after parsing the lockfile](https://github.com/astral-sh/uv/blob/main/crates/uv-resolver/src/lock.rs#L82). However, I believe this PR now restricts markers to the *narrowed* python requirement, which we do not reapply, so I'm surprised we aren't seeing failing test cases. Is there something I'm missing and can we remove the extra code in `Lock::from_toml` now?

---

_@charliermarsh reviewed on 2024-08-14 03:00_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1517 on 2024-08-14 03:00_

That is... interesting. If I remove that code from `from_toml`, `lock_conditional_dependency_extra` fails (for one).

---

_@charliermarsh reviewed on 2024-08-14 03:01_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1517 on 2024-08-14 03:01_

It may just be that this _doesn't_ work (for that particular problem) and we just don't have a proper test case for it.

---

_@charliermarsh reviewed on 2024-08-14 03:05_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1517 on 2024-08-14 03:05_

Any other ideas on how we can solve this problem?

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:1517 on 2024-08-14 03:05_

I think we could fix this by removing the whole idea of a restricted range with a special "rest" range, and then only apply simplification when displaying a marker, using disjointness checks instead for requires-python filtering. I was hoping we could use restriction to our advantage but it seems that may not be possible with narrowing.

---

_@ibraheemdev reviewed on 2024-08-14 03:05_

---

_@charliermarsh reviewed on 2024-08-14 03:07_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1517 on 2024-08-14 03:07_

I guess the other option is: try to write this to the lockfile.

---

_@charliermarsh reviewed on 2024-08-14 03:40_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1517 on 2024-08-14 03:40_

Can we use `environment-markers` to try and enforce this narrowing when we deserialize the lockfile?

---

_@ibraheemdev reviewed on 2024-08-14 04:18_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:1517 on 2024-08-14 04:18_

Maybe.. how would we map those to requirements?

---

_@charliermarsh reviewed on 2024-08-14 14:13_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1713 on 2024-08-14 14:13_

I guess this is a regression? I need to look at it.

---

_@charliermarsh reviewed on 2024-08-14 14:35_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1517 on 2024-08-14 14:35_

Could we instead just retain the lower-bound, like `>=3.7, <3.10` instead of using `<3.10`? Does that cause other problems?

---

_@charliermarsh reviewed on 2024-08-14 14:42_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1713 on 2024-08-14 14:42_

Fixed.

---

_Comment by @charliermarsh on 2024-08-14 14:44_

I think something like this is critical (it fixes a bunch of issues), but need some solution to the problem Ibraheem brought up. Let me try to write a test for it.

---

_@charliermarsh reviewed on 2024-08-14 15:04_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1517 on 2024-08-14 15:04_

Ok @ibraheemdev -- I added a failing test for this.

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:1924 on 2024-08-14 15:04_

This instability is due to the problem @ibraheemdev mentioned.

---

_@charliermarsh reviewed on 2024-08-14 15:04_

---

_Comment by @charliermarsh on 2024-08-14 15:11_

@ibraheemdev -- Maybe an alternative idea... Could we add `requires-python` to `Dependency`? And only set it when it differs from the top-level?

---

_Comment by @charliermarsh on 2024-08-14 15:13_

Hmm, but that might not cover `fork_markers`, just dependency markers.

---

_Comment by @charliermarsh on 2024-08-14 22:27_

I think we can safely do this if we merge https://github.com/astral-sh/uv/pull/6091?

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-08-15 00:18_

---

_@T-256 reviewed on 2024-08-15 12:29_

---

_Review comment by @T-256 on `crates/uv/tests/snapshots/ecosystem__warehouse-lock-file.snap`:10 on 2024-08-15 12:29_

shouldn't this remove too? since it doesn't fill in `>=3.11, <3.12` required range.

---

_@charliermarsh reviewed on 2024-08-15 12:58_

---

_Review comment by @charliermarsh on `crates/uv/tests/snapshots/ecosystem__warehouse-lock-file.snap`:10 on 2024-08-15 12:58_

We don't respect upper-bounds on `requires-python`.

---

_@BurntSushi reviewed on 2024-08-15 14:57_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:2770 on 2024-08-15 14:57_

Logically, I think this simplification here is right and even necessary. Because while `fork.markers`, by construction, should already be simplified, negating that here might wind up with a marker that includes (or is completely excluded by) `requires-python`. So re-doing the simplification does indeed seem correct.

I think that the same simplification ought to happen to `markers` too right? Specifically, before passing it to `fork.intersect` below.

---

_@BurntSushi approved on 2024-08-15 14:59_

I think I found one possible additional simplification that should be done, but otherwise I think this LGTM assuming @ibraheemdev's concern is addressed. (I think it is now with #6102 merged?)

---

_@ibraheemdev approved on 2024-08-15 15:08_

This looks good to me now that we no longer rely on marker equality for `--locked`.

---

_@charliermarsh reviewed on 2024-08-15 15:30_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2770 on 2024-08-15 15:30_

I thought `markers` was already simplified because we simplify when generating `PubGrubDependency`.

---

_@charliermarsh reviewed on 2024-08-15 15:30_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2770 on 2024-08-15 15:30_

I.e., here: https://github.com/astral-sh/uv/pull/6076/files#diff-6be6d80fe4821c47b70a372260f55e73b8da8182b8dcad7525d5cd3eb584532bR1523

---

_Merged by @charliermarsh on 2024-08-15 15:50_

---

_Closed by @charliermarsh on 2024-08-15 15:50_

---

_Branch deleted on 2024-08-15 15:50_

---

_@BurntSushi reviewed on 2024-08-15 15:53_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:2770 on 2024-08-15 15:53_

Talking over DM, the markers here are already simplified.

---
